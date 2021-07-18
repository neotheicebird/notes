
meta properties:

```
obj
{
	entity_id:UUID		# id of the obj
	version:String		# timestamp+user_id
	active:Bool			# marks if item is not deleted
	
}
```

Ideas:

1. By means of making `version` into a timestamp and user_id combination, the top item queried with entity_id and scanIndexForward=False, would always be the latest item, irrespective of if the item is active or not


Lists:

1. I create a list obj named `shopping`. 

```
{
  entity_id
  version
  obj_type
  created_by = prashanth
  changed_by = prashanth
  active
  latest
  family_id = squirrel
  name = shopping
}
```

2. I invite mom to join the app. So there could be an invite obj like

```
{
  _meta
  family_id = squirrel
  invited_email = laxmi.k.viji@gmail.com
  inviter_id = prashanth
  access_code = ABCD123
}
```

3. using index `by_family_id_and_latest` mom can get the latest list objs
4. she can edit the name of `shopping`{List} to `grocery`{List}

```
{
  entity_id
  version
  obj_type
  created_by = prashanth
  changed_by = viji
  active
  latest
  family_id = squirrel
  name = grocery
}
```

Every obj has an owned_by, that attr could be set to an user_id or any other id. i.e. a `profile` type has owned_by set to user_id, but a `list` type would have owned_by set to family_id.

why do we need `owned_by`? So that we can create `owned_by_and_latest`