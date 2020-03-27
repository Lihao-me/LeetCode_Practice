# 寻找重复数
***
#### 2020.03.27

### 问题
>给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。
假设只有一个重复的整数，找出这个重复的数。

### 示例
>输入: [1,3,4,2,2]     
输出: 2          

>输入: [3,1,3,4,2]          
输出: 3         

### 说明
>1.不能更改原数组（假设数组是只读的）。
2.只能使用额外的 O(1) 的空间。
3.时间复杂度小于 O(n2) 。
4.数组中只有一个重复的数字，但它可能不止重复出现一次。        

### 思路
>将数组排序，当出现和前面一个数字相同的元素，即为所求。         

>将各元素出现的次数进行记录，若出现大于2的则为所求。

### 代码
```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int res;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i]==nums[i-1])
            res=nums[i];
        }
        return res;
    }
};
```
优化后
```c
int findDuplicate(int* nums, int numsSize){
    int*a=(int*)malloc(numsSize*sizeof(int));
    memset(a,0,sizeof(int)*numsSize);
    for(int i=0;i<numsSize;i++)
    {
        a[nums[i]]++;
    }
    int res;
    for(int i=0;i<numsSize;i++)
    {
        if(a[i]>1)
        res=i;
    }
    return res;
}
```

### 分析
 - 优化前，我的提交执行用时：28 ms已经战胜 10.56 % 的 cpp 提交记录，我的提交消耗内存：11 MB已经战胜 12.48 % 的 cpp 提交记录。优化后，
   执行用时 :8 ms, 在所有 C 提交中击败了96.00%的用户内存消耗 :7 MB, 在所有 C 提交中击败了100.00%的用户。
 - 第一遍写的代码在时空上都不高效，原因在于没读清题，说明中要求不能改变原数组。所以后来优化的时候申请了一个数组，但是虽然结果还算理想，但是
   我觉得也并不是十分简洁。
 - 题目本身的简单之处在于只有一个重复元素，且元素的大小不会超过数组长度。我认为这道题之所以被认定为中档题，全在那几条说明的限制上。
 
二分查找
```c++
class Solution {
public:
    int findDuplicate(vector<int> &nums) {
        int len = nums.size();
        int left = 1;
        int right = len - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            int cnt = 0;
            for (int num:nums) {
                if (num <= mid) {
                    cnt++;
                }
            }

            // 根据抽屉原理，小于等于 4 的数的个数如果严格大于 4 个，
            // 此时重复元素一定出现在 [1, 4] 区间里

            if (cnt > mid) {
                // 重复的元素一定出现在 [left, mid] 区间里
                right = mid;
            } else {
                // if 分析正确了以后，else 搜索的区间就是 if 的反面
                // [mid + 1, right]
                // 注意：此时需要调整中位数的取法为上取整
                left = mid + 1;
            }
        }
        return left;
    }
};

作者：liweiwei1419
```
链接：https://leetcode-cn.com/problems/find-the-duplicate-number/solution/er-fen-fa-si-lu-ji-dai-ma-python-by-liweiwei1419/

循环检测
```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int result = 0;
        int slow = nums[0];
        int quick = nums[0];
        do{
            slow = nums[slow];
            quick = nums[nums[quick]];
        }while(slow!=quick);
        
        int p = nums[0];
        while(p!=slow){
            slow = nums[slow];
            p = nums[p];
        }
        result = p;
        return result;
    }
};

作者：jie-ju-wei-ming-ming
```
链接：https://leetcode-cn.com/problems/find-the-duplicate-number/solution/xun-huan-jian-ce-fang-fa-xi-shuo-by-jie-ju-wei-min/

快慢指针
```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        
        int fast = 0, slow = 0;
        while(true){
            fast = nums[nums[fast]];
            slow = nums[slow];
            if(fast == slow)
                break;
        }
        int finder = 0;
        while(true){
            finder = nums[finder];
            slow = nums[slow];
            if(slow == finder)
                break;        
        }
        return slow;
    }
};

作者：zjczxz
```
链接：https://leetcode-cn.com/problems/find-the-duplicate-number/solution/kuai-man-zhi-zhen-de-jie-shi-cong-damien_undoxie-d/
