# IP地址无效化
***
#### 2020.05.03

### 问题
>给你一个有效的 IPv4 地址 address，返回这个 IP 地址的无效化版本。            
所谓无效化 IP 地址，其实就是用 "[.]" 代替了每个 "."。                                   

### 示例
>输入：address = "1.1.1.1"                     
输出："1[.]1[.]1[.]1"                                

>输入：address = "255.100.50.0"                   
输出："255[.]100[.]50[.]0"                                  

>提示
给出的 address 是一个有效的 IPv4 地址                       

### 代码
```c++
class Solution {
public:
    string defangIPaddr(string address) {
        string res;
        int i=0;
        for(i=0;i<address.size();i++)
        {
            if(address[i]=='.')
            {
                res.push_back('[');
                res.push_back('.');
                res.push_back(']');
            }
            else
            res.push_back(address[i]);
        }

        return res;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.2 MB, 在所有 C++ 提交中击败了100.00%的用户。
 
```c++
class Solution {
public:
    string defangIPaddr(string address) {
        for(int i = address.size(); i >= 0; i--){
            if(address[i] == '.'){
                address.replace(i, 1, "[.]");
            }
        }
        return address;
    }
};

作者：QQqun902025048
```

```c++
class Solution {
public:
    string defangIPaddr(string address) {
        int len = address.size();//直接计算出IP地址的长度
        for (int i = 0; i < len; i++)//遍历每一个字符
        {
            if (address[i] == '.')//如果当前字符为'.'
            {
                address.insert(i, "[");//在它的前面添加'['字符
                address.insert(i + 2, "]");//在它的后面添加']'字符
                i = i + 2;//由于添加了2个字符,因此当前位置应该加上2
                len = len + 2;//字符串长度也要加上2
            }
        }
        return address;//返回处理好的IP地址
    }
};

作者：xslxsl
