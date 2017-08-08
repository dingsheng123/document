### redis学习

#### 1. 下载安装

下载地址：https://github.com/MicrosoftArchive/redis/releases

解压到目录  d:\Java\redis  下

执行cmd窗口进入该目录下，输入命令：

> redis-server.exe redis.windows.conf

然后启动一个新的cmd窗口，原来的不能关闭，进入redis目录下，输入以下命令：

> redis-cli.exe -h 127.0.0.1 -p 6379



**PS** : 

连接本地redis命令

> redis-cli

连接远程redis命令(host为服务器地址，port为端口号，password为密码)

> redis-cli -h host -p port -a password



**PPS:** redis 安装成windows服务

> redis-server  --service-install redis.windows.conf
>
> --启动命令
>
> redis-server --service-start
>
> --停止命令
>
> redis-server -service-stop

#### 2. redis设置密码

修改redis安装目录下的 redis.windows.conf文件，修改 requirepass  参数后的密码，如将密码改为root

> requirepass root

然后重启redis，执行redis命令

>redis-cli -h 127.0.0.1 -p 6379
>
>-- 输入其他命令会有报错信息  (error) ERR operation not permitted
>
>--输入授权命令即可：
>
>auth root
>
> 
>
>-- 在redis中查看密码
>
>config get requirepass
>
>-- 在redis中设置密码（临时的，重启后密码失效）
>
>config set requirepass password



#### 3. redis的数据类型

redis支持5中数据类型：string（字符串），hash（哈希），list（列表），set（集合），zset（sorted set,有序集合）

1. String

   string是redis最基本的数据类型，二进制安全，一个键最大能存储512M

   > set name "abc"
   >
   > get name


2. Hash

   Redis 是一个键值对集合，是一个String类型的field和value的映射表，hash特别适合存储对象

   每个hash可以存储2^(32-1) 键值对（40多亿）

   > hmset user:1 username abc password 123 point 100
   >
   > ​
   >
   > hgetall user:1


3. list 

   Redis list 是简单的字符串列表，按照插入的顺序排序

   list最多可存储 2^(32-1) 元素(4294967295, 每个列表可存储40多亿)

   > lpush name aaa
   >
   > lpush name bbb
   >
   > lpush name ccc
   >
   >  
   >
   > lrange name 0 10

4. set

   Redis set是string类型的无序集合。集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)

   集合内元素具有唯一性，第二次插入的重复数据将被忽略

   集合中最大成员数为  2^(32-1)  (4294967295, 每个列表可存储40多亿)

   > sadd username aaa
   >
   > sadd username bbb
   >
   > sadd username ccc
   >
   > sadd username bbb
   >
   >  
   >
   > smembers username

5. zset

   Redis zset和set一样，也是string类型元素的集合，且不允许重复的成员

   不同的是，每个元素都会关联一个double类型的分数，redis根据分数来为集合中的成员进行从小到大排序，分数可以重复

   > zadd name 1 aaa
   >
   > zadd name 0 bbb
   >
   > zadd name 4 ccc
   >
   > zadd name 3 ddd
   >
   >  
   >
   > zrangebyscore name 0 1000


#### Redis 常用命令集

1. 连接

   - quit：关闭连接（connection）
   - auth：简单密码认证
   - help cmd： 查看cmd帮助，例如：help quit

2. 持久化

   - save：将数据同步保存到磁盘
   - bgsave：将数据异步保存到磁盘
   - lastsave：返回上次成功将数据保存到磁盘的Unix时戳
   - shundown：将数据同步保存到磁盘，然后关闭服务

3. 远程服务控制

   - nfo：提供服务器的信息和统计
   - monitor：实时转储收到的请求
   - slaveof：改变复制策略设置
   - config：在运行时配置Redis服务器

4. 对value操作

   - exists(key)：确认一个key是否存在
   - del(key)：删除一个key
   - type(key)：返回值的类型
   - keys(pattern)：返回满足给定pattern的所有key
   - randomkey：随机返回key空间的一个
   - keyrename(oldname, newname)：重命名key
   - dbsize：返回当前数据库中key的数目
   - expire：设定一个key的活动时间（s）
   - ttl：获得一个key的活动时间
   - select(index)：按索引查询
   - move(key, dbindex)：移动当前数据库中的key到dbindex数据库
   - flushdb：删除当前选择数据库中的所有key
   - flushall：删除所有数据库中的所有key

5. String

   - set(key, value)：给数据库中名称为key的string赋予值value
   - get(key)：返回数据库中名称为key的string的value
   - getset(key, value)：给名称为key的string赋予上一次的value
   - mget(key1, key2,…, key N)：返回库中多个string的value
   - setnx(key, value)：添加string，名称为key，值为value
   - setex(key, time, value)：向库中添加string，设定过期时间time
   - mset(key N, value N)：批量设置多个string的值
   - msetnx(key N, value N)：如果所有名称为key i的string都不存在
   - incr(key)：名称为key的string增1操作
   - incrby(key, integer)：名称为key的string增加integer
   - decr(key)：名称为key的string减1操作
   - decrby(key, integer)：名称为key的string减少integer
   - append(key, value)：名称为key的string的值附加value
   - substr(key, start, end)：返回名称为key的string的value的子串

6. List

   - rpush(key, value)：在名称为key的list尾添加一个值为value的元素
   - lpush(key, value)：在名称为key的list头添加一个值为value的 元素
   - llen(key)：返回名称为key的list的长度
   - lrange(key, start, end)：返回名称为key的list中start至end之间的元素
   - ltrim(key, start, end)：截取名称为key的list
   - lindex(key, index)：返回名称为key的list中index位置的元素
   - lset(key, index, value)：给名称为key的list中index位置的元素赋值
   - lrem(key, count, value)：删除count个key的list中值为value的元素
   - lpop(key)：返回并删除名称为key的list中的首元素
   - rpop(key)：返回并删除名称为key的list中的尾元素
   - blpop(key1, key2,… key N, timeout)：lpop命令的block版本。
   - brpop(key1, key2,… key N, timeout)：rpop的block版本。
   - rpoplpush(srckey, dstkey)：返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部

7. Set

   - sadd(key, member)：向名称为key的set中添加元素member
   - srem(key, member) ：删除名称为key的set中的元素member
   - spop(key) ：随机返回并删除名称为key的set中一个元素
   - smove(srckey, dstkey, member) ：移到集合元素
   - scard(key) ：返回名称为key的set的基数
   - sismember(key, member) ：member是否是名称为key的set的元素
   - sinter(key1, key2,…key N) ：求交集
   - sinterstore(dstkey, (keys)) ：求交集并将交集保存到dstkey的集合
   - sunion(key1, (keys)) ：求并集
   - sunionstore(dstkey, (keys)) ：求并集并将并集保存到dstkey的集合
   - sdiff(key1, (keys)) ：求差集
   - sdiffstore(dstkey, (keys)) ：求差集并将差集保存到dstkey的集合
   - smembers(key) ：返回名称为key的set的所有元素
   - srandmember(key) ：随机返回名称为key的set的一个元素

8. Hash

   hset(key, field, value)：向名称为key的hash中添加元素field

   hget(key, field)：返回名称为key的hash中field对应的value

   hmget(key, (fields))：返回名称为key的hash中field i对应的value

   hmset(key, (fields))：向名称为key的hash中添加元素field 

   hincrby(key, field, integer)：将名称为key的hash中field的value增加integer

   hexists(key, field)：名称为key的hash中是否存在键为field的域

   hdel(key, field)：删除名称为key的hash中键为field的域

   hlen(key)：返回名称为key的hash中元素个数

   hkeys(key)：返回名称为key的hash中所有键

   hvals(key)：返回名称为key的hash中所有键对应的value

   hgetall(key)：返回名称为key的hash中所有的键（field）及其对应的value