## 数组、切片和映射

### 数组

数组分配的内存是连续的，由于内存连续，CPU 能把正在使用的数据缓存更久，而且内存连续很容易计算索引，可以快速迭代数组里的所有元素。数组内的每个元素类型相同且连续分配，就可以以固定速度索引数组中的任意数据，速度非常快。

### 切片

切片有三个字段：

- 指针：指向当前切片的地址
- 长度：当前切片内的可访问元素
- 容量：当前切片允许增长到的元素个数

切片可以动态增长和缩小，切片的底层内存也是在连续块中分配的，所以切片也可以获得索引、迭代以及垃圾回收优化的好处。

**计算切片的长度和容量**

一个底层数组容量是 k 的切片 `slice[i:j]`

长度：j - i

容量：k - i

### 映射

映射是无序的，通过散列函数计算出散列值，将键值存放在不同的桶中。

```go
// 空映射
colors := map[string]string{}

// nil 映射
var colors map[string]string
colors["red"] = "#da1337" // 对 nil 映射赋值会报运行时错误
```

## go 语言类型系统

```go
type duration int64
var dur duration
dur := int64(1000) // 会抛出一个错误
```

上面的代码，虽然 duration 类型的基础类型是 int64，但是 go 编译器认为这是两个不同的类型，即便两种类型的值互相兼容，也不能相互赋值。编译器不会对不同类型的值做隐式转换。

# 并发

Go语言并发指的是能让某个函数独立于其他函数运行的能力。

Go语言运行时的调度器是一个复杂的软件，能管理被创建的所有goroutine并为其分配执行时间。调度器在任何给定的时间，都会全面控制哪个goroutine要在哪个逻辑处理器上运行。

Go语言的并发同步模型来自一个叫作通信顺序进程（Communicating  Sequential Process,CSP）的范型。CSP是一种消息传递模型，通过在goroutine之间传递数据来传递消息，而不是对数据加锁来实现同步访问。

### 竞争状态

如果两个或者多个goroutine在没有互相同步的情况下，访问某个共享资源，并试图同时读和写这个资源，就处于相互竞争的状态，这种情况被称为竞争状态（race condition）。

对于一个共享资源的读写操作必须是原子化的，就是同一时刻只能有一个 goroutine 对共享资源进行读写操作。

