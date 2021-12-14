



# 包、变量和函数

关于包：

```go
package main // 每个程序由包构成，从 main 开始
import (
	"math/rand" 
    // 在这个包中，肯定以 package rand 开始
    // 申明包的名字
)
```

关于导出名：

```go
fmt.Println(math.Pi) //一个名字以大写字母开头，那么它就是已导出的
fmt.Println(math.pi) //未导出
```



函数的形式：

```go
func add(x int, y int) int {
	return x + y
}
// 类型相同的参数可以简写
x int, y int
x,y int

// 可以返回任意数量的值
func swap(x, y string) (string, string) {
	return y, x
}

// 直接写 return 会返回函数中定义了的所有变量
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return  
    // 返回 x 和 y, 此处是 7,10
    // 此处返回值会转成 int
}
```



关于变量：

```go
// 声明变量
var c, python, java bool // 默认赋值为 false
var i int // 默认赋值为 0

// 初始值
vari, j int = 1, 2 // 可以设置初始值
var c, python, java = true, false, 'no!' // 不指定类型而直接指定初始值

// 声明变量简单法，用 := 替代 var
c, python, java := true, false, "no!"

```



基本类型：

```go
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
byte // uint8 的别名
rune // int32 的别名
    // 表示一个 Unicode 码点
float32 float64
complex64 complex128

// 使用语法块声明变量
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

// 没有给初始值，默认会赋值, 数值是0， 布尔 false，字符串是空字符
```



类型转换：

```go
// int -> float -> uint
var i int = 42
var f float = float64(i)
var u uint = uint(f)

// 简单的形式
i := 42
f := float64(i)
u := uint(f)
```



类型推导:

```go
// 可以准确推导的情况下，直接使用
var i int
j := i

// 不能准确推导或者因为值运算的结果，需要改变精度
i := 42
j := 3.142
g := 0.867 + 0.5i 
// 注意: g = 0.867 + 0.5i 会报错
```



常量:

```go
const Pi = 3.14
const Pi := 3.14 //syntax error: unexpected :=, expecting =
```



数值常量：

```go
// 将 1 左移 100 位来创建一个非常大的数字
// 即这个数的二进制是 1 后面跟着 100 个 0
Big = 1 << 100

// 再往右移 99 位，即 Small = 1 << 1，或者说 Small = 2
Small = Big >> 99
```



# 流程控制

Go 只有一种循环结构即 for 循环，分成三部分

- 初始化语句：在第一次迭代前执行

- 条件表达式：在每次迭代前求值

- 后置语句：在每次迭代的结尾执行



它有三种写法，for 后面没有小括号， 大括号 `{ }` 则是必须的

- **for init; condition; post{}**
- **for condition {}**
- **for {}**



```go
// for init; condition; post{}
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}

// for condition {}
sum := 1
for ; sum < 1000; {
    sum += sum
}

sum := 1
for sum < 1000 {
    sum += sum
}

// for {} 无限循环
package main
func main() {
	for {
	}
}
```



关于 If 语句：

```go
// 无(), 必须有 {}
if x < 0{
    // do something
}

// 条件表达式前可以执行一个简单的语句
if v := add(x, y); v < 10 {
    return v
}

// else 语法
if v:= add(x, y) {
    // do a
} else {
    // do b
}
// 此处不可以使用 v 了
```



关于 switch:

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
// Go 只运行选定的 case，而非之后所有的 case
// switch 的 case 无需为常量，且取值不必为整数


// 求值顺序从上到下，找到匹配成功
switch i {
	case 0:
	case f(): // f() 不会执行
}

// 没有条件的 switch
func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```



关于 defer:

```go
// defer 会将函数推迟到外层函数返回之后执行
// 推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用
// 下面的代码输出 hello \n world
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}

// 推迟的函数被压入栈，先进后出
// 所以下面的函数会以 987.. 的顺序打印
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```



