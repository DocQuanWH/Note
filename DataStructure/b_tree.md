# B树
**B树（B-Tree）**和**红黑树**，**AVL树**一样都是**平衡查找树**，其执行查找、顺序读取、插入和删除操作的**时间复杂度为O(logn)**。由于B树和B树的变种在**降低磁盘I/O操作次数**方面表现优异，常用于设计文件系统和数据库。

使用**阶**来定义B树的节点结构，**阶**表示节点所包含的**子节点最大数量**。比如，一棵**阶为3的B树**，其节点最多存放**2个元素**，最多拥有**3**个子节点。

## 1. 2-3树
**2-3树**相当于**3阶的B树**，其满足如下性质：
* 每个节点包含1个或2个元素（关键字，Key）：
    - 包含**1个元素**的节点最多**2个子节点**
    - 包含**2个元素**的节点最多**3个子节点**
* 所有**叶子节点**都拥有**相同的深度**

### 2-3树插入操作
1. 定位**元素X**应被插入的**叶子节点位置**
2. 如果叶子节点此时包含**元素个数为1**，则**直接**插入到该叶子节点中，结束插入操作
3. 如果叶子节点此时包含**元素个数为2**，则**暂时**插入到该叶子节点中。然后将**中间**的元素**上溯**到其父节点，并将该节点分裂成两个子节点。
4. 重复2和3操作直至根节点，具体操作如下图所示：

![2-3树插入操作](https://github.com/leechengpeng/Note/blob/master/Resources/Images/2_3_tree1.gif)
![2-3树插入操作](https://github.com/leechengpeng/Note/blob/master/Resources/Images/2_3_tree2.gif)

## 2. B树的定义
**2-3树**实际上是一棵阶为3的B树，也是最简单的B树。一棵m阶的B树，满足以下条件：
* 每个节点容纳的子节点大小范围为[m/2（取上整）, m]
* 拥有m个子节点的非叶节点将包含m-1个键值
* 所有叶节点都在**同一层**
* 如果根节点包含子节点，则至少包含2个子节点
* 插入操作同2-3树

假如每个**盘块**正好存放一个**B树的节点BNode**，那么这个**BNode**就表示这个**盘块**，而该**BNode**的子节点指针存放指向另外一块盘块的地址。一般，一个**NTFS盘块**的大小为**4KB**，可以存放大量节点的关键字和子节点指针信息，大大减少了树的深度，从而加快查询速度。每次检索的一个节点，都会进行一次IO，因此，树的高度越低，IO次数就越少，从而检索速度越快。

## 3. B+树
**B+树**是**B树**的变种，**B+树**比**B树**更适合实际应用中操作系统的文件索引和数据库索引，其特点如下：
* **内部节点**仅具有**索引作用**，只存放元素的关键字
* **叶子节点**存放元素所有信息，并且**所有叶节点可以构成一个有序链表**（前一个叶子节点存放了指向后一个叶子节点的指针）

**B+树**相对于**B树**有如下优点：
* **磁盘IO效率更高**：内部节点仅存放元素的关键字，不存放指向元素的指针，因此**盘块**能存放更过的索引信息，能进一步降低树的高度，IO次数也更少。
* **B+树检索效率更加稳定**：内部节点仅包含索引，所有命中都是在叶子节点进行，根节点到叶节点的路径长短一致，从而检索每个元素效率都相当

**B树**提高了磁盘IO的效率，但没有解决元素遍历效率问题，而**B+树**通过叶节点的连接，能有效的遍历所有元素，解决了**B**的遍历问题。**B+树**支持范围查询而**B树**不支持或效率低下。

参考文章: 
1. [B 树、B+ 树、B\* 树](http://www.cnblogs.com/Bob-FD/archive/2012/06/20/2556505.html)
2. [人人都是 DBA（VII）B 树和 B+ 树](http://www.cnblogs.com/gaochundong/p/btree_and_bplustree.html)
