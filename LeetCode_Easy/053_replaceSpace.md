# 替换空格
***
#### 2020.03.30

### 问题
>请实现一个函数，把字符串 s 中的每个空格替换成"%20"。            

### 示例
>输入：s = "We are happy."                
输出："We%20are%20happy."                 

### 限制
>0 <= s 的长度 <= 10000                 

### 代码
>遍历整个字符串，当遇到空格就把空格替换为指定的字符串。

### 代码
```c++
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]==' ')
            {
                res += "%20";
                continue;
            }
            res.push_back(s[i]);
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.2 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题的难点在遇到空格时不能
   直接插入字符串，所以需要重新定义一个字符串。但总体来说还是很简单的。
 - 看其他人的题解的时候发现有使用append()函数的，所以也学习了一下这个函数。目前已知的可以将字符串拼接的方法有strcat()函数、直接+,那么append()
   则是又一种方法。 https://blog.csdn.net/wxn704414736/article/details/78551886              
   也有写的比较好的其他代码可以参考。
   
```
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for(auto c : s){
            if(c == ' ')
                res += "%20";
            else
                res += c;
        }
        return res;
    }
};

作者：wangxiaole
