---
title: C语言 函数形参理解
tags:
- C语言
categories: 编程
---

C语言中的形参只有传值，传址本质上也是传值，只不过传的是地址，我们可以通过访问地址的方式进行修改原来的值。

比如

```c
void add(int a){
    a++;
}

int main(){
    int a=5;
    add(a);
    printf("%d", a);//a=5而不是6
}
```

而我们可以通过传指针的方式在add中访问main中的a，去修改main中a的值

```c
void add(int *a){
    (*a)++;
}

int main(){
    int a=5;
    add(&a);
    printf("%d", a);//a=6
}
```

上面的例子比较简单，下面这个是最近做题遇到的问题，困扰了我很久。

这个问题大概内容就是在链表把重复元素删除掉，只保留一个

我的思路就是先用一个指针固定一个元素，然后在用一个指针往后遍历，遇见和固定到的一样就删除掉就行了

```c
void LocateElem(LinkList head, int k,LinkList p){
        LinkList  q,m;
        m=p;
        q=p;
        p=p->next;
        while(p!=NULL){
            if(p->data==k){
                ListDelete(q,p);
            }
            else{
                q=p;
                p=p->next;
            }
        }
        p=m;
    }

void ListDelete(LinkList q,LinkList p){
    q->next=p->next;
    free(p);
    p=q->next;
}

int main()
{
    int n,m;
    LinkList head, p;
    scanf("%d", &m);
    while(m--){
        scanf("%d", &n);
        head=InitList(n);//构造长度为n的线性表并返回
        p=head->next;
        while(p!=NULL){
            LocateElem(head,p->data,p);
            p=p->next;
        }
        ShowLnode(head);//展示线性表
    }
    return 0;
}
```

LocateElem的作用是遍历链表，而ListDelete删除某个节点，看样子并没有问题，我传的是一个指针，应该可以删除吧，但其实这个程序运行不起来，而把LocateElem中的ListDelete(q,p)中替换为

```c
 q->next=p->next;
 free(p);
 p=q->next;
```

就可以了，这是为什么呢？

其原因就出现在了ListDelete中的p=q->next;

**这个p是ListDelete中的p，并不是LocateElem中的p，虽然说p的指向是一样的（也就是说*p是一样的，试着输出p->data看一看）但它们两个不是一个变量！**

这就充分的证明了C语言形参只传值，**变量要再拷贝一份的**

下面这个例子还能加深理解一下。

```c
typedef struct {
    int data[100001];
    int length;
}SqList;
//二分查找
int midsearch(SqList *list, int k, int l, int r ){
    int mid;
    mid=(l+r)/2;
    if(l>r){
        return -1;
    }
    else if(k==list->data[mid]){
       return mid;
    }
    else if(k<list->data[mid]){
        return midsearch(list,k,0,mid-1);//不用取地址了，已经是main中list的地址了，如过还取地址，那就成Sqlist**类型了，取了一个Sqlist指针的指针
    }
    else {
        return midsearch(list,k,mid+1,r);
    }
}

int main()
{
    SqList list;
    int n, t, k, m;
    scanf("%d", &n);
    initList(&list,n);//构造线性表
    scanf("%d", &t);
    while(t--){
        scanf("%d", &k);//查找值为k的元素，并返回位置
        m=midsearch(&list, k,0,(list.length)-1);//在主函数里要取地址
        if(m!=-1){
            printf("%d", m+1);
        }else{
        printf("No Found!");
        }
        printf("\n");
    }
    return 0;
}
```

接下来分析如何解决在链表删除元素的问题

其实也就是把ListDelete中的p地址可以传到LocateElem中，把p,q想成单纯的变量，不要思考它是指针， 

```c
ListDelete(Linklist *q, Linklist *p){
    (*q)->next=(*p)->next;//(*p)就是LocateElem中的p了，**p就是main中的p了
    free(*p);
    *p=*(q)->next;
}
```

调用ListDelete(&q,&p);就可以了，

其实上面LocateElem中还有一个地方需要修改，那就是m，我m的本意是保存p的地址，好在主函数里用，但是LocateElem中的p根本和main中的p没有关系，所以m就没用了。





