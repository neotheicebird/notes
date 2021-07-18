Libraries:

1. Pymongo:
```
db['movies'].update({'_id': id},
                    {'$set': {'name': 'My new title'}})
```

2. mongoengine

```
Movies.objects(id=id).update(name='My new title')
```

3. mongokit (archived)

Setting up mongo docker compose: https://faun.pub/managing-mongodb-on-docker-with-docker-compose-26bf8a0bbae3


Ref:

1. https://dev.to/paurakhsharma/flask-rest-api-part-1-using-mongodb-with-flask-3g7d#:~:text=1)%20Pymongo%20is%20a%20low,writing%20the%20MongoDB%20query%20directly.&text=Mongoengine%20uses%20predefined%20schema%20for,the%20Schemaless%20nature%20of%20MongoDB.

add cors middleware
add name field to signup params
return profile_pic_path in user


fix signup, and login
remove vuex code



