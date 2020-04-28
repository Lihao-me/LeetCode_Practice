# 找到所有数组中消失的数字
***
#### 2020.04.28

### 问题
>给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。                    
找到所有在 [1, n] 范围之间没有出现在数组中的数字。                      
您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。                    

### 示例
>输入:               
[4,3,2,7,8,2,3,1]               
输出:                    
[5,6]                         

### 代码
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int len=nums.size();

        vector<int>res;
        int *p=new int[len+1]();
        p[0]=-1;
        for(int i=0;i<len;i++)
        {
            p[nums[i]]++;
            //cout<<p[nums[i]]<<" "<<nums[i]<<" "<<i<<endl;
        }
        //cout<<"......."<<endl;
        for(int i=0;i<len+1;i++)
        {
            if(p[i]==0){
            res.push_back(i);
           // cout<<i<<endl;
           }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :120 ms, 在所有 C++ 提交中击败了35.35%的用户内存消耗 :33.4 MB, 在所有 C++ 提交中击败了7.14%的用户。由于也是类似哈希表的遍历
   做法，时间和空间上并不是很乐观。
   
原数组操作
将数组元素对应为索引的位置加n
遍历加n后的数组，若数组元素值小于等于n，则说明数组下标值不存在，即消失的数字
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        if(nums.empty()) return nums;
        for(int i=0;i<nums.size();i++)
        {
            int index=(nums[i]-1)%nums.size();
            nums[index]+=nums.size();
        }
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]<=nums.size())
                res.push_back(i+1);
        }
        return res;
    }
};
作者：haydenmiao
```

桶排序
桶排序，时间复杂度为 O(n)，可以把出现了的数字放在正确的位置。
为了避免转换下标的麻烦，在末尾插入一个0，排序后0会出现在首位。
[4,3,2,7,8,2,3,1,0]
会被排序为
[0,1,2,3,4,2,3,7,8]
然后把不等于值的索引返回。
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        nums.push_back(0);
        for (int i = 0; i < nums.size(); ++i)
            while (nums[i] != nums[nums[i]])
                swap(nums[i], nums[nums[i]]);
        vector<int> ans;
        for (int i = 1; i < nums.size(); ++i)
            if (i != nums[i]) ans.push_back(i);
        return ans;
    }
};

作者：hareyukai
```

set（用了额外的空间）
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> ans;

        set<int> s(nums.begin(), nums.end());
        for (int i = 1; i <= nums.size(); i++) {
            if (s.find(i) == s.end()) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};

作者：zhjlance
