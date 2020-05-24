# 重塑矩阵
***
#### 2020.05.24

### 问题
>在MATLAB中，有一个非常有用的函数 reshape，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。
给出一个由二维数组表示的矩阵，以及两个正整数r和c，分别表示想要的重构的矩阵的行数和列数。
重构后的矩阵需要将原始矩阵的所有元素以相同的行遍历顺序填充。
如果具有给定参数的reshape操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

### 示例
>输入:              
nums =                              
[[1,2],
 [3,4]]                       
r = 1, c = 4           
输出:             
[[1,2,3,4]]                             
解释:                         
行遍历nums的结果是 [1,2,3,4]。新的矩阵是 1 * 4 矩阵, 用之前的元素值一行一行填充新矩阵。                           

>输入:                      
nums =                    
[[1,2],                   
 [3,4]]                  
r = 2, c = 4                 
输出:                  
[[1,2],                 
 [3,4]]                 
解释:                   
没有办法将 2 * 2 矩阵转化为 2 * 4 矩阵。 所以输出原矩阵。                          
                    
### 注意
>2.给定矩阵的宽和高范围在 [1, 100]。                 
1.给定的 r 和 c 都是正数。           

### 代码
```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        if(nums.empty()||nums[0].empty())
        return nums;
        int R=nums.size();
        int C=nums[0].size();
        int n=R*C;
        if(n%r!=0||n/r!=c)
        return nums;
        vector<vector<int>>
        res(r,vector<int>(c,0));
        for(int i=0;i<n;i++){
            res[i/c][i%c]=nums[i/C][i%C];
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :20 ms, 在所有 C++ 提交中击败了71.31%的用户内存消耗 :10.6 MB, 在所有 C++ 提交中击败了100.00%的用户。这种解法可能是比较直接的，
   但是也非常好理解。题解中有人使用了队列，也很不错。
   
### 优解
队列
```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
    	size_t num_eles = nums.size() * nums[0].size();
    	if(static_cast<size_t>(r*c) > num_eles) return nums;

    	list<int> orig_eles;
    	for(auto& row:nums){
    		for(auto& col:row){
    			orig_eles.push_back(col);
    		}
    	}
//    	//for debug
//    	for(auto& iter:orig_eles){
//    		cout << iter << " ";
//    	}
//    	cout << endl;

    	vector<vector<int>> matrix_reshape;
    	for(int i=0;i<r;++i){
    		vector<int> curr_row;
    		for(int j=0;j<c;++j){
    			curr_row.push_back(orig_eles.front());
    			orig_eles.pop_front();
    		}
    		matrix_reshape.push_back(curr_row);
    	}

    	return matrix_reshape;
    }
};

作者：user2473E
```
