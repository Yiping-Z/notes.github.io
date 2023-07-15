---
layout: 
title: Go 基础语法
description: 
image: 
hide:
    -toc
---

在Golang中，import导入的是目录，而不是包名。

在Golang的引用中，填写的是源文件所在的相对路径。

在代码中引用包内的成员时，使用包名而不是目录名。

项目在引用包的时候，会首先在GOROOT/src中寻找，例如fmt输入输出包，当无法找到的时候，会寻找PROJECT GOPATH/src,如果还是无法找到，将会寻找GLOBAL GOPATH/src。

```go
// 初始化
var c, d float64
e, f := 9, 10

// 匿名变量
_, v, _ := getData()
func getData() (int, int, int) {
  // 返回3个值
  return 2, 4, 8
}

// 常量
const, no :=

// if
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
	    return v
	}
	return lim
}

// for
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

// 对于相同类型的两个参数，参数类型可以只写一个
func add(x, y int) int {
	return x + y
}

// Golang中的函数可以返回任意多个返回值
func swap(x, y string) (string, string) {
	return y, x
}

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

// defer 语句会将函数推迟到外层函数返回之后执行, defer后面必须是函数调用语句
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}

// 数组
var a [10]int

// 包括第一个元素，但排除最后一个元素
a[low : high]

// 切片
a := make([]int, 5) // len(a) = 5
b := make([]int, 0, 5) // len = 0, cap = 5
```
首字母小写：包内使用， private

首字母大写： 可以在包外被引入使用， public

指针：用&取地址，用*取地址中的值

Golang中的切片，不是拷贝，而是定义了新的指针，指向了原来数组所在的内存空间。修改了切片数组的值，也就相应的修改了原数组的值。
len （定义的长度）, cap (内存的长度)

```go
goroutine
go a()
go b()
go c()
```

通道的要点：

1.类似unix中管道（pipe），先进先出；

2.线程安全，多个goroutine同时访问，不需要加锁；

3.channel是有类型的，一个整数的channel只能存放整数。

消息的传递和获取必须成对出现，传数据用channel <- data，取数据用<- channel

使用range关键字，使用在channel上时，会自动等待channel的动作一直到channel被关闭。

select中会有case代码块，用于发送或接收数据。

用在有多个信道的情况

```go
var ch0 chan int
var ch1 chan string
var ch2 chan map[string]string

type stu struct{}

var ch3 chan stu
var ch4 chan *stu

package main

import (
  "fmt"
  "time"
)

var ch chan int

func a() {
  time.Sleep(3 * time.Second)
  a := 5
  ch <- a
  fmt.Println("out of a")
}
func b() {
  time.Sleep(1 * time.Second)
  fromA := <- ch
  b := fromA + 3
  fmt.Println("b is ", b)
}
func main() {
  ch = make(chan int, 1)
  go a()
  go b()
  time.Sleep(20 * time.Second)
  fmt.Println("out of main")
}

chSync := make(chan int) //无缓冲
chAsyn := make(chan int,1) //有缓冲
```

无缓冲场景：一直要等有别的协程通过<-chSync接手了这个参数，那么chSync<-1才会继续下去，要不然就一直阻塞着。

有缓冲场景：chAsyn<-1则不会阻塞，因为缓冲大小是1，只有当放第二个值的时候，第一个还没被人拿走，这时候才会阻塞。

```go
type Shape interface {
    Area() float64
    Perimeter() float64
}

type Rect struct {
    height float64
    weight float64
}

func (p *Rect) Area() float64 {
    return p.height * p.height
}

func (p *Rect) Perimeter() float64 {
    return 2* (p.height + p.height)
}

func main() {
    var s Shape = &Rect{height:10, weight:8}
    fmt.Println(s.Area())
    fmt.Println(s.Perimeter())
}

// go convey
package main

import (
 "testing"

 "github.com/smartystreets/goconvey/convey"
)

func TestRect(t *testing.T) {
 convey.Convey("TestRect", t, func() {
  var s Shape = &Rect{height: 10, weight: 8}
  convey.So(s.Area(), convey.ShouldEqual, 80)
  convey.So(s.Perimeter(), convey.ShouldEqual, 30)
 })
}

// http gin框架
package main

import (
   "github.com/gin-gonic/gin"
   "log"
   "net/http"
)

func SayHello(c *gin.Context) {
   c.String(http.StatusOK, "hello") //以字符串"hello"作为返回包
}

func main() {
   engine := gin.Default() //生成一个默认的gin引擎
   engine.Handle(http.MethodGet,"/say_hello", SayHello) 
   err := engine.Run(":8080") //使用8080端口号，开启一个web服务
   if err != nil {
      log.Print("engine run err: ", err.Error())
      return
   }
}

// 结构体
type 结构体名 struct {
    字段1 类型
    字段2 类型
}

// 第一种： 返回该实例的结构类型
var s T
s.a = 1
s.b = 2

// 第二种：返回指针
ming := new(people)
ming.name = "xiao ming"
ming.age = 18

// 第三种：返回指针
ming := &people{"xiao ming", 18}

// 第二种，第三种声明形式
(*ming).name = "xiao wang"

// 方法
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

只有指针接收者类型的方法，才能修改这个接收器的成员值，非指针接收者，方法修改的只是这个传入的指针接收者的一个拷贝。

```go
// slice
var arr = [...]int{1,2,3,4}
fmt.Println(arr) //[1,2,3,4]
slice := arr[:]
fmt.Println(slice) //[1,2,3,4]
slice = append(slice,[]int{5,6,7}...) //此时slice的引用地址已经发生改变了,它引用的底层数组再也不是arr了,而是一个新的数组newarr[1,2,3,4,5,6,7]
```