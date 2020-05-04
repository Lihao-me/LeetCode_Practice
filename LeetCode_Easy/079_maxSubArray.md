# 最大子序和
***
#### 2020.05.04

### 问题
>给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

### 示例
>输入: [-2,1,-3,4,-1,2,1,-5,4],                                     
输出: 6                                  
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。                                    

### 进阶
>如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。                

### 代码
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        for(int i=1;i<nums.size();i++)
        {
            nums[i]=max(nums[i],nums[i-1]+nums[i]);
            nums[0]=max(nums[0],nums[i]);
        }
        return nums[0];
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :7.1 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题也是参考了网友的动态
   规划的思路。显然效果还不错。
   
dp
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int size = static_cast<int>(nums.size());
        int presum = 0, sum = 0, max = INT_MIN;                
        for(int i=0; i<size; ++i){
            int cur = nums[i];
            sum = presum + cur;
            if(sum < cur){
                sum = cur;
            } 
            if(sum > max){
                max = sum;
            }
            presum = sum;
        }
        return max;
    }
};

作者：happyfire
```

贪心
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int tmp, max = nums[0];
        for (int i=1; i<nums.size(); i++)
        {
            tmp = nums[i] + nums[i-1];
            if (tmp > nums[i])
            {
                nums[i] = tmp; 
                max = tmp>max?tmp:max;
            }
            else
            {
                max = nums[i]>max?nums[i]:max;
            }
        }
        return max;
    }
};

作者：xiao-ji-ye-bo
