[TOC]

> springboot上使用MongoTemplate和MongoRepository这两种方式来操作mongodb

[本文csdn连接](https://blog.csdn.net/sinat_32602455/article/details/103564927)



# 一.Spring boot 使用MongoTemplate

## 1.配置mongodb连接

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://账号:密码@IP:27017/test
```

![](C:\Users\DELL\Documents\md图片\连接.jpg)



再serve或者controller下注入MongoTemplate

```java
@Autowired
MongoTemplate mongoTemplate;
```



![](C:\Users\DELL\Documents\md图片\注入.jpg)

## 2.插入数据

①第一种方式，使用mongoTemplate.insert，这种方式就相当于mysql的insert语句，会发生主键冲突

```java
User user = new User();
user.setId(UUID.randomUUID().toString());
user.setName("Ryan");
user.setAge(25);
mongoTemplate.insert(user);
```

![](C:\Users\DELL\Documents\md图片\插入insert.jpg)

②第二种方式，使用mongoTemplate.save，这个方式相当于mysql的replace语句，遇到存在的主键会更新数据

```java
User user2 = new User();
user2.setId(UUID.randomUUID().toString());
user2.setName("Ryan2");
user2.setAge(25);
mongoTemplate.save(user2);
```

![](C:\Users\DELL\Documents\md图片\插入save.jpg)



执行插入语句后，查询数据，验证结果

![](C:\Users\DELL\Documents\md图片\验证插入.jpg)



## 3.查询数据

上面插入了两条数据，现在执行查询语句

①使用mongoTemplate.findOne，查询返回第一条name为Ryan的数据

```java
Query query = new Query();
query.addCriteria(Criteria.where("name").is("Ryan2"));
User user = mongoTemplate.findOne(query, User.class);
logger.info(gson.toJson(user));
```

![](C:\Users\DELL\Documents\md图片\查询ryan2.jpg)



执行查询，在控制台查看打印的查询结果

![image-20191216150845726](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20191216150845726.png)

②使用mongoTemplate.find和Pattern进行模糊查询，查询name包含ryan的数据

```java
Query query = new Query();
Pattern pattern= Pattern.compile("^.*"+ "ryan" +".*$", Pattern.CASE_INSENSITIVE);
query.addCriteria(Criteria.where("name").regex(pattern));
List<User> list = mongoTemplate.find(query, User.class);
logger.info(gson.toJson(list));
```

![](C:\Users\DELL\Documents\md图片\模糊查询.jpg)

执行查询，在控制台查看打印的查询结果

![](C:\Users\DELL\Documents\md图片\模糊查询结果.jpg)

③使用orOperator进行多条件查询，orOperator相当于mysql的or

```jav
Query query = new Query();
query.addCriteria(new Criteria().orOperator(Criteria.where("name").is("Ryan"), Criteria.where("name").is("Ryan2")));
List<User> list = mongoTemplate.find(query, User.class);
logger.info(gson.toJson(list));
```

![](C:\Users\DELL\Documents\md图片\or查询.jpg)

执行查询，在控制台查看打印的查询结果

![](C:\Users\DELL\Documents\md图片\or查询结果.jpg)



## 4.更新数据

①使用mongoTemplate.updateFirst更新name为Ryan的数据，把age字段更新为100

```java
Update update = new Update();
update.set("age", "100");
Query query = new Query();
query.addCriteria(Criteria.where("name").is("Ryan"));
UpdateResult result = mongoTemplate.updateFirst(query, update, User.class);
logger.info(gson.toJson(result));
```

![](C:\Users\DELL\Documents\md图片\update100.jpg)



查询数据库，验证结果

![](C:\Users\DELL\Documents\md图片\updete100结果.jpg)

②使用mongoTemplate.updateMulti进行批量更新

```java
Update update = new Update();
update.set("age", "30");
Query query = new Query();
Pattern pattern= Pattern.compile("^.*"+ "ryan" +".*$", Pattern.CASE_INSENSITIVE);
query.addCriteria(Criteria.where("name").regex(pattern));
UpdateResult result = mongoTemplate.updateMulti(query, update, User.class);
logger.info(gson.toJson(result));
```

![](C:\Users\DELL\Documents\md图片\update批量.jpg)

查询数据库，验证结果

![](C:\Users\DELL\Documents\md图片\update批量结果.jpg)



## 5.删除数据

使用mongoTemplate.remove删除name为Ryan2的数据

```java
Query query = new Query();
query.addCriteria(Criteria.where("name").is("Ryan2"));
DeleteResult result = mongoTemplate.remove(query, User.class);
logger.info(gson.toJson(result));
```

![](C:\Users\DELL\Documents\md图片\delete.jpg)

查询数据库，验证结果

![](C:\Users\DELL\Documents\md图片\delete结果.jpg)



# 二.Spring boot 使用MongoRepository

MongoRepository和Jpa的JpaRepository的使用方法事一样的

```java
// TODO 更新MongoRepository使用教程
```