---
layout: post
title:  "Note : Strong and Efficient Consistency with Consistency-Aware Durability"
date:   2020-03-24 12:00:00
categories: PaperReading
tags: [Tunning, FileSystem]
---

## Reference

> Aishwarya Ganesan, Ramnatthan Alagappan, Andrea Arpaci-Dusseau,and Remzi Arpaci-Dusseau. [Strong and Efficient Consistency with Consistency-Aware Durability](https://www.usenix.org/system/files/fast20-ganesan.pdf). In Proc. of USENIX FAST, 2020. (**Awarded Best Paper**)

## What

Introduce consistency-aware durability(CAD), which is a new approach to durability in distributed storage that enables strong consistency while delivering high performance. And build ORCA, which modified ZooKeeper to implements CAD and cross-client monotonic reads.

<!-- more -->

## Why

Existing durability models cannot achieve both high performance and strong consistency.

## How



## Some Details

* Consistency models in distributed systems (strong to weak):
    * Linearizability
    * Sequential
    * Causal+
    * Causal
    * PRAM (Parallel Random Access Machine)
    * Monotonic reads
    * Eventual
* Durability models:
    * Immediate durability: Strong consistency & Slow
    * Eventual durability: Weak consistency & Fast

## Summary

### Strength



### Weekness
