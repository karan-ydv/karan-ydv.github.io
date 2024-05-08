+++
title = "Database Storage Engines"
date = ""
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["databases", "indexes"]
keywords = ["http", "web servers", "middlewares"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
Toc = true
+++


## What are the disadvantages of hash based index for append log segments?
* The simplest kind of db is a log based db

## Append only log seems wasteful, Why you don't update the file in place?
* Write Efficiency - Appending and segment merging are essentially sequential write operations. Sequential writes are much more efficient in spinning disk hard drives, and in ssds too!
* Concurrency and Crash Recovery - are simple when records are append only and immutability. Ex - You don't have to worry about Recovering from crashes due to interrupts while overriting a value.
* Fragmentation - Merging old segments avoids the fragmentation of files over time.

## Disadvantages of hash indexes
* The index may not fit into the system memory. Writing the index to disk is not feasible as there would be a lot of random reads. It is expensive to grow when it becomes full and collisions require fiddly logic.
* Range queries are not efficient. For example, you cannot easily scan over all keys between kitty00000 and kitty99999—you’d have to look up each key individually in the hash maps. 


Better Indexing Structures -> SSTable and LSMTrees

## SSTable - Sorted Segment Table or Sorted String Table
* A SSTable is a disk file format in which the key value pairs in segments are sorted by key

  #### SSTable Advantages over log segments with hash indexes
  * Merging segments are much more simpler. It is like mergesort algorithms. Adjacent segments are merged. Value of Key in more recent segment is kept.
  * Sparsed index - can be kept in the memory. As the index keys are sorted, looks for key can be done by scanning a handful of keys in the block of few kilboytes of size.
  * Compression - Since scanning of key involes scanning keys in a block, the block can be compressed before writing to disk, now keys can point the start of the compression block. It also reduces IO bandwidth.
    
  #### How are keys written in sorted order?
  * input can be in any order, new keys then cannot be appeded directly. While Maintaining a sorted structure on disk is possible (using B-Trees), keeping an in memory sorted strucure is much easier using RB Trees or AVL Trees.
  * Storage engine works as follows
    * key is inserted into an in memory data structure #Memtable
    * the keys are written to disk when the size of data structure exceeds threshold (typically few megabytes)
    * Read Request - are served by first looking in the memtable then in the most recently written segment and so on
    * from time to time, run a compaction process in the background to combine segment files
  * There is only one problem, what if the power goes down. To prevent memtable data loss, all inserts are logged into a append only log. It can be used to reconstruct the the memtable when server is restarted.

# Algorithms used by [LevelDB](https://github.com/google/leveldb) and [RocksDB](https://github.com/facebook/rocksdb)

# LSMTree Index
# BTreeIndex


# Key takeaways
* What is an index in database?
  * It's a data structure (often times key value) that lives in the memory and is used to search for values on the disk with minmum disk scanning. Ex - Hash Index, BTree, GIN (Generalized Inverted Index), GiST (Generalized Search Tree).
  * There are two types of indexes
    * Primary   - The key denotes unique primary key
    * Secondary - Secondary index keys can be duplicates, so either the values are ids of primary keys or keys are paired with pk to make unique keys

