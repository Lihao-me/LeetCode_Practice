# 子集
***
#### 2020.05.29

### 问题
>给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。      
说明：解集不能包含重复的子集。                                  

### 示例
>输入: nums = [1,2,3]                   
输出:                       
[                       
  [3],                            
  [1],                                        
  [2],                      
  [1,2,3],                    
  [1,3],                 
  [2,3],                           
  [1,2],                   
  []                  
]                  

### 代码
```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>>res={{}};
        if(nums.size()==0)
        return res;
        for(int i=0;i<nums.size();i++)
        {
            int n=res.size();
            for(int j=0;j<n;j++)
            {
                vector<int>a=res[j];
                res[j].push_back(nums[i]);
                res.push_back(a);
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了66.78%的用户内存消耗 :6.9 MB, 在所有 C++ 提交中击败了100.00%的用户。遍历原数组，在前一个数组的
   基础上存储并添加新的元素构成新的子集。
   
### 优解
#### 1.位运算
```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int m = nums.size();
        int n = 1 << m;
        vector<vector<int>> ans;
        for (int i = 0; i != n; ++i) {
            vector<int> v;
            for (int j = 0; j != m; ++j)
                if (i & (1 << j)) v.push_back(nums[j]);
            ans.push_back(v);
        }
        return ans;
    }
};

作者：Gary_coding
```

#### 2.深搜
```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> tmp;
    void find(int dep, vector<int>& nums)
    {
        // 已经处理完最后一位，将目前存储的集合存入 ans ，并回溯
        if(dep <= 0)
        {
            ans.push_back(tmp);
            return;
        }
        // 情况一：集合中有该元素
        tmp.push_back(nums[dep - 1]);
        find(dep - 1, nums);
        tmp.pop_back();
        // 情况二：集合中无该元素
        find(dep - 1, nums);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        find(nums.size(), nums);
        return ans;
    }
};

作者：iswJXkYVv3
```

#### 3.动态规划
```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        //特判
        if(nums.size()==0)return {{}};
        vector<vector<int>> ans;
        ans.push_back({});
        //当前新加入的值nums[i]
        for(int i = 0;i<nums.size();i++){
            //前面i-1子集中元素个数
            int last_size = ans.size();
            //将nums[i]分别放进ans[j]的各个vector中，再放进ans中
            for(int j = 0;j<last_size;j++){
                //nums[i]放进空集
                if(ans[j].empty()==1)ans.push_back({nums[i]});
                else{
                    //可以写成一句，拆开方便阅读
                    vector<int> temp;
                    temp = ans[j];
                    temp.push_back(nums[i]);
                    ans.push_back(temp);
                }
                
            }
        }
        return ans;
    }
};

作者：aaron-ng
