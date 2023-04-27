# LinkStack-2

这里采取

1. 带头结点
2. 头插法存储
3. 栈顶指向栈的栈顶元素的下一位元素



链表存储结构和栈存储结构对比

1. 链表中的头指针就是栈顶指针
2. 链表采用头插法的方式存储结点
3. 这个链表就是把栈倒过来设计

![image-20230422140300177](lession2.3-LinkedStack-2.assets/image-20230422140300177.png)

## 链栈的数据结构

`带头结点`的

```c++
typedef int ElemType;
typedef struct StackNode{
	ElemType data;
	StackNode *next;
}*LinkStack;
```

## 链栈的基本操作

//初始化栈。构造一个空栈 S，分配内存空间。

### bool InitStack(LinkStack &S);

```c++
//初始化栈。构造一个空栈 S，分配内存空间。
bool InitStack(LinkStack &S){
    S=(LinkStack) malloc(sizeof (StackNode));
    if(S==NULL) return false;
    S->next= NULL;
    S->data=0;
    return true;
}
```

//判断一个栈 S 是否为空。若S为空，则返回

### bool StackEmpty(LinkStack S);

```c++
//判断一个栈 S 是否为空。若S为空，则返回
bool StackEmpty(LinkStack S){
    //判断输入是否合法
    if(S==NULL) return false;
    if(S->next==NULL) return true;
    return false;
}
```

//读栈顶元素。若栈 S 非空，则用 x 返回栈顶元素

### bool GetTop(LinkStack S,ElemType &x);

```c++
//读栈顶元素。若栈 S 非空，则用 x 返回栈顶元素
bool GetTop(LinkStack S,ElemType &x){
    //判断输入是否合法
    if(StackEmpty(S)) return false;
    //声明一个栈结点q指向栈顶元素
    StackNode *q=S->next;
    x=q->data;
    return true;
}
```

//进栈，若栈S未满，则将x加入使之成为新栈顶。

### bool Push(LinkStack &S,ElemType x);

```c++
//进栈，若栈S未满，则将x加入使之成为新栈顶。
bool Push(LinkStack &S,ElemType x){
    //判断输入是否合法
    if(S==NULL) return false;
    //创建一个准备好的栈结点p
    StackNode *p=(StackNode*) malloc(sizeof (StackNode));
    p->next=NULL;
    p->data=x;
    //声明一个栈结点q，并且将q定义为栈顶元素
    StackNode *q=S->next;
    //入栈
    S->next=p;
    p->next=q;
    return true;
}
```

//出栈，若栈S非空，则弹出栈顶元素，并用x返回。

### bool Pop(LinkStack &S,ElemType &x);

```c++
//出栈，若栈S非空，则弹出栈顶元素，并用x返回。
bool Pop(LinkStack &S,ElemType &x){
    //判断输入是否合法
    if(S==NULL || StackEmpty(S)) return false;
    //声明一个栈结点q，并且将q定义为栈顶元素
    StackNode *q=S->next;
    //出栈
    x=q->data;
    S->next=q->next;
    free(q);
    return true;
}
```

//销毁栈。销毁并释放栈 S 所占用的内存空间。

### bool DestroyStack(LinkStack &S);

```c++
//销毁栈。销毁并释放栈 S 所占用的内存空间。
bool DestroyStack(LinkStack &S){
    //遍历S
    while (S!=NULL){
        StackNode *q=S;
        S=S->next;
        free(q);
    }
}
```

