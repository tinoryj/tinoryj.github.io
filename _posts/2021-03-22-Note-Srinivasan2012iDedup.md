---
layout: post
title:  "Note: iDedup: Latency-aware, inline data deduplication for primary storage."
date:   2021-03-22 12:00:00
categories: PaperReading
tags: [PrimaryStorage, Deduplication]
---

## Reference

> Kiran Srinivasan, Tim Bisson, Garth Goodson, Kaladhar Voruganti. [iDedup: Latency-aware, inline data deduplication for primary storage](https://www.usenix.org/legacy/event/fast12/tech/full_papers/Srinivasan.pdf). In Proc. of FAST 2012.

## What

An inline deduplication solution iDedup for primary workloads, while minimizing extra IOs and seeks.
<!-- more -->

## Background

* Offline dedupe
  * First copy on stable storage is not deduped
  * Dedupe is a post-processing/background activity
* Inline dedupe
  * Dedupe before first copy on stable storage
  * Primary => Latency should not be affected!
  * Secondary => Dedupe at ingest rate (IOPS)!

The reason to do inline deduplication for primary data.
Provisioning/Planning is easier
• Dedupe savings are seen right away
• Planning is simpler as capacity values are accurate
No post-processing activities
• No scheduling of background processes
• No interference => front-end workloads are not affected
• Key for storage system users with limited maintenance windows
Efficient use of resources
• Efficient use of I/O bandwidth (offline has both reads + writes)
• File system’s buffer cache is more efficient (deduped)
Performance challenges have been the key obstacle
• Overheads (CPU + I/Os) for both reads and writes hurt latency

## Paper Summary

spatial locality exists in duplicated primary data, temporal locality exists in the access patterns of duplicated data. 


## Strength


## Weakness

## Comments
