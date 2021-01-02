MongoDB 是由 C++ 语言编写的非关系型数据库，是一个基于分布式文件存储的开源数据库系统，其内容存储形式类似 JSON 对象，它的字段值可以包含其他文档、数组及文档数组，非常灵活。

MongoDB 支持多种平台，如 Windows、macOS、Linux等，在[官网](https://www.mongodb.com/download-center/community)均可找到对应的安装包。

在本文中，我们学习 Python3 下的 MongoDB 存储操作。

## 1 MongoDB 的安装

MongoDB 数据库支持单机、主从、分片等多种部署架构，我为了学习使用，仅使用 MongoDB 最基本的单机版，在 Windows 平台安装 MongoDB。

在官网下载 msi 安装包，下载地址：https://www.mongodb.com/try/download/community

 下载后双击进行安装，在安装过程中，点击 “Custom” 来自定义安装目录，此处我的安装目录是 D:\MongoDB\Server\4.4。然后点击 Next 进行下一步，默认选中 “Install MongoDB Compass”，我们可以先取消选中，否则需要很长时间执行安装。MongoDB Compass 是一个图形界面管理工具，所以我们可以在安装好 MonogoDB 后再安装。然后 Next 继续进行安装。

注意：我在安装过程中出现如下问题：

<img src="https://gitee.com/linwang0714/ImgHosting/raw/master/article_card_img//1-1607775506141.png" style="zoom:50%;" />

解决办法：

打开 services.msc 服务界面，找到 MongoDB Server，右键 -> 属性 -> 登录，登录身份选择**本地系统账户(L)**。

<img src="https://gitee.com/linwang0714/ImgHosting/raw/master/article_card_img//20201212-02.gif" style="zoom: 33%;" />

设置完成后可以手动启动 MongoDB Server 服务，或者点击刚刚安装过程中弹窗上的 Retry 重试。

安装好 MongoDB 后，需要继续配置 MongoDB 服务。

在安装目录下的 data 文件夹中新建两个文件夹 db 和 log，db 是用来存储数据，log 是用来存储日志数据。在 log 文件夹中新建一个 mongodb.log 文件，用于存储日志。

然后以管理员模式打开命令行，切换到 MongoDB 安装路径下的 bin 目录中，然后执行以下命令：

```
mongod --bind_ip 0.0.0.0 --logpath "D:\MongoDB\Server\4.4\data\log\mongodb.log" --logappend --dbpath "D:\MongoDB\Server\4.4\data\db" --port 27017 --serviceName "MongoDB" --serviceDisplayName "MongoDB" --install
```

--bind_ip 参数用于绑定 IP，参数值 0.0.0.0  表示任意 IP 均可访问。--logpath 指定日志存储路径，--dbpath 指定数据存储路径，--port 指定端口，--serviceName 指定服务名称，--serviceDisplayName 指定服务名称，有多个 mongodb 服务时执行。

<img src="https://gitee.com/linwang0714/ImgHosting/raw/master/article_card_img//20201212-03.png" style="zoom:50%;" />

如果没有出现错误提示，则证明 MongoDB 服务已经安装成功。

我们可以通过以下命令启动、停止服务：

net start MongoDB 开启服务

net stop MongoDB 关闭服务

启动 MongoDB 服务后，我们就可以在命令行利用 mongo 进入 MongoDB 命令交互环境了。可能会出现命令不存在的情况，需要将 MongoDB 路径下的 bin 目录添加到系统环境环境中。

这里推荐一个 MongoDB 可视化工具 Robo 3T，下载地址：https://robomongo.org/download

我们安装了 MongoDB 数据库后，还需要安装一个 PyMongo 存储库，它是 Python 用来操作 MongoDB 的第三方库。

直接用 pip 安装：

```bash
pip install pymongo
```

## 2 使用 PyMongo 操作 MongoDB

### 2.1 连接 MongoDB

方式一：

使用 PyMongo 库里面的 MongoClient 方法，传入 MongoDB 的 IP 及端口即可，其中第一个参数为地址 host，第二个参数为端口 port（如果不给它传递参数，则默认是 27017）。

```python
import pymongo

client = pymongo.MongoClient(host='localhost', port=27107)
```

方式二：

MongoClient 的第一个参数 host 还可以直接传入 MongoDB 的连接字符串，它以 mongodb 开头。

```python
client = MongoClient('mongodb://localhost:27017/')
```

### 2.2 指定数据库

MongoDB 中可以建立多个数据库，我们需要指定操作其中一个数据库。

```python
db = client.test # 等价于：db = client['test']
```

调用 client 的 test 属性即可返回 test 数据库。

### 2.3 指定集合

MongoDB 的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表。

下一步需要指定要操作的集合，这里我们指定一个名称为 students 的集合。与指定数据库类似，指定集合也有两种方式：

```python
collection = db.students # 等价于：collection = db['students']
```

这样我们便声明了一个 Collection 对象。

### 2.3 插入数据

**插入单条记录：insert_one**

```python
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}
result = collection.insert_one(student)
print(result)
print(result.inserted_id)
# 输出
<pymongo.results.InsertOneResult object at 0x0000021383202C40>
5fefe5ef0e91a3455b9c63f6
```

返回的是 InsertOneResult 对象。在 MongoDB 中，每条数据其实都有一个 `_id` 属性来唯一标识，我们可以调用其 `inserted_id` 属性获取 `_id`。

**插入多条记录：insert_many**

```python
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}
student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}
result = collection.insert_many([student1, student2])
print(result)
print(result.inserted_ids)
# 输出
<pymongo.results.InsertManyResult object at 0x000001F0F3D77F40>
[ObjectId('5fefe75350350dbbfb8f3ac9'), ObjectId('5fefe75350350dbbfb8f3aca')]
```

该方法返回的类型是 InsertManyResult，调用 `inserted_ids` 属性可以获取插入数据的 `_id` 列表。

此时我们可以看到集合 students 中有 3 条记录：

<img src="https://gitee.com/linwang0714/ImgHosting/raw/master/article_card_img//image-20210102223733842.png" style="zoom:50%;" />

### 2.4 查询

插入数据后，我们可以利用 find_one 或 find 方法进行查询，其中 find_one 查询得到的是单个结果，find 则返回一个生成器对象。

**单条数据的查询：**

```python
result = collection.find_one({'name': 'Mike'})
print(type(result))
print(result)
# 输出
<class 'dict'>
{'_id': ObjectId('5fefe75350350dbbfb8f3aca'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}
```

返回结果是字典类型。可以发现，它多了 _id 属性，这就是 MongoDB 在插入过程中自动添加的。

如果查询结果不存在，则会返回 None。

**多条数据的查询：**

```python
results = collection.find({'age': 20})
print(results)
for result in results:
    print(result)
# 输出
<pymongo.cursor.Cursor object at 0x000001E374CF46D0>
{'_id': ObjectId('5fefe5ef0e91a3455b9c63f6'), 'id': '20170101', 'name': 'Jordan', 'age': 20, 'gender': 'male'}
{'_id': ObjectId('5fefe75350350dbbfb8f3ac9'), 'id': '20170101', 'name': 'Jordan', 'age': 20, 'gender': 'male'}
```

返回结果是 Cursor 类型，它相当于一个生成器，我们需要遍历获取的所有结果，其中每个结果都是字典类型。

如果要查询年龄大于 20 的数据，则写法如下：

```python
results = collection.find({'age': {'$gt': 20}})
print(results)
for result in results:
    print(result)
# 输出
<pymongo.cursor.Cursor object at 0x00000179E66146D0>
{'_id': ObjectId('5fefe75350350dbbfb8f3aca'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}
```

这里查询的条件键值已经不是单纯的数字了，而是一个字典，其键名为比较符号 $gt，意思是大于，键值为 20。

**比较符号归纳如下：**

| 符号 |    含义    |            示例             |
| :--: | :--------: | :-------------------------: |
| $lt  |    小于    |    {'age': {'$lt': 20}}     |
| $gt  |    大于    |    {'age': {'$gt': 20}}     |
| $lte | 小于或等于 |    {'age': {'$lte': 20}}    |
| $gte | 大于或等于 |    {'age': {'$gte': 20}}    |
| $ne  |   不等于   |    {'age': {'$ne': 20}}     |
| $in  |  在范围内  | {'age': {'$in': [20, 23]}}  |
| $nin | 不在范围内 | {'age': {'$nin': [20, 23]}} |

另外，还可以进行正则匹配查询。例如，查询名字以 M 开头的学生数据，示例如下：

```python
results = collection.find({'name': {'$regex': '^M.*'}})
print(results)
for result in results:
    print(result)
# 输出
<pymongo.cursor.Cursor object at 0x00000270AB9746D0>
{'_id': ObjectId('5fefe75350350dbbfb8f3aca'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}
```

这里使用 $regex 来指定正则匹配，^M.* 代表以 M 开头的正则表达式。

**其他一些功能符号归类如下：**

|  符号   |      含义      |             示例             |     示例含义     |
| :-----: | :------------: | :--------------------------: | :--------------: |
| $regex  | 匹配正则表达式 | {'name': {'$regex': '^M.*'}} |  name 以 M 开头  |
| $exists |  属性是否存在  | {'name': {'$exists': True}}  |  name 属性存在   |
|  $type  |    类型判断    |  {'age': {'$type': 'int'}}   | age 的类型为 int |
|  $mod   |   数字模操作   |  {'age': {'$mod': [5, 0]}}   |  age 模 5 余 0   |

### 2.5 计数

要统计查询结果有多少条数据，可以调用 count 方法。

```python
count1 = collection.find().count() # 统计所有数据条数
count2 = collection.find({'age': 20}).count()  # 统计符合某个条件的数据
print(count1)
print(count2)
# 
3
2
```

### 2.6 排序

排序时，我们可以直接调用 sort 方法，并在其中传入排序的字段及升降序标志。

```python
results = collection.find().sort('name', pymongo.ASCENDING)
print([result['name'] for result in results])
# 输出
['Jordan', 'Jordan', 'Mike']
```

这里调用 pymongo.ASCENDING 指定升序。如果要降序排列，可以传入 pymongo.DESCENDING。

### 2.7 偏移

在某些情况下，我们可能只需要取某几个元素，这时可以利用 skip 方法偏移几个位置。

```python
results = collection.find().sort('name', pymongo.ASCENDING).skip(1)
print([result['name'] for result in results])
# 输出
['Jordan', 'Mike']
```

skip(1) 表示忽略第一个元素，得到第 2 个及以后的元素。

另外，我们还可以用 limit 方法指定要取的结果个数。

```python
results = collection.find().sort('name', pymongo.ASCENDING).skip(1).limit(1)
print([result['name'] for result in results])
# 输出
['Jordan']
```

### 2.8 更新

对于数据更新，我们可以使用 update 方法，指定更新的条件和更新后的数据即可。

**更新单条数据：**

```python
condition = {'name': 'Mike'}
student = collection.find_one(condition)
student['age'] = 26
result = collection.update_one(condition, {'$set': student})
print(result)
print(result.matched_count, result.modified_count)
# 输出
<pymongo.results.UpdateResult object at 0x0000021967C93740>
1 1
```

第 2 个参数需要使用 {'$set': student} 这样的形式，其返回结果是 UpdateResult 类型。然后分别调用 matched_count 和 modified_count 属性，可以获得匹配的数据条数和影响的数据条数。

**更新多条数据：**

```python
condition = {'age': {'$gte': 20}}
result = collection.update_many(condition, {'$inc': {'age': 1}})
print(result)
print(result.matched_count, result.modified_count)
# 输出
<pymongo.results.UpdateResult object at 0x0000023F8E87D7C0>
3 3
```

### 2.9 删除

直接调用 remove 方法指定删除的条件即可，此时符合条件的所有数据均会被删除。

```python
result = collection.remove({'name': 'Jordan'})
print(result)
# 输出
{'n': 2, 'ok': 1.0}
```

另外，这里依然存在两个新的推荐方法 —— delete_one 和 delete_many。delete_one 即删除第一条符合条件的数据，delete_many 即删除所有符合条件的数据。它们的返回结果都是 DeleteResult 类型，可以调用 deleted_count 属性获取删除的数据条数。

```python
result = collection.delete_one({'name': 'Jordan'})
print(result)
print(result.deleted_count)
result = collection.delete_many({'age': {'$lt': 25}})
print(result.deleted_count)
# 输出
<pymongo.results.DeleteResult object at 0x000001776BFF4AC0>
0
1
```

