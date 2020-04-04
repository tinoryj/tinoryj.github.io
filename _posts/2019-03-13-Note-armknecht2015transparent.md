---
layout: post
title:  "Note : Transparent data deduplication in the cloud"
date:   2019-03-13 12:00:00
categories: PaperReading
tags: [Deduplication, FileSystem]
---

## Reference

> Armknecht, Frederik and Bohli, Jens-Matthias and Karame, Ghassan O and Youssef, Franck.[Transparent data deduplication in the cloud](http://www.ghassankarame.com/dedup.pdf).In Proc. of CCS, 2015.

## What

Design and implement ClearBox which allows a storage service provider to transparently attest to its customers the deduplication patterns of the (encrypted) data that it is storing.
<!-- more -->

## Why

Storage saving has not directly benefit to users as there is no transparent relation between effective storage costs and the prices offered to the users.

## How

* System security: Put/Get/Attest/Delete/Verify Protocol.
* Cryptographic accumulators （one-way membership functions）answer a query whether a given candidate belongs to a set.
* Time-Dependent Randomness.
* Server-Aided Key Generation: Blind BLS signature.
* Proofs of Ownership: Halevi's work.

## Some Details

### Blind BLS Signature

* Using PBC Library 
* Using random g1 and r for generate blind number k, $k=g1^r$.
* Blind by h * k, where h is the origin hash of a chunk.
* Unblind by sig / k, where sig is the blind BLS signature of blind hash (h * k). 
