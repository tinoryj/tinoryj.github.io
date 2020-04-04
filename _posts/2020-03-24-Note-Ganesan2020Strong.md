---
layout: post
title:  "Note : Strong and Efficient Consistency with Consistency-Aware Durability"
date:   2020-03-24 12:00:00
categories: PaperReading
tags: [FileSystem, Dutability]
---

## Reference

> Aishwarya Ganesan, Ramnatthan Alagappan, Andrea Arpaci-Dusseau,and Remzi Arpaci-Dusseau. [Strong and Efficient Consistency with Consistency-Aware Durability](https://www.usenix.org/system/files/fast20-ganesan.pdf). In Proc. of USENIX FAST, 2020. (**Awarded Best Paper**)

## What

Introduce consistency-aware durability(CAD), which is a new approach to durability in distributed storage that enables strong consistency while delivering high performance. And build ORCA, which modified ZooKeeper to implements CAD and cross-client monotonic reads.

<!-- more -->

## Why

Existing durability models cannot achieve both high performance and strong consistency.

## How

* Consistency-aware Durability (CAD):
    * **Shifts the point of durability to reads from writes**: reads do not immediately follow writes – natural in many workloads.
    * A read from a client guaranteed to return at least the latest state returned to a previous read from any client.
    * Does not prevent staleness like many weaker models
* OCRA implement consistency aware durability and cross client monotonic reads in *leader based majority systems*
    * Write path: immediately ack writes, and replication & persistence in background.
    * Read path:
        * Durable-index: index of the latest durable item in the system.
        * Update-index of item i: index of the last update that modified i.
        * Durability check: i durable if update-index of i ≤ durable-index of system.
        * If i's update-index <= durable-index, read immediately. If not, make durable before read.
    * Cross Client Monotonic Reads in ORCA: read only at leader node.

## Some Details

* Consistency models in distributed systems (strong to weak):
    * Linearizability (Immediate durability): must synchronously replicate and persist data on a majority to tolerate failures.
    * Sequential
    * Causal+
    * Causal
    * PRAM (Parallel Random Access Machine)
    * Monotonic reads
    * Eventual: Return write success before synchronously replicate and persist. Many deployments prefer eventual durability for performance (MongoDB, Redis)
* Durability models:
    * Immediate durability: Strong consistency & Slow
    * Eventual durability: Weak consistency & Fast
* Leader based systems (MongoDB, ZooKeeper)
    * Leader: dedicated node
    * Followers: others nodes
    * Writes flow through leader to establishes a single order write
* Majority: data is safe when persisted on majority nodes (like 3 out of 5 servers)


## Summary

### Strength

* Paper writing is excellent and clear with the help of good examples.
* Combining the good side of immediate durability and eventual durability, simple idea but very sufficient.
* Very useful for many deployments that currently adopt eventual durability.

### Weekness

* Not suitable for systems with high real-time performance.