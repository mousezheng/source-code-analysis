B站[**go语言速成视频**](https://www.bilibili.com/video/BV1gf4y1r79E?t=131) 有后端基础速成笔记

# Go 语言特点

Go 为什么叫 "Golang"呢，有这么几种说法，
- "go" 这个词汇太常见了不易搜索
- go language 的简称
- go 官方是 golang.org

## Golang 的优势

- 极简单的部署
  - 可直接编译成机器码
  - 不依赖其他库（lbb）
  - 直接 build 即可部署
- 静态类型语言
  - 编译时检查
- 语言层面并发
  - 原生支持并发
  - 充分利用多核
- 强大标准库
  - runtime 系统调度机制
  - 高效的 GC 垃圾回收
  - 丰富的标准库
- 简单易学
  - 25 个关键字
  - 类C，内嵌C 语法 支持
  - 面向对象特征
  - 跨平台
- 大厂领军
  - google facebook
  - 腾讯 百度 京东 小米 七牛 阿里...

## Golang 的强项

1. 云计算基础设施
   - **docker**
   - **k8s**
   - etcd
   - consul
   - cloudflare CDN
   - 七牛云存储
2. 基础后端软件
   - tidb
   - influxdb
   - cockroachdb
3. 微服务
   - go-kit
   - micro
   - monzo bank 的 typhon
   - bilibili
4. 互联网基础设置
   - 以太坊
   - hyperledger

## Golang 缺点
- 包管理，大部分包都在 github 个人账户上
- 无泛化类型，2.0 计划加（传言）
- 所有 exception 都是用 Error 来处理（有争议）
- 对 C 的降级处理，并非无缝，没有 C 降级到 asm 那么完美（序列化问题）

# 知识点

## hello
```go
package main	// 包名，main 通常作为入口包

import "fmt"	// 导入 fmt 标准库

func main(){	// main() 方法通常用做入口方法
	fmt.Print("你好，Golang")	// 调用 fmt 方法打印信息
}
```
### 分号？
 可以不加分号，用换行作为一行代码的结束（加上也 ok）

### import
导入多个包时，可以有以下三种方式
```go
import "fmt"	// 导入 fmt 标准库
import "time"	// 导入 time 标准库
```
```go
import ("fmt" ; "time")
```
```go
// 推荐使用这种方式
import (
	"fmt" 
 	"time"
 )
```
### 函数体
关于函数体开始的方括号"{"，必须与函数在同一行，如果像如下方式编写

```go
package main	// 包名，main 通常作为入口包

import (
	"fmt" 
 	"time"
 )	// 入 fmt 标准库

func main()  
{	// main() 方法通常用做入口方法
	time.Sleep(1000)
	fmt.Print("你好，Golang")	// 调用 fmt 方法打印信息
}
```
```shell
go run 时，报错信息

# command-line-arguments
./hello.go:8:6: missing function body
./hello.go:9:1: syntax error: unexpected semicolon or newline before {
```

## 变量

### go 支持的变量
#### 内置基础类型
布尔型 : bool
整形 : int , int8(byte) , int16 , int32 , int64,uint , uint8 , uint16 , uint32 , uint64
浮点型 : float , float32 , float64
复数 : complex64 , complex128
字符 : char
字符串 : string
字符类型 :rune
错误类型 : error

#### 复合类型
指针（pointer）
数组（array）
切片（slice）
字典（map）
通道（chan)
结构体（struct)
接口（interface）

### 单个变量
#### 局部变量四种方式
```go
func main(){	
	// 先声明，后使用，声明时必须指定类型，使用时不允许修改
	var a int
	fmt.Printf("a = %2d 类型: %T\n", a, a)// a =  0 类型: int
	a = 10;
	fmt.Printf("a = %2d 类型: %T\n", a, a)// a = 10 类型: int
	
	// 声明类型并赋值
	var b float64 = 3.14
	fmt.Printf("b = %2.8f 类型: %T\n", b, b)// b = 3.14000000 类型: float64
	b = 3.1415926;
	fmt.Printf("b = %2.8f 类型: %T\n", b, b)// b = 3.14159260 类型: float64
	// 浮点数默认小数点只打六位
	fmt.Printf("b = %f 类型: %T\n", b, b)// b = 3.141593 类型: float64

	// 直接赋值，类型自动匹配
	var c = 'a'// go 的字符型类似 C int char 互转
	fmt.Printf("c = %c 类型: %T\n", c, c)// c = a 类型: int32
	c = 't';
	fmt.Printf("c = %c 类型: %T\n", c, c)// c = t 类型: int32

	// 简化直接赋值，类型自动匹配
	d := "hello"
	fmt.Printf("d = %s 类型: %T\n", d, d)// d = hello 类型: string
	d = "你好";
	fmt.Printf("d = %s 类型: %T\n", d, d)// d = 你好 类型: string
}
```
#### 全局变量三中声明方式

d := XX 必须在函数体内才能使用，否则报错【syntax error: non-declaration statement outside function body】 
```go
var gA = "hello"
var gB int = 10
var gC float64
// gD := 10 // syntax error: non-declaration statement outside function body

func main(){	
	fmt.Printf("gA = %s 类型: %T\n", gA, gA)// gA = hello 类型: string
	fmt.Printf("gB = %d 类型: %T\n", gB, gB)// gB = 10 类型: int
	gC = 3.14
	fmt.Printf("gC = %f 类型: %T\n", gC, gC)// gC = 3.140000 类型: float64
}
```

### 多变量声明
```go
func main(){	
	a, b, c := 1,3.14,"cc"

	fmt.Printf("a = %d 类型: %T\n", a, a)// a = 1 类型: int
	fmt.Printf("b = %f 类型: %T\n", b, b)// b = 3.140000 类型: float64
	fmt.Printf("c = %s 类型: %T\n", c, c)// c = cc 类型: string

	var(
		d int = 1
		e string = "ee"
	)
	fmt.Printf("d = %d 类型: %T\n", d, d)// d = 1 类型: int
	fmt.Printf("e = %s 类型: %T\n", e, e)// e = ee 类型: string
} 
```

## const 与 iota （常量/枚举）

```go
const pai = 3.14 

const(
	a = iota //初始值为 0
	b
)

const(
	c = iota + 1	// 0 + 1 = 1
	d 				// 1 + 1 = 2
	e , f = iota * 2 , iota * 3 // 2*2=4 2*3 = 6 

	g, h	// 3*2=6 3*3=9 
)

func main(){	
	// pai = 3   //cannot assign to pai
	fmt.Printf("pai = %f 类型: %T\n", pai, pai)// pai = 3.140000 类型: float64

	fmt.Printf("a = %d 类型: %T\n", a, a)// a = 0 类型: int
	fmt.Printf("b = %d 类型: %T\n", b, b)// b = 1 类型: int
	fmt.Printf("c = %d 类型: %T\n", c, c)// c = 10 类型: int
	fmt.Printf("d = %d 类型: %T\n", d, d)// d = dd 类型: string
	fmt.Printf("e = %d 类型: %T\n", e, e)// e = 4 类型: int
	fmt.Printf("f = %d 类型: %T\n", f, f)// f = 6 类型: int
	fmt.Printf("g = %d 类型: %T\n", g, g)// g = 6 类型: int
	fmt.Printf("h = %d 类型: %T\n", h, h)// h = 9 类型: int
} 
```

## 函数

### 简单Demo
```go
// a , b 两个整形的形参，返回 int
func max(a int, b int) int {
	if a > b {
		return a
	} else {
		return b
	}
}

func main() {
	fmt.Println(max(3, 5))	// 5
}
```

### 函数多返回值
```go
// a , b 两个整形的形参，返回两个值
func swap1(a int, b int) (int, int) {
	return b, a
}

// a , b 两个整形的形参，返回两个值
func swap2(a int, b int) (c int, d int) {
	fmt.Println(c) //0
	fmt.Println(d) //0
	c = b
	d = a
	return
}
func getThree() (string, string, string) {
	return "魏", "蜀", "吴"
}
func main() {
	a := 10
	b := 5
	a, b = swap1(a, b)
	fmt.Println(a) //5
	fmt.Println(b) //10
	a, b = swap2(a, b)
	fmt.Println(a) //10
	fmt.Println(b) //5

	c, d, _ := getThree()
	fmt.Println(c) //魏
	fmt.Println(d) //蜀
}
```

## import 与 init()

### import 过程以及 init() 执行情况
```sequence
main->pkg2: import pkg2
pkg2->pkg3: import pkg3
Note right of pkg3: no import
Note right of pkg3: cost
Note right of pkg3: var
Note right of pkg3: init() 
pkg3->pkg2: pkg3 加载完毕
Note right of pkg2: cost
Note right of pkg2: var
Note right of pkg2: init()
pkg2->main: pkg2 加载完毕
Note right of main: cost
Note right of main: var
Note right of main: init()
Note right of main: main()
Note left of main: exit
```

### 参考代码
相关注意事项
- 代码使用了 go mod，关于 goroot 的配置使用，略有繁琐
- 同目录下 package 需要一样否则会报错 【found packages pkg2 (say.go) and pkg1 (say2.go) in /XXX/pkg1】
- 函数首字母大写代表公有，可供外部调用，小写代表私有
- import 的包必须都使用否则会报错【imported and not used: "fmt"】

打印结果及参考代码如下
```txt
init pkg2
init pkg1
我是  pkg1
我要调用 pkg2 
我是  pkg2
```

```go
// src/main.go
package main

import (
	"hello/pkg1"
)

func main() {
	pkg1.Say()
}
```
```go
// src/pkg1/say.go
package pkg1

import (
	"fmt"
	"hello/pkg2"
)

const name = "pkg1"

func init() {
	fmt.Printf("init %s\n", name)
}

func Say() {
	fmt.Println("我是 ", name)
	fmt.Println("我要调用 pkg2 ")
	pkg2.Say()
}
```
```go
// src/pkg2/say.go
package pkg2

import "fmt"

const name = "pkg2"

func init() {
	fmt.Printf("init %s\n", name)
}

func Say() {
	fmt.Println("我是 ", name)
}
```

### import 匿名导入

上文提到，导入的包必须都使用，实际业务中可能需要导入但不使用，可以使用"匿名导入"，在包前面加个"_"下划线即可，**只是为了执行 init()**

```go
import (
	_ "fmt"
)

func main() {
	
}
```

### import 别名导入

import 新的包可能出现重名的情况，有时候需要做别名。在包前写别名即可，参考代码如下(默认别名是报名，的目录名)
~~别名可以是 . 如【 import . "fmt" 】表示把 fmt 包中的方法复制到当前包中~~


```go
import (
	fmt2 "fmt"
)

func main() {
	fmt2.Println("aaa")
}
```

## defer 

### demo
defer 在函数执行完后执行，defer 执行顺序是类似**栈**的方式
```go
func main() {
	a := 1;
	b := 1;
	var c = a / b;
	defer fmt.Println("defer1")
	defer fmt.Println("defer2")
	fmt.Println(c)
}
// fmt.Println(c) 1
// defer2
// defer1
```
### defer 与 return 

defer 会在 return 后执行相关代码


```go
func main() {
	testDefer()
}

func show(s string) int {
	fmt.Println(s)
	return 0
}
func testDefer() int {
	defer show("defer")
	return show("return")
}

// return
// defer
```

## 切片（slice）

### 数组
- 长度固定，声明时需确定
- 形参值传递
```go
func main() {
	// 固定长度数组声明，默认 0
	var arr1 [3]int
	fmt.Printf("\narr1 %T\t", arr1)
	for i := 0; i < len(arr1); i++ {
		fmt.Printf("%d\t", arr1[i])// 0, 0, 0
	}
	// 固定长度数组部分初始化，会做 0 补全
	var arr3 = [4]int{1, 2}
	fmt.Printf("\narr1 %T\t", arr3)
	for _, v := range arr3 {
		fmt.Printf("%d\t", v)// 1, 2, 0, 0
	}
	changeArr(arr3)// 值传递，未修改 arr3
	for _, v := range arr3 {
		fmt.Printf("%d\t", v)// 1, 2, 0, 0
	}
}
// 值传递，未修改 arr3
func changeArr(arr [4]int){
	arr[0] = 111;
}
```

### 动态数组 slice 
存储原理
- 切片使用一块连续内存，传递时通过引用传参即 指针传参
- 每个切片都有自己的头指针共享同一块内存
- 切片属于大切片的一部分，根据 头指针 和 length 确定（也可能是尾指针，length 性能好）
- cap 作为扩容常熟，通常扩容当前一倍（子切片的 cao 很讲究）

#### 声明
可以通过以下三种方式声明
- ``` slice := []int{1, 2, 3} ``` slice 为 1,2,3
- ``` slice := make([]int, 3) ``` slice 为 0,0,0
- ``` var slice []int ``` slice 为 [] slice = nil

```go
func main() {
	// 固定数组自动赋值
	slice1 := []int{1, 2, 3}
	fmt.Printf("slice1 %T\t", slice1)// []int
	fmt.Println(slice1)// [1 2 3]
	changeslice(slice1) // []int 引用传参，修改成功
	fmt.Printf("slice1 %T\t", slice1)// []int
	fmt.Println(slice1)// [111 2 3]

	slice2 := make([]int, 3)
	fmt.Printf("slice2 %T\t", slice2)// []int
	fmt.Println(slice2)// [0 0 0]

	slice1 = make([]int, 4)// 覆盖之前元素
	fmt.Printf("slice1 %T\t", slice1)// []int
	fmt.Println(slice1)// [0 0 0 0]
	

	var slice3 []int
	fmt.Printf("slice3 %T\t", slice3)// []int
	fmt.Println(slice3)// []
	fmt.Println(slice3 == nil)// true

	fmt.Println()
}

func changeslice(slice []int) {
	slice[0] = 111
}
```
#### 扩容机制
- 通过 append 追加
- 扩容规律，按当前的一倍扩容
```go
func main() {
	slice := []int{1, 2}
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 2 2 [1 2]
	slice = append(slice, 3)
	slice = append(slice, 4)
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 4 4 [1 2 3 4]
	slice = append(slice, 5)
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 5 8 [1 2 3 4 5]


	slice = make([]int, 3)
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 3 3 [0 0 0]
	slice = append(slice, 3)
	slice = append(slice, 4)
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 5 6 [0 0 0 3 4]
	slice = append(slice, 5)
	slice = append(slice, 6)
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 7 12 [0 0 0 3 4 5 6]
}
```

#### 切片的使用 
- 类似python 通过 ":"取区间，如 [:3] 从 0 -3，[3:] 从 3-最后
- 注意下面例子 **cap 很智能呀**，可见 go 设计者很考究性能

```go
func main() {
	sliceInit := []int{1, 2, 3, 4, 5}
	slice := sliceInit[1:4]
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 3 4 [2 3 4]
	slice = sliceInit[2:4]
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 2 3 [3 4]
	slice = sliceInit[3:]
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 2 2 [4 5]
	slice = sliceInit[:3]
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 3 5 [1 2 3]
	slice = sliceInit[:1]
	fmt.Printf("%d %d %v\n", len(slice), cap(slice), slice) // 1 5 [1]
}
```

### map

#### 创建方式
主要有两种，
- 通过 make
- 通过 map 给初始值的方式

```go
func main() {
	var map1 map[string]string
	fmt.Println(map1)        // map[]
	fmt.Println(map1 == nil) // true
	map1 = make(map[string]string, 1)// 自动扩容
	map1["a"] = "aa"
	map1["c"] = "cc"
	map1["b"] = "bb"
	for k, v := range map1 {
		fmt.Printf("%s %s\t", k, v)// a aa    c cc    b bb   e cc
	}
	fmt.Println() 

	map2 := map[string]string{//创建时给默认值
		"a":"aa",
		"b":"bb",
	}
	fmt.Println(map2)// map[a:aa b:bb]
	map2["c"] = "cc"
	fmt.Println(map2)// map[a:aa b:bb c:cc]
}
```

#### 操作方式
增删改查分别是
- ``` map1["a"] = "aa" ```
- ``` dedelete(map1, "a") lete```
- ``` map1["a"] = "bb" ```
- ``` map1["a"] ```

## 面向对象

### 封装

- 传值问题，通过引用传递/通过拷贝传递
- 包导入的时候通过首字母大小写确保可用性

```go
type Student struct {
	id   int
	name string
}

func (this Student) getName() string {
	return this.name
}

// set 时不使用指针为值拷贝，无法正常赋值
func (this Student) setName(name string) Student {
	this.name = name
	return this
}

// 使用指针时可作为引用拷贝，能够正常复制
func (this *Student) setName2(name string) *Student {
	this.name = name
	return this
}

func main() {
	stu := Student{
		id:   1,
		name: "aa",
	}
	fmt.Println(stu) // {1 aa}
	stu.setName("bb")
	fmt.Println(stu) // {1 aa}
	stu = stu.setName("bb")
	fmt.Println(stu) // {1 bb}
	stu.setName2("bb")
	fmt.Println(stu) // {1 bb}
}
```
### 继承
不得不说，go 的继承与封装是有点生硬，但语法特性还算可以接受。主要通过结构体重声明成员变量为另一个结构体即可，这种情况是可能出现多继承的。

```go
type Tv struct {// 基类 tv
	id int
}

func (this Tv) watch() {
	fmt.Println("Tv")
}

type HaierTv struct {// 子类 haierTv 继承 tv 
	Tv
}

func (this HaierTv) watch() {
	fmt.Println("HaierTv")
}

func main() {
	tv1 := Tv{
		id: 1,
	}
	tv1.watch()// Tv
	fmt.Println(tv1)// {1}
	tv2 := HaierTv{
		Tv: tv1,
	}
	tv2.watch()// HaierTv
	fmt.Println(tv2)// {{1}}
}
```

### 多态
但看多态，go 的方案还是很不错的，无需显示的声明，只需要实现接口中所有的方法即可实现自动转换
```go
type Tv interface {
	watch()
}

type HaierTv struct {
	id   int
	name string
}

func (this HaierTv) watch() {
	fmt.Println("HaierTv")
}

func main() {
	var haier Tv//声明为接口类型
	haier = HaierTv{id: 1, name: "HaierTv"}// 创建 HaierTV 类型
	haier.watch()
	fmt.Println(haier)
}
```
