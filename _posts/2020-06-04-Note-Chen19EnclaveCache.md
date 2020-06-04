---
layout: post
title:  "Note : EnclaveCache: A Secure and Scalable Key-value Cache in Multi-tenant Clouds using Intel SGX"
date:   2020-06-04 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Chen Lixia, Li Jian, Ma Ruhui, Guan Haibing, Jacobsen Hans-Arno. [EnclaveCache: A Secure and Scalable Key-value Cache in Multi-tenant Clouds using Intel SGX](https://dl.acm.org/doi/pdf/10.1145/3361525.3361533). In Proc. of Middleware, 2019.

## What

EnclaveCache is a multi-tenant key-value cache that provides data confidentiality and privacy based on SGX. It utilizes **multiple SGX enclaves** to enforce data isolation among co-located tenants, and designed a key distribution procedure to ensures that a tenant-specific encryption key is securely guarded by an enclave to perform cryptography operations towards tenant’s data.

<!-- more -->

## Paper Summary

In-memory key-value caches have been widely used to speed up web applications and reduce the burden on backend databases, but the major concerns are the security and confidentiality of users’ data stored in multi-tenant clouds. Traditional cache management strategies (virtualization, containerization) could not solve that problem. But if use SGX, how to utilize SGX enclaves to secure key-value caches within the limited trusted memory, remains an open problem.

This work designed EnclaveCache to solve the two main problems, multi-tenant isolation, and cloud trust issues. It protects users’ data against both co-located tenants and privileged cloud providers. Because the enclave memory and resource limitations, EnclaveCache designed to only store the sensitive information and the critical code into enclaves, and provide a dedicated enclave for each tenant (require the client to provide the message encryption key to enclave). Similarly, to reduce the enclave remote attestation overhead, the client will temporarily store the session keys. For the query process, part of query information (have sensitive data) are encrypted by enclave, the other data are discarded as plaintext to use the untrusted database.

To evaluate the system, this paper implements the system in switchless and switch-mode(switchless a feature in SGX 
SDK 2.2 which let enclave ECALLs/OCALLs be performed asynchronously), and compared with Graphene-SGX. They tested the performance with different value size, workload. And the CPU hotspot with Intel VTune. Finally, they tested the scalability for up to 40 clients.

## Strength

* Simple idea, provide much better performance than existing work Graphene-SGX.

## Weakness

* Thesis writing in general, and the questions about the enclave management are unclear.
* There are few descriptions of data privacy protection, and the lack of corresponding security analysis

## Comments

* The EnclaveCache in this paper uses multiple enclaves to serve different users, and gives a comparison of the implementation and testing of switchless, but there is no way to know how switchless is implemented for many enclaves.
