---
layout: post
title:  "Note: Improving restore speed for backup systems that use inline chunk-based deduplication."
date:   2021-04-28 12:00:00
categories: PaperReading
tags: [Deduplication]
---

## Reference

> Mark Lillibridge, Kave Eshghi, Deepavali Bhagwat. [Improving restore speed for backup systems that use inline chunk-based deduplication](https://www.usenix.org/system/files/conference/fast13/fast13-final124.pdf). In Proc. of FAST 2013.

## Problem Statement

* Slow restore performance due to chunk fragmentation
    * Store chunks according to the order of appearance of unique chunks.
    * Restore the newer snapshot, the more random reads, the lower the performance.

<!-- more -->

## Literature Summary

## Approaches

## Results

