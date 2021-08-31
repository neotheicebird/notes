Asynchronous Python for the Complete Beginner PyCon 2017
https://www.youtube.com/watch?v=iG6fr81xHKA
talk by: Miguel Grinberg


1. How is concurrency achieved?

a. Threads: Depends on OS to "speed up" programs. GIL is a python feature/problem that ensures that effectively only one thread runs at a time, making threading not a great feature for python

b. Multicore programming (multiprocessing), depends on number of cores, typical computers have 4 or max 8 cores, so it is a good "patch" to speed up programs but isn't really scalable

c. Asynchronous programming: No OS involvement, basically a new style of programming, with a fresh set of low level libraries, that leads to "faster" programs.

2. Real life example:

In a chess exhibition match by judit polgar, as a child she plays multiple opponents at a time defeating every one of them. This is how async code works as well.

Lets say Polgar takes 5 secs per move, she has 24 opponents and each opponent takes 55 secs per move, an average match lasts 30 pairs of moves.

In synchronous scenario, the total time polgar has to play is 30x1minx24 = 12hours.

But in an async scenario, she would take 5 secs play table 1, move to table 2 take 5 secs, and so on, she needs 24x5secs = 2mins to complete one round, by which time every opponent gets enough time (>55secs) to play their move, so to go 30 rounds, polgar would take 60mins. That's 12x faster!

3. How does async really work?

a. Async functions need the ability to "suspend and resume"
b. functions entering waiting period is suspended and only resumed when the wait is over
c. Four ways to implement suspend/resume in python:
	- Callback functions (gross in python)
	- Generator functions
	- Async/Await (>py3.5)
	- Greenlets

4. Scheduling async tasks

- A scheduler is a system that helps decide which function gets to run when one function enters waiting period, generally it is called the "event loop".
- The loop keeps track of all the running tasks
- when a function is suspended, control returns to the loop which then finds another function to start or resume
- this is called "cooperative multitasking"
- Old versions of MacOS and Windows did this, so this is an old idea.


Examples:

1. sync: wait for 3 secs and print

```
from time import sleep

def hello():
	print("hello")
	sleep(3)
	print("world")

if __name__ == "__main__":
	hello()
```

async version-a:

```
import asyncio
loop = asyncio.get_event_loop()

@asyncio.corountine
def hello():
	print("hello")
	yield from asyncio.sleep(3)
	print("world")

if __name__ == "__main__":
    loop.run_until_complete(hello())
```

async version-b:

```
import asyncio
loop = asyncio.get_event_loop()

async def hello():
    print("hello")
	await asyncio.sleep(3)
	print("world")

if __name__ == "__main__":
	loop.run_until_complete(hello())
```
In JS, an event loop looks at the "call stack" and the "task queue" and if call stack is empty, it pops the top item from task queue and inserts it into the call stack to be executed next. Hopefully, event loop here works similarly.

functionally async version a & b are equivalent.

5. Async Pitfalls

- Long CPU intensive tasks must routinely release the CPU to avoid starving other tasks
- This can be done by "sleeping" periodically, such as once per loop iteration
- To tell the loop to return control back as soon as possible, such as `await asyncio.sleep(0)`
- Blocking library functions are incompatible with async frameworks, we need a totally new set of low level apis
- all async frameworks provide non-blocking replacements for these
- we need to relearn stuff we know to do in sync way


Hi Jay, thanks for asking, I am doing lot better now, my post covid health issues are slowly getting resolved. I dont have the cognitive mileage I used to have earlier, but I am able to adapt and form some good habits which is making me feel confident.  how are you?

As for the Ecor Rouge work, I was a bit out of touch last week, but I am enjoying it right now. The async nature of mongodb connections and how it fits in the current design took me some time to figure out, but I think I have good patterns figured out for the rest of the adapter and will be coding them tomorrow and day after. I am trying to keep the repository design as such with just minor changes, the adapter should take 3-4 more hours to complete.

I also started working with another client for over a month. Its another light opportunity where I can put in <10hrs a week and I took it as I wanted to explore some new tech, but I didn't want to over commit.

Covid situation in India is stable right now. The counts in my state and city is at the bottom, but some nearby states still has considerable count. Vaccination drive is going well and situation could remain as good as now, hopefully. 

I wanted to ask you about your India visit plans when we chatted next, are you still planning your visit for end of 2021? Please let me know if you would need any help with your planning, I would be excited if your plan is still on! 