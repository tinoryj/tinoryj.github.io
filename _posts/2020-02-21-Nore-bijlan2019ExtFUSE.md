---
layout: post
title:  "Note : Extension Framework for File Systems in User space"
date:   2020-02-21 12:00:00
categories: PaperReading
tags: [FileSystem, System]
---

## Reference

> Ashish Bijlani and Umakishore Ramachandran. [Extension framework for File systems in User space](https://www.usenix.org/system/files/atc19-bijlani.pdf). In Proc. Of ATC, 2019.

## What

Design and implement ExtFUSE which is an extension framework for user file systems that offers the performance of kernel file systems, while retaining the safety properties of user file systems.
<!-- more -->

## Why

FUSE need frequent user-kernel round-trip communication and data copying, and the overhead is higher with faster storage media (83% overhead for metadata-heavy workloads on SSD).

Current Solutions like Zero-copy Data Transfer (Splicing) could only used for over 4K requests, while small writes/reads will incur data copying overheads; Utilizing system-wide shared VFS caches could improves I/O throughput by batching requests, but no help to random reads while introduce higher write latency.

The biggest problem: how to balancing safety and performance between user space file system and kernel space file system?
  
## How

* Using eBPF (Extended Berkeley Packet Filters) to cache metadata in kernel and minimize the content switch between user space and kernel space.
    * eBPF can check the code and information entering the kernel to ensure kernel security.
    * Cache operation instructions and metadata to reduce the overhead of copying data from the kernel space to the user space.
    * Reduce CPU usage for user space file system.
* Direct passthrough read/write operation to lower file system (e.g. ext4) to reduce latency.

## Some Details

* The performance overhead and higher latency of the user file system mainly comes from the CPU occupation caused by data frequently crossing user space and kernel space.
* File system test important data set: [FileBench](https://github.com/filebench/filebench/wiki).
* Strategies such as read-ahead-delay-write may cause small file performance degradation and higher operation latency
* In combination with existing widely used systems, modification of the original system should be avoided as much as possible, and it should be done in the form of additional components.
