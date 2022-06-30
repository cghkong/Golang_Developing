# Golang 并发

## Goroutine
Goroutine是由Go runtime管理的轻量级线程，go f(x,y) 会启动一个新的 Go 程并执行f(x,y)。需要注意的是参数计算发生在当前线程，f执行发生在新的线程。

## 信道
1. 信道：带有类型的管道，可以通过它用信道操作符 <- 来发送或者接收值。
2. 默认情况下，发送和接收操作在另一端准备好之前都会阻塞。这使得 Go 程可以在没有显式的锁或竞态变量的情况下进行同步。
```
channel := make(chan int)   // 创建信道
channel <- v    // 将 v 发送至信道 ch。
v := <-channel  // 从 ch 接收值并赋予 v。

// example:将数组分到两个不同的线程中计算，最后合并
s := []int{7, 2, 8, -9, 4, 0}
c := make(chan int)
go sum(s[:len(s)/2], c)
go sum(s[len(s)/2:], c)
x, y := <-c, <-c   // 从 c 中接收

fmt.Println(x, y, x+y)
```

3. 信道可以是带缓冲的。将缓冲长度作为第二个参数提供给 make 来初始化一个带缓冲的信道。仅当信道的缓冲区填满后，向其发送数据时才会阻塞。而当缓冲区为空时，接受方会阻塞。
```
ch := make(chan int, 100)  //创建一个缓存为100的信道
```
4. 关闭信道：**发送者(只有发送者可以关闭)** 可通过 close 关闭一个信道来表示没有需要发送的值了。接收者可以通过为接收表达式分配第二个参数来测试信道是否被关闭。
而循环 for i := range c 会不断从信道接收值，直到它被关闭。

**注意：向一个已经关闭的信道传输数据会引起panic
```
v, ok := <-ch    // ok==false表示信道已经关闭

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```

## select语句
select语句使一个 Go 程可以等待多个通信操作。select 会阻塞到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。
```
for {
		select {
		case <-tick:
			fmt.Println("tick.")
		case <-boom:
			fmt.Println("BOOM!")
			return
		default:                // 当其它case都没有准备好时，默认行为
			fmt.Println("    .")
			time.Sleep(50 * time.Millisecond)
		}
	}
```
## sync.Mutex(互斥锁)
sync.Mutex是Go语言提供的互斥锁，拥有Lock()和Unlock()两个方法，保证在Lock()和Unlock()之间的代码互斥执行，也可以在Unlock前面加上defer语句保证Unlock一定会被执行

