# 有多少小于当前数字的数字
***
#### 2020.03.26

### 问题
>给你一个数组 nums，对于其中每个元素 nums[i]，请你统计数组中比它小的所有数字的数目。    
换而言之，对于每个 nums[i] 你必须计算出有效的 j 的数量，其中 j 满足 j != i 且 nums[j] < nums[i] 。      
以数组形式返回答案。       
   
### 示例
>输入：nums = [8,1,2,2,3]        
输出：[4,0,1,1,3]             
解释：          
对于 nums[0]=8 存在四个比它小的数字：（1，2，2 和 3）。             
对于 nums[1]=1 不存在比它小的数字。               
对于 nums[2]=2 存在一个比它小的数字：（1）。           
对于 nums[3]=2 存在一个比它小的数字：（1）。              
对于 nums[4]=3 存在三个比它小的数字：（1，2 和 2）。            

>输入：nums = [6,5,4,8]           
输出：[2,1,0,3]             

>输入：nums = [7,7,7,7]               
输出：[0,0,0,0]             

### 提示
1.2 <= nums.length <= 500         
2.0 <= nums[i] <= 100                

### 思路
>一次遍历数组的时候，定位每一个元素，二次遍历数组时若该元素比定位元素小，则计数加一。

### 代码
```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int>res(nums.size());
        for(int i=0;i<nums.size();i++){
            for(int j=0;j<nums.size();j++){
                if(nums[j]<nums[i])
                res[i]++;
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :64 ms, 在所有 C++ 提交中击败了7.03%的用户内存消耗 :7.1 MB, 在所有 C++ 提交中击败了100.00%的用户。两个循环无疑消耗大量时间。但
   是我实在想不出更好的办法了。只能用暴力的方法。

桶思想：
定义一排桶 int arr[101] , 其中 arr[i] 里存放数字 n 出现的次数
遍历 nums，初始化桶数组 arr
累加处理桶数组arr ， 使得 arr[i] 表示比 i 小的数字的个数
最后遍历 nums ，取出对应桶里的结果即可。
```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        int arr[101];
        memset(arr, 0, sizeof(arr));
        // 初始化计数桶
        for (auto i : nums) {
            arr[i] ++;
        }
        // 累加处理计数桶，使得 arr[i] 表示比 i 小的数字的个数
        int cnt = 0;
        for (int i = 0; i <= 100; i ++) {
            int temp = arr[i];
            arr[i] = cnt;
            cnt += temp;
        }
        vector<int> ret;
        // 遍历 nums，取出对应桶 arr[i] 里的结果即可
        for (int i : nums) {
            ret.push_back(arr[i]);
        }
        return ret;
    }
};

作者：huwt
```

排序
```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {

        vector<int> nums_copy(nums.begin(), nums.end());
        sort(nums.begin(), nums.end());

        int n = 0;
        vector<int> v(128); // v的下标为nums中的数
        v.at(nums.at(n)) = n;

        //统计每个数 小于它的数
        for(int i = 0; i < nums.size() - 1; ++i)
        {
            if(nums.at(i + 1) != nums.at(i))
            {
                n = i + 1;
            }
            v.at(nums.at(n)) = n;
        }

        vector<int> res;
        res.reserve(nums.size());
        for(int i = 0; i < nums_copy.size(); ++i)
        {
            res.push_back(v.at(nums_copy.at(i)));
        }

        return res;
    }
};

作者：eric-345
```

技术累加
```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> cnt(102, 0);
        for (auto num : nums)
            ++cnt[num + 1];

        for (int i = 1; i < cnt.size(); ++i)
            cnt[i] += cnt[i - 1];

        for (int i = 0; i < nums.size(); ++i)
            nums[i] = cnt[nums[i]];
        return nums;
    }
};

作者：Gary_coding
```

类似动态规划
```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        //利用题目给出的条件
        vector<int> label(101,0);
        for(int i = 0; i < nums.size(); i++){
            ++ label[nums[i]];
        }
        vector<int> label_2(101,0);
        for(int i = 1; i < 101; i++){
            label_2[i] = label[i-1] + label_2[i-1];
        }

        for(int i = 0; i < nums.size(); i++){
            nums[i] = label_2[nums[i]];
        }
        return nums;
     }
};
作者：shi-shu-de-chi-huo-a2ncz4Q5Lg
