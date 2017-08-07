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

