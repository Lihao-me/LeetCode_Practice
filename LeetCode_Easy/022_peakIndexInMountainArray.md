# 山脉数组的峰顶索引
***
#### 2020.02.14

### 问题
>我们把符合下列属性的数组 A 称作山脉：
	A.length >= 3
	存在 0 < i < A.length - 1 使得A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
给定一个确定为山脉的数组，返回任何满足 A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1] 的 i 的值。

### 示例
>输入：[0,1,0]
输出：1

>输入：[0,2,1,0]
输出：1

>提示：
1.3 <= A.length <= 10000
2.0 <= A[i] <= 10^6
3.A 是如上定义的山脉

### 思路
>从头开始查找，当遇到下一个数比这个数小时即为“山顶”。

### 代码
```c
int peakIndexInMountainArray(int* A, int ASize){
    int start=0;
    for(start;start<ASize-1;start++){
        if(A[start]>A[start+1])
        break;
    }
    return start;

}
```

### 分析
 - 昨天求解了一道判断“山脉数组”的题目，时间复杂度上比这道题要大，所以今天这道题就变得很简单了，而且已知这是一组“山脉”数组了，所以更大大降低了题目的
   难度。
 - 但是令我选这道题的缘由是网友的题解，虽然他们将“小题大做”，但是这些方法确实可以给我以启发以及学习的空间。
 
 向前差分
```c++
 class Solution {
public:
    int peakIndexInMountainArray(vector<int>& A) {
        if (A.empty())
        {
            return NULL;
        }
        //前向差分
        vector<int>::iterator it;
        for (it = A.begin()+1; it != A.end(); ++it)
        {
            if ( *(it-1)<*(it) && *(it)>*(it+1) )
            {
                // return it - A.begin();
                return distance(A.begin(), it);
            }
        }
        return NULL;
    }
};

作者：1050669722
```
最大值
```c++
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& A) {
        if (A.empty())
        {
            return NULL;
        }
        //最大值
        vector<int>::iterator biggest = max_element(A.begin(), A.end());
        return distance(A.begin(), biggest);
    }
};

作者：1050669722
```
二分查找
```c++
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& A) {
        if (A.empty())
        {
            return NULL;
        }
        //二分查找
        int p = 0, q = A.size()-1, k;
        while (p < q)
        {
            k = (p+q)/2;
            if (A[k-1]<A[k] && A[k]<A[k+1])
            {
                p = k + 1;
            }
            else if (A[k-1]>A[k] && A[k]>A[k+1])
            {
                q = k - 1;
            }
            else
            {
                return k;
            }  
        }
        return p;
    }
};

作者：1050669722
