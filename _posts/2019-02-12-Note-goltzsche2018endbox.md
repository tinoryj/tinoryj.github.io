---
layout: post
title:  "Note : ENDBOX: Scalable Middlebox Functions Using Client-Side Trusted Execution"
date:   2019-02-12 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Goltzsche, David and Rusch, Signe and Nieke, Manuel and Vaucher, S{\'e}bastien and Weichbrodt, Nico and Schiavoni, Valerio and Aublin, Pierre-Louis and Cosa, Paolo and Fetzer, Christof and Felber, Pascal and others. [Endbox: scalable middlebox functions using client-side trusted execution](https://ieeexplore.ieee.org/abstract/document/8416500/) . In Proc. of DSN, 2018.

## What

ENDBOX enable secure networking by client-Side trusted execution.
It is a scalable middlebox that enable secure networking by client-Side trusted execution.
<!-- more -->
## Why

* Network attacks -> Operators use middleboxes to improve network performance and security -> High costs.
* Problems of Current Middleboxes:
    * Centralized hardware -> expensive, vulnerable, limited scalability.
    * Offloading to cloud services -> higher complexity and latency, requires trust in cloud provider, processing of encrypted traffic problematic.
* Client-Side Middleboxes Functionality has problems -> Leverage trusted execution
    * Both users and client machines cannot be trusted.
    * Users are forced to use middlebox function.

## How

* Shifting Middleboxes to clients.
* Middlebox functions run inside enclave.
* Route packets through SGX enclaves using VPN tunnel.

## Some Detail

* Middleboxes: a computer networking device that transforms, inspects, filters, or otherwise manipulates traffic for purposes other than packet forwarding.
* Using OpenVPN v2.4.0 & Click Modular Router for compare in multiple use cases:
    * Forwarding (FOR)
    * Firewall (FW)
    * Intrusion Prevention (IDPS) 
    * Load balancer (LB)
    * DDoS protection (DDoS)
* Evaluation: 
    * Throughput: Different packet size compare.
    * CPU usage & Throughput: Different clients number.  
