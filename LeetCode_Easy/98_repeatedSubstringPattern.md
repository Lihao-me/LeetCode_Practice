# 重复的子字符串
***
#### 2020.07.02

### 问题
>给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

### 示例
>输入: "abab"                     
输出: True                                
解释: 可由子字符串 "ab" 重复两次构成。                              

>输入: "aba"                   
输出: False                                    

>输入: "abcabcabcabc"                   
输出: True                             
解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)       

### 代码
```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string str;
        str=s+s;
        return str.find(s,1)!=s.size();
    }
};
```

### 分析
 - 执行用时：24 ms, 在所有 C++ 提交中击败了83.45%的用户内存消耗：12.1 MB, 在所有 C++ 提交中击败了80.00%的用户。
 - 这道题的思路也参考了网友的解法。主要是使用find函数，假设我们所给的字符串有n个子字符串且符合要求，那么我们将这个字符串与它本身连接，那么连接后就有了2n个子字符串，如果我们找到的子字符
   串个数等于原字符串的长度那么就符合要求。想法非常的巧妙和简单。
 - 然而除了这种简单的方法，题解中也提供了一般的思路解法。而其中有一种KMP算法还听巧妙但是也有点难理解，具体的分析可以见:https://blog.csdn.net/dark_cy/article/details/88698736
   
### 优解
#### 1.KMP算法
```c++
class Solution {
public:
    //运用公式 x.size() = length - next[length]; x为最小周期子串，这里只能用最基本的KMP算法，不能用改进的
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        vector<int> next(n+1,-1);
        int i = 0;
        int j = next[0];
        while (i < n){
            if (j == -1 || s[i] == s[j]){
                i++;
                j++;
                next[i] = j;
            }
            else
                j = next[j];
        }
        int x = n - next[n];
        if (n % x == 0 && x != n)
            return true;
        return false;

    }
};

作者：ch080139
```

#### 2.指针搜索
```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int size = s.size();
        int index = 1;
        while(index < size/2 + 1){
            if(s[index] == s[0]){
                int templow = 0;
                int temphigh = index;
                int count = 0;
                while(temphigh < size){
                    if(s[templow] == s[temphigh]){
                        temphigh++;
                        templow++;
                        count++;
                    }
                    else
                        break;
                }
                if(temphigh == size && count%index == 0)
                    return true;
            }
            index++;
        }
        return false;
    }
};

作者：xing-chen-da-hai-36
```
