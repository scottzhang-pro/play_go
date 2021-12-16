# 方法

Go 没有类。不过你可以为结构体类型定义方法，Go 中方法是带特殊的 {接收者} 参数的函数（方法其实就是函数，只是带了个接收者）。

```go
package main

import (
    "fmt"
    "math"
)

// 定义了一个结构体
type Vertex struct {
    X, Y float64
}

/*-------------后续代码将省略上面的部分------------------*/

// {接收者} 位于 func 关键字和方法名之间
// 在下面的例子中，Abs 方法拥有一个名为 v，类型为 Vertex 的接收者
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    v := Vertex{3, 4}
    fmt.Println(v.Abs())
}

/*---------------------------------------------------------*/

// 注：方法相当于函数，所以下面的写法功能是一样的
func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
// 但是在使用的时候，不再是 v.Abs(), 而是 Abs(v)
func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}

```

非结构体也是可以声明方法的，比如对于 float：

```go
type MyFloat float64

// {接收者} 为 (f MyFloat)，方法名为 Abs, 返回 float64
// 没有参数
func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

// 使用
f := MyFloat(-1)
f.Abs()
```

但是，你只能为在同一包内定义的类型的接收者声明方法，而不能为其它包内定义的类型（包括 int 之类的内建类型）的接收者声明方法。

上面的接收者为值接收者，实际上接收者还有一种叫做指针接收者，因为使用指针接收者，你可以在方法内部修改值，所以指针接收者比值接收者更常用。

```go
// 值接收者 (v Vertex)
func (v Vertex) Abs() float64 {
    // 取值、计算
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

// 指针接收者 (v *Vertex)
func (v *Vertex) Scale(f float64) {
    // 修改值
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)

// 分析输出
// 如果是指针接收者:
// Scale: 3*10 = 30; 4*10 = 40
// Abs: Sqrt(30*30 + 40*40) = 50

// 如果将指针接收者改为值接收者
// 即 (v *Vertex) -> (v Vertex)
// Scale: 3 ; 4
// Abs: Sqrt(30*30 + 40*40) = 50

```

# 接口


# 并发
