# 存在重复元素
***
#### 2020.04.30

### 问题
>给定一个整数数组，判断是否存在重复元素。                          
如果任意一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。                               

### 示例                 
输入: [1,2,3,1]                                         
输出: true                          
                                          
输入: [1,2,3,4]                               
输出: false                   

输入: [1,1,1,3,3,4,3,2,4,2]                    
输出: true               

### 代码
```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        if(nums.size()==0)
        return 0;
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size()-1;i++)
        {
            if(nums[i]==nums[i+1])
            return 1;
        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :48 ms, 在所有 C++ 提交中击败了68.32%内存消耗 :15.3 MB, 在所有 C++ 提交中击败了91.43%的用户。先排个序，遍历一下就找出来了。很
   简单，没啥好说的。
   
哈希
```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {

        unordered_map <int,int>mp;
        for(int i:nums)
        {
            mp[i]++;  //i对应的value值++

            if(mp[i]>1) //i对应的value值大于1，则说明存在重复元素
                return true;
        }
        return false;
    }
};

作者：zrita
```

集合
```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {

        unordered_set <int> st (nums.begin(),nums.end());
        return nums.size() > st.size();//如果原数组的大小大于集合的大小，则说明存在重复元素
    }
};

作者：zrita
```

set
```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set s(nums.begin(), nums.end());
        return s.size() != nums.size();
    }
};

作者：QQqun902025048
