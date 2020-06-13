---
layout: post
title:  "Note : NEXUS: Practical and Secure Access Control on Untrusted Storage Platforms using Client-side SGX"
date:   2020-06-10 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Djoko Judicael B, Lange Jack, Lee Adam J. [NEXUS: Practical and Secure Access Control on Untrusted Storage Platforms using Client-side SGX](https://dl.acm.org/doi/pdf/10.1145/3361525.3361533). In Proc. of DSN, 2019.

## What

NEXUS is a stackable filesystem, which leverages Intel SGX to provide confidentiality and integrity for user files stored on untrusted platforms. The stackable filesystem is an abstraction layer at kernel level below VFS and above a concrete filesystem which allows pre-processing and post-processing of filesystem operations. 

<!-- more -->

## Paper Summary

Protection of unauthorized access or tampering is the main problem in cloud storage. Current solutions need expensive cryptographic mechanisms and server-side support.  

This work designs NEXUS to balance security portability and performance based on the client-side enclave, and it only modified the client-side application that can use for local or remote storage platform. The main idea is to use the enclave to encrypt all files before uploading it to the cloud and share the encryption key between authorized users inside the enclave. So, the file encryption key never leaked to untrusted memory.

In practice, all cryptographic data protection is performed within an enclave (Isolated Execution), and keys are persisted to disk using SGX sealing facilities on the local machine. Then, before sharing keys with authorized users, use the remote attestation feature to ensure the exchange occurs between valid NEXUS enclaves running on genuine SGX processors. As a result, encryption keys are never leaked to untrusted memory, and as such, kept under the complete control of the NEXUS enclave.

To evaluate the system, this paper implements the system to run on top of the AFS filesystem. Compare the utility, performance, and revocation efficiency by database benchmark and other Linux applications (git, tar, du, grep, cp, mv, etc.), to show that method solves the security issue while provide good performance compare with pure cryptographic techniques.

## Strength

* Simple idea, provide much better performance than pure cryptographic techniques, even compare with no encryption way, it only introduces lightly performance overhead.

## Weakness

* The security analysis is slightly inadequate and does not match the previous adversary model.
* Some lacks in utility experiments, only analysis the Linux application latency, but not include I/O performance.

## Comments

* This design get the help of remote authentication between users, how the solution affects the user experience when attestation request is frequent.
