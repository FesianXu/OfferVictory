<div align='center'>
    <b> 二叉树的遍历，递归与迭代的解法</b>
</div>

<div align='right'>
    FesianXu, 19' 9.29 at UESTC
</div>

# 前言

树结构是一种非线性的数据结构，而二叉树是树结构中常见的一种树，这章我们考虑如何对二叉树进行元素的遍历，在这里我们会分别考虑**递归**和**迭代**两种不同的解法。



----

$\nabla$  <font color="#FF9D6F">目录</font>  $\nabla$

[TOC]



----



# 所谓二叉树以及其遍历

## 二叉树

相对于队列，栈这种抽象的线性数据结构来说，树是一种非线性的数据结构，而二叉树是树的一种经典的，常见的种类。对二叉树的遍历是一个很基础也很重要的问题，遍历指的是依据一定的顺序对树的元素进行访问，最后需要访问到整个树的所有元素。一般来说，对树的遍历可以有递归和迭代两种不同的实现，通常来说递归的解法直接明了，但是在树的深度很大的时候，容易导致stack overflow；而迭代方法通常更快，更节省内存，但是通常理解起来没有递归方法简单。

要描述一棵树，首先其单元结构可以用一个结构体描述，如：

```c++
 struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 ;
```

其中，一棵树的叶子节点指的是其左右节点皆为空

```c++
node->left == NULL && node->right == NULL ;
```





## 遍历的策略

对于每个非叶子节点而言，其都有根节点D， 左节点L和右节点R，如果遍历的时候只考虑从左到右的这种方向，我们可以有三种排列

1. DLR，前序遍历
2. LDR，中序遍历
3. LRD，后序遍历

我们发现，我们是根据访问D的位置来确定其命名的，这里举几个例子。加入树长成这个样子

![sampletree][sampletree]

那么其前序遍历结果为： `3 -> 9 -> 20 -> 15 -> 7`

中序遍历结果为： `9 -> 3 -> 15 -> 20 -> 7`

后序遍历结果为： `9 -> 15 -> 7 -> 20 -> 3`

那么针对这三种遍历策略，我们后续进行代码编写分析和实验。



## 代码平台

本文中代码可在`LeetCode`中进行运行，大家可以随意选择一个与树相关的题目进行测试代码（LeetCode内部提供了Tree数据结构的支持），比如说笔者是在`https://leetcode-cn.com/problems/binary-tree-preorder-traversal/`中进行的，我们只需要指定了树的结构（可以通过在控制台指定其树的每个元素，如下图所示），显然只要我们有了目标树的输入，我们便可以开始我们的实验了。

![sampletree][sampletree]



# 递归方案

对于树的遍历来说，递归方案是很容易理解的，这种方法也是必须要掌握的。

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return vector<int>();
        if (!root->left && !root->right)
            return vector<int>(1, root->val);
        vector<int> ret;
        ret.push_back(root->val); // 如果这里读取了当前节点的值，则前序遍历
        if (root->left) {
            vector<int> tmp = preorderTraversal(root->left);
            ret.insert(ret.end(), tmp.begin(), tmp.end());
        }
        // ret.push_back(root->val); // 如果这里读取了当前节点值，则是中序
        if (root->right) {
            vector<int> tmp = preorderTraversal(root->right);
            ret.insert(ret.end(), tmp.begin(), tmp.end());
        }
        // ret.push_back(root->val); // 如果这里读取了当前节点值，则是后续
        return ret;
    }
};
```

**我们会发现，决定其顺序的，是`ret.push_back(root->val)`这句代码的顺序而已。**

在这段代码中，#4在考虑其根节点为空的情况，这对于解题来说是必须的；#5在考虑递归到最后是叶子节点的情况，我们这里如果遇到了叶子节点，直接返回其叶子节点的值就行了；#9-#12和#14-#17这两段代码的作用是相似的，主要考虑当左子树或者右子树存在的时候，如何对其返回值的值进行拼接，这个是核心部分。

```c++
            vector<int> tmp = preorderTraversal(root->left);
            ret.insert(ret.end(), tmp.begin(), tmp.end());
```

这段代码其实是对左子树所有的先序遍历结果进行了求解，然后将其和现在已有的结果`ret`进行拼接，至于左子树是怎么得到结果的呢？那就是其下一层递归的结果啦。

如果把其遍历结果设成一个参数，就可以去掉返回值了。

```c++
class Solution {
public:
    void _helper(TreeNode* root, vector<int> &nums) {
        if (!root) return ;
        if (!root->left && !root->right) {
            nums.push_back(root->val);
            return ;
        }
        nums.push_back(root->val);
        if (root->left) 
            _helper(root->left, nums);
        if (root->right)
            _helper(root->right, nums);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ret;
        _helper(root, ret);
        return ret;
    }
};
```

不过笔者毕竟喜欢第一个版本的代码。

----

# 迭代方案

比起递归的方案，迭代的方案去遍历树比较难，在`LeetCode`中属于`中等`难度的题目[1,2,3]，不过对其进行理解有助于我们更好的理解树的遍历，因此我们还是要求掌握。

对于迭代方案来说，就不再和递归一样具有一个统一的范式进行求解了。

## 前序遍历





# Reference

[1]. https://leetcode-cn.com/problems/binary-tree-preorder-traversal/

[2]. https://leetcode-cn.com/problems/binary-tree-inorder-traversal/

[3]. https://leetcode-cn.com/problems/binary-tree-postorder-traversal/







[sampletree]: ./imgs/bt/sampletree.jpg