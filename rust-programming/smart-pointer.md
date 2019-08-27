
smart pointer和reference最大的区别是reference只会borrow数据,而smart pointer可以own数据
smart pointer通常由struct实现,不过这些struct必须实现Deref和Drop这两个traits, Deref trait允许smart pointer表现的和reference一样,Drop 允许我们可以定制化数据超出作用域时的销毁策略

##　Box<T>
Box 允许我们把数据存储在堆上,而非在栈中,在栈中只保留pointer,相对于unboxed的value,内存占用会大点,而且取值的时候需要进行解引用再获取真正的值,开销也会打一点,但是通过box,所有对象看起来像是以同样的大小存储,因为只需要一个指针就够了,应用程序可以同等看待

```
Vec<i32>

(stack)    (heap)
┌──────┐   ┌───┐
│ vec1 │──→│ 1 │
└──────┘   ├───┤
           │ 2 │
           ├───┤
           │ 3 │
           ├───┤
           │ 4 │
           └───┘
```

```
Vec<Box<i32>>

(stack)    (heap)   ┌───┐
┌──────┐   ┌───┐ ┌─→│ 1 │
│ vec2 │──→│   │─┘  └───┘
└──────┘   ├───┤    ┌───┐
           │   │───→│ 2 │
           ├───┤    └───┘
           │   │─┐  ┌───┐
           ├───┤ └─→│ 3 │
           │   │─┐  └───┘
           └───┘ │  ┌───┐
                 └─→│ 4 │
                    └───┘
```
## Rc 和 Arc
在rust中默认同一个资源只有一个所有者,但是这种限制过于严格,rust在std 标准包中引入引用计数,来实现同一资源多有所有权拥有者.

1. `Rc` 用于单线程,通过`use std::rc::Rc` 引入,通过 `clone()`来增加引用
   - 用Rc包装的对象是不可变的,只读的
   - 一旦最后一个拥有者消失,资源就会被回收,这个生命周期,在编译器就确定了
   - Rc只能用于单线程
   - Rc实际上就是一个指针,不影响包裹他的对象的调用形式
2. `Rc weak` 是一个weak指针,可以引用对象,但是不会增加引用计数
   - 可访问,但不拥有对象,不增加引用计数,因此不会对资源回收造成影响
   - 可由`Rc<T>` 调用`downgrade()`方法转换为`Weak<T>`
   - `Weck<T>`调用`upgrade()`方法转换为`Option<Rc<T>>`如果资源已释放,则Option的值为None
   - 常用与解决循环引用

3. `Arc` 原子引用计数,`Rc`的多线程版 通过`use std::sync:Arc`引入,通过`clone()`来增加引用
   - `Arc`可跨线程传递
   - 用`Arc`包裹的对象,对可变性没有要求
   - 一旦最后一个拥有者消失,资源就会被回收,这个生命周期,在编译器就确定了
   - `Arc`实际上就是一个指针,不影响包裹他的对象的调用形式
   - `Arc`对于多线程共享是必须(减少复制,提高性能)
4. `Arc weak` 多线程版的`Rc weak`,用法性质基本一致

## Mutex 和 RwLock

1. `Mutex` 是互斥对象,用来保护共享对象 
   - `Mutex`会等待获取访问令牌(token),等待过程中阻塞线程,同一个时刻只有一个线程的`Mutex`对象可以获取到锁
   - `Mutex`通过`.lock()`或`.try_lock()`来尝试获得令牌,必须通过RAII返回的守卫(smart pointer MutexGuard)来操作
   - 当RAII 守卫作用域结束,锁自动解开
   - 多线程中 `Mutex` 一般和`Arc` 一同使用

2. `RwLock` 读写锁,
   - 同时允许多个读,最多只能有一个写
   - 读写不能同时存在.

## Cell 和 RefCell
在默认场景下,我们要修改数据,要么标记该对象为`mut` 或者使用`&mut`引用.这种限制过于严格,因此rust在标准库中引入了`Cell`,`RefCell`来弥补了Rust所有权灵活行不足的场景

1. `Cell` 
   - 只能用于实现了`copy trait`的类型
   - 使用`get()`方法,返回内部值
   - 使用`set()`方法,来修改内部值
    ```
    use std::cell::Cell;

    let c = Cell::new(5);
    
    let five = c.get();

    c.set(10);
    ```
2. `RefCell` 可以包裹所有类型,相比于`Cell` 更加通用,相比于标准的`静态借用`.`RefCell`实现了`运行时借用`.意味着编译器不会对`RefCell`中内容检查,需要用户负责
   -  在不确定一个对象是否实现了`Copy`时,使用`RefCell`
   -  如果被包裹的对象,同事被可变借用两次,会导致程序崩溃,需要用户进行判断
   -  `RefCell`只能用于内部线程.不能跨线程
   -  `RefCell`通常可`Rc`配合使用
   -  `.borrow()` 可同时多次调用,可同时存在多个不可变引用
   -  `.borrow_mut()` 同时只有一个不可变引用
   -  `.into_inner()` 取出包裹的值
    ```
    use std::collections::HashMap;
    use std::cell::RefCell;
    use std::rc::Rc;

    fn main() {
        let shared_map: Rc<RefCell<_>> = Rc::new(RefCell::new(HashMap::new()));
        shared_map.borrow_mut().insert("africa", 92388);
        shared_map.borrow_mut().insert("kyoto", 11837);
        shared_map.borrow_mut().insert("piccadilly", 11826);
        shared_map.borrow_mut().insert("marbles", 38);
    }****
    ```
    