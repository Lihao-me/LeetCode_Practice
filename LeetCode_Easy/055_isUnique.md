# 判定字符是否唯一
***
#### 2020.04.02

### 问题
>实现一个算法，确定一个字符串 s 的所有字符是否全都不同。

### 示例
>输入: s = "leetcode"           
输出: false                

>输入: s = "abc"               
输出: true                  

### 限制
>	1.0 <= len(s) <= 100 
2.如果你不使用额外的数据结构，会很加分。

### 思路
>记录每一个字母的出现次数，如果出现大于1的，则说明不符合。

### 代码
```c++
class Solution {
public:
    bool isUnique(string astr) {
        int n=astr.size();
        int *word=new int[27]();
        for(int i=0;i<n;i++){
            int m=astr[i]-'a';
            word[m]++;
            if(word[m]>1)
            return 0;

        }
        return 1;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.2 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题比较简单，思路也比较
   好想吧。
 - 也有其他的算法，比如位运算。这个方法也是我一直很难想到的。
 
```c++
class Solution {
public:
    bool isUnique(string astr) {
        int bit = 0;
        for (char c: astr) {
            int index = c - 'a';
            int newBit = 1<<index;
            if ((bit & newBit) == newBit) {
                return false;
            }
            bit |= newBit;
        }
        return true;
    }
};

作者：bmyxb
