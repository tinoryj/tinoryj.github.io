---
layout: post
title:  "Note: iDedup: Latency-aware, inline data deduplication for primary storage."
date:   2020-10-14 12:00:00
categories: PaperReading
tags: [FileSystem, Deduplication]
---

## Reference

> Kiran Srinivasan, Tim Bisson, Garth Goodson, Kaladhar Voruganti. [iDedup: Latency-aware, inline data deduplication for primary storage](https://static.usenix.org/events/fast/tech/full_papers/Srinivasan2-10-12.pdf). In Proc. of FAST, 2012.

## What

IDedup is an inline/foreground dedupe technique for primary data with minimal impact on latency-sensitive workloads.

<!-- more -->

## Background

* Offline deduplication : First copy on stable storage is not deduped, so that dedupe is a post-processing/background activity.
* Inline deduplication :  Dedupe before first copy on stable storage.
  * Primary data : Latency should not be affected. 
  * Backup data : Dedupe at ingest rate (IOPS).


## Paper Summary

Primary workloads are typically read-intensive, but deduplication causes disk-level fragmentation, which leads sequential reads turn random and introduce more latency in read path. Meanwhile, the deduplication algorithm add more metadata lookups and updates in write path.

This work design iDedup to get a good tradeoff between capacity saving and latency perormance. First, it perform deduplication to only sequences of disk blocks

## Strength

## Weakness  

## Comments
