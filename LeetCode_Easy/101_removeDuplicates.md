# 删除字符串中的所有相邻重复项
***
#### 2020.07.05

### 问题
>给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。                      
在 S 上反复执行重复项删除操作，直到无法继续删除。                   
在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。                           

### 示例
>输入："abbaca"                                               
输出："ca"                
解释：                       
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

### 提示
>1.1 <= S.length <= 20000
2.S 仅由小写英文字母组成。

### 代码
#### 1.超时版
```c++
class Solution {
public:
    string removeDuplicates(string S) {
        if(S.size()==1)
        return S;
        stack<char>s;
        int k=0;
        s.push(S[S.size()-1]);
        //cout<<s.top()<<endl;
        for(int i=S.size()-2;i>=0;i--)
        {
            if(s.empty()!=1&&s.top()==S[i])
            {
                s.pop();
                k--;
                //cout<<s.top()<<i<<endl;
            }
            else if(s.empty()==1||s.top()!=S[i]){
            s.push(S[i]);
            k++;
            }
            //cout<<i<<s.top()<<endl;
            
        }
        string res="";
        if(s.empty()!=1){
        for(int i=0;i<k+1;i++)
        {
            res=res+s.top();
            s.pop();
        }
        }
        return res;
    }
};
```

#### 2.通过版
```c++
class Solution {
public:
    string removeDuplicates(string S) {
        if(S.size()==1)
        return S;
        string res="";
        for(auto c:S)
        {
            if(res.size()&&res.back()==c)
            res.pop_back();
            else
            res += c;
        }
        return res;
    }
};
```

### 分析
 - 执行用时：36 ms, 在所有 C++ 提交中击败了59.87%的用户内存消耗：10.2 MB, 在所有 C++ 提交中击败了100.00%的用户。该提交结果为通过版本的结果。
 - 我第一次超时的版本代码也记录如上。可以看出来思路非常清晰。新建一个栈，然后将字符串的每一个元素与栈顶的相比较，如相同则弹出该元素，否则压入栈，最后将所有的元素弹出组成字符串。但是
   当运行到第93/98结果时出现超时，很显然我没有注意到S最高可至2000的提示，而两次的循环也注定了它会超时。但是第一次的代码的得出是不容易的。总结如下：
   1.遗忘了栈顶的元素比较法，而将s.pop()作为元素参与比较，事实上s.pop()只是一种操作类型；     
   2.对于栈的元素数量计算掌握不好，经过我的一遍遍试验和研究可以得出s.size()是栈的空间大小，而非最终的元素数目，所以我最终采取了定义整型计数器k的方法来得知元素数目；   
 - 抱着对“栈”的执念，我发现一网友用string来实现栈的思路非常好，而我之所以没想到是因为对string模板了解不够，其中的res.back()提取末项以及res.pop_back()“弹出”末项等功能都不是很了解。而
   掌握了这些之后，于是有了我的通过版代码。
 - 大部分的题解就是我的两个思路。我查看了所有解法中用时最少8ms的，很匪夷所思。一般情况下这种双指针的类似数学推理真的很难想，但一个循环、无新数组，还是应该赞一个，原理值得深思。
   
### 优解
#### 1.双指针
```c++
class Solution {
public:
    string removeDuplicates(string S) {
        int i=0, len = S.size();
        for (int j=0;j<len;j++,i++) {
            S[i]=S[j];
            if(i>0 && S[i]==S[i-1])
                i-=2;
        }
        return S.substr(0,i);
    }
};
```

#### 2.修改原字符串
```c++
class Solution {
public:
    string removeDuplicates(string S) {
        int top = 0;
        for (char ch : S) {
            if (top == 0 || S[top - 1] != ch) {
                S[top++] = ch;
            } else {
                top--;
            }
        }
        S.resize(top);
        return S;
    }
};
```
