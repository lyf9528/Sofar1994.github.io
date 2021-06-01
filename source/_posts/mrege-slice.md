---
title: '小技巧:合并两个切片'
author: Sofar
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-28 17:40:10
password:
summary:
tags:
    - 小技巧
categories:
---

总所周知，Slice追加元素使用 `append()`,而`append`函数是这样的`append(slice []Type, elems ...Type)`,
这里的`...`就是关键所在。切片合并`append([]int{1,2},[]int{3,4})`
就像其他任何可变参数函数一样.

```go
    func foo(is... int) {
        for i := 0; i < len(is); i++ {
            fmt.Println(is[i])
        }
    }

    func main() {
        foo([]int{1,2,3,4,5}...)
    }
```
