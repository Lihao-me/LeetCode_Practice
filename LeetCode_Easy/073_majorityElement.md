# 主要元素
***
#### 2020.04.24

### 问题
>如果数组中多一半的数都是同一个，则称之为主要元素。给定一个整数数组，找到它的主要元素。若没有，返回-1。                     

### 示例
>输入：[1,2,5,9,5,9,5,5,5]                        
输出：5                     

### 示例
>输入：[3,2]                   
输出：-1                      

### 示例
>输入：[2,2,1,1,1,2,2]                      
输出：2                             

### 说明
>你有办法在时间复杂度为 O(N)，空间复杂度为 O(1) 内完成吗？            

### 代码
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        if(nums.size()==1)
        return nums[0];
        sort(nums.begin(),nums.end());
        int a=nums[0],c=1;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]==a){
            c++;
            if(c*2>nums.size())
            return nums[i];
            }
            else
            {
                a=nums[i];
                c=1;
            }

        }
        return -1;
    }
};
```

### 分析
 - 执行用时 :84 ms, 在所有 C++ 提交中击败了9.95%的用户内存消耗 :18.9 MB, 在所有 C++ 提交中击败了100.00%的用户。看到这道题是那么的似曾相识，
   这不就是我前天做的那道题吗？
 - 权当是复习一下吧，更何况当初做那道题也不是很顺利。
 
摩尔投票
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cnt = 0, major;
        for(int n : nums){
            if(cnt == 0){
                major = n;
                cnt ++;
            }
            else{
                if(major == n) cnt ++;
                else cnt --;
            }
        }
        return major;
    }
};

作者：jarvis1890
```

位运算
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int ans = 0;
        //统计每位数字的第i位二进制
        for(int i = 0; i < 32; i++){
            int cnt = 0;
            for(int j = 0; j < nums.size(); j++){
                //如果第i位为1
                if(nums[j] & (1 << i)) cnt++;
            }
            //如果所有数字的二进制数中，第i位1比0多
            if(cnt > nums.size() / 2) ans ^= (1 << i);
        }
        return ans;
    }
};

作者：jarvis1890
```

分治
```c++
class Solution {
public:

    int majority_judge(vector<int>& nums, int num, int left, int right){
        int count = 0;
        for(int i=left;i<=right;i++){
            if(nums[i] == num)
                count ++;
        }
        return count; 
    }

    int helper(vector<int>& nums, int left, int right){
        if(left == right) return nums[left];
        else{
            int mid = (left + right) >> 1;
            int left_majority = helper(nums, left, mid);
            int right_majority = helper(nums, mid+1, right);
            int left_majority_count = majority_judge(nums, left_majority, left, right);
            int right_majority_count = majority_judge(nums, right_majority, left, right);
            return left_majority_count > right_majority_count ? left_majority : right_majority;
        }
    }

    int majorityElement(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;
        int result = helper(nums, left, right);
        return result;
    }
};

作者：jarvis1890
```

哈希表
```c++
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

随机化
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        while (true) {
            int candidate = nums[rand() % nums.size()];
            int count = 0;
            for (int num : nums)
                if (num == candidate)
                    ++count;
            if (count > nums.size() / 2)
                return candidate;
        }
        return -1;
    }
};

作者：LeetCode-Solution
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
```
