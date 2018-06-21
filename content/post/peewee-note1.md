---
title: peewee笔记(1)——基本语法
tags: 
- peewee
- MySQL
- Python
date: 2016-03-23 10:44:27
---

# peewee使用指南

本系列文章基于peewee 2.6.4，同时会参照Django Orm做点比较。

## 1.peewee介绍
peewee是一个轻量级Python ORM，支持Sqlite、MYSQL、PostgresqlD等数据库。

## 2.使用指南

### 2.1.连接mysql

#### 2.1.1.连接形式

使用url连接，方式和django类似，如：
```
DATABASES = {
    'default': {
        'ENGINE': 'mysql',
        'NAME': 'name',
        'USER': 'user',                      
        'PASSWORD': 'pass',                  
        'HOST': 'url',
        'PORT': '',
        'CONN_MAX_AGE': 100
    }
}
```
<!-- more --> 
### 2.2.model定义

#### 2.2.1.概述
一句话描述：**和Django一样用。**

model定义基本和Django一致，
需要注意的是：
1. 属性和Django基本一致
2. 有些Django并不影响使用的东西peewee不支持，目前注意到的是`Field.choices`。
3. peewee可以使用`playhouse.signals`实现信号机制。

详见：<http://docs.peewee-orm.com/en/stable/peewee/models.html#fields>


## 3.ORM操作（CRUD）
示例Model：

```python
from peewee import *

db = SqliteDatabase('people.db')

class Person(Model):
    name = CharField(default='')
    age = IntegerField(default=0) 

    class Meta:
        database = db # This model uses the "people.db" database.

```
注：这是个model示例，实际上'database=db'已经封装在`db.model.Model`中，在python-service-base中不用写。

### 3.1.增加（Create）

1. create()方法：

```python
grandma =Person.create(name='Grandma', age = 1)
```

2. save()方法

```python
person = Person()
person.name = 'bill'
person.age =10
person.save()
```

### 3.2.查（Retrieve）

peewee中查询记录分为get（查询单条记录）、select(条件查询批量记录)。


#### 3.2.1.查询单条记录

注：get方法获取不存在的记录会抛出异常。

1. `Product.get(name='abc')`
或者
`Product.get(Product.name=='abc')`

2. `Product.select().where(Product.name=='abc').get()`
3. `Product.select().where(Product.name=='abc').first()`

**注:**
1. get()支持name='abc'也支持Product.name=='abc'的写法，where**只**Product.name=='abc'。
2. 前两种都是get方法，查询不到会抛出异常，select方法查询不到返回None。


#### 3.2.2.查询全部

`Product.select()`等价于Django中的`Product.select().all()`，返回一个可迭代的SelectQuery对象。

#### 3.2.3.条件查询
```
Product.select().where(Product.name=='abc')
```
返回一个可迭代的SelectQuery对象。

- 或(OR)查询：**|**，例如：`User.select().where((User.is_staff == True) | (User.is_superuser == True))`
- 且(AND)查询:**&**，例如：`Product.select().where(Product.name=='abc',Product.owner_id==10086)` 或者 `Product.select().where((Product.name=='abc') & (Product.owner_id==10086))`
- 非(NOT)：**~**,没用过。。。
- 比较：peewee支持如下比较符

    ```
    ==  x equals y
    <   x is less than y
    <=  x is less than or equal to y
    >   x is greater than y
    >=  x is greater than or equal to y
    !=  x is not equal to y
    <<  x IN y, where y is a list or query
    >>  x IS y, where y is None/NULL
    %   x LIKE y where y may contain wildcards
    **  x ILIKE y where y may contain wildcards
    ~   Negation
    ```

- 其他查询

    ```
    .contains(substr) 	Wild-card search for substring.
    .startswith(prefix) 	Search for values beginning with prefix.
    .endswith(suffix) 	Search for values ending with suffix.
    .between(low, high) 	Search for values between low and high.
    .regexp(exp) 	Regular expression match.
    .bin_and(value) 	Binary AND.
    .bin_or(value) 	Binary OR.
    .in_(value) 	IN lookup (identical to <<).
    .not_in(value) 	NOT IN lookup.
    .is_null(is_null) 	IS NULL or IS NOT NULL. Accepts boolean param.
    .concat(other) 	Concatenate two strings using ||.
    ```

示例：

查询id in ids，ids是个list。

 - `Product.select().where(Product.id << ids)`
 - `Product.select().where(Product.id.in_(ids))`

等价于Django中的`Product.objects.filter(id__=ids)`。


#### 3.2.3.计数count
`Product.select().where(Product.name=='abc').count()`。
SelectQuery对象并不支持len()方法，即执行`len(Product.select().where(Product.name=='abc'))`会抛出TypeError异常。虽然可以实现支持len，但滥用可能带来性能风险。

#### 3.2.5.排序order_by

- 降序：使用"-"或者"desc()",`Tweet.select().order_by(Tweet.created_date.desc())` 等价于 `Tweet.select().order_by(-Tweet.created_date)`
- 升序：使用"+"或者"asc()"或者不写，`User.select().order_by(+User.username)`


### 3.3.删除（Delete）

**建议使用方法1**

1. Model.delete (Python class method, in API Reference)
`mall_models.Order.delete().where(mall_models.Order.id=order.id).execute()`
或者
```python
p = mall_models.Order.delete().where(mall_models.Order.id=order.id)
p.execute()
```
也就是执行了execute()才会执行删除操作。

2. Model.delete_instance (Python method, in API Reference)
```
p = Product.get(Product.name=='abc')
p.elete_instance()
```

3. DeleteQuery
`dq = DeleteQuery(User).where(User.active == False).execute()`

### 3.4.改（Update）

1. update修改(推荐)
```python
query = Stat.update(counter=Stat.counter + 1).where(Stat.url == request.url)
query.execute()
```
同delete,也就是执行了execute()才会执行update操作。

2. save修改
```python
for stat in Stat.select().where(Stat.url == request.url):
     stat.counter += 1
     stat.save()
```

两种方式都类似Django语句，**需要特别注意的是，peewee的update是原子的（Atomic ）**，在并发情况下可靠，即peewee的update语句效果等价于Django使用F表达式的update，如`rules.update(remained_count=F('remained_count') - 1, get_count=F('get_count') + 1)`

## 4.外键与join

```python
class OrderHasProduct(models.Model):
	order = models.ForeignKey(Order)
	product = models.ForeignKey(Product, related_name='product')
	price = models.FloatField()  # 商品单价


class Order(models.Model):
	order_id = models.CharField(max_length=100)  # 订单号
	webapp_user_id = models.IntegerField()  # WebApp用户的id
	webapp_id = models.CharField(max_length=20, verbose_name='店铺ID')  # webapp,订单成交的店铺id
	webapp_source_id = models.IntegerField(default=0, verbose_name='商品来源店铺ID')  # 订单内商品实际来源店铺的id，已废弃
	buyer_name = models.CharField(max_length=100)  # 购买人姓名


class Product(models.Model):
	owner = models.ForeignKey(User, related_name='user-product')
	name = models.CharField(max_length=256)  # 商品名称
	physical_unit = models.CharField(default='', max_length=256)  # 计量单位
	price = models.FloatField(default=0.0)  # 商品价格

```

OrderHasProduct有外键Product和外键Order。

### 4.1.外键赋值

```
mall_models.OrderHasProduct.create(
	order=self.db_model,
	product=product.id,
	...
```
可以看到把**外键实例**或者**外键实例的id**都是可以的。

### 4.2.涉及外键的查询

#### 4.2.1 根据外键的主键查询
直接`OrderHasProduct.select().where(where(mall_models.Order.id=order.id).product_id=10086)`

#### 4.2.2 根据外键的某字段查询（join外键）
```
class Rule(models.Model):
	owner = models.ForeignKey(User)
	name = models.CharField(max_length=20, db_index=True) 
	valid = models.DecimalField(max_digits=65) 


class Card(models.Model):
	owner = models.ForeignKey(User)
	rule = models.ForeignKey(Rule) 
```
Card有外键Rule。

```
count = Card.select().join(wzcard_models.Rule).where(Card.card_id.in_(ids),Rule.valid >0).count()
```
的确很长，peewee不支持django风格的"__"（双下划线），所以就直街上join了，peewee默认使用INNER join。

