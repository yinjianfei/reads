## string
是一个UTF8编码的,可增长的字符串,存储在heap中

## str
**str** 也叫作 string slice, 通常是已引用形式出现 &str, &str 是一个已存在string外观表示,
&str 是个共享且不可变的string 类型的引用, str 类型的存在是为了防止在操作string时产生新的string 变量
&str 是static lifetime
&str 由两部分组成,指向一个地址的指针,和一个长度
&str 高效,因为没有heap的copy

## 总结

In summary, use String if you need owned string data (like passing strings to other threads, or building them at runtime), and use &str if you only need a view of a string
