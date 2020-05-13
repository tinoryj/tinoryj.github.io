---
layout: post
title:  "Note : AniFilter: Parallel and Failure-Atomic Cuckoo Filter for Non-Volatile Memories"
date:   2020-05-07 12:00:00
categories: PaperReading
tags: [FileSystem]
---

## Reference


> Hyungjun Oh, Bongki Cho, Changdae Kim, Heejin Park, Jiwon Seo. [AniFilter: Parallel and Failure-Atomic Cuckoo Filter for Non-Volatile Memories](https://dl.acm.org/doi/pdf/10.1145/3342195.3387528). In Proc. of EuroSys, 2020.

## What

AniFilter (AF) is an optimized persistent Approximate Membership Query (AMQ) for NVM, which is based on Cuckoo Filter (CF).
<!-- more -->

**Approximate Membership Query (AMQ)**: space-efficient probabilistic data structure used to test whether an element is a member of a set.

## Paper Summary

AMQ data structures are widely used in storage systems. With the development of Non-Volatile Memory (NVM) technologies, AMQ data structures need to fit NVM's byte-addressable and high-performance features.

This work proposed AniFilter (AF), which based on Cuckoo Filter (CF) to improve insertion throughput on NVM. CF is a space-efficient probabilistic data structure that is used to test whether an element is a member of a set like a Bloom filter does but support element eviction. When used for NVM, it's typical bucket size 6 byte is not suitable for NVM's 8-byte atomic write unit, and its eviction overhead in high load factors(>75%) is worse in NVM because of higher latency.

It mainly used three techniques to optimize CF for NVM. The "Spillable Buckets" method allows fingerprint (FP) insert for each bucket store at a neighboring bucket, which could still fit the cache line size to limit eviction operations (only use the first slot for spill). The "Lookaheah Eviction" evict a fingerprint that does not incur further eviction, which use an occupancy flag to help select an FP to evict by enumerating the FPs in the bucket and evict the one whose alternate bucket is not full according to the occupancy flags. The "Bucket Primacy" makes one of the two hashes buckets as the prime bucket, only access secondary bucket when the prime bucket has no matching or free space.

For the three optimization schemes, this work gives the model and theoretical analysis and implements the available prototypes. Finally, they evaluate it on a real NVM device (Intel Optane) by comparing it with four existing filters for insert & search queries.

## Strength

* Simple and effective idea, good practice effect

## Weakness

* It would be better if the comparison and testing of logging and existing solutions are more detailed

## Comments

* Since the atomic write unit of NVM is 8byte, why not directly modify the size of each bucket of the filter to directly match NVM?