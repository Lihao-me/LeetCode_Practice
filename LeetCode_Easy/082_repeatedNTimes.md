# 重复N次的元素
***
#### 2020.05.08

### 问题
>在大小为 2N 的数组 A 中有 N+1 个不同的元素，其中有一个元素重复了 N 次。    
返回重复了N次的那个元素。              

>输入：[1,2,3,3]                                                        
输出：3                       

>输入：[2,1,2,5,3,2]                                      
输出：2                             

>输入：[5,1,5,2,5,3,5,4]                        
输出：5                              

### 提示
>1.4 <= A.length <= 10000                  
2.0<= A[i] < 10000                 
3.A.length 为偶数                       

### 代码
```c++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {
        int n=A.size();
        int num=1,max=0,flag=A[0];
        sort(A.begin(),A.end());
        for(int i=1;i<n;i++)
        {
            if(A[i]!=A[i-1])
            {
                if(num>max){
                max=num;
                flag=A[i-1];}

                num=1;
            }
            num++;
        }
        if(num>max)
        flag=A[n-1];
        
        return flag;
    }
};
```

### 分析
 - 执行用时 :120 ms, 在所有 C++ 提交中击败了17.79%的用户内存消耗 :23.9 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 比较暴力的解法，用了一次排序，时间效果当然不好。
 
摩尔投票
```c++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {
        int n = A[0];
        int c = 1;
        int N = A.size();
        for (int i = 1; i < A.size(); ++i) {
            if (A[i] == n) {
                ++c;
            } else if (--c == 0) {
                n = A[i];
                c = 1;
            }
            if (c > 1) return n;
        }
        c = 0;
        for (auto x : A) c += x == n;
        return c > 1 ? n : A[0];
    }
};

作者：da-li-wang
```

检查n-1与n-2是否相同
```c++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {
        int nSize = A.size();
        if (nSize == 0)
            return 0;
        int nTag = A[0];
        int nLast = A[1];
        for (int i = 2; i < nSize; ++i){
            int nTmp = A[i];
            if (nTmp == nTag || nTmp == nLast)
                return nTmp;
            else{
                nTag = nLast;
                nLast = nTmp;
            }
        }
        return nLast;
    }
};

作者：devil-35
```

逐个比较
```c++
    int repeatedNTimes(vector<int>& A) {
       for(int i = 0;i<A.size();++i)
       for(int j = i+1;j<A.size();++j)
          if(A[i] == A[j]) return A[i];
    return 0;
    }

作者：shi-fen-fang-qi-zhong
```

随机算法
```c++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {

        for(int i=0; i<20; i++){
            srand(time(0)+i*12434);
            int q=rand()%A.size();
            srand(time(0)+i*12434+43254);
            int c=rand()%A.size();
            if(A[q]==A[c] && q!=c)
                return A[q];
        }
        return -1;
    }
};

作者：tengyun-3
