---
layout: post
title: Hola Redis - ch 1
date: 2021-05-06
categories: books, redis
---

I decided to explore a little bit more about redis, so I started to take a look at [Redis in Action](https://redislabs.com/ebook/redis-in-action/), the book is available online so that is very convenient.

As I learn more by writing, here are some of the notes about chapter 1. There's also a link about my implementation of the example provided in part 1.3 of the book.

## What is Redis?

In memory remote non relational db.
Stores keys for 5 data structures types values:
  - Strings
  - lists
  - Sets
  - hashes
  - Sorted sets

Redis supports writing of data to disk in two ways:
  - Point-in-dumb data 
    This can happen explicitly, calling a dump-to-disk command or when a series of conditions are met (number of writes)

  - Use of append-only-file of commands

For performance redis supports:
  - Master/Slave replication
    Slaves connect to master and receive a copy of the data. Then as writes occur, slaves are updated.
    Clients can connect to slaves to improve reading time.

Some benefits include:
  - Avoid writing unnecessary data to a table

## Redis Data structures

Redis allow us to store keys for this different values:

### STRING
  Basic operations: GET, SET, DEL

    Ex:
      - SET hello angela
      - GET hello
      - DEL hello

### LIST
  Lists are ordered sequence of strings. Can contain repeated strings
  Lists start at index 0. Last index can be fetched with -1

  Basic operations:

    - LPUSH/RPUSH --> Push items to the front and to the back
    - LPOP/RPOP --> Pop items to the front and to the back
    - LINDEX --> Fetches an item in a given position
    - LRANGE --> Fetches a range of items (0 -1 for all the list)

### SET
  Unordered set of unique strings. Redis uses a hash table to make sure all strings are unique

  Basic operations:

    - SREM --> Add/Remove an item from the set
    - SISMEMBER --> Find if an item is in the set
    - SMEMBERS ---> Return the whole set


### HASH
  Store a mapping of keys to values. The values can be strings or numbers

  Basic operations:

    - HSET --> Stores the value in the given keys
    - HGET --> Fetches the value of the given keys
    - HGETALL --> Fetches the entire hash
    - HDEL --> Removes a key from the hash if exists

### ZSET (Sorted sets)
  Stores a mapping of members/scores. The concept is similar to the key/value in the HASH structure.
  The members are unique strings (as keys), but the scores are floating point numbers.

  Items of a ZSET can be accessed by member or scores. When we access by member, we use the member name
  (as we use keys in the hash). But for scores, we can access them by order.

  Basic operations:

    - ZADD --> Adds a member with the given score to the ZSET
    - ZRANGE --> Fetches the associated items ordered by score
    - ZRANGEBYSCORE --> Fetches the associated items whose score is in the range. Ordered by score
    - ZREM --> Removes the item from the ZSET

Commands Reference: https://redis.io/commands

Notes on experiments:
  1. Commands are case insensitive
    set == SET

  2. Keys are case sensitive:
      GET hello != GET HELLO

## Hello Redis

I experimented a bit with the proposed exercise of this section [here](https://github.com/anvicordova/redis-vote-example)