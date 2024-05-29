---
categories: 算法
title: 2024-05-29刷题
tags:
  - 二叉树
  - leetcode
date: 2024-05-29
description: 530.二叉搜索树的最小绝对差  501.二叉搜索树中的众数 236. 二叉树的最近公共祖先
---
## 2024-05-28刷题
### 530.二叉搜索树的最小绝对差
14:01
#### 思路
因为是使用二叉搜索树，所以使用中序遍历就是从小到大遍历，当前节点与之前节点的值相减，如果，小于最小值则替换。

所以，递归函数的参数应该有当前节点与上一个节点，可以将最小值放在外面。
#### 注意⚠️
prev 节点的确定
prev必须是最左下角，最小元素。
#### 题解
##### C++
递归
```C++
class Solution {
public:
    TreeNode *prev = nullptr;
    int Min = INT_MAX;
    void traversal(TreeNode* root) {
        if(root == nullptr) return;
        traversal(root->left);
        if(prev!=nullptr && abs(root->val - prev->val) < Min){
            Min = abs(root->val - prev->val);
        }
        prev = root;
        traversal(root->right);
        return;
    }
    int getMinimumDifference(TreeNode * root){
        traversal(root);
        return Min;
    }
};
```
迭代：

```C++
class Solution {
public:
    //使用迭代法
    int getMinimumDifference(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode *cur = root;
        TreeNode *pre = nullptr;
        int Min = INT_MAX;
        //cur可能到叶子节点下面
        //还可能栈中还有其他元素
        while(cur != nullptr || !st.empty()){
            //所以这里有两种情况，cur为null ，不为null
            //按照左，中，右处理
            if(cur != nullptr){
                //入栈意味着不处理
                //左
                st.push(cur);
                //遍历
                cur = cur->left;
            }else{
                //中
                cur = st.top();
                st.pop();

                if(pre != nullptr && abs(pre->val - cur->val) < Min){
                    Min = abs(pre->val - cur->val);
                }
                pre = cur;
                //右
                cur = cur->right;
            }            
        }
        return Min;
    }
};
```

### 501. 二叉搜索树中的众数
15:19
#### 思路
还是两个前后的节点，如果前后的值相同则，累加，不同则进行比较修改结果集合。
#### 复杂度
#### 注意⚠️
单个元素需要单独处理
```C++
if (pre == nullptr) { // 第一个节点
            count = 1;
        } else if (pre->val == cur->val) { // 与前一个节点数值相同
            count++;
        } else { // 与前一个节点数值不同
            count = 1;
        }
        pre = cur;
        if(count == maxCount){
            result.push_back(cur->val);
        }
        if(count > maxCount){
            maxCount = count;
            result.clear();
            result.push_back(cur->val);
        }
```
#### 题解
##### C++
```C++
class Solution {
public:
    int maxCount = 0; // 最大频率
    int count = 0; // 统计频率
    TreeNode* pre = nullptr;
    vector<int> result;
    void traversal(TreeNode * cur){
        if(cur == nullptr) return;
        traversal(cur->left);

        if (pre == nullptr) { // 第一个节点
            count = 1;
        } else if (pre->val == cur->val) { // 与前一个节点数值相同
            count++;
        } else { // 与前一个节点数值不同
            count = 1;
        }
        pre = cur;
        if(count == maxCount){
            result.push_back(cur->val);
        }
        if(count > maxCount){
            maxCount = count;
            result.clear();
            result.push_back(cur->val);
        }
        traversal(cur->right);
        return;
    }
    vector<int> findMode(TreeNode* root) {
        traversal(root);
        return result;
    }
};
```

### 236.二叉树的最近公共祖先
15:28
#### 思路
我确实在想，要是能自底向上查找就好了
所以回溯
后序遍历（左右中）就是天然的回溯过程，可以根据左右子树的返回值，来处理中节点的逻辑。

递归
- 确定递归函数返回值以及参数
来告诉我们是否找到节点q或者p，那么返回值为bool类型就可以了。
但我们还要返回最近公共节点，可以利用上题目中返回值是TreeNode * ，那么如果遇到p或者q，就把q或者p返回，返回值不为空，就说明找到了q或者p。
```C++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q)
```
- 确定终止条件
 如果root 为p，q，null均返回。
```C++
if(root == p || root == q || root == NULL) return root;
```
- 递归逻辑
后序遍历，左右中，比较左右的值，如果都不为空则意味着找到结果，直接返回。
有不为空的值则返回对应的值。
```C++
		TreeNode *left = lowestCommonAncestor(root->left,p,q);
        TreeNode *right = lowestCommonAncestor(root->right,p,q);
        if (left != NULL && right != NULL) return root;
        
        if(left == NULL && right != NULL) return right;
        else if(left != NULL && right == NULL) return left;
        else {
            return NULL;
        }
```
#### 题解
##### C++
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == p || root == q || root == NULL) return root;

        TreeNode *left = lowestCommonAncestor(root->left,p,q);
        TreeNode *right = lowestCommonAncestor(root->right,p,q);
        if (left != NULL && right != NULL) return root;
        
        if(left == NULL && right != NULL) return right;
        else if(left != NULL && right == NULL) return left;
        else {
            return NULL;
        }

    }
};
```







