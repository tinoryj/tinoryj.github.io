---
layout: post
title:  "Note: Austere Flash Caching with Deduplication and Compression"
date:   2020-09-24 12:00:00
categories: PaperReading
tags: [Cache, FileSystem, Compression, Deduplication]
---

## Reference

> Wang Qiuping, Li Jinhong, Xia Wen, Kruus Erik, Debnath Biplob, Lee Patrick PC. [Austere Flash Caching with Deduplication and Compression](https://www.usenix.org/system/files/atc20-wang-qiuping.pdf). In Proc. of USENIX ATC, 2020.

## What

A flash cache mechanism for accelerating HDD based on deduplication and compression with less memory overhead.

* Flash Cache: Accelerate HDD storage by caching frequently accessed blocks in flash (SSD/PM).

<!-- more -->

## Paper Summary

Flash cache can significantly improve the IO performance of the disk system, but the space efficiency and endurance remains a critical challenging due to Nand flash itself (Cost per GB, rewritable times). Deduplication and compression could saving storage space and I/O operation via remove duplicate content, but incur substantial memory overhead for index management. 

This paper build AustereCache to support memory-efficient indexing while preserving data benefits of deduplication and compression in flash cache.

The main idea of AustereCache is hashing to partition index and cache space which use the prefix index in the memory and the complete index in the flash to reduce the memory index space overhead; Use the bucket-based cache scheduling replaces the scheduling for entire cache to optimize the efficiency of multi-threaded access; Combined with compression and  deduplication to reduce the storage space of data in the cache.


We propose AustereCache, a new flash caching design that aims for memory-efficient indexing, while preserving the data reduction benefits of deduplication and compression. AustereCache emphasizes austere cache management and proposes different core techniques for efficient data organization and cache replacement, so as to eliminate as much indexing metadata as possible and make lightweight in-memory index structures viable. Trace-driven experiments show that our AustereCache prototype saves 69.9-97.0% of memory usage compared to the state-of-the-art flash caching design that supports deduplication and compression, while maintaining comparable read hit ratios and write reduction ratios and achieving high I/O throughput.


The trusted execution environment has rapidly matured over the last decade, like Intel SGX. However, software technology lags behind hardware technology.

This paper establishes the first open platform for SGX research and development. It is an initial exploration of system support, interface design, and the security issues involving system calls and user libraries for SGX programming.

In practice, they use QEMU to implement OpenSGX's OS emulation layer and hardware emulation, such as SGX instructions. It provides a productive development environment allowing the research community to emulate a program running inside an enclave efficiently. It supports instruction-level compatibility to SGX-aware binaries by implementing the core Intel SGX instructions. To enforce EPC access protection, OpenSGX interposes every memory access and checks the execution context and the corresponding access permission. OpenSGX user library provides a dedicated communication channel between an enclave and its host.

The evaluation of OpenSGX demonstrates that it can run nontrivial applications, such as the Tor anonymity network. New ideas can be easily implemented and evaluated as a proof-of-concept using the framework.

## Strength

* Developing a complete platform for SGX development that includes emulated hardware and operating system components, an enclave program loader, an OpenSGX user library, and debugging and performance monitoring support.

## Weakness

* The size of the index structure is directly related to the total size of the hard disk for caching, so the scalability is weak.
* Use the slice+padding method to divide the compressed variable-length chunk into n fixed-length subchunks, which wastes part of the flash space.

## Comments

* Is it possible to replace the existing slice+padding method with a compression technology with a determined output length to achieve the same effect without wasting space? Reference: Gao Xiang, Dong Mingkai, Miao Xie, Du Wei, Yu Chao, Chen Haibo. [EROFS: A Compression-friendly Readonly File System for Resource-scarce Devices](https://www.usenix.org/system/files/atc19-gao.pdf). In Proc. of USENIX ATC, 2019.

