# 左旋转字符串
***
#### 2020.04.03

### 问题
>字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。
比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

### 示例
>输入: s = "abcdefg", k = 2               
输出: "cdefgab"             

>输入: s = "lrloseumgh", k = 6                
输出: "umghlrlose"                      

### 限制
>1 <= k < s.length <= 10000

### 思路
>先将两个相同的字符串拼接，然后从第k位提取相同大小的字符串。

### 代码
```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        if(n==0)
        return s;
        int size=s.size();
        string cmp=s+s;
        string res=cmp.substr(n,size);
        return res;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :7.8 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题比较简单。看到的时候
   首先想到了原来做过的“判断反转字符串”一题。
   https://github.com/Lihao-me/LeetCode_Practice/blob/master/LeetCode_Easy/044_isFlipedString.md
   那一道题可能难度还更大一些。
 - 但是学习就是不断复习巩固。初做这道题的时候还是把substr的函数参数用错，以为是从第n位到第n+size位。正好复习一下。
   https://blog.csdn.net/weixin_39645344/article/details/79354662
   
数学公式法
```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string S = "";
        for(int i = 0; i<s.length(); i++)
        {
            int x = (i + n) % s.length();
            S += s[x];
        }
        return S;
    }
};

作者：zsj123
```

原位操作, 内存占用比较少,但耗时长
```c++
string reverseLeftWords(string s, int n) {
    for(int i = 0; i < n; ++i){
        s.push_back( *s.begin( ) );
        s.erase( s.begin( ) );
    }
    return s;
}

作者：NiceBlueChai
```

构造一个新的字符串, 用时短, 但消耗内存多
```c++
string reverseLeftWords(string s, int n) {
    string ret( s.cbegin( ) + n, s.cend( ) );
    ret += string( s.cbegin( ), s.cbegin( ) + n );
    return ret;
 }

作者：NiceBlueChai
