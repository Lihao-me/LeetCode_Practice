# 连续数列
***
#### 2020.03.22

### 问题
>给定一个整数数组（有正数有负数），找出总和最大的连续数列，并返回总和。

### 示例
>输入： [-2,1,-3,4,-1,2,1,-5,4]        
输出： 6        
解释： 连续子数组 [4,-1,2,1] 的和最大，为 6。       

### 进阶
>如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

### 思路
>累加法，max记录最大的累加数据，temp记录一段的累加数据，当一段的累加数据小于0时，放弃这一段，继续往后累加。

### 代码
```c++
int maxSubArray(int* nums, int numsSize){
    int max=-INT_MAX;
    int temp=0;
    for(int i=0;i<numsSize;i++){
        temp+=nums[i];
        if(temp>max)
        max=temp;
        if(temp<0)
        temp=0;
    }
    return max;
}
```

### 分析
 - 执行用时 :8 ms, 在所有 C 提交中击败了86.54%的用户内存消耗 :6.1 MB, 在所有 C 提交中击败了100.00%的用户。这段代码并不是独立想出来的，本来
   以为特别复杂，但没想到即使是用c语言也有非常好的办法。利用数组中的正数和负数的存在原理，以及max的记忆特性，不得不说真的太巧妙了。
 - 而又有动态规划以及在原数组上解决的感觉并没有这个好理解。但还是一并贴上。
 
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxsum = INT_MIN;
        int sum = 0;
        for (auto i: nums)
        {
            sum = max(i + sum, i);
            maxsum = max(maxsum, sum);
        }
        return maxsum;
    }
};

作者：z1m
```

Dp原地
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        if(nums.size() == 1) return nums[0];
        int max = nums[0];
        for(decltype(nums.size()) i = 1; i < nums.size(); ++i)
        {   
            if(nums[i-1] > 0)
                nums[i] += nums[i-1];   
            if(nums[i] > max)
                max = nums[i];
        }
        return max;
    }
};

作者：yi-chui-ding-yin
