---
layout: post
title:  "Note : CoSMIX: A Compiler-based System for Secure Memory Instrumentation and Execution in Enclaves"
date:   2020-05-28 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Orenbach Meni, Baumann, Andrew, Silberstein Mark. [CoSMIX: A Compiler-based System for Secure Memory Instrumentation and Execution in Enclaves](https://www.usenix.org/system/files/atc19-orenbach.pdf). In Proc. of USENIX ATC, 2019.

## What

CoSMIX is a Compiler-based system for *Secure Memory Instrumentation and eXecution* of applications in secure enclaves. It is a memory store abstraction that allows implementation of application-level **secure page fault handlers** that are invoked by a lightweight enclave runtime.
<!-- more -->

## Paper Summary

Hardware secure enclaves are increasingly used to run complex applications. But we can not execute any kind of x86 applications inside enclaves (e.g., memory-mapped files / file mapping / compressed memory). To solve the problem, currently method like ORAM (Oblivious RAM) and SUVM (Software Userspace Virtual Memory, in Eleos) but alought they are generic mechanisms that could be useful in many applications, integrating them with the application logic requires intrusive code modifications.

This work mainly target to solve efficient page fault customization missing while not rely on hardware and do not change the applications. The main idea is to automatically change applicataions with memory instrumentation by build a instrumenting complier (based on llvm) and runtime system.  

In order to design the memory access mode, this work designed Memory Stores (MStore) as another layer of virtual memory on top of an abstract backing store, which take the place of users to use ORAM or SUVM by pointer analysis and use in enclave locality based memory cache to improve the preformance.

At last, this paper do evaluation based on several production applications (e.g., memcached / redis / SQLite / Phoenix suite / Face verification) shows how CoSMIX improves their security and performance by recompiling them with appropriate memory stores. In unmodified Redis and Memcached keyvalue stores, it achieve about 2× speedup by using a self-paging memory store while working on datasets up to 6× larger than the enclave’s secure memory.

## Strength

* Further optimization and improvement of existing work, with the help of the compiler, avoids the inconvenience of modifying the code when using the existing work plan.
* Real application test shows that it works well.

## Weakness

* The work is based on SCONE / Eleos, etc., but the experimental part did not compare its runtime effect.

## Comments

* Only the basic methods were compared, but there was no test to compare the effect of using Eleos runtime with CoSMIX.