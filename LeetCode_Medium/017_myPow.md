# Pow(x,n)
***
#### 2020.07.23

### 问题
>实现 pow(x, n) ，即计算 x 的 n 次幂函数。              

### 示例
>输入: 2.00000, 10                    
输出: 1024.00000                    

>输入: 2.10000, 3     
输出: 9.26100            

>输入: 2.00000, -2
输出: 0.25000               
解释: 2-2 = 1/22 = 1/4 = 0.25                     

### 说明
>	-100.0 < x < 100.0                              
	n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。          
  
### 代码
```cpp
//超时
class Solution {
public:
    double pow(double x,double y,int N)
    {
        if(N==1)
        return y;
        else
        return pow(x,x*y,N-1);
    }
    double myPow(double x, int n) {
        if(n==0||x==1)
        return 1;
        else if(n>0)
        {
            return pow(x,x,n);
        }
        else if(n<0)
        return 1/pow(x,x,-n);

        return x;
    }
};
```

```cpp
//通过
class Solution {
public:
    double myPow(double x, long long n) {
        if(n==0||x==1)
        return 1;
        if(n<0)
        return 1/myPow(x,-n);
        else{
        double t=myPow(x,n/2);
        if(n%2==0)
        {
            
            return t*t;
        }
        else
        return x*t*t;
        }

    
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了40.61%的用户内存消耗：6 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 第一个答案主要是想用递归进行解决，但是到倒数第二个用例的时候超出时间限制，所以在题解中看到有人使用分治+递归从而进一步减少时间消耗。
 
### 优解
#### 1.map+递归(0ms范例)
```cpp
class Solution {
public:
    unordered_map<int,double> tmp;
    double func(double x,int n,double result)//
    {
        //double result;
        if(n==0)
        {
            return result;
        }
        if(abs(result)<0.00000001)
                return 0;
        return func(x,n-1,x*result);
    }
    double myPow(double x, int n) {
        if(n==0)
        {
            return 1;
        }
        /*if(n<0)
        {
            return func(1/x,abs(n),1);
        }
        else
        {
            return func(x,n,1);
        }*/
        /*if(x==-1)
        {
            if(n%2==0)
            {
                return 1;
            }
            else{
                return -1;
            }
        }
        if(x==1)
        {
            return 1;
        }
        int flag=0;
        if(n<0)
        {
            x=1/x;
            n++;
            n=-n;
            flag=1;
            //n=abs(n);
        }*/
        /*double result=1;
        //double result=1;
        while(n)//循环迭代
        {
            if(abs(result)<0.00000001)
                return 0;
            result*=x;
            --n;
        }
        return result*(flag==1?x:1);*/
        //return func(x,n,1)*(flag==1?x:1);
        return pow(x,n);
    } 
};
```

#### 2.快速幂+递归(官方题解)
```cpp
class Solution {
public:
    double quickMul(double x, long long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/powx-n/solution/powx-n-by-leetcode-solution/
```

#### 3.快速幂+迭代(官方题解)
```cpp
class Solution {
public:
    double quickMul(double x, long long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/powx-n/solution/powx-n-by-leetcode-solution/
```

#### 4.二进制转换
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        //0和-1的处理
        if(n == 0) return 1;
        if(x == 0) return n>0? 0:1/x;
        if(x==1)
            return x;
        if(x==-1) 
            return n % 2 == 0? 1: -1;
        //标志正负数, 用于后面返回时决定要不要用1去除sum
        bool flat = n > 0 ? 0 : 1; 
        if(n < 0 && n != -2147483648) {
		    n = -n;
	    }
        double sum = 1;
        //一边取二进制位, 一边累乘
        while(n != 0) {
            //特殊处理-2147483648
            if(n == -2147483648) {
                sum*=x;//先乘一次, 然后降幂
                n++;//降幂, 避免溢出
                n = -n;
            }
            if(n % 2 == 1) {
                sum *= x;
            }
            x *= x;
            n = n / 2;
        }
        return !flat?sum: 1/sum;
    }
};

作者：huangHT
链接：https://leetcode-cn.com/problems/powx-n/solution/cji-bai-shuang-bai-zhuan-hua-er-jin-zhi-an-quan-zh/
```
