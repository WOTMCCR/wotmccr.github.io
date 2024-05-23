---
categories: 算法
tags:
  - leetcode
  - 树
title: 2024-05-23刷题
date: 2024-05-23
description: 递归以及迭代，和统一迭代实现先序，中序，后序遍历
---
## 2024-05-23 刷题
### 递归实现树的三种遍历
#### 先序遍历
打印出二叉树的先序遍历的节点
- 确定参数与返回值：
	因为要打印出前序遍历节点的数值，所以参数里需要传入vector来放节点的数值不需要什么返回值
```C++
void traversal(TreeNode* cur, vector<int>& vec)
```

- 确定终止条件：
	当前遍历的节点是空了，则本次递归结束
```C++
if (cur == NULL) return;
```

- 确定单层递归的逻辑
	前序遍历是中左右的循序，所以在单层递归的逻辑，是要先取中节点的数值，代码如下：
```C++
vec.push_back(cur->val);    // 取值
traversal(cur->left, vec);  // 遍历左树
traversal(cur->right, vec); // 遍历右树
```
##### 完整代码
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
    void traversal(TreeNode *cur , vector<int> &result){
        if(cur == nullptr) return;
        result.push_back(cur->val);
        traversal(cur->left,result);
        traversal(cur->right,result);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root , result);
        return result;
    }
};
```

#### 中序遍历
同先序遍历的返回值与终止条件
因为中序遍历的遍历顺序是，左中右，所以在单层递归中的逻辑是先遍历左树然后取值，之后遍历右子树。
```C++
traversal(cur->left,vec);  //遍历左
vec.push_back(cur->val);   //取
traversal(cur->right,vec); //遍历右
```
##### 完整代码
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
    void traversal(TreeNode *cur , vector<int> &result){
        if(cur == nullptr) return;
        traversal(cur->left , result);
        result.push_back(cur->val);
        traversal(cur->right , result);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root , result);
        return result;
    }
};
```
#### 后序遍历
```C++
traversal(cur->left,vec);  //遍历左
traversal(cur->right,vec); //遍历右
vec.push_back(cur->val);   //取
```
##### 完整代码
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
    void traversal(TreeNode *cur , vector<int> &result){
        if(cur == nullptr) return;
        traversal(cur->left , result);
        traversal(cur->right , result);
        result.push_back(cur->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root,result);
        return result;
    }
};
```

### 迭代实现遍历的三种方式
#### 先序遍历
使用栈模拟递归的过程，每次将第一个出栈取值，为了要保证中左右遍历，根据栈的特点要按：右节点，左节点的顺序入栈。
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
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if(root == nullptr) return result;
        st.push(root);
        while(!st.empty()){
            TreeNode *node = st.top();
            st.pop();
            result.push_back(node->val);
            //如果不为空入栈
            if(node->right != nullptr) st.push(node->right);
            if(node->left != nullptr) st.push(node->left);
        }
        return result;
    }
};
```
#### 中序遍历
使用栈记录遍历的过程，一直遍历到左子树为空，出栈当前元素，并且将右子树添加到栈中。
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
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        TreeNode *cur = root;
        while(cur != nullptr || !st.empty()){
            if(cur != nullptr){
                //如果当前节点不为空的话，则需要继续将左子树入栈
                st.push(cur);
                cur = cur->left;
            }else{
                //当前节点为空的话，到达了最底层，需要遍历的节点在栈中
                TreeNode *node = st.top();
                st.pop();
                result.push_back(node->val);
                cur = node -> right;                
            }
        }
        return result;
    }
};
```
#### 后序遍历
前序遍历的过程是中左右
后序遍历的过程是左右中
所以将前序遍历的左右遍历顺序颠倒为，中右左
然后将最后的结果reverse ， 得到： 左右中
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
    vector<int> postorderTraversal(TreeNode* root) {
        //后序遍历左右中
        stack<TreeNode *> st;
        vector<int> result;
        if(root == nullptr) return result;
        st.push(root);
        while(!st.empty()){
            TreeNode *node = st.top();
            st.pop();
            result.push_back(node->val);
            if(node->right != nullptr) st.push(node->right);            
            if(node->left != nullptr) st.push(node->left);
        }
        reverse(result.begin(),result.end());
        return result;
    }
};
```
### 统一迭代实现遍历的三种方式
**将访问的节点放入栈中，把要处理的节点也放入栈中但是要做标记。**
**在要处理的节点放入栈之后，紧接着放入一个空指针作为标记。** 这种方法也可以叫做标记法。

#### 关键
空指针标记需要放到结果集中的元素
#### 先序遍历
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
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode *> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode *node = st.top();
            if(node != nullptr){
                st.pop();
                //不为空则将右左中入
                if(node->right!= nullptr) st.push(node->right);
                if(node->left != nullptr) st.push(node->left);
                st.push(node);
                st.push(nullptr); //插入标记
            }else{
                //为空，是标记，意味着下一个该入结果集
                st.pop();
                result.push_back(st.top()->val);
                st.pop();
            }
        }
        return result;         
    }
};
```
#### 中序遍历
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
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode *> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode *node = st.top();
            if(node != nullptr){
                //该节点不为空，开始处理该节点的左中右
                st.pop(); //弹出当前节点，避免多次处理
                //右中左入栈，因为栈是先进后出

                //右
                if(node->right != nullptr) st.push(node->right);
                //中
                st.push(node);
                st.push(nullptr); //标记该处理这个节点
                //左
                if(node->left !=  nullptr) st.push(node->left);
            }else{
                //为空，所以下一个元素应该存到result中
                st.pop();
                result.push_back(st.top()->val);
                st.pop();
            }
        }
        return result;
    }
};
```
#### 后序遍历
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
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            //看看当前是否为标记，如果不是标记，则说明，还没遍历完
            TreeNode *node = st.top();
            if(node != nullptr){
                //继续入栈
                //中右左
                st.pop();
                st.push(node);
                st.push(nullptr);
                if(node->right !=nullptr) st.push(node->right);
                if(node->left != nullptr) st.push(node->left);
            }else{
                st.pop();
                result.push_back(st.top()->val);
                st.pop();
            }
        }
        return result;
    }
};
```
