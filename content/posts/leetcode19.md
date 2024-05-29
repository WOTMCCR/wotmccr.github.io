---
categories: 算法
tags:
  - leetcode
  - 树
date: 2024-05-29
description: 235. 二叉搜索树的最近公共祖先  701.二叉搜索树中的插入操作  450.删除二叉搜索树中的节点
title: 2024-05-29刷题2
---
## 2024-05-29 刷题2
### 235.二叉搜索树的最近公共祖先
16:49
#### 思路
根据二叉搜索树的性质，当节点的值处于p，q的区间中的时候，则属于公共祖先。
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    TreeNode* traversal(TreeNode *cur , TreeNode *p,TreeNode *q){
        if(cur == NULL) return cur;
        
        if (cur->val > p->val && cur->val > q->val) {   // 左
            TreeNode* left = traversal(cur->left, p, q);
            if (left != NULL) {
                return left;
            }
        }

        if (cur->val < p->val && cur->val < q->val) {   // 右
            TreeNode* right = traversal(cur->right, p, q);
            if (right != NULL) {
                return right;
            }
        }
        return cur;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return traversal(root,p,q);
    }
};
```
迭代法
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(root){
            //目标在根的左边
            if(root->val > p->val && root->val > q->val){
                root = root->left;
            }else if(root->val < p->val && root->val < q->val){
                root = root->right;
            }else {
                return root;
            }
        }
        return NULL;
    }
};
```





### 701.二叉搜索树中的插入操作
18:14
#### 思路
##### 递归
1. 确定参数以及返回值，当前节点以及要插入的值，返回节点简化逻辑
```C++
TreeNode* insertIntoBST(TreeNode* root, int val)
```
2. 终止条件，就是当前节点为空，即为需要添加节点的位置了
```C++
if (root == NULL) {
    TreeNode* node = new TreeNode(val);
    return node;
}
```
3. 单层递归逻辑
```C++
		if(root->val > val) root->left = insertIntoBST(root->left,val);
        if(root->val < val) root->right = insertIntoBST(root->right,val);
        
        return root;
```
##### 迭代
需要记录节点的父节点的值
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        //当遍历到空节点则到达了需要添加节点的位置
        if(root == nullptr){
            TreeNode *node = new TreeNode(val);
            return node;
        }

        if(root->val > val) root->left = insertIntoBST(root->left,val);
        if(root->val < val) root->right = insertIntoBST(root->right,val);
        
        return root;
    }
};
```
迭代
```C++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(root == nullptr){
            TreeNode *node =new TreeNode(val);
            return node;
        }

        TreeNode *cur = root;
        TreeNode *prev = root;
        while(cur != nullptr){
            prev =cur;
            if(cur->val > val) cur = cur->left;
            else cur = cur->right;
        }

        TreeNode *node = new TreeNode(val);
        if(prev->val > val) prev->left = node;
        else prev->right = node;
        return root;
    }
};
```

### 450.删除二叉搜索树中的节点
18:43
#### 思路
有以下五种情况：
- 第一种情况：没找到删除的节点，遍历到空节点直接返回了
- 找到删除的节点
    - 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
    - 第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
    - 第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
    - 第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        //第一种情况
        if(root == nullptr) return root;
        if(root->val == key){
            // 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
            if(root->left == nullptr && root->right ==nullptr){
                delete root;
                return nullptr;
            }else if(root->left == nullptr && root->right !=nullptr){
                auto retNode = root->right;
                delete root;
                return retNode;
            }else if(root->left != nullptr && root->right == nullptr){
                auto retNode = root->left;
                delete root;
                return retNode;
            }else{
                TreeNode* cur = root->right; // 找右子树最左面的节点
                while(cur->left != nullptr) {
                    cur = cur->left;
                }
                cur->left = root->left;

                TreeNode* tmp = root;   
                root = root->right;     
                delete tmp;             
                return root;
            }
        }
        if(root->val > key) root->left = deleteNode(root->left,key);
        if(root->val < key) root->right = deleteNode(root->right,key);
        return root;
    }
};
```






