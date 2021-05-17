---
layout: blog
comments: true
istop: false
code: true
title: "Redis Ordinary Commands Cookbook"
background-image: https://cdn.worldvectorlogo.com/logos/redis.svg
date:  2021-05-17 23:55:00
category: code
tags:
- redis
---

# Redis Ordinary Commands Cookbook

## 1. Redis Key (struct hash.)

```
DEL [key] // delete key when exist
```

```
DUMP [key] // serialize the specified key, and return the serialized value
```

```
EXISTS [key] // check whether key exist
```

```
EXPIRE [key] [seconds] // set expire time for key, with second as unit
```

```
KEYS [pattern] // find all keys match the specified pattern
```

```
SCAN cursor [MATCH pattern] [COUNT count]
e.x. SCAN 0 MATCH * COUNT 1000
```

```
TTL [key] // query the TTL(time to live) of the specified key, with second as unit
```

```
TYPE [key] // query type of the specified key
```



## 2. Redis String (struct SDS)

```
GETRANGE [key] [start] [end] // query segemented value of the specified key
```

```
SET [key] [value] // add key value pair
```

```
GET [key] // query value of the specified key
```

```
SETNX [key] [value] // set key if not exist
```

```
GETSETNX [key] [value] // set key as value, return the old value
```

```
SETEX [key] [seconds] [value] // set key as value, together with expire time
```

```
STRLEN [key] // query value length of the specified key
```

```
MGET [key1] ([key2...]) // batch query values of the specified keys
```

```
MSET [key value] ([key value ...]) // batch set
```

```
INCR/DECR [key] // increment/decrement the stored value by 1 
```

```
INCRBY/DECRBY [key] [increment] // increment the stored value by increment
```



## 3. Redis Hash (struct hash.)

```
HDEL [key] [field1] ([field2 ...]) // delete 1 or multiple hash table fields
```

```
HEXISTS [key] [field] // check whether speifified [field] exists in the [key] hash table
```

```
HGET [key] [field] // query speifified [field] in the [key] hash table
```

```
HGETALL [key] // query all fields and values in the [key] hash table
```

```
HKEYS [key] // query all fields in the [key] hash table
```

```
HLEN [key] // query number of field-value pairs in the [key] hash table
```

```
HVALS key // query all values in the [key] hash table
```

```
HSET [key] [field] [value] // set [key] hash table [field] as [value]
```

```
HSETNX [key] [field] [value] // set [key] hash table [field] as [value] if not exist
```

```
HMSET [key] [field1 value1] ([field2 value2 ...]) // batch set [key] hash table [field] as [value] if not exist
```

```
HSCAN [key] [cursor] [MATCH pattern] [COUNT count]
e.x. HSCAN map 0 MATCH * COUNT 1000
```



## 4. Redis List

```
BLPOP/BRPOP [key1] ([key2 ...]) [timeout] 
// pop the first/last element in the list, block until timeout if no element in list
```

```
BRPOPLPUSH [source] [destination] [timeout]
// pop the last element in the [source] list, insert into the head of another list and return the element, block until timeout if no element in list
```

```
LINDEX [key] [index] // get element in list by index(from 0)
```

```
LLEN key // query the [key] list length
```

```
LPOP/RPOP [key] // pop the first/last element in the list, block until timeout if no element in list
```

```
LPUSH [key] [value1] ([value2 ...]) // push one or multiple values to the head of the list
```

````
LPUSHX [key] [value] // push one value to the head of the existing [key] list
````

```
LRANGE [key] [start] [stop] // get the elements in the [key] list within scope (both start and stop include)
```

```
LREM [key] [count] [value] // remove elements of [value], maximum remove a total of [count] elements
```

```
LSET [key] [index] [value] // set the element in list of [index] to [value], the [index] must exist
```



## 5. Redis Set

```
SADD [key] [member1] ([member2 ...]) // add member(s) into [key] set
```

```
SCARD [key] // get the number of members in [key] set
```

```
SDIFF/SINTER/SUNION [key1] [key2] // get the difference/intersection/union between set [key1] and [key2]
```

```
SDIFFSTORE/SINTERSTORE/SUNIONSTORE [destination] [key1] [key2] 
// get the difference/intersection/union between set [key1] and [key2], and store the result in destination
```

```
SISMEMBER [key] [member] // check wether [member] exist in [key] set
```

```
SMEMBERS [key] // list all the members in [key] set
```

```
SMOVE [source] [destination] [member] // remove [member] from source, add [member] to destination, no operation happens when source does not contains [member]
```

```
SREM [key] [member1] ([member2 ...]) // remove one or multiple members from [key] set
```

```
SSCAN [key] [cursor] [MATCH patten] [COUNT count]
e.x. SSCAN set_1 0 MATCH * COUNT 1000
```



## 6. Redis Sorted Set

```
ZADD [key] [score1 member1] ([score2 member2 ...]) // add one or multiple members to a sorted set, or update the score of existing members
```

```
ZCARD [key] // get the number of members in [key] sorted set 
```

```
ZCOUNT [key] [min] [max]* // count the number of members within the score range
```

```
ZRANGE [key] [start] [stop] ([WITHSCORES]) // get the members within the index range
```

```
ZRANGEBYSCORE [key] [min] [max] ([WITHSCORES]) ([LIMIT offset count])
// get members in [key] sorted set with score within range, 
```

```
ZRANK [key] [member] // get the index of [member] in [key] sorted set
```

```
ZREM [key] [member] ([member ...]) // remove one or multiple members from [key] sorted set
```

```
ZSCORE [key] [member] // get the score of [member]
```

```
ZSCAN [key] [cursor] [MATCH patten] [COUNT count] 
e.x. ZSCAN sset 0 MATCH * COUNT 1000
```



\* [min] [max] grammar: 

- [ indicates inclusion
- ( indicates exclusion
- \- can represent [MIN (the minimum value) 
- \+ can represent [MAX (the maximum value) 

### Reference

[1]. https://www.runoob.com/redis