
# Table of Contents

1.  [LinkedList](#org1f3e960)
    1.  [文档](#orga8a9ab6)
    2.  [Usage](#orgef8073a)
        1.  [创建链表](#org2cfc0f7)
        2.  [添加数据](#orgffdeb78)
        3.  [删除数据](#org63a3edf)
        4.  [迭代相关](#org1db54d2)
        5.  [查找相关](#org41229d3)



<a id="org1f3e960"></a>

# LinkedList

大幅度删改了以下，文档也是  


<a id="orga8a9ab6"></a>

## 文档

[旧版本文档](./docs/oldversion.md)  
[新版本文档](./docs/LinkedList.md)  


<a id="orgef8073a"></a>

## Usage


<a id="org2cfc0f7"></a>

### 创建链表

    list = List(Int) # 创建一个单链表
    queue = Queue(Int) # 创建一个单链表队列
    stack = Stack(Int) # 创建一个单链表栈
    
    double_list = List(Int; isdouble = true) # 创建一个双链表，其他链表类似的，可以对 isdouble 赋值


<a id="orgffdeb78"></a>

### 添加数据

1.  通用的方法

        push!(list, 1) # 对 List 类型会默认添加到末尾
        push!(queue, 1) # 同上
        push!(stack, 1) # 对 Stack 类型默认添加到头

2.  建议只用在 `List` 上

        iter = findfirst(isequal(1), list)
        if !isnothing(iter)
          pushnext!(list, iter, -1)
        end


<a id="org63a3edf"></a>

### 删除数据

1.  通用的方法

        pop!(list) # 默认删除末尾的数据
        pop!(queue); pop!(stack) # 默认删除头的数据

2.  建议只用在 `List` 上

        iter = findfirst(isequal(-1), list)
        if !isnothing(iter)
          popat!(list, iter)
        end


<a id="org1db54d2"></a>

### 迭代相关

迭代方法已经为定义好了  

    iterate(linkedlist::ListType) where ListType <: AbstractLinkedList =
      iterate(linkedlist.baselist)
    
    iterate(linkedlist::ListType, state::NodeType) where {ListType <: AbstractLinkedList, NodeType <: ListCons} =
      iterate(linkedlist.baselist, state)
    
    iterate(::ListType, state::NilNode) where ListType <: AbstractLinkedList = nothing

相关高阶函数可以调用，如 `map` , `reduce`  
不过 `filter` 需要自己定义，还好我也写了  


<a id="org41229d3"></a>

### 查找相关

查找所需要的接口是 `keys` 函数  

    keys(linkedlist::ListType) where ListType <: AbstractLinkedList = keys(linkedlist.baselist)

这样以后，可以直接调用 `find` 系列函数，返回值是 `链表节点` 或者空值 `nothing`  

