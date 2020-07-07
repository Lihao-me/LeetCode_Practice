# 赎金信
***
#### 2020.07.07

### 问题
>给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。    
(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。)                                     

### 注意
>你可以假设两个字符串均只含有小写字母。                                 
canConstruct("a", "b") -> false             
canConstruct("aa", "ab") -> false                
canConstruct("aa", "aab") -> true                   

### 代码
```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        if(ransomNote.size()>magazine.size())
        return 0;
        int hash[26];
        memset(hash,0,26*sizeof(int));
        for(auto c:magazine)
        {
            hash[c-'a']++;
        }
        for(auto c:ransomNote)
        {
            if(hash[c-'a']<1)
            return 0;

            hash[c-'a']--;
        }
        return 1;
    }
};
```

### 分析
 - 执行用时：44 ms, 在所有 C++ 提交中击败了46.10%的用户内存消耗：8.6 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题我采用的是哈希表的思想，感觉这种思路是最好想到的。因为这道题的本质在于每一个“赎金信”的字母能否在字典中找到相同个数的。所以哈希表的方法是自然而然想到的。但是从时间消耗上
   来看并不是最好的思路。尽管其时间复杂度只有O(n)。
 - 查看了用时最少的4ms解法的static部分，我表示以我现在的C++水平还不是很明白，这个解法还需要多研究一下。试着搜索了一下，发现和C++11的新特性有关，许多竞赛者通过这一段代码来提高cin、
   cout的速度，因为二者本身的效率是非常低的。而下半部分主函数的思路就和我差不多了。而对于这段代码的详细解析可见博客：https://blog.csdn.net/weixin_42658928/article/details/82974658 。
 - 而大部分题解都是我的这种解法了，而所谓的字典序以及map解法我认为其实质上和我的这种思路是大同小异的。
   
### 优解
#### 1.4ms解法(含提速代码)
```c++
static const auto io_speed_up = []()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    return 0;
}();

class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        if(ransomNote.size()>magazine.size())
            return false;
        int mag[26] = {0};
        for(int i=0;i<magazine.size();i++)
        {
            mag[magazine[i]-'a']++;
        }
        for(int i=0;i<ransomNote.size();i++)
        {
            if(mag[ransomNote[i]-'a']==0)
                return false;
            else
                mag[ransomNote[i]-'a']--;
        }
        return true;
    }
};
```

#### 2.字典序
```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) 
    {
        int res=0;
        map<char,int>frequency;
        for(int i=0;i<magazine.size();i++)
        {
            frequency[magazine[i]]++;
        }

        for(int j=0;j<ransomNote.size();j++)
        {
            if(frequency[ransomNote[j]]>0)
            {
                res++;
                frequency[ransomNote[j]]--;
            }
        }

        if(res==ransomNote.size())  return true;
        return false;
    }
};

作者：yun-shen-zibu-zhi-chu
链接：https://leetcode-cn.com/problems/ransom-note/solution/383-shu-jin-xin-by-yun-shen-zibu-zhi-chu/
```

#### 3.map
```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int> m;
        for(auto s:magazine) m[s]++;
        for(auto s1:ransomNote)
            if(m[s1]--==0) return false;
        return true;

    }
};

作者：zsx3
链接：https://leetcode-cn.com/problems/ransom-note/solution/c-ha-xi-map-by-zsx3/
```
