---
layout: post
title:  "Note: SecureKeeper: Confidential ZooKeeper using Intel SGX"
date:   2020-07-24 12:00:00
categories: PaperReading
tags: [SGX, Database]
---

## Reference

> Stefan Brenner, Colin Wulf, David Goltzsche, Nico Weichbrodt, Matthias Lorenz, Christof Fetzer, Peter Pietzuch, and Rüdiger Kapitza. [SecureKeeper: Confidential ZooKeeper using Intel SGX](https://weichbrodt.me/_media/pubs:2016-middleware-brenner-securekeeper.pdf). In Proc. of Middleware, 2016.

## What

SecureKeeper proposed a secure ZooKeeper with SGX storing the coordination data in an untrusted region.
<!-- more -->

## Paper Summary

Cloud computing requires trust in data. Especially third-party cloud computing is accompanied by potentially sensitive application data.

SecureZookeeper is an enhanced version of zookeeper with the usage of SGX to protect the confidentiality and integrity of sensitive data. It uses multiple small enclaves to ensure that (i) user-provided data in ZooKeeper is always kept encrypted while not residing inside an enclave, and (ii) essential processing steps that demand plaintext access can still be performed securely.

The security guarantees of SecureKeeper are based on an integration of two enclave types: (i) an entry enclave is instantiated for each client and responsible for protecting the client-replica connection and the data security of the ZooKeeper data store; (ii) a counter enclave is instantiated only once at the leader replica of the ZooKeeper cluster and handles special write requests of sequential nodes that perform data processing. SecureKeeper takes a middle ground and stores and processes data in encrypted form where feasible and redirects plaintext processing to enclaves. To solve that memory usage problem in zookeeper when using it inside the enclave, the implements of SecureKeeper using tailored enclaves and only move critical code sections into enclaves, so that the memory is smaller when processing data.

They evaluate the SecureKeeper by comparing it with the two versions of the original Zookeeper. They focus on throughput, Fault Tolerance, and Memory Consumption to show that SecureKeeper provides comparable performance with original ZooKeeper while keeping data secure.

## Strength

* SecureKeeper demonstrates how tailored enclaves can be integrated minimal-invasively to ensure confidentiality of all managed user data and integrity for plain nodes.
* The protection of SecureKeeper keeps the security of all user-provided data in zookeeper databases.

## Weakness

* The design part is separate from the SecureKeeper section, which is a little mess.
* The implement is not that detailed.
* The defense of replay attack and DoS attack cannot be defended because one solution is Byzantine fault-tolerant agreement protocol but this requires significant changes to the ZooKeeper architecture, which is incompatible with the design goals of SecureKeeper.

## Comments

* It can be assumed that other services that mostly handle data but do not process it can be protected similarly. Simple KVS and blob storage may be a good fit because they access data by a unique identifier.
