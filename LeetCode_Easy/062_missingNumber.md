# 0~n中缺失的数字
***
#### 2020.04.09

### 问题
>一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，
请找出这个数字。

### 示例
>输入: [0,1,3]                    
输出: 2                     

>输入: [0,1,2,3,4,5,6,7,9]                  
输出: 8                   

### 限制
>1 <= 数组长度 <= 10000

### 代码
```c
int missingNumber(int* nums, int numsSize){
    int sum=((numsSize)*(numsSize+1))/2;
    for(int i=0;i<numsSize;i++)
    {
        sum=sum-nums[i];
    }
    return sum;
}
```

### 分析
 - 执行用时 :32 ms, 在所有 C 提交中击败了31.56%的用户内存消耗 :6.3 MB, 在所有 C 提交中击败了100.00%的用户。今天刚好了解到一些数据结构的基础
   知识了解到这一题。这是一道很简单但也很老的题目。有很多种做法。比如排序、hashtable、取异或以及我所用的数学公式法。但貌似时间上还是差点火候。
   
二分法
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        // 如果这个数在中间[0,1,3]
        int res = recursion(nums,0,n-1);
        if(res != -1) return res; 
        // 排除[0,1,2]与[1,2,3]有序结果
        return nums[0] == 0 ? nums[n-1]+1 : 0;   
    }
    int recursion(vector<int>& nums,int left,int right) {
        if(left>right) return -1;
        
        int mid = left + (right-left) / 2;
        if(mid+1 < nums.size() && nums[mid] + 2 == nums[mid+1])
            return nums[mid]+1;
        int res1 = recursion(nums,left,mid-1);
        int res2 = recursion(nums,mid+1,right);
        if(res1 == -1) return res2;
        return res1;
    }
};

作者：2869526170
```

位运算
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size(),res = 0;
        for(int i = 0;i < n;i++) res ^= (nums[i] ^ i);
        return res ^ n;
    }
};

作者：qhu_
```

桶排序
```c++
int missingNumber(std::vector<int> &nums) {
    if (nums.empty()) {
        return -1;
    }

    // 定义次数数组（桶）
    std::vector<int> count(nums.size() + 1, 0);

    // 将数出现的次数存放在数对应的索引上
    for (int num : nums) {
        count[num]++;
    }

    // 遍历次数数组，如果次数为0表示该索引对应的数缺失
    for (int i = 0; i < count.size(); i++) {
        if (count[i] == 0) {
            return i;
        }
    }

    return -1;
}

作者：luo-jing-yu-yu
