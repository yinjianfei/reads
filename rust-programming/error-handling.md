## recoverable error
enum Result<T, E> {
  程序可以处理的异常使用:
    Ok(T),
    Err(E),
}
## unrecoverable error
当遇到程序无法处理的错误时使用panic!, 当程序遇到panic!时，会打印错误消息，清除栈数据后退出程序
