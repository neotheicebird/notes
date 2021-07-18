meta props of every event:

`entity_id` - Every event belongs to an entity and each entity should have an entity_id
`version` 
`prev_version` - previous version helps us track the order of events (there could be 2 events with same previous version)
`ts` - timestamp (ISOtimestamp or Integer timestamp?)
`type` - event type (possible events are `create`, `update`, `delete`)
`active` - is entity active or is deleted (active=false)
`author` - who created this event (should all entities have authors? is there any event without an author?)

