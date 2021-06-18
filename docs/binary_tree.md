# 二叉树

### 什么是二叉树
在计算机科学中，二叉树（英语：Binary tree）是每个节点最多只有两个分支（即不存在分支度大于2的节点）的树结构。通常分支被称作“左子树”或“右子树”。二叉树的分支具有左右次序，不能随意颠倒。

### 左子树和右子树
* 左子树: 左子树是以当前结点的左孩子结点作为根节点的子树
* 右子树: 以左孩子结点作为根节点的子树

### 二叉树的五种状态

<img src="../images/binary_tree_five_types.png" width = "300" >

### 二叉树的作用
* 存放数据
* 提高查找效率
* 实现简单而相当有效的数据压缩算法
* 大型数据库等软件实现快速搜索功能

### 满二叉树

满二叉树是最后一层是叶子结点，其余结点度是2的二叉树

<img src="../images/full_binary_tree.png" width = "300" >

性质: 
* 同样深度的二叉树中满二叉树结点数量最多
* 第`i`层的结点数量为: 2<sup>i - 1</sup>
* 深度为`k`的满二叉树，结点数量为: 2<sup>k</sup>-1，叶子数为2<sup>k-1</sup>
* 满二叉树中不存在度为 1 的节点，每一个分支点中都两棵深度相同的子树，且叶子节点都在最底层
* 具有 n 个节点的满二叉树的深度为 log<sub>2</sub>(n+1)

### 完全二叉树
完全二叉树是在一棵满二叉树基础上自左向右连续增加叶子结点得到的二叉树

<img src="../images/complete_binary.svg" width = "300" >

性质:
* 叶子结点只能出现在最后两层
* 最后一层的叶子结点集中在左边连续的位置
* 除最后一层是一棵满二叉树

注意: 满二叉树一定是完全二叉树，但是完全二叉树不一定是满二叉树

### 二叉树的性质

* 性质1: 在非空二叉树的第i层上，至多有2<sup>i-1</sup>个结点(`i>=1`)
* 性质2: 在深度为K的二叉树上最多有2<sup>k</sup>-1个结点（k>=1)
* 性质3: 在任意一棵二叉树中，叶子结点的数目比度数为2的结点的数目多1,即 `n0 =  n2 + 1`
* 性质4: 具有n个结点的完全二叉树的深度为 log<sub>2</sub>n + 1，结果需要向下取整
* 性质5: 完全二叉树结点的编号方法是从上到下，从左到右，根节点为1号结点，设完全二叉树的结点数为n，某结点编号为 i
    - 若 i=1，则该结点是二叉树的根
    - 若 2*i>n，则该结点无左孩子，否则，其左孩子结点是编号为 2*i 的结点
    - 若 2*i+1>n，则该结点无右孩子结点，否则，其右孩子结点是编号为2*i+1 的结点

### 二叉树的遍历
在计算机科学里，树的遍历（也称为树的搜索）是图的遍历的一种，指的是按照某种规则，不重复地访问某种树的所有节点的过程。具体的访问操作可能是检查节点的值、更新节点的值等。不同的遍历方式，其访问节点的顺序是不一样的。

#### 先序遍历(Pre-Order Traversal)
1. 访问跟节点
2. 遍历左子树
3. 遍历右子树

<img src="../images/binary_tree_preorder.png" width = "300" >

深度优先遍历 - 前序遍历：F, B, A, D, C, E, G, I, H.

<img src="../images/binary-tree-1-pre-order-small.gif" width = "300" >

代码实现
``` c
void preOrder(Node *root) {
    if (root != NULL) {
        printf("%d", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}
```

#### 中序遍历(In-Order Traversal)
1. 遍历左子树
2. 访问跟节点
3. 遍历右子树

<img src="../images/binary_tree_inorder.png" width = "300" >

<img src="../images/binary-tree-1-order-small.gif" width = "300" >


代码实现
``` c
void inOrder(Node *root) {
    if (root != NULL) {
        inOrder(root->left);
        printf("%d", root->data);
        inOrder(root->right);
    }
}
```

#### 后序遍历(Post-Order Traversal)
1. 遍历左子树
2. 遍历右子树
3. 访问跟节点

<img src="../images/binary_tree_postorder.png" width = "300" >

<img src="../images/binary-tree-1-post-order-small.gif" width = "300" >

代码实现
``` c
void postOrder(Node *root) {
    if (root != NULL) {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d", root->data);
    }
}
```

#### 广度优先遍历
和深度优先遍历不同，广度优先遍历会先访问离根节点最近的节点。二叉树的广度优先遍历又称按层次遍历。算法借助队列实现。



### 二叉树的顺序存储结构

#### 完全二叉树(满二叉树)的顺序存储结构
使用数组依次从上到下、自左向右存储所有的结点元素

示例完全二叉树

<img src="../images/binary_tree_numober.png" width = "300" >

<img src="../images/array_structure_of_binary_tree.png"/>

我们还可以根据满二叉树的顺序存储结构恢复二叉树


#### 存储非完全二叉树

* 添加虚构结点构造完全二叉树
<img src="../images/not_complete_binary_tree.png" width = "300"/>

* 由上往下，由左往右存放，虚设结点表示为空

<img src="../images/array_structure_of_binary_tree_02.png" width = "300"/>


#### 顺序存储存在的问题

* 浪费存储空间，存储密度低

<img src="../images/not_complete_binary_tree_02.png " width = "300"/>

对应的数组示意图

<img src="../images/array_structure_of_binary_tree_03.png" width = "300">

### 二叉树的链式存储(二叉链表)
因为二叉树中只存在度为`0`, `1`, `2` 的结点，所以我们可以把二叉树的结点类型定义为包含 数据域、左孩子指针、右孩子指针三个成员属性。

<img src="../images/tree_parent_presentation_pro_node.png" width = "300">

下图表示了二叉树的二叉链表存储结构

<img src="../images/binary_tree_presentation_01.PNG" width = "300">

二叉链表节点定义
``` c
typedef int ElemType; /* 假设数据类型为char */
typedef struct Node {
    ElemType data; /* 数据域 */
    struct Node *left; /* 左孩子指针 */
    struct Node *right; /* 右孩子指针 */
} Node;
```


手动构造一颗二叉树

<img src="../images/example_binary_tree.png" width = "300">


``` c
Node *createNode(int data) {
    Node *newNode = (malloc(sizeof(Node)));
    newNode->data = data;
    return newNode;
}

void test() {
    /* see images/example_binary_tree.png */
    Node *root = createNode(1);
    Node *node2 = createNode(2);
    Node *node3 = createNode(3);
    Node *node4 = createNode(4);
    Node *node5 = createNode(5);

    root->left = node2;
    root->right = node3;

    node2->left = node4;
    node2->right = node5;

    node3->left = node3->right = NULL;
    node4->left = node4->right = NULL;
    node5->left = node5->right = NULL;

    preOrder(root); /* output: 12453 */
    printf("\n");

    inOrder(root); /* output: 42513 */
    printf("\n");

    postOrder(root); /* output:  45231 */
    printf("\n");
}
```

