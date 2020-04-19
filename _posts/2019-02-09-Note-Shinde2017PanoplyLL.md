---
layout: post
title:  "Note : PANOPLY: Low-TCB Linux Applications with SGX Enclaves"
date:   2019-02-09 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Shweta Shinde and Dat Le Tien and Shruti Tople and Prateek Saxena. [Panoply: Low-TCB Linux Applications With SGX Enclaves](https://www.semanticscholar.org/paper/Panoply%3A-Low-TCB-Linux-Applications-With-SGX-Shinde-Tien/72acf6830064ee89b955e554013420def6154c12). In Proc. of NDSS, 2017.


## What

PANOPLY provides middleware for SGX and Linux operating systems which has low TCB and support all standard POSIX APIs.
<!-- more -->

## Why

* Enclaves have severely limited capabilities: no native access to system calls and standard OS abstractions. 
* Current systems have a large TCB which leads to too much overhead.
* There are security risks in Multi-Enclave applications.

## How

* Using microns (micro-container) keep libc outside the enclave.
* micron is a unit of application logic which runs on the Intel SGX hardware enclaves.

##### Some Detail

* Evaluation on four real world software (Tor v0.2.5.11 , H2O v2.0.0 , OpenSSL v1.0.1m , FreeTDS v0.95.81 ): 
    * Expressiveness & Security.
    * TCB -> How much TCB reduction achieve over Library OSes
    * Performance -> Perform compared to Library OSes
