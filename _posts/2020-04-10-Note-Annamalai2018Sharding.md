---
layout: post
title:  "Note: Sharding the Shards: Managing Datastore Locality at Scale with Akkio"
date:   2020-04-10 12:00:00
categories: PaperReading
tags: [FileSystem]
---

## Reference

> Muthukaruppan Annamalai, Kaushik Ravichandran, Harish Srinivas, Igor Zinkovsky, Luning Pan, Tony Savor, and David Nagle, Michael Stumm. [Sharding the Shards: Managing Datastore Locality at Scale with Akkio](https://www.usenix.org/system/files/osdi18-annamalai.pdf). In Proc. of USENIX OSDI, 2018.

## What

A locality management service called "**Akkio**", which is layered between client applications and distributed datastore systems.

<!-- more -->

## Why

* Capital and operational costs matter: $2 million per 100 petabytes per month.
* Service request movements: The load of the data center is uneven, and can only choose to expand the capacity when facing hot spots.
* Low read-write ratios: Fully-replicated data would incur significant cross-data center communication, as all replicas would have to be updated on writes.
* Ineffectiveness of distributed caches:
    * In order to ensure the hit rate, the data center cache requires huge hardware input.
    * Low read-write ratios lead to excessive communication over cross-data center links because the data being written will be remote.
    * Require strong consistency. It significantly increases the complexity of the solution, and it incurs a large amount of extra cross-data center communication, further exacerbating WAN latency overheads.
     
## How 

Design and implementation in the context of a single client application service "**ViewState**", which uses ZippyDB as its underlying data storage system.
* System overview
![AKKIO overview](img/paperReading/AKKIO-1.jpg)
* The Akkio Location Service (ALS)
    * Maintains location database.
    * The location database is used on each data access to look up the location of the target µ-shard.
    * The location database is updated when a µ-shard is migrated.
* An Access Counter Service (ACS)
    * Maintains access counter database, which is used to track all accesses so that proper µ-shard placement and migration decisions can be made. 
    * Include the type of access, and the location from which the access was made. This request is issued asynchronously so that it is not in the critical path.
* Data Placement Service (DPS)
    * Decides where to place each µ-shard so as to minimize access latencies and reduce resource usage. 
    * Initiates and manages µ-shard migrations.
    * The Akkio Client Library asynchronously notifies the DPS that a µ-shard placement may be suboptimal whenever a data access request needs to be directed to a remote datacenter(The DPS re-evaluates the placement of a µ-shard only when it receives such a notification).
    * Lock only at the DPS layer for µ-shard migration.
     

## Some Details

* Each "µ-shard" is defined to contain related data that exhibits some degree of access locality with client applications.
    * Cut the original data slice into smaller µ-shard, and ensure that the smaller µ-shard will not span different data slices.
* Design guidelines:
    * Uses an additional level of indirection: it maps µ-shards onto shard replica set collections whose shards are in turn mapped to datastore storage servers. 
    * Structured to keep most operations asynchronous and not on any critical path — the only operation in the critical path is the µ-shard location lookup needed for each data access to identify in which replica set collection the target µ-shard is located. 
    * minimizes the intersection with the underlying application datastore tier (e.g., ZippyDB), which makes it more portable.
* DPS select replica collection for a µ-shard:
    * Choose the one with the highest score from among the available replica set collections, excluding those with replicas in datacenters with exceptionally high disk usage or exceptionally high computing loads.
    * Compute a per-datacenter score by summing the number of times the µ-shard was accessed from that datacenter over the last X days (where X is configurable), weighting more recent accesses more strongly.

## Summary

### Strength

* Facebook commercialization system, the effectiveness of which has been tested by the industry.

### Weakness

* Paper's writing is hard to read and understand.
* Many important details, including how to get the best data storage location, are not clear.
