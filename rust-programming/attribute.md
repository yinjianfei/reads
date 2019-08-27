# 概念
1. 属性(attribute)是表达元数据的一种形式,和java的annotation类似.
2. 箱(Crate)是一个独立的可编译单元,就是一个或者一批文件,其中一个文件是crate入口,编译后可以得到一个库或可执行文件
3. 项(item) 是Crate的一个组件, iteam 包括
    - modules
    - extern crate declarations
    - use declarations
    - function definitions
    - type definitions
    - struct definitions
    - enumeration definitions
    - union definitions
    - constant items
    - static items
    - trait definitions
    - implementations
    - extern blocks

## attribute

inner attribute 语法#![xxxx] 一般在花括号内部.
outter attribute 语法#[] 在花括号外部

>Attributes may be applied to many things in the language:
>All item declarations accept outer attributes while external blocks, functions, implementations, and modules accept inner attributes.
>Most statements accept outer attributes (see Expression Attributes for limitations on expression statements).
>Block expressions accept outer and inner attributes, but only when they are the outer expression of an expression statement or the final expression of another block expression.
>Enum variants and struct and union fields accept outer attributes.
>Match expression arms accept outer attributes.
>Generic lifetime or type parameter accept outer attributes.
>Expressions accept outer attributes in limited situations, see Expression Attributes for details.

### Testing Attribute(测试属性)
1. test 标识一个函数为测试函数.只有在测试模式下才会编译
2. ignore 被标识为[test]的函数,也可以被[ignore="xxx"]来标识,标识这个函数在测试时不会执行,但仍然会被编译
3. should_panic 被标识为[test]并且返回为()的函数,也可以被标识为[should_panic(excepted = "xxx")]

### Derive (生成属性)

derive 属性标识允许被标识的item自动生成代码, 它使用 [MetaListPaths](https://doc.rust-lang.org/reference/attributes.html#meta-item-attribute-syntax)语法来制定一系列的trait 或者是 derive marco的 paths来实现代码自动生成
如可以使用#[derive(PartialEq, Clone)] 来标识一个struct自动生成clone(),eq() ne()方法.

### Diagnostic attributes (诊断属性)
Diagnostic attributes 用于在编译期间产生的诊断信息

1. **Lint check attribute** lint attribute `allow`, `warn`,`deny`,`forbid` 它使用 [MetaListPaths](https://doc.rust-lang.org/reference/attributes.html#meta-item-attribute-syntax)语法来更改lint level, 可以使用rustc -W help 来查看所有支持的诊断name
2. **Tool lint attribute** Tool lint允许使用一些工具提供的检查语法,目前只有clippy这一个lint tool,[list of lint](https://rust-lang.github.io/rust-clippy/master/index.html)来查看所有支持的诊断的name

### Code generation attribute
code generation attribute 用来在代码生成是提供控制信息,或者优化建议

1. `inline` 优化建议,建议在生成代码时将被标识的函数copy到调用者内部 `#[inline]`,`#[inline(always)]`,`#[inline(never)]`
2. `cold` 优化建议, 标识为不太会被掉用的方法
3. `no_buildins` 应用于crate上, 去掉内建函数。
4. `target_feature` 应该使用在unsafe funtion上,可以针对不同的平台架构生成不同的代码

### limit attribute
设置编译期的限制
1. `recursion_limit` 设置编译期的最大递归层次,#![recursion_limit = "4"]
2. `type_length_limit` 

### [conditional compilation](https://doc.rust-lang.org/reference/conditional-compilation.html)
条件编译是指使用attribute `cfg` `cfg_attr`或者是build-in `cfg marco`执行一个表达式,根据表达式的结果来决定是否需要编译相应代码
1. `cfg` 除了在泛型上不能使用外,其他attribute可以使用的地方都可以使用`cfg`
    - #[cfg(target_os = "macos")]
    - #[cfg(any(foo, bar))]
    - #[cfg(all(unix, target_pointer_width = "32"))]
    - #[cfg(not(foo))]
2. `cfg_attr`  除了在泛型上不能使用外,其他attribute可以使用的地方都可以使用`cfg_attr`
    - #[cfg_attr(linux, path = "linux.rs")]
    - #[cfg_attr(windows, path = "windows.rs")]
3. `cfg marco`
    ``` 
    let machine_kind = if cfg!(unix) {
    "unix"
    } else if cfg!(windows) {
    "windows"
    } else {
    "unknown"
    }
    ```
