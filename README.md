This is a demo PouchDB SQLite database to demonstrate an issue where attachment digests are not checked for integrity, leading to potentially corrupt attachments being saved to the database.

The test database contains a document with an attachment which has a false digest. When replicating into PouchDB the attachment will be given a new digest instead of failing.

To test run (in this directory):

`npx pouchdb-server --sqlite --port 5984`

Then send a POST to http://127.0.0.1:5984/_replicator

```
{
  "_id": "test-replciation",
  "source": {
    "headers": {},
    "url": "http://127.0.0.1:5984/test"
  },
  "target": {
    "headers": {},
    "url": "http://127.0.0.1:5984/nonsense"
  },
  "create_target": true,
  "continuous": false
}
```

or, go to http://127.0.0.1:5984/_utils and create a new replication in Fauxton.


You will then see the document is replicated sucessfully but the digests do not match. In the event this was a corrupted transfer a new revision (or full resync) is required to rectify.