---
layout: post
title:  "Note : OBLIVIATE: A Data Oblivious File System for Intel SGX"
date:   2019-01-26 12:00:00
categories: PaperReading
tags: [SGX, FileSystem]
---

## Reference

> Ahmad, Adil and Kim, Kyungtae and Sarfaraz, Muhammad Ihsanulhaq and Lee, Byoungyoung [OBLIVIATE: A Data Oblivious File System for Intel SGX](http://wp.internetsociety.org/ndss/wp-content/uploads/sites/25/2018/02/ndss2018_06A-2_Ahmad_paper.pdf).  internetsociety, 2018.


## What

OBLIVIATE redesigned ORAM for SGX filesystem operations for confuse access patterns to protect user privacy. It is a data oblivious filesystem for Intel SGX which adapting the ORAM protocol to read and write data from a file within an SGX enclave. It supports SGX programs without changes in application layer. <!-- more -->

## Why
 
All existing SGX filesystems are vulnerable to system call snooping, page fault, or cache based side-channel attacks.

## How

* Run isolated filesystem enclave in a separate process and using encrypted communication channels to communicate with applications.
* Using message queues and shared memory for intra-process and inter-process communication. 
* ORAM implementation is exposed to side-channel attacks against the enclave. -> Use data oblivious algorithms in accessing key data structures of ORAM.
* Maintain ORAM server storage efficiently -> Additional security memory region with non-encrypted memory regions of SGX (Avoid costly context switches).
* Reduce ORAM latency -> Asynchronous ORAM server update (Returns the required data when available and performs path updates asynchronously, rather than waiting for expensive ORAM path updates).


## Some Detail

* Introduce three current SGX Filesystem with their limitations.
* Test current SGX filesystem with `Syscall Snooping Attack`, `Page Fault based Attack`, `Cache Based Attacks` to show their hidden dangers -> Lead to the design of OBLIVIATE.
* Evaluation
    * Security test
    * Micro Benchmark -> Running Speed, Overhead, Optimization impact.
    * Macro Benchmark -> Compare OBLIVIATE and other filesystems on **real world** test: SQLite & Lighttpd.
