# LinkQueue（非循环队列）

#### 带头结点：

​	队头指向头结点

​	队尾指向最后一个元素

​	队列为空时：`Q.front==Q.rear`

​	队列满时：一般来说不会满，内存满，队列满

#### 不带头结点：

​	队头指向第一个元素

​	队尾指向最后一个元素

​	队列为空时：`Q.front==NULL`

`疑问：Q.front==Q.rear==NULL 为什么会返回true`

`回答：Q.front==Q.rear==NULL的判断实际上是先判断Q.front==Q.rear，然后再将其结果与NULL比较，这个比较操作的结果是一个指针类型的值，不能直接存储在bool类型的变量中。`

​	队列满时：一般来说不会满，内存满，队列满

### 链式队列数据结构

```c++
//队列中结点的数据结构
typedef struct LinkNode{
	ElemType data;
	LinkNode *next;
}LinkNode;
//队列的数据结构
typedef strutc{
	LinkNode *front;
	LinkNode *rear;
}LinkQueue;
```

#### 带头结点的链式队列的基本操作

//初始化队列，构建一个空的队列

#### bool  InitQueue(LinkQueue &Q);

```c++
bool  InitQueue(LinkQueue &Q){
    //判断输入是否合法

    //初始化操作
    Q.front=(LinkNode *) malloc(sizeof (LinkNode));
    if(Q.front==NULL) return false;
    Q.rear=Q.front;
}
```

//判队列空，若队列Q为空返回true，否则返回false

#### bool QueueEmpty(LinkQueue Q);

```c++
//判队列空，若队列Q为空返回true，否则返回false
bool QueueEmpty(LinkQueue Q){
    if(Q.front==Q.rear) return true;
    return false;
}
```

//入队，若队列Q未满，将x加入，使之成为新的队尾

#### bool EnQueue(LinkQueue &Q,ElemType x);

```c++
//入队，若队列Q未满，将x加入，使之成为新的队尾
bool EnQueue(LinkQueue &Q,ElemType x){
    //判断输入数据是否合法
    if(Q.front==NULL) return false;
    //创建的准备好的结点p
    LinkNode *p=(LinkNode *) malloc(sizeof (LinkNode));
    if(p==NULL) return false;
    p->data=x;
    p->next=NULL;
    //将准备好的结点p后插入尾结点
    Q.rear->next=p;
    Q.rear=Q.rear->next;

    return true;
}
```

//出队，若队列Q未空，删除队头元素，并用x返回

#### bool DeQueue(LinkQueue &Q,ElemType &x);

```c++
//出队，若队列Q未空，删除队头元素，并用x返回
bool DeQueue(LinkQueue &Q,ElemType &x){
    //判断输入是否合法
    if(Q.front==NULL || QueueEmpty(Q)) return false;
    //创建一个指针q指向需要出队的结点
    LinkNode *q;
    q=Q.front->next;
    //出队
    x=q->data;
    Q.front->next=q->next;
    if(Q.rear==q) Q.rear=Q.front;
    free(q);
    return true;
}
```

//读队头元素，若队列Q非空，则将队头元素赋值给x

#### bool GetHead(LinkQueue Q,ElemType &x);

```c++
//读队头元素，若队列Q非空，则将队头元素赋值给x
bool GetHead(LinkQueue Q,ElemType &x){
    //判断输入是否合法
    if(Q.front==NULL || QueueEmpty(Q)) return false;
    x=Q.rear->data;
}
```

### 不带头结点的链式队列的基本操作

//初始化队列，构建一个空的队列

#### void InitQueue(LinkQueue &Q);

```c++
void  InitQueue(LinkQueue &Q){
    Q.front=NULL;
    Q.rear=NULL;
}
```

//判队列空，若队列Q为空返回true，否则返回false

#### bool QueueEmpty(LinkQueue Q);

```c++
bool QueueEmpty(LinkQueue Q){
    if(Q.front==NULL) return true;
    return false;
}
```

//入队，若队列Q未满，将x加入，使之成为新的队尾

#### bool EnQueue(LinkQueue &Q,ElemType x);

```c++
bool EnQueue(LinkQueue &Q,ElemType x){
    //判断输入是否合法

    //创建一个准备好的结点p
    LinkNode *p=(LinkNode *) malloc(sizeof (LinkNode));
    if(p==NULL) return false;
    p->data=x;
    p->next=NULL;
    //入队
    if(Q.front==NULL){
        Q.front=p;
        Q.rear=Q.front;
    }
    Q.rear->next=p;
    Q.rear=Q.rear->next;
    return true;
}
```

//出队，若队列Q未空，删除队头元素，并用x返回

#### bool DeQueue(LinkQueue &Q,ElemType &x);

```c++
bool DeQueue(LinkQueue &Q,ElemType &x){
    //判断输入是否合法
    if(QueueEmpty(Q)) return false;
    //创建一个指针q指向要删除的结点
    LinkNode *q;
    q=Q.front;
    //出队
    x=q->data;
    Q.front=q->next;
    if(Q.rear==q) Q.rear=NULL;
    free(q);
    return true;
}
```

//读队头元素，若队列Q非空，则将队头元素赋值给x

#### bool GetHead(LinkQueue Q,ElemType &x);

```c++
bool GetHead(LinkQueue Q,ElemType &x){
    //判断输入是否合法
    if(QueueEmpty(Q)) return false;
    //读
    x=Q.rear->data;
    return true;
}
```

