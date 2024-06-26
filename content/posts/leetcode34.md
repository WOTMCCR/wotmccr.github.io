---
categories: 算法题
title: 2024-06-24刷题
tags:
  - 动态规划
  - leetcode
date: 2024-06-24
---
## 2024-06-24刷题
### 46. 携带研究材料（第六期模拟笔试）
10:25
#### 思路
dp数组, `dp[i][j]`代表行李箱空间为j的情况下,从下标为`[0, i]`的物品里面任意取,能达到的最大价值
递推公式
```C++
// 装这个物品的最大值由容量为j - weight[i]的包任意放入序号为[0, i - 1]的最大值 + 该物品的价值构成
dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
```
#### 题解
##### C++
```C++
#include<vector>
#include<iostream>
#include<algorithm>
#include<math.h>
using namespace std;

int n,bagweight;
void slove(){
    vector<int> weight(n,0);
    vector<int> value(n,0);
    for(int i ; i < n ; i++){
        cin>>weight[i];
    }
    for(int i ; i < n ; i++){
        cin>>value[i];
    }
    
    vector<vector<int>> dp(weight.size() , vector<int>(bagweight+1 , 0));
    // 初始化, 因为需要用到dp[i - 1]的值
    // j < weight[0]已在上方被初始化为0
    // j >= weight[0]的值就初始化为value[0]
    for(int i = weight[0] ; i <=bagweight ; i++){
        dp[0][i] = value[0];
    }

    for(int i = 1 ; i < weight.size() ; i++){ //科研物品
        for(int j = 0 ; j <= bagweight ; j++){ // 行李箱容量
            //如果装不下该物品
            if(j<weight[i]) dp[i][j] = dp[i-1][j];
            //装得下该物品
            //如果能装下,就将值更新为 不装这个物品的最大值 和 装这个物品的最大值 中的 最大值
            // 装这个物品的最大值由容量为j - weight[i]的包任意放入序号为[0, i - 1]的最大值 + 该物品的价值构成
            else dp[i][j] = max(dp[i-1][j],dp[i-1][j - weight[i]] + value[i]);
        }
    }
    cout << dp[weight.size() - 1][bagweight]<<endl;
}

int main(){
    cin>>n>>bagweight;
    slove();
    return 0;
}

```

### 01背包理论基础（滚动数组）
20:57
#### 题目描述
背包大小为bagweight，物品数量为n
输入物品重量，物品价值
放物品的价值最大
#### 思路
改用滚动数组的方式存储dp
`dp[j]`为容量为j的背包所背的最大价值
```C++
dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
```
#### 题解
##### C++
```C++
#include<vector>
#include<iostream>
#include<algorithm>
#include<math.h>
using namespace std;

int n,bagweight;
void slove(){
    vector<int> weight(n,0);
    vector<int> value(n,0);
    for(int i ; i < n ; i++){
        cin>>weight[i];
    }
    for(int i ; i < n ; i++){
        cin>>value[i];
    }
    vector<int> dp(bagweight+1,0);
    for(int i = 0 ; i < n ; i++){
        for(int j = bagweight ; j >= weight[i] ; j--){
            dp[j]= max(dp[j] , dp[j - weight[i]] + value[i]);
        }
    }
    cout<<dp[bagweight]<<endl;
}

int main(){
    cin>>n>>bagweight;
    slove();
    return 0;
}

```


### 416. 分割等和子集
22:09
#### 题目描述
只包含**正整数**的非空数组
判断是否可以分割成两个元素和相等的子集

**示例 1：**
**输入：nums = `[1,5,11,5]`
输出：true
解释：数组可以分割成 `[1, 5, 5]` 和 `[11]` 。**
#### 思路
应该先排序
- 背包的体积为sum / 2
- 背包要放入的商品（集合里的元素）重量为 元素的数值，价值也为元素的数值
- 背包如果正好装满，说明找到了总和为 sum / 2 的子集。
- 背包中每一个元素是不可重复放入。
#### 复杂度
时间复杂度：O(n^2)
空间复杂度：O(n)，虽然dp数组大小为一个常数，但是大常数
#### 注意⚠️
从大到小遍历，每个元素只放一次
题目中物品是`nums[i]`，重量是`nums[i]`，价值也是`nums[i]`，背包体积是sum/2。
#### 题解
##### C++
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;

        // dp[i]中的i表示背包内总和
        // 题目中说：每个数组中的元素不会超过 100，数组的大小不会超过 200
        // 总和不会大于20000，背包最大只需要其中一半，所以10001大小就可以了
        vector<int> dp(10001, 0);
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
        }
        // 也可以使用库函数一步求和
        // int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 == 1) return false;
        int target = sum / 2;

        // 开始 01背包
        for(int i = 0; i < nums.size(); i++) {
            for(int j = target; j >= nums[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }
        // 集合中的元素正好可以凑成总和target
        if (dp[target] == target) return true;
        return false;
    }
};
```




