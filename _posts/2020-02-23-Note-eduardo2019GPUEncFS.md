---
layout: post
title:  "Note : Speculative Encryption on GPU Applied to Cryptographic File Systems"
date:   2020-02-23 12:00:00
categories: PaperReading
tags: [FileSystem, Encryption, GPU]
---

## Reference

> Vandeir Eduardo, Wagner M. Nunan Zola, and Luis C. Erpen de Bona. [Speculative Encryption on GPU Applied to Cryptographic File Systems](https://www.usenix.org/system/files/fast19-eduardo.pdf). In Proc. Of FAST, 2019.

## What

A CTR-based AES cryptographic file system that effectively utilizes the highly parallel capabilities of the GPU.
<!-- more -->
## Why

### The problem with CFS

* CFS use symmetric block ciphers (good security/speed ratio)
* The following festures lead to higher CPU utilization:
    * Larger data volumes
    * faster media
    * alternative ciphers
    * larger keys

### Why CTR mode

* The most suitable for parallel processing.
* Similar security to CBC.
* Allows speculative encryption (creation encryption masks ahead of time)
* XOR on CPU (avoids CPU to GPU data transfer)

But there are problems:

* Each recording and rewriting of a block requires a new nonce (IV) which need to store.
* Overhead of nonce storage could negatively impact CFS performance (Storage format; Access mechanism; Granularity)

## How

### EncFS++

* Based on FUSE which is in user space
* CUDA API is in user space, (if running in kernel space, it need an intermediate process to use which lead to higher complexity and latency)

### Encryption Contexts Management

* Nonce Storage : Same format with linux file system inode
* Encryption and decryption of content pool, similar to sliding window to generate encryption mask (Increase the counter value when the window is sliding, update the counter in ctr mode)

## Some Details

* XOR on CPU (avoids CPU to GPU data transfer)
