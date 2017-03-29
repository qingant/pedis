# Pedis (Parallel Redis)

## What's Pedis?

Pedis is the NoSQL data store using the SEASTAR framework, compatible with REDIS. The name of Pedis is an acronym of **P**arallel r**edis**, which with high throughput and low latency.

Redis is very popular *data structures* server. For more infomation, see here: http://redis.io/
Seastar is an advanced, open-source C++ framework for high-performance server applications on modern hardware.
For more infomation, see here: http://www.seastar-project.org/


Now, the redis commands were supported by Pedis as follow:
  * **KEY**: DEL, EXISTS, TTL, PTTL, EXPIRE, PEXPIRE
  * **STRING**: GET, SET, DECR, INCR, DECRBY, INCRBY, APPEND, STRLEN, MGET, MSET
  * **LIST**: LINDEX, LINSERT, LLEN, LPUSH, LPUSHX, LPOP, LRANGE, LREM, LTRIM, LSET, RPOP, RPUSH, RPUSHX
  * **HASH**: HSET, HDEL, HGET, HLEN, HSTRLEN, HMSET, HMGET, HKEYS, HVALS, HEXISTS, HINCRBY
  * **SET**: SADD, SMEMBERS, SISMEMBER, SREM, SDIFF, SDIFFSTORE, SINTER, SINTERSTORE, SUNION, SUNIONSTORE, SMOVE, SPOP
  * **SORTED SET**: ZADD, ZCARD, ZCOUNT, ZINCRBY, ZRANGE, ZRANK, ZREM, ZREMRANGEBYSCORE, ZREMRANGEBYRANK, ZREVRANGE, ZREVRANGEBYSCORE, ZREVRANK, ZSCORE, ZUNIONSTORE, ZINTERSTORE
  * **GEO**: GEOADD, GEOPOS, GEOHASH, GEODIST, GEORADIUS, GEORADIUSMEMBER
  * **OTHER**: ECHO, PING, SELECT

## Building Pedis on Ubuntu 14.04

In addition to required packages by Seastar, the following packages are required by Pedis.

Installing required packages:
```
sudo apt-get install libaio-dev ninja-build ragel libhwloc-dev libnuma-dev libpciaccess-dev libcrypto++-dev libboost-all-dev libxen-dev libxml2-dev xfslibs-dev
```

Installing GCC 4.9 for gnu++1y. Unlike the Fedora case above, this will
not harm the existing installation of GCC 4.8, and will install an
additional set of compilers, and additional commands named gcc-4.9,
g++-4.9, etc., that need to be used explicitly, while the "gcc", "g++",
etc., commands continue to point to the 4.8 versions.

```
# Install add-apt-repository
sudo apt-get install software-properties-common python-software-properties
# Use it to add Ubuntu's testing compiler repository
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
# Install gcc 4.9 and relatives
sudo apt-get install g++-4.9
# Also set up necessary header file links and stuff (?)
sudo apt-get install gcc-4.9-multilib g++-4.9-multilib
```

To compile Seastar explicitly using gcc 4.9, use:
```
git clone https://github.com/fastio/pedis.git
cd pedis
git submodule update --init --recursive
./configure.py --compiler=g++-4.9
ninja 
```


## Run Pedis 

```
./build/release/pedis --smp 1

```

## Benchmark

The following describe the details of the Pedis benchmark making it reproducible.
The result data was generated by memtier_benchmark(https://github.com/RedisLabs/memtier_benchmark).

Latest Results(Sep. 2016)

![](https://github.com/fastio/pedis/blob/master/docs/benchmark.png)

Pedis's latency was less then 0.5ms for 99% of all requests.

Test bed:

* Server 1: Pedis/Redis Server
* Server 2: memtier_benchmark tool

Software:

* OS RedHat EL7
* Pedis (latest)
* Redis (2.8)

Hardware:

* Memory: 128GB
* SSD: 500GB
* CPU: 24 logical cores 
* NIC: 1000Mb 
