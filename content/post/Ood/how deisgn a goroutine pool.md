---
layout:   page
title:       "如何设计一个优雅的goroutine池子"
subtitle:    ""
description: ""
date:        2024-01-07
author:   "aiqiu"
#image:       "https://golearning.oss-cn-shanghai.aliyuncs.com/202304161638319.png"
categories:  ["Geeks"]
published: true
---
# 如何设计一个优雅的goroutine池子

要实现一个优雅的goroutine 池需要考虑以下方面的内容：

1. **池的大小和动态调整**：
    - 确定池的大小：这通常取决于你的应用程序的特性和目标硬件的特性。考虑CPU的核心数、内存限制和预期的并发量。
    - 动态调整：在运行时根据工作负载动态调整池的大小。例如，可以在高负载时增加goroutines数量，在低负载时减少。
2. **任务队列管理**：
    - 实现一个高效的任务队列来存储待处理的任务。
    - 需要决定是使用无缓冲的通道来实现即时执行，还是使用有缓冲的通道来允许排队。
3. **goroutine 生命周期管理**：
    - 确定何时启动和停止goroutines。例如，可以在启动时创建所有goroutine，然后在结束时统一关闭，或者根据需求动态创建和销毁。
4. **错误处理和恢复**：
    - 实现机制来捕捉并处理goroutine中的panic，确保单个任务的失败不会影响整个池的稳定性。
    - 提供日志记录和监控，以便于追踪问题和性能瓶颈。
5. **资源共享和同步**：
    - 考虑goroutines之间的资源共享问题，避免竞争条件和死锁。
    - 使用锁、通道或其他同步原语来协调对共享资源的访问。
6. **优雅地关闭和清理**：
    - 实现优雅地关闭goroutine池的机制，确保所有进行中的任务都能完成后再停止。
    - 清理资源，比如关闭打开的文件、网络连接等。
7. **性能考量**：
    - 测试和优化性能，确保goroutine池不会成为性能瓶颈。
    - 考虑内存和CPU使用情况，避免过度消耗资源。

一个考虑以上内容的具体实现
```go
import (
	"errors"
	"log"
	"sync"
)

type GoroutinePool struct {
	pool    chan struct{}
	maxsize int
	closed  bool
	mu      sync.Mutex
	wg      sync.WaitGroup
}

// NewGoroutinePool 初始化函数
func NewGoroutinePool(size int) *GoroutinePool {
	return &GoroutinePool{
		pool:    make(chan struct{}, size),
		maxsize: size,
	}
}

func (p *GoroutinePool) Resize(size int) {
	p.maxsize = size
	p.pool = make(chan struct{}, size)
}

func (p *GoroutinePool) Submit(task func()) error {
	p.mu.Lock()
	if p.closed {
		p.mu.Unlock()
		return errors.New("goroutine pool is closed")
	}
	p.wg.Add(1) // 在启动goroutine之前增加计数器
	p.mu.Unlock()

	p.pool <- struct{}{} // 如果池满了，这里会阻塞
	go func() {
		defer func() {
			<-p.pool
			p.wg.Done() // goroutine完成后减少计数器
		}()
		task()
	}()

	return nil
}

func (p *GoroutinePool) Shutdown() {
	p.mu.Lock()
	p.closed = true
	p.mu.Unlock()

	p.wg.Wait() // 等待所有goroutine完成
	close(p.pool)
}

// SafeSubmit 保证提交任务的安全性 抛出error 由调用者处理
func SafeSubmit(pool *GoroutinePool, task func()) error {
	return pool.Submit(func() {
		defer func() {
			if r := recover(); r != nil {
				log.Printf("Recovered in task: %v", r)
			}
		}()
		task()
	})
}
```