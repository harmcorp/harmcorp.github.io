---

title: "Redis 기초"
date:
  created: 2019-08-27
tags:
  - database
  - redis
---

> [Redis 설정](https://bstar36.tistory.com/349) 이 참 많다...

> gcp 에서 docker 로 올린 redis 를 host 에서 설치한 redis-cli 로 공부를 해보자.
> key-value 쌍으로 메모리에 저장되는 빠른 디비... 정도로 이해하고 있는데..

## Common Command

### 모든키 검색
``` shell
127.0.0.1:6379> KEYS *
1) "user:herdin"
2) "hset"
3) "hlist"
4) "redisTest1"
5) "counter"
6) "hello"
```

### 권한획득(requirepass 옵션 사용 시)

``` shell
127.0.0.1:6379> KEYS *
(error) NOAUTH Authentication required.
127.0.0.1:6379> AUTH <YOUR PASSWORD>
OK
```

## Data Type

[Redis Data Type](https://redis.io/topics/data-types)

- Strings
- Lists
- Sets
- Hashes
- Sorted Sets
- Bitmaps and HyperLogLogs

### Strings

[String Function](https://redis.io/commands/#string)

> binary safe 하다는 뜻이 뭘까? 최대 512M
> INCR, DECR, INCRBY
> APPEND
> GETRANGE, SETRANGE.

``` shell
127.0.0.1:6379> SET hello redis01234
OK
127.0.0.1:6379> SET hello
"redis01234"

#INCR INCRBY DECR DECRBY
127.0.0.1:6379> SET counter 1000
OK
127.0.0.1:6379> INCR counter
(integer) 1001
127.0.0.1:6379> INCRBY counter 50
(integer) 1051
127.0.0.1:6379> DECR counter
(integer) 1050
127.0.0.1:6379> DECRBY counter 10
(integer) 1040

#GETRANGE SETRANGE
127.0.0.1:6379> GETRANGE hello 0 2
"red"
127.0.0.1:6379> GETRANGE hello 0 3
"redi"
127.0.0.1:6379> GETRANGE hello 1 2
"ed"
127.0.0.1:6379> SETRANGE hello 2 !!
(integer) 10
127.0.0.1:6379> GET hello
"re!!s01234"

#GETBIT SETBIT
127.0.0.1:6379> GET hello
"re!!s01234"
127.0.0.1:6379> GETBIT hello 0
(integer) 0
127.0.0.1:6379> GETBIT hello 1
(integer) 1
127.0.0.1:6379> GETBIT hello 5
(integer) 0
127.0.0.1:6379> GETBIT hello 4
(integer) 0
127.0.0.1:6379> SETBIT hello 0 1
(integer) 0
127.0.0.1:6379> GET hello
"\xf2e!!s01234"
127.0.0.1:6379> SETBIT hello 0 0
(integer) 1
127.0.0.1:6379> GET hello
"re!!s01234"
```

### Lists

> Lists 의 최대 길이는 2^32 - 1
> LPUSH, LPOP, RPUSH, RPOP
> LRANGE
> LTRIM

[List Function](https://redis.io/commands#list)

``` shell
#LPUSH LPOP RPUSH RPOP
127.0.0.1:6379> LPUSH hlist a
(integer) 1
127.0.0.1:6379> LPUSH hlist b
(integer) 2
127.0.0.1:6379> LPUSH hlist c
(integer) 3
127.0.0.1:6379> RPOP hlist
"a"
127.0.0.1:6379> LPOP hlist
"c"
127.0.0.1:6379> LPOP hlist
"b"
127.0.0.1:6379> LPOP hlist
(nil)

#LRANGE
127.0.0.1:6379> LPUSH hlist a b c d e
(integer) 5
127.0.0.1:6379> LRANGE hlist 0 1
1) "e"
2) "d"
127.0.0.1:6379> LRANGE hlist 1 1
1) "d"
127.0.0.1:6379> LRANGE hlist 0 0
1) "e"
127.0.0.1:6379> LRANGE hlist 0 1
1) "e"
2) "d"
127.0.0.1:6379> LRANGE hlist 0 100
1) "e"
2) "d"
3) "c"
4) "b"
5) "a"
127.0.0.1:6379> LTRIM hlist 0 1
OK
127.0.0.1:6379> LRANGE hlist 0 100
1) "e"
2) "d"
```

### Sets

순서가 없는 Strings 모음. 최대크기 2^32-1
추가, 삭제, O(1) 시간에 조회
아래에 Command reference 를 링크할 거라 커맨드는 생략

[Sets Function](https://redis.io/commands#set)

``` shell
#SADD SREM SPOP SRANDMEMBER SCARD SMEMBERS SISMEMBER
127.0.0.1:6379> SADD hset one #add member "one" to hset
(integer) 1
127.0.0.1:6379> SADD hset two #add member "two" to hset
(integer) 1
127.0.0.1:6379> SADD hset three #add member "three" to hset
(integer) 1
127.0.0.1:6379> SADD hset four #add member "four" to hset
(integer) 1
127.0.0.1:6379> SMEMBERS hset #return all member from hset
1) "one"
2) "four"
3) "three"
4) "two"

127.0.0.1:6379> SPOP hset #remove and return random member from hset
"three"
127.0.0.1:6379> SPOP hset 2 #remove and return random 2 members from hset
1) "two"
2) "one"
127.0.0.1:6379> SMEMBERS hset #return all member from hset
1) "four"

127.0.0.1:6379> SADD hset five #add member "five" to hset
(integer) 1
127.0.0.1:6379> SADD hset six #add member "six" to hset
(integer) 1
127.0.0.1:6379> SRANDMEMBER hset 3 #just return random 3 members from hset
1) "five"
2) "six"
3) "four"
127.0.0.1:6379> SMEMBERS hset #return all member from hset
1) "six"
2) "five"
3) "four"
127.0.0.1:6379> SCARD hset #return member count from hset
(integer) 3

127.0.0.1:6379> SREM hset six #remove specific member from hset
(integer) 1

127.0.0.1:6379> SMEMBERS hset #return all member from hset
1) "five"
2) "four"
127.0.0.1:6379> SISMEMBER hset six #check specific member from hset
(integer) 0
```

### Hashes, Sorted Sets 생략..

참고
- [레디스 이렇게 쓰지 말자](https://www.zdnet.co.kr/view/?no=20131119174125)
