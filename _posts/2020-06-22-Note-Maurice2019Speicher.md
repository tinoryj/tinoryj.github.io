---
layout: post
title:  "Note : SPEICHER: Securing LSM-based Key-Value Stores using Shielded Execution"
date:   2020-06-22 14:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Maurice Bailleu, Jörg Thalheim, and Pramod Bhatotia, Christof Fetzer, Michio Honda, Kapil Vaswani. [SPEICHER: Securing LSM-based Key-Value Stores using Shielded Execution](https://www.usenix.org/system/files/fast19-bailleu.pdf). In Proc. of FAST, 2019.

## What

SPEICHER is a secure key-value storage system that provides confidentiality and integrity properties, while ensures data freshness to protect against rollback/forking attacks. 

<!-- more -->

## Paper Summary

Shielded execution is mainly designed for securing in-memory computation, but the storage system requires securing the non-volatile state. The main problem is how to use trusted hardware to secure untrusted storage in a stateful setting?

This paper designs SPEICHER to provide confidentiality, integrity, and freshness security properties. The challenges are enclave physical memory size is limited, the storage device is untrusted, the enclave I/O syscall is expensive, and the enclave counter is too slow for the key-value store. 

This work mainly solves these challenges by split the in-memory table's (MemTable) key in memory and value on SSD; Build Merkle tree for an on-disk table (SSTable) based on blocks hash; Use log files (WAL & Manifest, both designed in RocksDB) to guarantee freshness based on the trusted counter (Asynchronous Monotonic Counter [AMC], which defer the counter increment until the data is persisted); Use SPDK library to support fast I/O without existing enclave.

This work implements SPEICHER based on RocksDB and evaluated its performance using the RocksDB benchmark, which shows SPEICHER incurs
reasonable overheads for providing strong security guarantees, while keeping the trusted computing base (TCB) small.


Protection of unauthorized access or tampering is the main problem in cloud storage. Current solutions need expensive cryptographic mechanisms and server-side support.  

## Strength

* Simple and direct system design thesis, the designed system is effective and based on the commercial database engine, with high practicality.

## Weakness

* Insufficient innovation, splitting key-value pairs to reduce memory overhead, using the counter to ensure data freshness, and using SPDK programming to solve the problem of excessive I/O overhead is all the integration of existing methods.
* Lack of security analysis.

## Comments

* About the design of AMC is not detailed enough, how to delay the counter increment?
* Lack of security analysis, how to show that adding SGX support to RocksDB provides better security?