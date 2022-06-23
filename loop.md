# Go Loop

## for循环
Go语言中只有一种循环结构(for 循环），基本的 for 循环由三部分组成，它们用分号隔开：

  初始化语句：在第一次迭代前执行(可选）
  
  条件表达式：在每次迭代前求值（可选，但循环中需要有退出循环的语句）
  
  后置语句：在每次迭代的结尾执行（可选）
  
初始化语句通常为一句短变量声明，该变量声明仅在 for 语句的作用域中可见。一旦条件表达式的布尔值为 false，循环迭代就会终止。

```
for i := 0; i < 10; i++ {
		sum += i
	}
```
值得注意的是，for循环后面没有小括号， 但大括号 { } 是必须的。

## if语句
1. 与for循环类似，表达式外无需小括号 ( )，而大括号 { } 则是必须的。
```
if x < 0 {
		return sqrt(-x) + "i"
}
```
2. if 语句可以在条件表达式前执行一个简单的语句。该语句声明的变量作用域仅在 if 之内。
```
if v := math.Pow(x, n); v < lim {
		return v
	}
```
3. if 和 else 

需要注意的是，if 的简短语句中声明的变量同样可以在任何对应的 else 块中使用
```
if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
```

## switch
switch 是编写一连串 if - else 语句的简便方法。它运行第一个值等于条件表达式的 case 语句。不过Go只运行选定的 case，而非之后所有的 case。 实际上，Go 自动提供了在这些语言中每个 case 后面所需的 break 语句。Go 的另一点重要的不同在于 switch 的 case 无需为常量，且取值不必为整数。
```
switch os := runtime.GOOS; os {
      case "darwin":
	   fmt.Println("OS X.")
      case "linux":
	   fmt.Println("Linux.")
      default:
	   fmt.Printf("%s.\n", os)
  }
```

## Tips和注意点
   1.当省略初始化语句和后置语句时，for循环可以退化为C中while循环
   ```
   sum := 1
	 for sum < 1000 {
		  sum += sum
	 }
   ```
   2.值得注意的是，for循环后面没有小括号， 但大括号 { } 是必须的。
   
   3.无限循环
   ```
   for {
	 }
   ```


