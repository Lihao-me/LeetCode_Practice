# 转换成小写字母
***
#### 2020.04.10

### 问题
>实现函数 ToLowerCase()，该函数接收一个字符串参数 str，并将该字符串中的大写字母转换成小写字母，之后返回新的字符串。

 

### 示例
>输入: "Hello"            
输出: "hello"              

>输入: "here"              
输出: "here"                        

>输入: "LOVELY"               
输出: "lovely"            

### 代码
```c++
class Solution {
public:
    string toLowerCase(string str) {
        for(int i=0;i<str.size();i++)
        {
            if(str[i]<91&&str[i]>63)
            str[i]=str[i]+32;
        }
        return str;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.1 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题很简单，没啥好说的。
   但是依然有可以优化的地方。
   
transform
```c++
class Solution {
public:
    inline string toLowerCase(string str) {
        transform(str.begin(), str.end(), str.begin(), [] (auto& ch) { return tolower(ch); });
        return str;
    }
};

作者：klaxxi
```

位运算
```c++
class Solution {
public:
    string toLowerCase(string str) {
        for (int i = 0; i <str.size(); i++){
            str[i] |= 32;
        }
        return str;

    }
};

作者：xu-zhou-geng
```

```c
char * toLowerCase(char * str){
    char *t = str;
    while(*t != '\0'){
        if(*t >= 'A' && *t <= 'Z')
            *t |= 0b00100000;
        t++;
    }
    return str;
}

作者：zxhy-3
