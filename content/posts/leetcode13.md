---
categories: 算法
title: 2024-05-23刷题2
tags:
  - leetcode
  - 队列
  - 树
date: 2024-05-23
description: " 层序遍历  10  226.翻转二叉树  101.对称二叉树 2"
---
## 2024-05-23 刷题2
### 二叉树层次遍历
#### 思路
需要借用一个辅助数据结构即队列来实现，**队列先进先出，符合一层一层遍历的逻辑，而用栈先进后出适合模拟深度优先遍历也就是递归的逻辑。**
**而这种层序遍历方式就是图论中的广度优先遍历**
在出队列的时候，将出队列元素的左右孩子加入队列
1. 初始化结果数组以及需要需要的队列
2. 每一层，出队列，并且，将下一层的节点加入队列中
	1. 因为，每次都有下一层的元素入队列，所以，需要保存当前层的数量
3. 当下一层全部为nullptr的时候，队列为空结束

#### 实现
非递归
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode *> que;
        if(root != nullptr) que.push(root);
        vector<vector<int>> result;
        while(!que.empty()){
            //一层，因为会新加下一层的元素，所以使用size来记录这一层的元素个数
            int size = que.size();
            vector<int> tier;
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                tier.push_back(node->val);
                if(node->left != nullptr) que.push(node->left);
                if(node->right != nullptr) que.push(node->right);
            }
            result.push_back(tier);
        }
        return result;
    }
};
```

### 107.二叉树的层次遍历 II
16:19
#### 思路
层次遍历，然后reverse结果

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> qu;
        vector<vector<int>> result;
        if(root != nullptr) qu.push(root);
        while(!qu.empty()){
            int size = qu.size();
            vector<int> tier;
            //遍历当前层
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = qu.front();
                qu.pop();
                tier.push_back(node->val);
                if(node->left!=nullptr) qu.push(node->left);
                if(node->right!=nullptr) qu.push(node->right);
            }
            result.push_back(tier);
        }
        reverse(result.begin(),result.end());
        return result;
    }
};
```


### 199.二叉树的右视图
17:02
#### 思路
层次遍历，如果是最后一个元素则添加到结果集中
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
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> que;    
        vector<int> result;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                if(node->left != nullptr) que.push(node->left);
                if(node->right != nullptr) que.push(node->right);
                if(i == size-1){
                    result.push_back(node->val);
                }
            }
        }
        return result;
    }
};
```




### 637. 二叉树的层平均值
17:04
#### 思路
层次遍历求平均值
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
    vector<double> averageOfLevels(TreeNode* root) {
        //数据初始化需要结果集合以及用于存储元素的队列
        queue<TreeNode*> que;
        vector<double> result;
        if(root != nullptr) que.push(root);
        //每次循环都是一层
        while(!que.empty()){
            double sum = 0;
            int size = que.size();
            //当前层的所有元素
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                //将该元素的下一层的元素存到队列中
                if(node->left != nullptr) que.push(node->left);
                if(node->right != nullptr) que.push(node->right);
                
                sum += (double)node->val;
            }
            result.push_back(sum/size);
        }
        return result;
    }
};
```
### 429. N 叉树的层序遍历
17:19
#### 思路
node里有一个孩子的数组，遍历该数组，如果不为nullptr则添加到队列中
#### 题解
##### C++
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        //初始化
        queue<Node*> que;
        vector<vector<int>> result;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> tier;
            for(int i = 0 ; i < size ; i++){
                Node *node = que.front();
                que.pop();
//                vector<Node*> children;
                for(Node * chiled : node->children){
                    if(chiled != nullptr) que.push(chiled);
                }
                tier.push_back(node->val);
            }
            result.push_back(tier);
        }
        return result;        
    }
};
```
### [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)
17:20
#### 思路
层次遍历，取最大值
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
    vector<int> largestValues(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if(root!=nullptr) que.push(root);

        while(!que.empty()){
            int size = que.size();
            int max = INT_MIN;

            for(int i = 0 ; i < size ; i++){

                TreeNode *node = que.front();
                que.pop();
                if(node->val > max) max = node->val;

                if(node->left != nullptr) que.push(node->left);                
                if(node->right!= nullptr) que.push(node->right);
            }

            result.push_back(max);
        }
        return result;
    }
};
```

### 116. 填充每个节点的下一个右侧节点指针
17:29
#### 思路
两个node，pre和node，如果为0的时候，是开头的则将pre赋值，不是0则将其他的node赋值，并将pre的next指向node
循环结束将next赋值为null
#### 题解
##### C++
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);

        while (!que.empty()) {
            int size = que.size();
            // vector<int> vec;
            Node* nodePre;
            Node* node;
             for (int i = 0; i < size; i++) {
                if (i == 0) {
                    nodePre = que.front(); // 取出一层的头结点
                    que.pop();
                    node = nodePre;
                } else {
                    node = que.front();
                    que.pop();
                    nodePre->next = node; // 本层前一个节点next指向本节点
                    nodePre = nodePre->next;
                }
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
             }
             nodePre -> next = NULL;
        }
        return root;
    }
};
```

### 117.填充每个节点的下一个右侧节点指针II
17:49
#### 思路
一样
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            Node *nodepre;
            Node *node;
            for(int i = 0 ; i < size ; i++){
                if(i == 0){
                    //第一个涉及到pre的更新
                    nodepre = que.front();
                    que.pop();
                    node = nodepre;
                }else{
                    node = que.front();
                    que.pop();
                    nodepre->next = node;
                    nodepre = node;                    
                }
                //添加下一层
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            node->next = NULL;
        }
        return root;
    }
};
```

### 104.二叉树的最大深度
18:00
#### 思路
层次遍历，每层+1
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
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> que;
        if(root != nullptr) que.push(root);
        int maxd = 0;
        while(!que.empty()){
            int size = que.size();
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            maxd++;
        }
        return maxd;
    }
};
```

### 111. 二叉树的最小深度
18:30
#### 思路
每层记录深度，如果左右孩子为nullptr则，当前深度与最小深度进行比较。
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
    int minDepth(TreeNode* root) {
        queue<TreeNode*> que;
        int min = INT_MAX;
        if(root) 
            que.push(root);
        else
            return 0;

        int temp = 1;
        while(!que.empty()){
            int size = que.size();
            
            for(int i = 0 ; i < size ; i++){
                TreeNode *node = que.front();
                que.pop();

                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);

                if(node -> left == nullptr && node->right == nullptr){
                    if(temp < min){
                        min = temp;
                    }
                }
            }
            temp++;
        }
        return min;
    }
};
```



###  翻转二叉树
18:37
#### 思路
相当于前序遍历取值的过程变成了交换的过程；
#### 复杂度
#### 注意⚠️
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
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;
        //相当于前序遍历取值的过程变成了交换的过程；
        swap(root->left,root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```
迭代：
前序：
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
    TreeNode* invertTree(TreeNode* root) {
        stack<TreeNode*> st;
        if(root) st.push(root);
        while(!st.empty()){
            TreeNode *node = st.top();
            st.pop();
            swap(node->left , node->right);
            if(node->left)  st.push(node->left);
            if(node->right) st.push(node->right);
        }
        return root;
    }
};
```
统一法
```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop();
                if (node->right) st.push(node->right);  // 右
                if (node->left) st.push(node->left);    // 左
                st.push(node);                          // 中
                st.push(NULL);
            } else {
                st.pop();
                node = st.top();
                st.pop();
                swap(node->left, node->right);          // 节点处理逻辑
            }
        }
        return root;
    }
};
```
层序
```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                swap(node->left, node->right); // 节点处理
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return root;
    }
};
```



### 101. 对称二叉树
18:38
#### 思路
使用迭代法，相当于，同时遍历左右两个树，左边添加右节点，则右面添加左节点。
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
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);
        while(!que.empty()){
            TreeNode *left = que.front(); que.pop();
            TreeNode *right = que.front(); que.pop();
            //左右为空
            if(!left&&!right){
                continue;
            }
            if(!left && right || left && !right || left->val != right->val){
                return false;
            }

            que.push(left->left);
            que.push(right->right);
            que.push(left->right);
            que.push(right->left);
        }
        return true;        
    }
};
```






















