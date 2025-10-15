# Step 1.指针与结构体

## 什么是指针？

1. - 如何在C语言中定义指针变量：
   与一般变量声明相似，只是必须在变量名前加星号。根据对象类型不同，选用 int,float,double 等。
   - 指针变量大小是固定的吗，其大小和什么有关：
   不固定。和系统架构有关，如32位系统一般占4字节，64位通常为8字节。在同一系统架构下不同类型指针变量大小一般相同，包括数组指针。特殊的是函数指针大小在一些编译器和平台大小会与数据指针不同。
2. - 输出结果为 20 10,即 x=20,y=10。在所给代码中，x 初始赋值为10，然后对指针变量 p 进行初始化，指向 x 的地址`*p=&x`，此时 *p 赋值为20，也就修改了 x 的值为20。
   - 指针变量 *q 指向 arr, ` *q=arr ` 并未写出数组下标，此时含义为指向 arr[0] 。 `++*arr`等价于 `++(*arr)` ，先取值再自增，等于3+1=4。而 `*++q` 应该与前一个做区分，应视作 `*(++q)` ，指针自增意为所指向的数组地址下标加一，所以等于 arr[1+1]=arr[2]=6。最终 y=4+6=10。
3. - 野指针：
   即指向无效内存地址的指针。危害极大，可能使程序崩溃，造成数据损坏，也可能使程序结果与预想结果不同同时又不好检查，以及安全漏洞。
   - 如何避免：
   先得知道成因，野指针常见成因有指针未初始化，数组越界，局部变量超出了作用域，还有指向的内存被释放。所以应该重点注意以下：
     - 初始化指针；
     - 指针与数组结合时要注意下标，防止越界；
     - 指向局部变量的指针要避免在作用域之外使用；
     - 释放内存后把指向该内存地址的指针置为空指针。
4. 设计函数：
   之前那个 CS-EASY-01 的那个函数失败的原因就是没改变实参地址中的值，所以用指针变量作为形参指向实参地址。

```c
void swap(int *a,int *b){
  int temp = *a;
  *a = *b;
  *b = temp;
}
int main(){
  int a = 10;
  int b = 20;
  swap(&a, &b);
  printf("%d %d",a,b);
}
```

## 什么是结构体？

1. 别名我就用 perinfo 了，正好本意是个人信息，我觉得挺合适。

```c
#include<stdio.h>
typedef struct {
    char name[10];
    char xingbie[6];//female 是6个字母
    int age;
    double tall;
} perinfo;
```

2. 结构体指针：
   结构体指针就是指向结构体变量的指针，声明方式为 `struct 结构体名 *指针变量名` ，初始化的基本方式为在后面加上 `=&结构体变量名` 。
3. 首先先用10个字节储存 name，然后由于 xingbie 占6个，前面变成12（加两个空字节），此时共占18个。再往下，int 占4个，前面变20个，共24个。最后 double 为8字节，正好24为8的倍数，直接加8，最终占32字节。
   
```c
#include<stdio.h>
typedef struct {
    char name[10];
    char xingbie[6];
    int age;
    double tall;
} perinfo;
int main(){
    printf("%zu",sizeof(perinfo));
    return 0;
}
```

验证了一下，对了。


# Step 2.基础数据结构

1. 数组和链表存取机制：

- 数组是连续内存空间存储，访问可以随机，存取直接找到地址读取写入就行。
- 链表内存不一定连续，是用指针连接，所以访问没法随机，只能从头节点开始，时间复杂度比数组高很多。

2. - 单向链表结构特点：
由节点组成，每个节点都有一个数据（数据域）和指向下一个节点的指针（指针域），是单向链式结构，节点可以比较容易地插入和删除。

- 定义一个只存储一个整数的单向链表节点：

```c
#include<stdio.h>
#include<stdlib.h>
struct jiedian {
    int data;
    struct jiedian* next;
};
typedef struct jiedian lianbiao;
```

3. 栈和队列的特点：
   栈是一种后进先出的数据结构，一个经典的类比是比作一叠盘子，从最顶上的也就是最后放的开始取。
   队列则是先进先出，队首的也就是最先放入的先取出。
4. - 图的存储：
   可以使用邻接矩阵，如对于有 n 个节点的图可以用 n*n 的矩阵，用数字表示顶点，用一个矩阵元素（juzhen[i][j]）表示一条边(从 i 节点到 j 节点)。
   也可以用邻接表，即以链表为元素的数组，每个链表中的节点都是一个顶点的相邻顶点。
   - 树的存储：
    双亲表示法：用一个数组储存树，每个元素都包含数据域和双亲位置域。
    此外，还有二叉树表示法，孩子兄弟表示法等等。

# Step 3.综合应用

## Part 1.链起来再说

- 预备操作：

```c
#include<stdio.h>
#include<stdlib.h>
struct jiedian {
    int data;
    struct jiedian* next;
};
typedef struct jiedian lianbiao;
lianbiao* initList(){
    lianbiao* head=(lianbiao*)malloc(sizeof(lianbiao));
    head->data=0;
    head->next=NULL;
    return head;
}
```

如下：

```c
#include<stdio.h>
#include<stdlib.h>
struct jd {
    int data;
    struct jd* next;
};
typedef struct jd lb;
lb* inilist(){
    lb* head=(lb*)malloc(sizeof(lb));
    head->data=0;
    head->next=NULL;
    return head;
}//初始化
lb* tcr(lb* head,int data){
    lb* xjd=(lb*)malloc(sizeof(lb));
    xjd->data=data;
    xjd->next=head;
    return xjd;
}//头部插入函数
lb* wcr(lb* head,int data){
    lb* xjd=(lb*)malloc(sizeof(lb));
    xjd->data=data;
    xjd->next=NULL;
    lb* current=head;
    while(current->next!=NULL){
        current=current->next;
    }
    current->next=xjd;
}//尾部插入函数
lb* sjd(lb* head,int l){
    if(l==1){
        lb* temp=head;
        head=head->next;
        free(temp);
        return head;
    }
    lb* current=head;
    int count=1;
    while (current!=NULL && count<l-1) {
        current = current->next;
        count++;
    }
    if (current != NULL&&current->next!=NULL) {
        lb* temp=current->next;
        current->next=temp->next;
        free(temp);
    }
    return head;
}
lb* fz(lb* head) {
    lb* prev = NULL;
    lb* current=head;
    lb* next = NULL;
    while (current != NULL) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    return prev;
}//反转
bianlishuchu(lb* head){
    lb* current = head;
    while (current != NULL) {
        printf("%d", current->data);
        if (current->next != NULL) {
            printf(" -> ");
        }
        current = current->next;
    }
    printf(" -> NULL\n");
}//遍历输出
freeList(lb* head) {
    lb* current = head;
    while (current != NULL) {
        lb* temp = current;
        current = current->next;
        free(temp);
    }
}
int main(){
    lb* head=inilist();
    char x;
    int a,b,c;
    while(1){
        scanf("%c %d %d %d",&x,&a,&b,&c);
        if(x=='H'||x=='T'){
        if(x=='H'){
            head=tcr(head,c);
            head=tcr(head,b);
            head=tcr(head,a);

        }
        else{
            wcr(head,c);
            wcr(head,b);
            wcr(head,a);
        }}
        else if(x=='D'){
            scanf("%d",&a);
            head=sjd(head,a);
        }
        else if(x=='R'){
            head=fz(head);
        }
        else{
            break;
        }
    }
    bianlishuchu(head);
    freeList(head);
    return 0;
}
```

尽力了，只能运行两次，不知道为什么。让 ai 帮我改了一下，如下：

```c
#include<stdio.h>
#include<stdlib.h>

struct jd {
    int data;
    struct jd* next;
};
typedef struct jd lb;

// 初始化链表
lb* inilist(){
    lb* head = (lb*)malloc(sizeof(lb));
    head->data = 0;
    head->next = NULL;
    return head;
}

// 头部插入函数
lb* tcr(lb* head, int data){
    lb* xjd = (lb*)malloc(sizeof(lb));
    xjd->data = data;
    xjd->next = head->next;  // 修正：新节点指向原第一个节点
    head->next = xjd;        // 头节点指向新节点
    return head;
}

// 尾部插入函数
lb* wcr(lb* head, int data){
    lb* xjd = (lb*)malloc(sizeof(lb));
    xjd->data = data;
    xjd->next = NULL;
    
    lb* current = head;
    while(current->next != NULL){
        current = current->next;
    }
    current->next = xjd;
    return head;  // 添加返回值
}

// 删除第l个节点（从1开始计数）
lb* sjd(lb* head, int l){
    if(l < 1) return head;
    
    if(l == 1){
        if(head->next != NULL){
            lb* temp = head->next;
            head->next = temp->next;
            free(temp);
        }
        return head;
    }
    
    lb* current = head;
    int count = 0;
    while (current != NULL && count < l-1) {
        current = current->next;
        count++;
    }
    
    if (current != NULL && current->next != NULL) {
        lb* temp = current->next;
        current->next = temp->next;
        free(temp);
    }
    return head;
}

// 反转链表
lb* fz(lb* head) {
    if(head == NULL || head->next == NULL) return head;
    
    lb* prev = NULL;
    lb* current = head->next;  // 跳过头节点
    lb* next = NULL;
    
    while (current != NULL) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    
    head->next = prev;  // 头节点指向新的第一个节点
    return head;
}

// 遍历输出
void bianlishuchu(lb* head){  // 添加返回类型void
    lb* current = head->next;  // 跳过头节点
    while (current != NULL) {
        printf("%d", current->data);
        if (current->next != NULL) {
            printf(" -> ");
        }
        current = current->next;
    }
    printf(" -> NULL\n");
}

// 释放链表
void freeList(lb* head) {  // 添加返回类型void
    lb* current = head;
    while (current != NULL) {
        lb* temp = current;
        current = current->next;
        free(temp);
    }
}

int main(){
    lb* head = inilist();
    char x;
    int a, b, c;
    
    while(1){
        // 读取操作类型
        scanf(" %c", &x);  // 注意空格，跳过换行符
        
        if(x == 'H' || x == 'T'){  // 使用单引号比较字符
            scanf("%d %d %d", &a, &b, &c);
            if(x == 'H'){
                head = tcr(head, a);
                head = tcr(head, b);
                head = tcr(head, c);
            }
            else if(x == 'T'){
                head = wcr(head, a);
                head = wcr(head, b);
                head = wcr(head, c);
            }
        }
        else if(x == 'D'){
            scanf("%d", &a);
            head = sjd(head, a);
        }
        else if(x == 'R'){
            head = fz(head);
        }
        else{
            break;
        }
        
        // 每次操作后显示当前链表状态（可选）
        printf("当前链表: ");
        bianlishuchu(head);
    }
    
    printf("最终链表: ");
    bianlishuchu(head);
    freeList(head);
    return 0;
}
```

得到111101111221112112122。

## Part 2.一栈到底


