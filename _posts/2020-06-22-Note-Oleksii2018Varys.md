---
layout: post
title:  "Note : Varys: Protecting SGX Enclaves from Practical Side-Channel Attacks"
date:   2020-06-22 12:38:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Oleksii Oleksenko, Bohdan Trach, Robert Krahn, André Martin, Mark Silberstein, Christof Fetzer. [Varys: Protecting SGX Enclaves from Practical Side-Channel Attacks](https://www.usenix.org/system/files/conference/atc18/atc18-oleksenko.pdf). In Proc. of USENIX ATC, 2018.

## What

Varys is a system that protects unmodified programs running in SGX enclaves from cache timing and page table side-channel attacks. 

<!-- more -->

## Paper Summary

Intel SGX is vulnerable to cache timing and page table side-channel attacks. Existing methods that protect against these attacks incur high-performance overhead or require significant code modifications. The main problem is the gap between low-performance overhead and code modification to run programs in the enclave.

This paper designs Varys which incurs small performance overhead while require no code changes to protect these attacks. The main idea is to create an isolated environment and verify it at runtime. 

The known page table and L1/L2 cache timing side-channel attacks (SCAs) on SGX rely on an abnormally high rate of asynchronous enclave exit, or/and an adversary controlled sibling hyperthread on the same physical core. The main idea to prevent SACs, Varys monitors when asynchronous enclave exits (AEX, for scheduling another process on the core or handling an exception) occur, and restricts the frequency, while keep threads run on same physical core belongs to same enclave program. 

It implements an LLVM plugin and runtime library for the code compiler (on SCONE) to achieve on code modification requires. It asks OS for lower interrupt rate, paired thread allocation, and evict caches on enclave exits.

To evaluate Varys, they run a performance test to evaluate runtime overhead and multithreading performance and set up a case study based on Nginx. They also conducted a security analysis for Varys.

## Strength

* For the first time, both no code modification requirements and low-performance overhead goals were achieved.
* Comprehensive analysis of page table-based and cache timing-based SACs, and put forward solutions, the final security analysis is perfect.

## Weakness

* A very complex paper with perfect functions, which can also be an advantage.

## Comments

* As an experimental comparison, can it be compared with the two existing works of running unmodified code in enclave?
