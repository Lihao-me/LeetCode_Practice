# 搜索插入位置
***
#### 2020.05.15

### 问题
>给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
你可以假设数组中无重复元素。

### 示例
>输入: [1,3,5,6], 5               
输出: 2                         

>输入: [1,3,5,6], 2                          
输出: 1                                       
                        
>输入: [1,3,5,6], 7                   
输出: 4                                   

>输入: [1,3,5,6], 0                   
输出: 0              

### 代码
```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if(target<=nums[0])
        return 0;
        for(int i=1;i<nums.size();i++)
        {
            if(target==nums[i-1])
            return i-1;
            if(target>nums[i-1]&&target<nums[i])
            return i;
        }
        if(target==nums[nums.size()-1])
        return nums.size()-1;
        
        return nums.size();
    }
};
```

### 分析
 - 执行用时 :8 ms, 在所有 C++ 提交中击败了52.67%的用户内存消耗 :6.9 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题如果使用遍历的方法进行
   操作，在开头和结尾的特殊情况等细节需要充分考虑。
   
### 优解
#### 二分查找
```c++
#include <iostream>
#include <vector>

using namespace std;

class Solution {
public:
    int searchInsert(vector<int> &nums, int target) {
        int size = nums.size();
        if (size == 0) {
            return 0;
        }

        int left = 0;
        // 因为有可能数组的最后一个元素的位置的下一个是我们要找的，故右边界是 len
        int right = size;

        while (left < right) {
            int mid = left + (right - left) / 2;
            // 小于 target 的元素一定不是解
            if (nums[mid] < target) {
                // 下一轮搜索的区间是 [mid + 1, right]
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
};

作者：liweiwei1419
```

#### lower_bound函数
```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        return lower_bound(nums.begin(),nums.end(),target)-nums.begin();
    }
};

作者：yangwuqi
```

#### 二分法
```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int len = nums.size();
        int l = 0, r = len - 1;
        if(nums[r] < target){
            return len;
        }
        if(nums[l] >= target){
            return 0;
        }
        while(l <= r){
            int mid = (l+r)/2;
            //============ 核心部分 ============
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                if(mid > 0 && nums[mid-1] < target){
                    return mid;
                }
                r = mid - 1;
            }else{
                // nums[mid] < target
                if(mid+1 < len && nums[mid+1] > target){
                    return mid+1;
                }
                l = mid+1;
            }
            //============ 核心部分 ============
        }
        if(nums[l] < target){
            return l+1;
        }else{
            return l;
        }
    }
};

作者：chengm15
