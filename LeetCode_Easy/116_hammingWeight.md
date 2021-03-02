# 位1的个数
***
#### 2020.03.02

### 问题
>编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。

### 提示
>请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。

### 进阶
>如果多次调用这个函数，你将如何优化你的算法？

### 示例
>输入：00000000000000000000000000001011        
输出：3                         
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。                          

>输入：00000000000000000000000010000000                     
输出：1                   
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。                      

>输入：11111111111111111111111111111101                  
输出：31                           
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。      

### 提示
>输入必须是长度为 32 的 二进制串 。   

### 代码
```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int num=0;
        while(n!=0)
        {
            if(n&1) num++;
            n >>= 1;
        }
        return num;
        
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了39.59%的用户；内存消耗：5.9 MB, 在所有 C++ 提交中击败了56.40%的用户。
 - 时隔一年，又重新回来刷题，慢慢来吧。在学习了数字电子技术基础后对二进制有了更多的了解，也希望接下来的数据结构能学好。

### 优解
#### 1.0ms范例
```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        uint32_t a = 1;
        for(int i = 0;i < 32;++i)
        {
            if((a & n) != 0)
                ++res;
            
            a <<= 1;        
        }

        return res;
    }
};
```

#### 2.最少内存消耗(5780kb)
```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int a=0,b;
        if(n==0)return 0;
        while(n>1){
            if(n%2!=0){
                a++;
            }
            n/=2;
        }
        return a+1;
    }
};
```

#### 3.巧妙解法
利用 n & (n - 1) 来消除n最右边的1，然后如果n还是不等于0的话，让count++，同时继续消除n最右边的1。
这种方法比上一种更快，因为这种方法是n有多少个1，就遍历多少次，而上一种是要从头遍历到尾，每一个元素都要判断一遍。
```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while(n != 0)
        {
            count ++;
            n = n & (n - 1);
        }
        return count;
    }
};

作者：superkakayong
链接：https://leetcode-cn.com/problems/number-of-1-bits/solution/zi-jie-ti-ku-191-jian-dan-wei-1de-ge-shu-1shua-by-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 4.取巧解法
```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        return ((bitset<32>)n).count();
    }
};

作者：superkakayong
链接：https://leetcode-cn.com/problems/number-of-1-bits/solution/zi-jie-ti-ku-191-jian-dan-wei-1de-ge-shu-1shua-by-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
