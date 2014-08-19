# Load Example Data

[Table of Contents](/README.md)

Prerequisite

* **Curl** installed
* **Elastic Search** installed

### Accounts

1. Run the following commands from the *root* of this **Project Repository**

```
$ curl -XPOST 'localhost:9200/accounts/internal' --data-binary @data/account-1.json
$ curl -XPOST 'localhost:9200/accounts/internal' --data-binary @data/account-2.json
$ curl -XPOST 'localhost:9200/accounts/internal' --data-binary @data/account-3.json
$ curl -XPOST 'localhost:9200/accounts/internal' --data-binary @data/account-4.json
$ curl -XPOST 'localhost:9200/accounts/internal' --data-binary @data/account-5.json
```

2. Check data in **Elastic Search**

Run command:

```
curl 'localhost:9200/_cat/indices?v'
```

Output should be similar to:

```
health index pri rep docs.count docs.deleted store.size pri.store.size
yellow accounts    5   1       5            0    45.1kb        45.1kb
```
