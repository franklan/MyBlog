---
layout: default
title: Database Sharding based on Amoeba
---

### Requirement: ###

To enlarge the capacity and improve the speed performance  of our databases, we refer to database sharding to meet our requirement.

### Horizontal Sharding: ###

[Horizontal partitioning](http://en.wikipedia.org/wiki/Shard_(database_architecture)) is a database design principle whereby rows of a database table are held separately, rather than being split into columns (which is what normalization and vertical partitioning do, to differing extents). Each partition forms part of a shard, which may in turn be located on a separate database server or physical location.
There are numerous advantages to this partitioning approach. Since the tables are divided and distributed into multiple servers, the total number of rows in each table in each database is reduced. This reduces index size, which generally improves search performance. A database shard can be placed on separate hardware, and multiple shards can be placed on multiple machines. This enables a distribution of the database over a large number of machines, which means that the database performance can be spread out over multiple machines, greatly improving performance.

### Solution: ###
Amoeba is a proxy that sits between your client and DB server(s) that can monitor, analyze or transform their communication. written in java. 1.load balancing 2、failover; 3、queries Route.projects:amoeba for mysql.


<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/U6CDq7q8NJuQsZgcoQjVIdMTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh6.googleusercontent.com/-M-LZVe_3ffI/U1pyd7Tg33I/AAAAAAAAAQ4/UudtNzvT-aQ/s400/topology.jpg" height="212" width="400" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

### Demo ###

Proxy server shows:

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/XnISBn1WrF4tB9bW2sL-2dMTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh4.googleusercontent.com/-r2Zd9F6qB1U/U1pydDx5FEI/AAAAAAAAAQw/l5xIEWEum0U/s800/201.jpg" height="499" width="772" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

DB1 shows:

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/kXBddzNJPKbcxwc5-tJMFdMTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh5.googleusercontent.com/-G6t-BaT9Ues/U1pydFg2sUI/AAAAAAAAAQ0/eU_kBBY6KFU/s800/105.jpg" height="437" width="776" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

DB2 shows:

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/KCyh-BVRidXBuvrQHEwH59MTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh3.googleusercontent.com/-1ik_b2BeNZY/U1pydHloSiI/AAAAAAAAAQo/L59t6flRNTg/s800/106.jpg" height="424" width="780" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

