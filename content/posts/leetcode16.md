---
categories: 算法
title: 2024-05-25刷题
date: 2024-05-25
tags:
  - leetcode
  - 树
description: " 513.找树左下角的值 112. 路径总和  113.路径总和ii 106.从中序与后序遍历序列构造二叉树  105.从前序与中序遍历序列构造二叉树"
---
## 2024-05-25刷题
### 513.找树左下角的值
20:52
#### 思路
使用层次遍历，将每层最左边的值存放在结果中。
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
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        if(root) que.push(root);
        int result = 0;
        while(!que.empty()){
            int size = que.size();
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                if(!node->left && !node->right && i == 0) result = node->val;                
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);                
            }
        }
        return result;
    }

};
```

### 112.路径总和
21:07
#### 思路
##### 递归
终止条件
```C++
if (!cur->left && !cur->right && count == 0) return true; // 遇到叶子节点，并且计数为0
if (!cur->left && !cur->right) return false; // 遇到叶子节点而没有找到合适的边，直接返回
```
逻辑，左右节点，减去对应的值
```C++
if (cur->left) { // 左 （空节点不遍历）
    // 遇到叶子节点返回true，则直接返回true
    if (traversal(cur->left, count - cur->left->val)) return true; // 注意这里有回溯的逻辑
}
if (cur->right) { // 右 （空节点不遍历）
    // 遇到叶子节点返回true，则直接返回true
    if (traversal(cur->right, count - cur->right->val)) return true; // 注意这里有回溯的逻辑
}
return false;
```
##### 迭代
**栈里一个元素不仅要记录该节点指针，还要记录从头结点到该节点的路径数值总和**
```C++
stack<pair<TreeNode*, int>> st;
st.push(pair<TreeNode*, int>(root, root->val));
```

如果新加入的节点的值等于目标值则直接返回
```C++
if(!node.first->left && !node.first->right && node.second == targetSum)
            return true;
            if(node.first->right){
                st.push(pair<TreeNode*,int>(node.first->right,node.second+node.first->right->val));
            }

            if(node.first->left){
                st.push(pair<TreeNode*,int>(node.first->left,node.second+node.first->left->val));
            }
```
#### 题解
##### C++
```C++
//递归
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
    bool traversal(TreeNode *cur , int count){
        //到叶子节点返回条件成功
        if(!cur->left && !cur->right && count == 0) return true;
        //到叶子节点返回不成功
        if(!cur->left && !cur->right) return false;

        if(cur->left){
            if(traversal(cur->left,count-cur->left->val))
                return true;
        }
        if(cur->right){
            if(traversal(cur->right,count-cur->right->val)) 
                return true;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return traversal(root,targetSum-root->val);
    }
};

//迭代
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
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;

        stack<pair<TreeNode*, int>> st;
        st.push(pair<TreeNode*,int>(root,root->val));
        while(!st.empty()){
            pair<TreeNode* , int> node = st.top();
            st.pop();
            if(!node.first->left && !node.first->right && node.second == targetSum)
            return true;
            if(node.first->right){
                st.push(pair<TreeNode*,int>(node.first->right,node.second+node.first->right->val));
            }

            if(node.first->left){
                st.push(pair<TreeNode*,int>(node.first->left,node.second+node.first->left->val));
            }

        }
        return false;
    }
};
```

### 113.路径总和二
21:53
#### 思路
使用回溯的思想，遍历所有路径，如果符合条件就加入到结果集合
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
    vector<vector<int>> result;
    vector<int> path;
    void traversal(TreeNode *cur , int count){
        //终止条件，当叶子节点的时候，结果为0则加入到结果集合中
        if(!cur->left && !cur->right && count == 0) {
            result.push_back(path);
            return;
        }
        if(!cur->left && !cur->right) return;
        //逻辑
        //使用回溯，需要在遍历之后弹出path中的元素 
        if(cur->right){
            path.push_back(cur->right->val);
            traversal(cur->right , count - cur->right->val);
            path.pop_back();
        }
        if(cur->left){
            path.push_back(cur->left->val);
            traversal(cur->left , count - cur->left->val);
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(root){
            path.push_back(root->val);    
            traversal(root,targetSum-root->val);
        }
        return result;
    }
};
```










### 106.从中序与后序遍历序列构造二叉树
10:25
#### 思路
根据数据结构的基本知识可以通过中序遍历与其他一种遍历方式唯一确定构造一课二叉树。

就是以 后序数组的最后一个元素为切割点，先切中序数组，根据中序数组，反过来再切后序数组。一层一层切下去，每次后序数组最后一个元素就是节点元素。

说到一层一层切割，就应该想到了递归。
来看一下一共分几步：
- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。
- 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割后序数组，切成后序左数组和后序右数组
- 第六步：递归处理左区间和右区间
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
    - 第一步：如果数组大小为零的话，说明是空节点了。
    - 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。
    - 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点
    - 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
    - 第五步：切割后序数组，切成后序左数组和后序右数组
    - 第六步：递归处理左区间和右区间
 */
class Solution {
public:
    TreeNode *traversal(vector<int>& inorder, int inorderBegin,int inorderEnd,vector<int>& postorder,int postorderBegin,int postorderEnd){
        //如果数组大小为0的话，说明是空节点
        if(postorderBegin == postorderEnd) return nullptr;

        // 第二步：后序遍历数组最后一个元素，就是当前的中间节点
        int rootValue = postorder[postorderEnd - 1];
        TreeNode* root = new TreeNode(rootValue);

        //叶子节点
        if (postorderEnd - postorderBegin == 1) return root;

        //找中序遍历切割点
        int delimiterIndex;
        for(delimiterIndex = inorderBegin ; delimiterIndex < inorderEnd ; delimiterIndex++){
            if(inorder[delimiterIndex] == rootValue) 
                break;
        }

        //切割中序数组 左闭右开
        int leftInorderBegin  = inorderBegin;
        int leftInorderEnd    = delimiterIndex;

        int rightInorderBegin = delimiterIndex + 1;
        int rightInorderEnd   = inorderEnd;

        //切割后序数组 左闭右开
        int leftPostorederBegin  = postorderBegin;
        //begin + 中序遍历left区间的大小 必须是到开头的
        int leftPostorederEnd    = postorderBegin +delimiterIndex -  inorderBegin;

        int rightPostorederBegin = postorderBegin +delimiterIndex -  inorderBegin;
        //最后一个节点为root
        int rightPostorederEnd   = postorderEnd - 1;    


        //添加孩子
        root -> left  = traversal(inorder,leftInorderBegin,leftInorderEnd,postorder,leftPostorederBegin,leftPostorederEnd);
        root -> right = traversal(inorder,rightInorderBegin,rightInorderEnd,postorder,rightPostorederBegin,rightPostorederEnd);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size() == 0 || postorder.size() == 0){
            return nullptr;
        }
        return traversal(inorder,0,inorder.size(),postorder,0,postorder.size());
    }
};
```

### 105. 从前序与中序遍历序列构造二叉树
11:12
#### 思路
同后序与中序遍历构造
#### 注意⚠️
是开始位置除去第一个root，加上分割区间长度。
```C++
//分割
        int leftPreorderBegin  = preorderBegin + 1;        
        int leftPreorderEnd    = preorderBegin + 1 + delimiterIndex - inorderBegin;

        int rightPreorderBegin = preorderBegin + 1 + delimiterIndex - inorderBegin;
        int rightPreorderEnd   = preorderEnd;
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
    TreeNode* traversal(vector<int>& preorder,int preorderBegin , int preorderEnd,vector<int>& inorder , int inorderBegin , int inorderEnd){
        if(preorderBegin == preorderEnd) return nullptr;

        //获得节点值
        int rootValue = preorder[preorderBegin];
        TreeNode *root = new TreeNode(rootValue);

        //只有一个节点，叶子节点
        if(preorderEnd - preorderBegin == 1) return root;

        //获得中序遍历的切割位置
        int delimiterIndex;
        for(delimiterIndex = inorderBegin ; delimiterIndex < inorderEnd ; delimiterIndex++){
            if(inorder[delimiterIndex] == rootValue){
                break;
            }
        }

        int leftInorderBegin   = inorderBegin;
        int leftInorderEnd     = delimiterIndex;
        int rightInorderBegin  = delimiterIndex + 1;
        int rightInorderEnd    = inorderEnd;

        //分割
        int leftPreorderBegin  = preorderBegin + 1;        
        int leftPreorderEnd    = preorderBegin + 1 + delimiterIndex - inorderBegin;

        int rightPreorderBegin = preorderBegin + 1 + delimiterIndex - inorderBegin;
        int rightPreorderEnd   = preorderEnd;

        root -> left  = traversal(preorder,leftPreorderBegin,leftPreorderEnd,inorder,leftInorderBegin,leftInorderEnd);
        root -> right = traversal(preorder,rightPreorderBegin,rightPreorderEnd,inorder,rightInorderBegin,rightInorderEnd);
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() == 0 || preorder.size() == 0){
            return nullptr;
        }
        return traversal(preorder,0,preorder.size(),inorder,0,inorder.size());
    }
};
```





