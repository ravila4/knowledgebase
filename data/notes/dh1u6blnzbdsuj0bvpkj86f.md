
## API

Some examples of common API uses.

### Create index

`curl -X PUT "localhost:9200/user_genesets?pretty"`

### Delete index

`curl -X DELETE "localhost:9200/user_genesets?pretty"`

### Set index alias

```bash
curl -XPOST 'http://localhost:9200/_aliases' -d '{
    "actions" : [
        { "add" : { "index" : "mygeneset_20210323_k1udhp5n",
          "alias" : "mygeneset_current" } }
    ]
}' -H "Content-Type: application/json"
```
### See list of indices

`curl -x GET 'http://localhost:9200/_cat/indices'`

### Search for documents

In general, we use the endpoint:

`curl -X GET "localhost:9200/index/_search"`

for example (the parameter 'pretty' requires return type `application/json`):

```bash
curl -X GET "es8.biothings.io:9200/my_index/_search?&pretty" \
-H 'Content-Type: application/json' -d'
{
  "query": {
    "term": {
      "source": "smpdb"
    }
  }
}
'
```

### Insert document

### Modify document

### Delete document