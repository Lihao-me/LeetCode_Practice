# 汉明距离
***
#### 2020.04.29

### 问题
>两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。            
给出两个整数 x 和 y，计算它们之间的汉明距离。                      
注意：                 
0 ≤ x, y < 231.                  

### 示例                   
>输入: x = 1, y = 4               
输出: 2                   
解释:              
1   (0 0 0 1)                       
4   (0 1 0 0)                  
       ↑   ↑                           
上面的箭头指出了对应二进制位不同的位置。                        

### 思路
>先将这两个数异或，再计算异或后的1地数目。

### 代码
```c++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int n=x^y;
        int count=0;
        while(n>0)
        {
            count += n&1;
            n >>=1;
        }
        return count;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :5.9 MB, 在所有 C++ 提交中击败了100.00%的用户。
 
```c++
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
}

作者：one-piece-11
```
