---
layout: post
title:  "Note: HardIDX: Practical and Secure Index with SGX"
date:   2020-07-17 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Benny Fuhry, Raad Bahmani, Ferdinand Brasser, Florian Hahn, Florian Kerschbaum, Ahmad-Reza Sadeghi. [HardIDX: Practical and Secure Index with SGX](http://www.fkerschbaum.org/dbsec17a.pdf). In Proc. of DBSec, 2017.

## What

HardIDX is a hardware-based approach for search over encrypted data based on Intel SGX.
<!-- more -->

## Paper Summary

Search over encrypted data based on software approaches is still challenged by a lack of proper, low-leakage encryption, or slow performance. Existing hardware-based approaches do not scale well (due to hardware limitations and software designs are not optimized for hardware), and are not well analyzed for their security.  

This paper introduces HardIDX which improves the performance compare with software approaches and improves security and scalability compare with existing hardware approaches.

Specifically, this paper provides a search service through the enclave to ensure that the server cannot obtain any information about the query. At runtime, the enclave loads all (or part) of the database search tree (B+ tree) into the enclave memory and decrypts the user's search request inside enclave to complete the search. The search tree is called a hardware secured B+ tree (HSBT), which could use enclave keys to encrypt the search tree.

They evaluate the performance of query 1e6 key-value pairs compared with existing approaches (better than software-based approaches) and give security analysis to show HardIDX could prevent side-channel leakages (better than current hardware-based approaches).

## Strength

* A compromise was made in software and hardware solutions, resulting in stronger security than existing hardware solutions and better performance than existing software solutions.

## Weakness

* The design is too direct and the memory management description is unclear.

## Comments

* The system designed by the scheme is very simple and the effect is acceptable.
* There is no more efficient arrangement in memory management, which may even lead to the disclosure of some information (current security is based on the random insertion of keys in the B+ tree instead of ordered insertion).