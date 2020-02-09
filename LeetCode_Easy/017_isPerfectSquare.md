# 有效的完全平方数
***
#### 2020.02.09

### 问题
>给定一个正整数 num，编写一个函数，如果 num 是一个完全平方数，则返回 True，否则返回 False。
说明：不要使用任何内置的库函数，如  sqrt。

### 示例
>输入：16
输出：True

>输入：14
输出：False

### 代码
```c
bool isPerfectSquare(int num){
    int start=1,end=46340;
    int mid;
    while(start<=end){
        mid=(start+end)/2;
        long n=mid*mid;
        if(n>num)
        end=mid-1;
        else if(n<num)
        start=mid+1;
        else
        return 1;
    }
    return 0;
}
```

### 分析
 - 这道题理解起来并不难，但也不是想象中的那么简单。一开始的代码
 ```c
 bool isPerfectSquare(int num){
    int i=0;
    long long n;
    while(i<num){
        i=i+1;
        n=pow(i,2);
        if(n==num)
        return 1;
    }
    return 0;

}
```
   不出所料超时。后来也查询了解决办法，那就是在平方时强制转化为double型。选择这道题也是今天本选择了中档题的“超级丑数”，但发现对二分法理解的还不是
   很透彻，于是先选择这道简单的运用二分法的题练练手。
 - 对这道题的解释也很明确：令 x = (left + right) / 2 作为一个猜测，计算 guess_squared = x * x 与 num 做比较：
   如果 guess_squared == num，则 num 是一个完全平方数，返回 true。
   如果 guess_squared > num ，设置右边界 right = x-1。
   否则设置左边界为 left = x+1。
   还有一个图片以供很好理解：！[图片](https://github.com/Lihao-me/Photos/blob/master/untitled.png)
 - 终于对二分法有了深入的理解。这道题另一个需要领会的点在于溢出问题，在多次尝试都是溢出后参考了网友的解释：首先要知道一个前提，整型底数上
   限为46340 即  整数最大值为 2147483647 而其中最大的有效的完全平方数为 46340 *46340 = 2147395600。
 - 而就这道题本身来说也有很多不错的思路。
  
  牛顿迭代法
```c++
  class Solution {
public:
    bool isPerfectSquare(int num) {
        if(num < 2) return true;
        
        long x = num / 2;
        while(x * x > num){
            x = (x + num / x) / 2;
        }
        return (x * x == num);
    }
};

作者：speen123
```
  先筛选后暴力
```c
class Solution {
public:
    bool isPerfectSquare(int num) {
        if(num == 1) return true; 
        if(num % 10 == 2 || num % 10 == 3 || num % 10 == 7 || num % 10 == 8) return false;
        for(int i = 0; i < 46341; i++)
            if(i*i == num) return true;
        return false;
    }
};

作者：logan-31
```
公式法： 1+3+5+7+9+…+(2n-1)=n^2
```c++
lass Solution {
public:
    bool isPerfectSquare(int num) {
        int i=1;
        while(num>0)
        {
            num-=i;
            i+=2;
        }
        return num==0;
    }
};

作者：chenlele
