# 4的幂
***
#### 2020.05.20

### 问题
>给定一个整数 (32 位有符号整数)，请编写一个函数来判断它是否是 4 的幂次方。     

### 示例
>输入: 16                                   
输出: true                        

>输入: 5                            
输出: false           

### 进阶
>你能不使用循环或者递归来完成本题吗？         

### 代码
```c++
class Solution {
public:
    bool isPowerOfFour(int num) {
        if(num==0)
        return 0;
        if(num==4||num==1)
        return 1;
        if(num%4==0)
        return isPowerOfFour(num/4);
        else
        return 0;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :5.9 MB, 在所有 C++ 提交中击败了100.00%的用户。觉得这种类型的题目用递归是
   最简单的了。看到这个进阶确实有点束手无策。
   
### 优解
#### 1.位运算
```c++
class Solution {
public:
    bool isPowerOfFour(int num) {
        if(num < 0) return false;
        int t = num & (-num);
        if(num - t != 0) return false;
        return num % 3 == 1;
    }
};

作者：wangdh15
```

#### 2.开方
```c++
class Solution {
public:
    bool isPowerOfFour(int num) {
        if (num <= 0) return false;
        int t = sqrt(num);
        return t * t == num && ((1 << 30) % num) == 0;
    }
};

作者：HLHD_Steven
```

#### 3.Lowbit
```c++
class Solution {
public:
    bool isPowerOfFour(int num) {
        if(num < 0) return false;
        int t = num & (-num);
        if(num - t) return false;
        return num % 3 == 1;
    }
};

作者：HLHD_Steven
```
