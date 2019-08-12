# 一、切片的创建和初始化

```
1、通过 make() 函数创建切片
使用 Golang 内置的 make() 函数创建切片，此时需要传入一个参数来指定切片的长度
// 创建一个整型切片
// 其长度和容量都是 5 个元素
slice := make([]int, 5)

此时只指定了切片的长度，那么切片的容量和长度相等。也可以分别指定长度和容量：
// 创建一个整型切片
// 其长度为 3 个元素，容量为 5 个元素
slice := make([]int, 3, 5)
分别指定长度和容量时，创建的切片，底层数组的长度是指定的容量，但是初始化后并不能访问所有的数组元素。
```
参考文档：

https://www.jianshu.com/p/354fce23b4f0  Golang 入门 : 切片(slice)

https://www.cnblogs.com/sparkdev/tag/Golang/   golang基础
