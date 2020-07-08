# 区域和检索-数组不可变
***
#### 2020.07.08

### 问题
>给定一个整数数组  nums，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。              

### 示例
>给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()                
sumRange(0, 2) -> 1           
sumRange(2, 5) -> -1                
sumRange(0, 5) -> -3            

### 说明
>1.你可以假设数组不可变。            
2.会多次调用 sumRange 方法。           

### 代码
```c++
class NumArray {
public:
    int *sum;
    NumArray(vector<int>& nums) {
        if(nums.empty()==1)
        return;
        sum=new int[nums.size()+1];
        sum[0]=nums[0];
        for(int i=0;i<nums.size()-1;i++)
        sum[i+1]=sum[i]+nums[i+1];
    }
    
    int sumRange(int i, int j) {
        return i==0?sum[j]:(sum[j]-sum[i-1]);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```
 
### 分析
  - 执行用时：40 ms, 在所有 C++ 提交中击败了98.98%的用户内存消耗：16.6 MB, 在所有 C++ 提交中击败了100.00%的用户。
  - 这道题开始看的时候很纳闷，为什么凭空还要多出一个构造函数NumArray，因为事实上如果直接将数组地址作为参数传给sumRange函数也是可以做出来的。但是我认为题目只是尽可能地限制“数组不可变
    这一原则”。然而，如果我们直接新建一个数组numArray对象，将题目数组“传给”它，在sumRange中也是可以实现直接将第i到j的元素求和的暴力操作的，但是时间消耗上就可能很高了。但是可能这种求
    前缀和的思路更符合出题者的本意。
  - 题解中除了前缀和的方法，也有更加复杂的思路，例如借助map等，也值得研究和学习。而所谓的动态规划，我认为其实质也是和我的思路是一样的，一种记忆功能的体现。而hash缓存的思路虽然耗时高，
    但也值得学习。
  
### 优解
#### 1.动态规划(32ms最优解)
```c++
class NumArray {
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        dp.reserve(n);
        if(nums.size() == 0)
            return;
        dp[0] = nums[0];
        for(int i = 1; i < n; i++)
            dp[i] = dp[i - 1] + nums[i];
    }
    
    int sumRange(int i, int j) {
        if(i == 0)
            return dp[j];
        return dp[j] - dp[i - 1];
    }
private:
    vector<int> dp;
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
 ```
 
 #### 2.Hash缓存(超时)
 ```c++
 class NumArray {

 public:
     map<tuple<int, int>, int> myMap;
 public:
     NumArray(vector<int>& nums) {
         for(int i=0;i<nums.size();i++){
             int sum=0;
             for(int j=i;j<nums.size();j++){
                 sum+=nums[j];
                 tuple<int, int> pair(i, j);
                 myMap[pair]=sum;
             }

         }
     }
    
     int sumRange(int i, int j) {
         tuple<int, int> pair(i, j);
         return myMap[pair];
     }
};

作者：alfred-mu
链接：https://leetcode-cn.com/problems/range-sum-query-immutable/solution/c-bao-li-ha-xi-qian-zhui-he-by-alfred-mu/
```
