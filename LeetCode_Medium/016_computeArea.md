# 矩形面积Ⅰ
***
#### 2020.07.11

### 问题
>在二维平面上计算出两个由直线构成的矩形重叠后形成的总面积。
每个矩形由其左下顶点和右上顶点坐标表示，如图所示。
[示例]

### 示例
>示例:
输入: -3, 0, 3, 4, 0, -1, 9, 2
输出: 45

### 说明 
>假设矩形面积不会超出 int 的范围。

### 代码
```c++
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int area1=((D-B)*(C-A));
        int area2=(H-F)*(G-E);
        if(D<=F||B>=H||A>=G||C<=E)
        return area1+area2;
        else
        return area1-(min(D,H)-max(B,F))*(min(G,C)-max(A,E))+area2;
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了95.18%的用户内存消耗：5.9 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题是基于今天做的简单难度的“矩形重叠”题目所做的。这道题分两种情况，一种是两个矩阵没有重叠，运用“矩阵重叠”这道题目的判断条件，直接返回两矩阵面积之和；另一种情况是两矩阵出现重叠，
   那么返回两矩阵面积之和减去重合面积。矩阵重叠题目：https://github.com/Lihao-me/LeetCode_Practice/blob/master/LeetCode_Easy/107_isRectangleOverlap.md
 - 这道题值得注意的点是两数相加的溢出问题。虽然题目已经说明了矩形面积不会超出int范围，但是两个矩形面积相加可能超出范围。所以要先减后加。
 - 而题解中的事件差异也主要体现在条件判断的方式上。
 
 ### 优解
 #### 1.0ms范例
 ```c++
 class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
            //  AC EG   //BD FH

        int sum1=abs(C-A)*abs(D-B);
        int sum2=abs(G-E)*abs(H-F);
        bool flag=max(A,E)<min(C,G)&&max(B,F)<min(D,H);
        int sum3=flag?abs(min(C,G)-max(A,E))*abs(min(D,H)-max(B,F)):0;

        return sum1-sum3+sum2;

    }
};
```

#### 2.8140kb范例
```c++
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        
        int a = max(A, E);
        int b = max(B, F);
        int c = min(C, G);
        int d = min(D, H);
        int l = a < c ? c-a : 0;
        int h = b < d ? d-b : 0;
        
        return (C-A) * (D-B) - l * h + (G-E) * (H-F);
    }
};
```
