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
