# 顺序表的优缺点：

**优点：**可以随机存取，存储密度高

**缺点：**要求大片连续空间，改变容量不方便

# 顺序表的基本操作

## 1. 静态顺序表

### *静态*   顺序表的全局变量

```c++
//！！: 定义全局变量的时候是没有用分号结尾的，但是自定义变量名的时候使用分号结尾的，函数声明的时候也用分号结尾。
#define Max_Size 50
typedef int ElemType; 
```

### *静态*   顺序表的数据结构

```c++
typedef struct {
	ElemType data[Max_Size];
	int length; //静态顺序表的当前长度
}SqList;
```

### *静态*   顺序表的初始化操作

```c++
void InitList(SqList &L){
	for(int i=0;i<Max_Size;i++){ //这一步其实可以省略，因为访问的时候应该是用length访问
		L.data[i]=0;
	}
	L.length=0;
}
```

### 静态 顺序表的插入操作（时间复杂度 O(n) ）

```c++
bool List_0_Insert(SqList_0 &L,int i,int e){
    if(i<1 || i>L.length+1) return false;
    if(L.length==Max_Size) return false;
    for(int j=L.length;j>=i;j--){
        L.data[j]=L.data[j-1];
    }
    L.data[i-1]=e;
    L.length++;
    return true;
}
```

### 静态 顺序表的删除操作（时间复杂度 O(n) ）

```c++
bool List_0_Delete(SqList_0 &L,int i,int &e){
    if(i<1 || i>L.length) return false;
    if (L.length==0) return false;//这一步显得有点多余了。因为第一个if
    e=L.data[i-1];
    for(int j=i-1;j<=L.length;j++){
        L.data[j]=L.data[j+1];
    }
    L.length--;
}
```



## 2. 动态顺序表

### *动态*  顺序表的全局变量

```c++
#define InitSize 10 //动态顺序表的初始容量
typedef int ElemType;
```

### *动态*  顺序表的数据结构

```c++
typedef struct {
	ElemType *data;
	int MaxSize; //存储动态顺序表当前的最大容量
	int length; //动态顺序表的当前长度
}SqList;
```

### *动态*  顺序表的初始化操作

```c++
void InitList(SqList &L){
	L.data=(ElemType*) malloc( sizeof( ElemType ) * InitSize );
	L.MaxSize=InitSize; //第一次创建的时候，动态顺序表的最大容量为初始容量
	L.length=0;
}
```

### *动态*  顺序表的扩容操作

```c++
void IncreaseList(SqList &L,int len){
	ElemType *p=L.data; //存储久的动态顺序表
	L.data=(ElemType*) malloc( sizeof( ElemType ) * ( L.MaxSize+len )); //动态顺序表扩容
	for(int i=0;i<L.length;i++){ //将久的动态顺序表中的数据复制到新的动态顺序表当中
		L.data[i]=p[i];
	}
	L.MaxSize+=len; //改变动态顺序表中的当前最大容量
	free(p); //释放久的动态顺序表
}
```

### 动态 顺序表的插入操作（时间复杂度 O(n) ）

```c++
bool List_Insert(SqList &L,int i,int e){
    if(i<1 || i>L.length+1) return false;
    if(L.length==L.MsxSize){//当动态顺序表满时
        IncreaseList(L,10);//动态顺序表扩容10
    }
    for(int j=L.length;j>=i;j--){
        L.data[j]=L.data[j-1];
    }
    L.data[i-1]=e;
    L.length++;
    return true;
}
```

### 动态 顺序表的删除操作（时间复杂度 O(n) ）

```c++
bool List_Delete(SqList &L,int i,int &e){
    if(i<1 || i>L.length) return false;
    if(L.length==0) return false;
    e=L.data[i-1];
    for(int j=i-1;j<L.length;j++){
        L.data[j]=L.data[j+1];
    }
    L.length--;
    return true;
}
```

### 顺序表-按位查找（ O(1) ）

```c++
Element GetElement(SqList L,int i){
    return L.data[i-1];
}
```

### 顺序表-按值查找（ O(n) ）

```c++
int LocatedElement(SqList L,Element e){
    for(int i=0;i<L.length;i++){
        if(L.data[i]==e){
            return i+1;
        }
    }
    return 0;
}
```

