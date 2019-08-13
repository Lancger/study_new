```
简单介绍
map数据类型在很多语言中都有，是一个key，value形式的hash表，从而将key，value进行一一映射，进行快速查找、添加、删除等操作。在Go语言中也不例外，提供了map数据结构类型。
内建map切忌开箱即用
Golang中，map是引用类型，如指针切片一样，通过下面的代码声明后指向的是nil。这点在golang官方文档中也说明了，所以千万别直接声明后就使用，开始可能经常会犯下面的错：
var m map[string]string
m["result"] = "result"

上面的第一行代码并没有对map进行一个初始化，而却对其进行写入操作，就是对空指针的引用，这将会造成一个painc。所以，得记得用make函数对其进行分配内存和初始化：
m := make(map[string]string)
m["result"] = "result"

golang中的map并不是并发安全的
经常使用map，平时用着也很爽，但是突然某天流量上来了，程序不知不觉就挂了，还不清楚是怎么回事，明明以前用着好好的呀。所以有些好习惯在刚开始就养成，比如断言检查，并发安全考虑等。
map纵然很好用，但也得谨慎。或许很多人还不知道，golang内建map其实并不是并发安全的，下面我自定义了一个结构体，赋其map属性，给其绑定Get，Set方法方便操作。请看下面代码：
// M
type M struct {
    Map    map[string]string
}

// Set ...
func (m *M) Set(key, value string) {
    m.Map[key] = value
}

// Get ...
func (m *M) Get(key string) string {
    return m.Map[key]
}


上面的代码中，给一个结构体赋予了一个map属性，且绑定了两个方法，进行读写操作。当你在写了一个test在单个goroutine中跑的时候或许没什么问题，甚至10个goroutine，20个都还正常。
// TestMap  ...
func TestMap(t *testing.T) {
    c := helper.M{Map: make(map[string]string)}
    wg := sync.WaitGroup{}
    for i := 0; i < 21; i++ {
        wg.Add(1)
        go func(n int) {
            k, v := strconv.Itoa(n), strconv.Itoa(n)
            c.Set(k, v)
            t.Logf("k=:%v,v:=%v\n", k, c.Get(k))
            wg.Done()
        }(i)
    }
    wg.Wait()
    t.Log("ok finished.")
}

然而当你你再添加的时候，goroutine再增加的时候，会报下面的错,也就是map并发写入出错.
fatal error: concurrent map writes

goroutine 75 [running]:
runtime.throw(0x13b2011, 0x15)

需要说明一点的是，在Http请求时，我们通常对参数封装，encode，可能会调用golang自带的url.Values{}，通过读源码可以发现也是线程不安全的。
如何解决
其实要解决上面的问题也不难，出错原因golang已经写得很清楚了，concurrent map writes,并发写map异常，这个时候肯定想的是并发操作上能不能解决。
很显然，我们可以用锁机制解决上面的问题。我们将上面的map结构改成如下:
// M
type M struct {
    Map    map[string]string
    lock sync.RWMutex // 加锁
}

// Set ...
func (m *M) Set(key, value string) {
    m.lock.Lock()
    defer m.lock.Unlock()
    m.Map[key] = value
}

// Get ...
func (m *M) Get(key string) string {
    return m.Map[key]
}


在上面的代码中，我们引入了锁机制操作，从而保证了map在多个goroutine中的安全。这时再执行我们的test会发现其正常输出。
=== RUN   TestMap
--- PASS: TestMap (0.00s)
    map_test.go:27: k=:5,v:=5
    map_test.go:27: k=:17,v:=17
    map_test.go:27: k=:20,v:=20
    map_test.go:27: k=:19,v:=19
    map_test.go:27: k=:6,v:=6

或许你可以尝试下sync.Map
golang中的sync.Map是并发安全的，其实也就是sync包中golang自定义的一个名叫Map的结构体。结构体原型如下：
type Map struct {
   mu Mutex
   read atomic.Value // readOnly
   dirty map[interface{}]*entry
   misses int
}


可以看见有 Mutex，很显然也是用了锁机制的，从而来保证了并发安全。该包中的Map提供了Store、Load、Delete、Range等操。并且sync包中的Map是开箱可用的，也即是声明后就可以直接使用，如下：
var m sync.Map
m.Store("method", "eth_getBlockByHash")
value, ok := m.Load("method")
t.Logf("value=%v,ok=%v\n",value,ok)

当然也可以用Range遍历
// TestMap  ...
func TestMap(t *testing.T) {
    var m sync.Map
    m.Store("method", "eth_getBlockByHash")
    m.Store("jsonrpc", "2.0")
    //value, ok := m.Load("method")
    //t.Logf("value=%v,ok=%v\n", value, ok)
    //f := func(key, value string) {
    //
    //}
    //f("method", "eth_getBlockByHash")
    m.Range(func(key, value interface{}) bool {
        t.Logf("range k:%v,v=%v\n", key, value)
        return true
    })
}

结果如下:
=== RUN   TestMap
--- PASS: TestMap (0.00s)
    map_test.go:43: range k:jsonrpc,v=2.0
    map_test.go:43: range k:method,v=eth_getBlockByHash
PASS

```
参考文档：

https://www.jianshu.com/p/0c8519f4498e  慎用golang中的map，特别是在并发操作中
