# Golang 并发

## Goroutine
Goroutine是由Go runtime管理的轻量级线程，go f(x,y) 会启动一个新的 Go 程并执行f(x,y)。需要注意的是参数计算发生在当前线程，f执行发生在新的线程。


