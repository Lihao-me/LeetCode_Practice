# 高度检查器
***
#### 2020.04.19

### 问题
>学校在拍年度纪念照时，一般要求学生按照 非递减 的高度顺序排列。                     
请你返回能让所有学生以 非递减 高度排列的最小必要移动人数。            
注意，当一组学生被选中时，他们之间可以以任何可能的方式重新排序，而未被选中的学生应该保持不动。                 

### 示例
>输入：heights = [1,1,4,2,1,3]                        
输出：3                 
解释：                         
当前数组：[1,1,4,2,1,3]                   
目标数组：[1,1,1,2,3,4]                
在下标 2 处（从 0 开始计数）出现 4 vs 1 ，所以我们必须移动这名学生。                    
在下标 4 处（从 0 开始计数）出现 1 vs 3 ，所以我们必须移动这名学生。                
在下标 5 处（从 0 开始计数）出现 3 vs 4 ，所以我们必须移动这名学生。              

>输入：heights = [5,1,2,3,4]             
输出：5            

>输入：heights = [1,2,3,4,5]                  
输出：0                                

### 提示
1.1 <= heights.length <= 100
2.1 <= heights[i] <= 100

### 代码
```c++
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        int num=0;
        vector<int>res;
        res=heights;
        sort(heights.begin(),heights.end());
        for(int i=0;i<heights.size();i++)
        {
            if(heights[i]!=res[i])
            num++;
        }
        return num;
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了78.31%的用户内存消耗 :8.5 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题的难点不在编程实现上，
   而是读懂题上。我开始以为是让我写一个程序，判断移动多少次可以实现数组由大到小排序，但是一直提交不了。后来也是看了评论之后才知道仅仅比较
   原数组和排序后数组的元素差异个数即可。

统计排序
```c++
class Solution {
public:
    
    int heightChecker(vector<int>& heights) {
        
        vector<int> count(101,0);
        int n = heights.size();
         vector<int> h_copy(n);
        //统计排序
        for(int i=0;i<n;i++){
            count[heights[i]]++;
        }
        int pre=0;
        for(int i=0;i<101;i++){
            if(count[i]!=0) {
                count[i]+=pre;
                pre = count[i];
            }
            
        }
        for(int i=n-1;i>=0;i--){
            count[heights[i]]--;
            h_copy[count[heights[i]]] = heights[i];
            
        }
        //统计乱序
         int sum = 0;
        for(int i=n-1;i>=0;i--){
            if(h_copy[i]!= heights[i]) sum++;
            
        }
        return sum;

    }
};

作者：gorgeousyurou 
