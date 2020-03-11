---
layout: post
title:  "Note : WAN-optimized replication of backup datasets using streaminformed delta compression"
date:   2020-03-13 12:00:00
categories: PaperReading
tags: [FileSystem, Compression]
---

## Reference

> P. Shilane, M. Huang, G. Wallace, and W. Hsu, [WAN-optimized replication of backup datasets using streaminformed delta compression](https://static.usenix.org/event/fast/tech/slides/Shilane.pdf), ACM Transactions on Storage (TOS), vol. 8, no. 4, p. 13, 2012.

## What


 <!-- more -->

## Why


## How



## Some Details


## Summary

### Strength

* Theoretical analysis of the cost of each part in system.
* Simple but very effective idea, which makes use of the locality of the chunks and achieve better results than DARE.
* Complete experiments, experimental comparison of each modules' algorithm selection.

### Weekness

* This solution can only be used for the current input data stream, and cannot do deduplication and compression with existing data (Limited effect on long / large data writes).
* For large files, there is a problem that the memory overhead is too large or cannot be used (only the maximum 3GB file was tested in the experiment).