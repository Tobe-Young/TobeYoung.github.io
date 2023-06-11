
# Dictionary、List<T> 元素类似是值类型时，容器元素存储在哪？

最近突然彻底搞清楚这个问题
```
struct E {
    public int a;
}

Dictionary<int, E> dic = new Dictionary<int, E>();
dic[1] = new E();
dic[2] = new E();
//那么dic[1]对应的值存在堆还是栈呢？
```
答案是堆，此处一定搞清楚c#对于valueType的定义，并编译器角度来看待它，就能很容易得出答案。

## 引用
1. https://learn.microsoft.com/en-us/archive/blogs/ericlippert/the-stack-is-an-implementation-detail-part-one
2. https://learn.microsoft.com/en-us/archive/blogs/ericlippert/the-truth-about-value-types