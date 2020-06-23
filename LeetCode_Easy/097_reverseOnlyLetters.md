# 仅仅翻转字母
***
#### 2020.06.23

### 问题
>给定一个字符串 S，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。

### 示例
>输入："ab-cd"
输出："dc-ba"

>输入："a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"

>输入："Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"

### 提示
>1.S.length <= 100
2.33 <= S[i].ASCIIcode <= 122 
3.S 中不包含 \ or "

### 思路
>双指针的思路，两个指针分别从首尾搜索，当两个指向都是字母的时候就交换位置。

### 代码
```c++
class Solution {
public:
    string reverseOnlyLetters(string S) {
        int l=0,r=S.size()-1;
        while(l<r)
        {
            if(!isalpha(S[l]))
            l++;
            if(!isalpha(S[r]))
            r--;
            if(isalpha(S[l])&&isalpha(S[r]))
            swap(S[l++],S[r--]);
        }
        return S;
    }
};
```

### 分析
 - 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗：6.3 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 看到这道题，我首先发现对于原数组，交换后的数组的非字母是不改变位置的，所以我的思路本来是把字母专门存放到一个新的数组中，然后交换顺序后依次
   塞到原数组的字母位置，从而实现。但是很显然是很耗时和空间的。参考了一位网友的解法，发现还有isalpha函数很好，以前也没有听说过。所以这种思路
   就很简便。
   
### 优解
#### 1.记录索引
```c++
class Solution {
public:
    string reverseOnlyLetters(string S) {
        string s = S;
        const int n  = s.size();
        int d;
        stack<int> index;
        for(int i = 0;i < n;++i)
        {
            if((s[i]>='a'&&s[i]<='z')||(s[i]>='A'&&s[i]<='Z'))
            {
                index.push(i);
            }
        }
        for(int i = 0;i < n;++i)
        {
            if((S[i]>='a'&&S[i]<='z')||(S[i]>='A'&&S[i]<='Z'))
            {
                
                S[i] = s[index.top()];
                index.pop();
            }
        }
        return S;

    }
};

作者：cong-nan-dao-bei-4
```

#### 2.辅助队列
```c++
class Solution {
public:
	string reverseOnlyLetters(string S) {
		string strAlpha("");
		string res(S);
		queue<int> idxSave;
		int i = 0;
		for (auto ch : S) {
			if (isalpha(ch)) strAlpha += ch;
			else idxSave.push(i);
			i++;
		}
		for (i = 0; i < S.size(); ++i) {
			if (idxSave.front() == i) {
				res[i] = S[i];
				idxSave.pop();
			}
			else {
				res[i] = strAlpha.back();
				strAlpha.pop_back();
			}
		}
        return res;
	}
};

作者：Edison_whu
```
