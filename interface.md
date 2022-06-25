# 接口和方法

## 方法

### 方法的基本概念
1. Golang中没有类。不过你可以为结构体类型定义方法，通过这些方法的组合来实现复杂的对象行为。

方法就是一类带**特殊的接收者作为参数**的函数(接收者就是该函数的拥有者）。方法接收者在它自己的参数列表内，位于 func 关键字和方法名之间。

简而言之，方法只是个带接收者参数的函数（注意只能有一个接收者）。
```
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

2. 也可以为其它非结构体类型建立方法，但是接收者的类型定义和方法声明必须在**同一包内**；**且不能为内建类型（如int）声明方法**,但可以使用type对内建类型重命名来解决。
```
type MyInt int

func (f MyInt) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyInt(3)
	fmt.Println(f.Abs())
}
```

### 指针接收者
可以为指针接收者声明方法，意味着对于某类型 T，接收者的类型可以用 *T 的文法（但是T不能是像 *int 这样的指针）

指针接收者的方法可以修改**接收者指向的值**。若使用值接收者，那么方法只会对**接受者的副本进行操作**。由于方法经常需要修改它的接收者，指针接收者比值接收者更常用。
```
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
  (&v).Scale(10)  //会改变v指向的值
  v.Scale(10)  //也可以使用值调用指针作为接收者的方法
}
```
### 方法和指针的重定向
1. 带指针参数的函数必须接受一个指针。而以指针为接收者的方法被调用时，接收者既能为值又能为指针，**操作的均是指针指向的内存**

2. 接受一个值作为参数的函数必须接受一个指定类型的值。而以值为接收者的方法被调用时，接收者既能为值又能为指针，**但是操作的均是副本，有复制开销**
```
var v Vertex
fmt.Println(v.Abs()) // OK ,操作副本
p := &v
fmt.Println(p.Abs()) // OK，操作副本

```

## 接口
1.接口类型

接口类型 是由一组方法签名定义的集合。接口类型的变量可以保存任何实现了这些方法的值。

接口隐式实现是指类型通过实现一个接口的所有方法来实现该接口，无需专门显式声明，这样接口的实现可以出现在任何包中

```
type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}
  
	a = f  // 因为MyFloat 隐式实现了 Abser
  fmt.Println(a.Abs())
	a = &v // 因为*Vertex 隐式实现了 Abser
	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```
2.接口值

2.1 接口也是值。它们可以像其它值一样传递。接口值可以用作函数的参数或返回值。在内部，接口值可以看做包含**值和具体类型的元组**。接口值调用方法时会执行其**底层类型的同名方法**，有点像多态。

2.2 即便接口内的**具体值为 nil**（简单来说就是没有初始化的引用），方法仍然会被 nil 接收者调用，但方法中使用具体值会产生报错。

2.3 **nil 接口值**(只定义的接口变量，未指向任何实现接口的类型)既不保存值也不保存具体类型，为 nil 接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个 具体 方法的类型

2.4 **空接口**：指定了零个方法的接口值。空接口被用来处理未知类型的值

```
type I interface {
	M()
}
type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i interface{} //空接口
	describe(i)

	i = 42
	describe(i)   // output:42 int

	i = "hello"
	describe(i)  // output:hello string
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}

```

## 类型断言



