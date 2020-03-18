# 复数乘法
***
#### 2020.03.18

### 问题
>给定两个表示复数的字符串。   
返回表示它们乘积的字符串。注意，根据定义 i2 = -1 。   

### 示例
>示例 1:     
输入: "1+1i", "1+1i"     
输出: "0+2i"      
解释: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i ，你需要将它转换为 0+2i 的形式。     

>示例 2:      
输入: "1+-1i", "1+-1i"      
输出: "0+-2i"       
解释: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i ，你需要将它转换为 0+-2i 的形式。      

### 注意:     
>1.输入字符串不包含额外的空格。      
2.输入字符串将以 a+bi 的形式给出，其中整数 a 和 b 的范围均在 [-100, 100] 之间。输出也应当符合这种形式。      

### 代码
```c++
#include<cstring>
class Solution {
public:
    string complexNumberMultiply(string a, string b) {
        string x,y,m,n;
        int flag1,flag2,flag3,flag4;
        int i=0;
        for(i=0;i<a.size();++i)
        {
            if(a[i]=='+'){
            flag1=i;
            break;}
            if(a[i]=='i'){
                flag2=i;
                break;
            }
        }
        for(i=0;i<b.size();++i)
        {
            if(b[i]=='+'){
                flag3=i;
                break;
            }
            if(b[i]=='i'){
                flag4=i;
            }
        }
        x=a.substr(0,flag1);
        y=a.substr(flag1+1,flag2-flag1-1);
        m=b.substr(0,flag3);
        n=b.substr(flag3+1,flag4-flag3-1);
        int p=stoi(x),q=stoi(m),k=stoi(y),r=stoi(n);
        string res1=to_string(p*q-k*r);
        string res2=to_string(p*r+k*q);
        return res1+'+'+res2+'i';
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :7.2 MB, 在所有 C++ 提交中击败了100.00%的用户。又是一个双百。这次仅仅是在函数的
   使用上学习了网友，但思路还是自己想出来的，所以最后能做到双百非常开心。
 - 这道题的思路是不难想的，关键是字符串与整型数的相互转化上，这样就不得不提到代码中所使用的stoi和to_string了。stoi是直接将数字字符串转化成其相应的
   整型数的函数，而to_string顾名思义就是一个逆过程。但其实这两个函数的限制还是非常多的。对于stoi函数，详见：
   https://blog.csdn.net/weixin_30502965/article/details/102374637       
   还有和atoi的比较，值得一看：https://blog.csdn.net/qq_33221533/article/details/82119031     
   再就是to_string: https://blog.csdn.net/lzuacm/article/details/52704931    
   另外值得一提的是前面已经用过两次的substr,在这里再加深一下印象：https://www.cnblogs.com/xzxl/p/7243490.html       
   
代码可简化：
```c++
class Solution {
public:
    string complexNumberMultiply(string a, string b) {
        int a1 = stoi(a), b1 = stoi(b);
        int i = 0, j = 0;
        while (a[i] != '+') i++;
        while (b[j] != '+') j++;
        a = a.substr(i + 1), b = b.substr(j + 1);
        int a2 = stoi(a), b2 = stoi(b);
        string res1 = to_string(a1 * b1 - a2 * b2);
        string res2 = to_string(a1 * b2 + a2 * b1);
        return res1 + "+" + res2 + "i";
    }
};
作者：yao-chi
```

stringstream切割：
```c++
class Solution {
public:
    string complexNumberMultiply(string a, string b) {
        // 切割出实部和虚部
        vector<string> as;
        stringstream ss(a);
        string tmp;
        while (getline(ss, tmp, '+')) as.push_back(tmp);
        as[1].erase(as[1].end() - 1);
        vector<string> bs;
        stringstream ss2(b);
        while (getline(ss2, tmp, '+')) bs.push_back(tmp);
        bs[1].erase(bs[1].end() - 1);
        // 转成int然后计算
        int a_real = stoi(as[0]);
        int a_img = stoi(as[1]);
        int b_real = stoi(bs[0]);
        int b_img = stoi(bs[1]);

        return to_string(a_real * b_real - a_img * b_img) + "+" + to_string(a_real * b_img + a_img * b_real) +"i";
    }
};
作者：zhenying
```

数据结构：
```c++
class Solution {
public:
    struct Complex {
        int re;
        int im;
        Complex(int r, int i) : re(r), im(i) {};
        Complex operator * (const Complex& other) {
            int r = re * other.re - im * other.im;
            int i = re * other.im + im * other.re;
            return Complex(r, i);
        }
        string serial() {
            return to_string(re) + "+" + to_string(im) + "i";
        }
    };
    Complex parse(const string& s) {
        int i = s.find_first_of('+');
        int re = stoi(s.substr(0, i));
        int im = stoi(s.substr(i + 1, s.size() - 1 - i));
        return Complex(re, im);
    }
    string complexNumberMultiply(string a, string b) {
        return (parse(a) * parse(b)).serial();
    }
};
作者：da-li-wang
```

正则表达式：
```c++
class Solution:
    def complexNumberMultiply(self, a: str, b: str) -> str:
        import re
        re1 = re.findall(r'(.*?)\+',a)
        re2 = re.findall(r'(.*?)\+',b)
        Im1 = re.findall(r'.*\+(.*?)i',a)
        Im2 = re.findall(r'.*\+(.*?)i',b)
        Re = int(re1[0]) * int(re2[0]) - int(Im1[0]) * int(Im2[0])
        Im = int(re1[0]) * int(Im2[0]) + int(re2[0]) * int(Im1[0])
        return str(Re) + '+' + str(Im) + 'i';
作者：he-fang-yuan
```

c语言sscanf(),sprintf():
```c
#define LEN 64
char * complexNumberMultiply(char * a, char * b){
    int x1,y1;
    int x2,y2,sk1 = 1,sk2 = 1;
    int x = 0, y = 0;
    int p_ri = 0, t_ri = 0, ri = 0;
    char pre[LEN], tail[LEN];
    char *arr = (char *)malloc(sizeof(char) * LEN);
    sscanf(a,"%d+%d",&x1,&y1);//这个把字符转换到数字
    sscanf(b,"%d+%d",&x2,&y2);
    x = x1 * x2 - y1 * y2;//公式计算
    y = y1 * x2 + x1 * y2;
    sprintf(arr,"%d+%di",x,y);//输入数组
    return arr;
}
hai-gen
```

如何字符串转数字   
atoi(char*), return int   
如何数字转字符串    
sprintf(str,"%d",value)      
itoa(int value,char* str,int base)      
如何string转char*      
(char*)string.c_str      
如何获取子字符串      
str.substr()      
如何查找字符或子字符串位置       
str.find()，注意输入的pos和返回的pos都为size_t类型，若找不到需与string::npos比较判断      
如何拼接字符串    
直接+      
作者：nick
