---
layout: post
title:  "Note: Shielding applications from an untrusted cloud with haven"
date:   2020-07-01 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Baumann Andrew, Peinado Marcus, Hunt Galen. [Shielding applications from an untrusted cloud with haven](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiH1pycwbLqAhXbxYsBHfryCNgQFjAAegQIBRAB&url=https%3A%2F%2Fwww.usenix.org%2Fsystem%2Ffiles%2Fconference%2Fosdi14%2Fosdi14-paper-baumann.pdf&usg=AOvVaw2xBw82LMRSfrePxZijHZ6e). ACM TOCS, 2015.

## What

Haven achieves shielded execution of unmodified legacy applications based on Intel SGX.
<!-- more -->

## Paper Summary

Current cloud computing infrastructure requires substantial trust, but hypervisor vulnerabilities are real in the cloud.

Haven is the first system to achieve shielded execution of unmodified legacy applications on Windows and Intel SGX. It shields applications using mechanisms such as private scheduling, distrustful virtual memory management, and an encrypted and integrity-protected file system.

The main challenges in the design are protecting from a malicious host OS and executing existing binaries in an enclave. Haven shields applications using mechanisms such as private scheduling, distrustful virtual memory management, and an encrypted and integrity-protected file system. The pico process and LibOS enable sandboxing of unmodified Windows applications with comparable security to virtual machines, but substantially lower overheads. It has some limitations: SGX limitations such as exception handling, disallowed instruction, and thread-local storage. It does not currently prevent the rollback of filesystem state beyond the enclave’s lifetime.

For large, complex, CPU and memory-intensive applications such as SQL Server, and for OS-intensive applications like a modern web stack, Haven’s performance penalty vs. a VM is 31–54%, which significant classes of users will readily accept such overheads, in return for not needing to trust the cloud.
Open questions


## Strength

* The first to implement a secure system based on SGX and support running unmodified applications.

## Weakness

* SGX-1 limitations such as exception handling, disallowed instruction, and thread-local storage could not be solved by software.
* Haven does not currently prevent the rollback of filesystem state beyond the enclave’s lifetime.

## Comments

* The work may be improved by diminishing untrusted time and enable cloud management like VM.
* Haven is built on SGX-1, most security problems talked in haven are solved in SGX-2 now.