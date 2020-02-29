# 反转字符串
***
#### 2020.02.29

### 问题
>编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。
不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。
你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

### 示例
>输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]

>输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]

### 思路
>从头和尾向里两两交换实现逆序。

### 代码
```c
void reverseString(char* s, int sSize){
    float a;
    a=(sSize+1)/2;
    for(int i=0;i<a;i++){
        char temp;
        temp=s[i];
        s[i]=s[sSize-1-i];
        s[sSize-1-i]=temp;        
    }
    return s;

}
```

### 分析
 - 这道题也非常简单，用双指针的思想。执行用时：108 ms战胜 5.99 % 的 c 提交记录，内存消耗：13.6 MB战胜 66.14 % 的 c 提交记录。
 - 还可以考虑用swap函数，reverse函数和递归。
 
 
swap函数
```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n=s.size();
        for(int i=0;i<n/2;i++)
        {
            if(s[i]==s[n-1-i])  continue;       //优化
            swap(s[i],s[n-1-i]);
        }
    }
};

作者：zrita
```

reverse函数
```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        reverse(s.begin(), s.end());
    }
};

作者：shiyi23
```

递归
```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        
        helper(s, 0, s.size());

    }
    
    void helper(vector<char>& s, int pos, int s_len){
        if ( pos >= s_len / 2) return;
        char tmp = s[pos];
        s[pos] = s[s_len - 1 - pos];
        s[s_len - 1 - pos ] = tmp;
        
        helper(s, pos+1, s_len);
    }
};

作者：xu-zhou-geng
