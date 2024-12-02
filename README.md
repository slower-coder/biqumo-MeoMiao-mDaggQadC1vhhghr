
**大纲**


**1\.索引原理**


**2\.二叉查找树**


**3\.平衡二叉树(AVL树)**


**4\.红黑树**


**5\.B\-Tree**


**6\.B\+Tree**


**7\.Hash索引**


**8\.聚簇索引与非聚簇索引**


 


**1\.索引原理**


索引会在数据文件中(ibd文件)，通过数据页(Page)进行存储。索引可以加快检索速度，但也会降低增删改速度，索引维护需要代价。


 


MySQL默认使用B\+树结构管理索引。B\+树中的B代表平衡，B\+树是由二叉树、平衡二叉树(AVL)和B\-Tree逐步优化而来的。


 


**2\.二叉树**


**(1\)二叉树的定义**


**(2\)特殊的二叉树**


**(3\)二叉树的存储**


**(4\)二叉树的遍历**


 


**(1\)二叉树的定义**


二叉树是n个结点的有限集合：或者为空二叉树，即n\=0。或者由一个根结点和两个互不相交的被称为根的左子树和右子树组成，左子树和右子树又分别是一棵二叉树。


 


二叉树的特点是每个结点至多有两棵子树，子树分左右且次序不能颠倒。


 


二叉树是有序树，若将其左右子树颠倒，则成为另一棵不同的二叉树。


 


**(2\)特殊的二叉树**


**一.满二叉树**


一棵高度为h，且含有2^h \- 1个结点的二叉树称为满二叉树。


 


**二.完全二叉树**


高度为h、有n个结点的二叉树，当且仅当其每个结点都与高度为h的满二叉树中编号为1～n的结点一一对应时，称为完全二叉树。


 


**三.二叉排序树**


左子树上所有结点的值均小于根结点的值。


右子树上所有结点的值均大于根结点的值。


左子树和右子树又各是一棵二叉排序树。


 


**四.平衡二叉树**


树上任一结点的左子树和右子树的高度差不超过1。


 


**(3\)二叉树的存储**


**一.完全二叉树和满二叉树采用顺序存储比较合适**


顺序存储指用一组地址连续的存储单元依次存储完全二叉树上的元素，也就是将完全二叉树上编号为i的元素存储在一维数组的下标为i\-1的位置中。


 


**二.普通二叉树一般采用链式存储比较合适**


由于顺序存储的空间利用率较低，因此二叉树一般采用链式存储结构，链式存储就是用链表结点来存储二叉树中的每个结点。


 


在二叉树中，结点结构通常包括若干数据域和若干指针域。二叉链表至少包含3个域：数据域data、左指针域lchild、右指针域rchild。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/6725046ae09946e8aa0fae4fd7b868f7~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=4%2BgoQZPEJkFfNG1wej8WqCPp86M%3D)
 


**(4\)二叉树的遍历**


**一.先序遍历**


步骤1：访问根结点；


步骤2：先序遍历左子树；


步骤3：先序遍历右子树；



```
public void preOrder(BiTree t) {
    if (t != null) {
        visit(t);//访问根结点
        preOrder(t.getLchild());//递归遍历左子树
        preOrder(t.getRchild());//递归遍历右子树    
    }
}
```

**二.中序遍历**


步骤1：中序遍历左子树；


步骤2：访问根结点；


步骤3：中序遍历右子树；



```
public void inOrder(BiTree t) {
    if (t != null) {
        inOrder(t.getLchild());//递归遍历左子树
        visit(t);//访问根结点
        inOrder(t.getRchild());//递归遍历右子树    
    }
}
```

**三.后序遍历**


步骤1：后序遍历左子树；


步骤2：后序遍历右子树；


步骤3：访问根结点；



```
public void postOrder(BiTree t) {
    if (t != null) {
        postOrder(t.getLchild());//递归遍历左子树
        postOrder(t.getRchild());//递归遍历右子树
        visit(t);//访问根结点  
    }
}
```

例子如下所示：


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/10535381e65b4832a8aaec60af281d50~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=B8f5GxrRKKdYGFTak8oykuEU1ww%3D)
 


**3\.二叉查找树(二叉排序树BST)**


**(1\)没有索引的查询**


**(2\)使用二叉查找树进行优化**


**(3\)二叉查找树的定义**


**(4\)二叉查找树的查找**


**(5\)二叉查找树的插入**


**(6\)二叉查找树的删除**


**(7\)二叉查找树的实现**


**(8\)二叉查找树的缺点**


 


**(1\)没有索引的查询**


如下数据库的一张表有两列：分别是Col1和Col2。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/fe7314462f9d442e8761f75734624ae7~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=%2Bn%2FuVV98CvOf%2BURtLZpsnsJwYmU%3D)
 


查找col2\=87的这行数据，SQL语句如下：



```
mysql> select * from a where col2 = 87;
```

如果执行上面的查询时没有使用索引，那么会从磁盘中扫描一条一条的数据进行对比来找到结果。当数据表很大且数据又在表尾的时候，需要花费很多时间进行检索，效率比较低下。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/096e3d0eab7344f08a3c571aac77bbfd~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=SfleoiEvMCedlQI498W0o6WQA%2Bc%3D)
 


**(2\)使用二叉查找树进行优化**


为了加快查找，可以维护一个二叉树。


 


二叉树具有的性质：


左子树的键值小于根的键值，右子树的键值大于根的键值(左小右大)；


每个结点分别保存字段数据和一个指向对应数据记录物理地址的指针；


 


这样查找时，就可以使用二叉树查找获取相应的数据，从而快速检索出符合条件的记录。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/483b2a5bdb8a41c7830ce63e9de130a3~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=494Q6%2B5i%2FIOZwCcxiZ6zLSPOkSw%3D)
 


对该二叉树的结点进行查找发现：深度为1的结点的查找次数为1，深度为2的查找次数为2，深度为n的结点的查找次数为n。因此其平均查找次数为：(1\+2\+2\+3\+3\+3\+3\) / 6 \= 2\.8次。


 


**(3\)二叉查找树的定义**


二叉排序树(二叉查找树)或者是一棵空树，或者是具有以下特性的二叉树：


一.若左子树非空，则左子树上所有结点的值均小于根结点的值；


二.若右子树非空，则右子树上所有结点的值均大于根结点的值；


三.左右子树也分别是一棵二叉排序树；


 


根据二叉查找树的定义：左子树结点值 \< 根结点值 \< 右子树结点值，所以对二叉查找树进行中序遍历，可以得到一个递增的有序序列。


 


**(4\)二叉查找树的查找**


二叉排序树的查找是从根结点开始，沿某个分支逐层向下比较的过程。若二叉排序树非空，先将给定值与根结点的值比较，如果相等，则查找成功。


 


如果查找结点值key小于根结点的值，则在根结点的左子树上查找。如果查找结点值key大于根结点的值，则在根结点的右子树上查找。



```
public BSTNode bstSearch(BiTree t, ElemType key) {
    while (t != null && key != t.getData()) {//若二叉查找树为空或等于根结点值，则结束循环
        if (key < t.getData()) {
            t = t.getLchild();//小于，则在左子树上查找
        } else {
            t = t.getRchild();//大于，则在右子树上查找
        }
        return T;
    }
}
```

**(5\)二叉查找树的插入**


二叉排序树的特点是树的结构通常不是一次生成的，而是在查找过程中，发现树中不存在结点值等于插入值的时候，再进行插入的。


 


插入的过程如下：


若原二叉排序树为空，则直接插入结点，否则：


若插入结点值小于根结点值，则插入到左子树；


若插入结点值大于根结点值，则插入到右子树；


 


插入的结点一定是一个新添加的叶结点，而且是查找失败时的查找路径上访问的最后一个结点的左孩子或右孩子。



```
public int bstInsert(BiTree t, KeyType key) {
    if (t == null) {//原二叉查找树为空，新插入的记录为根结点
        t.data = key;
        t.lchild = null;
        t.rchild = null;
        return 1;//返回1，插入成功
    } else if (key == t.getData()) {//树中存在相同值的结点，插入失败
        return 0;
    } else if (key < t.getData()) {//插入到t的左子树
        return bstInsert(t.getLchild(), key);
    } else {//插入到t的右子树
        return bstInsert(t.getRchild(), key);
    }
}
```

**(6\)二叉查找树的删除**


一.若删除结点没有左右子树，则直接删除；


二.若删除结点右子树为空，则用左子女填补；


三.若删除结点左子树为空，则用右子女填补；


四.若删除结点左右子树均不空，则在右子树上找中序第一个子女填补；


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/f95cfc253c414aa8818d237fe7cc83d9~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=3Ko%2FaovBPq2pL8a1I5PNZBX3fb8%3D)
 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/2a642bdcb53948e4a669e8f369f6ece3~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=gGlcIi8O5YS9JbbFllSwqNq9QXg%3D)
 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/02de81fb9801471184d3575156112956~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=6Eq95LQYH1JEVWIaoB8GAA8eqow%3D)
 


**(7\)二叉查找树的实现**



```
public class BST<Key extends Comparable>, Value> {
    public static void main(String[] args) {
        BST bst = new BST();
        bst.put(10, 100);
        bst.put(8, 80);
        bst.put(12, 120);
        bst.put(6, 60);
        bst.put(9, 90);
        bst.put(5, 50);
    }


    private Node root;//二叉查找树的根结点
    
    private class Node {
        private Key key;//键
        private Value val;//值
        private Node left, right;//左右子树
        private int n;//以该结点为根的子树中的结点总数


        public Node(Key key, Value val, int n) {
            this.key = key;
            this.val = val;
            this.n = n;
        }
    }


    public int size() {
        return size(root);
    }
    
    private int size(Node x) {
        if (x == null) {
            return 0;
        } else {
            return x.n;
        }
    }


    //查找二叉排序树的结点
    public Value get(Key key) {
        return get(root, key);
    }


    //在以x为根结点的子树中查找并返回key所对应的值，如果找不到则返回null
    private Value get(Node x, Key key) {
        if (x == null) {
            return null;
        }
        int cmp = key.compareTo(x.key);
        if (cmp < 0) {
            return get(x.left, key);
        } else if (cmp > 0) {
            return get(x.right, key);
        } else {
            return x.val;
        }
    }


    //查找key，找到则更新它的值，否则为它创建一个新的结点
    public void put(Key key, Value val) {
        root = put(root, key, val);
    }


    //如果key存在于以x为根结点的子树中则更新它的值
    //否则将以key和val为键值对的新结点插入到该子树中
    private Node put(Node x, Key key, Value val) {
        if (x == null) {
            //新添加的结点，以该结点为根的子树的结点个数为1
            return new Node(key, val, 1);
        }
        int cmp = key.compareTo(x.key);
        if (cmp < 0) {
            x.left = put(x.left, key, val);
        } else if (cmp > 0) {
            x.right = put(x.right, key, val);
        } else {
            x.val = val;
        }
        x.n = size(x.left) + size(x.right) + 1;
        return x;
    }
}
```

**(8\)二叉查找树的缺点**


MySQL索引底层使用的并不是二叉树，因为二叉树有个不平衡的缺点。在存储有序的数据时，最终的排列结构会形成一个单向链表，如下图示：树的高度过高，这时候读取某个指定结点的效率会很低。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/6f15cd9b859f4c73ad5041089d55d7d2~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=%2FEv9S0GQLrhQMoOLBeSoFvM81ks%3D)
 


测试链接：



```
https://www.cs.usfca.edu/~galles/visualization/BST.html
```

 


**4\.平衡二叉树(AVL树)**


**(1\)平衡二叉树的定义**


**(2\)AVL树与非AVL树对比**


**(3\)AVL树的插入**


**(4\)AVL树的删除**


**(5\)AVL树的优缺点**


 


**(1\)平衡二叉树的定义**


**一.定义**


为了避免二叉排序树的高度增长过快，降低二叉排序树的性能。规定在插入和删除二叉树结点时，要保证任意结点左右子树高度差不超1，这样的二叉排序树就称为平衡二叉树。


 


平衡二叉树可定义为：


或者是一棵空树，或者是具有下列性质的二叉树，它的左子树和右子树都是平衡二叉树，且左右子树的高度差绝对值不超1。


 


**二.总结**


由于二叉查找树存在不平衡的问题，所以可以通过叶子结点自旋和调整，让二叉树始终保持基本平衡的状态，这样就能够保持二叉查找树的最佳性能。


 


AVL树就是基于以上思路的自动调整平衡的二叉树。平衡二叉树(AVL树)在符合二叉查找树的条件下，还满足任何结点的左右子树的高度差最大为1。


 


**(2\)AVL树与非AVL树对比**


左边是AVL树，它的任何结点的两个子树的高度差 \<\= 1。右边不是AVL树，其根结点的左子树高度为3，而右子树高度为1。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/36b0779209e54e769543a6d996fb414b~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=AVfikbL2lKN%2BoVm6oKwxJLXhyc0%3D)
 


**(3\)AVL树的插入**


二叉排序树保证平衡的基本思想如下：


每当在二叉排序树中插入(或删除)一个结点时，首先检查其插入路径上的结点是否因为此次操作而导致了不平衡。若导致不平衡，则先找到插入路径上离插入结点最近的最小不平衡子树。再调整最小不平衡子树的各结点的位置关系，使之重新达到平衡。


 


AVL树失去平衡后，可以通过旋转使其恢复平衡。接下来介绍四种失去平衡的情况下对应的旋转方式。下面所说的根结点，指的是最小不平衡子树的根结点。


 


**一.LL旋转(右单旋转)—根结点的左子树高度比右子树高度高2**


插入的具体情况是：在根结点的左孩子(L)的左子树(L)上插入新结点。


 


步骤1：原来根结点的左孩子作为新的根结点；


步骤2：原来根结点作为新根结点的右孩子；


步骤3：原来根结点的左孩子的右孩子作为新根结点的右孩子的左孩子；


 


注意：旋转时是按插入结点的爷结点进行右单上旋；


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/db75f44eba7c491d814ea7b12da4f442~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=FumgczuHUwoSI0TyfvqG0bUkMew%3D)
 


**二.RR旋转(左单旋转)—根结点的右子树高度比左子树高度高2**


插入的具体情况是：在根结点的右孩子(R)的右子树(R)上插入新结点。


 


步骤1：原根结点的右孩子作为新根结点；


步骤2：原根结点作为新根结点的左孩子；


步骤3：原来根结点的右孩子的左孩子作为新根结点的左孩子的右孩子；


 


注意：旋转时是按插入结点的爷结点进行左单上旋；


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/36456defc0584f3a860af54ca9b6fb2b~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=EiTJxxFDTacbMHMRMOCUrl8GRBE%3D)
 


**三.LR旋转(先左旋后右旋)**


插入的具体情况是：在根结点的左孩子(L)的右子树(R)上插入新结点。如果还是按插入结点的爷结点使用右单上旋，那么旋转后的高度差还是2。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/7386f27d9114403280c25d01f12e145d~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=m2gfHag%2FB4mxoKaKf2t9TWlBSj4%3D)
 


所以，这种情况需要先左旋再右旋。左旋时是按插入结点的爷结点进行左下旋，右旋时是按插入结点的爷(父)结点进行右上旋。


 


**情况一：**


在根结点的左孩子(L)的右子树(R)上插入左结点


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/fc638f8d598e4d5eaed13aeeb0701737~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=wLZclfEdy%2FrOcrrg4qs3cr7CSs8%3D)
 


**情况二：**


在根结点的左孩子(L)的右子树(R)上插入右结点


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/07e58175530c4d7f925062d20263f6b9~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=FM%2BGZy%2FFVfbDHDhSjCVXCLQNw6c%3D)
 


**四.RL旋转(先右旋后左旋)**


插入的具体情况是：在根结点的右孩子(R)的左子树(L)上插入新结点。如果还是按插入结点的爷结点使用左上旋转，那么旋转后的高度差还是2。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/e7e89f0ea99d49abbe8e9d72ff852649~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=hBxEeYU%2FamqkeAajGhOfb6WnK3k%3D)
 


所以，这种情况需要先右旋再左旋。右旋时是按插入结点的爷结点进行右下旋，左旋时是按插入结点的爷(父)结点进行左上旋。


 


**情况一：**


在根结点的右孩子(R)的左子树(L)上插入右结点


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/1cb75b61fcd94aa8ba529861f4b7e832~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=vF%2Fqw2ezi6eEmgRgvq%2Bcam4%2FmE4%3D)
 


**情况二：**


在根结点的右孩子(R)的左子树(L)上插入左结点


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/b9be84e312c24011ba186d0b03419b1f~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=PYBlsNfalk%2FyxgTbTd2Cs0FKAOI%3D)
 


**(4\)AVL树的删除**


以删除结点w为例说明AVL树删除操作的步骤：


一.用二叉排序树的方法对结点w执行删除操作


二.从结点w开始向上回溯，找到第一个不平衡的结点z(最小不平衡子树)


最小不平衡子树的根结点为结点z，结点z的高度最高的孩子结点为y，结点y的高度最高的孩子结点为x。


三.对以结点z为根结点的最小不平衡子树进行调整


 


其中结点x、结点y、结点z的位置情况有四种：


一.y在z的左，x在y的左(LL，右单旋转)


二.y在z的右，x在y的右(RR，左单旋转)


三.y在z的左，x在y的右(LR，先左旋后右旋)


四.y在z的右，x在y的左(RL，先右旋后左旋)


 


**(5\)AVL树的优缺点**


**AVL树的优点：**


一.叶子结点的层级减少；


二.形态上能够保持平衡；


三.查询效率提升，大量的顺序插入也不会导致查询性能的降低；


 


**AVL树的缺点：**


一.一个结点最多分裂出两个子结点，树的高度太高，导致IO次数过多；


二.结点里面只保存着一个关键字，每次操作获取的目标数据太少；


 


**4\.红黑树**


**(1\)红黑树的定义和性质**


**(2\)红黑树的推论**


**(3\)红黑树的插入**


**(4\)红黑树的删除**


 


**(1\)红黑树的定义和性质**


为了保持平衡二叉树的平衡性，插入和删除都要频繁调整结点的位置。为此在平衡二叉树的平衡标准上进一步放宽条件，引入红黑树的结构。


 


为了理解红黑树，对于n个结点的红黑树，会引入n\+1个外部叶结点，以保证红黑树中每个结点(内部结点)的左右孩子均不为空，其中红黑树的叶结点是虚构的外部结点、是一个null结点。


 


一棵红黑树是满足如下性质的二叉排序树：


性质一：每个结点或是红色或是黑色


性质二：根结点是黑色


性质三：叶结点都是黑色


性质四：红结点的父结点和孩子结点都是黑色


性质五：每个结点到任一叶结点的简单路径上所含黑结点数量相同


 


从红黑树的某个结点出发(不含该结点)，到达一个叶结点的任一简单路径上的黑结点总数称为该结点的黑高(bh)，红黑树的根结点的黑高称为红黑树的黑高。


 


**(2\)红黑树的推论**


**推论一：从根结点到叶结点的最长路径不大于最短路径的2倍；**


**推论二：有n个内部结点的红黑树的高度h \<\= 2log(n\+1\)；**


**推论三：新插入红黑树中的结点初始着色为红色；**


 


可见，红黑树的适度平衡，由平衡二叉树的高度平衡，降低到任一结点左右子树的高度，相差不超过2倍，从而降低了动态操作时，调整结点位置的频率。


 


对于一棵二叉查找树，如果插入和删除比较少，查找比较多，可用AVL树。如果插入和删除比较多，那么使用红黑树会更加合适。但由于维护平衡二叉树的高度平衡所付出的代价比收益大，一般用红黑树。


 


假设新插入的结点初始着色为黑色，那么这个结点所在的路径就会比其他路径多出一个黑结点，这样就破坏了性质五，而且调整起来也比较麻烦。


 


如果新插入的结点初始着色为红色，此时所有路径上的黑结点数量不变，仅出现连续两个红结点才需要调整，这样调整起来就比较简单了。


 


**(3\)红黑树的插入**


用二叉查找树的插入法插入结点z，并着色为红色。


若插入结点z的父结点是黑色，则无须做任何调整，此时就是一棵标准红黑树；


若插入结点z的父结点是根结点，则将结点z着色为黑色(树的黑高增加1\)；


若插入结点z不是根结点 \+ 插入结点z的父结点是红色，则分如下三种情况处理：


 


**情况一：插入结点z的叔结点y是黑色的，且插入结点z是一个左孩子**


A.如果结点z是爷结点的左孩子(L)的左孩子(L)


首先对结点z的父结点做一次右上旋转(LL，右单旋转)，然后交换结点z的原父结点和原爷结点的颜色。这样就可以保持性质五，也不会改变红黑树的黑高，且红黑树也不会再有连续的两个红结点。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/e4952a5d13224f74a6da74d2d9f40ddc~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=M6%2BfMW6DggnP9dxSPJLEhH8tFuE%3D)
 


B.如果结点z是爷结点的右孩子(R)的左孩子(L)


首先对结点z的父结点做一次右下旋转，这样就会转变为情况二B。然后再对结点z的新父结点做一次左下旋转(RL，先右旋再左旋)，最后交换结点z和右旋后结点z的父结点的颜色。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/600157815f1047e59ccde2885c004a5c~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=0s%2B%2FhIx3KEdADsJ3CUo9YgRZ6bg%3D)
 


**情况二：插入结点z的叔结点y是黑色的，且插入结点z是一个右孩子**


A.如果结点z是爷结点的左孩子(L)的右孩子(R)


首先对结点z的父结点做一次左下旋转，这样就会转变为情况一A。然后再对结点z的新父结点做一次右下旋转(LR，先左旋再右旋)，最后交换结点z和左旋后结点z的父结点的颜色。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/e55badb02ca74e7d8088d40453caada3~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=OxsFcdVMTHgtTeiV32f3zJsEPko%3D)
 


B.如果结点z是爷结点的右孩子(R)的右孩子(R)


首先对结点z的父结点做一次左上旋转(RR，左单旋转)，然后交换结点z的原父结点和原爷结点的颜色。这样就可以保持性质五，也不会改变红黑树的黑高，且红黑树也不会再有连续的两个红结点。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/7d820551c57f4c1a946429522624fa9b~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=8XPEygVJjFR7q8j%2BnFCRKxS7Lzo%3D)
 


**情况三：插入结点z的叔结点y是红色(插入结点z是左孩子还是右孩子不影响)**


此时，结点z的父结点和叔结点y都是红色，结点z的爷结点是黑色。所以将结点z的父结点和叔结点y着为黑色，将结点z的爷结点着为红色。如果爷结点是根结点，那么继续着为黑色，这样也能保持性质五。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/b7b330be84734630aa5aba0eab4568e4~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=kev0%2FkmPkiz5dxWe8hbkHcoeSpA%3D)
 


插入结点的情况总结：


一.插入结点为根结点


直接将插入的结点变成黑色。


二.父结点为黑色结点


此时不需要任何操作。


三.父结点为红色，叔结点为红色


将叔结点和父结点改为黑色，爷结点改为红色。然后又将爷结点当作插入结点看待，一直进行上面操作。直到当前结点为根结点，然后将根结点变成黑色。


四.父结点为红色，叔结点为黑色


情况一：父结点为左，插入结点为左 \|\| 父结点为右，插入结点为右


将父结点和爷结点的颜色互换，然后对爷结点进行一次右旋(左旋)。


情况二：父结点为左，插入结点为右 \|\| 父结点为右，插入结点为左


首先对父结点进行左旋(右旋)，左旋(右旋)后的情况必定是情况一，于是便可以按照情况一来进行处理。


 


**(4\)红黑树的删除**


**一.删除结点的过程原理**


首先按二叉查找树的删除方法进行删除。


 


**假设删除结点有两个孩子：**


那么根据二叉查找树的删除结点的方法，要找删除结点的中序后继填补，也就是需要找删除结点的右子树中最小的结点和删除结点进行位置交换，然后删除交换位置后，在原来中序后继结点位置的删除结点。由于原来的中序后继结点(因为是最小的)最多只有一个孩子，于是就将删除结点有两孩子转换为没有孩子或者只有一个孩子的情况了。


 


**假设删除结点只有一个孩子：**


由于删除结点还有一个空的黑色叶结点，其唯一孩子结点也会有一个空的黑色叶结点，所以其唯一孩子结点必然是红色。于是按照二叉查找树的删除方法，用红色的孩子结点替换删除结点即可。


 


**假设删除结点没有孩子：**


如果删除结点是红色，可以直接删除，无须调整；如果删除结点是黑色，那么有4种不同的情况；


 


**二.删除结点没有孩子且是黑色的4种情况**


假设删除结点为y，经过二叉查找树的删除方法删除结点之后，会使用结点x来替换结点y(如果y是叶结点，那么x是黑色的空叶结点)。删除y结点后将导致先前包含结点y的任何路径上的黑结点数量减1，因此结点y的任何祖先都不再满足红黑树的性质五。


 


修正办法就是将替换y的结点x视为还有额外一重黑色，定义为双黑结点。也就是说，如果将任何包含结点x的路径上的黑结点数量加1。在此假设下，性质五得到了满足，但多出了一个双黑结点，破坏了性质一。于是，删除操作便可以转化为将双黑结点恢复为普通结点的操作。


 


**情况一：x的兄弟结点w是红色的**


由于结点w是红色，所以结点w的父结点和孩子结点必然是黑色的。于是交换结点w和结点x的父结点x.p的颜色，然后对x.p做一次左旋，这次左旋不会破坏红黑树的任何规则。


 


如下图示，x的新兄弟结点是旋转之前w的某个孩子结点，其颜色为黑色。这样就将情况一转换为情况二、情况三或情况四了。


 


![](https://p26-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/f8bc43309dc24a8d877b94ed378c9044~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=zYE6U0hy0JpFTb2DVltwIXjd4yI%3D)
 


**关于上图的补充说明：**


假设从结点A出发到叶结点的普通路径有n个黑色结点(包含出发结点)，那么从黑色结点B出发到叶结点的普通路径就应该有n \+ 2个黑色结点，从红色结点D出发到叶结点的普通路径就应该有n \+ 1个黑色结点，从黑色结点C出发到叶结点的普通路径就应该有n \+ 1个黑色结点，从黑色结点E出发到叶结点的普通路径就应该有n \+ 1个黑色结点。


 


**情况二：x的兄弟结点w是黑色的，w的右孩子是红色，w的左孩子可红可黑**


由于x的兄弟结点w的右孩子是红色，即红结点是其爷结点的右孩子的右孩子。所以交换x的兄弟结点w和x的父结点x.p的颜色，然后把w的右孩子着为黑色，并对x的父结点x.p做一次左旋，最后就可以将x恢复为普通的黑色结点。此时红黑树的性质不会再收到破坏了，其中x的父结点x.p是黑还是红不影响。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/bf4fc385e4944742a0fc40d3d72b0724~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=AVoeVpurAiTTC9qrOwxqAw104Qc%3D)
 


**情况三：x的兄弟结点w是黑色的，w的右孩子是黑色，w的左孩子是红色**


由于x的兄弟结点w的左孩子是红色，即红结点是其爷结点的右孩子的左孩子，所以交换x的兄弟结点w和其左孩子的颜色，然后对x的兄弟结点w做一次右旋。这样情况三就转换为情况二了，此时x的父结点x.p是黑还是红不影响。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/a89b71ccf83446ec9898478f32c34e5d~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=fn7GcpvrZftgyExBO8gEYbShMNo%3D)
 


**情况四：x的兄弟结点w是黑色的，w的右孩子是黑色，w的左孩子也是黑色**


因为w也是黑色的，所以可以将x和其兄弟结点w上去掉一重黑色，从而使得x只有一重黑色，而其兄弟结点w则变成红色。


 


为了补偿从x和w中去掉的一重黑色，可以把x的父结点x.p额外着一层黑色，从而保持局部的黑高不变。


 


如果x.p是红色，此时将x.p着为黑色即可；


如果x.p是黑色，则将x.p作为新结点x(x上升一层)来继续循环处理；


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/d987f4e876ca4f5e939a43eabf474c8d~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=8YgBMPIF86qP3FMiRwu6uW6c1UA%3D)
 


**总结：**


情况一：x的兄弟结点w是红色的


情况二：x的兄弟结点w是黑色的，w的右孩子是红色，w的左孩子可红可黑


情况三：x的兄弟结点w是黑色的，w的右孩子是黑色，w的左孩子是红色


情况四：x的兄弟结点w是黑色的，w的右孩子是黑色，w的左孩子也是黑色


 


**5\.B\-Tree**


**(1\)B\-Tree介绍**


**(2\)B\-Tree结构存储索引的特点**


**(3\)B\-Tree的查找操作**


**(4\)B\-Tree总结**


 


因为AVL树存在的缺陷，MySQL并没有把它作为索引存储的数据结构，如何通过降低树的高度减少磁盘IO是数据库提升性能的关键。如果每个结点多存储一些数据，每次磁盘IO就能多加载一些数据到内存，B\-Tree就是基于这样的思想设计的。


 


**(1\)B\-Tree介绍**


B\-Tree是一种平衡的多路查找树，B树允许一个结点存放多个数据。这样在尽可能减少树的深度的同时，存放更多的数据，也就是把瘦高的树变得矮胖。


 


B\-Tree中所有结点的子树个数的最大值称为B\-Tree的阶，用m表示。一棵m阶的B树，如果不为空，就必须满足以下条件。


 


m阶的B\-Tree要满足以下条件：


一.每个结点最多拥有m个子树


二.每个结点最多拥有m\-1个数据


三.根结点至少有两个子树


四.分支结点至少有(m/2\)棵子树，防止变成二叉树


五.所有叶子结点都在同一层，并且以升序排序


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/c4776f72828441cb9f454f4b3d158657~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=NbZsQ0BLCEHbGL%2Fm4I0piW1cpoA%3D)
 


**什么是B\-Tree的阶？**


所有结点中，结点\[60, 70, 90]拥有的子结点数目最多。由于结点\[60, 70, 90]拥有四个子结点(灰色结点)，所以上面的B\-Tree为4阶B树。


 


**(2\)B\-Tree结构存储索引的特点**


为了描述B\-Tree首先定义一条记录为一个键值对\[key, data]，key为记录的键值，对应表中的主键值(聚簇索引)，data为一行记录中除主键外的数据。对于不同的记录，key值互不相同。


 


B\-Tree结构存储索引的特点如下：


一.索引值和data数据分布在整棵树的各个结点中


二.白色块部分是指针，存储着子结点的地址信息


三.每个结点可以存放多个索引值及对应的data数据


四.树结点中的多个索引值从左到右升序排列


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/dcb3e2f8f2fa4a1cb4666338cd89a586~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=dfvn9ZJ6qqOQKkTBzw6qepGtZKo%3D)
 


**(3\)B\-Tree的查找操作**


B\-Tree的每个结点的元素可以视为一次IO读取，树的高度表示最多的IO次数。


在相同数量的总元素个数下：每个结点的元素个数越多，高度越低，查询所需的IO次数越少。


 


B\-Tree的查找可以分为3步：


步骤1：首先要查找结点


因为B\-Tree通常是在磁盘上存储的，所以这步需要进行磁盘IO操作。


步骤2：然后查找关键字


当找到某个结点后将该结点读入内存，然后通过顺序或折半查找来查找关键字，如果命中就结束查找。


步骤3：若没有找到关键字，则需要判断大小来找到合适的分支继续查找。如果已经找到了叶子结点，就结束查询。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/370cfb34238548ad8fa60f8cbffce561~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=17o7vNxiuR8pTlvab3ihgOKjYUg%3D)
 


下面是在一个3阶B\-Tree中查找元素21的过程：



```
演示地址: https://www.cs.usfca.edu/~galles/visualization/BTree.html
插入顺序: 32 3 12 54 1 9 14 21 54 65 66
```

**(4\)B\-Tree总结**


优点：B树可以在内部结点存储键值和相关记录数据，把频繁访问的数据放在靠近根结点位置，可提高热点数据的查询效率。


 


缺点：B树中每个结点不仅包含数据的key值，还有data数据。所以当data数据较大时，会导致每个结点存储的key值减少，并且导致B树的层数变高，增加查询时的IO次数。


 


使用场景：B树主要应用于文件系统以及部分数据库索引，如MongoDB。大部分关系型数据库索引则是使用B\+树实现。


 


**6\.B\+Tree**


**(1\)B\+Tree的特征**


**(2\)B\+Tree的优势**


**(3\)一棵B\+Tree可以存放多少数据**


 


B\+Tree是在B\-Tree基础上的一种优化，使其更适合实现存储索引结构。InnoDB存储引擎就是用B\+Tree实现其索引结构。


 


**(1\)B\+Tree的特征**


特征一：非叶子结点只存储键值信息


特征二：所有叶子结点之间都有一个链指针


特征三：数据记录都存放在叶子结点中


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/59db1f3e9e874b86bd8c4e6580acabc8~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=gkUbptW8PLJVImqMAZqVZqQNzWA%3D)
 


**(2\)B\+Tree的优点**


**优点一：B\+Tree降低树高和增大结点存储数据量**


B\+Tree是B\-Tree的变种，B\-Tree能解决的问题，B\+Tree也能够解决。


 


**优点二：B\+Tree扫库和扫表能力更强**


对B\-Tree的数据表的所有数据进行扫描，需要遍历整棵树。对B\+Tree的数据表的所有数据进行扫描，只需遍历所有叶子结点。因为B\+Tree的叶子结点才存放完整的数据，以及之间有指针进行链接。


 


**优点三：B\+Tree磁盘读写能力更强，因根结点和分支结点不保存数据**


如果所有根结点和分支结点同样大小，B\+Tree保存的key要比B\-Tree多。所以B\+Tree读写一次磁盘加载的关键字key要比B\-Tree更多。


 


**优点四：B\+Tree排序能力更强**


B\+Tree天然具有排序功能。


 


**优点五：B\+Tree查询效率更加稳定，每次查询数据的IO次数相对稳定**


当然如果直接看B\-Tree可以根据根结点命中而直接返回，则效率更高。


 


**(3\)一棵B\+Tree可以存放多少数据**


**一.B\+树是如何存放的**


B\+Tree的结点的大小等于一个数据页的大小(16K)。B\+Tree的根结点保存在内存中，子结点才存储在磁盘上。


 


设计一个结点等于一个页的目的是每个结点只需一次IO就可完全载入。InnoDB的一个数据页的大小是16K，所以每个结点的大小也是16K。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/54531dda7c5f41c294c8c23a109dfd7f~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=Roh0u96EyLzTLf29jo47lr0W63Q%3D)
 


假设一个B\+树高为2，即存在一个根结点和若干个叶子结点。那么这棵B\+树的存放总记录数为：根结点指针数 \* 单个叶子结点记录行数。


 


**二.计算根结点指针数**


假设表的主键为int类型占用4字节，指针大小为6字节。那么一个页即B\+Tree中的一个结点，大概可以存储的指针数量为：16384B / (4B \+ 6B) \= 1638，一个结点最多可存储1638个索引指针。


 


**三.计算每个叶子结点的记录数**


假设一行记录的数据大小为1K，那么一个叶子结点就可以存储16行数据：16K / 1K \= 16。


 


**四.一棵高度为2的B\+Tree可以存放多少记录数**


一棵高度为2的B\+Tree可以存放：1638 \* 16 \= 2\.6万条数据记录，同理一棵高度为3的B\+Tree可存放：1638 \* 1638 \* 16 \= 4千万条记录。所以一个高度为3的B\+Tree就可以满足千万级别的数据存储。


 


InnoDB中的B\+Tree的高度一般就是1～3层，由于查找一次数据页代表一次IO，所以通过主键索引查询通常只需要1～3次IO即可查到数据。


 


**7\.Hash索引**


**(1\)Hash索引**


**(2\)Hsah索引的优点**


**(3\)Hash索引的缺点**


 


**(1\)Hash索引**


MySQL中索引的数据结构有两种：一种是B\+Tree，另一种是Hash。Hash底层是由Hash表来实现的，根据键值存储数据，非常适合根据key查找value值，也就是单个key查询，或者等值查询。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/0bff557850364c01a0386ce06b89dec9~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=287hYCB76XeioH1O%2FTcw%2FjujAQw%3D)
 


对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码。哈希码是一个较小的值，如果出现哈希码值相同的情况会拉出一条链表。


 


**(2\)Hsah索引的优点**


一.因为索引自身只需要存储对应的Hash值，所以索引结构非常紧凑。只做等值查询而不包含排序或范围查询的需求，都适合使用哈希索引。


 


二.没有哈希冲突的情况下，等值查询访问哈希索引的数据非常快。如果发生Hash冲突，必须遍历链表中的所有行指针，逐行进行比较。


 


**(3\)Hash索引的缺点**


缺点一：哈希索引只包含哈希值和行指针，不存储字段值，所以不能使用索引中的值来避免读取行。


 


缺点二：哈希索引只支持等值比较查询，不支持任何范围查询和部分索引列匹配查找。


 


缺点三：哈希索引数据并不是按照索引值顺序存储的，所以也就无法用于排序。


 


缺点四：如果发生哈希冲突，存储引擎就必须遍历链表，来逐行比较。


 


**8\.聚簇索引与非聚簇索引**


**(1\)聚簇索引(主键索引)**


**(2\)非聚簇索引(非主键索引或二级索引)**


**(3\)使用聚簇索引时要注意的问题**


**(4\)使用非聚簇索引要注意的问题**


 


假设有一个表中的记录如下，表中id是聚集索引，name是普通索引。


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/3f16be4b32084614a692b55f72de83c1~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=zyQidc1JCryjWzmLPIqAlbRFxJA%3D)
 


**(1\)聚簇索引(主键索引)**


聚簇索引将数据存储与索引放到了一块，索引结构的叶子结点保存了行数据。


 


聚簇索引是一种数据存储方式，InnoDB的聚簇索引就是按照主键顺序构建B\+Tree结构。


 


B\+Tree的叶子结点就是行记录，行记录和主键值紧凑地存储在一起，这也意味着InnoDB的主键索引就是数据表本身。主键索引按主键顺序存放了整张表的数据，占用的空间是整个表的大小，通常说的主键索引就是聚簇索引。


 


InnoDB的表要求必须要有聚簇索引：


一.如果表定义了主键，则主键索引就是聚簇索引


二.如果表没有定义主键，则第一个非空的唯一列作为聚簇索引


三.如果表没有定义主键 \+ 也没有非空的唯一列，则会创建一个隐藏的row\-id作为聚簇索引


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/64df9554bd2f4fa4a4f000440ce932a1~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=vkOzlKrVvixYXj01GYV6jedA7dw%3D)
 


**(2\)非聚簇索引(非主键索引或二级索引)**


非聚簇索引将数据与索引分开存储，索引结构的叶子结点只保存索引列和主键信息。


 


InnoDB的二级索引，是根据索引列构建B\+Tree结构，但在B\+Tree的叶子结点中只存储索引列和主键的信息，所以二级索引占用的空间会比聚簇索引小很多。通常创建二级索引就是为了提升查询效率，一个表InnoDB只能创建一个聚簇索引，但可以创建多个二级索引。


 


![](https://p26-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/1fd7b6640b4e4ab4bcb32b4137368036~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=O%2FQdnRXaHU%2FKxm7pZ0b4Cx64n2M%3D)
 


**(3\)使用聚簇索引时要注意的问题**


一.主键最好不要使用UUID


因为UUID的值太过离散，不适合排序且可能新增的UUID会插入在索引树中间的位置，导致索引树调整复杂度变大，消耗更多的时间和资源。


 


二.建议使用int类型的自增


方便排序且会在索引树的末尾增加主键值，对索引树的结构影响最小。主键值占用的存储空间越大，二级索引中保存的主键值也会跟着变大，占用的存储空间会影响到IO操作读取到的数据量。


 


**(4\)使用非聚簇索引要注意的问题**


**一.什么是回表**


下面来执行这样一条SQL语句：



```
mysql>  select * from A where name = 'ls';
```

在通过name进行查询时，需要扫描两遍索引树：


一.先通过非聚簇索引定位到主键值 \= 1


二.根据主键值在聚簇索引中定位到具体记录


 


![](https://p3-sign.toutiaoimg.com/tos-cn-i-6w9my0ksvp/f33f057a40bd44c5921ca96675c0e83c~tplv-obj.image?lk3s=ef143cfe&traceid=202412010019538CACD48C6ADA7B26B3AE&x-expires=2147483647&x-signature=f8dqCLVhMOaYEmqWVeFSuz%2FQMOM%3D)
 


回表总结：先根据普通索引查询主键值，再根据主键值在聚集索引中获取行记录。回表查询，相对于只扫描一遍聚集索引树的性能要低一些。


 


**二.使用覆盖索引来避免回表**


**什么是覆盖索引：**


如果一个索引包含了所有需要查询的字段值，那么该索引就是覆盖索引。覆盖索引是一种避免回表查询的优化策略，只需在一棵索引树上就能获取所需的所有列数据，无需回表，速度更快。


 


**覆盖索引的实现方式：**


对被查询的字段建立普通索引或联合索引，查询时直接返回索引数据。不需要再通过聚集索引去定位行记录，避免了回表。


 


 本博客参考[wgetcloud全球加速服务机场](https://wa7.org)。转载请注明出处！
