#  一、golang数据类型map

## 1、map简介

    map是一堆键值对的未排序集合，类似Python中字典的概念，它的格式为map[keyType]valueType，是一个key-value的hash结构。map的读取和设置也类似slice一样，通过key来操作，只是slice的index只能是int类型，而map多了很多类型，可以是int，可以是string及所有完全定义了==与!=操作的类型。
    在C++/Java中，map一般都以库的方式提供，比如在C++中是STL的std::map<>，在Java中是Hashmap<>，在这些语言中，如果要使用map，事先要引用相应的库。而在Go中，使用map不需要引入任何库，并且用起来也更加方便。

## 2、map声明

```
声明map的语法如下

var map变量名 map[key] value

其中：key为键类型，value为值类型
```

```
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
## 3、map初始化
```
3.1 直接初始化（创建）

rating := map[string] float32 {"C":5, "Go":4.5, "Python":4.5, "C++":2 }

myMap := map[string] personInfo{"1234": personInfo{"1", "Jack", "Room 101,..."},}

3.2 通过make初始化（创建）

Go语言提供的内置函数make()可以用于灵活地创建map。
创建了一个键类型为string,值类型为int的map

numbers := make(map[string] int)

创建了一个键类型为string,值类型为personInfo的map

myMap = make(map[string] personInfo)

也可以选择是否在创建时指定该map的初始存储能力，如创建了一个初始存储能力为5的map

myMap = make(map[string] personInfo, 5)

创建后初始化如下：

numbers["one"] = 1 
myMap["1234"] = personInfo{"1", "Jack", "Room 101,..."}
```

## 4、map元素查找

    在Go语言中，map的查找功能设计得比较精巧。判断是否成功找到特定的键，不需要检查取到的值是否为nil，只需查看第二个返回值。要从map中查找一个特定的键，可以通过下面的代码来实现：
```
value, ok := myMap["1234"]
if ok{
    //处理找到的value
}
```

## 5、map元素修改（赋值）

5.1 直接修改
```
numbers["one"] = 11
```

5.2 间接修改

map是一种引用类型，如果两个map同时指向一个底层，那么一个改变，另一个也相应的改变。
```
numbersTest := numbers
numbersTest["one"] = "111"

现在numbers["one"]的值变为"111"了。
```

## 6、map元素删除

Go语言提供了一个内置函数delete()，用于删除容器内的元素。如

```
delete(number, "one")
```

上面的代码将从myMap中删除键为“one”的键值对。如果“one”这个键不存在，那么这个调用将什么都不发生，也不会有什么副作用。但是如果传入的map变量的值是nil，该调用将导致程序抛出异常（panic）。

## 7、实例代码
```
package main

import(
    "fmt"
)

type persionInfo struct{
    ID string
    Name string
    Address string
}

func main(){
    /*
    //声明一个map变量numbers,键名为string,值为int
    var numbers map[string] int
    //给map变量创建值,同时指定最多可以存储3个int值
    numbers = make(map[string] int, 3)
    //map元素赋值
    numbers["one"] = 1
    numbers["two"] = 2
    numbers["three"] = 3
    */

    //上面方式的简写方法
    numbers := map[string] int{
        "one": 1,
        "two": 2,
        "three": 3,
    }
    
    var myMap map[string] persionInfo
    myMap = make(map[string] persionInfo)
    myMap["persion1"] = persionInfo{"1", "Amiee", "Street 101"}
    myMap["persion2"] = persionInfo{"2", "Beva", "Street 102"}
    myMap["persion3"] = persionInfo{"3", "Cencey", "Street 103"}
    
    /*
    // 上面方式的简写方法
    myMap := map[string] persionInfo{
        "persion1": persionInfo{"1", "Amiee", "Street 101"},
        "persion2": persionInfo{"2", "Beva", "Street 102"},
        "persion3": persionInfo{"3", "Cencey", "Street 103"},
    }
    */
    
    //map元素打印
    fmt.Printf("%v\n", numbers)
    fmt.Println(numbers)
    fmt.Println(numbers["two"])

    fmt.Println(myMap)
    fmt.Println(myMap["persion1"])
    
    //map元素查找
    p1, ok := myMap["persion1"]
    if ok{
        fmt.Println("Found persion1, name", p1.Name, ", info", p1 )
    }else{
        fmt.Println("Not Found persion1")
    }
    
    //map元素修改
    //map是一种引用类型，如果两个map同时指向一个底层，那么一个改变，另一个也相应的改变。
    numbersTest := numbers
    numbersTest["one"] = 11
    fmt.Println(numbers)   

    //map元素删除
    delete(numbers, "one")
    fmt.Println(numbers)    
}
```
输出结果如下
```
[root@localhost mygo]# go run  test.go 
map[one:1 two:2 three:3]
map[one:1 two:2 three:3]
2
map[persion1:{1 Amiee Street 101} persion2:{2 Beva Street 102} persion3:{3 Cencey Street 103}]
{1 Amiee Street 101}
Found persion1, name Amiee , info {1 Amiee Street 101}
map[one:11 two:2 three:3]
map[two:2 three:3]
```
## 8、注意事项

```
map是无序的，每次打印出来的map都会不一样，它不能通过index获取，而必须通过key获取。
map的长度是不固定的，也就是和slice一样，也是一种引用类型。
内置的len函数同样适用于map，返回map拥有的key的数量。
map的值可以很方便的修改，通过重新赋值即可。

```
参考文档：

https://studygolang.com/articles/3637   golang数据类型map 

https://blog.csdn.net/lengyuezuixue/article/details/81383183  Golang中map的初始化及使用  
