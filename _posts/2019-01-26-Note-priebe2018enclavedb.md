---
layout: post
title:  "Note : EnclaveDB: A Secure Database using SGX"
date:   2019-01-26 12:30:00
categories: PaperReading
tags: [Database, SGX]
---

## Reference

> Priebe, Christian and Vaswani, Kapil and Costa, Manuel. [EnclaveDB: A Secure Database using SGX](https://www.computer.org/csdl/proceedings/sp/2018/4353/00/435301a405-abs.html). In Proc. of IEEE Symposium on Security and Privacy, 2018.

## What

SGX-based secure cloud database ensures security while dramatically reducing system overhead. It uses SGX security properties to secure database operations.
<!-- more -->
## Why

* The cloud database is continuously attacked by malicious entities, resulting in frequent data leakage.
* Semantic security encryption provides protection for data in static and transit, but the program can still decrypt ciphertext in memory during database query.
* Attribute retention encryption allows query processing of encrypted data, but query capabilities are limited and information leakage is prone to occur.

## How

* In-memory database with enclaves to provide strong security properties.
* combination of encryption -> confidentiality
* native compilation -> integrity
* scalable protocol -> checking integrity and freshness
* Small TCB (trusted computing base) - Over 100X smaller than a conventional database server.


## Some Details

* Compare the key differences between EnclaveDB and traditional SQL database to propose four optimizations
* Experiment with CPU usage, memory and disk throughput under four conditions (the operating conditions are gradually approaching the actual use of EnclaveDB).
    * BASE: Hekaton running outside the enclave
    * CRYPT: EnclaveDB running in simulated enclave mode
    * CRYPT-CALL: adds context switching costs to CRYPT
    * CRYPT-CALL-MEM: increases the cost of memory encryption to CRYPT-CALL.
