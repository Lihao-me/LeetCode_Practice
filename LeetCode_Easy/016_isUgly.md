# 丑数Ⅰ
***
#### 2020.02.07

### 问题
>编写一个程序判断给定的数是否为丑数。
丑数就是只包含质因数 2, 3, 5 的正整数。

### 示例
>输入: 6
输出: true
解释: 6 = 2 × 3

>输入: 8
输出: true
解释: 8 = 2 × 2 × 2

>输入: 14
输出: false 
解释: 14 不是丑数，因为它包含了另外一个质因数 7。

>说明：
1.1 是丑数。
2.输入不会超过 32 位有符号整数的范围: [−231,  231 − 1]。

### 思路
>负数一定不是丑数，因为其因数一定至少有一个含负数；判断2、3、5是否为其因数，是则除以相对应的因数继续判断直至结果为1则为丑数，否则不是丑数。

### 代码
```c
bool isUgly(int num){
    if(num<=0)
    return 0;
    else{
        while(1){
            if(num%2==0){
                num=num/2;
                continue;
            }
            else if(num%3==0){
                num=num/3;
                continue;
            }
            else if(num%5==0){
                num=num/5;
                continue;
            }
            else if(num==1)
            return 1;
            else
            return 0;
        }
    }
}
```

### 分析
 - 本来今天准备做一道中等难度的题，看上了“丑数Ⅱ”，那这道丑数Ⅰ权当是做一个铺垫吧。思路非常简单也比较好理解。
 - 网上也大体是我的思路，另一种就是递归了，其实质都差不多。我的解法时间上大败100%，还是很高兴的，在题解中也看到了更为简洁的代码。就贴上递归解法以作补
   充。
 
 ```c
 class Solution {
public:
    bool isUgly(int num) {
        if(num==0) return false;
        if(num==1) return true;
        if(num%2==0) return isUgly(num/2);
        if(num%3==0) return isUgly(num/3);
        if(num%5==0) return isUgly(num/5);
        return false;
    }
};

作者：chenlele
