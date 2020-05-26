# 平方数之和
***
#### 2020.05.26

### 问题
>给定一个非负整数 c ，你要判断是否存在两个整数 a 和 b，使得 a2 + b2 = c。

### 示例
>输入: 5       
输出: True                  
解释: 1 * 1 + 2 * 2 = 5                  

>输入: 3                
输出: False               

### 代码
```c++
class Solution {
public:
    bool judgeSquareSum(int c) {
        long l=0,r=sqrt(c);
        while(l<=r)
        {
            if(l*l+r*r==c)
            return 1;

            if(l*l+r*r>c)
            r--;
            else if(l*l+r*r<c)
            l++;

        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :5.8 MB, 在所有 C++ 提交中击败了100.00%的用户。我运用的是双指针的思路，
   可以看出来效果还不错。值得注意的是用long类型，否则会溢出。
 - 貌似除了双指针没啥别的办法，所以本题也没有所谓的更优的解了。
 
 ### 优解
 #### 二分法
 ```c++
 class Solution {
public:
    bool judgeSquareSum(int c) {
        long n=sqrt((double)c);
        long  i=0,j=n,sum;
        while(i<=j){
            sum=i*i+j*j;
            if(sum==c) return true;
            else if(sum>c) j--;
            else i++;
        }
        return false;
    }
};

作者：gao-yi-meng
