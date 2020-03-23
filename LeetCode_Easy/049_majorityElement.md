# 多数元素
***
#### 2020.03.23

### 问题
>给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

### 示例
>输入: [3,2,3]       
输出: 3       

>输入: [2,2,1,1,1,2,2]        
输出: 2       

### 思路
>将数组元素由小到大排列，依次查找，记录各元素出现的次数，直到找到符合条件的元素。

### 代码
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n=0;
        int flag=1;
        if(nums.size()==1)
        return nums[0];
        sort(nums.begin(),nums.end());
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]!=nums[i-1])
            flag=1;
            if(nums[i]==nums[i-1])
                flag++;
            if(2*flag>nums.size())
            n=nums[i];
            
        }
        return n;
    }
};
```

### 分析
 - 执行用时 :32 ms, 在所有 C++ 提交中击败了34.37%的用户内存消耗 :20.6 MB, 在所有 C++ 提交中击败了5.23%的用户。这道题比较简单，但是时空方面
   做的并不是很好。一般与元素个数有关的数，我更倾向于对数组进行排序。也是提交了好几次看到几个特殊样例才把代码一步步完善。
 - 看了官方题解发现我的代码其实可以进一步简化。
 
排序
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};

作者：LeetCode-Solution
```

哈希表
算法
我们使用哈希映射（HashMap）来存储每个元素以及出现的次数。对于哈希映射中的每个键值对，键表示一个元素，值表示该元素出现的次数。
我们用一个循环遍历数组 nums 并将数组中的每个元素加入哈希映射中。在这之后，我们遍历哈希映射中的所有键值对，返回值最大的键。
我们同样也可以在遍历数组 nums 时候使用打擂台的方法，维护最大的值，这样省去了最后对哈希映射的遍历。
```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> counts;
        int majority = 0, cnt = 0;
        for (int num: nums) {
            ++counts[num];
            if (counts[num] > cnt) {
                majority = num;
                cnt = counts[num];
            }
        }
        return majority;
    }
};

作者：LeetCode-Solution
```

Moore投票
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {    //摩尔投票法，投我++，不投--，超过一半以上的人投我，那我稳赢哇
        int count=0,candidate;
        for(int n:nums)
        {
            if(count==0)        candidate=n;

            if(n==candidate)    count++;
            else                count--;
        }
        return candidate;
    }
};
```

随机数
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {    //每一轮随机选择一个数字，统计出现次数，因为目标出现频率大于二分之一，所以效率较高
        while(true)
        {
            int candidate=nums[rand() % nums.size()],count=0;   
            for(int n:nums)
            {
            if(n==candidate)     
                count++;
            }
            if(count > nums.size()/2)           return candidate;
        }
        return -1;
    }
};
```

位运算
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
    int res=0;
    for(int i=0;i<32;i++)
    {
        int ones=0;
        for(int n:nums)
            ones += (n >> i) & 1;                        //位运算法统计每个位置上1出现的次数，每次出现则ones+1
        res += (ones > nums.size()/2) << i;              //如果1出现次数大于2分之1数组长，1即为这个位置的目标数字
    }
    return res;
}
};
作者：zrita
