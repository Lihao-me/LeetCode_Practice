# 只出现一次的数字
***
#### 2020.04.12

### 问题
>给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

### 说明
>你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

### 示例
>输入: [2,2,1]                       
输出: 1                         

>输入: [4,1,2,1,2]                     
输出: 4                

### 代码
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size();i=i+2)
        {
            if((i<nums.size()-1&&nums[i]!=nums[i+1])||(i==nums.size()-1))
            return nums[i];

        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :104 ms, 在所有 C++ 提交中击败了5.16%的用户内存消耗 :11.9 MB, 在所有 C++ 提交中击败了5.55%的用户。时间、空间都才这么点，难受。
   虽然看到说明中的要求了，但还是打不开思路。还是虚心学习吧。
   
哈希
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_set<int> bobo;
        int ans;
        for(auto i : nums){
            if(bobo.count(i))   bobo.erase(i);
            else    bobo.insert(i);
        }
        for(auto j : bobo)  ans = j;
        return ans;
    }
};

作者：youlookdeliciousc
```

双指针
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for(int i = 0, j = 1; j < nums.size(); i += 2, j += 2){
            if(nums[i] != nums[j])  return nums[i];
        }
        return nums[nums.size() - 1];
    }
};

作者：youlookdeliciousc
```

异或
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = nums[0];
        for(int i = 1; i < nums.size(); ++ i){
            ans = ans ^ nums[i];
        }
        return ans;
    }
};

作者：youlookdeliciousc
```

map
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        map<int,int> n;
        int ans = 0;
        for(int i = 0; i < nums.size(); ++ i){
            n[nums[i]] == 1? n[nums[i]] = 2: n[nums[i]] = 1;
        }
        for(int j = 0; j < nums.size(); ++ j){
            if(n[nums[j]] == 1)  ans = nums[j];
        }
        return ans;
    }
};

作者：youlookdeliciousc
```

快排
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int i,cur;
        int len=nums.size();

        sort(nums.begin(),nums.end());//进行快速排序，得到例如1，1，3，3，2

        for(i=0;i<len;i=i+2)//每次后移两个位置
        {
            cur=nums[i];
            if(i==len-1)  //如果遍历到最后一个元素,说明该元素为single number
                return cur;
            if(cur!=nums[i+1])//如果当前元素不等于后一个元素即为single number
                return cur;
           
        }
        return -1;
    }
};

作者：zrita
```
