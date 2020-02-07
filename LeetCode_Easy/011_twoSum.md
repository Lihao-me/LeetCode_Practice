# 两数之和Ⅱ
***
#### 2020.02.02

### 问题
>给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。
函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

>说明:
1.返回的下标值（index1 和 index2）不是从零开始的。
2.你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

### 示例
>输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* numbers, int numbersSize, int target, int* returnSize){
    int start=0,end=numbersSize-1;
    while(start<end){
        int flag=numbers[start]+numbers[end];
        if(flag>target)
        end=end-1;
        else if(flag<target)
        start=start+1;
        else{
            break;
        }
    }
    *returnSize=2;
    int *a=(int*)malloc(2*sizeof(int));
    a[0]=start+1;
    a[1]=end+1;
    return a;
}
```

### 分析
 - 今天这道题的完成可真是有点艰难呐。首先想到的就是暴力解法，将所有数据两两组合，相加为目标值即可终止遍历，不出我所料超出时间限制，当然其中的一些细节
   错误以及查找也是用了很长时间。
 - 根据网友的解法我又学习了一下他们所说的二分法，类似于数学上“夹逼”的思想找出这两个下标。编写的过程中就就觉察到其运行也不比暴力求解简单多少。依然不出
   所料超出时间限制。
 - 最后不得不学习双指针思路，因为这个数组按升序排列，这一条件用双指针可以得到充分利用。其方法是让两指针分别指向数组首尾，当相加大于目标值，说明尾指针
   与任何数相加都大于目标值，则尾指针向前逼近；同理，当相加值小于目标值，说明首指针过小，应向后逼近，直至出现目标值。
 - 双指针时间上依然不是很理想，仅击败42.92%，但内存击败了85.37%。虽然前两个方法没有通过测试，但也算两种方法了。也贴上以纪念我的一下午的编写。
 - 这道题没有选择cpp进行编写，之后发现事实上可以用这种语言使用map进行破解，那也把cpp的这一方法列出以供学习。
 
 C语言之暴力解法：
 ```c
 /**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* numbers, int numbersSize, int target, int* returnSize){
    int *a;
    a=(int*)malloc(2*sizeof(int));
    int index1,index2,k=0;
    for(int i=0;i<numbersSize-1;i++){
        for(int j=i+1;j<=numbersSize-1;j++){
            if(numbers[i]+numbers[j]==target){
                index1=i;
                index2=j;
                k=1;
                break;
            }
        }
        if(k==1)
        break;
    }
    *returnSize=2;
    a[0]=index1+1;
    a[1]=index2+1;
    return a;
}
```

C语言之二分法：
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* numbers, int numbersSize, int target, int* returnSize){
    int *a=(int*)malloc(2*sizeof(int));
    int index1,index2,k=0,start,end,middle;
    for(int i=0;i<numbersSize-1;i++){
        start=i+1;
        end=numbersSize-1;
        middle=(start+end)/2;
        while(start<=end){
            if(numbers[middle]>target-numbers[i])
            end=middle-1;
            else if(numbers[middle]<target-numbers[i])
            start=middle+1;
            else{
                k=1;
                index1=i;
                index2=middle;
                break;
            }
        }
        if(k==1)
        break;
    }
    *returnSize=2;
    a[0]=index1+1;
    a[1]=index2+1;
    return a;
}
```

C++之Map解法：
```c++
版本一
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int,int> mp;
        vector<int> res;
        for(int i=0;i<numbers.size();i++) mp[numbers[i]]=i;
        for(int i=0;i<numbers.size();i++){
            if(mp.count(target-numbers[i])&&mp[target-numbers[i]]!=i){
                res.emplace_back(i+1);
                res.emplace_back(mp[target-numbers[i]]+1);
                break;
            }
        }
        return res;
    }
};

版本二，应用空间相对小一点
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int,int> mp;
        vector<int> res;
        int tmp=target-numbers[0],i=0;
        if(tmp<0) return res;
        for(;i<numbers.size();i++){
            if(numbers[i]>tmp) break;
            mp[numbers[i]]=i;
        } 
        for(int j=0;j<i;j++){
            if(mp.count(target-numbers[j])&&mp[target-numbers[j]]!=j){
                res.emplace_back(j+1);
                res.emplace_back(mp[target-numbers[j]]+1);
                break;
            }
        }
        return res;
    }
};

作者：24shi-01fen-_00_01
