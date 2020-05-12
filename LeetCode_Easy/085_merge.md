# 合并排序的数组
***
#### 2020.05.12

### 问题
>给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。         
初始化 A 和 B 的元素数量分别为 m 和 n。                                           

### 示例
>输入:                    
A = [1,2,3,0,0,0], m = 3                                   
B = [2,5,6],       n = 3                              
输出: [1,2,2,3,5,6]                                                        

### 说明
>A.length == n + m               

### 代码
```c++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        for(int i=0;i<n;i++)
        {
            A[m+i]=B[i];
        }
        sort(A.begin(),A.end());
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了80.16%的用户内存消耗 :9.4 MB, 在所有 C++ 提交中击败了100.00%的用户。这种方法最简单也最好想。除此
   之外也可以考虑双指针等思路。
   
### 优解
#### 1.双指针
```c++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        int pa = 0, pb = 0;
        int sorted[m + n];
        int cur;
        while (pa < m || pb < n) {
            if (pa == m)
                cur = B[pb++];
            else if (pb == n)
                cur = A[pa++];
            else if (A[pa] < B[pb])
                cur = A[pa++];
            else
                cur = B[pb++];
            sorted[pa + pb - 1] = cur;
        }
        for (int i = 0; i != m + n; ++i)
            A[i] = sorted[i];
    }
};

作者：LeetCode-Solution
```

#### 2.逆向双指针
```c++
PythonC++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        int pa = m - 1, pb = n - 1;
        int tail = m + n - 1;
        int cur;
        while (pa >= 0 || pb >= 0) {
            if (pa == -1)
                cur = B[pb--];
            else if (pb == -1)
                cur = A[pa--];
            else if (A[pa] > B[pb])
                cur = A[pa--];
            else
                cur = B[pb--];
            A[tail--] = cur;
        }
    }
};

作者：LeetCode-Solution
```

#### 3.三指针
```c++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        if(n==0) return;//如果沒有B
         int all=m+n-1,p=m-1,q=n-1;
         while(all!=p){
             if(p>=0&&A[p]>=B[q]) A[all--]=A[p--];//如果還有A中元素并且P位置大於q位置
             else A[all--]=B[q--];
        }
    }
};

作者：dai-52
