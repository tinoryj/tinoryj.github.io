---
layout: post
title:  "Note : Wormhole: A Fast Ordered Index for In-memory Data Management"
date:   2020-04-18 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Xingbo Wu, Fan Ni and Song Jiang. [Wormhole: A Fast Ordered Index for In-memory Data Management](https://dl.acm.org/doi/pdf/10.1145/3302424.3303955?download=true). In Proc. of EuroSys, 2019.

## What

A new ordered index structure, named **Wormhole**, which takes O(log L) worst-case time for looking up a key with a length of L.

The low cost is achieved by simultaneously leveraging strengths of three indexing structures, namely hash table, prefix tree, and B+ tree, to orchestrate a single fast ordered index. Wormhole’s range operations can be performed by a linear scan of a list after an initial lookup. This improvement of access efficiency does not come at a price of compromised space efficiency. Instead,Wormhole’s index space is comparable to those ofB+ tree and skip list. Experiment results show that Wormhole outperforms skip list, B+ tree, ART, and Masstree by up to 8.4×, 4.9×, 4.3×, and 6.6× in terms ofkey lookup throughput, respectively.

<!-- more -->

## Why

Problems with current index for in-memory data management systems (e.g., Key-value store):

* Unordered indexes - Hash Table:
    * Point search with O(1) time
    * Cannot be used when require range queries
* Ordered indexes - B+ Tree & Skip List:
    * Point search with O(log N) time, where N is number of keys in an index
    * Support range queries
    * For an ordered index hosting billions of keys, it may take more than 30 key-comparisons in a lookup

## Main Idea


## How





## Some Details


## Summary

### Strength

### Weekness
