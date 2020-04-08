# 和为s的两个数字
***
#### 2020.04.08

### 问题
>输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

### 示例
>输入：nums = [2,7,11,15], target = 9               
输出：[2,7] 或者 [7,2]                  

>输入：nums = [10,26,30,31,47,60], target = 40             
输出：[10,30] 或者 [30,10]                         

### 限制
>1.1 <= nums.length <= 10^5                
2.1 <= nums[i] <= 10^6                              

### 代码
```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>res;
        int l=0,r=nums.size()-1;
        while(1)
        {
            if(nums[l]+nums[r]==target){
            res.push_back(nums[l]);
            res.push_back(nums[r]);
            return res;
            }
            if(nums[l]+nums[r]>target)
            r--;
            if(nums[l]+nums[r]<target)
            l++;
        }
    }
};
```

### 分析
 - 执行用时 :560 ms, 在所有 C++ 提交中击败了21.82%的用户内存消耗 :100.8 MB, 在所有 C++ 提交中击败了100%的用户。时间消耗有点大，真是一个
   while毁所有。
 - 这道题简单的地方在于已经排好了序。所以很容易想到双指针，两个指针向中间靠拢，若相加的数大于目标数则指针所指太大，右指针向左移动；若相加
   的数小于目标数，则指针所指太小，左指针向右移动，最终得到一对数。

```c++
class Solution {
public:

    //1、二分查找
    int binary_search(vector<int>& nums, int left, int right, int target)
    {
        while(left <= right)
        {
            int mid = left + (right - left)  / 2;
            if(target == nums.at(mid))
            {
                return mid;
            }
            else if(target < nums.at(mid))
            {
                right = mid - 1;
            }
            else
            {
                left = mid + 1;
            }
        }

        return -1;
    }

    vector<int> twoSum(vector<int>& nums, int target) {
        
        vector<int> v(2);

        for(int i = 0; i < nums.size(); ++i)
        {
            //缩小二分查找的范围
            int index = binary_search(nums, i + 1, nums.size() - 1, target - nums.at(i));
            if(index != -1 && index != i)
            {
                v.at(0) = nums.at(i);
                v.at(1) = nums.at(index);
                break;
            }
        }

        return v;
    }

    //2、hash set
    vector<int> twoSum(vector<int>& nums, int target) {
        
        vector<int> v(2);
        unordered_set<int> set;
        set.reserve(nums.size());
        // set<int> set;

        for(int i = 0; i < nums.size(); ++i)
        {
            auto iter = set.find(target - nums.at(i));
            if(iter != set.end())
            {
                v.at(0) = nums.at(i);
                v.at(1) = *iter;
                break;
            }
            else
            {
                set.insert(nums.at(i));
            }
        }

        return v;
    }
};

作者：eric-345
