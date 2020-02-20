# 按奇偶排序数组Ⅱ
***
#### 2020.02.20

### 问题
>给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。
对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。
你可以返回任何满足上述条件的数组作为答案。

### 示例
>输入：[4,2,5,7]
输出：[4,5,2,7]
解释：[4,7,2,5]，[2,5,4,7]，[2,7,4,5] 也会被接受。

>提示：
1.2 <= A.length <= 20000
2.A.length % 2 == 0
3.0 <= A[i] <= 1000

### 思路
>查找整个数组将偶数置于偶数位，再查找数组将奇数置于奇数位。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sortArrayByParityII(int* A, int ASize, int* returnSize){
    int *b;
    b=(int*)malloc(ASize*sizeof(int));
    int j=0;
    for(int i=0;i<ASize;i++){
        if(A[i]%2==0){
            b[j]=A[i];
            j+=2;
        }
    }
    j=1;
    for(int i=0;i<ASize;i++){
        if(A[i]%2!=0){
            b[j]=A[i];
            j+=2;
        }
    }
    *returnSize=ASize;
    return b;

}
```

### 分析
 - 昨天做的是“按奇偶排序数组Ⅰ”，本来以为会比昨天的题有所进阶，没想到还是一样的思路，两遍遍历即可搞定。同样的，因为样例的大小限制，时间复杂度也并不高。
   用时136ms,击败14.05%；内存14.7MB，击败55.39%。
 - 思路同昨天一样，也可以选择双指针进行优化。
 
我们可以根据 A[i]A[i]A[i] 的奇偶性来控制奇偶指针，这样做遍历次数会减少为 1 次。
 ```c++
 class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
        int sizeA = A.size();
        vector<int> res(sizeA);
        for(int i=0, t=0; i<sizeA,t<sizeA; i++)
        {
            if(A[i]%2 == 0)
            {
                res[t] = A[i];
                t += 2;
            }
                
        }
        for(int i=0, k=1; i<sizeA,k<sizeA; i++)
        {
            if(A[i]%2 == 1)
            {
                res[k] = A[i]; 
                k += 2;
            }
        }
        return res;
    }
};

作者：Jesse
```

于方法二的双指针思想，方法二需要一个辅助数组 arrarrarr 存储遍历时的元素，这还不够好。
再从题目出发，题目明确表示数组 AAA 的元素有一半是奇数，一半是偶数，而且要求奇数位置只能存放奇数，偶数位置只能存偶数。这好像是一个突破点。
似乎我们只要找到放在偶数下标的奇数和放在奇数下标的偶数，再把它们交换 swapswapswap 就可以实现原地奇偶排序了。比如当 A=1,2,3,4A=1,2,3,4
A=1,2,3,4 时，
- ```c A[0] = 1，A[1] = 2，swap(A[0], A[1])，A=[2,1,3,4]，even += 2 ```
- ```c A[2] = 3，A[1] = 1，odd += 2 ```
- ```c A[3] = 4，A[2] = 3，swap(A[3], A[1])，A=[2,1,4,3]，even += 2 ```
```c++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
        int sizeA = A.size();
        for(int even=0, odd=1; even<sizeA; even += 2)
        {
            if(A[even]%2 == 1)
            {
                while(A[odd]%2 == 1)
                    odd += 2;
                swap(A[odd],A[even]);
            }
        }
        return A;
    }
};

作者：Jesse
```
作者：peter_pan
