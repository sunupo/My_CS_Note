# Hash索引和BTree索引区别

[![img](https://upload.jianshu.io/users/upload_avatars/4424012/8c2875c0-56bf-4bbf-8b26-4b21dd71854f.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/efde241036b3)



- Hash仅支持=、>、>=、<、<=、between。BTree可以支持like模糊查询

- 索引是帮助mysql获取数据的数据结构。最常见的索引是Btree索引和Hash索引。

- 不同的引擎对于索引有不同的支持：Innodb和MyISAM默认的索引是Btree索引；而Mermory默认的索引是Hash索引。

我们在mysql中常用两种索引算法BTree和Hash，两种算法检索方式不一样，对查询的作用也不一样。

# 一、BTree

BTree索引是最常用的mysql数据库索引算法，因为它不仅可以被用在**=,>,>=,<,<=**和between这些比较操作符上，而且还可以用于**like**操作符，只要它的查询条件是一个不以通配符开头的常量，例如：
select * from user where name like ‘jack%’;
select * from user where name like ‘jac%k%’;
**如果以通配符开头，或者没有使用常量，则<u>不会使用索引</u>**，例如：
select * from user where name like ‘**%jack**’;
select * from user where name like simply_name;

# 二、Hash

Hash索引只能用于**对等比较，例如=,<=>（相当于=）操作符**。由于是一次定位数据，不像BTree索引需要从根节点到枝节点，最后才能访问到页节点这样多次IO访问，所以检索效率远高于BTree索引。
但为什么我们使用BTree比使用Hash多呢？主要**Hash**本身由于其特殊性，也带来了很多限制和**弊端**：

1. - Hash索引仅仅能满足**“=”,“IN”,“<=>”查询**，**不能使用范围查询**。
2. - 联合索引中，Hash索引**不能利用部分索引键**查询。
     对于联合索引中的多个列，Hash是要么全部使用，要么全部不使用，并不支持BTree支持的联合索引的最优前缀，也就是联合索引的前面一个或几个索引键进行查询时，Hash索引无法被利用。
3. - Hash索引**无法避免数据的排序**操作
     由于Hash索引中存放的是经过Hash计算之后的Hash值，而且Hash值的大小关系并不一定和Hash运算前的键值完全一样，所以数据库无法利用索引的数据来避免任何排序运算。
4. - Hash索引任何时候都**不能避免表扫描**
     Hash索引是将索引键通过Hash运算之后，将Hash运算结果的Hash值和所对应的行指针信息存放于一个Hash表中，由于不同索引键存在相同Hash值，所以即使满足某个Hash键值的数据的记录条数，也无法从Hash索引中直接完成查询，还是要通过访问表中的实际数据进行比较，并得到相应的结果。
5. - Hash索引<u>遇到大量Hash值相等的情况后性能并不一定会比BTree高</u>
     对于选择性比较低的索引键，如果创建Hash索引，那么将会存在大量记录指针信息存于同一个Hash值相关联。这样要定位某一条记录时就会非常麻烦，会浪费多次表数据访问，而造成整体性能底下。

- hash索引查找数据基本上能一次定位数据，当然有大量碰撞的话性能也会下降。而btree索引就得在节点上挨着查找了，很明显在数据精确查找方面hash索引的效率是要高于btree的；
- 那么不精确查找呢，也很明显，因为hash算法是基于等值计算的，所以对于“like”等范围查找hash索引无效，不支持；
- 于btree支持的[联合索引](https://www.baidu.com/s?wd=联合索引&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的最优前缀，hash也是无法支持的，[联合索引](https://www.baidu.com/s?wd=联合索引&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)中的字段要么全用要么全不用。提起最优前缀居然都泛起迷糊了，看来有时候放空得太厉害；
- hash不支持索引排序，索引值和计算出来的hash值大小并不一定一致。