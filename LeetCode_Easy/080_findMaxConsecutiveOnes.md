# 最大连续的1个数
***
#### 2020.05.05

### 问题
>给定一个二进制数组， 计算其中最大连续1的个数。                      

### 示例                
>输入: [1,1,0,1,1,1]                                 
输出: 3                  
解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.                                       

### 注意
>1.输入的数组只包含 0 和1。                             
2.输入数组的长度是正整数，且不超过 10,000。                          
 
 ### 代码
 ```c++
 class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max1=0,num=0;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]==1)
            num++;

            else
            {
                max1=max(num,max1);
                num=0;
            }
        }
        return max1>num?max1:num;
    }
};
```

### 分析
 - 执行用时 :84 ms, 在所有 C++ 提交中击败了40.96%的用户内存消耗 :33.4 MB, 在所有 C++ 提交中击败了8.33%的用户。比较暴力的解法，所以效果不是
   很好。
   
队列
```c++
class Solution {
private:
    queue<int> temp; //辅助队列

public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int maxlength = 0; //最大长度
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] == 1){
                temp.push(nums[i]);
                maxlength = max(maxlength, (int) temp.size());
            }
            if (nums[i] == 0) {
                maxlength = max(maxlength, (int) temp.size());
                while (!temp.empty()) temp.pop(); //清空当前队列
            }
        }
        return maxlength;
    }
};

作者：candygirl2019
```

桶排序
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int j=0;
        vector<int>m(10000,0);
        for(int i=0;i<nums.size();i++){
            if(nums[i]==1){
                m[j]++;
            }
            else
                j++;
    }
        sort(m.begin(),m.end());
        return m[m.size()-1];
    }
};

作者：li-jun-yi-2
