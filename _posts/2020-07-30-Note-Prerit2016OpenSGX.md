---
layout: post
title:  "Note: OpenSGX: An Open Platform for SGX Research"
date:   2020-07-30 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Prerit Jain, Soham Desai, Seongmin Kim, Ming-Wei Shih, JaeHyuk Lee, Changho Choi, Youjung Shin, Taesoo Kim, Brent Byunghoon Kang, Dongsu Han. [OpenSGX: An Open Platform for SGX Research](https://cysec.kaist.ac.kr/publications/jain-opensgx.pdf). In Proc. of NDSS, 2016.

## What

An open-source platform for SGX research that consists of a QEMU-based emulator and a software development kit

<!-- more -->

## Paper Summary

The trusted execution environment has rapidly matured over the last decade, like Intel SGX. However, software technology lags behind hardware technology.

This paper establishes the first open platform for SGX research and development. It is an initial exploration of system support, interface design, and the security issues involving system calls and user libraries for SGX programming.

In practice, they use QEMU to implement OpenSGX's OS emulation layer and hardware emulation, such as SGX instructions. It provides a productive development environment allowing the research community to emulate a program running inside an enclave efficiently. It supports instruction-level compatibility to SGX-aware binaries by implementing the core Intel SGX instructions. To enforce EPC access protection, OpenSGX interposes every memory access and checks the execution context and the corresponding access permission. OpenSGX user library provides a dedicated communication channel between an enclave and its host.

The evaluation of OpenSGX demonstrates that it can run nontrivial applications, such as the Tor anonymity network. New ideas can be easily implemented and evaluated as a proof-of-concept using the framework.

## Strength

* Developing a complete platform for SGX development that includes emulated hardware and operating system components, an enclave program loader, an OpenSGX user library, and debugging and performance monitoring support.

## Weakness

* OpenSGX is not secure for any security-related projects.
* Intel SGX has a very restricted set of the I/O channels. It is essential to establish a secure channel between users and the enclave program.

## Comments

* OpenSGX can be utilized or extended for secure development of Intel SGX such as toolchains or a library, precise profiling of SGX programs.
* This work exploration of potential research opportunities beyond the software boundary that the Intel SGX can not flexibly enable.
