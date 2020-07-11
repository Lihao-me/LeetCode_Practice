# 矩阵重叠
***
#### 2020.07.11

### 问题
>矩形以列表 [x1, y1, x2, y2] 的形式表示，其中 (x1, y1) 为左下角的坐标，(x2, y2) 是右上角的坐标。              
如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。                              
给出两个矩形，判断它们是否重叠并返回结果。                                   

### 示例
>输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]                  
输出：true                       

>输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]                   
输出：false                        

### 提示
>  1.两个矩形 rec1 和 rec2 都以含有四个整数的列表的形式给出。                      
	2.矩形中的所有坐标都处于 -10^9 和 10^9 之间。                                         
	3.x 轴默认指向右，y 轴默认指向上。                                    
	4.你可以仅考虑矩形是正放的情况。                                 
  
### 代码
```c++
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        return !(rec1[3]<=rec2[1]||rec1[1]>=rec2[3]||rec1[0]>=rec2[2]||rec1[2]<=rec2[0]);
    }
};
```

### 分析
 - 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗：8 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题的主要步骤就是判断条件，开始的时候是准备把所有的重叠条件列出来，但是发现太复杂了，不停地提交试错再增加条件不是很好的办法，所以通过逆向思考，判断不重叠的条件就简单了很多。分别是
   第二矩形分别位于第一矩形的上方、下方、左方和右方。这样一行简单的代码就判断出来了。
 - 由于大家的做法大都大同小异，所以仅仅是判断条件的不同而已。
 
### 优解
#### 1.最大值比较法
```c++
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        return (min(rec1[2], rec2[2]) > max(rec1[0], rec2[0]) &&
                min(rec1[3], rec2[3]) > max(rec1[1], rec2[1]));
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/rectangle-overlap/solution/ju-xing-zhong-die-by-leetcode-solution/
```

#### 2.x、y分别比较法
```c++
bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
    bool x_overlap = !(rec1[2] <= rec2[0] || rec2[2] <= rec1[0]);
    bool y_overlap = !(rec1[3] <= rec2[1] || rec2[3] <= rec1[1]);
    return x_overlap && y_overlap;
}

作者：nettee
链接：https://leetcode-cn.com/problems/rectangle-overlap/solution/tu-jie-jiang-ju-xing-zhong-die-wen-ti-zhuan-hua-we/
```
