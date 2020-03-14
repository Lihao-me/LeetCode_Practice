# 字符串轮转
***
#### 2020.03.14

### 问题
>字符串轮转。给定两个字符串s1和s2，请编写代码检查s2是否为s1旋转而成（比如，waterbottle是erbottlewat旋转后的字符串）。

### 示例
>输入：s1 = "waterbottle", s2 = "erbottlewat"    
 输出：True    
 
>输入：s1 = "aa", s2="aba"     
 输出：False    

### 提示
>字符串长度在[0, 100000]范围内。

### 说明
>你能只调用一次检查子串的方法吗？

### 思路
>将s1与它本身拼接，寻找里面有没有s2，若有则符合，否则不符合。

### 代码
```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if(s1.size()==0&&s2.size()==0)
        return 1;
        if(s1.size()!=s2.size())
        return 0;
        string cmp=s1+s1;
        int judge=cmp.find(s2);
        if(judge==string::npos)
        return 0;
        else
        return 1;
    }
};
```

### 分析
 - 第一眼看的思路当然还是想通过暴力求解，由于字符串发生轮转后可被切为两部分，找到切割点后进行复原判断即可，但不得不说很麻烦了。我也想寻求更为简便的算
   法，然后就学习到了find函数及其应用。
 - fing函数是c++中专门用来找字符串中的子字符串的。string 类提供了 6 种查找函数,每种函数以不同形式的 find 命名。这些操作全都返回 
   string::size_type 类型的值，以下标形式标记查找匹配所发生的位置；或者返回一个名为 string::npos 的特殊值，说明查找没有匹配。string 类将
   npos 定义为保证大于任何有效下标的值。关于find及其返回类型可以详见：https://blog.csdn.net/linwh8/article/details/50752733 。而关于find函数的
   拓展可以详见：https://blog.csdn.net/YJH13599955133/article/details/90732646 。对于npos参数的详细解释可以参见：
   https://blog.csdn.net/linwh8/article/details/50752733 。
 - 执行用时 :12 ms, 在所有 C++ 提交中击败了65.17%的用户内存消耗 :9.6 MB, 在所有 C++ 提交中击败了100.00%的用户。也有字符串匹配的方法，详解是： 
   https://blog.csdn.net/hanzhen7541/article/details/104155275 。
   
字符串匹配
```c++
class Solution {
public:
	bool isFlipedString(string s1, string s2) {
		if (s1.size() != s2.size()) return false;
		/*这是最朴素的字符串匹配,时间复杂度O(m^2),空间复杂度O(1)*/
		// return match(s1, s2);
		/*kmp算法匹配，时间复杂度是O(m * 2) = O(m),空间复杂度是O(m)*/
		vector<int> next;
		getnext(s1, next);
		return match_kmp(s1, s2, next);
	}
	bool match(string s1, string s2){
		s2 = s2 + s2;
		int i = 0;
		int j = 0;
		while (i < s1.size() && j < s2.size())
		{
			if (s1[i] == s2[j]){
				i++;
				j++;
			}
			else{
				i = 0;
				j = j - i + 1;
			}
		}
		return i == s1.size();
	}
	/*
	求next数组，注意next[j]表示的是索引j前面的最大的前缀后缀相同的子串长度
	*/
	void getnext(string s1, vector<int> &next){
		next.push_back(-1);
		int k = -1;
		for (int j = 1; j < s1.size(); j++){
			while (k != -1 && s1[j - 1] != s1[k])
			{
				//这一步是关键
				k = next[k];
			}
			k++;
			next.push_back(k);
		}
	}
	bool match_kmp(string s1, string s2, vector<int> next){
		s2 = s2 + s2;
		int i = 0;
		int j = 0;
		while (i < (int)s1.size() && j < s2.size())
		{
			if (i == -1 || s1[i] == s2[j]){
				i++;
				j++;
			}
			else{
				i = next[i];
			}
		}
		return i == s1.size();
	}
	
};

作者：Wzing
```

双指针
```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if (s1.size() != s2.size()) return false;
        if (s1.size() == 0) return true;
        int length = s1.size();
        
        vector<int> begins;
        bool ans = 0;
        for (int i = 0; i<length; i++){
            if (s2[i] == s1[0])
                begins.push_back(i);
        }
        
        for (int k=0;k<begins.size();k++){
            int i = begins[k];
            int j = 0;
            bool ans_ = 1; //用来标志从begins[k]开始是否能match
            while(i<length){
                if (s2[i] != s1[j]){
                    ans_ &= 0;
                    break;
                }
                else{
                    i++;
                    j++;
                }
            }
            i = 0;
            while(j<length){
                if (s2[i] != s1[j]){
                    ans_ &= 0;
                    break;
                }
                else{
                    i++;
                    j++;
                }
            }

            ans |= ans_; //对begins中每个元素开始的所有对比进行或运算
        }
        return ans;
    }
};

作者：jerrylu
```

模拟旋转
```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if(s1.size()!=s2.size())
            return false;
        if(s1==s2)
            return true;
        for(int i=0;i<s1.size();i++)
        {
            s1.insert(s1.size(),s1.substr(0,1));
            s1.erase(0,1);
            if(s1==s2)
                return true;
        }
        return false;
    }
};

作者：xiao-bin-ge
```

substr获得子串
```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if(s1.size()!=s2.size()) return false;
        if(s1.size()==0) return true;
        string s3=s2+s2;
        int l=s1.size();
        for(int i=0;i<s3.size()-l+1;i++)
        {
            string p=s3.substr(i,l);
            if(p==s1)
            {
                return true;
            }
        }
        return false;
    }
};

作者：crush-22
```

C语言拼接法
```c
bool isFlipedString(char* s1, char* s2){

    if (strlen(s1) != strlen(s2)) {
        return false;
    }
    int len = strlen(s1);
    int i = 0;
    char* arr = (char*)malloc(sizeof(char) * len * 2 + 1);
    for (; i < 2 * len; i++) {
        arr[i] = s1[i % len];
        //printf("%c ", arr[i]);
    }
    arr[i] =  '\0';
    if (strstr(arr, s2) != NULL){
        return true;
    }

    return false;
}

作者：_lixuchen
```

C语言字符串函数的应用
```c
bool isFlipedString(char* s1, char* s2){
    int len1 = strlen(s1);
    int len2 = strlen(s2);
    if(len1 != len2) {
        return false;
    }
    char *temp = malloc(2*len1+1);
    memset(temp,0,2*len1+1);
    strncpy(temp,s2,len1);
    strncat(temp,s2,len1);
    if(strstr(temp,s1)) {
        return true;
    }
    return false;
}

作者：jone-13
