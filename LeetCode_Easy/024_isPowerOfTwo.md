# 2的幂
***
#### 2020.02.16

### 问题
>给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

### 示例
>输入: 1
输出: true
解释: 20 = 1

>输入: 16
输出: true
解释: 24 = 16

>输入: 218
输出: false

### 思路
>将数一直除以2，若能除至1则为2的幂。

### 代码
```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n==1)
        return 1;
        else if(n<=0||n%2!=0)
        return 0;
        else{
            n=n/2;
            while(n!=1){
                if(n%2!=0){
                    return 0;
                break;}
                n=n/2;
            }
            return 1;
        }
       return 0; 
    }
};
```

### 分析
 - 看到这题首先想到的还是循环，没想到时间复杂度并不高没有超时。
 - 原来做过一个“3的幂”和这个差不多，但是它却没有2的幂这么特殊了，前者用到了循环迭代等方法，但后者却可以用网友所说的位运算。

首先可以观察一些2的幂的二进制。
例：
1 = 1;
2 = 10;
4 = 100;
8 = 1000;
...
是的，所有为2的幂的整数，其二进制均是除了其有效最高位以外全是0的数。也就是说，我们只需要判断这个数是否有除了有效最高位之外还有没有其他的1即可。
那么这里可以用到一个位运算小技巧：给定一个数n，将n和(n-1)做一次与运算，即可将n的最后一位1去掉：
例：
设 n = 9;
如图：！[图片](https://github.com/Lihao-me/Pictures/blob/master/isPowerOfTwo.png)
那么对于所有2的幂，我们将它与它-1后的数做一次与运算，就会将其唯一一位1消去，最后等于0。
由此可以写出一行return，解决问题。
```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && !(n & (n - 1));
    }
};

作者：kadgang
```
