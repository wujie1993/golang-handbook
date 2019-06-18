# 锁

当多个协程间使用到了相同内存空间中的变量时，有可能出现同时对数据进行读写的情况，这时可能会导致数据不准确，打个比方：

用户A和用户B是一对夫妻，两人共同使用着一张银行卡，卡中余额为50000元。碰巧有一天，用户A正将6000元工资存入银行，在同一时刻用户B支出了500元购买衣物，两笔操作同时发生。

按照正常逻辑，要么是先存入后支出，余额为50000+6000-500=55500，要么是先支出后存入50000-500+6000=55500，这两种方式都不会有问题。但在程序执行的过程中，这两笔操作是不具备原子性的，都要对数据先读后写，分两步才能完成。有可能A的操作刚读出数据还未写入时，B的操作完成了读写，之后A的写操作才完成，这样B的操作就被无效了，余额为50000+6000=56000元；也有可能B的操作刚读出数据还未写入时，A的操作完成了读写，之后B的写操作才完成，这样A的操作就被无效了，余额变为50000-500=49500元。我们可以通过以下例子模拟上述的情况。

例子：多协程并发读写导致数据错误

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

// myAccount 表示我的银行账户余额
var myAccount int

// getAccount 模拟读取数据库中我的账户余额
func getAccount() int {
	// 随机休眠0~1秒，模拟从数据库读取数据花费的时间
	time.Sleep(time.Duration((float64)(time.Second) * rand.Float64()))
	
	return myAccount
}

// setAccount 模拟更新数据库中我的账户余额
func setAccount(account int) {
	// 随机休眠0~1秒，模拟将数据写入数据库花费的时间
	time.Sleep(time.Duration((float64)(time.Second) * rand.Float64()))
	
	myAccount = account
}

// trade 模拟对我的账号发起一笔交易，income为正表示收入，为负表示支出
func trade(income int, wg *sync.WaitGroup) {
	// 读取我的账户余额
	account := getAccount()
	// 更新我的账户余额
	setAccount(account + income)
	
	wg.Done()
}

func main() {
	var waitGroup sync.WaitGroup

	for i := 0; i < 10; i++ {
		myAccount = 50000
		// 添加两个计数器
		waitGroup.Add(2)
		// 收入6000元
		go trade(6000, &waitGroup)
		// 支出500元
		go trade(-500, &waitGroup)

		waitGroup.Wait()

		fmt.Println(myAccount)
	}
}

```

{% embed url="https://play.golang.org/p/mxnbwSzU5aM" caption="在线例子：多协程并发读写导致数据错误" %}

以上代码的运行结果：

```text
56000
55500
56000
56000
49500
56000
49500
56000
49500
49500
```

为了解决类似问题，程序中处理的过程中需要引入锁机制，用来保证数据并发读写时的原子性。其原理是锁具有互斥性，同一个锁在一个协程中加锁后在另一个协程中是无法加锁的，会一直阻塞直到锁被解开。因此可以在事务开始前加锁，在事务结束后解锁，保证一个时间段内只能有一个事务在进行。

锁可以分很多种，对于分布式系统可以有分布式锁，对于线程可以有线程锁，对于协程也可以有协程锁。

在golang内置的sync库中实现了互斥锁，我们可以通过Mutex类型解决上方的问题。

例子：通过互斥锁保证多协程操作的原子性

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

// myMutex 用于给我的账户交易时加锁
var myMutex sync.Mutex

// myAccount 表示我的银行账户余额
var myAccount int

// getAccount 模拟读取数据库中我的账户余额
func getAccount() int {
	// 随机休眠0~1秒，模拟从数据库读取数据花费的时间
	time.Sleep(time.Duration((float64)(time.Second) * rand.Float64()))

	return myAccount
}

// setAccount 模拟更新数据库中我的账户余额
func setAccount(account int) {
	// 随机休眠0~1秒，模拟将数据写入数据库花费的时间
	time.Sleep(time.Duration((float64)(time.Second) * rand.Float64()))

	myAccount = account
}

// trade 模拟对我的账号发起一笔交易，income为正表示收入，为负表示支出
func trade(income int, wg *sync.WaitGroup) {
	// 交易开始前加锁
	myMutex.Lock()
	// 读取我的账户余额
	account := getAccount()
	// 更新我的账户余额
	setAccount(account + income)
	// 交易结束后解锁
	myMutex.Unlock()
	wg.Done()
}

func main() {
	var waitGroup sync.WaitGroup

	for i := 0; i < 10; i++ {
		myAccount = 50000
		// 添加两个计数器
		waitGroup.Add(2)
		// 收入6000元
		go trade(6000, &waitGroup)
		// 支出500元
		go trade(-500, &waitGroup)

		waitGroup.Wait()

		fmt.Println(myAccount)
	}
}

```

{% embed url="https://play.golang.org/p/OBophu\_uZDw" caption="在线例子：通过锁保证多协程操作的原子性" %}

以上代码的运行结果：

```text
55500
55500
55500
55500
55500
55500
55500
55500
55500
55500
```

在跨协程间访问同一个资源时，并不是每次都需要加解锁，如果都是只读的情况就没必要加锁，因为数据不会发生变化；而如果读写同时发生或者都是写，那就需要加锁，因为数据的变化有可能在另外的协程中没获取到。

除了互斥锁Mutex外sync包中还提供了读写锁RWMutex类型，对于锁的作用做了进一步的延申。其中包含了两种类型的锁：读锁和写锁。写锁和互斥锁一样，在判断到已经有锁的情况下就无法再锁上；而读锁有所不同，只要不存在写锁，多个读锁可以同时锁上。换而言之，在没有写操作的情况下读操作可以同时发生。通过读写锁可以减少程序因加锁而带来的性能损失。

例子：通过读写锁降低性能损耗

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// ReadOnly 运行多个加了读锁的任务，count为任务数
func ReadOnly(count int) {
	var rwMutex sync.RWMutex
	var waitGroup sync.WaitGroup

	startTime := time.Now()

	waitGroup.Add(count)
	for i := 0; i < count; i++ {
		go func(wg *sync.WaitGroup) {
			rwMutex.RLock()
			defer rwMutex.RUnlock()
			time.Sleep(time.Second)
			wg.Done()
		}(&waitGroup)
	}
	waitGroup.Wait()

	fmt.Printf("只读耗时：%s\n", time.Now().Sub(startTime))
}

// ReadWrite 运行多个既有读锁又有写锁的任务，count为任务数
func ReadWrite(count int) {
	var rwMutex sync.RWMutex
	var waitGroup sync.WaitGroup

	startTime := time.Now()

	waitGroup.Add(count)
	for i := 0; i < count/2; i++ {
		go func(wg *sync.WaitGroup) {
			rwMutex.RLock()
			defer rwMutex.RUnlock()
			time.Sleep(time.Second)
			wg.Done()
		}(&waitGroup)
		go func(wg *sync.WaitGroup) {
			rwMutex.Lock()
			defer rwMutex.Unlock()
			time.Sleep(time.Second)
			wg.Done()
		}(&waitGroup)
	}
	waitGroup.Wait()

	fmt.Printf("读写耗时：%s\n", time.Now().Sub(startTime))
}

// WriteOnly 运行多个加了写锁的任务，count为任务数
func WriteOnly(count int) {
	var rwMutex sync.RWMutex
	var waitGroup sync.WaitGroup

	startTime := time.Now()

	waitGroup.Add(count)
	for i := 0; i < count; i++ {
		go func(wg *sync.WaitGroup) {
			rwMutex.Lock()
			defer rwMutex.Unlock()
			time.Sleep(time.Second)
			wg.Done()
		}(&waitGroup)
	}
	waitGroup.Wait()

	fmt.Printf("只写耗时：%s\n", time.Now().Sub(startTime))
}

func main() {
	WriteOnly(6)

	ReadWrite(6)

	ReadOnly(6)
}

```

{% embed url="https://play.golang.org/p/HGlUbZoq6Xq" caption="例子：通过读写锁降低性能损耗" %}

以上代码的运行结果：

```text
只写耗时：6s
读写耗时：4s
只读耗时：1s
```

{% hint style="info" %}
只要不是协程安全的类型，在跨协程使用过程中会发生数据变化的，都应当加锁。加锁时应当以事务为单位加，且不能出现嵌套加锁，否则会出现死锁。

只要事务中出现了写操作，就应当加写锁；只有在事务中不存在写操作时，才加读锁。
{% endhint %}



