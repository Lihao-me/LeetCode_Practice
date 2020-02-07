# 三个数的最大乘积
***
#### 2020.1.30

### 问题
>给定一个整型数组，在数组中找出由三个数组成的最大乘积，并输出这个乘积。

### 示例
>输入: [1,2,3]
输出: 6

>输入: [1,2,3,4]
输出: 24

>注意：
1.给定的整型数组长度范围是[3,104]，数组中所有的元素范围是[-1000, 1000]。
2.输入的数组中任意三个数的乘积不会超出32位有符号整数的范围。

### 思路
>把数组按升序排列，如果全是正数或者最小的两个负数乘积小于倒数第二、三数的乘积，那么最大乘积就是最后三个数的乘积；否则是第一、二个数与最后一个数的乘积。

### 代码
```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int size=nums.size();
        sort(nums.begin(),nums.end());
        return max(nums[0]*nums[1]*nums[size-1],nums[size-1]*nums[size-2]*nums[size-3]);
    }
};
```

### 分析
 - 对于这道题很显然要对数组进行排序，而我也为了代码的简洁性选择C++进行编写。我们都知道C语言是一门开发性的语言，而相对而言C++的工具就比较多了。今天这道
   题也促使我更全面地了解algorithm库下的一些工具函数，而sort函数就是一个非常常用的排序函数。
 - 这篇帖子就介绍了几个algorithm头文件下的常用函数：https://blog.csdn.net/hy971216/article/details/80056933 。而我今天用到的sort函数也可以有效
   拓展：https://blog.csdn.net/qq_41943867/article/details/81569340 。
 - 虽然函数的使用大大提升了代码的简洁性，但最后的结果并不理想，在所有cpp用户中时间上仅击败55.97%的人，内存上仅击败5.13%的人，可以说我认为的代码简洁
   并不是真正的简洁。顺便贴上另一种“查找”思路的方案吧。
   
```c++
思路一：排序
代码
时间复杂度取决于排序：O(nlogn)
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int size = nums.size();
        int a = INT_MIN, b = INT_MIN;
        if (nums[0] < 0 && nums[1] < 0) {
            a = nums[0] * nums[1] * nums[size-1];
        }
        b = nums[size-1] * nums[size-2] * nums[size-3];        
        return max(a, b);
    }
};

思路二：查找
寻找三个最大数和两个最小数，比较三个最大数乘积和两个最小数和一个最大数乘积。
代码
时间复杂度：O(n)
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        //假设max1 > max2 > max3
        int max1 = INT_MIN;
        int max2 = INT_MIN;
        int max3 = INT_MIN;
        //假设min1 < min2
        int min1 = INT_MAX;
        int min2 = INT_MAX;        
        for (int a : nums) {
            if (a > max1) {
                max3 = max2;
                max2 = max1;
                max1 = a;
            } else if (a > max2) {
                max3 = max2;
                max2 = a;
            } else if (a > max3) {
                max3 = a;
            }
            
            if (a < min1) {
                min2 = min1;
                min1 = a;
            } else if (a < min2) {
                min2 = a;    
            }            
        }
        return max(min1 * min2 * max1, max1 * max2 * max3);
    }
};

作者：guohaoding
