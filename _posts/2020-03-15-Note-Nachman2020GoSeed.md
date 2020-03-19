---
layout: post
title:  "Note : GoSeed: Generating an Optimal Seeding Plan for Deduplicated Storage"
date:   2020-03-15 12:00:00
categories: PaperReading
tags: [Deduplication, Compression, Database, FileSystem]
---

## Reference

> Nachman Aviv, Yadgar Gala, Sheinvald Sarai. [GoSeed: Generating an Optimal Seeding Plan for Deduplicated Storage](https://www.usenix.org/system/files/fast20-nachman.pdf). In Proc. of USENIX FAST, 2020.

## What

Deduplication decreases the physical occupancy of files in a storage volume by removing duplicate copies of data chunks, but creates data-sharing dependencies that complicate standard storage management tasks. Specifically, data migration plans must consider the dependencies between files that are remapped to new volumes and files that are not. Thus far, only greedy approaches have been suggested for constructing such plans, and it is unclear how they compare to one another and how much they can be improved. We set to bridge this gap for seeding—migration in which the target volume is initially empty. We present GoSeed, a formulation of seeding as an integer linear programming (ILP) problem, and three acceleration methods for applying it to realsized storage volumes. Our experimental evaluation shows that, while the greedy approaches perform well on “easy” problem instances, the cost of their solution can be significantly higher than that of GoSeed’s solution on “hard” instances, for which they are sometimes unable to find a solution at all.


<!-- more -->

## Why

* Deduplication save storage space while create data-sharing dependenices, which complicate storage management task.
* 

## How

## Summary

### Strength

### Weekness
