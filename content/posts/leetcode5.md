---
tags:
  - leetcode
  - 哈希表
date: 2024-05-14
title: 2024-05-14刷题2
description: 哈希表理论基础 242.有效的字母异位词  349. 两个数组的交集  202. 快乐数 1. 两数之和
---
## 2024-05-14刷题
### 242.有效的字母异位词
20:22 20:47
#### 思路
很简单，遍历两个字符串，并将对应char的内容放入到unordered_map中，有重复的就将次数加一
之后再进行一次遍历判断两个数组中对应字符的个数是否相同
#### 题解
##### C++
```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()){
            return false;
        }
        unordered_map<char,int> a;
        unordered_map<char,int> b;
        for(int i = 0 ;i < s.length(); i++){
            a[s[i]]++;
            b[t[i]]++;
        }
        for(int i = 0 ;i < s.length() ; i++){
            if(a[s[i]] != b[s[i]])
                return false;
        }
        return true;
    }
};
```
##### java
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] record = new int[26];
        for (char c : s.toCharArray()) {
            record[c - 'a'] += 1;
        }
        for (char c : t.toCharArray()) {
            record[c - 'a'] -= 1;
        }
        for (int i : record) {
            if (i != 0) {
                return false;
            }
        }
        return true;
    }
}
```

###  349. 两个数组的交集
20:47 21:11
#### 思路
创建unordered_set集合，先将一个数组中的所有数都放到集合中，然后遍历另一个集合，如果包含于之前的哈希表，则将元素添加到结果集中。
#### 注意⚠️
STL的使用
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result;
        unordered_set<int> num_set(nums1.begin(),nums1.end());
        for(int num : nums2){
            if(num_set.find(num) != num_set.end()){
                result.insert(num);
            }            
        }
        return vector<int> (result.begin(),result.end());
    }
};
```
##### java
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0){
            return new int [0];
        }
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resSet = new HashSet<>();

        for(int i : nums1){
            set1.add(i);
        }

        for(int i : nums2){
            if(set1.contains(i)){
                resSet.add(i);
            }
        }
        return resSet.stream().mapToInt(x->x).toArray();
    }
}
```
##### go
```go
func intersection(nums1 []int, nums2 []int) []int {
    m := make(map[int]int)
    for _,v := range nums1 {
        m[v] = 1
    }
    var res [] int
    for _,v := range nums2 {
        if count,ok := m[v];ok&&count>0 {
            res = append(res,v)
            m[v]--
        }
    }
    return res
}
```
##### swift
```swift
class Solution {
    func intersection(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var set1 = Set<Int>()
        var set2 = Set<Int>()

        for num in nums1 {
            set1.insert(num)
        }

        for num in nums2 {
            if set1.contains(num){
                set2.insert(num)
            }
        }
        return Array(set2)
    }
}
```

### 202.快乐数
21:12
#### 思路
分两部分，一部分拆分每一位数字并存储到一个数组中
第二部分，将每一次拆分的结果存到一个集合中，下一次计算的时候需要判断集合中是否已经存在之前的数，如果存在则证明是一个循环
#### 注意⚠️
取模的运算优先级与乘除相同，从左往右计算
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> getdigit(int num){
        vector<int> result;
        while(num){
            result.push_back(num%10);
            num = num/10;
        }
        return result;
    }
    bool isHappy(int n) {
        unordered_set<int> a;
        while(n!=1){
            vector<int> digit = getdigit(n);
            n = 0;
            for(int num : digit){
                n += num * num;
            }
            if(a.find(n) != a.end()){
                return false;
            }
            a.insert(n);
        }
        return true;
    }
};
```
##### java
```java
class Solution {
public int getNextDigit(int n) {
    int sum = 0;
    while (n > 0) {
        int digit = n % 10; // 提取最后一位数字
        sum += digit * digit; // 计算最后一位数字的平方并累加到结果中
        n = n / 10; // 去掉最后一位数字
    }
    return sum;
}

    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextDigit(n);
        }
        return n == 1;
    }
}
```
##### go
```go
func isHappy(n int) bool {
    m := make(map[int]bool)
    for n != 1 && !m[n] {
        n, m[n] = getSum(n), true
    }
    return n == 1
}

func getSum(n int) int {
    sum := 0
    for n > 0 {
        sum += (n % 10) * (n % 10)
        n = n / 10
    }
    return sum
}
```
##### swift
```swift
class Solution {
    
// 计算一个数的每个位置上的数字的平方和
func getSum(_ number: Int) -> Int {
    var sum = 0
    var num = number
    while num > 0 {
        let temp = num % 10
        sum += (temp * temp)
        num /= 10
    }
    return sum
}

// 判断一个数是否为快乐数
func isHappy(_ n: Int) -> Bool {
    var set = Set<Int>()
    var num = n
    while num != 1 && !set.contains(num) {
        set.insert(num)
        num = getSum(num)
    }
    return num == 1
}

}
```


### 1.两数之和
22:00
#### 思路
遍历当前元素，并且寻找是否有匹配的key
没有找到则，将元素和下标加到map中
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            // 遍历当前元素，并在map中寻找是否有匹配的key
            auto iter = map.find(target - nums[i]); 
            if(iter != map.end()) {
                return {iter->second, i};
            }
            // 如果没找到匹配对，就把访问过的元素和下标加入到map中
            map.insert(pair<int, int>(nums[i], i)); 
        }
        return {};
    }
};
```
##### java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }
            map.put(nums[i], i);
        }
        // 如果找不到满足条件的数对，则返回一个空数组
        return new int[0];
    }
}

```
##### go
```go
func twoSum(nums []int, target int) []int {
    m := make(map[int]int)
    for index, val := range nums {
        if preIndex,ok := m[target-val];ok {
            return [] int {preIndex,index}
        }else{
            m[val] = index
        }
    }
    return [] int {}
}
```
##### swift
```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var map = [Int :Int]()
        for (i, e) in nums.enumerated() {
            if let v = map[target-e] {
                return [i,v]
            }else{
                map[e] = i
            }
        }
        return []
    }
}
```












--------

