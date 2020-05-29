---
layout: post
title:  "Note : Autarky: Closing controlled channels with self-paging enclaves"
date:   2020-05-23 12:00:00
categories: PaperReading
tags: [SGX]
---

## Reference

> Orenbach Meni, Baumann, Andrew, Silberstein Mark. [Autarky: Closing controlled channels with self-paging enclaves](https://dl.acm.org/doi/pdf/10.1145/3342195.3387541). In Proc. of EuroSys, 2020.

## What

Autarky is a set of minor, backward-compatible modifications to the SGX ISA that hide an enclave’s page access trace from the host, and give the enclave full control over its page faults.
<!-- more -->

**Controlled-channel attacks**: based on page table attacks. This type uses the page table's access control to the enclave page, setting the enclave page to be inaccessible. At this time, any access will trigger a page fault exception, which can distinguish which pages the enclave visited. Combining these pieces of information in chronological order, it is possible to deduce certain states of the enclave and protected data.

## Paper Summary

Enclave's environment has a lot of resources and is shared with non-enclave outside, such as *cache*, *page table*, *DRAM*, and *branch predictor*, etc., which provides attackers with a rich side-channel attack surface. Moreover, the untrusted operating system is still responsible for managing system resources, such as page tables, memory, interrupts, process scheduling, etc., which further facilitates the attacker to reduce the noise during the side-channel attack, thereby improving the side-channel attack Success rate.

This work is mainly aimed at reducing the probability of a Page fault side-channel attack. In particular, specifically to solve the Controlled-channel attack, which attacker controls the channel that has no noise and very precise. The main idea of this work is to force the system to call enclave for each page fault conditions and make enclave controls all page table allocation with OS.

The two main design principles have forced the OS to call the enclave on every page fault which gives enclave rights to control all page faults and secure demand-paging which makes enclave-OS cooperative the paging, hide fault information from the OS and let enclave enforce its own paging policy. It modifies intel SGX library OS and SDK to add these features. The original attack required millions of page faults, this work build a rate-limiting policy for enclave paging policy, build an ORAM policy only for paging, and due to some applications do not need oblivious paging across all pages clusters (cooperative paging for all pages in the cluster).

Finally, this paper evaluates the performance changes brought by the proposed SGX architecture changes. And compares the effect of page clustering and ORAM. In addition, the effect analysis of real-world programs such as Image processing with libjpe, Spell checking server (Hunspell), FreeType library (font rendering library) are conducted.

## Strength

* The paper is well written.
* Simple and effective idea, good practice effect.
* Test analysis of a variety of real applications, indicating strong practicality.

## Weakness

* Only basic security discussions were conducted, lack of some actual attack analysis, and there was no practical test for the effect of improving security.
* Existing side-channel attacks against SGXoften mix multiple sources of vulnerabilities (*cache*, *page table*, *DRAM* and *branch predictor*, etc). It may be difficult to prevent existing attacks by a single solution to vulnerabilities caused by page faults.

## Comments

* Cache miss-based attacks are similar to page fault-based attacks. Can this method be modified simply to jointly prevent these two attacks?
* Note that existing attack methods can complete the attack [without the aid of AEX](https://www.inforsec.org/wp/?p=2298). Should this situation be added to the security analysis of this article?
