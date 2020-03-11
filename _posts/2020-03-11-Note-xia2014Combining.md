---
layout: post
title:  "Note : Combining deduplication and delta compression to achieve low-overhead data reduction on backup datasets"
date:   2020-03-11 12:00:00
categories: PaperReading
tags: [FileSystem, Compression, Deduplication]
---

## Reference

> W. Xia, H. Jiang, D. Feng, and L. Tian, [Combining deduplication and delta compression to achieve low-overhead data reduction on backup datasets](https://cswxia.github.io/pub/dcc-wen-delta-2014.pdf), In Proc. of IEEE DCC, 2014.

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