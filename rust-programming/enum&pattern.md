## enum

The enum keyword allows the creation of a type which may be one of a few different variants. Any variant which is valid as a struct is also valid as an enum.

##　使用option来避免空指针异常
  - 如果返回值永远不可能为空，直接返回，接受返回值的地方可直接使用，无需判断
  - 如果返回值可能为空，则返回option，需要在使用的地方进行非空判断

## match
**match: ** 执行一些pattern的比较后执行一段代码逻辑．
match功能强大的原因就是来自于pattern的丰富的表达形式，并且编译器可以确保所有可能的情况都被处理到．
Patterns可以由字面量，参数名，通配符，等很多东西组成

`-` placeholder '-'代表所有其他的可能性，类似java的default

``` 
match Expression except struct expression {
    InnerAttribute*
    MatchArms?
} 
```
