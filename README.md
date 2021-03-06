# Redis Common Commands

keys *	
-- check all the keys in db, but do not use in production, too many keys	
	
exists key	
-- return 1 if key exists, or return 0	
	
del key	
-- return 1 if deleting key exists, or return 0	
	
del key1 key2 ... keyn	
-- return the count that is successfully deleted, i.e.  "del key1 key2 key3", only key1 & key2 exist, then return 2	
	
expire key seconds	
-- set the expiration time of key. return 1 if the key exist, or return 0	
	
ttl key	
-- check the time-to-live of key,	
-- returning an integer which is >=0, this integer mean the remaining time of key,	
-- returning -1 means no expiration time is set for this key.	
-- returning -2 means the key does not exist(once expired, redis will delete the key)	
	
type key	
-- check the type of the key, there are five types: string, hash, list, set, zset. return none if key not exist	
	
object encoding key	
-- check the internal data structure implementation of a key.	
-- one type can have different data structure implementation. i.e. the type "string" has possible 3 encoding "embstr", "int" & "raw"	
	
set key value [ex seconds] [px milliseconds] [nx|xx]	
-- set value to key	
-- ex is expiration time in seconds	
-- px is expiration time in milliseconds	
-- nx is not exist, means it can be only set successfully when the key not exist. xx is exist, on the contrary to nx.	
	
setex key seconds value	
-- like "set key value ex seconds"	
	
setnx key value	
-- like "set key value nx"	
	
mset key1 value1 [key2 valule2 ... keyn valuen]	
-- set key value by batch	
	
mget key1 key2 ... keyn	
-- get key value by batch	
	
incr key	
-- increase key by 1, key must be integer type, or it will return "(error) ERR value is not an integer or out of range"	
	
decr key	
-- on the contrary to "incr key"	
	
incrby key increment	
-- like "incr key", but you can define the delta	
	
decrby key increment	
-- on the contrary to "incrby key increment"	
	
incrbyfloat key increment	
-- like "incrby key increment", but the key can be integer or float	
	
append key value	
-- append the value to the end of original value	
	
strlen key	
-- show the key's length, but key has to be a string	
	
getset key newValue	
-- return the old value, then set the new value to it	
	
setrange key offset value	
-- i.e.  "set str aaaest" "setrange str 0 bbb", it will return bbbest	
	
getrange key start end	
-- get part of the string	
	
hset key field value	
-- hash set, i.e. "hset user:1 name tom"	
	
hget key field	
-- hash get, i.e. "hget user:1 name", return "tom"	
	
hdel key field1[field2 ...]	
-- hash delete	
	
hmset key field1 value1 [field2 valule2 ...]	
-- hash set by batch	
	
hmget key field1[field2 ...]	
-- has get by batch	
	
hkeys key	
-- return all the fields of the key, the key must be a hash type. (here I think, rename "hkeys" to "hfields" will be more proper, because it return the fields of a hash)	
	
hvals key	
-- return all the values of the key	
	
hgetall key	
-- return all the key-value pairs of the key	
	
hincrby key field increment	
-- like incr, but this is for hash, not for integer	
	
hincrbyfloat key field increment	
-- like hincrby	
	
hstrlen key field	
-- like strlen, but it seems not work in Windows Redis, only work in Linux Redis	
	
rpush key value1 value2 ....	
-- i.e. "rpush list a b c"  the list will be "a b c"	
	
lpush key value1 value2 ...	
-- i.e. "lpush list a b c"  the list will be "c b a"	
	
lrange key start end	
-- get the range of a key, key must be a list	
-- i.e. if list is  "a b c d e", "lrange list 1 2" will return "b c". and "lrange list 0 -1"  will return all.	
	
lindex key index	
-- i.e. if list is "a b c d e", "lindex list 2" will return "c"	
	
llen key	
-- return the list length	
	
lpop key	
-- pop up an item from left side	
	
rpop key	
-- pop up an item from right side	
	
lrem key count value	
-- remove the value from key, and the count is a positive or negetive value will determine the removal direction	
-- i.e. if the list is "a a b b java java a a b b java java"  (rpush list a a b b java java a a b b java java)	
-- "lrem list 3 jave" will return "a a b b a a b b java"	
-- "lrem list -3 java" will return "a a b b java a a b b"	
	
lset key index newValue	
-- set a new value to the index	
-- i.e. if the list is "a b c d e", "lset list 2 java" will return "a b java d e"	
	
blpop key1 [key2 ...]  timeout	
-- similar to lpop, but blpop will block for seconds defined by timeout.	
-- i.e. "blpop list 10", if the list is not empty then it will return the left first item of list at once. if the list is empty, this operation will wait for 10 seconds at most, then give up.	
-- ant please noticed, it there are more than on client is running "blpop list 10", it will in a state of competition, if only one item in the list, only one client will win.	
	
brpop key1 [key2 ...] timeout	
-- refer to blpop	
	
sadd key element1 [element2 ...]  
-- i.e. "sadd myset a b c d e" , add element "a", "b", "c", "d", "e" in the set "myset"
-- return the elements count that are successfully added

srem key element1 [element2]
-- remeove the elements in a set, return the elements count that are successfully removed

scard key
-- show how many elements are in the set

sismember key element
-- judge if the element is in the set, if yes return 1 else return 0

srandommember key [count]
-- return one or more elements in a set randomly

smembers key
-- return all the elements in a set

spop key
--pop up one random element in a set

sinter key1 key2
--calculate the intersection of key1 and key2

sinterstore newKey key1 key2
--calculate the intersection of key1 and key2 and store the result int newKey

sunion key1 key2
--refer to sinter, but it is a union set

sunionstore newKey key1 key2
--refer to sinterstore, but it is a union set


sdiff key1 key2
--refer to sinter, but it is a difference set

sdiff newKey key1 key2
--refer to sinterstore, but it is a difference set

zadd key score1 member1 [score2 member2 ...] [nx|xx]
--add element(s) in a zset.

zrem key member
--remove the member from a zset

zcard key
--show how many elements are in a zset

zscore key member
--return the sore of the member 

zrank|zrevrank key member 
--return the rank or the reverse rank of the member, order by score(asc|desc)

zincrby key increment member
-- increase the score of a member by delta

zrange | zrevrange key start end [withscores]
-- return a range of zset,  particularly "zrange key 0 -1" will return all
-- noticed , the start and end is the ranking of the element

zrangebyscore | zrevrangebyscore key [(]min max[)] [withscores] 
-- return a range of zset, the min and max are score, "(" and ")" means exclude the min and  max themselves
-- also we can use -inf as min, or +inf as max to say we want the infinity

zcount key min max
-- count the member where their score are between min and max

zremrangebyrank key start end
--remove a range of elements in zset, start and end mean the ranking

zremrangebyscore key min max
--refer to zremrangebyrank, but min and max mean the score

zinterstore destination key1 [key2 ...]  [weights weight1, weeight2 ... weightN] [aggregate sum |min |max]
-- calculate the intersection of key1 [key2...], default the aggregate will be sum

zuniostore destination key1 [key2 ...]  [weights weight1, weeight2 ... weightN] [aggregate sum |min |max]
-- refer to zinterstore, but it is a union set













