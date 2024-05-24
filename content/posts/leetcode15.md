---
categories: 算法
title: 2024-05-24刷题
tags:
  - leetcode
  - 树
date: 2024-05-24
description: 110.平衡二叉树 257.二叉树的所有路径 404.左子树之和
---
## 2024-05-24
### 110.平衡二叉树
10:36
#### 思路
平衡二叉树为，该树所有节点的左右子树的深度相差不超过1
- 二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数。
- 二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数。
##### 递归法：
其实，各种遍历方法中对中节点的访问，并不仅仅只能是取值，也代表着访问顺序。
可以根据这一点，来选取不同的遍历方式处理问题。
比如求平衡二叉树，我需要知道当前节点的左右子树的高度然后进行比较，所以我需要先访问左右再访问当前节点进行高度差的计算。

这个顺序符合左右中的后序遍历顺序，所以可以使用后序遍历解决这个问题。

- 确定递归函数的参数以及返回值，因为需要知道高度所以设定返回值为高度，传递当前节点
```C++
int getHeight(TreeNode * node)
```
- 确定终止条件，当当前节点为null的时候也就是到达叶子节点的孩子，返回高度为0，终止递归
```C++
        //终止条件
        if(node == nullptr){
            return 0;
        }
```
- 单层递归逻辑，调用递归函数求左高度和右高度，如果高度返回值是-1代表已经不是平衡二叉树了，将返回结果一直传递。
```C++
		//左子树的高度 左
        int leftHeight = getHeight(node->left);
        //如果返回值有-1要一直传递
        if(leftHeight == -1) return -1;
        //右子树的高度 右
        int rightHeight = getHeight(node->right);
        if(rightHeight == -1) return -1;    
```
- 求高度差，如果高度差大于1则返回-1证明不是平衡二叉树，如果没有大于1，则选取左右子树的最大高度，再加上自己节点的高度1，向上返回。
```C++
        //如果相差大于1返回-1代表不是平衡二叉树
        if(abs(leftHeight - rightHeight) > 1){ // 中
            return -1;
        }else{
            return max(leftHeight,rightHeight)+1;
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
    //递归
    int getHeight(TreeNode * node){
        //终止条件
        if(node == nullptr){
            return 0;
        }
        //左子树的高度 左
        int leftHeight = getHeight(node->left);
        //如果返回值有-1要一直传递
        if(leftHeight == -1) return -1;
        //右子树的高度 右
        int rightHeight = getHeight(node->right);
        if(rightHeight == -1) return -1;        

        //如果相差大于1返回-1代表不是平衡二叉树
        if(abs(leftHeight - rightHeight) > 1){ // 中
            return -1;
        }else{
            return max(leftHeight,rightHeight)+1;
        }
    }
    bool isBalanced(TreeNode* root) {
        if(getHeight(root) == -1){
            return false;
        }else{
            return true;
        }
    }
};
```





### 257.二叉树的所有路径
11:02
#### 思路
回溯思想，到每个节点的时候把路径记录下来，需要回溯来回退一个路径再进入另一个路径。

每遍历到一个节点需要先将自己的节点加入到路径中
所以，使用先序遍历，中左右，访问父节点的时候将自己的节点加入到路径中。
##### 递归法
- 递归函数参数以及返回值
要传入根节点，是用vector记录每一条路径的path，和存放结果集的result，
```C++
void traversal(TreeNode* cur, vector<int>& path, vector<string>& result)
```
- 终止条件,遇到叶子节点，
```C++
if (cur->left == NULL && cur->right == NULL) { // 遇到叶子节点
    string sPath;
    for (int i = 0; i < path.size() - 1; i++) { // 将path里记录的路径转为string格式
        sPath += to_string(path[i]);
        sPath += "->";
    }
    sPath += to_string(path[path.size() - 1]); // 记录最后一个节点（叶子节点）
    result.push_back(sPath); // 收集一个路径
    return;
}
```
- 确定单层递归逻辑：因为是前序遍历，需要先处理中间节点，中间节点就是我们要记录路径上的节点，先放进path中。
- 并且，回溯，需要与递归的过程对应
```C++
if (cur->left) {
    traversal(cur->left, path, result);
    path.pop_back(); // 回溯
}
if (cur->right) {
    traversal(cur->right, path, result);
    path.pop_back(); // 回溯
}
```

##### 迭代法
使用栈模拟递归过程，两个栈一个用来存储节点，一个用来存储到达该节点的路径。
```C++
        stack<TreeNode*> st;
        stack<string> st_path;
        vector<string> result;
        
        if(root == nullptr) return result;
        st.push(root);
        st_path.push(to_string(root->val));
```
先序遍历，先访问中间节点
```C++
        while(!st.empty()){
            //取当前路径
            TreeNode *node = st.top();
            st.pop();
            string path = st_path.top();
            st_path.pop();
```
如果当前节点的左右孩子都空则是叶子节点，需要将当前节点的路径添加到结果集中
```C++
            if(node->left == nullptr && node->right == nullptr){
                result.push_back(path);
            }
```
遍历右左节点(因为栈模拟先进后出， 左右->右左入栈)
将节点入栈，并将该节点加到路径上。
```C++
if(node->right){ 
                st.push(node->right);
                st_path.push(path + "->" + to_string(node->right->val));
            }
            if(node->left){ 
                st.push(node->left);
                st_path.push(path + "->" + to_string(node->left->val));
            }
```



#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
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
    //迭代法，先序遍历
    vector<string> binaryTreePaths(TreeNode* root) {
        //迭代法的框架
        stack<TreeNode*> st;
        stack<string> st_path;
        vector<string> result;

        if(root == nullptr) return result;
        st.push(root);
        st_path.push(to_string(root->val));
        while(!st.empty()){
            //取当前路径
            TreeNode *node = st.top();
            st.pop();
            string path = st_path.top();
            st_path.pop();
            if(node->left == nullptr && node->right == nullptr){
                result.push_back(path);
            }
            if(node->right){ 
                st.push(node->right);
                st_path.push(path + "->" + to_string(node->right->val));
            }
            if(node->left){ 
                st.push(node->left);
                st_path.push(path + "->" + to_string(node->left->val));
            }

        }
        return result;
    }
};
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
    void traversal(TreeNode* cur, vector<int>& path, vector<string>& result) {
        //到达该节点
        path.push_back(cur->val);
        if(cur->left == nullptr && cur->right == nullptr){
            string sPath;
            //添加路径到结果集合
            for (int i = 0; i < path.size() - 1; i++) {
                sPath += to_string(path[i]);
                sPath += "->";
            }
            sPath += to_string(path[path.size() - 1]);
            result.push_back(sPath);
            return;
        }
        //进入左右
        if(cur->left){
            traversal(cur->left , path,result);
            //回溯，从这个递归出来，要将加入的点，弹出
            path.pop_back();
        }
        if(cur->right){
            traversal(cur->right,path,result);
            path.pop_back();
        }
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        vector<int> path;
        if(root == nullptr) return result;
        traversal(root , path , result);
        return result;
    }
};
```
### 404.左子树之和
13:11
#### 思路
层序遍历，如果是左的且是叶子节点，则将其值sum
#### 复杂度
#### 注意⚠️
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
    int sumOfLeftLeaves(TreeNode* root) {
        queue<TreeNode*> que;
        int sum = 0;
        if(root) que.push(root);
        while(!que.empty()){
            int size = que.size();
            for(int i = 0 ; i < size ; i++){
                TreeNode * node = que.front();
                que.pop();
                if(node->left){
                    que.push(node->left);
                    if(node->left->right == nullptr && node->left->left == nullptr){
                    sum+=node->left->val;
                    }
                }
                if(node->right){
                    que.push(node->right);
                }
            }
        }
        return sum;
    }
};
//迭代先序
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        stack<TreeNode*> st;
        if (root == NULL) return 0;
        st.push(root);
        int result = 0;
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            if (node->left != NULL && node->left->left == NULL && node->left->right == NULL) {
                result += node->left->val;
            }
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
        return result;
    }
};

//递归
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == NULL) return 0;
        if (root->left == NULL && root->right== NULL) return 0;

        int leftValue = sumOfLeftLeaves(root->left);    // 左
        if (root->left && !root->left->left && !root->left->right) { // 左子树就是一个左叶子的情况
            leftValue = root->left->val;
        }
        int rightValue = sumOfLeftLeaves(root->right);  // 右

        int sum = leftValue + rightValue;               // 中
        return sum;
    }
};
```



