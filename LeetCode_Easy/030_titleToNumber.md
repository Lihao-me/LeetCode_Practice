# Excel表列序号
***
#### 2020.02.22

### 问题
>给定一个Excel表格中的列名称，返回其相应的列序号。
例如，
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...

### 示例
>输入: "A"
输出: 1

>输入: "AB"
输出: 28

>输入: "ZY"
输出: 701

### 代码
```c++
class Solution {
public:
    int titleToNumber(string s) {
        int n=0;
        for(auto e:s){
            n=n*26+(e-'A'+1);
        }
        return n;

    }
};
```

### 分析
 - 题目本身还是很简单的，但是思考了好久。一直在找序列的规律并利用ASCII将其转化。所以把它当作的一个数学规律的问题。
 - 事实上题目本身就是一个进制转化的问题，将26进制转化为10进制。时间4ms，击败79.20%；内存8.3MB，击败85.22%。题解中一位网友提供了创建getNum函数的
   办法，虽然这位网友的解法在时间上和内存上并不是十分理想，但是其代码很清晰地表达出序列的本质规律。
   

观察规律，给的数都是大写字母，A对应1，因此写个getNum()函数处理单个字母变成数字的问题
一连串的字母呢，发现AA就是26* 1+1=27，ZY就是26 *  26+25=701，每一位都是本身数字乘上26的幂次方
```c++
 int getNum(char c)
    {
        return 1+c-'A';
    }

    int titleToNumber(string s) {
        int ret=0;
        int len=s.size();
        for(int i=len-1;i>=0;--i)
        {
            ret+=getNum(s[i])*pow(26,len-1-i);
        }
        return ret;
    }

作者：d__wade
