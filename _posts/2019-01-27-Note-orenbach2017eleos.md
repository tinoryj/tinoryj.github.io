---
layout: post
title:  "Note : Eleos: ExitLess OS Services for SGX Enclaves"
date:   2019-01-27 12:00:00
categories: PaperReading
tags: [SGX, System]
---

## Reference

> Orenbach, Meni and Lifshits, Pavel and Minkin, Marina and Silberstein, Mark. [Eleos: ExitLess OS services for SGX enclaves](https://dl.acm.org/citation.cfm?id=3064219). In Proc of the Twelfth European Conference on Computer Systems, 2017. 

## What

Eleos enabling exit-less system calls and exit-less paging in enclaves to tackle performance issues in SGX applications. 
<!-- more -->

## Why

* Running I/O-intensive, memory-demanding server applications in en-claves leads to significant performance degradation. 
* Main reason for the application slowdown with SGX is substantial load on the in-enclave system call and secure paging mechanisms.
* Other reason for slowdown:
    * Thousands-of-cycles long SGX management instructions.
    * Enclave exits cost too high due to associated TLB flushes and processor state pollution.

## How

* Reduced cache pollution due to system calls -> Limiting the LLC space available to the RPC thread using the Cache Allocation Technology.
* Application-managed paging -> User-level library SUVM: per-enclave page table and page cache in EPC along with a secure backing store in host memory.
* Low-overhead software address translation -> Memory accesses via spointers resolve to the SUVM page cache or trigger a software page fault to a page in evicted pages. 
* Graceful handling of multiple enclaves -> All enclaves share the same PRM, so SUVM coordinates the size of its page cache with the SGX driver to avoid thrashing when new enclave invocation. 
* Optimized eviction and memory access policies -> Exposing SUVM management to the application. 
    * preventing write back of clean pages to the backing store.
    * providing direct access to the backing store at sub-page granularity. 

## Some Details

* Analyze the operational overhead of the various components of the system before system design
* Evaluate end-to-end by two real server applications: memcached and face verification (Modify origin code).
* Evaluate the RPC and SUVM mechanisms on several microbenchmarks (Cost in different usage scenarios).
