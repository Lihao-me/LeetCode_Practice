# 判定是否为字符重排
***
#### 2020.04.18

### 问题
>给定两个字符串 s1 和 s2，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。

### 示例
>输入: s1 = "abc", s2 = "bca"                
输出: true                     

>输入: s1 = "abc", s2 = "bad"                   
输出: false                    

### 说明
1.0 <= len(s1) <= 100 
2.0 <= len(s2) <= 100 

### 代码
```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        if(s1.size()!=s2.size())
        return 0;
        int sum1=0,sum2=0;
        for(int i=0;i<s1.size();i++)
        sum1 += s1[i];

        for(int j=0;j<s2.size();j++)
        sum2 += s2[j];

        if(sum1==sum2)
        return 1;

        return 0;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.1 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题我采用的思是通过各字符串
   ASCII码相加，判断两值是否相等来做的。本来以为通过不了，结果通过了。感觉应该是测试样例的缺陷，我也向平台提供了一个反馈。过一段时间应该可以知道答案。
 - 这道题还是用常规的思路比较保险。
 
位运算
```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        int s1bin = 0;
        for(char c : s1) {
            int index = c - 'a';//index向左移动一位--若字母是a a-'a'=0不能表示a出现过于是向左移动一位
                                //此时a->00..001 b->00..010 c->00..100 .........
            int newBit = 1<<index;
            s1bin |= newBit;  
        }
        int s2bin = 0;
        for(char c : s2) {
            int index = c - 'a';
            int newBit = 1<<index;
            s2bin |= newBit;  
        }
        return s1bin == s2bin;
    }
};

作者：ceng-jing-14
```

异或运算
```c++
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        if (s1 == null || s2 == null) {
            return false;
        }
        int length = s1.length();
        if (length != s2.length()) {
            return false;
        }
        int result = 0;
        char[] s1Char = s1.toCharArray();
        char[] s2Char = s2.toCharArray();
        for (int i = 0; i < s1Char.length; i++) {
            result = result ^ s1Char[i] ^ s2Char[i];
        }
        return result == 0;
    }
}

作者：peng-187
```

排序
```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        sort(s1.begin(),s1.end());
        sort(s2.begin(),s2.end());
        return s1==s2;
    }
};

作者：kkiyeer
```

哈希
```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        unordered_map<char,int>mp;
        for(auto i : s1){
            mp[i]++;
        }
        for(auto i : s2){
            mp[i]--;
            if(mp[i]<0)return false;
        }
        return true;
    }
};

作者：mei-you-ni-de-liu-yue-tian
```

map
```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        map<char, int> mp;
        for (auto s : s1) mp[s]++;
        for (auto s : s2) {
            if (!mp[s]) return false;
            mp[s]--;
        }
        for (auto t : mp) if (t.second) return false;
        return true;
    }
};

作者：taoyu
