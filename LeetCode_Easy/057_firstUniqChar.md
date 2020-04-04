# 第一个只出现一次的字符
***
#### 2020.04.04

### 问题
>在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。

### 示例
>s = "abaccdeff"              
返回 "b"             

>s = ""                  
返回 " "                   

### 限制
>0 <= s 的长度 <= 50000                     

### 代码
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        if(s=="")
        return ' ';
        int j=0;
        vector<int>res(128,0);
        for(int i=0;i<s.size();i++)
        {
            res[s[i]]++;
            while(res[s[j]]>1)
            j++;
        }
        if(j<s.size()) return s[j];
        return ' ';
    }
};
```

### 分析
 - 执行用时 :36 ms, 在所有 C++ 提交中击败了80.58%的用户内存消耗 :10.8 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题对我来说还是有点小
   难度的。也是参考了网友的解法，发现原来这么简单。
 - 设一个j进行保存就行了。而对于网友的bitset的解法，我也详细了解了一下：https://blog.csdn.net/qll125596718/article/details/6901935

map
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        if(s == "")
            return ' ';
        //map中是对应字符出现的次数
        unordered_map<char,int> cmap;
        for(auto i = 0;i < s.size();i++)
        {
            cmap[s[i]]++;
        }
        //按字符串的顺序在map中查找到第一个出现一次的字符
        for(auto i = 0;i < s.size();i++)
        {
            if(cmap[s[i]] == 1)
                return s[i];
        }
        //没有找到
        return ' ';
    }
};

作者：jian-yi-9
```

数组
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        
        //字符计数
        vector<int> v(128);
        for(int i = 0; i < s.size(); ++i)
        {
            ++v.at(s.at(i));
        }

        //遍历字符串 去计数数组中找次数为1的
        for(int i = 0; i < s.size(); ++i)
        {
            if(v.at(s.at(i)) == 1)
            {
                return s.at(i);
            }
        }

        return ' ';
    }
};

作者：eric-345
```

bitset
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        
        bitset<128> bs1, bs2;
        for(int i=0; i<s.size(); i++) {
            if (!bs1[s[i]] && !bs2[s[i]])
                bs1[s[i]] = 1;
            else if (bs1[s[i]] && !bs2[s[i]])
                bs2[s[i]] = 1;
        }
        for(auto item: s){
            if (bs1[item] && !bs2[item])
                return item;
        }
        return ' ';
    }
};

作者：yl-precious
