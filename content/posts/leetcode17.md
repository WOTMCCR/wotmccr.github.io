---
categories: 算法
tags:
  - 树
  - leetcode
  - 二叉树
date: 2024-05-27
title: 2024-05-27刷题
description: 654.最大二叉树  617.合并二叉树  700.二叉搜索树中的搜索  98.验证二叉搜索树
---
## 2024-05-27刷题
### 654.最大二叉树
11:05
#### 思路
只需要一个数组，以及开始结束索引。
1. 终止条件
	当，左索引大于右索引的时候终止。
2. 操作逻辑
	1. 创建一个根节点，其值为 `nums` 中的最大值。
	2. 递归地在最大值 **左边** 的 **子数组前缀上** 构建左子树。
	3. 递归地在最大值 **右边** 的 **子数组后缀上** 构建右子树。
3. 创建左右子树
```C++
 root->left = traversal(nums,start,max_index);
        root->right = traversal(nums,max_index+1,end);
        
        return root;
```
#### 注意⚠️

```C++
        int max_index = start;
        for(int i = start + 1 ; i < end ; i++){
            if(nums[i] > nums[max_index]) max_index = i;
        }
```
#### 题解
##### C++
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode * traversal(vector<int> nums,int start , int end){
        //终止条件
        if(start >= end)
            return nullptr;

        //找最大值
        int max_index = start;
        for(int i = start + 1 ; i < end ; i++){
            if(nums[i] > nums[max_index]) max_index = i;
        }
        
        TreeNode *root = new TreeNode(nums[max_index]);    
        //递归创建
        root->left = traversal(nums,start,max_index);
        root->right = traversal(nums,max_index+1,end);
        
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if(nums.size()){
            return traversal(nums,0,nums.size());
        }
        return nullptr;
    }
};
```

### 617.合并二叉树
11:36
#### 思路
覆盖二叉树，也就是说，当位置一致的时候，则相加合并。
所以，在一个二叉树上进行节点的加减操作

迭代法：模拟层序遍历。
1. 当另一个二叉树的相同位置上有值,
	1. 则相加更改该节点的值
	2. 将两个节点的孩子都加入队列
2. 当另一个二叉树有，本二叉树没有，则新建一个节点
3. 当另一个二叉树没有，本二叉树有，则不处理。
递归：
1. **确定递归函数的参数和返回值：**
首先要合入两个二叉树，那么参数至少是要传入两个二叉树的根节点，返回值就是合并之后二叉树的根节点。

```C++
TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
```
2. **确定终止条件：**
因为是传入了两个树，那么就有两个树遍历的节点t1 和 t2，如果t1 == NULL 了，两个树合并就应该是 t2 了（如果t2也为NULL也无所谓，合并之后就是NULL）。
反过来如果t2 == NULL，那么两个数合并就是t1（如果t1也为NULL也无所谓，合并之后就是NULL）。
```C++
if (t1 == NULL) return t2; // 如果t1为空，合并之后就应该是t2
if (t2 == NULL) return t1; // 如果t2为空，合并之后就应该是t1
```
3. **确定单层递归的逻辑：**
单层递归的逻辑就比较好写了，这里我们重复利用一下t1这个树，t1就是合并之后树的根节点（就是修改了原来树的结构）。
那么单层递归中，就要把两棵树的元素加到一起。
```
t1->val += t2->val;
```
接下来t1 的左子树是：合并 t1左子树 t2左子树之后的左子树。
t1 的右子树：是 合并 t1右子树 t2右子树之后的右子树。
最终t1就是合并之后的根节点。
```C++
t1->left = mergeTrees(t1->left, t2->left);
t1->right = mergeTrees(t1->right, t2->right);
return t1;
```

#### 题解
##### C++
迭代
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == nullptr) return root2;
        if(root2 == nullptr) return root1;
        queue<TreeNode*> que;
        que.push(root1);
        que.push(root2);

        while(!que.empty()){
            //取节点
            TreeNode *node1 = que.front(); que.pop();
            TreeNode *node2 = que.front(); que.pop();

            //此时一定不为空
            node1->val += node2->val;

            //入节点
            //两树，左节点都不为空
            if(node1->left != nullptr && node2->left != nullptr){
                que.push(node1->left);
                que.push(node2->left);
            }
            //两树，右节点都不为空
            if(node1->right != nullptr && node2->right != nullptr){
                que.push(node1->right);
                que.push(node2->right);
            }

            //只用判断root1为空的情况，如果不为空的话就不用处理
            if(node1->left == nullptr && node2->left != nullptr){
                node1->left = node2->left;
            }
            if(node1->right == nullptr && node2->right != nullptr){
                node1->right = node2->right;
            }
        }
        return root1;
    }
};
```
递归
```C++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == nullptr) return root2;
        if(root2 == nullptr) return root1;

        root1->val += root2->val;
        root1->left = mergeTrees(root1->left,root2->left);
        root1->right = mergeTrees(root1->right,root2->right);
        return root1;
    }
};
```


### 700.二叉搜索树
14:22
#### 思路
二叉搜索树，左孩子小于root，右孩子大于root
递归实现
1. 确定参数
	返回节点，传递当前节点，与目标值
```C++
TreeNode* searchBST(TreeNode* root, int val)
```
2. 终止条件，在这里处理节点为空的情况，可以减少递归逻辑处的逻辑
```C++
if (root == NULL || root->val == val) return root;
```
3. 单层递归逻辑,大于root则搜索右子树，小于root则搜索左子树
```C++
 if (root->val > val) return searchBST(root->left, val);
        if (root->val < val) return searchBST(root->right, val);
        return NULL;
```
#### 题解
##### C++
递归：
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        //可以在开始处理null
        if(root == nullptr || root->val == val) return root;
        if(root->val > val) return searchBST(root->left , val);
        if(root->val < val) return searchBST(root->right, val);
        return nullptr;
    }
};
```
迭代：
```C++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root!=nullptr){
            if(root->val > val) root = root->left;
            else if(root->val < val) root = root->right;
            else return root;
        }
        return nullptr;
    }
};
```

### 98.验证二叉搜索树
14:44
#### 思路
中序遍历下，输出的二叉搜索树节点的数值是有序序列。

所以可以记录中序遍历前一个节点的值
遍历过程中需要每次与之前的元素进行比较。
#### 注意⚠️
注意要小于等于，**搜索树里不能有相同元素**
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> result;
    void traversal(TreeNode* root){
        if(root == nullptr) return;
        traversal(root->left);
        result.push_back(root->val);
        traversal(root->right);
    }
    bool isValidBST(TreeNode* root) {
        result.clear(); // 不加这句在leetcode上也可以过，但最好加上
        traversal(root);
        for(int i = 1 ; i < result.size() ; i++){
            if(result[i] <= result[i-1]){
                return false;
            }
        }
        return true;
    }
};
```

先遍历到最左下角的最小值。再进行比较。
```C++
class Solution {
public:
    TreeNode* pre = NULL; // 用来记录前一个节点
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left = isValidBST(root->left);

        if (pre != NULL && pre->val >= root->val) return false;
        pre = root; // 记录前一个节点

        bool right = isValidBST(root->right);
        return left && right;
    }
};
```

迭代法
```C++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode *cur = root;
        TreeNode *pre = nullptr;
        while(cur != nullptr || !st.empty()){
            if(cur != nullptr){
                st.push(cur);
                cur = cur->left;
            }else{
                //中
                cur = st.top();
                st.pop();
                //判断
                if(pre!=nullptr && pre->val >= cur->val){
                    return false;
                }
                pre = cur;
                //右
                cur = cur->right;
            }
        }
        return true;
    }
};
```




