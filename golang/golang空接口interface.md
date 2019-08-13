```
interface{}叫做空接口，空接口表示包含了0个方法的集合，由于任何类型都至少实现了0个方法，所以空接口可以承接任意类型。

前面学习的变量都要在声明时候确定类型，虽然用:=可以不用指定类型，但实际是类型推倒，在赋值后类型就确定下来不能改了。

空接口：不用指定类型变量，并且类型可变。比如：

var i interface{}
i = 42  //这个时候i就是int类型
i = "hello" //这个时候i就是string类型
```
参考资料：

https://cyent.github.io/golang/method/interface_empty/  empty interface空接口
