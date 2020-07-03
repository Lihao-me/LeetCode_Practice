# 最长公共前缀
***
#### 2020.07.03

### 问题
>编写一个函数来查找字符串数组中的最长公共前缀。                                
如果不存在公共前缀，返回空字符串 ""。                      

### 示例 
>输入: ["flower","flow","flight"]                
输出: "fl"                                                         
                                       
>输入: ["dog","racecar","car"]                         
输出: ""                   
解释: 输入不存在公共前缀。

### 说明
>所有输入只包含小写字母 a-z 。                                                           

### 代码
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
       string res= strs.empty()==1?"":strs[0];
       for(auto s:strs)
       {
           while(s.find(res)!=0)
           {
               res=res.substr(0,res.size()-1);
           }
       }
       return res;
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了91.60% 的用户内存消耗：7.1 MB, 在所有 C++ 提交中击败了100.00% 的用户。
 - 本题也是参考了网友的思路，觉得非常好。当看到题目，我就意识到使用find()和sunstr()函数可能会更简单一些，但是如何巧妙运用是一个问题。而本题的巧妙之处在于将每个字符串依次判断，运行到最后最简单的前缀
   也就出现了。两个循环运用的很巧妙。而官方的题解则似乎更为优化。这样就显得我们的解法有些取巧。
   
### 优解
#### 1.横向扫描
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        string prefix = strs[0];
        int count = strs.size();
        for (int i = 1; i < count; ++i) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (!prefix.size()) {
                break;
            }
        }
        return prefix;
    }

    string longestCommonPrefix(const string& str1, const string& str2) {
        int length = min(str1.size(), str2.size());
        int index = 0;
        while (index < length && str1[index] == str2[index]) {
            ++index;
        }
        return str1.substr(0, index);
    }
};


作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/
```

#### 2.纵向扫描
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        int length = strs[0].size();
        int count = strs.size();
        for (int i = 0; i < length; ++i) {
            char c = strs[0][i];
            for (int j = 1; j < count; ++j) {
                if (i == strs[j].size() || strs[j][i] != c) {
                    return strs[0].substr(0, i);
                }
            }
        }
        return strs[0];
    }
};


作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/
```
#### 3.分治
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        else {
            return longestCommonPrefix(strs, 0, strs.size() - 1);
        }
    }

    string longestCommonPrefix(const vector<string>& strs, int start, int end) {
        if (start == end) {
            return strs[start];
        }
        else {
            int mid = (start + end) / 2;
            string lcpLeft = longestCommonPrefix(strs, start, mid);
            string lcpRight = longestCommonPrefix(strs, mid + 1, end);
            return commonPrefix(lcpLeft, lcpRight);
        }
    }

    string commonPrefix(const string& lcpLeft, const string& lcpRight) {
        int minLength = min(lcpLeft.size(), lcpRight.size());
        for (int i = 0; i < minLength; ++i) {
            if (lcpLeft[i] != lcpRight[i]) {
                return lcpLeft.substr(0, i);
            }
        }
        return lcpLeft.substr(0, minLength);
    }
};


作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/
```

#### 4.二分查找

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        int minLength = min_element(strs.begin(), strs.end(), [](const string& s, const string& t) {return s.size() < t.size();})->size();
        int low = 0, high = minLength;
        while (low < high) {
            int mid = (high - low + 1) / 2 + low;
            if (isCommonPrefix(strs, mid)) {
                low = mid;
            }
            else {
                high = mid - 1;
            }
        }
        return strs[0].substr(0, low);
    }

    bool isCommonPrefix(const vector<string>& strs, int length) {
        string str0 = strs[0].substr(0, length);
        int count = strs.size();
        for (int i = 1; i < count; ++i) {
            string str = strs[i];
            for (int j = 0; j < length; ++j) {
                if (str0[j] != str[j]) {
                    return false;
                }
            }
        }
        return true;
    }
};


作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution
```

#### 5.寻找字典序最大最小字符串
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";
        // c++17 结构化绑定
        // str0, str1 分别是一个 pair<string, string> 的 first 和 second
        const auto [str0, str1] = minmax_element(strs.begin(), strs.end());
        for(int i = 0; i < str0->size(); ++i)
            if(str0->at(i) != str1->at(i)) return str0->substr(0, i);
        return *str0;
    }
};


作者：you-yuan-de-cang-qiong
链接：https://leetcode-cn.com/problems/longest-common-prefix/solution/zi-dian-xu-zui-da-he-zui-xiao-zi-fu-chuan-de-gong-/
```
