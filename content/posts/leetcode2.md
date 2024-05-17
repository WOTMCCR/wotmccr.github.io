---
categories: 算法
tags:
  - leetcode
  - 数组
date: 2024-05-10
description: 977.有序数组的平方 ，209.长度最小的子数组 ，59.螺旋矩阵II ，总结
title: 2024-05-10刷题2
---

## 2024-05-10 刷题
### 977.有序数组的平方
1h20min
#### 思路
我原本想在一个数组上进行处理，但发现不行。
所以，新建一个数组，然后在原数组上从左右开始向中间遍历，如果大的就添加到新的数组中，并更新新数组的下标
#### 复杂度
时间复杂度：O(n) 。n 的长度为数组的长度。
空间复杂度：O(1)。除了存储答案的数组以外，我们只需要维护常量空间。
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector <int> sorted(nums.size(),0);
        int l = 0;
        int r = nums.size() - 1;
        int k = r;
        while(l<=r){
            if(nums[r]*nums[r] > nums[l]*nums[l]){
                sorted[k--] = nums[r] * nums[r];
                r--;
            }else{
                sorted[k--] = nums[l] * nums[l];
                l++;
            }
        }
        return sorted;
    }
};
```
##### java
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int [] result = new int [n];
        int l = 0;
        int r = n - 1;
        int k = r;
        while(l <= r){
            if(nums[r]*nums[r] > nums[l]*nums[l]){
                result[k--] = nums[r]*nums[r];
                r--;
            }else{
                result[k--] = nums[l]*nums[l];
                l++;
            }
        }
        return result;

    }
}
```
##### go
```go
func sortedSquares(nums []int) []int {
    n := len(nums)
    result := make([] int, n)
    i,j,k:=0,n-1,n-1
    for i <= j{
        if nums[j]*nums[j] > nums[i]*nums[i]{
            result[k] = nums[j]*nums[j]
            j--
        }else{
            result[k] = nums[i]*nums[i]
            i++
        }
        k--
    }
    return result
}
```
##### swift
```swift
class Solution {
    func sortedSquares(_ nums: [Int]) -> [Int] {
        var n = nums.count - 1
        var k = n
        var l = 0
        var r = n
        var result = Array<Int>(repeating:0,count:nums.count)
        while l <= r{
            if(nums[r]*nums[r]>nums[l]*nums[l]){
                result[k] = nums[r]*nums[r]
                r -= 1
            }else{
                result[k] = nums[l]*nums[l]
                l += 1
            }
            k-=1
        }
        return result
    }
}
```

### 209.长度最小的子数组
#### 思路
使用滑动窗口，窗口的左右两端从左往右进行移动
右边界移动增加sum，当sum超过target的时候，移动左边界，减少sum，并更新最短的长度。
也就是根据当前子序列的大小不断调节子序列的边界
#### 复杂度
- 时间复杂度：O(n)
- 空间复杂度：O(1)
#### 注意⚠️
min长度大小计算需要+1
#### 题解
##### C++
```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size() - 1;
        if(n == -1)
        {
            return 0;
        }
        int l = 0;
        int r = 0;
        int sum = 0;
        int min = INT_MAX;
        while(r <= n){
            sum+=nums[r];
            while(sum >= target){
                min = (min > r-l+1) ? r-l+1:min;
                sum-=nums[l++];
            }
            r++;
        }
        return min == INT_MAX ? 0:min;
    }
};
```
##### java
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length - 1;
        int min = Integer.MAX_VALUE;
        int l = 0;
        int r = 0;
        int sum = 0;
        while( r<=n ){
            sum+=nums[r];
            while(sum >= target){
                min = Math.min(min,r-l+1);
                sum -= nums[l++];
            }
            r++;
        }
        return min == Integer.MAX_VALUE ? 0:min;
    }
}
```
##### go
```go
func minSubArrayLen(target int, nums []int) int {
    n := len(nums)
    min := n+1
    l,r,sum := 0,0,0
    for r <= n-1 {
        sum += nums[r]
        for sum >= target{
            if r-l+1 < min {
                min = r - l + 1
            }
            sum -= nums[l]
            l++
        }
        r++
    }
    if min == n+1 {
        return 0
    } else {
        return min
    }
}
```
##### swift
```swift
class Solution {
    func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
        var l = 0
        var r = 0
        var sum = 0
        var result = Int.max
        var n = nums.count-1
        while(r <= n){
            sum += nums[r]
            while(sum >= target){
                result = min(r-l+1,result)
                sum -= nums[l]
                l+=1
            }
            r += 1
        }
        return result == Int.max ? 0:result
    }
}
```

### 59.螺旋矩阵
#### 思路
一直循环走路，如果到达边界，则用stepj和strepk进行方向的变化。
先遍历外层的状态，之后在判断，内层的移动
#### 复杂度

#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> a(n, vector<int>(n, 0));
        int j=0;
        int k=0;
        int stepj = 0;
        int stepk = 1;
        a[0][0] = 1;
        //走
        for(int i=2;i<=n*n;i++){
            j+=stepj;
            k+=stepk;
            a[j][k] = i;
			//处理边缘
            if(k==n-1)
            {
                if(j==0)
                {
                    stepj = 1;
                    stepk = 0;
                }
                if(j==n-1){
                    stepj = 0;
                    stepk = -1;
                }
                continue;
            }

            if(k==0&&j==n-1){
                stepj = -1;
                stepk = 0;
                continue;
            }
            
			//处理内部
            if(stepk!=0 && a[j][k+stepk]!=0)
            {
                stepj = stepk;
                stepk = 0;
                continue;
            }

            if(stepj!=0 && a[j+stepj][k]!=0)
            {
                stepk = -stepj;
                stepj = 0;
                continue;
            }
        }
        return a;
    }
};
```
##### java
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] a = new int[n][n];
        a[0][0] = 1;
        int stepj = 0;
        int stepk = 1;
        int j = 0;
        int k = 0;
        for(int i = 2 ;i <= n*n;i++){
            j += stepj;
            k += stepk;
            a[j][k] = i;

            //处理外层
            if(k == n-1){
                if(j == 0){
                    stepk = 0;
                    stepj = 1;
                    continue;
                }
                if(j == n-1){
                    stepk = -1;
                    stepj = 0;
                    continue;
                }
            }

            if(k==0 && j==n-1){
                stepk = 0;
                stepj = -1;
                continue;
            }

            //处理内层
            if(stepj!=0 && a[j+stepj][k]!=0){
                stepk = -stepj;
                stepj = 0;
                continue;
            }

            if(stepk != 0 && a[j][k+stepk] != 0){
                stepj = stepk;
                stepk = 0;
                continue;
            }
        }
        return a;
    }
}
```



