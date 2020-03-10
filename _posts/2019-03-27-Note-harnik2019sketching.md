---
layout: post
title:  "Note : Sketching Volume Capacities in Deduplicated Storage"
date:   2019-03-27 12:00:00
categories: PaperReading
tags: [FileSystem, Deduplication, Sketch]
---

## Reference

> Harnik, Danny and Hershcovitch, Moshik and Shatsky, Yosef and Epstein, Amir and Kat, Ronen. [Sketching Volume Capacities in Deduplicated Storage](https://www.usenix.org/system/files/fast19-harnik.pdf). In Proc. of FAST, 2019.

## What

* Sketch-based estimations of fundamental capacity measures required for managing storage system(Whole Flash System)
    * Managing storage capacities in storage systems with deduplication.
    * Capacity queries for volume groups across multiple storage systems.
    * 1 PB Volume, metadata using 10TB, sketching using 300MB.
<!-- more -->
## Why
Deduplication challenges storage management: Physical capacities associated with volumes are no longer readily available. 

1. Cross-volume deduplication: no longer clear which volume owns what data.

2. Computational challenge:
    * No deduplication -> aggregated per each volume.
    
    * With deduplication-> Volume capacity are affected by any write operation to any volume in the system.

## How

Realm of streaming algorithms in which the metadata of each volume is sampled using a content-based sampling technique to produce a so-called sketch (or capacity sketch) of the volume. 
