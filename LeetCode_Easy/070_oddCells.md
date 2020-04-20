# 奇数值单元格的数目
***
#### 2020.04.20

### 问题
>给你一个 n 行 m 列的矩阵，最开始的时候，每个单元格中的值都是 0。                                
另有一个索引数组 indices，indices[i] = [ri, ci] 中的 ri 和 ci 分别表示指定的行和列（从 0 开始编号）。                    
你需要将每对 [ri, ci] 指定的行和列上的所有单元格的值加 1。                    
请你在执行完所有 indices 指定的增量操作后，返回矩阵中 「奇数值单元格」 的数目。                      

### 示例
>输入：n = 2, m = 3, indices = [[0,1],[1,1]]                      
输出：6                  
解释：最开始的矩阵是 [[0,0,0],[0,0,0]]。               
第一次增量操作后得到 [[1,2,1],[0,1,0]]。              
最后的矩阵是 [[1,3,1],[1,3,1]]，里面有 6 个奇数。              

>输入：n = 2, m = 2, indices = [[1,1],[0,0]]               
输出：0                    
解释：最后的矩阵是 [[2,2],[2,2]]，里面没有奇数。                   

### 提示
>1.1 <= n <= 50
2.1 <= m <= 50
3.1 <= indices.length <= 100
4.0 <= indices[i][0] < n
5.0 <= indices[i][1] < m

### 思路
>用两个数组分别记录对应行所应相加的1的个数，最后逐个单元格统计。

### 代码
```c++
class Solution {
public:
    int oddCells(int n, int m, vector<vector<int>>& indices) {
        int *r,*c;
        r=new int[50]();
        c=new int[50]();
        for(int i=0;i<indices.size();i++)
        r[indices[i][0]]++;
        for(int j=0;j<indices.size();j++)
        c[indices[j][1]]++;
        int num=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if((r[i]+c[j])%2!=0)
                num++;
            }
        }
        return num;
    }
};
```

### 分析
 - 执行用时 :8 ms, 在所有 C++ 提交中击败了51.89%的用户内存消耗 :7.5 MB, 在所有 C++ 提交中击败了100.00%的用户。一般的思路可能是和题目所说
   的一样，申请一个二维数组，然后在矩阵上进行操作得到最后的结果。但是向我这样按一维的思路来也挺好理解。

```c++
class Solution {
public:
    int oddCells(int n, int m, vector<vector<int>>& indices) {
        vector<int> r(n,0);
        vector<int> c(m,0);
        for(int i = 0; i < indices.size(); i++)
        {
            r[indices[i][0]] = 1-r[indices[i][0]];
            c[indices[i][1]] = 1-c[indices[i][1]];
        }
        int sum_r = accumulate(r.begin(),r.end(),0);
        int sum_c = accumulate(c.begin(),c.end(),0);
        
        return (sum_r*m+sum_c*n-2*sum_r*sum_c);
    }
};

作者：ji-er
```

位运算
```c++
用false代表偶数，true代表奇数，偶数增加1变为奇数，奇数增加1变为偶数，可以表示为逻辑取反。
当某一行(列)增加偶数次时奇偶不变，增加奇数次时奇偶相反，
记录各行(列)增加的效果，每个单元格由所在行和列的异或决定奇数次还是偶数次。
class Solution {
public:
    int oddCells(const int n, const int m, const vector<vector<int>>& indices) {
        vector<bool> row(n, false);
        vector<bool> col(m, false);
        for (const auto& indice : indices) {
            row[indice[0]] = !row[indice[0]];
            col[indice[1]] = !col[indice[1]];
        }
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                row[i]^col[j] ? ans++ : ans;
        return ans;
    }
};

作者：hareyukai
