---
categories: 算法
title: 2024-05-23刷题3
tags:
  - 树
  - leetcode
description: 104.二叉树的最大深度 559.n叉树的最大深度  111.二叉树的最小深度见2024-05-23 刷题2 222. 完全二叉树的节点个数
date: 2024-05-23
---
## 2024-05-23 刷题3
  104.二叉树的最大深度 559.n叉树的最大深度
  111.二叉树的最小深度
见2024-05-23 刷题2
### 222. 完全二叉树的节点个数
20:03
#### 思路
层次遍历，然后统计个数
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
    int countNodes(TreeNode* root) {
        queue<TreeNode*> que;
        if(root) que.push(root);
        else return 0;
        int sum = 0;
        while(!que.empty()){
            int size = que.size();
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                sum++;
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return sum;
    }
};
```