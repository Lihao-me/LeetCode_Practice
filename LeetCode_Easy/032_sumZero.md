# 和为零的N个唯一数组
***
#### 2020.02.23

### 问题
>给你一个整数 n，请你返回 任意 一个由 n 个 各不相同 的整数组成的数组，并且这 n 个数相加和为 0 。

### 示例
>输入：n = 5
输出：[-7,-1,1,3,4]
解释：这些数组也是正确的 [-5,-1,1,2,3]，[-3,-1,2,-2,4]。

>输入：n = 3
输出：[-1,0,1]

>输入：n = 1
输出：[0]

>提示：
1 <= n <= 1000

### 思路
>以0为对称中心的数组之和为0。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sumZero(int n, int* returnSize){
    *returnSize=n;
    int *b;
    int i=1;
    b=(int*)malloc(n*sizeof(int));
    if(n==1)
    b[0]='0';

        b[0]=(-1)*(n/2);
        while(i<n){
            if(i==(n+1)/2&&(n%2==0)){
                b[i]=b[i-1]+2;
                i++;
            continue;}
            b[i]=b[i-1]+1;
            i++;
        }
    return b;

}
```

### 分析
 - 这道题的简单之处在于对数组没有其他的限制，只要是每个整数不相同即可，而以0为对称中心的数组恰好符合条件。值得注意的是要分奇偶情况，奇数要保留0；
   偶数要跳过0。
 - 用时12ms，击败17.77%C提交用户；内存消耗7.6MB，击败94.16%C提交用户。
 - 当然，用栈可以缩短时间至0ms。

```c++
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> ans;
        if (0 == n) return ans;
        if (n & 0x1) {
            ans.push_back(0);
        }
        for (int i = 1; i < ((n >> 1) + 1); i++) {
            ans.push_back(i);
            ans.push_back(-1*i);
        }
        return ans;
    }
};

作者：wf0312
