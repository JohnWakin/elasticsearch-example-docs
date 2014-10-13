# Multiple Aggregation Terms

`Work In Progress`

## ALIAS / AKA: Facets

> Facets are deprecated and will be removed in a future release. You are encouraged to migrate to aggregations instead.

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

Search Results returned are too large to be useful.

## Context
(Applicability)

Applying `Aggregations` (previously named `Facets`) displays the additional optional filters that can be applied to the result set to make them more relevant.

## Forces
(Motivation)

## Solution
(Participants, Structure,Collaborations,Implementation)

## Example
(Sample Code)

### Request

Along with the `query` or `filter` the `terms aggregation` can be applied to the result set returned.

```json
{
    "aggs" : {
        "groups" : {
            "terms" : {
                "field" : "group",
                "size" : 5
            }
        },
        "active" : {
            "terms" : {
                "field" : "isActive"
            }
        }
    }
}
```

## Resulting Context
(Consequences)

### Response

```json
...
"aggregations":{
    "active":{
        "buckets":[
            {
                "key":"F",
                "doc_count":3
            },
            {
                "key":"T",
                "doc_count":2
            }
        ]
    },
    "groups":{
        "buckets":[
            {
                "key":"alpha",
                "doc_count":2
            },
            {
                "key":"beta",
                "doc_count":2
            },
            {
                "key":"gamma",
                "doc_count":1
            }
        ]
    }
}
```

## Rationale
*(optional)*

## Known Uses

## Related Patterns
