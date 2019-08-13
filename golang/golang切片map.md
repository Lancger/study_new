#  一、golang数据类型map

## 1 map简介

    map是一堆键值对的未排序集合，类似Python中字典的概念，它的格式为map[keyType]valueType，是一个key-value的hash结构。map的读取和设置也类似slice一样，通过key来操作，只是slice的index只能是int类型，而map多了很多类型，可以是int，可以是string及所有完全定义了==与!=操作的类型。
    在C++/Java中，map一般都以库的方式提供，比如在C++中是STL的std::map<>，在Java中是Hashmap<>，在这些语言中，如果要使用map，事先要引用相应的库。而在Go中，使用map不需要引入任何库，并且用起来也更加方便。

## 2 map声明

```
声明map的语法如下

var map变量名 map[key] value

    其中：key为键类型，value为值类型

    例如：value不仅可以是标注数据类型，也可以是自定义数据类型

var numbers map[string] int
var myMap map[string] personInfo

personInfo为自定义结构体，存储个人信息，定义如下

type personInfo struct {
    ID string
    Name string
    Address string
}
```
