# golang 数据类型

## 基本类型
   1.布尔值：bool
   
   2.字符串：string
   
   3.有符号整型：int  int8  int16  int32  int64
   
   4.无符号整型：uint uint8 uint16 uint32 uint64 uintptr
   
   5.byte // uint8 的别名
   
   6.rune // int32 的别名，表示一个 Unicode 码点
   
   7.浮点类型：float32 float64
   
   8.复合类型：complex64 complex128  //如虚数表示   var z complex128 = cmplx.Sqrt(-5 + 12i)

``` 
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

## 变量和类型
   1.变量:var 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。变量可以在包中或函数中声明使用。
   
   2.短变量声明：在函数中，简洁赋值语句 := 可在类型明确的地方代替 var 声明。函数外的每个语句都必须以关键字开始（var, func 等等），因此 := 结构不能在函数外使用。
   
   3.类型推导：在声明一个变量而不指定其类型时（即使用不带类型的 := 语法或 var = 表达式语法），变量的类型由右值推导得出。
   
   4.类型转换：表达式 T(v) 将值v转换为类型 T
   
   5.常量：使用const关键字，常量可以是字符、字符串、布尔值或数值，常量不能用 := 语法声明。
   
