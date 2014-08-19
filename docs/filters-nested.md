# Nested Filters

[Table of Contents](/README.md)

* [Request](#request)
* [Response](#response)

Nested filters with `Sort` & `size`.

## Request

*Note: It is possible to specify a `type` in the url to filter results `http://localhost:9200/accounts/[TYPE]/_search`*

```json
$ curl -XGET 'http://localhost:9200/accounts/_search' -d '{
    "query" : {
        "filtered" : {
            "filter" : {
                "bool" : {
                    "should" : [
                         {
                            "bool": {
                                "must": [
                                    {
                                        "term": {
                                            "group": "alpha"
                                        }
                                    },
                                    {
                                        "term": {
                                            "age": 20
                                        }
                                    }
                                ]
                            }
                         },
                         {
                             "bool": {
                                 "must": [
                                     {
                                         "term": {
                                             "group": "beta"
                                         }
                                     },
                                     {
                                         "term": {
                                             "age": 20
                                         }
                                     }
                                 ]
                             }
                          }
                    ]
                }
            }
        }
    },
    "sort": {
        "registered": {
            "order": "desc"
        }
    },
    "size": 5
}
'
```

Also using `terms`, this is also equal to

```json
$ curl -XGET 'http://localhost:9200/accounts/_search' -d '{
    "query" : {
        "filtered" : {
            "filter" : {
                "bool" : {
                    "should" : [
                         {
                            "bool": {
                                "must": [
                                    {
                                        "terms": {
                                            "group": ["alpha", "beta"]
                                        }
                                    },
                                    {
                                        "term": {
                                            "age": 20
                                        }
                                    }
                                ]
                            }
                         }
                    ]
                }
            }
        }
    },
    "sort": {
        "registered": {
            "order": "desc"
        }
    },
    "size": 5
}
'
```

## Response
