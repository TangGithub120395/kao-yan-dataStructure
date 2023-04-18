# 双链表

双链表的结点定义

```c++
//双链表的结点定义
typedef struct DLNode{
    ElemType data;
    DLNode *next;
    DLNode *prior;
}*DLinkList;
```

双链表的初始化操作

```c++
//双链表的初始化操作(带头结点)
bool Init(DLinkList &L){
    L=(DLNode*) malloc(sizeof (DLNode));
    if(L==NULL) return false;//当分配空间失败时，返回false
    L->next=NULL;//头结点的后驱指针只有当双链表为空时为NULL
    L->prior=NULL;//头结点的前驱指针永远都是NULL
    return true;
}
```

双链表的判断链表空操作

```c++
//双链表的判断链表空操作
bool Empty(DLinkList L){
    if(L==NULL){
        printf("该链表不存在\n");
        return false;
    }
    if(L->next==NULL) return true;
    return false;
}
```

获取双链表长度操作

```c++
//获取双链表的长度
int Length(DLinkList L){
    //判断输入是否合法
    if(L==NULL) return -1;
    else{
        //创建一个指针遍历双链表
        DLNode *p=L;
        int flag=0;
        while (p->next!=NULL){
            p=p->next;
            flag++;
        }
        return flag;
    }
}
```

双链表的按位查找操作

```c++
//双链表的按位查找操作
DLNode* Locate(DLinkList L,int i){
    //声明一个指针q指向双链表的第i个位置
    DLNode *q=L;
    for (int j = 0; j < i; ++j) {
        q=q->next;
    }
    return q;
}
```

结点前插结点操作

```c++
//双链表的结点前插结点操作
bool Insert_Node_P(DLNode *p,DLNode *node){
    //判断输入参数是否合法
    if(p==NULL || node==NULL) return false;
    //定义prior_p指针指向p结点的前驱
    DLNode *prior_p=p->prior;
    //因为该链表有头结点，所以不需要判断插入位置的结点的前驱是否为空
    //将结点插入
    prior_p->next=node;
    node->prior=prior_p;
    node->next=p;
    p->prior=node;
    return true;
}
```

结点后插结点操作

```c++
//双链表的结点后插结点操作
bool Insert_Node_N(DLNode *p,DLNode *node){
    //判断输入参数是否合法
    if (p==NULL || node==NULL) return false;
    //定义next_p指针指向p结点的后驱
    DLNode *next_p=p->next;
    //判断插入位置的结点的后驱是否为空
    if(next_p!=NULL){
        next_p->prior=node;
    }
    node->next=next_p;
    p->next=node;
    node->prior=p;
    return true;
}
```

删除结点操作

```c++
//删除双链表结点操作
bool Delete_Node(DLNode *p,ElemType &e){
    //声明一个指针p_prior指向p结点的前驱
    DLNode *p_prior=p->prior;
    //声明一个指针p_next指向p结点的后驱
    DLNode *p_next=p->next;
    //赋值
    e=p->data;
    //删除结点---前驱的next指针指向后驱，后驱的prior指针指向前驱，p指针释放
    p_prior->next=p_next;
    p_next->prior=p_prior;
    p->next=NULL;
    p->prior=NULL;
    free(p);
}
```

双链表的前插结点操作

```c++
//双链表的前插结点操作
bool Insert_List_P(DLinkList &L,int i,ElemType e){
    //判断L输入是否合法
    if(L==NULL || Empty(L)) return false;//当双链表为空时，无法对头结点进行前插操作
    if(i<1 || i> Length(L)) return false;
    //创建需要插入的结点
    DLNode *p=(DLNode *) malloc(sizeof (DLNode));
    p->data=e;
    p->next=NULL;
    p->prior=NULL;
    //创建q指针,找到需要插入的位置
    DLNode *q= Locate(L,i);
    //前插操作
    Insert_Node_P(q,p);
    return true;
}
```

双链表的后插结点操作

```c++
//双链表的后插结点操作
bool Insert_List_N(DLinkList &L,int i,ElemType e){
    //判断L是否合法
    if(L==NULL) return false;
    //判断i是否合法
    if(i<0 || i> Length(L)) return false;
    //创建一个准备好的结点
    DLNode *p=(DLNode *) malloc(sizeof (DLNode));
    p->data=e;
    p->next=NULL;
    p->prior=NULL;
    //创建q指针指向需要插入的位置
    DLNode *q= Locate(L,i);
    //后插操作
    Insert_Node_N(q,p);
    return true;
}
```

双链表的删除结点操作

```c++
//双链表的删除结点操作
bool Delete_List_Node(DLinkList &L,int i,ElemType &e){
    //判断输入L是否合法
    if(L==NULL) return false;
    //判断输入i是否合法
    if(i<1 || i> Length(L)) return false;
    //声明q结点指向需要删除的位置
    DLNode *q= Locate(L,i);
    //删除结点操作
    Delete_Node(q,e);
}
```

双链表的链表销毁操作

```c++
//双链表的链表销毁操作
bool Destroy_List(DLinkList &L){
    //声明指针q遍历双链表--将每个结点删除，并且释放L
    DLNode *q=L;
    ElemType e;
    while (q!=NULL){
        Delete_Node(q,e);
    }
    free(L);
}
```

双链表的后向遍历操作

```c++
//双链表的后向遍历操作
bool Print_List_N(DLinkList L){
    //判断输入L是否合法
    if(L==NULL) return false;
    //创建q结点指向第一个结点
    DLNode *q=L->next;
    //遍历输出
    while (q!=NULL){
        printf("%d ",q->data);
        q=q->next;
    }
    printf("\n");
    return true;
}
```

双链表的前向遍历操作

```c++
//双链表的前向遍历操作
bool Print_List_P(DLinkList L){
    //判断输入数据L是否合法
    if(L==NULL) return false;
    //声明指针q指向双链表的最后一个元素
    DLNode *q=L;
    for (int i = 0; i < Length(L); ++i) {
        q=q->next;
    }
    //遍历输出
    while (q->prior!=NULL){
        printf("%d ",q->data);
        q=q->prior;
    }
    printf("\n");
    return true;
}
```

双链表的按值查找操作

