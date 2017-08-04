### MongoDB 学习



#### 1. 安装

1. 从官网下载后安装到本地路径，如：d:\java\MongoDB
2. 在该目录下创建文件夹d:\java\MongoDB\data\db和d:\java\MongoDB\logs\mongo.log 分别存放数据库数据和日志文件
3. 进入d:\java\MongoDB\bin目录，以管理员身份执行cmd
4. 输入一下命令：

> mongod --dbpath "d:\java\MongoDB\data\db" --logpath "d:\java\MongoDB\logs\MongoDB.log"  --auth  --install --serviceName "MongoDB"

5. 启动mongodb服务：

> net start MongoDB

**ps**:如果提示服务无法启动，错误码100，需要进入d:\java\MongoDB\data\db 目录下，删除mongod.lock和storage.bson两个文件，重新输入该命令即可启动服务成功。

6. 在cmd窗口输入命令   mongo   连接mongodb数据库



经过以上步骤，mongodb安装成功。

7. 设置用户名和密码

   > use admin
   > db.createUser({
   >
   > user: "root",
   >
   > pwd:"root",
   >
   > roles: [{role: "root", db: "admin"}]
   >
   > });
   >
   >  
   >
   > --下次登录需要验证时先进admin数据库，然后输入以下命令
   >
   > db.auth("root","root")

   ​

#### 2. MongoDB可视化工具

从官网下载并安装Robo 3T



#### 3. MongoDB基本操作

1. 基本概念

| SQL术语       | MongoDB术语   | 说明                      |
| ----------- | ----------- | ----------------------- |
| database    | database    | 数据库                     |
| table       | collection  | 数据库表/集合                 |
| row         | document    | 数据记录行/文档                |
| column      | field       | 数据字段/域                  |
| index       | index       | 索引                      |
| table joins |             | 表连接，MongoDB不支持          |
| primary key | primary key | 主键，MongoDB自动将_id字段设置为主键 |



2. 查看数据库

   > show dbs

   ​

3. 创建数据库test

   > use test

   ​

4. 删除数据库

   > use test;
   >
   > db.dropDatabase()

   ​

5. 插入文档

   > use test;
   >
   > db.test.insert({
   >
   > ​	name:'abc', 
   >
   > ​	age:35, 
   >
   > ​	mobile:'12345678901', 
   >
   > ​	emial:'123456789@163.com'
   >
   > })
   >
   > ​
   >
   > --以变量形式插入
   >
   > document = ({
   >
   > ​	name:'abc', 
   >
   > ​	age:25, 
   >
   > ​	mobile:'12345678901', 
   >
   > ​	emial:'123456789@163.com'
   >
   > });
   >
   > db.test.insert(document)
   >
   >  
   >
   > -- 插入单条数据
   >
   > var document = db.test.insertOne({name:'aaa'})
   >
   > document
   >
   >  
   >
   > -- 插入多条数据
   >
   > var document = db.test.insertMany([{name:'bbb'},{name:'ccc'}])
   >
   > document

   ​

6.  更新文档

   > use test;
   >
   > db.test.insertMany([{name:'aaa', age:1},{name:'aaa',age:2},{name:'bbb',age:1},{name:'bbb',age:2}])
   >
   >  
   >
   > -- 只更新第一条发现的文档
   >
   > db.test.update({name:'aaa'},{$set:{name:'ccc'}})
   >
   > ​
   >
   > -- 修改多条满足条件的文档
   >
   > db.test.update({name:'bbb'},{$set:{name:'ddd'}}, {multi:true})
   >
   >  
   >
   > --替换现有文档，用新的内容替换_id为100的文档
   >
   > db.test.save({_id:100,name:'eee',age:3})

   ​

7. 删除文档

   > use test
   >
   > db.dropDatabase()
   >
   > use test
   >
   > db.test.insertMany([{name:'aaa', age:1},{name:'aaa',age:2},{name:'bbb',age:1},{name:'bbb',age:2}])
   >
   >  
   >
   > -- 删除name=aaa的所有数据
   >
   > db.test.remove({name:'aaa'})
   >
   > -- 删除name=aaa的一条数据
   >
   > db.test.remove({name:'aaa'},1)
   >
   > -- 删除所有数据
   >
   > db.test.remove({})
   >
   > ​

8. 查询文档

   > db.test.find().pretty()

   | 操作         | 示例                                       | 等效语法                                     |
   | ---------- | ---------------------------------------- | ---------------------------------------- |
   | 相等         | db.test.find({age:25})                   | where age = 25                           |
   | 小于         | db.test.find({age:{$lt:25}})             | where age < 25                           |
   | 小于等于       | db.test.find({age:{$lte:25}})            | where age <= 25                          |
   | 大于         | db.test.find({age:{$gt:25}})             | where age > 25                           |
   | 大于等于       | db.test.find({age:{$gte:25}})            | where age >= 25                          |
   | 不等于        | db.test.find({age:{$ne:25}})             | where age != 25                          |
   | and        | db.test.find({$and:[{name:'aaa'},{age:25}]}) | where name='aaa' and age = 25            |
   | or         | db.test.find({$or:[{name:'aaa'},{name:'bbb'}]}) | where name='aaa' or name='bbb'           |
   | and和or混合使用 | db.test.find({age:{$gt:25}, $or:[{name:'aaa'},{name:'bbb'}]}) | where age < 25 and (name='aaa' or name='bbb') |

   ​

9. limit方法和skip方法

   > use test
   >
   > db.dropDatabase()
   >
   > use test
   >
   > db.test.insertMany([{name:'aaa', age:1},{name:'aaa',age:2},{name:'bbb',age:1},{name:'bbb',age:2}])
   >
   >  -- 查询指定条数的数据：查询name=aaa的1条数据
   >
   > db.test.find({name:'aaa'}).limit(1)
   >
   >  --查询name=aaa的2条数据，并且跳过歉2条数据
   >
   > db.test.find({name:'aaa'}).limit(2).skip(2)

10. 排序

  > use test
  >
  > db.dropDatabase()
  >
  > use test
  >
  > db.test.insertMany([{name:'aaa', age:1},{name:'aaa',age:2},{name:'bbb',age:1},{name:'bbb',age:2}])
  >
  > ​
  >
  >  -- 按照age大小倒序排列:1为正序，-1为倒序   (第一个{}放where条件，为空表示返回集合中的所有文档)
  >
  > db.test.find({}).sort({age:-1})

11. 索引

    > use test
    >
    > db.dropDatabase()
    >
    > use test
    >
    > db.test.insertMany([{name:'aaa', age:1},{name:'aaa',age:2},{name:'bbb',age:1},{name:'bbb',age:2}])
    >
    >  -- 为age创建索引，1为升序，-1为倒序
    >
    > db.test.ensureIndex({age:1})





#### 4. MongoDB role类型说明

1. 数据库用户角色
   + **read** : 授予用户只读数据的权限
   + **readWrite** : 授予用户读写数据的权限
2. 数据库管理角色
   + **dbAdmin** : 在当前数据库中执行管理操作
   + **dbOwner** : 在当前数据库中执行任意操作
   + **userAdmin** : 在当前数据库中管理用户
3. 备份和还原角色
   + **backup** 
   + **restore** 
4. 跨库角色
   + **readAnyDatabase** : 授予在所有数据库上读取数据的权限
   + **readWriteAnyDatabase** : 授予在所有数据库上读写数据的权限
   + **userAdminAnyDatabase** : 授予在所有数据库上管理用户的权限
   + **dbAdminAnyDatabase** : 授予管理所有数据库的权限
5. 集群管理角色
   - **clusterAdmin** : 授予管理集群的最高权限
   - **clusterManager** : 授予管理和监控集群的权限
   - **clusterMonitor** : 授予监控集群的权限，对监控工具具有readonly的权限
   - **hostManager** : 管理server



#### 5. Java 连接 MongoDB

1. 为数据库设置用户权限：role 为 dbOwner   数据库为test，collection为test

   >use test
   >
   >db.createUser({
   >
   >​	user: "root",
   >
   >​	pwd: "root",
   >
   >​	roles: [{ 
   >
   >​		role: "dbOwner,
   >
   >​		db: "test"
   >
   >​	}]
   >
   >})

2.  引用jar包为： mongo-java-driver

3. 具体代码

   + MongoManager.java

     ```java
     package com.demo;

     import com.mongodb.*;
     import com.mongodb.client.MongoDatabase;

     import java.util.Arrays;
     import java.util.List;

     public class MongoManager {

         //服务器地址
         private static final String SERVER = "127.0.0.1";

         //端口号
         private static final int PORT = 27017;

         //数据库名
         private static final String DB_NAME = "test";

         //用户名
         private static final String USERNAME = "root";

         //密码
         private static final String PASSWORD = "root";

         private static MongoClient mongoClient = null;

         /**
          * 初始化连接池
          */
         private MongoManager() {

             init();
         }

         /**
          * 获取MongoClient实例
          * @return
          */
         private static MongoClient getInstance() {
             if (mongoClient == null) {
                 init();
             }
             return mongoClient;
         }

         /**
          * 初始化连接池
          */
         private static void init() {
             MongoClientOptions clientOptions = new MongoClientOptions.Builder().connectionsPerHost(100).threadsAllowedToBlockForConnectionMultiplier(200).build();
             List<MongoCredential> lstCredentials = Arrays.asList(MongoCredential.createCredential(USERNAME, DB_NAME, PASSWORD.toCharArray()));
             mongoClient = new MongoClient(new ServerAddress(SERVER), lstCredentials, clientOptions);
         }

         /**
          * 获取数据库
          * @return
          */
         public static MongoDatabase getDatabase() {

             return getInstance().getDatabase(DB_NAME);
         }

     }
     ```

   + MongoMain.java

     ```java
     package com.demo;

     import com.mongodb.Block;
     import com.mongodb.MongoClient;
     import com.mongodb.client.FindIterable;
     import com.mongodb.client.MongoCollection;
     import com.mongodb.client.MongoDatabase;
     import org.bson.Document;

     import java.util.Arrays;

     public class MongoDBMain {
         public static void main(String[] args) {
             MongoCollection<Document> collection = MongoManager.getDatabase().getCollection("test");

             //clean collection data at first
             collection.drop();
             System.out.println("drop collection successfully...");

             //insert data
             Document document1 = new Document("name", "aaa").append("age", 20).append("sex", "male");
             Document document2 = new Document("name", "bbb").append("age", 21).append("sex", "female");
             Document document3 = new Document("name", "ccc").append("age", 22).append("sex", "male");
             Document document4 = new Document("name", "ddd").append("age", 23).append("sex", "male");
             Document document5 = new Document("name", "eee").append("age", 24).append("sex", "female");

             collection.insertMany(Arrays.asList(document1, document2, document3, document4, document5));

             //find all data
             FindIterable<Document> iterable = collection.find();
             printResult("find all data", iterable);

             printResult("find one data", collection.find(new Document("name", "aaa")));

             //find the data(age > 22)
     //        printResult("find the data(age > 22)", collection.find(new Document(gt("age", 22))));


         }

         /**
          * print data
          * @param description
          * @param iterable
          */
         public static void printResult(String description, FindIterable<Document> iterable) {
             System.out.println(description);
             iterable.forEach(new Block<Document>() {
                 @Override
                 public void apply(Document document) {
                     System.out.println(document);
                 }
             });
             System.out.println("*****************");
             System.out.println();
         }
     }

     ```

     ​

