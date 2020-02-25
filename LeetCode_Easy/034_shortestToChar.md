# 字符的最短距离
***
#### 2020.02.25

### 问题
>给定一个字符串 S 和一个字符 C。返回一个代表字符串 S 中每个字符到字符串 S 中的字符 C 的最短距离的数组。

### 示例
>输入: S = "loveleetcode", C = 'e'
输出: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]

>说明：
1.字符串 S 的长度范围为 [1, 10000]。
2.C 是一个单字符，且保证是字符串 S 里的字符。
3.S 和 C 中的所有字母均为小写字母。

### 思路
>遍历整个数组，从该数向两边查找，取最小的距离。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* shortestToChar(char * S, char C, int* returnSize){
    int *a;
    int n=strlen(S);
    * returnSize=n;
    a=(int*)malloc(n*sizeof(int));
    for(int i=0;i<n;i++){
        if(S[i]==C)
        a[i]=0;
        else{
            int left=n+1,right=n+1;
            for(int j=i;j>=0;j--){
                if(S[j]==C){
                left=i-j;
                break;}
            }
            for(int k=i;k<n;k++){
                if(S[k]==C){
                right=k-i;
                break;
                }
            }
            a[i]=left<right?left:right;
        }
    }
    return a;
}
```

### 分析
 - 采用了常规的暴力求解法，时间上不是太糟糕。用时12ms，击败66.10%C提交用户；内存7.7MB，击败53.13用户。
 - 值得优化的地方在于可以考虑使用二分查找法或贪心法。

二分查找
将S中所有C的下标记录到一个数组indices中，那么indices是一个升序数组。对于每个位置i，二分查找出它应该被插入的位置即可。
时间复杂度：O(NlogM)，其中N为S的长度，M为C的个数。
```c++
class Solution {
public:
    vector<int> shortestToChar(string S, char pivot) {
        const int n = S.size();

        vector<int> indices; // 保存pivot的所有下标（升序）
        for (int i = 0; i < n; ++i) {
            if (S[i] == pivot) {
                indices.push_back(i);
            }
        }

        vector<int> res(n);
        for (int i = 0; i < n; ++i) {
            if (S[i] == pivot) {
                res[i] = 0;
            } else {
                res[i] = getMinDist(indices, i);
            }
        }
        return res;
    }

    // 二分查找：寻找离`target`最近的下标
    // 返回距离
    int getMinDist(vector<int> &indices, int target) {
        int left = 0, right = indices.size();
        while (left < right) {
            int mid = left + (right - left >> 1);
            if (target > indices[mid]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 两个候选值，取距离更小者
        int res = INT_MAX;
        if (left < indices.size()) {
            res = min(res, abs(indices[left] - target));
        }
        if (left - 1 >= 0) {
            res = min(res, abs(indices[left - 1] - target));
        }
        return res;
    }
};

作者：FatK
```

贪心
对于某个位置i来说，答案肯定在两个候选值中产生：
1.到i左边最近的C的距离
2.到i右边最近的C的距离
除此之外的C根本不可能是答案，所以暴力法是没有必要的。贪心的做法就是：首先从左到右扫描，不断更新C的位置，并求出每个位置
距离它左边的最近的C的距离；然后从右到左扫描，求出每个位置距离它右边的最近的C的距离，取二者中的min即可。
```c++
class Solution {
public:
    vector<int> shortestToChar(string S, char pivot) {
        const int n = S.size();
        vector<int> res(n, INT_MAX);

        // 从左到右扫描，求离左边最近的pivot的距离
        int last = -1; // last表示左边最近的pivot所在下标
        for (int i = 0; i < n; ++i) {
            if (S[i] == pivot) {
                res[i] = 0;
                last = i;
            } else if (last >= 0) {
                res[i] = min(res[i], i - last);
            }
        }

        // 从右到左扫描，求离右边最近的pivot的距离
        last = n; // 此时last表示右边最近的pivot所在下标
        for (int i = n - 1; i >= 0; --i) {
            if (S[i] == pivot) {
                // res[i] = 0;
                last = i;
            } else if (last < n) {
                res[i] = min(res[i], last - i);
            }
        }

        return res;
    }
};

作者：FatK
