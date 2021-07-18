RabbitMQ is described as an open source distributed message broker. Implented over AMQP protocol, its capabilities can be expanded with plug-ins

Jargons:

1. `Producing` - Essentially means sending a message. A program that sends a message is a "producer". 
2. `Queue` - Messages are stored in a queue, which is limited by the host's memory and disk limits. Essentially a large message buffer.
3. `Consuming` - Receving a message. A consumer is a program that waits to receive messages.

Note: Producer, Broker and Consumer need not recide in same host. In most cases they dont.

Pika - Python client

Consumers wait for any messages in queue, when a message is delivered a callback function is called, which will handle the message. For example in the code below, we have a worker that can receive tasks as strings like "Hello..." and sleep for 3 secs 1 sec for each "." auto acknowledge is also switched off, so that the message is re-queued if the worker fails in processing the message (and doesnt ack it). This re-queing happens after the worker dies.

Note: if you miss the `basic_ack` part and also set `auto_ack=False`, RabbitMQ wont be able to release any messages and it will start consuming more and more memory in the host and also redeliver to workers as they spawn.

```python
connection = pika.BlockingConnection(pika.ConnectionParameters(host='localhost'))

channel = connection.channel()
channel.queue_declare(queue='hello')


def callback(ch, method, properties, body):
	print(" [x] Received %r" % body.decode())
	time.sleep( body.count('.') )
	print(" [x] Done")
	ch.basic_ack(delivery_tag = method.delivery_tag)


channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=False)

print(' [*] Waiting for messages. To exit press CTRL+C')

channel.start_consuming()
```

Now we know the importance of `ack`ing the messages and how we can make sure that a worker process getting killed doesn't lose the message. There is another way we can lose messages, which is if the rabbit server crashes for any reason. To make messages durable inspite of crash, when it comes back up it remembers, we can add a argument `durable=True` when creating the queue

`channel.queue_declare(queue='hello', durable=True)`

Make sure both send.py and worker.py has defined queue as durable or not for consistent behaviour. (we dont know which process creates the queue in production. queue_declare is an idempotent method that creates the queue or checks and ignores if queue exists.)

For a message to persist in a "durable" server, while sending it has to be marked as a persistant by setting a "delivery_mode", like:

```
channel.basic_publish(exchange='',
                      routing_key="task_queue",
                      body=message,
                      properties=pika.BasicProperties(
                         delivery_mode = 2, # make message persistent
                      ))
```

### Fair Dispatch:

RabbitMQ by default uses Round-Robin method to dispatch messages. i.e. If there are N workers, each one gets one message before the first worker getting a new message. This assumes all messages to be of equal complexity. This might not work in all cases. For example when we have 2 workers and all odd messages are light and even messages are heavy, we have a situation in which a worker is always busy and another mostly free.

How to make things more performant?

RabbitMQ doesn't look at the number of acknowledgements each workers make, instead, it blindly assigns a message to one of the n workers. A better strategy would be to set `prefetch_count=1`, which means that at a time assign only one message to a worker. So until a worker acknowledges a new message is not assigned. This also means that a worker that quickly acknowledges gets a new message faster making dispatch fair.

### Publish/Subscribe

We can deliver a message to multiple consumers, so how to do ack (assuming all consumers listen to same queue)? lets see.

In the core ideas of producer, queue and consumer, there is another element called "exchange" which wasn't discussed earlier.

Producer can only send messages to an exchange (earlier a default exchange was used). Exchange is a simple piece, on one side it receives messages from producer and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives. The rules for what to do are defined by "exchange type"

exchange types: direct, topic, headers, fanout

For e.g. `fanout` means the exchange broadcast the message to all the queues that the exchange knows.

So by broadcasting to a exchange instead of a queue, the messages gets enqueued into mutiple queues, each of which is consumed by one consumer. So no discrepency in ack.