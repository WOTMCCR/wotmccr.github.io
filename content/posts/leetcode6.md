---
tags:
  - leetcode
  - 哈希表
  - 双指针
title: 2024-05-15刷题
date: 2024-05-15
description: 454.四数相加II  383. 赎金信  15. 三数之和  18. 四数之和
---
## 2024-05-15刷题
### 454. 四数相加 II
09:02 09:51
#### 思路
将前两数的和放到map中，并且统计数量
将后两个数的和的相反数判断是否存在hashmap里，如果存在，则将，所有等于的值的数量添加到返回值中
#### 复杂度
时间复杂度：$O(n^2)$
空间复杂度：$O(n^2)$
#### 注意⚠️
使用auto进行遍历map，不要太像C的写法
#### 题解
##### C++
```C++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        int n = nums1.size();
        unordered_map<int,int> val;
        int num = 0;
        //遍历num1 ， num2
        for(auto i : nums1){
            for(auto j : nums2){
                val[i+j]++;
            }
        }    
        //遍历num3，num4
        for(auto i : nums3){
            for(auto j : nums4){
                num += val[-i-j];
            }
        }
        return num;
    }
};
```
##### java
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer,Integer> val = new HashMap<Integer,Integer>();
        for(int u : nums1){
            for(int v : nums2){
                val.put(u+v,val.getOrDefault(u+v,0)+1);
            }
        }

        int num = 0;
        for(int u:nums3){
            for(int v:nums4){
                if(val.containsKey(-u-v))
                    num += val.get(-u-v);
            }
        }
        return num;
    }
}
```
##### go
```go
func fourSumCount(nums1 []int, nums2 []int, nums3 []int, nums4 []int) int {
        sum1 := map[int]int{}
        for _,u:= range nums1 {
            for _,v := range nums2 {
                sum1[u+v]++
            }
        }
        result := 0
        for _,u := range nums3 {
            for _,v := range nums4 {
                result += sum1[-u-v];
            }
        }
        return result
}
```
##### swift
```swift
class Solution {
func fourSumCount(_ nums1: [Int], _ nums2: [Int], _ nums3: [Int], _ nums4: [Int]) -> Int {
    var map = [Int: Int]()
    
    // Populate the map with sums of elements from nums1 and nums2
    nums1.forEach { num1 in
        nums2.forEach { num2 in
            let sum = num1 + num2
            map[sum, default: 0] += 1
        }
    }
    
    var count = 0
    
    // Check for sums in nums3 and nums4 that negate the sums in the map
    nums3.forEach { num3 in
        nums4.forEach { num4 in
            let target = -(num3 + num4)
            if let occurrences = map[target] {
                count += occurrences
            }
        }
    }
    
    return count
}

}
```
### 383.赎金信
12:43
#### 思路
将magazine中的字符添加到map中，然后使用ransomNote中的字符进行匹配判断是否对应
#### 复杂度
时间复杂度：O(m+n)
#### 题解
##### C++
```C++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int> str1;
        for(int i = 0; i<magazine.length();i++){
            str1[magazine[i]]++;
        }
        for(int i = 0 ; i < ransomNote.length() ; i++){
            if(--str1[ransomNote[i]] < 0){
                return false;
            }
        }
        return true;
    }
};
```
##### java
```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if(ransomNote.length() > magazine.length()){
            return false;
        }
        int [] cnt = new int[26];
        for(char c : magazine.toCharArray()){
            cnt[c - 'a']++;
        }
        for(char c: ransomNote.toCharArray()){
            cnt[c-'a']--;
            if(cnt[c - 'a'] < 0){
                return false;
            }
        }
        return true;
    }
}
```
##### go
```go
func canConstruct(ransomNote string, magazine string) bool {
    if len(ransomNote) > len(magazine) {
        return false
    }
    cnt := [26]int{}
    for _,ch := range magazine {
        cnt[ch - 'a']++
    }
    for _,ch := range ransomNote {
        cnt[ch - 'a']--
        if cnt[ch - 'a'] < 0 {
            return false
        }
    }
    return true
}
```
##### swift
```swift
class Solution {
    func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {
        var record = Array(repeating:0,count:26)
        let aUnicodeScalarValue = "a".unicodeScalars.first!.value
        for unicodeScalar in magazine.unicodeScalars {
            let idx : Int = Int(unicodeScalar.value - aUnicodeScalarValue)
            record[idx] += 1
        }

        for unicodeScalar in ransomNote.unicodeScalars {
            let idx : Int = Int(unicodeScalar.value - aUnicodeScalarValue)
            record[idx] -= 1
            if record[idx] < 0{
                return false
            }
        }
        return true

    }
}
```

### 15.三数之和
15:17
#### 思路
先排序
然后固定一个左指针从左往右进行遍历，然后新建两个指针，对右边的区域从两端开始进行遍历
如果，右区域的遍历指针遇到重复的元素，即进行去重处理。
如果左侧的指针当前元素等于前一个元素也即进行去重。
#### 复杂度
- 时间复杂度: $O(n^2)$
- 空间复杂度: O(1)
#### 注意⚠️
去重逻辑应在找到一个三元组之后,因为排过序了
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        //排序
        sort(nums.begin(),nums.end());

        //从左开始遍历
        for(int i = 0;i<nums.size();i++){
            //如果起始元素>0可以剪枝
            if(nums[i]>0){
                return result;
            }

            //起始去重
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }

            //右区间开始相向遍历
            int left = i + 1;
            int right = nums.size()-1;
            while(left < right){
                //和大于0，则right--
                if(nums[i]+nums[left]+nums[right]>0){
                    right--;
                }else if(nums[i]+nums[left]+nums[right] < 0){ //和小于0，则left++
                    left++;
                }else{
                    result.push_back(vector<int>{nums[i],nums[left],nums[right]});
                    //去重逻辑应在找到一个三元组之后,因为排过序了
                    //去重
                    while(left < right && nums[left] == nums[left+1]) left++;
                    while(left < right && nums[right]== nums[right-1]) right--;
                    //更新
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
};
```
##### java
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for(int i = 0 ; i<nums.length;i++){
            //如果大于0则不可能等于0
            if(nums[i] > 0){
                return result;
            }

            //去重
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;            
            while(left < right){
                if(nums[i]+nums[left]+nums[right] > 0){
                    //大于0则，左移right
                    right--;
                }else if(nums[i]+nums[left]+nums[right] < 0){
                    //小于0则，右移left
                    left++;
                }else{
                    //相等，则添加List，并进行去重
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while(left < right && nums[right] == nums[right-1]) right--;
                    while(left < right && nums[left] == nums[left+1]) left++;

                    //更新left和right
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
}
```
##### go
```go
func threeSum(nums []int) [][]int {
    sort.Ints(nums)
    res := [][]int{}

    for i:=0; i<len(nums)-2;i++ {
        n1 := nums[i]
        if n1 > 0 {
            break
        }
        if i > 0 && n1 == nums[i-1] {
            continue
        }
        l,r := i+1,len(nums)-1
        for l < r {
            n2,n3 := nums[l],nums[r]
            if n1+n2+n3 == 0 {
                res = append(res,[]int{n1,n2,n3})
                for l<r && nums[l] == n2 {
                    l++
                }
                for l<r && nums[r] == n3 {
                    r--
                }          
            }else if n1+n2+n3 < 0 {
                l++
            }else {
                r--
            }
        }        
    }
    return res
}
```

### 18.四数之和
19:46
#### 思路
先进行排序然后，
左侧两个指针进行区间的确定，右侧，left和right进行相向遍历，进行判断
去重操作与上题相同
#### 复杂度
- 时间复杂度: O(n^3)
- 空间复杂度: O(1)
#### 注意⚠️
减枝操作，与上一道题有比较大的不同
需要保证sum结果不溢出使用long
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        if(nums.size() < 4){
            return result;
        }
        for(int i = 0;i < nums.size()-2;i++){
            if (nums[i] > target && nums[i] >= 0) {
            	break; // 这里使用break，统一通过最后的return返回
            }
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }
            for(int j = i + 1 ; j < nums.size()-1 ; j++){
                if(nums[j] + nums[i] > target && nums[j] + nums[i] >= 0){
                    break;
                }
                if(j > i + 1 && nums[j] == nums[j-1]){
                    continue;
                }
                //进行右侧的相向遍历
                int left = j + 1;
                int right = nums.size()-1;
                while(left < right){
                    if((long) nums[i]+nums[j]+nums[left]+nums[right] > target){
                        right--;
                    }else if((long) nums[i]+nums[j]+nums[left]+nums[right] < target){
                        left++;
                    }else{
                        result.push_back(vector<int>{nums[i],nums[j],nums[left],nums[right]});
                        while(left<right && nums[left] == nums[left+1]) left++;
                        while(left<right && nums[right] == nums[right-1]) right--;
                        //更新
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```















