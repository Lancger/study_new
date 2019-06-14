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

3、常量和枚举
```
a、普通枚举类型
b、自增值枚举类型

变量类型写在变量名之后
编译器可推测变量类型
没有char,只有runne
原始支持复数类型

package main

import "fmt"

// 常量和枚举
func enmus() {

	// const (
	// 	cpp    = 0
	// 	java   = 1
	// 	python = 2
	// 	golang = 3
	// )
	// fmt.Println(cpp, java, python, golang)

	const (
		cpp = iota
		_
		java
		python
		golang
	)
	fmt.Println(cpp, java, python, golang)

	//字节转换
	const (
		b = 1 << (10 * iota)
		kb
		mb
		gb
		tb
		pb
	)
	fmt.Println(b, kb, mb, gb, tb, pb)

}

func main() {
	enmus()
}

#输出结果
API server listening at: 127.0.0.1:5595
0 2 3 4
1 1024 1048576 1073741824 1099511627776 1125899906842624
```
