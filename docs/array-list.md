# 顺序表

### 什么是顺序表
顺序表是在计算机内存中以数组的形式保存的线性表，是指用一组地址连续的存储单元依次存储数据元素的线性结构，使得线性表中在逻辑结构上相邻的数据元素存储在相邻的物理存储单元中，即通过数据元素物理存储的相邻关系来反映数据元素之间逻辑上的相邻关系。

#### 顺序表是如何实现随机存取元素的，观察下面代码
``` c
#include <stdio.h>

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    for (int i = 0, size = sizeof(arr) / sizeof(arr[0]); i < size; ++i) {
        printf("&arr[%d] = %p\n", i, &arr[i]);
    }
    return 0;
}
```
输出结果:
```
&arr[0] = 0x7ffee5ec9620
&arr[1] = 0x7ffee5ec9624
&arr[2] = 0x7ffee5ec9628
&arr[3] = 0x7ffee5ec962c
&arr[4] = 0x7ffee5ec9630
```
观察发现相领元素之间的地址相差的地址是4，因为 `int` 类型占用4个字节。如果把 `int` 类型改为 `char` 的结果如下:

``` c
#include <stdio.h>

int main() {
    char arr[] = {'A', 'B', 'C', 'D', 'E'};
    for (int i = 0, size = sizeof(arr) / sizeof(arr[0]); i < size; ++i) {
        printf("&arr[%d] = %p\n", i, &arr[i]);
    }
    return 0;
}
```
输出结果:
```
&arr[0] = 0x7ffee57bd637
&arr[1] = 0x7ffee57bd638
&arr[2] = 0x7ffee57bd639
&arr[3] = 0x7ffee57bd63a
&arr[4] = 0x7ffee57bd63b
```
因为顺序表中的元素地址下表都是响铃的，所以顺序表可以实现随机存取操作，时间复杂度为 `O(n)`。

顺序表内存地址的计算，假设第一个元素的地址是 `loc(a1)`，每个元素所占的字节素为 `c`，则第 `i` 个元素的地址为: `loc(ai) = loc(a1) + (i - c) * c`

### 顺序表抽象数据类型
``` c
#define INIT_CAPACITY 100
#define INCREMENT 10
typedef int ElemType;
typedef struct ArrayList {
    ElemType *base;
    int size;
    int capacity;
} ArrayList;
```

### 顺序表的基本操作

顺序表的初始化
``` c
void init(ArrayList *plist) {
    plist->base = malloc(sizeof(ElemType) * INIT_CAPACITY);
    plist->size = 0;
    plist->capacity = INIT_CAPACITY;
}
```

判断顺序表是否空，时间复杂度: `O(1)`
``` c
bool isEmpty(ArrayList *pList) {
    return pList->size == 0;
}
```

获取指定位置的元素(下标)，时间复杂度: `O(1)`
``` c
ElemType get(ArrayList *pList, int index) {
    if (index < 0 || index > pList->size - 1) {
        perror("invalid get index");
    }
    return pList->base[index];
}
```

顺序表特定下标插入元素，时间复杂度: `O(n)`
算法思路如下:
* 判断插入的下标是否合法，如果非法则结束函数调用
* 判断是否顺序表已经满了，如果满了则扩容，然后在插入
* 从顺序表最后一个元素开始，依次把元素往后移动到下一个位置，截止到插入下标位置为止(包括插入下标的元素)
* 把代插入的元素存放到下标 `index` 的位置
* 顺序表长度 `+1`
* 返回1表示插入成功

``` c
bool insertNth(ArrayList *plist, int index, ElemType element) {
    if (index < 0 || index > plist->size) {
        perror("invalid insertion index");
        return false;
    }
    if (plist->size == plist->capacity) {
        growCapacity(plist);
    }
    for (int i = plist->size - 1; i >= index; --i) {
        plist->base[i + 1] = plist->base[i];
    }
    plist->base[index] = element;
    plist->size++;
    return true;
}
```

删除特定下标的元素，时间复杂度 `O(n)`，算法思路如下
* 判断删除下标是否合法
* 保存要删除的元素
* 从删除位置开始逐个把后面的元素往前移动一个位置。
* 顺序表长度 `-1`
* 返回 `1` 表示成功

``` java
ElemType deleteNth(ArrayList *plist, int index) {
    if (index < 0 || index > plist->size - 1) {
        perror("invalid deletion index");
    }

    ElemType deletedElem = plist->base[index];
    for (int i = index; i < plist->size - 1; i++) {
        plist->base[i] = plist->base[i + 1];
    }
    plist->size--;
    return deletedElem;
}
```

修改对应下标的元素，时间复杂度: `O(1)`，算法思路如下
* 判断下标是否合法
* 保存原始的数据
* 修改对应下标的元素
* 返回修改前的数据

``` c
ElemType set(ArrayList *plist, int index, ElemType newValue) {
    if (index < 0 || index > plist->size - 1) {
        perror("invalid update index");
    }
    ElemType oldValue = *(plist->base + index);
    plist->base[index] = newValue;
    return oldValue;
}
```

获取顺序表的元素个数，时间复杂度: `O(n)`,算法思路如下
* 直接返回 size 属性即可
``` c
int size(ArrayList *pList) {
    return pList->size;
}
```

遍历顺序表的元素，时间复杂度: `O(n)`，算法思路如下
* 从数组下标 `0` 的位置遍历到最后一个元素结束
```
void travel(ArrayList *pList) {
    for (int i = 0; i < pList->size; i++) {
        printf("%d\t", pList->base[i]);
    }
    printf("\n");
}
```

顺序表的扩容: 思路新建一片空间，把原有的数据复制过去。
``` c
void growCapacity(ArrayList *plist) {
    int newCapacity = (plist->capacity + INCREMENT);
    ElemType *newBase = realloc(plist->base,
                                sizeof(ElemType) * newCapacity);
    if (newBase != NULL) {
        plist->base = newBase;
        plist->capacity = newCapacity;
    } else {
        perror("stackoverflow");
    }
}
```