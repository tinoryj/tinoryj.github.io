---
layout: post
title:  "Note : SmartDedup: Optimizing Deduplication for Resource-constrained Devices"
date:   2020-03-30 12:00:00
categories: PaperReading
tags: [Deduplication, FileSystem]
---

## Reference

> Qirui Yang, Runyu Jin, and Ming Zhao. [SmartDedup: Optimizing Deduplication for Resource-constrained Devices](https://www.usenix.org/system/files/atc19-yang-qirui.pdf). In Proc. of USENIX ATC, 2019.

## What

SmartDedup achieves significant performance and endurance improvement with low resource cost, which is more suitable for mobile devices.
<!-- more -->

## Why

* Resource are highly limited on edge and IoT: Limited I/O performance, storage capacity, and endurance
* **Main problem**: Is there enough data duplication in device workloads, and how to exploit it using limited resources on the device?

## How

* Analysis smart phone trace ([link](http://visa.lab.asu.edu/traces)), all workloads have good level of deduplication.
* Two-level fingerprint stores: On-disk and In-memory
    * In-memory fingerprint store only containes important fingerprints and organized by prefix tree (first 6 bit, second 6 bit, leaf->fingerprint group)
    * On-disk fingerprint store share same data structure with In-memory part, switch content as fingerprint group level.
* Integrated hybrid deduplication: Out-of-line deduplicates data skipped by in-line
    * In-line deduplication only search with In-memory fingerprint store, and pass non-deduplicated fingerprints to Out-of-line deduplication with *Skipped Buffer*
    * Use page cache to reduce read overhead.
* Dynamically adapted deduplication: According to resource availability and workload characteristics
    * Not occupy too much CPU and stop deduplication when battery at low state
    * Gradually reduce the rate if the duplication level is dropping
    * Quickly increase the rate if the duplication level is growing


## Some Details

* Evaluation with fixed size chunking to make use of page cache.
* Use FIO to test influence of storage performance overhead.

## Summary

### Strength

* Based on real world trace analysis to find the problem.
* Good fingerprint store design to fit for resource limited devices.

### Weekness

* The "Dynamically adapted deduplication" idea is very straight forward, and evaluation on it is not enough (Lack of long-term testing effect).