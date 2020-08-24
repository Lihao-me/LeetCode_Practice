# 同构字符串
***
#### 2020.08.24

### 问题
>给定两个字符串 s 和 t，判断它们是否是同构的。             
如果 s 中的字符可以被替换得到 t ，那么这两个字符串是同构的。                       
所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身。                                              
               
### 示例
>输入: s = "egg", t = "add"                      
输出: true                    

>输入: s = "foo", t = "bar"                        
输出: false                       

>输入: s = "paper", t = "title"                 
输出: true                              

### 说明
>你可以假设 s 和 t 具有相同的长度。        

### 代码
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        if(s.size()!=t.size())
        return 0;
        for(int i=0;i<s.size();i++)
        {
            if(s.find(s[i])!=t.find(t[i]))
            return 0;
        }
        return 1;
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了99.40%的用户内存消耗：6.9 MB, 在所有 C++ 提交中击败了89.63%的用户。
 - 通过比较相同位置的字符是否相等从而判断。find函数的使用非常巧妙。
 
### 优解
#### 1.0ms解法
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
      int sdict[256] = {0}, tdict[256] = {0};
      for (int i = 0; i < s.size(); ++i) {
        if (sdict[s[i]] == 0) sdict[s[i]] = i + 1;  // 说明首次出现
        if (tdict[t[i]] == 0) tdict[t[i]] = i + 1; 
        if (sdict[s[i]] != tdict[t[i]]) return false;
      }
      return true;
    }
};
```

#### 2.哈希映射
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char,char> smap;
        unordered_map<char,char> tmap;
        for(int i = 0; s[i] != '\0'; ++ i){
            char ss = s[i];
            char tt = t[i];
            if(smap.count(ss)){
                if(smap[ss] != tt)    return false;
            }
            else if(tmap.count(tt)){
                if(tmap[tt] != ss)  return false;
            }
            else{
                smap[ss] = tt;
                tmap[tt] = ss;
            }
        }
        return true;
    }
};

作者：youlookdeliciousc
链接：https://leetcode-cn.com/problems/isomorphic-strings/solution/cxiang-xi-ti-jie-shuang-jie-by-youlookdeliciousc/
```
