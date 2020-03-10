---
layout: post
title:  "Note : Efficient distributed backup with delta compression"
date:   2020-03-05 12:00:00
categories: PaperReading
tags: [FileSystem, Compression]
---

## Reference

> Randal C Burns, and Darrell E. Long. [Efficient distributed backup with delta compression](https://dl.acm.org/doi/10.1145/266220.266223). In Proc. Of IOPADS, 1997.

## What

Design and implement a modified version storage system based on version jumping, which could restore delta encoded file versions with at most two accesses to teriary storage to minimizes server workload and network transmission time on file restore. Meanwhile, at client side, a small file stored which contains copies of previously backed up files retains versions in order to generate delta files. <!-- more -->

## Why

* Delta backup is managed with the help of a log system. For some frequently modified data, the log records will be larger than the data increment (similar to the growth rate of metadata in deduplication).
* In delta backup, in order to restore a certain data version, you need to check all versions from the first version to the version that needs to be restored. This operation will cause the restore time to increase linearly.

## How

### **Version Jumping** version control method

* Linear delta chain: $V_1,\Delta(V_1,V_2),\codts,\Delta(V_{i},V_{i+1})$
* Reverse delta chain: $\Delta(V_2,V_1),\codts,\Delta(V_{n},V_{n-1},V_n)$
* Version jumping delta chain: $V_1,\Delta(V_1,V_2),\codts,\Delta(V_{1},V_{i-1}),V_i,\Delta(V_{i},V_{i+1}),\cdots)$

### Delta compression backup system

* Modified based on commercialized system AdStar Distributed Storage Manager (ADSM).

#### Delta Compression Client

* Add differencing and reconstruction algorithms and the addition of a reference file store.
* Add new files directly to the refernece store which is a specially designed LRU cache when backing up.
* Each file in the backup has no overall time difference (simultaneous writing), so the weight is given by calculating the ratio of the file increment to the original file size (the file with the worst delta comression effect is selected), and the one with the higher weight will evict from the cache.

#### Delta Version Server

* Use version jump chain to manage different versions of file.
* Maintain a pointer to the reference file and a reference counter for each income file.
* Delta encoded version points to its reference, and uncompressed file don't has the poniter value.
* Reference counter stores the number of inbound edges to a node which is used for garbage collection.
* When reference counter is 0, delete the node and decrease the counter of its reference.

## Summary

### Strength

* Mathematical axiomatic performance analysis
* In theory, version jumping chain has high practicality, and can be compromised in compression effect and reduction speed.

### Weekness

* Only theoretical analysis was performed, and the ADSM system was proposed to be modified without any testes and analysis.
* Three delta chains are introduced in this article, but only linear delta chain and version jumping delta chain are analyzed in the performance analysis section.
* All effect analysis is limited to theoretical parameter calculations. The motivational dataset used to analyze delta compression is not used for final effect analysis.
* Client handle the majro workload of delta compression, and need to maintain a lot of version reference information and files.
