# 回文数
***
#### 2020.02.17

### 问题
>判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

### 示例
>输入: 121
输出: true

>输入: -121

输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

>输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

>进阶:
你能不将整数转为字符串来解决这个问题吗？

### 思路
>将该数的各数位从头尾向中间分别各位比较，若出现对应数位不同的数则不符合，否则符合。

### 代码
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0)
        return 0;
        if(x==0)
        return 1;
        int *a=new int[100];
        int i=0;
        while(x>=1){
            a[i]=x%10;
            x=x/10;
            i++;
        }
        double k=(i-1)/2;
        for(int j=0;j<=k;j++){
            if(a[j]!=a[i-1-j]){
            delete []a;
            return 0;}
        }
        delete []a;
        return 1;   
    }
};
```

### 分析
 - 其实看到进阶我对转化成字符的解决方案并没有什么头绪，首先想到的是用数组记录各个数位再用双指针的思想逐一比较。时间和内存上并不是很理想。28ms和14.5
   MB。
 - 而网友那里也大致是字符串、双指针和倒转三种方法。而C++中对于动态数组的申请我则发现用vector比我的new和delete更为有效。
 
方法一：转字符串
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        string tmp=to_string(x);
        string tmp2=tmp;
        reverse(tmp2.begin(),tmp2.end());
        if(tmp==tmp2) return true;
        return false;
        // for(int i=tmp.size()-1,j=0;i>0,j<i;i--,j++)
        //     if(tmp[i]!=tmp[j]) return false;
        // return true;
    }
};

作者：24shi-01fen-_00_01
```
方法二：利用vector比较首尾
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0) return false;
        if(x/10==0) return true;
        vector<int> tmp;
        while(x>0){
            tmp.emplace_back(x%10);
            x=x/10;
        }
        for(int i=tmp.size()-1,j=0;i>0,j<i;i--,j++)
            if(tmp[i]!=tmp[j]) return false;
        return true;
    }
};
作者：24shi-01fen-_00_01
```
方法三：将数倒转
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0) return false;
        if(x/10==0) return true;
        long long cur = 0;
        int num = x;
        while(num != 0) {
            cur = cur * 10 + num % 10;
            num /= 10;
        }
        return cur == x;   
    }
};

作者：24shi-01fen-_00_01
