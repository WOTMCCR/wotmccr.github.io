---
tags:
  - leetcode
  - 字符串
date: 2024-05-15
title: 2024-05-15刷题2
description: 344.反转字符串 541. 反转字符串II 卡码网：54.替换数字  151.翻转字符串里的单词 卡码网：55.右旋转字符串
---
## 2024-05-15刷题2
### 344.反转字符差
20:49
#### 思路
左右指针向中间遍历，进行交换
#### 复杂度
O(n)
#### 题解
##### C++
```C++
class Solution {
public:
    void reverseString(vector<char>& s) {
        auto left = s.begin();
        auto right = s.end() - 1;  // right should point to the last element

        while(left < right) {
            // Swap the characters pointed by left and right
            char temp = *left;
            *left = *right;
            *right = temp;

            // Move the iterators
            ++left;
            --right;
        }
    }
};

```
##### java
```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while(l < r){
            char temp = s[l];
            s[l++] = s[r];
            s[r--] = temp;
        }    
    }
}
```
##### go
```go
func reverseString(s []byte)  {
    l := 0
    r := len(s)-1
    for l < r {
        s[l], s[r] = s[r], s[l]
        l++
        r--
    }

}
```

### 541. 反转字符串II
21:07
#### 思路
循环增加2k个的区间长度，然后进行判断，k大小的区间是否合法，如果合法，则进行前k空间的交换，不合法则交换致末尾。
#### 复杂度
时间复杂度O(n)
空间复杂度O(1)
#### 题解
##### C++
```C++
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i = 0 ; i < s.length() ; i += (2*k) ){
            if(i+k >= s.length()){
                reverse(s.begin() + i , s.end());
            }else{
                reverse(s.begin() + i , s.begin() + i + k);
            }
        }
        return s;
    }
};
```
##### java
```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0;i < ch.length;i += 2 * k){
            int start = i;
            // 判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1,start + k - 1);
            while(start < end){
                
                char temp = ch[start];
                ch[start] = ch[end];
                ch[end] = temp;

                start++;
                end--;
            }
        }
        return new String(ch);
    }
}
```
### 替换数字
21:28
#### 思路
先将空间扩充为字符+数字更改为number之后的总长度
之后，双指针，空间结尾与数组结尾，向前遍历
如果为数字，则填充number
如果为字母，则移到后面
#### 题解
##### C++
```C++
#include<iostream>
using namespace std;
int main(){
    string s;
    cin>>s;
    int n = s.length()-1;
    int count = 0;
    for(int i = 0 ; i<=n;i++){
         if (s[i] >= '0' && s[i] <= '9') {
             count++;
         }
    }
    s.resize(s.size()+5*count);
    int r = s.size()-1;
    int l = n;
    while(l>=0){
        //如果为数字，则补number
        if (s[l] >= '0' && s[l] <= '9'){
                s[r--] = 'r';
                s[r--] = 'e';
                s[r--] = 'b';
                s[r--] = 'm';
                s[r--] = 'u';
                s[r--] = 'n';
        }else{
            s[r--] = s[l];
        }
        l--;
    }
    cout << s << endl;
}
```

### 151.反转字符串中的单词
21:48
#### 思路
先将字符串全部反转，并且移除掉多余的空格
之后，再根据空格，对单词进行反转
#### 注意⚠️
删除空格的操作比较复杂
#### 题解
##### C++
```C++
class Solution {
public:
    //删除多余空格
    void removeExtraSpace(string &s){
        int slow = 0;
        for(int i = 0 ; i<s.size();i++){
            //去掉开头的空格
            //并且，当单词间有多个空格的时候，直接跳过
            if(s[i] != ' '){
                //添加单词间的空格
                //当到单词尾的时候，添加空格
                if(slow != 0)
                    s[slow++] = ' ';

                //将单词向左移动
                while(i<s.size()&&s[i]!=' '){
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow);
    }

    //reverse
    void reverse(string &s , int begin , int end){
        while(begin<end){
            swap(s[begin++],s[end--]);
        }
    }
    string reverseWords(string s) {
        removeExtraSpace(s);
        //从头到尾reverse
        reverse(s,0,s.size()-1);

        int start = 0;
        //逐个单词reverse
        for(int i = 0 ; i <= s.size(); i++){
            //如果为空格则前面为单词
            if(s[i] == ' ' || i == s.size()){
                reverse(s,start,i-1);
                start = i + 1;
            }
        }
        return s;
    }
};
```
##### java
```java

```
##### go
```go

```
















### 卡码网：55.右旋转字符串
22:16
#### 思路
将后k位移到前k位，可以先将，这个字符串进行反转，这样，后面的内容就到了前面，然后，再分别对前k位和后n-k位进行反转。
#### 题解
##### C++
```C++
#include<iostream>
#include<algorithm>
using namespace std;

void reverse(string &s , int begin ,int end){
    while(begin<end){
        swap(s[begin++],s[end--]);
    }
}

int main(){
    //输入
    int k;
    cin >> k;
    string s;
    cin>>s;
    reverse(s,0,s.size()-1);
    reverse(s,0,k-1);
    reverse(s,k,s.size()-1);
    cout<<s<<endl;
    
}
```

