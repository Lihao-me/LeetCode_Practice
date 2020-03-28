# 递归乘法
***
#### 2020.03.28

### 问题
>递归乘法。 写一个递归函数，不使用 * 运算符， 实现两个正整数的相乘。可以使用加号、减号、位移，但要吝啬一些。

### 示例
>输入：A = 1, B = 10           
输出：10              

>输入：A = 3, B = 4                      
输出：12                       

### 提示
>保证乘法范围不会溢出               

### 思路
>将被乘数相加乘数次。

### 代码
```c
int multiply(int A, int B){
    if(B==0)
    return 0;
    else
    return A+multiply(A,B-1);
}
```

### 分析
 - 执行用时 :0 ms, 在所有 C 提交中击败了100.00%的用户内存消耗 :5 MB, 在所有 C 提交中击败了100.00%的用户。做完这道题有点诧异这么简单的题为
   什么会被列为中档题。题目给的提示感觉很清楚了。不能使用乘号但使用递归。递归本身就是一种非常高效的算法，所以双百也不足为奇。
 - 那就贴上我并不熟悉的位运算和二进制算法吧。
 
二进制乘法
```c++
class Solution {
public:
    int multiply(int A, int B) {
        int ans=0;
        for(int i=0;i<32&&B!=0;i++){
            //判断B的最后一位是0还是1来决定结果是加上A<<i还是加0；
            ans=(B&1)==1?ans+(A<<i):ans;
            B=B>>1;
        }
        return ans;
    }
};

作者：solo-tu
```

尾递归
```c++
int multiply(int A, int B) {
  if (B == 0) return 0;
  if (B % 2 == 1) return A + multiply(A, B - 1);
  else            return multiply(A, B / 2) << 1;
}

作者：rhanqtl-2
```

位移解法
```c++
class Solution {
public:
    int multiply(int A, int B) {
        if (A < B) {
            return multiply(B, A);
        }

        if (A==0 || B == 0) {
            return 0;
        }

        int num = multiply(A<<1, B>>1);
        if (B&1 == 1) {            
            return num + A;
        } else {            
            return num;
        }
    }
};

作者：bmyxb
