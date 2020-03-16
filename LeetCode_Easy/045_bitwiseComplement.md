# 十进制整数的反码
***
#### 2020.03.16

### 问题
>每个非负整数 N 都有其二进制表示。例如， 5 可以被表示为二进制 "101"，11 可以用二进制 "1011" 表示，
依此类推。注意，除 N = 0 外，任何二进制表示中都不含前导零。
二进制的反码表示是将每个 1 改为 0 且每个 0 变为 1。例如，二进制数 "101" 的二进制反码为 "010"。
给你一个十进制数 N，请你返回其二进制表示的反码所对应的十进制整数。

### 示例
>输入：5    
输出：2     
解释：5 的二进制表示为 "101"，其二进制反码为 "010"，也就是十进制中的 2 。     

>输入：7      
输出：0       
解释：7 的二进制表示为 "111"，其二进制反码为 "000"，也就是十进制中的 0 。     

>输入：10     
输出：5      
解释：10 的二进制表示为 "1010"，其二进制反码为 "0101"，也就是十进制中的 5 。     

### 提示
>0 <= N < 10^9  

### 代码
```c++
class Solution {
public:
    int bitwiseComplement(int N) {
        if(N==0)
        return 1;
        int temp=N,res=1;
        while(temp>0){
            temp=temp>>1;
            res=res<<1;
        }
        return N^(res-1);
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了61.39%的用户内存消耗 :7.6 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题是关于位运算，详见：
   https://blog.csdn.net/a1351937368/article/details/77746574 。
 - 还可以一位一位异或运算，也可以高位运算，还有
 
 一位一位算
 ```c++
 class Solution {
public:
    int bitwiseComplement(int N)
    {
        int one = 1;
        int bit = bitCount(N);
        for (int i = 0; i < bit; i++)
        {
            N ^= one;
            one <<= 1;
        }
        return N;
    }
    int bitCount(int n)
    {
        if (n == 0) return 1;
        int cnt = 0;
        while (n)
        {
            cnt++;
            n >>= 1;
        }
        return cnt;
    }
};

作者：int-myheart
```

高位差值法
```c++
class Solution {
public:
    int bitwiseComplement(int N) {
        
        int temp = 2;
        
        while(temp<=N){
            
            temp = temp << 1;
        }
        
        return temp - N - 1;
        
    }
};

作者：keyway1984

暴力法
```c
int bitwiseComplement(int N){
    if(N == 0)
        return 1;
    int temp = N;
    int j = 0;
    int sum = 0;
    int record;
    while(temp != 0)
    {
        record = temp % 2;
        if(record == 0)
        {   
            sum = sum + pow(2,j);
        }
        j++;
        temp = temp/2;       
    }
    return sum;
}

作者：a-er-ding-de-shen-deng
```

```c
int bitwiseComplement(int N){
    char bit[64];
    int i = 0;
    if(!N) {
        return 1;
    }
    
    while(N) {
        bit[i++] = N % 2;
        N /= 2;
    }

    int left,right,tmp;
    left = 0;
    right = i - 1;
    while(left < right) {
        tmp = bit[left];
        bit[left] = bit[right];
        bit[right] = tmp;
        left++;
        right--;
    }

    int res = 0;
    for (left = 0; left < i; left++) {
        if (bit[left] == 0) {
            res = res * 2 + 1;
        }
        else {
            res = res * 2;
        }
    }

    return res;
}

作者：cuizhigang
