## 2-3树
**2-3树**和**红黑树**，**AVL树**一样都是**平衡查找树**，其能够保证树的高度为O(logn)，满足如下性质：
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

## 2-4树