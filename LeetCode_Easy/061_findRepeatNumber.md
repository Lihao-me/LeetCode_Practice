# 数组中重复的数字
***
#### 2020.04.08

### 问题
>找出数组中重复的数字。             
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，
也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

### 示例
>输入：             
[2, 3, 1, 0, 2, 5, 3]             
输出：2 或 3               

### 限制
>2 <= n <= 100000

### 代码
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int n=nums.size();
        sort(nums.begin(),nums.end());
        for(int i=1;i<n;i++)
        {
            if(nums[i-1]==nums[i])
            return nums[i];
        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :136 ms, 在所有 C++ 提交中击败了12.69%的用户内存消耗 :23.3 MB, 在所有 C++ 提交中击败了100.00%的用户。这个有很多种思路，也很简单
   。没啥好说的。权当是写着玩。但是有一点是时间运行不是很理想。可以改进。
   
布尔数组判重
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        bool flag[nums.size()];
        memset(flag, false, sizeof(flag));
        for(int i = 0; i < nums.size(); i++)
            if(flag[nums[i]])
                return nums[i];
            else
                flag[nums[i]] = true;
        return -1;
    }
};

作者：lhh2001-2
```

集合
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        set<int> count;
        for(auto i:nums){
            if(count.count(i) == 1){
                return i;
            }else{
                count.insert(i);
            }
        }
        return 0;
    }
};

作者：z1m
```

map
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        map<int,int> q;
        for(auto iter = nums.begin();iter!=nums.end();iter++){
            if(q[*iter] != 1){
                q[*iter] = 1;
            }
            else{
                return *iter;
            }
        }
        return -1;
    }
};

作者：misby-3-m
