# 两数之和
***
#### 2020.1.23

### 问题
>给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

### 示例
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target, int* returnSize){
    
    int i,j;
    int *a=(int*)malloc(sizeof(int)*2);
    for(i=0;i<numsSize-1;i++)
    {
        for(j=i+1;j<numsSize;j++)
        {
            if(nums[i]+nums[j]==target)
            {
                a[0]=i;
                a[1]=j;
                *returnSize=2;
                return a;
            }
        }
    }
*returnSize=0;
return a;
}
```

### 分析
 - 第一次使用LeetCode提交题目还是有些不习惯，要根据所给的函数的形式参数含义充分理解我们所需利用的变量，同时明白要返回什么。
 - 起初多次提交不成功，在于没有充分理解题头要求，应该用malloced申请一个数组。
   ps:int * a=(int * )malloc(n* sizeof(int)); 表示定义一个int类型的指针变量a，并申请n* sizeof(int)个字节（即4* n个字节）的存储空间。
   malloc是在C语言中是一个申请内存单元的函数。
   函数原型：void *malloc(unsigned size);
   功       能：分配size个字节的内存空间
   返 回  值：成功，返回分配的内存单元的起始地址；否则返回0
 - 对于该题虽然表面上没有用到*returnSize这一变量，但是题中的隐含要求把它赋值否则结果错误。
 - 通过欣赏一些C语言网友的解法获益匪浅，其实自己的暴力解法并非最优的，用时上仅仅击败了17.19%的用户。网友所说的hash表解法还有待学习和掌握。
   这篇帖子用比喻的方式生动地介绍了哈希表的概念，也算对哈希表有了初步的认识吧。https://blog.csdn.net/qq_34888036/article/details/80880487
   同时把运用哈希表法解此题的几种方法贴上，相信对数据结构有较好掌握了回过头来看这道题应该有更大收获吧。
   
   #### 1、使用hash表，两次遍历数组
   ```c
    // vector<int> twoSum(vector<int>& nums, int target) {

    //     vector<int> v(2);
    //     unordered_map<int, int> map;
    //     for (int i = 0; i < nums.size(); ++i)
    //     {
    //         map.insert(pair<int, int>(nums.at(i), i));
    //     }

    //     for (int i = 0; i < nums.size(); ++i)
    //     {
    //         auto found = map.find(target - nums.at(i));
    //         if (found != map.end() && found->second != i)  //注意：两个元素的下标不能相同  题目：你不能重复利用这个数组中同样的元素。
    //         {
    //             v.at(0) = i;
    //             v.at(1) = found->second;
    //             return v;
    //         }
    //     }

    //     return v;
    // }
    ```

    #### 2、使用hash表，一次遍历数组
    ```c
    vector<int> twoSum(vector<int>& nums, int target) {

        vector<int> v(2);
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); ++i)
        {
            map.insert(pair<int, int>(nums.at(i), i));

            auto found = map.find(target - nums.at(i));
            if (found != map.end() && found->second != i) //注意：下标不能相同  题目：你不能重复利用这个数组中同样的元素。
            {
                //注意：这里的found->second会小于i
                v.at(0) = found->second;
                v.at(1) = i;

                return v;
            }
        }

        return v;
    }
    ```

    #### 3、先从hash表中找，找不到再保存
    ```c
    vector<int> twoSum(vector<int>& nums, int target) {

        vector<int> v(2);
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); ++i)
        {
            auto found = map.find(target - nums.at(i));
            if (found != map.end())
            {
                //注意：这里的found->second会小于i（因为i还没存）
                v.at(0) = found->second;
                v.at(1) = i;

                return v;
            }

            map.insert(pair<int, int>(nums.at(i), i));
        }

        return v;
    }
```
   (作者：eric-345)
