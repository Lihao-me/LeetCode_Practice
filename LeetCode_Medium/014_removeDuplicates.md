# 删除排序数组中的重复项Ⅱ
***
#### 2020.03.31

### 问题
>给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

### 示例
>给定 nums = [1,1,1,2,2,3],
函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。                 
你不需要考虑数组中超出新长度后面的元素。

>给定 nums = [0,0,1,1,1,1,2,3,3],
函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。          
你不需要考虑数组中超出新长度后面的元素。

### 说明
>为什么返回数值是整数，但输出的答案是数组呢?            
请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:
```c+++
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

### 代码
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()<=2)
        return nums.size();
        int sp=1;
        for(int fp=2;fp<nums.size();fp++)
        {
            if(nums[fp]!=nums[sp-1])
            nums[++sp]=nums[fp];
        }
        return sp+1;
    }
};
```

### 分析
 - 执行用时 :8 ms, 在所有 C++ 提交中击败了90.34%的用户内存消耗 :6.5 MB, 在所有 C++ 提交中击败了100.00%的用户。题目中有一句话：
   为什么返回数值是整数，但输出的答案是数组呢?很发人深思。我第一次尝试的时候还试图通过修改元素然后重排序进行实现。但看样子其内部调用
   并不是我所想象的。所以只好看了大家的题解。
 - 发现大部分人用的都是快慢指针。印象中原来做过一道用快慢指针解答的题，似曾相识。而具体的运作交换机制这个网友的图解很不错：
   https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/solution/c-shuang-zhi-zhen-dan-ci-sao-miao-tu-jie-by-dexin/
   
替换无效位
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        // 特殊情况处理
        if(nums.size() == 0) return 0;
        // k 存放当前有效位数，flag 为判断符，flag为true：当前已经有2个了，flag为false：当前是第1个
        int k = 0;
        bool flag = false;
        for(int i = 1; i < nums.size(); i ++)
        {
            if(nums[i] == nums[k])
            {
                if(flag) continue;
                flag = true;
            }
            else flag = false;
            k ++;
            nums[k] = nums[i];
        }
        // 务必注意这里返回的是新数组的“长度”，因此是 k + 1
        return k + 1;
    }
};

作者：iswJXkYVv3
```

erase
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }
        uint8_t count{1};
        int tempN = nums[0];
        auto p = nums.begin()+1;
        while(p != nums.end()){
            if (*p == tempN) {
                tempN = *p;
                ++count;
                if (count > 2) {
                    nums.erase(p);
                }else{
                    p++;
                }
            } else {
                tempN = *p;
                count = 1;
                p++;
            }
        }
        return nums.size();
    }
};

作者：zhang-jia-kun
