# 托普利茨矩阵
***
#### 2020.04.14

### 问题
>如果一个矩阵的每一方向由左上到右下的对角线上具有相同元素，那么这个矩阵是托普利茨矩阵。
给定一个 M x N 的矩阵，当且仅当它是托普利茨矩阵时返回 True。

### 示例
>输入:              
matrix = [             
  [1,2,3,4],                
  [5,1,2,3],              
  [9,5,1,2]                  
]                 
输出: True                   
解释:                     
在上述矩阵中, 其对角线为:                       
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]"。                    
各条对角线上的所有元素均相同, 因此答案是True。                      

>输入:                  
matrix = [                    
  [1,2],                     
  [2,2]                 
]                   
输出: False                
解释:                      
对角线"[1, 2]"上的元素不同。                   

### 说明
>1.matrix 是一个包含整数的二维数组。
2.matrix 的行数和列数均在 [1, 20]范围内。
3.matrix[i][j] 包含的整数在 [0, 99]范围内。

### 进阶
>1.如果矩阵存储在磁盘上，并且磁盘内存是有限的，因此一次最多只能将一行矩阵加载到内存中，该怎么办？
2.如果矩阵太大以至于只能一次将部分行加载到内存中，该怎么办？

### 代码
```c++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        int r=matrix.size();
        int c=matrix[0].size();
        if(r==1&&c==1)
        return 1;
        
        int a[100];
        memset(a,-1,sizeof(a));
        for(int i=0;i<r;i++)
        {
            for(int j=0;j<c;j++)
            {
                if(a[i-j+30]==-1)
                {
                    a[i-j+30]=matrix[i][j];
                    //cout<<a[i-j+30]<<"/"<<matrix[i][j]<<endl;
                }
                else{
                    if(a[i-j+30]!=matrix[i][j]){
                    //cout<<a[i-j+30]<<","<<matrix[i][j]<<endl;
                    return 0;}
                }
            }
        }
        return 1;
    }
};
```

### 分析
 - 执行用时 :12 ms, 在所有 C++ 提交中击败了88.12%的用户内存消耗 :16.5 MB, 在所有 C++ 提交中击败了14.29%的用户。这道题是借鉴了别人的思路。因
   为暴力地比较所有对角线上的数不是很好。数学方法则似乎更为简洁。
   
和左上角比较
```c++
 bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        if(matrix.size()==1 || matrix[0].size()==1) return true;
        for(int i=1; i<matrix.size(); i++){
            for(int j=1; j<matrix[0].size(); j++){
                if(matrix[i][j]!=matrix[i-1][j-1]) return false;
            }
        }
        
        return true;
    }

作者：shi-fen-fang-qi-zhong
```

比较相同元素
```c++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        // 1 2 3 4   ->     1 2 3 4
        // 5 1 2 3   ->   5 1 2 3
        // 9 5 1 2   -> 9 5 1 2
        for (int i = 0; i < matrix.size(); i++) {
            for (int j = 0; j < matrix[0].size(); j++) {
                // 这里 i,j 必须大于 0，因为后面需要比较上一行前一列的元素 i - 1, j - 1
                if (i > 0 && j > 0 && matrix[i][j] != matrix[i - 1][j - 1])
                    return false;
            }
        }

        return true;
    }
};

作者：dlonng
