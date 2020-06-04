# 反转字符串中的元音字母
***
#### 2020.06.04

### 问题 
>编写一个函数，以字符串作为输入，反转该字符串中的元音字母 。                        

### 示例
>输入: "hello"                              
输出: "holle"                         

>输入: "leetcode"                       
输出: "leotcede"                                     

### 说明
>元音字母不包含字母"y"。
                     
### 代码
```c++
class Solution {
public:
    string reverseVowels(string s) {
        int l=0,r=s.size()-1;
        while(l<r)
        {
            if((s[l]=='a'||s[l]=='e'||s[l]=='i'||s[l]=='o'||s[l]=='u'||s[l]=='A'||s[l]=='E'||s[l]=='I'||s[l]=='O'||s[l]=='U')&&(s[r]=='a'||s[r]=='e'||s[r]=='i'||s[r]=='o'||s[r]=='u'||s[r]=='A'||s[r]=='E'||s[r]=='I'||s[r]=='O'||s[r]=='U'))
            {
                char temp=s[l];
                s[l]=s[r];
                s[r]=temp;
                l++;
                r--;
            }
            else if((s[l]!='a'&&s[l]!='e'&&s[l]!='i'&&s[l]!='o'&&s[l]!='u'&&s[l]!='A'&&s[l]!='E'&&s[l]!='I'&&s[l]!='O'&&s[l]!='U')&&(s[r]=='a'||s[r]=='e'||s[r]=='i'||s[r]=='o'||s[r]=='u'||s[r]=='A'||s[r]=='E'||s[r]=='I'||s[r]=='O'||s[r]=='U'))
            l++;
            else if((s[l]=='a'||s[l]=='e'||s[l]=='i'||s[l]=='o'||s[l]=='u'||s[l]=='A'||s[l]=='E'||s[l]=='I'||s[l]=='O'||s[l]=='U')&&(s[r]!='a'&&s[r]!='e'&&s[r]!='i'&&s[r]!='o'&&s[r]!='u'&&s[r]!='A'&&s[r]!='E'&&s[r]!='I'&&s[r]!='O'&&s[r]!='U'))
            r--;
            else
            {
                l++;
                r--;
            }
        }
        return s;
    }
};
```

### 分析
 - 执行用时 :16 ms, 在所有 C++ 提交中击败了42.71%的用户内存消耗 :7.7 MB, 在所有 C++ 提交中击败了100.00%的用户。采用双指针的思想，
   条件判断上比较笨的做法，但也比较简单。
 - 要是能熟练使用STL会更好。而看了其他人双指针的解法，其实在条件判断上可以写个函数来优化，不至于我写的这么冗杂。
   
### 优解
#### 1.双端队列
```c++
class Solution {
public:
    string reverseVowels(string s) {
        deque<int> q;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u' || 
                s[i] == 'A' || s[i] == 'E' || s[i] == 'I' || s[i] == 'O' || s[i] == 'U') {
                q.emplace_back(i);
            }
        }
        
        while(q.size() > 1) {
            swap(s[q.front()], s[q.back()]);
            q.pop_front();
            q.pop_back();
        }
        return s;
    }
};

作者：hdye
```

#### 2.哈希表
```c++
class Solution {
public:
    string reverseVowels(string s) {
        deque<int> q;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u' || 
                s[i] == 'A' || s[i] == 'E' || s[i] == 'I' || s[i] == 'O' || s[i] == 'U') {
                q.emplace_back(i);
            }
        }
        
        while(q.size() > 1) {
            swap(s[q.front()], s[q.back()]);
            q.pop_front();
            q.pop_back();
        }
        return s;
    }
};

作者：hdye
