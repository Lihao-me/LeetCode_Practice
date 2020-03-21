# 数组中重复的数据
***
#### 2020.03.21

### 问题
>给定一个整数数组 a，其中1 ≤ a[i] ≤ n （n为数组长度）, 其中有些元素出现两次而其他元素出现一次。     
找到所有出现两次的元素。      
你可以不用到任何额外空间并在O(n)时间复杂度内解决这个问题吗？      

### 示例
>输入:           
[4,3,2,7,8,2,3,1]     

输出:      
[2,3]           

### 思路
>将数组由小到大排列，依次查找数组中每个元素，出现和前一个相同的元素记录。

### 代码
```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        int n=nums.size();
        vector<int>res;
        sort(nums.begin(),nums.end());
        
        for(int i=1;i<n;i++){
            if(nums[i]==nums[i-1]){
                res.push_back(nums[i]);
        
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :100 ms, 在所有 C++ 提交中击败了45.77%的用户，内存消耗 :33.6 MB, 在所有 C++ 提交中击败了5.38%的用户。这道题的简单之处在于相同的
   数最多只有两个。但是不申请多余的内存真的很费脑筋，不知道怎么做。

```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        if(nums.empty()) return res;
        int n=nums.size();
        for(int i=0;i<n;i++)
        {
            int index=(nums[i]-1)%n;//对n取余，防止数字越界
            nums[index]+=n;         //对应位置加n
        }
        for(int i=0;i<n;i++)
        {
            //出现两次，则对应位置大于2n
            if(nums[i]>2*n)
                res.push_back(i+1);
        }
        return res;
    }
};
作者：haydenmiao
```

```c++
class Solution {
public:
    //方法一：swap
    vector<int> findDuplicates(vector<int>& nums) {
        for(int i=0;i<nums.size();i++){
            while(nums[i] != nums[nums[i]-1] && i+1 != nums[i]){
                swap(nums[i], nums[nums[i]-1]);
            }
        }
        vector<int> res;
        for(int i=0;i<nums.size();i++){
            if(nums[i] != i+1) res.push_back(nums[i]);
        }
        return res;
    }

    //方法二：自身取反
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for(int i=0;i<nums.size();i++){
            int temp = abs(nums[i]);
            if(nums[temp-1] < 0) res.push_back(temp);
            nums[temp-1] = -nums[temp-1];
        }
        return res;
    }
```
