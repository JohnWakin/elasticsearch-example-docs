# Fields / Source Filtering

[Table of Contents](/README.md)

In page Navigation

* [Problem (Intent)](#problem)
* [Context (Applicability)](#context)
* [Forces (Motivation)](#forces)
* [Solution (Participants, Structure,Collaborations,Implementation)](#solution)
* [Example (Sample Code)](#example)
* [Resulting Context (Consequences)](#resulting-context)
* [Rationale](#rationale) *(optional)*
* [Known Uses](#known-uses) *(optional)*
* [Related Patterns](#related-patterns)

---

## Problem
(Intent)

The Response return from ElasticSearch is too large & only a selection of Properties are required.

## Context
(Applicability)

As the Document stored gets larger the Response size increase.

## Forces
(Motivation)

To make a more efficient HTTP Response. 

## Solution
(Participants, Structure,Collaborations,Implementation)

### Fields *(fields)*

This will flattern the Response into Key/Value pairs.

### Source *(_source)*

This will keep the original structure with the selected data returned. 

Note: If a selected field does not exist in the original Document, it is just ignored

## Example
(Sample Code)

### Fields

#### Request

Specifying the property collection `fields` with the properties you wish returned in the Response.

```json
{ 
    "fields": ["group"],
    "query": { "match_all":{} }
}
```

#### Reponse

This is just 1 result example. Using the Example data or Demo Sandbox 5 results are actually returned.

**Notice that the original structure is lost & the results are flattern.**

```json
[
    {
        "_index":"demo",
        "_type":"internal",
        "_id":"0000-3b19d2dd-91dc-4eb8-be07-dc88ff79f08a",
        "_score":1.0,
        "fields":{
            "group":["beta"],
            "friends.name":[
                "Deloris Parsons",
                "Holman Walton",
                "Charmaine Taylor"
            ]
        }
    }
]
```

### Source

#### Request

```json
{ 
    "_source": ["group", "friends.name"],
    "query": { "match_all":{} }
}
```

#### Response

This is just 1 result example. Using the Example data or Demo Sandbox 5 results are actually returned.

**Notice the original Document format is kept**

```json
{
    "_index":"demo",
    "_type":"internal",
    "_id":"0000-ef06a2f7-6411-4d48-8bce-9d86cb4ccf1b",
    "_score":1.0,
    "_source":{
        "friends":[
            {
            "name":"Deloris Parsons"
            },
            {
            "name":"Holman Walton"
            },
            {
            "name":"Charmaine Taylor"
            }
        ],
        "group":"alpha"
    }
}
```

## Resulting Context
(Consequences)

A smaller and selective Response object is return with all relevant fields.

## Rationale
*(optional)*

## Known Uses

## Related Patterns
