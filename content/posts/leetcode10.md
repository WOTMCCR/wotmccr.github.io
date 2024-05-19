---
categories: 算法
tags:
  - 栈
  - leetcode
date: 2024-05-19
title: 2024-05-19刷题
description: 20. 有效的括号 1047. 删除字符串中的所有相邻重复项 150. 逆波兰表达式求值
---
## 2024-05-19刷题
### 20. 有效的括号
19:01
#### 思路
因为这道题， `s` 仅由括号 `'()[]{}'` 组成而没有交错的情况，所以只需要判断栈顶的元素与当前元素是否匹配即可
#### 题解
##### C++
```C++
class Solution {
public:
    bool isValid(string s) {
        //如果数组大小为奇数则一定不满足
        if(s.length() % 2 != 0) return false;
        stack<int> in;
        //将s入栈，可以直接，将左括号输入成对应的右括号，然后当右括号到达的时候进行出栈判断进行匹配即可
        for(int i = 0 ; i < s.length() ; i++){
            if(s[i] == '(') in.push(')');
            else if(s[i] == '{') in.push('}');
            else if(s[i] == '[') in.push(']');
            else if(in.empty() || in.top()!= s[i]) return false;
            else in.pop();
        }
        return in.empty() ? true : false;
    }
};
```
### 1047. 删除字符串中的所有相邻重复项
19:02
#### 思路
匹配前一个字符是否是否一致，如果一致则，出栈。
#### 题解
##### C++
```C++
//reverse
class Solution {
public:
    string removeDuplicates(string S) {
        stack<char> st;
        for (char s : S) {
            if (st.empty() || s != st.top()) {
                st.push(s);
            } else {
                st.pop(); // s 与 st.top()相等的情况
            }
        }
        string result = "";
        while (!st.empty()) { // 将栈中元素放到result字符串汇总
            result += st.top();
            st.pop();
        }
        reverse (result.begin(), result.end()); // 此时字符串需要反转一下
        return result;

    }
};

//字符串直接作为栈
class Solution {
public:
    string removeDuplicates(string S) {
        string result;
        for(char s : S) {
            if(result.empty() || result.back() != s) {
                result.push_back(s);
            }
            else {
                result.pop_back();
            }
        }
        return result;
    }
};
```
### 150. 逆波兰表达式求值
19:20
#### 思路
如果为运算符则出栈两次，进行运算然后再入栈
#### 注意⚠️
如果是数字不好判断，则可以判断是否为运算符，然后将数字入栈
`in.push(stoll(token));`用来将str转换成longlong
#### 题解
##### C++
```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        long long result = 0;
        stack<long long> in;
        for(string token : tokens){            
            //如果为运算符则出栈两次，进行运算然后再入栈
            if(token == "+" || token == "-" || token == "*" || token == "/"){
                long long b = in.top();
                in.pop();
                long long a = in.top();
                in.pop();
                if(token == "+") in.push(a+b);
                if(token == "-") in.push(a-b);
                if(token == "*") in.push(a*b);
                if(token == "/") in.push(a/b);
            }else{
                //数字进栈
                in.push(stoll(token));
            }
        }
        return in.top();
    }
};
```












