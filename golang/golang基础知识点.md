# 一、变量的定义

1、使用var关键字
```
var a, b, c bool

var s1, s2 string = "hello", "world"

可放在函数内，或直接放在包内

使用var()集中定义变量

var (
	aa = 3
	bb = true
	cc = false
)
```

2、使用 := 定义变量
```
a, b, i, s1, s2 := true, false, 3, "hello", "world"

只能在函数内使用

#样例程序
package main

import "fmt"

// 使用 := 定义变量(只能在函数内使用)
func varliablesVal() {
	a, b, i, s1, s2 := true, false, 3, "hello", "world"
	fmt.Println(a, b, i, s1, s2)
}

func main() {
	varliablesVal()
}
```
