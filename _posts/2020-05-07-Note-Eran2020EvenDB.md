---
layout: post
title:  "Note : EvenDB: optimizing key-value storage for spatial locality"
date:   2020-05-07 12:00:00
categories: PaperReading
tags: [FileSystem]
---

## Reference

> Eran Gilad, Edward Bortnikov, Anastasia Braginsky, Yonatan Gottesman, Eshcar Hillel, Idit Keidar, Nurit Moscovici, Rana Shahout. [EvenDB: optimizing key-value storage for spatial locality](https://dl.acm.org/doi/pdf/10.1145/3342195.3387523). In Proc. of EuroSys, 2020.

## What

EvenDB is an optimized KV-Store for spatially-local workloads, which combines spatial data partitioning with LSM-like batch I/O to achieve high throughput, ensures consistency under multi-threaded access, and reduces write amplification.
<!-- more -->

## Paper Summary

Applications with key-value stores always have spatial locality (e.g., Many data entries have identical composite key prefixes). The basic design of high-throughput KVStore is based on LSM tree, which initially group writes into files temporally rather than by key-range. This approach is not ideal for workloads with high spatial locality because of two reasons:

* Popular key ranges are fragmented across many files, which is generated based on insert time.
* Compaction is costly in terms of both performance (disk bandwidth) and write amplification.

This work modifies traditional KVStore to optimize for current applications' spatial locality. It combines a spatial data organization with LSM-like batch I/O. The pillars of EnvenBD's design are large chunks holding contiguous key ranges. The chunks are dynamically partitioned from keyspace, and it is the basic unit for disk I/O, compaction, memory caching, and concurrency control. At the same time, EvenDB introduces row cache, bloom filters and memory chunk cache (**Munks**) in memory to reduce the probability of accessing the disk as much as possible to improve throughput. The main method to get benefits from the locality is in memory prefix sort. To favor workloads with the spatial locality, EvenDB cache entire popular chunk in Munks, which have a compacted sorted prefix while new updates are appended at the end and remain unsorted until the next compaction.

This work implements the EvenDB prototype and conducts a complete experiment to evaluate the performance compare with RocksDB. It verified that EvenDB is significant performance better than RocksDB in spatially-local workloads, and has smaller write amplification.

## Strength

* Simple design achieves significant performance improvements.

## Weakness

* If the data lacks spatial locality and the active working set is big, EvenDB is ineffective.
* Paper writing is very complicated, which gives a variety of optimization methods but lacks experimental analysis of the effects of each special design.

## Comments

* This work gives row cache to solve the problem of the poor performance of large working data sets with the poor spatial locality, but there is a lack of comparison between row cache and no row cache to clarify the effect of row cache.
* The storage structure in this paper has greater requirements for memory, and it is not compatible with data sets with a poor spatial locality. Can EvenDB be combined with traditional KVStore to achieve better performance in both cases?