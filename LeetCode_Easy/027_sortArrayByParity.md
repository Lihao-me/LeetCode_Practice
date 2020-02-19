# 按奇偶排序数组
***
#### 2020.02.19

### 问题
>给定一个非负整数数组 A，返回一个数组，在该数组中， A 的所有偶数元素之后跟着所有奇数元素。
你可以返回满足此条件的任何数组作为答案。

### 示例
>输入：[3,1,2,4]
输出：[2,4,3,1]
输出 [4,2,3,1]，[2,4,1,3] 和 [4,2,1,3] 也会被接受。

>提示：
1.1 <= A.length <= 5000
2.0 <= A[i] <= 5000

### 思路
>将数组中的偶数挑出来，再将奇数安排在后面。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sortArrayByParity(int* A, int ASize, int* returnSize){
    int *b,j=0;
    b=(int*)malloc(ASize*sizeof(int ));
    for(int i=0;i<ASize;i++){
        if(A[i]%2==0){
            b[j]=A[i];
            j++;
        }
    }
    for(int i=0;i<ASize;i++){
        if(A[i]%2!=0){
            b[j]=A[i];
            j++;
        }
    }
    *returnSize=ASize;
    return b;

}
```

### 分析
 - 我的做法是申请一个相同长度的数组，一次循环将偶数提取并赋给数组，二次循环将奇数赋给数组。这道题的简单之处是对最终的数组的排序没有要求。之前做过一道
   更复杂的还要分别对奇数和偶数组排序。用时36ms，击败68.36%；内存24MB，击败24.86%。
 - 另一种双指针的解法也非常巧妙。
 
 左指针寻找奇数，右指针寻找偶数，再进行交换。
 ```c++
 class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        int i = 0, j = A.size()-1;
        while(i<j){
            while(A[i]%2==0&&i<j)  i++;
            while(A[j]%2==1&&i<j)  j--;
            swap(A[i],A[j]);
        }
        
        return A;
    }
};

作者：Daning_NFH
