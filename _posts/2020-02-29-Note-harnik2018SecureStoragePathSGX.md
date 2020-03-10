---
layout: post
title:  "Note : Securing the Storage Data Path with SGX Enclaves"
date:   2020-02-29 12:00:00
categories: PaperReading
tags: [FileSystem, SGX]
---

## Reference

> Danny Harnik, Eliad Tsfadia, Doron Chen, and Ronen Kat. [Securing the Storage Data Path with SGX Enclaves](https://arxiv.org/pdf/1806.10883.pdf). ArXiv preprint arXiv:1806.10883, 2018.

## What

Use SGX enclave to improve the security of handling keys and data in storage systems. And compare the performance difference between protecting only keys and protecting both data and keys, while analyze the reasons.
<!-- more -->
## Why

The two main questions:

* Can SGX enclaves achieve comparable performance to running the same operation without enclaves.
* How to get target performance in enclave.

## How

Four kind ways to run program:

* Running in the untrusted area (without enclave).
* Copy and compute: using ECALL with "in" direct option set in the edl file. Data copy from untrusted memory to enclave memory (memcpy & encryption)
* Compute on encrypted memory: prepare data before the ECALL (like using a previous ECALL). No data copy between untrusted memory and enclave memory.
* Compute on cleartext memory: using ECALL with "user_check" direct option set in the edl file. Data not copy and encrypted into enclave memory (enclave control untrusted memory data)

## Some Detail

* Compared `sgxsdk`, `openssl` and `sgxssl`, find performance problem in `sgxssl` (fixed by newer version)
