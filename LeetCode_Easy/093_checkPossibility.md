# 非递减数列
***
#### 2020.05.28

### 问题
>给你一个长度为 n 的整数数组，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。           
我们是这样定义一个非递减数列的： 对于数组中所有的 i (1 <= i < n)，总满足 array[i] <= array[i + 1]。                               

### 示例
>输入: nums = [4,2,3]                        
输出: true                    
解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。                                    

>输入: nums = [4,2,1]                        
输出: false                                          
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。                                    

### 说明
>1.1 <= n <= 10 ^ 4                    
2.- 10 ^ 5 <= nums[i] <= 10 ^ 5          

### 代码
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int flag=0;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i-1]>nums[i]){
                if(i-2>=0&&nums[i-2]>nums[i])
                nums[i]=nums[i-1];
                else
                nums[i-1]=nums[i];

                flag++;
            }
        }
        return flag <=1;
        
    }
};
```

### 分析
 - 执行用时 :36 ms, 在所有 C++ 提交中击败了32.87%的用户内存消耗 :8.3 MB, 在所有 C++ 提交中击败了100.00%的用户。代码是一遍一遍改过之后的，
   通过不断地数据测试修改判断条件，并参考了网友的解法，有了这个答案。比较暴力。
   
### 优解
#### 1.STL
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        ios::sync_with_stdio(false);
        vector<int>::iterator it = is_sorted_until(nums.begin(), nums.end());
        if (it == nums.end())
            return true;
        if (it + 1 == nums.end())
            *it = INT_MAX;
        else if (*(it + 1) > *(it - 1))
            *it = *(it + 1);
        else
            *(it - 1) = *it;
        return is_sorted(nums.begin(), nums.end());
    }
};

作者：vticn
```

#### 2.双指针
```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int n=nums.size(),i=0,j=n-1;
        while(i<j && nums[i]<=nums[i+1]) i++;
        while(i<j && nums[j]>=nums[j-1]) j--;
        int head=i==0?INT_MIN:nums[i-1];
        int tail=j==n-1?INT_MAX:nums[j+1];
        if(j-i<=1 && (head<=nums[j] || nums[i]<=tail)) return true;
        else return false;
    }
};

作者：Tangzixia
