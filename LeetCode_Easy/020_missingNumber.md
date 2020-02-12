# 缺失数字
***
#### 2020.02.12

### 问题
>给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。

### 示例
>输入: [3,0,1]
输出: 2

>输入: [9,6,4,2,3,5,7,0,1]
输出: 8

>说明:
你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?

### 思路
>将原数组排序，逐一排查，直到出现不符合的数。

### 代码
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int i;
        for(i=0;i<nums.size();i++){
            if(nums[i]!=i)
            return i;
        }
       return i; 
    }
};
```

### 分析
 - 看到说明中的线性复杂度，首先排除用两个循环解决的办法，接着就可以想到将数组排序后用一个循环即可排查出不符合原数组的数。
 - 令人眼前一亮的是还有三种c语言的解法，思路也非常简单。而其中的“异或”算法也是我未曾听说的。代码中的“^”即是异或运算符。
   在c语言中^的意思是按位异或。主要用在二进制中。举个例子9^5=000010001^00000101=00001100.结果就是12。规则就是：先将
   两个整数化成二进制位数。在每个对应的位数中，只有两者的该位上一个是1或者0，而另一个的改为上必须是相反的，那么做该运算
   该位的结果就是1。否则结果就是0。
   1 & 1 = 1， 1 | 1 = 1， 1 ^ 1 = 0,
   1 & 0 = 0， 1 | 0 = 1， 1 ^ 0 = 1,
   0 & 1 = 0， 0 | 1 = 1， 0 ^ 1 = 1,
   0 & 0 = 0， 0 | 0 = 0， 0 ^ 0 = 0
   
求和
```c
int missingNumber(int* nums, int numsSize){
    int ans = (numsSize + 1) * numsSize / 2, i;
    for(i = 0; i < numsSize; i++){
        ans -= nums[i];
    }
    return ans;
}

作者：zed-65536
```
异或
```c
int missingNumber(int* nums, int numsSize){
    int ans = numsSize, i;
    for(i = 0; i < numsSize; i++){
        ans ^= i ^ nums[i];
    }
    return ans;
}

作者：zed-65536
```
哈希
```c
int missingNumber(int* nums, int numsSize){
    int *p = (int*)calloc(numsSize + 1, sizeof(int));
    int i;
    for(i = 0; i < numsSize; i++){
        p[nums[i]] = 1;
    }
    for(i = 0; i <= numsSize; i++){
        if(p[i] == 0)
            return i;
    }
    return -1; //not found
}

作者：zed-65536
