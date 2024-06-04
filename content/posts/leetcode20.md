---
categories: 算法题
date: 2024-06-04
title: 2024-06-04刷题
description: " 669. 修剪二叉搜索树 108.将有序数组转换为二叉搜索树  538.把二叉搜索树转换为累加树"
tags:
  - 树
  - leetcode
---
## 2024-06-04 刷题
### 669.修剪二叉搜索树
19:49
#### 思路
确定函数参数
有返回值，更方便，可以通过递归函数的返回值来移除节点。
```C++
TreeNode* trimBST(TreeNode* root, int low, int high)
```
确定终止条件
因为修剪操作不是在终止条件上进行的，所以遇到空节点直接返回
```C++
if (root == nullptr ) return nullptr;
```
确定单层递归逻辑
处理是否在区间内，进入下一层
#### 注意⚠️
寻找符合条件的左右子树需要
```C++
        //小于low将，右孩子连到父的左边
        if(root->val < low){
            //需要找到符合条件的右孩子。
            TreeNode *right = trimBST(root->right,low,high);
            return right;
        }
        //大于high将，左孩子，连到父的右边
        if(root->val > high){
            TreeNode *left = trimBST(root->left,low,high);
            return left;
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
    //返回treeNode方便删除
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        //终止条件
        if(root == nullptr) return nullptr;

        //处理逻辑
        //小于low将，右孩子连到父的左边
        if(root->val < low){
            //需要找到符合条件的右孩子。
            TreeNode *right = trimBST(root->right,low,high);
            return right;
        }
        //大于high将，左孩子，连到父的右边
        if(root->val > high){
            TreeNode *left = trimBST(root->left,low,high);
            return left;
        }
        //下一层
        root->left = trimBST(root->left,low,high);
        root->right = trimBST(root->right,low,high);
        return root;
    }
};
```


### 108.将有序数组转换为二叉搜索树
20:12
#### 思路
因为是有序数组，所以，中间的节点就是根节点
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
    TreeNode *traversal(vector<int> &nums,int left ,int right){
        //叶子
        if(left > right) return nullptr;

        int mid = left + (right - left) / 2;

        TreeNode *node = new TreeNode(nums[mid]);
        node->left = traversal(nums,left,mid-1);
        node->right = traversal(nums,mid+1,right);
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode *root = traversal(nums,0,nums.size() - 1);
        return root;
    }
};
```

### 538.二叉搜索树转换为累加树
20:43
#### 思路
**从树中可以看出累加的顺序是右中左，所以我们需要反中序遍历这个二叉树，然后顺序累加就可以了**。
#### 题解
##### C++
```C++
class Solution {
public:
    int pre = 0;
    void traversal(TreeNode *root){
        if(root == nullptr) return;
        traversal(root->right);
        root->val = root->val + pre;
        pre = root->val;
        traversal(root->left);
    }
    TreeNode* convertBST(TreeNode* root) {
        traversal(root);
        return root;
    }
};
```






