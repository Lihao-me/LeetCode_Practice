# 排列硬币
***
#### 2020.09.03

### 问题
>你总共有 n 枚硬币，你需要将它们摆成一个阶梯形状，第 k 行就必须正好有 k 枚硬币。
给定一个数字 n，找出可形成完整阶梯行的总行数。
n 是一个非负整数，并且在32位有符号整型的范围内。

### 示例
>n = 5         
硬币可排列成以下几行:                           
¤                                          
¤ ¤                    
¤ ¤                             
因为第三行不完整，所以返回2.             

>n = 8                
硬币可排列成以下几行:                    
¤                
¤ ¤               
¤ ¤ ¤                  
¤ ¤                     
因为第四行不完整，所以返回3.          

### 代码
```cpp
class Solution {
public:

    int arrangeCoins(int n) {
        return int((sqrt(1.0+8.0*n)-1)/2.0);
    }
};
```

### 分析
 - 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗：5.9 MB, 在所有 C++ 提交中击败了44.11%的用户。
 - 这道题非常的简单，但开始的想复杂了，想通过循环把这个数给找出来，但是直接从数学的角度就可以知道了。值得注意的是这个方法对于浮点数的使用。
 
### 优解
#### 1.0ms解法（二分查找）
```cpp
class Solution {
public:
    int arrangeCoins(int n) {
        int left = 0,right = n;
        while(left <= right)
        {
            long mid = left + (right - left) / 2;
            long long cost = (mid + 1) * mid / 2;
            if(cost == n) 
                return mid;
            if(cost > n) 
                right = mid-1;
            else 
                left = mid + 1;
        }
        return right;
    }
};
```

#### 2.最少内存消耗（5724kb解法）
```cpp
class Solution {
public:
    int arrangeCoins(int n) {
        int left = 0,right = n;
        while(left <= right)
        {
            long mid = left + (right - left) / 2;
            long long cost = (mid + 1) * mid / 2;
            if(cost == n) 
                return mid;
            if(cost > n) 
                right = mid-1;
            else 
                left = mid + 1;
        }
        return right;
    }
};
```
