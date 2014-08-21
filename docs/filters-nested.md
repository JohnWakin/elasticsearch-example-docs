# Nested Filters

[Table of Contents](/README.md)

In page Navigation

* [Problem](#problem)
* [Context](#context)
* [Context](#context)
* [Forces](#forces)
* [Solution](#solution)
* [Resulting Context](#resulting-context)
* [Rationale](#rationale)
* [Known Issues](#known-issues)
* [Related Patterns](#related-patterns)

---

## Problem
(Intent)

Multiple Nested Filters (Nested Conditions) with `Sort` & `size`.

Similar SQL equivalent:

First example

```sql
...
FROM accounts
WHERE
    (group = "alpha" AND age = 20)
    OR
    (group = "beta" AND age = 20)
```

Alternative

```sql
...
FROM accounts
WHERE
    (group IN ("alpha", "beta") AND age = 20)
```

## Context
(Applicability)

> **Filters** are usually faster than **queries** because:

> * they don’t have to calculate the relevance _score for each document —  the answer is just a boolean “Yes, the document matches the filter” or “No, the document does not match the filter”.
> * the results from most filters can be cached in memory, making subsequent executions faster.

## Forces
(Motivation)

...

## Solution
(Participants, Structure,Collaborations,Implementation)

## Example
(Sample Code)

### Request

> If a query is not specified, it defaults to the match_all query. This means that the filtered query can be used to wrap just a filter, so that it can be used wherever a query is expected

*Performance Tip*: Even if performing a **Search**, exclude as many document as you can with a **Filter**, then query just the documents that remain will be faster.

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

Alternative, using `terms`, this is also equal to

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

## Resulting Context
(Consequences)

### Response

```json
{
   "took":2,
   "timed_out":false,
   "_shards":{
      "total":5,
      "successful":5,
      "failed":0
   },
   "hits":{
      "total":2,
      "max_score":null,
      "hits":[
         {
            "_index":"accounts",
            "_type":"internal",
            "_id":"7-PltOzKQAKMYUjmx8Vv5Q",
            "_score":null,
            "_source":{
               "group":"alpha",
               "guid":"0000-ef06a2f7-6411-4d48-8bce-9d86cb4ccf1b",
               "isActive":false,
               "balance":"$2,319.80",
               "picture":"http://placehold.it/32x32",
               "age":20,
               "eyeColor":"brown",
               "name":"Santos Butler",
               "gender":"male",
               "company":"QUARX",
               "email":"santosbutler@quarx.com",
               "phone":"+1 (824) 535-2312",
               "address":"316 Scott Avenue, Wadsworth, Tennessee, 2775",
               "about":"In minim ea velit culpa sit eiusmod dolor id excepteur commodo exercitation ad. Ipsum cillum culpa commodo elit Lorem eu dolor sint in est fugiat laboris nulla. Ex anim Lorem tempor amet est aute aliqua consequat nostrud esse. Pariatur veniam sit aliquip ex. Ipsum aliqua excepteur eiusmod esse nostrud labore non aliqua minim minim do nulla. Dolore commodo esse amet consequat sint cupidatat velit minim consequat adipisicing.\r\n",
               "registered":"2014-03-01T02:28:58 -00:00",
               "latitude":-40.079357,
               "longitude":-18.527777,
               "tags":[
                  "exercitation",
                  "deserunt",
                  "sit",
                  "esse",
                  "reprehenderit",
                  "nostrud",
                  "in"
               ],
               "friends":[
                  {
                     "id":1,
                     "name":"Deloris Parsons"
                  },
                  {
                     "id":2,
                     "name":"Holman Walton"
                  },
                  {
                     "id":3,
                     "name":"Charmaine Taylor"
                  }
               ],
               "greeting":"Hello, Santos Butler! You have 10 unread messages.",
               "favoriteFruit":"strawberry"
            },
            "sort":[
               "58"
            ]
         },
         {
            "_index":"accounts",
            "_type":"internal",
            "_id":"WII6Nj6sRr69-CNU6Al2fw",
            "_score":null,
            "_source":{
               "group":"beta",
               "guid":"0000-0002a259-df6a-471f-9c27-434675733c9c",
               "isActive":true,
               "balance":"$2,504.78",
               "picture":"http://placehold.it/32x32",
               "age":20,
               "eyeColor":"brown",
               "name":"Velez Elliott",
               "gender":"male",
               "company":"COSMETEX",
               "email":"velezelliott@cosmetex.com",
               "phone":"+1 (958) 483-3451",
               "address":"624 Suydam Street, Lorraine, Alabama, 1183",
               "about":"Quis incididunt laboris ut proident non cillum occaecat veniam commodo dolor est excepteur dolor. Id enim officia deserunt ex sit in. Deserunt excepteur aute ex nulla quis eu aute dolor non do ex sunt. Occaecat cillum anim veniam consequat mollit duis pariatur cillum do. Aliquip velit ea incididunt eiusmod adipisicing exercitation minim. Anim aliqua laborum laborum Lorem id in ullamco quis aliqua.\r\n",
               "registered":"2014-03-02T00:55:33 -00:00",
               "latitude":-51.194669,
               "longitude":123.18493,
               "tags":[
                  "do",
                  "deserunt",
                  "non",
                  "ipsum",
                  "sunt",
                  "occaecat",
                  "tempor"
               ],
               "friends":[
                  {
                     "id":1,
                     "name":"Emilia Patel"
                  }
               ],
               "greeting":"Hello, Velez Elliott! You have 2 unread messages.",
               "favoriteFruit":"strawberry"
            },
            "sort":[
               "55"
            ]
         }
      ]
   }
}
```

## Rationale
*(optional)*

...

## Known Issues

...

## Related Patterns

...

