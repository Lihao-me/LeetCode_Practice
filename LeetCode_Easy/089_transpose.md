# 矩阵转置
***
#### 2020.05.21

### 问题
>给定一个矩阵 A， 返回 A 的转置矩阵。                   
矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。                                 

### 示例
>输入：[[1,2,3],[4,5,6],[7,8,9]]                      
输出：[[1,4,7],[2,5,8],[3,6,9]]                             

>输入：[[1,2,3],[4,5,6]]                     
输出：[[1,4],[2,5],[3,6]]                          

### 提示
>1.1 <= A.length <= 1000                   
2.1 <= A[0].length <= 1000       

### 代码
```c++
class Solution {
public:
    vector<vector<int>> transpose(vector<vector<int>>& A) {
        vector<vector<int>>res(A[0].size());
        for(int i=0;i<A[0].size();i++)
        {
            for(int j=0;j<A.size();j++)
            {
                res[i].push_back(A[j][i]);
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :24 ms, 在所有 C++ 提交中击败了24.43%的用户内存消耗 :10 MB, 在所有 C++ 提交中击败了100.00%的用户。之前做过直接将矩阵转置输出的
   题目，现在在函数内操作，所以很不熟练。
 - 和直接输出的操作一样，我们将其按转置后顺序置于一个新的二维数组就行了。看大家的题解也都大同小异，也没有更优的解了。
 
### 优解
```c++
class Solution {
public:
    vector<vector<int>> transpose(vector<vector<int>>& A) {
        int rows = A.size();
        int cols = A[0].size();
        vector<vector<int>> result(cols, vector<int>(rows, 0));
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                 result[j][i] = A[i][j];
            }
        }
        return result;
    }
};
```
