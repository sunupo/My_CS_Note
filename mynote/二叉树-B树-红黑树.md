# B树、B-树、B+树与红黑树

![img](https://csdnimg.cn/release/phoenix/template/new_img/reprint.png)

[lipfff](https://me.csdn.net/qq_35008624) 2018-08-22 17:21:32 ![img](https://csdnimg.cn/release/phoenix/template/new_img/articleReadEyes.png) 58334 ![img](https://csdnimg.cn/release/phoenix/template/new_img/tobarCollect.png) 收藏 97

分类专栏： [数据结构](https://blog.csdn.net/qq_35008624/category_7960731.html)

转载：https://blog.csdn.net/qq_17612199/article/details/50944413

## 二叉查找树（BST）：

```
二叉排序树或者是一棵空树，或者是具有下列性质的二叉树：



（1）若左子树不空，则左子树上所有结点的值均小于它的根结点的值；



（2）若右子树不空，则右子树上所有结点的值均大于它的根结点的值；



（3）左、右子树也分别为二叉排序树；



（4）没有键值相等的节点（因此，插入的时候一定是叶子节点）。



 



插入有序节点，退化成单支树



    1.查找效率最好O(logn)，最坏O(n)



    2.插入效率和查找效率相同（只插入叶子节点）



    3.删除效率最好O(logn)+O(1)->只有左子树或者右子树



            最差O(logn)+O(logn)->左子树和右子树同时存在
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11

![这里写图片描述](https://p-blog.csdn.net/images/p_blog_csdn_net/manesking/1.JPG) 
[插入删除算法](http://baike.baidu.com/view/647462.htm?fromtitle=二叉查找树&fromid=7077965&type=syn)

## 插入算法

首先执行查找算法，找出被插结点的父亲结点。 
判断被插结点是其父亲结点的左、右儿子。将被插结点作为叶子结点插入。 
若二叉树为空。则首先单独生成根结点。 
注意：新插入的结点总是叶子结点。

## 删除算法

1. 若*p结点为叶子结点，即PL(左子树)和PR(右子树)均为空树。由于删去叶子结点不破坏整棵树的结构，则可以直接删除此子结点。
2. 若*p结点只有左子树PL或右子树PR，此时只要令PL或PR直接成为其双亲结点*f的左子树（当*p是左子树）或右子树（当*p是右子树）即可，作此修改也不破坏二叉排序树的特性。
3. 若*p结点的左子树和右子树均不空。在删去*p之后，为保持其它元素之间的相对位置不变，可按中序遍历保持有序进行调整，可以有两种做法： 
   其一是令*p的左子树为*f的左/右(依*p是*f的左子树还是右子树而定)子树，*s为*p左子树的最右下的结点，而*p的右子树为*s的右子树； 
   其二是令*p的直接前驱（或直接后继）替代*p，然后再从二叉排序树中删去它的直接前驱（或直接后继）－即让*f的左子树(如果有的话)成为*p左子树的最左下结点(如果有的话)，再让*f成为*p的左右结点的父结点。 
   在二叉排序树上删除一个结点的算法如下：

```
package 二叉查找树;



 



public class Main {



 



    private static class BinnarySearchTree {



 



        private class Node {



            private Node left = null;



            private Node right = null;



            private Node parent = null;



            private Integer value = null;



 



            public Node getLeft() {



                return left;



            }



 



            public void setLeft(Node left) {



                this.left = left;



            }



 



            public Node getRight() {



                return right;



            }



 



            public void setRight(Node right) {



                this.right = right;



            }



 



            public Integer getValue() {



                return value;



            }



 



            public void setValue(Integer value) {



                this.value = value;



            }



 



            public Node getParent() {



                return parent;



            }



 



            public void setParent(Node parent) {



                this.parent = parent;



            }



 



            public Node(Node left, Node right, Node parent, Integer value) {



                super();



                this.left = left;



                this.right = right;



                this.parent = parent;



                this.value = value;



            }



 



        }



 



        private Node root = null;



 



        public void insert(Integer value) throws Exception {



            insertNode(new Node(null, null, null, value));



        }



 



        private void insertNode(Node node) throws Exception {



            Node pre = null;



            Node x = this.root;



            while (x != null) {



                pre = x;



                if (x.getValue() > node.getValue()) {



                    x = x.getLeft();



                } else if (x.getValue() < node.getValue()) {



                    x = x.getRight();



                } else {



                    throw new Exception("value is existed");



                }



            }



            if (pre != null) {



                if (node.getValue() < pre.getValue()) {



                    pre.setLeft(node);



                } else {



                    pre.setRight(node);



                }



                node.setParent(pre);



            } else {



                // 根节点



                this.root = node;



            }



        }



 



        public Node find(Integer value) {



            Node x = root;



            while (x != null && x.getValue() != value) {



                if (x.getValue() > value) {



                    x = x.getLeft();



                } else if (x.getValue() < value) {



                    x = x.getRight();



                }



            }



            return x;



        }



 



        public void delete(Integer value) throws Exception {



            Node x = find(value);



            if (x != null) {



                if (x.getLeft() == null && x.getRight() == null) {



                    // 叶子节点



                    x.setLeft(null);



                    x.setRight(null);



                } else if (x.getLeft() != null) {



                    // 左子树不为空



                    Node y = x.getLeft();



                    while (y.getRight() != null) {



                        y = y.getRight();



                    }



                    x.setValue(y.getValue());



                    System.out.println(y.getParent());



                    y.getParent().setRight(null);



                } else {



                    // 右子树不为空



                    Node y = x.getRight();



                    if (x.getParent() != null) {



                        y.setLeft(x.getLeft());



                        if (x.getParent().getValue() > x.getValue()) {



                            x.getParent().setLeft(y);



                        } else {



                            x.getParent().setRight(y);



                        }



                    } else {



                        // 根节点



                        this.root = y;



                    }



                }



            } else {



                throw new Exception("value is not exists");



            }



        }



 



        public void inOrder(Node node) {



            if (node != null && node.getValue() != null) {



                inOrder(node.getLeft());



                System.out.println(node.getValue());



                inOrder(node.getRight());



            }



        }



 



        public Node getRoot() {



            return root;



        }



 



        public void setRoot(Node root) {



            this.root = root;



        }



 



    }



 



    public static void main(String[] args) throws Exception {



        BinnarySearchTree bt = new BinnarySearchTree();



        bt.insert(7);



        bt.insert(11);



        bt.insert(8);



        bt.insert(12);



        bt.insert(9);



        bt.insert(13);



        bt.insert(10);



        bt.inOrder(bt.getRoot());



        System.out.println("----------------------");



        bt.delete(11);



        bt.inOrder(bt.getRoot());



    }



}
```

-  
-  

## 平衡二叉查找树

[平衡二叉树参考1](http://blog.csdn.net/liyong199012/article/details/29219261) 
[平衡二叉树参考2](http://blog.csdn.net/niteip/article/details/11840691) 
**平衡二叉搜索树**：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。常用算法有红黑树、AVL、Treap、伸展树等。在平衡二叉搜索树中，我们可以看到，其高度一般都良好地维持在O（log2n），大大降低了操作的时间复杂度。

调整平衡的基本思想： 
当在二叉排序树中插入一个节点时，首先检查是否因插入而破坏了平衡，若破坏，则找出其中的**最小不平衡二叉树**，在保持二叉排序树特性的情况下，调整最小不平衡子树中节点之间的关系，以达到新的平衡。 
所谓最小不平衡子树，**指离插入节点最近且以平衡因子的绝对值大于1的节点作为根的子树**。

先插入指定节点，记录下当前节点的信息，LH，EH或者RH。 
\1. 若左子树高LH，查看其左子树根节点的信息，若是LH，则一次右旋；若是RH，则一次左旋+一次右旋 
\2. 若右子树高RH，查看右子树根节点的信息，若是RH，则一次左旋；若是LH，则一次右旋+一次左旋 
\3. 调整改变的节点信息

追求绝对的高度平衡，随着树的高度的增加，动态插入和删除的代价也随之增加

## 红黑树

[红黑树参考](http://www.cnblogs.com/daoluanxiaozi/p/3340382.html) 
红黑树（Red Black Tree） 是一种自平衡二叉查找树 
红黑树和AVL树类似，都是在进行插入和删除操作时通过特定操作保持二叉查找树的平衡，从而获得较高的查找性能。 
二叉平衡树的严格平衡策略以牺牲建立查找结构(插入，删除操作)的代价，换来了稳定的O(logN) 的查找时间复杂度 
它虽然是复杂的，但它的最坏情况运行时间也是非常良好的，并且在实践中是高效的： 它可以在O(log n)时间内做查找，插入和删除，这里的n 是树中元素的数目。

```
(1) 每个节点或者是黑色，或者是红色。



(2) 根节点是黑色。



(3) 每个叶子节点是黑色。 [注意：这里叶子节点，是指为空的叶子节点！]



(4) 如果一个节点是红色的，则它的子节点必须是黑色的。



(5) 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。
```

RBT 的操作代价分析： 
(1) 查找代价：由于红黑树的性质(**最长路径长度不超过最短路径长度的2倍**)，可以说明红黑树虽然不像AVL一样是严格平衡的，但平衡性能还是要比BST要好。**其查找代价基本维持在O(logN)左右，但在最差情况下(最长路径是最短路径的2倍少1)，比AVL要略逊色一点**。 
(2) 插入代价：RBT插入结点时，**需要旋转操作和变色操作**。但由于只需要保证RBT基本平衡就可以了。**因此插入结点最多只需要2次旋转，这一点和AVL的插入操作一样**。虽然变色操作需要O(logN)，但是变色操作十分简单，代价很小。 
(3) 删除代价：RBT的删除操作代价要比AVL要好的多，删除一个结点最多只需要3次旋转操作。 
RBT 效率总结 : 查找 效率最好情况下时间复杂度为O(logN)，但在最坏情况下比AVL要差一些，**但也远远好于BST**。 
插入和删除操作改变树的平衡性的概率要远远小于AVL（RBT不是高度平衡的）。**因此需要的旋转操作的可能性要小，而且一旦需要旋转，插入一个结点最多只需要旋转2次，删除最多只需要旋转3次(小于AVL的删除操作所需要的旋转次数)**。虽然变色操作的时间复杂度在O(logN)，但是实际上，这种操作由于简单所需要的代价很小。

红黑树能够以O(log2(N))的时间复杂度进行搜索、插入、删除操作。此外,任何不平衡都会在3次旋转之内解决。这一点是AVL所不具备的。

```
插入操作：



1.插入根节点（不需要操作）



2.父节点为黑色(不需要操作)



3.父节点和兄弟节点为红色，祖父节点为黑色，只需要变色，将祖父节点递归检查（原本检查自己）



4.父节点为红色，兄弟节点为黑色，祖父节点为红色，先两次旋转再调整颜色（左旋+右旋）
删除操作：



1.删除只有一个新的根节点（直接删除）



2.父节点为黑色，兄弟节点为红色（先旋转成左左，再删除）



3.父节点为黑色，兄弟节点为黑色（先将兄弟节点换成红色，变成情况2）



4.父节点为红色，自己和兄弟节点为黑色（将父节点变成黑色，兄弟节点变成红色，变成情况2）



5.兄弟节点为黑色，兄弟节点左子树根节点为红色（交换颜色，旋转成为左左）



6.情况2和情况5，调整性质5（将N删掉，用子节点顶替，若子节点为红色，则重绘为黑色）
```

![这里写图片描述](https://img-blog.csdn.net/20160330112859012) 
[参考](http://www.cnblogs.com/skywang12345/p/3624343.html)

## B树（多叉查找树）：

1、根结点至少有两个子女； 
2、每个非根节点所包含的关键字个数 j 满足：m/2 - 1 <= j <= m - 1； 
3、除根结点以外的所有结点（不包括叶子结点）的度数正好是关键字总数加1，故内部子树个数 k 满足：m/2<= k <= m ； 
4、所有的叶子结点都位于同一层。 
![这里写图片描述](https://img-blog.csdn.net/20160321105919538)

## B-树

![这里写图片描述](https://p-blog.csdn.net/images/p_blog_csdn_net/manesking/4.JPG) 
1、关键字集合分布在整棵树中； 
2、任何一个关键字出现且只出现在一个结点中； 
3、搜索有可能在非叶子结点结束； 
4、其搜索性能等价于在关键字全集内做一次二分查找； 
5、自动层次控制； 
**m阶B-树** 
1) 树中每个结点至多有m个孩子； 
2) 除根结点和叶子结点外，其它每个结点至少有[m/2]个孩子； 
3) 若根结点不是叶子结点，则至少有2个孩子； 
4) 所有叶子结点都出现在同一层，叶子结点不包含任何关键字信息(可以看做是外部接点或查询失败的接点，实际上这些结点不存在，指向这些结点的指针都为null)； 
5) 每个非终端结点中包含有n个关键字信息： (n，A0，K1，A1，K2，A2，……，Kn，An)。其中， 
a) Ki (i=1…n)为关键字，且关键字按顺序排序Ki < K(i-1)。 
b) Ai为指向子树根的接点，且指针A(i-1)指向子树种所有结点的关键字均小于Ki，但都大于K(i-1)。 
c) 关键字的个数n必须满足： [m/2]-1 <= n <= m-1 
**建立** 
由于B~树结点中的关键字个数必须>=[m/2]-1。因此和平衡二叉树不同，每一次插入一个关键字并不是在树中添加一个结点，而是首先在最低层的某个非终端结点中添加一个关键字，若该结点的关键字个数不超过m-1，则插入完成。否则，要产生结点的”分裂” 。 
**外存** 
我们现在把整棵树构造在磁盘中，假如每个盘块可以正好存放一个B~树的结点（正好存放2个文件名）。那么一个BTNode结点就代表一个盘块，而子树指针就是存放另外一个盘块 （详细见《外部存储器—磁盘 》）的地址。 
现在我们模拟查找文件29的过程： 
(1) 根据根结点指针找到文件目录的根磁盘块1，将其中的信息导入内存。【磁盘IO操作1次】 
(2) 此时内存中有两个文件名17，35和三个存储其他磁盘页面地址的数据。根据算法我们发现17<29<35，因此我们找到指针p2。 
(3) 根据p2指针，我们定位到磁盘块3，并将其中的信息导入内存。【磁盘IO操作2次】 
(4) 此时内存中有两个文件名26，30和三个存储其他磁盘页面地址的数据。根据算法我们发现26<29<30，因此我们找到指针p2。 
(5) 根据p2指针，我们定位到磁盘块8，并将其中的信息导入内存。【磁盘IO操作3次】 
(6) 此时内存中有两个文件名28，29。根据算法我们查找到文件29，并定位了该文件内存的磁盘地址。

```
  分析一下上面的过程，我们发现需要3次磁盘IO操作和3次内存查找操作。关于内存中的文件名查找，由于是一个有序表结构，可以利用折半查找提高效率。至于3次磁盘IO操作时影响整个B~树查找效率的决定因素。



  当然，如果我们使用平衡二叉树的磁盘存储结构来进行查找，磁盘IO操作最少4次，最多5次。而且文件越多，B~树比平衡二叉树所用的磁盘IO操作次数将越少，效率也越高。
```

## B+树：

B+ 树是一种树数据结构，是一个n叉树，每个节点通常有多个孩子，一颗B+树包含根节点、内部节点和叶子节点。根节点可能是一个叶子节点，也可能是一个包含两个或两个以上孩子节点的节点。 
B+ 树通常用于数据库和操作系统的文件系统中。 
NTFS, ReiserFS, NSS, XFS, JFS, ReFS 和BFS等文件系统都在使用B+树作为元数据索引。 
B+ 树的特点是能够保持数据稳定有序，其插入与修改拥有较稳定的对数时间复杂度。 
B+ 树元素自底向上插入。 
![这里写图片描述](http://hxraid.iteye.com/upload/picture/pic/56353/0988ccff-f845-36b3-804d-e57b93e1c5e6.jpg) 
所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大的顺序链接。(而B 树的叶子节点并没有包括全部需要查找的信息) 
所有的非终端结点可以看成是索引部分，结点中仅含有其子树根结点中最大（或最小）关键字。(而B 树的非终节点也包含需要查找的有效信息)

```
1) 有n棵子树的结点中含有n个关键字；  (B~树是n棵子树有n+1个关键字)



2) 所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大的顺序链接。 (B~树的叶子节点并没有包括全部需要查找的信息)



3) 所有的非终端结点可以看成是索引部分，结点中仅含有其子树根结点中最大（或最小）关键字。 (B~树的非终节点也包含需要查找的有效信息)



 



B+树的有效内容均在叶子节点，B-树的有效内容不全在叶子节点上



B+树的头指针有两个，一个指向根节点，另一个指向关键字最小的元素，因此B+树有两种遍历的方式：



1.从根节点开始随机查询



2.从最小关键词顺序查询
```

## B*树

![这里写图片描述](https://p-blog.csdn.net/images/p_blog_csdn_net/manesking/6.JPG)

## 为什么说B+树比B 树更适合实际应用中操作系统的文件索引和数据库索引？

**B+树的磁盘读写代价更低** 
B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B 树更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说IO读写次数也就降低了。 
举个例子，假设磁盘中的一个盘块容纳16bytes，而一个关键字2bytes，一个关键字具体信息指针2bytes。一棵9阶B-tree(**一个结点最多8个关键字**)的内部结点需要2个盘快。而B+树内部结点只需要1个盘快。当需要把内部结点读入内存中的时候，B 树就比B+树多一次盘块查找时间(在磁盘中就是盘片旋转的时间)。

**B+树的查询效率更加稳定** 
由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

## B+树和B-树的区别

B+树 
性质：B+树是B-树的变体，也是一种多路搜索树： 
　　

```
1.其定义基本与B-树同，除了：



2.非叶子结点的子树指针与关键字个数相同；



3.非叶子结点的子树指针P[i]，指向关键字值属于[K[i], K[i+1])的子树（B-树是开区间）；



4.为所有叶子结点增加一个链指针；



5.所有关键字都在叶子结点出现；
```

B-树 
性质：是一种多路搜索树（并不是二叉的）： 
　　

```
1.定义任意非叶子结点最多只有M个儿子；且M>2；



2.根结点的儿子数为[2, M]；



3.除根结点以外的非叶子结点的儿子数为[M/2, M]；



4.每个结点存放至少M/2-1（取上整）和至多M-1个关键字；（至少2个关键字）



5.非叶子结点的关键字个数=指向儿子的指针个数-1；



6.非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] < K[i+1]；



7.非叶子结点的指针：P[1], P[2], …, P[M]；其中P[1]指向关键字小于K[1]的子树，P[M]指向关键字大于K[M-1]的子树，其它P[i]指向关键字属于(K[i-1], K[i])的子树；



8.所有叶子结点位于同一层；
```

-  

## **红黑树和平衡二叉树区别如下：** 

1、红黑树放弃了追求完全平衡，追求大致平衡，在与平衡二叉树的时间复杂度相差不大的情况下，保证每次插入最多只需要三次旋转就能达到平衡，实现起来也更为简单。 
2、平衡二叉树追求绝对平衡，条件比较苛刻，实现起来比较麻烦，每次插入新节点之后需要旋转的次数不能预知。

## 小结

B-树：多路搜索树，每个结点存储M/2到M个关键字，非叶子结点存储指向关键字范围的子结点；所有关键字在整颗树中出现，且只出现一次，非叶子结点可以命中；B+树：在B-树基础上，为叶子结点增加链表指针，所有关键字都在叶子结点中出现，非叶子结点作为叶子结点的索引；B+树总是到叶子结点才命中；B*树：在B+树基础上，为非叶子结点也增加链表指针，将结点的最低利用率从1/2提高到2/3；

# 树的旋转

例如对于被破坏平衡的节点 a 来说：

| 插入方式 | 描述                                                  | 旋转方式     |
| -------- | ----------------------------------------------------- | ------------ |
| LL       | 在a的**左子树**根节点的**左子树**上插入节点而破坏平衡 | 右旋转       |
| RR       | 在a的**右子树**根节点的**右子树**上插入节点而破坏平衡 | 左旋转       |
| LR       | 在a的**左子树**根节点的**右子树**上插入节点而破坏平衡 | 先左旋后右旋 |
| RL       | 在a的**右子树**根节点的**左子树**上插入节点而破坏平衡 | 先右旋后左旋 |



作者：喵了个呜s
链接：https://www.jianshu.com/p/6988699625d5
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# 二叉树的遍历

~~~java
package com.sun.util;

import java.lang.reflect.Method;
import java.util.*;

class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int x){
        val=x;
    }
    public TreeNode getLeft() {
        return left;
    }
    public void setLeft(TreeNode left) {
        this.left = left;
    }
    public TreeNode getRight() {
        return right;
    }
    public void setRight(TreeNode right) {
        this.right = right;
    }
    
}
public class Solution {
    
    public TreeNode[] initTree(){
        TreeNode[] tree=new TreeNode[11];
        for(int i=0;i<tree.length;i++){
            tree[i]=new TreeNode(i+1);
        }
        tree[0].left=tree[1];
        tree[0].right=tree[2];
        tree[1].left=tree[3];
        tree[1].right=tree[4];
        tree[2].left=tree[5];
        tree[2].right=tree[6];
        
//        tree[4].left=tree[7];
//        tree[3].right=tree[8];
//        
//        tree[8].left=tree[9];
//        tree[8].right=tree[10];
        
        tree[4].left=tree[7];
        tree[4].right=tree[8];
        
        tree[8].left=tree[9];
        tree[8].right=tree[10];
        return tree;
        
    }
    
//    一\递归遍历
//    1.先根遍历
    public void preOrderRecursion(TreeNode root, List<Integer> nodeList){
        if (root!=null){
            nodeList.add(root.val);
            preOrderRecursion(root.left, nodeList);
            preOrderRecursion(root.right,nodeList);
        }
        
    }
//    2.中根遍历
    public void inOrderRecursion(TreeNode root, List<Integer> nodeList){
        if (root!=null){
            inOrderRecursion(root.left, nodeList);
            nodeList.add(root.val);
            inOrderRecursion(root.right,nodeList);
        }
        
    }
//    3.后根遍历
    public void afterOrderRecursion(TreeNode root, List<Integer> nodeList){
        if (root!=null){
            afterOrderRecursion(root.left, nodeList);
            afterOrderRecursion(root.right,nodeList);
            nodeList.add(root.val);
        }
        
    }
//    二 非递归遍历(栈实现)
            
//    1.先根遍历_1
    public void preOrderStack(TreeNode root, List<Integer> nodeList){
        Stack<TreeNode> nodeStack = new Stack<TreeNode>();
        TreeNode currentNode=root;
        while(!nodeStack.isEmpty()||currentNode!=null){
            while(currentNode!=null){
                nodeList.add(currentNode.val);System.out.println(currentNode.val);
                nodeStack.push(currentNode);
                currentNode=currentNode.left;
            }
            if(!nodeStack.isEmpty()){
                currentNode = nodeStack.pop();
                currentNode = currentNode.right;
            }
        }
    }    
        
    
//    2.中根遍历_1
    public void inOrderStackOne(TreeNode root, List<Integer> nodeList){
        Stack<TreeNode> nodeStack = new Stack<TreeNode>();
        nodeStack.push(root);
        TreeNode peekNode,popNode;
        while(!nodeStack.isEmpty()){// 第一个while循环
            peekNode = nodeStack.peek();
            while(peekNode!=null){// 第二个while循环
                nodeStack.push(peekNode.left);
                peekNode=nodeStack.peek();
            }
            nodeStack.pop();
            if(!nodeStack.isEmpty()){ 
                // 这个if判断主要是因为,当最后一个节点pop出战之后,还会把最后一个节点的右孩子入站,但是值为null,
                //导致会进入第一个while循环,不会进入第二个while循环,详细解释如下:
                /*当popNode为最后一个遍历到的节点的时候, 
                nodeStack.push(popNode.right);使栈还存在一个null,栈不为空,
                可以进入第一个while循环,
                但是此时由于栈里面的元素只剩下null,所以peekNode=nodeStack.peek()=null
                不会进入的二个while循环
                当执行到nodeStack.pop()之后,此时栈为空,这个时候就应该停止算法了.如果不加if条件,则会进入if体内,再次调用nodeStack.pop()出错
                */
                popNode = nodeStack.pop();
                nodeList.add(popNode.val);
                nodeStack.push(popNode.right);
            }
        }
    }
    
//    2.中根遍历_2
    public void inOrderStackTwo(TreeNode root, List<Integer> nodeList){
        Stack<TreeNode> nodeStack = new Stack<TreeNode>();
        TreeNode currentNode=root;
        while(!nodeStack.isEmpty()||currentNode!=null){
            while(currentNode!=null){
                nodeStack.push(currentNode);
                currentNode=currentNode.left;
            }
            if(!nodeStack.isEmpty()){
                currentNode = nodeStack.pop();
                nodeList.add(currentNode.val);//System.out.println(currentNode);
                currentNode = currentNode.right;
            }
        }        
    }    
//     3.后根遍历
    public void afterOrderStack(TreeNode root, List<Integer> nodeList){
        Stack<TreeNode> nodeStack = new Stack<TreeNode>();
        TreeNode currentNode=root,peekNode,popNode,rightNode;
        while(!nodeStack.isEmpty()|| currentNode!=null ){// 第一个while循环
            while(currentNode!=null){// 第二个while循环
                nodeStack.push(currentNode);
                currentNode = currentNode.left;
            }
            boolean flag=true;// 右孩子没有访问过的标志
            rightNode=null;
            while(!nodeStack.isEmpty()&&flag) {
                peekNode = nodeStack.peek();
                if (peekNode.right==rightNode) {
                    popNode = nodeStack.pop();
                    nodeList.add(popNode.val);//System.out.println(popNode.val);
                    if(nodeStack.isEmpty()){
                        return;
                    }else{
                        rightNode=popNode;
                    }
                }else{
                    currentNode = peekNode.right;
                    flag=false;
                }
            }
        }
    }
    public static void main(String[] args){
        
        Solution answer = new Solution();
        TreeNode[] tree = answer.initTree();
        List<Integer> nodeList = new ArrayList<Integer>();
        
        System.out.println("递归算法:");
        System.out.print("\t先根遍历\t");
        answer.preOrderRecursion(tree[0], nodeList);
        nodeList.forEach(System.out::print);
//        
        System.out.print("\n\t中根遍历\t");
        nodeList.clear();
        answer.inOrderRecursion(tree[0], nodeList);
        nodeList.forEach(System.out::print);
//        
        System.out.print("\n\t后根遍历\t");
        nodeList.clear();
        answer.afterOrderRecursion(tree[0], nodeList);
        nodeList.forEach(System.out::print);
//        
        System.out.print("\n非递归算法:");//        
        System.out.print("\n\t先根遍历\t");
        nodeList.clear();
        answer.preOrderStack(tree[0], nodeList);
        nodeList.forEach(System.out::print);

        System.out.print("\n\t中根遍历1\t");
        nodeList.clear();
        answer.inOrderStackOne(tree[0], nodeList);
        nodeList.forEach(System.out::print);
        
        System.out.print("\n\t中根遍历2\t");
        nodeList.clear();
        answer.inOrderStackTwo(tree[0], nodeList);
        nodeList.forEach(System.out::print);

        System.out.print("\n\t后根遍历\t");
        nodeList.clear();
        answer.afterOrderStack(tree[0], nodeList);
        nodeList.forEach(System.out::print);
        
    }

}
~~~

