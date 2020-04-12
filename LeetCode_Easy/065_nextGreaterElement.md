# 下一个更大的元素Ⅰ
***
#### 2020.04.12

### 问题
>给定两个没有重复元素的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。
nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出-1。

### 示例
>输入: nums1 = [4,1,2], nums2 = [1,3,4,2].                 
输出: [-1,3,-1]                   
解释:                          
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。                   
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。                 
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。                 

>输入: nums1 = [2,4], nums2 = [1,2,3,4].                          
输出: [3,-1]                                 
解释:             
    对于num1中的数字2，第二个数组中的下一个较大数字是3。
    对于num1中的数字4，第二个数组中没有下一个更大的数字，因此输出 -1。                          

### 注意
1.nums1和nums2中所有元素是唯一的。
2.nums1和nums2 的数组大小都不超过1000。

### 代码
```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int>res;
        for(int i=0;i<nums1.size();i++)
        {
            for(int j=0;j<nums2.size();j++)
            {
                if(nums1[i]==nums2[j])
                {
                    for(int k=j+1;k<nums2.size();k++)
                    {
                        if(nums2[k]>nums1[i]){
                        res.push_back(nums2[k]);
                        break;}
                        if(k==nums2.size()-1)
                        res.push_back(-1);
                    }
                    if(j==nums2.size()-1)
                    res.push_back(-1);
                    
                    break;
                }
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :32 ms, 在所有 C++ 提交中击败了8.87%的用户内存消耗 :8.4 MB, 在所有 C++ 提交中击败了100.00%的用户。时间不理想。谁让是暴力解法
   呢。三层循环嵌套，本以为会超时，但估计是样例不是很多数。
   
单调栈
```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        unordered_map<int, int> mp;
        stack<int> sk;
        for(int n: nums2){
            while(!sk.empty() && sk.top() < n){
                mp[sk.top()] = n;
                sk.pop();
            }
            sk.push(n);
        }
        while(!sk.empty()){
            mp[sk.top()] = -1;
            sk.pop();
        }
        for(int n: nums1){
            res.push_back(mp[n]);
        }
        return res;        
    }
};

作者：jarvis1890
```

map
```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        
         map<int,int> mp;
            stack<int> stack;
    
            for(int value: nums1) {
                mp[value] = -1;
            }
            
            for(int i=0;i<nums2.size();i++) {
                for(int j=i+1;j<nums2.size();j++)
                {
                    if(nums2[i]<nums2[j])
                    {
                        mp[nums2[i]]=nums2[j];
                        break;
                    }
                }
            }
            
            for(int i=0;i<nums1.size();i++) {
                nums1[i] = mp[nums1[i]];
            }
            return nums1;

    }
};

作者：bu-li-hai
```

哈希表
```c++
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> hashmap;
        vector<int> res;
        int index = 0;
        for(auto itm : nums2){
            hashmap[itm] = index++;
        }
        for(int i=0;i<nums1.size();i++){
            for(int j=hashmap[nums1[i]];j<nums2.size();j++){
                if(nums1[i] < nums2[j]){
                    res.push_back(nums2[j]);
                    break;
                }
                else{
                    if(j == nums2.size()-1){
                        res.push_back(-1);
                    }
                }
            }
        }
        return res;
    }

作者：shi-fen-fang-qi-zhong
```
