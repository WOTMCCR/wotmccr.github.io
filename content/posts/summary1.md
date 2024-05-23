---
categories: 算法总结
title: 二叉树遍历总结
tags:
  - 二叉树
date: 2024-05-23
description: 二叉树的遍历逐步讲解以及相关的实现
---
# 二叉树遍历总结
2024-05-23
回忆：今天学习了二叉树的四种遍历方式，先序遍历，中序遍历，后序遍历，层序遍历。分别使用递归以及迭代的方法实现了这四种遍历方式。
## 递归实现三种遍历
前中后三种遍历方式，分别代表访问节点的顺序所以，只要更改访问顺序即可实现。

- 传递当前节点，以及结果数组
- 当前节点为nullptr的时候返回
```C++
if(cur == nullptr) return;
```
- 然后下面三句的顺序，代表中左右，遍历，同理，当中序遍历和后序遍历的时候，更改这几行的顺序即可
```C++
		//先序遍历
        result.push_back(cur->val);
        traversal(cur->left,result);
        traversal(cur->right,result);
        
        //中序遍历
        traversal(cur->left,vec);  //遍历左
		vec.push_back(cur->val);   //取
		traversal(cur->right,vec); //遍历右
		
		//后序遍历
		traversal(cur->left,vec);  //遍历左
		traversal(cur->right,vec); //遍历右
		vec.push_back(cur->val);   //取
```


完整代码
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
		//先序遍历
        result.push_back(cur->val);
        traversal(cur->left,result);
        traversal(cur->right,result);
        
        //中序遍历
        traversal(cur->left,vec);  //遍历左
		vec.push_back(cur->val);   //取
		traversal(cur->right,vec); //遍历右
		
		//后序遍历
		traversal(cur->left,vec);  //遍历左
		traversal(cur->right,vec); //遍历右
		vec.push_back(cur->val);   //取
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root , result);
        return result;
    }
};
```

## 迭代法实现三种遍历
#### 先序遍历
使用栈模拟递归的过程，每次将第一个出栈取值，为了要保证中左右遍历。
需根据栈的特点要按：右节点，左节点的顺序入栈。
- 初始化:需要一个栈来模拟递归的过程，以及返回结果的数组，当root为空的时候可以直接返回，如果不为空，就将第一个节点入栈
```C++
		stack<TreeNode*> st;
        vector<int> result;
        if(root == nullptr) return result;
        st.push(root);
```
- 不断的读取栈中的元素，直到最后，到叶子节点没有新的元素
```C++
		while(!st.empty()){
            TreeNode *node = st.top();
            st.pop();
            result.push_back(node->val);
            //如果不为空入栈
            if(node->right != nullptr) st.push(node->right);
            if(node->left != nullptr) st.push(node->left);
        }
```
- 使用top()每次先读去中间的元素
```C++
		TreeNode *node = st.top();
        st.pop();
        result.push_back(node->val);
```
- 将中间元素的孩子如果不为空则按照右左入栈
```C++
if(node->right != nullptr) st.push(node->right);
            if(node->left != nullptr) st.push(node->left);
```

完整代码：
```C++
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

因为中序遍历需要先讲左子树遍历完
- 初始化过程相同
```C++
	stack<TreeNode*> st;
    vector<int> result;
```
- 循环逻辑有些不同,先取一个当前节点，只有当当前节点为空切栈为空时才退出循环
```C++
	TreeNode *cur = root;
    while(cur != nullptr || !st.empty())
```
- cur为一只遍历的指针和会一只指到叶子节点下面为null
```C++
		if(cur != nullptr){
        //如果当前节点不为空的话，则需要继续将左子树入栈
            st.push(cur);
            cur = cur->left;
        }
```
- 当cur为空，此时如果栈不为空，就代表左子树已经存储完成，可以进行取值了，是左的值。
	- 当cur是左叶子节点的时候，cur-right，也是nullptr所以还会进行一次取值，是中间节点的值，然后将中间节点的右孩子入栈。
```C++
    else{
    //当前节点为空的话，到达了最底层，需要遍历的节点在栈中
        TreeNode *node = st.top();
        st.pop();
        result.push_back(node->val);
        cur = node -> right;                
        }
```
- 当最后，cur的右孩子为nullptr，此时又已经将中间节点的右孩子弹出，这个时候栈空指针空，退出循环。

完整代码：
```C++
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
前序遍历的过程是**中左右**
后序遍历的过程是**左右中**
所以将前序遍历的左右遍历顺序颠倒为，中右左
```C++
result.push_back(node->val);
if(node->right != nullptr) st.push(node->right);
if(node->left != nullptr) st.push(node->left);
```

然后将最后的结果reverse ， 得到： 左右中
```C++
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

## 统一迭代法实现三种遍历
因为，中序遍历与其他两种遍历方法有较大差异，所以，应该有一种统一的迭代方法如递归一样，只需要调整顺序就可以达到，改变遍历顺序的效果
**使用标记法，标记将要访问的节点**
**在要处理的节点放入栈之后，紧接着放入一个空指针作为标记。** 这种方法可以叫做标记法。

- 初始化,需要一个栈来模拟递归，如果root不为null就入栈
```C++
    stack<TreeNode *> st;
    vector<int> result;
    if(root != nullptr) st.push(root);
```
- 循环取栈中的元素
```C++
		while(!st.empty()){
            TreeNode *node = st.top();
```
- 标记
```C++
		st.push(node);
        st.push(nullptr); //标记该处理这个节点
```
- 这时候有两种情况
	- 一种是取出来的节点为空，则代表是标记节点
	- 一种不为空，则继续遍历
- 不为空，则先讲这个节点pop出来，然后，按照需要的遍历顺序重新push并且，在需要放到结果集的元素的后面添加标记
	- 先序遍历，需要出栈顺序为中左右，所以，入栈顺序为右左中
	- 中序遍历，需要出栈顺序为左中右，所以，入栈顺序为右中左
	- 后序遍历，需要出栈顺序为左右中，所以，入栈顺序为中右左
```C++
		//先序遍历，入栈顺序为右左中
		if(node != nullptr){
            st.pop();
            //不为空则将右左中入
            if(node->right!= nullptr) st.push(node->right);
            if(node->left != nullptr) st.push(node->left);
            st.push(node);
            st.push(nullptr); //插入标记
        }
        
        //中序遍历，入栈顺序为右中左
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
        }
        //后序遍历入栈顺序为中右左
        if(node != nullptr){
            //继续入栈
            //中右左
            st.pop();
            st.push(node);
            st.push(nullptr);
            if(node->right !=nullptr) st.push(node->right);
            if(node->left != nullptr) st.push(node->left);
        }
        
```
- 如果，node为null意味着是标记位，该元素的下一个，为需要添加到结果集的元素
```C++
	else{
	        st.pop();
            result.push_back(st.top()->val);
            st.pop();
        }
```
### 完整实现
#### 先序遍历

```C++
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








## 迭代法层序遍历
需要借用一个辅助数据结构即队列来实现，**队列先进先出，符合一层一层遍历的逻辑，而用栈先进后出适合模拟深度优先遍历也就是递归的逻辑。**
**而这种层序遍历方式就是图论中的广度优先遍历**
在出队列的时候，将出队列元素的左右孩子加入队列
- 初始化结果数组以及需要需要的队列
```C++
	queue<TreeNode *> que;
    if(root != nullptr) que.push(root);
    vector<vector<int>> result;
```
- 每一层，出队列，并且，将下一层的节点加入队列中
	- 因为，每次都有下一层的元素入队列，所以，需要保存当前层的数量
```C++
//一层，因为会新加下一层的元素，所以使用size来记录这一层的元素个数
    int size = que.size();
    vector<int> tier;
```
- 读取当前层的全部元素，并将下一层的元素加入队列
```C++
for(int i = 0 ; i < size ; i++){
    TreeNode *node = que.front();
    que.pop();
    tier.push_back(node->val);
    if(node->left != nullptr) que.push(node->left);
    if(node->right != nullptr) que.push(node->right);
}
```
- 当下一层全部为nullptr的时候，队列为空结束

完整实现：
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