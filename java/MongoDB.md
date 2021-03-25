# MongoDB

### 1.概念

MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。在高负载的情况下，添加更多的节点，可以保证服务器性能。MongoDB 旨在为WEB 应用提供可扩展的高性能数据存储解决方案。 MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

```html
{
    name:"sue",
    age:26,
    groups:["news","sports"]
}
```

### 2.特点

- MongoDB 是一个面向文档存储的数据库，操作起来比较简单和容易。
- 你可以在MongoDB 记录中设置任何属性的索引 (如：FirstName="Sameer",Address="8 Gandhi Road")来实现更快的排序。
- 你可以通过本地或者网络创建数据镜像，这使得MongoDB 有更强的扩展性。
- 如果负载的增加（需要更多的存储空间和更强的处理能力） ，它可以分布在计算机网络中的其他节点上这就是所谓的分片。
- Mongo 支持丰富的查询表达式。查询指令使用JSON 形式的标记，可轻易查询文档中内嵌的对象及数组。
- MongoDb 使用update()命令可以实现替换完成的文档（数据）或者一些指定的数据字段 。
- Mongodb 中的Map/reduce 主要是用来对数据进行批量处理和聚合操作。
- Map 和Reduce。Map 函数调用emit(key,value)遍历集合中所有的记录，将key 与value 传给Reduce 函数进行处理。
- Map 函数和Reduce 函数是使用Javascript 编写的，并可以通过db.runCommand 或mapreduce 命令来执行MapReduce 操作。
- GridFS 是MongoDB 中的一个内置功能，可以用于存放大量小文件。
- MongoDB 允许在服务端执行脚本，可以用Javascript 编写某个函数，直接在服务端执行，也可以把函数的定义存储在服务端，下次直接调用即可。