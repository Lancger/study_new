interface{}叫做空接口，空接口表示包含了0个方法的集合，由于任何类型都至少实现了0个方法，所以空接口可以承接任意类型。

前面学习的变量都要在声明时候确定类型，虽然用:=可以不用指定类型，但实际是类型推倒，在赋值后类型就确定下来不能改了。

```
空接口：不用指定类型变量，并且类型可变。比如：

var i interface{}
i = 42  //这个时候i就是int类型
i = "hello" //这个时候i就是string类型


interface{}这么写比较方便，当然也写成2行：

type I interface {
}
```
完整例子
```
package main

import "fmt"

type I interface{}

func main() {
    var i I
    i = 42 //这个时候i就是int类型
    fmt.Printf("%v,%T\n", i, i)
    i = "hello" //这个时候i就是string类型
    fmt.Printf("%v,%T\n", i, i)
}
```
输出
```
42,int
hello,string
```

空接口对切片的影响

1、若将一个array或slice赋值给空接口，这个空接口无法再进行切片

2、array或slice赋值给空接口的行为不是复制，而是类似指针效果，只不过无法再进行切片，但元素和原来的array、slice及其衍生的，都有关联

```
package main

import "fmt"

func main() {
    sli := []int{2, 3, 5, 7, 11, 13}

    var e interface{}
    e = sli

    f := sli[0:3]
    f[2] = 55

    fmt.Printf("%T,%v\n", sli, sli)
    fmt.Printf("%T,%v\n", e, e)
    fmt.Printf("%T,%v\n", f, f)
}

```

输出
```
[]int,[2 3 55 7 11 13]
[]int,[2 3 55 7 11 13]
[]int,[2 3 55]
```

若改为
```
package main

import "fmt"

func main() {
    sli := []int{2, 3, 5, 7, 11, 13}

    var e interface{}
    e = sli

    g := e[1:3]
    fmt.Println(g)
}

```

报错
```
./example.go:11: cannot slice e (type interface {})

```

## 空接口承接指针变量¶

```
var empty interface{}
empty = w

```
由于空接口一定能够承接成功，因此这里不需要类型断言

下面这个例子是关于空接口承接了指针变量后如何获得承接的变量（指针）指向的值

```
package main

import "fmt"

func main() {
    var a int = 10
    p := &a
    var e interface{} = p

    fmt.Printf("p %T %v\n", p, p)
    fmt.Printf("*p %T %v\n", *p, *p)
    fmt.Printf("e %T %v\n", e, e)
    fmt.Printf("e.(*int) %T %v\n", e.(*int), e.(*int))
    fmt.Printf("*e.(*int) %T %v\n", *e.(*int), *e.(*int)) // *e.(*int)可以写为*(e.(*int))，前者是后者的简写方式
}

```
输出

```
p *int 0xc42000e248
*p int 10
e *int 0xc42000e248
e.(*int) *int 0xc42000e248
*e.(*int) int 10
```

上面这个例子是空接口的时候用类型断言来获得指针指向的值

参考资料：

https://cyent.github.io/golang/method/interface_empty/  empty interface空接口
