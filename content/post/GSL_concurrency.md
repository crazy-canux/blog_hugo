---
title: "GSL_concurrency"
date: 2018-01-01T10:49:21
categories: ["Golang"]
tags: ["concurrency"]
keywords: []
author: "Canux"
draft: false
---

# sync

sync用于goroutine同步.

    import "sync"

## constants

## variables

## functions

## Locker

一个可以加锁和解锁的接口

    type Locker interface {
        Lock()
        Unlock()
    }

## Pool

 Pool是一个可以分别存取的临时对象的集合。

## Once

只执行一次动作的对象(单例模式)

    type Once struct {}

method:

    // 只有第一次调用才执行
    func (o *Once) Do(f func())

## Mutex

互斥锁,锁和线程无关，可以由不同的线程加锁和解锁.

Mutex的初始值为未锁的状态，并且Mutex通常作为结构体的匿名成员存在。

同一时刻只能有一个线程进入临界区.

    type Mutex struct {}

    Mutex

method:

    // 加锁，如果已经加锁，阻塞至m解锁．
    func (m *Mutex) Lock()
    // 解锁，如果没有加锁，导致panic
    func (m *Mutex) Unlock()

## RWMutex

读写互斥锁,可以由不同的线程加锁和解锁.

    type RWMutex struct {}

method:

    // 锁定为写入状态，禁止其它线程读写
    func (rw *RWMutex) Lock()
    // 解除写入锁定，如果没有加锁，导致panic
    func (rw *RWMutex) Unlock()
    // 锁定为读取状态，禁止其他线程写入，但是可以读
    func (rw *RWMutex) RLock()
    // 解除读取锁，如果没有加锁，导致panic.
    func (rw *RWMutex) RUnlock()

## Cond

条件变量.

    type Cond struct {
        L Locker
    }

function:

    func NewCond(l Locker) *Cond

method:

    func (c *Cond) Broadcase()

    func (c *Cond) Signal()

    func (c *Cond) Wait()

## WaitGroup

用于等待一组线程结束，父线程用Add方法来设定应等待的线程数量;
每个被等待的线程在结束时应调用Done方法;
同时，主线程应调用Wait方法阻塞至所有线程结束．

struct:

    type WaitGroup sttruct {}

functions:

methods:

    # 增加计数
    func (*WaitGroup) Add(delta int)

    # 减少计数
    func (*WaitGroup) Done()

    # 阻塞直到计数器为0
    func (*WaitGroup) Wait()

***

# sync/atomic

提供底层的原子级内存操作，主要用于goroutine同步.

int32, int64, uint32, uint64, uintptr, pointer.

## function

    func LoadInt32(addr *int32) (val int32)
    func StoreInt32(addr *int32, val int32)
    func AddInt32(addr *int32, delta int32) (new int32)
    func SwapInt32(addr *int32, new int32) (old int32)
    func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)
