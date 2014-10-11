# Runtime Refresh Parameter

## ALIAS (Also Known As) Runtime Refresh Option

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

Default setup Refresh Paramater for ElasticSearch not suitable for every Query / Request.

## Context
(Applicability)

Making a Query / Request to ElasticSearch but wish to change Server Configuration at Runtime for the current Query / Request.

When adding a Document to an Index, it is not Saerchable immediately, *near realtime* is approximately 1sec by default. However, sometimes one requires the new Document to be available immediately.

## Forces
(Motivation)

Applied Server Configuration not suitable for a Query / Request.

## Solution
(Participants, Structure,Collaborations,Implementation)

On Query / Request pass additional Parameters / Options to be applied with that Query / Request.

> To refresh the shard (not the whole index) immediately after the operation occurs, so that the document appears in search results immediately, the refresh parameter can be set to true. Setting this option to true should ONLY be done after careful thought and verification that it does not lead to poor performance, both from an indexing and a search standpoint. Note, getting a document using the get API is completely realtime.

## Example
(Sample Code)

To force a Refresh of an Index one can use `http://localhost:9200/{INDEX-NAME}/_refresh`. However, this is a separate Query. 

For a single query with a forced Refresh:

```
curl -XPOST 'http://localhost:9200/{INDEX-NAME}/{TYPE}?refresh=true' -d '{
    "field" : "value"
}
```

The Default Heartbeat for ElasticSearch is 1sec. This can be changed for an Index & future Queries by using:

```
curl -XPUT 'http://localhost:9200/{INDEX-NAME}' -d '{
{
  "settings": {
    "refresh_interval": "30s" 
  }
}
```

## Resulting Context
(Consequences)

## Rationale
*(optional)*

## Known Uses

There is a performance penalty to forcing a **Refresh**, especially when used more frequently.

## Related Patterns
