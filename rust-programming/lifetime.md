## **lifetime**
>rust 要求被引用对象的lifetime 必须长于引用的生命周期, 大多数情况是编译器自动推算生命周期
>1. lifetime 专门针对reference
>2. lifetime 是为了避免dangling reference(悬空引用)

## **lifetime推导**

要推导Lifetime是否合法，先明确两点：
 - 输出值（也称为返回值）依赖哪些输入值
 - 输入值的Lifetime大于或等于输出值的Lifetime (准确来说：子集，而不是大于或等于)

Lifetime推导公式： 当输出值R依赖输入值X Y Z ...，当且仅当输出值的Lifetime为所有输入值的Lifetime交集的子集时，生命周期合法。

    ```Lifetime(R) ⊆ ( Lifetime(X) ∩ Lifetime(Y) ∩ Lifetime(Z) ∩ Lifetime(...) )```
