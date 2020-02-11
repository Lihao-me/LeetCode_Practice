# 有效的山脉数组
***
#### 2020.02.11

### 问题
>给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。
让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：
	A.length >= 3
	在 0 < i < A.length - 1 条件下，存在 i 使得：
		A[0] < A[1] < ... A[i-1] < A[i] 
		A[i] > A[i+1] > ... > A[B.length - 1]

### 示例
>输入：[2,1]
输出：false

>输入：[3,5,5]
输出：false

>输入：[0,3,2,1]
输出：true

### 思路
>1.从头开始找，找到比下一个数大的数时即为“山顶”，从“山顶”接着往下找若所有数据都递减则符合要求。
>2.从两边开始找，分别找到“山顶”，若这两个“山顶”是同一个“山顶”，则符合要求。

### 代码
循环
```c
bool validMountainArray(int* A, int ASize){
    if(ASize<3)
    return 0;
    int i;
    for(i=0;i<ASize-1;i++){
        if(A[i+1]<A[i])
        break;
    }
    if(i==ASize-1||i==0)
    return 0;
    for(i;i<ASize-1;i++){
        if(A[i+1]>=A[i])
        return 0;
    }
    return 1;
}
```
双指针法
```c++
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int start=0,Asize=A.size(),end=Asize-1;
        if(Asize<3)
        return 0;
        for(start;start<=Asize-2&&A[start+1]>A[start];start++);
        for(end;end>0&&A[end-1]>A[end];end--);

        if(start==end&&start!=0&&end!=Asize-1)
        return 1;
        else
        return 0;
    }
};
```

### 分析
 - 看到这道题我马上就想到可以把第一个“山顶”找出来，接着再判断是否这是唯一的山顶，经过不断地调试后终于成功了，但是无论是时间上还是内存上击败地人数都
   少得可怜，用时64ms,内存8.4MB。
 - 于是我马上想到原来做的题所用的“双指针”的方法，从数组两端向内寻找山顶，若是同一个“山顶”则符合要求，这样在代码的效率上是会有所提高的，但是也并没有
   提高太多，用时52ms，内存10.5MB。
 - 我本以为解题就这两种办法了，但网友还是有很多不同的想法，有的我也是第一次见。比如“小型状态机”就很新奇，原理很好理解，就是不断地跳转，但这个模型可以
   在很多实例中使用。另一种就是直接找最大地数进行判断操作，也很快了。
```c++
   class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        if(A.size()<3)
            return false;
        //找A中的最大值的位置maxpos，若最大值有重复，则找到最后一个出现的最大值位置
        //如果maxpos != 0 && maxpos != A.size()-1 && A[0] != max &&countmax == 1，返回true
        //否则返回false
        int max = A[0];     
        int maxpos = 0;  //最后一个出现的最大值位置
        int countmax = 1; //最大值的个数
        for(int i = 1; i < A.size(); i++){
            if(A[i] > max){
                max = A[i];
                maxpos = i;
                countmax = 1;
            } else if(A[i] == max){
                countmax++;
            }
            //判断是否满足A[0] < A[1] < ... A[i-1] < A[i]
            if(A[i] < max && i < maxpos && A[i] <= A[i-1])
                return false;
                //判断是否满足A[i] > A[i+1] > ... > A[B.length - 1]
            else if(i != A.size() - 1 && A[i] < max && i > maxpos && A[i] <= A[i+1])
                return false;
        }
        if(maxpos != 0 && maxpos != A.size()-1 && A[0] != max && countmax == 1)
            return true;
        else 
            return false;
    }
};

作者：MSjunlong
```
小型状态机
```c++
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int state = 0;
        for(int i=1;i<A.size();i++){
            switch(state){
                case 0:
                    if(A[i]>A[i-1]) state = 1;
                    else return false;
                    break;
                case 1:
                    if(A[i]<A[i-1])state = 2;
                    else if(A[i]==A[i-1])return false;
                    break;
                case 2:
                    if(A[i]>=A[i-1])return false;
                    break;
            }
        }
        return state == 2;
    }
};

作者：GoatKeeper
