# 整数反转
***
#### 2020.02.26

### 问题
>给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

### 示例
>输入: 123
输出: 321

>输入: -123
输出: -321

>输入: 120
输出: 21

>注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

### 思路
>从个位开始数起，将每一位至于前一位之后并乘以从翻转开始接下来的翻转次数的10。

### 代码
```c++
class Solution {
public:
    int reverse(int x) {
        int result=0;
        int n=x;
        while(x!=0){
            if(result>INT_MAX/10) return 0;
            if(result<INT_MIN/10) return 0;
            result=result*10;
            result=result+x%10;
            x=x/10;
        }
        return result;

    }
};
```

### 分析
 - 虽然最终的代码非常的简单，但也是我做了很多次尝试和参考网友的建议后最终形成的。对十进制数的数位做的文章的题已经做过很多了。例如提取各数位，各数位
   相加，3的个数等等。所以我初看这道题考虑的是常规的分离思路递归，但是这道题的限制却很多。
 - 第一应该注意的是这是一个有符号的整数，也即结果的符号是与原整数相同的；第二，当0移至第一位时则0消失；第三，整数反转后不能溢出。单是针对前两条我就
   通过分支讨论写了半天十分麻烦与冗杂。最后不得不参考网友的题解，发现一个循环的数学办法即可轻松解决前两个问题。而对于整数的溢出问题原来也接触过，怎样
   简便地讨论也是我想了解地。也因此我第一次了解到INT_MAX和INT_MIN的使用。INT_MAX,INT_MIN由标准头文件<limits.h>定义。
   INT_MAX=2^31-1(2,147,483,647),
   INT_MIN=-2^31(-2,147,483,648)；
   详见：https://blog.csdn.net/u010325193/article/details/80287777
 - 用时8ms,击败33.92%Cpp提交用户；内存8.3MB，击败12.59%。可以说时空上并不是十分理想。而我看到一个C的题解没有 用到<limits.h>也处理的很好。
 
```c
#define isOverLength 0

int reverse(int x){
    long lRet = 0;
    while(0 != x)
    {
        lRet = lRet * 10 + x % 10;
        x = x / 10;
    }

    if((int)lRet != lRet)
    {
        return isOverLength;
    }

    return (int)lRet;
}

作者：hua-jia-wang-tie-nan
