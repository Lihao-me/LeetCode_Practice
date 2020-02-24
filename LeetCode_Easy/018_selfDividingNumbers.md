# 自除数
***
#### 2020.02.24

### 问题
>自除数 是指可以被它包含的每一位数除尽的数。
例如，128 是一个自除数，因为 128 % 1 == 0，128 % 2 == 0，128 % 8 == 0。
还有，自除数不允许包含 0 。
给定上边界和下边界数字，输出一个列表，列表的元素是边界（含边界）内所有的自除数。

### 示例
>输入： 
上边界left = 1, 下边界right = 22
输出： [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]

>注意：
每个输入参数的边界满足 1 <= left <= right <= 10000。

### 思路
>逐一判断该数是否为“自除数”，若是则挑出。

### 代码
```c++
class Solution {
public:
    vector<int> selfDividingNumbers(int left, int right) {
        vector<int>a;
        for(int i=left;i<=right;i++){
            if(isselfDividingNumbers(i))
            a.push_back(i);
        }
        return a;
    }
    bool isselfDividingNumbers(int n){
        int post,num=n;
        if(num==0)
        return 0;
        while(num!=0){
            post=num%10;
            if(post==0||n%post!=0)
            return 0;

            num=num/10;
        }
        return 1;
    }
};
```

### 分析
 - 用时0ms，击败100.00%C++提交；内存8.7MB，击败5.80%。这个代码也算是一种暴力解法了，用时非常短，值得高兴，但是相应的也自然会有内存的牺牲。
 - 使用C++可以利用栈来解，那么用C语言则更冗杂一些。将C的解法放在这里以供对比。
 
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int * selfDividingNumbers( int left , int right , int * returnSize ){
    //used to save the number that meet the requirement
    int * buffer = ( int * )malloc( sizeof( int ) * ( right - left + 1 ) );
    //the index of the buffer
    int index = 0;
    //visiting the sequence of the number
    for( ; left <= right ; left++ ){
        int tmp = left , flag = 1;
        while( tmp != 0 ){
            //the bit must not 0 or should meet the conditions
            if( ( tmp % 10 == 0 ) || ( left % ( tmp % 10 ) ) != 0 ){
                flag = 0;
                break;
            } 
            tmp /= 10;
        }
        //save the number in the array
        if( flag == 1 ){
            *( buffer + index++ ) = left;
        }
    }
    //return the length of the array
    * returnSize = index;
    return buffer;
}

作者：big0rangecat-2
