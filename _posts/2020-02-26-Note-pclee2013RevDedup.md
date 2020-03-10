---
layout: post
title:  "Note : RevDedup: A Reverse Deduplication Storage System Optimized for Reads to Latest Backups
"
date:   2020-02-26 12:00:00
categories: PaperReading
tags: [FileSystem, Deduplication]
---

## Reference

> Chun-Ho Ng and Patrick P. C. Lee. [RevDedup: A Reverse Deduplication Storage System Optimized for Reads to Latest Backups](http://www.cse.cuhk.edu.hk/~pclee/www/pubs/apsys13.pdf). In Proc. Of APSYS, 2013.

## What

Design and implement RevDedup that hybird inline and offline deduplication (Inline: Coarse-Grained; Offline: Fine-Grained) and support reverse deduplication in offline approach to optimize read performance of latest backup.
<!-- more -->

## Why

* Inline deduplication deletes the duplicate chunks in the newest backup, lead to higher fragmentation which cause significant reduction in restore performance.
* Offline deduplication results in higher resource overhead, and it needs to be performed with file upload and download peaks, which has limitations.

## How

* Mitigate fragmentation by amortizing disk seeks on reading segments.
* Segment level inline deduplication, remove duplicate segment when file write.
* Chunk level offline deduplication, remove duplicate chunk in old backups:
    * In shared segment, use chunk reference counts
        * Add 1 for all chunks in a segment when referenced by a backup.
        * Minus 1 when a chunk refers to a future version.
        * Remove chunks when reference counts dropped to zero.
    * Remove chunks:
        * Block punching: `fallocate()` to release space to native file system which lead to free-space fragmentation (few chunks remove in segment).
        * Segment compaction: copy all remaining chunks to another segment and delete old segment which will incur I/O overhead (many chunk remove in segment).