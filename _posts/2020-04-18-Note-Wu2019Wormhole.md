---
layout: post
title:  "Note : Wormhole: A Fast Ordered Index for In-memory Data Management"
date:   2020-04-18 12:00:00
categories: PaperReading
tags: [Index]
---

## Reference

> Xingbo Wu, Fan Ni and Song Jiang. [Wormhole: A Fast Ordered Index for In-memory Data Management](https://dl.acm.org/doi/pdf/10.1145/3302424.3303955?download=true). In Proc. of EuroSys, 2019.

## What

A new ordered index structure, named **Wormhole**, which takes O(log L) worst-case time for looking up a key with a length of L.

<!-- more -->

## Why

Problems with current index for in-memory data management systems (e.g., Key-value store):

* Unordered indexes - Hash Table:
    * Point search with O(1) time
    * Cannot be used when require range queries
* Ordered indexes - B+ Tree & Skip List:
    * Point search with O(log N) time, where N is number of keys in an index
    * Support range queries
    * For an ordered index hosting billions of keys, it may take more than 30 key-comparisons in a lookup

## Main Idea

* Based on the existing hash index and tree index, combining the advantages of the two to create a new index, at the same time to achieve efficient query and support for range query.

## Design Goals

* Significantly reduce asymptotic lookup time than existing ordered indexes.
* Not increasing demand on space and cost of other modification operations, such as insertion.

## How

### Problem with B+ Tree's MetaTree

* B+ tree example with 12 keys
![b+ tree](img/paperReading/Wormhole-BTree.jpg)

* A leaf node's size is bounded in a predefined range $[\lceil \frac{2}{k} \rceil,k]$
* Major search cost in MetaTree is between $log_2N$ (N is the number of index keys)
* As the B+ tree grows, the MetaTree will contain more levels of internal nodes, and the search cost will increase at a rate of O(logN).

### Replacing MetaTree

* Replace the MetaTree with a structure whose search cost is not tied to N.
* *Direct take place the MetaTree with Hash Table?*: Unable to insert key quickly (need rehash); Not support range query

![MetaTrie](img/paperReading/Wormhole-MetaTrie.jpg)

* Using MetaTrie:
    * For each node in the LeafList create a key as its anchor and insert it into MetaTrie. 
    * Node’s anchor key is to serve as a borderline between this node and the node immediately on its left, assuming the sorted LeafList is laid out horizontally in an ascending order.
    * For the anchor key (anchor-key) of a node (Node b), left-key < anchor-key < node-key; anchor key cannot be the prefix of another key.
    * Just like a simple Prefix Tree with container leafs

### Two search phase

In the walk from the root of a MetaTrie to a search-key’s target leafnode:

* The first one: conduct the longest prefix match (LPM) between the search key and the anchors in the trie.
* If the longest prefix is not equal to an anchor, the second phase is to walk on a subtree rooted at a sibling of the token next to the matched prefix of the search key. The O(L) cost of each of the phases can be significantly reduced.

### MetaTrie Hash Table

![MetaTrie Hash Table](img/paperReading/Wormhole-Hash.jpg)

* Each hash item has two fields supporting efficient walk in the second search phase on a path to a leaf node.
* First field: bitmap
    * Meaningful only for internal items. 
    * A bit for every possible child of the corresponding internal node in the trie.
    * The bit is set when the corresponding child exists. With the bitmap, sibling(s) of an unmatched token can be located in O(1) time.
* Second field: two pointers.
    * Each pointing to one of the leaf nodes.
* The hash tag and keys are sorted separately, and then pointed by the pointer.

## Summary

### Strength

* Combining the advantages of prefix tree, B + tree and hash table, the algorithm complexity is lower than the existing index structure

### Weekness

* Complex algorithm expression, reading is not smooth.