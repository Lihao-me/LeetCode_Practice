# 判定字符是否唯一
***
#### 2020.03.24

### 实现一个算法，确定一个字符串 s 的所有字符是否全都不同。

### 示例
>输入: s = "leetcode"     
输出: false       

>输入: s = "abc"     
输出: true       

### 限制
>0 <= len(s) <= 100      
如果你不使用额外的数据结构，会很加分。      

### 思路
>遍历字符串中各元素，将其对应的计数加1，若出现大于1的计数，则不符合，否则符合。

### 代码
```c++
class Solution {
public:
    bool isUnique(string astr) {
        int n=astr.size();
        int *word=new int[27]();
        for(int i=0;i<n;i++){
            int m=astr[i]-'a';
            word[m]++;
            if(word[m]>1)
            return 0;

        }
        return 1;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :5.9 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题确实很简单，但是双百还
   是我没想到的。申请数组和循环其实都不是很理想。限制说不使用额外数据结构会加分，但是我也没想出来怎么不使用额外的数据结构。
   
定义一个128位的数据结构，每一位的1表示对应ascii字符有没有出现过，利用位运算实现。
由于不允许使用额外的数据结构，所以利用两个长整型代替。ascii可显示字符的十进制范围32-126，因此以95分界。
```c++
public:
    bool isUnique(string astr) {
        //return false;
        unsigned long long low_32_95 = 0;
        unsigned long long high_96_126 = 0;
        
        for(int i = 0; i < astr.length(); ++i)
        {
            if(astr[i] <= 95)
            {
                if((low_32_95 ^ (1<<(astr[i] - 32))) < low_32_95)
                    return false;
                
                low_32_95 = low_32_95 ^(1<<(astr[i] - 32));                
            }
            else
            {
                if((high_96_126 ^ (1<<(astr[i] - 95))) < high_96_126)
                    return false;
                
                high_96_126 = high_96_126 ^(1<<(astr[i] - 95));                
            }
        }
        return true;
    }
};

作者：Fanle
```

位运算
```c++
class Solution {
public:
    bool isUnique(string astr) {
        int bit = 0;
        for (char c: astr) {
            int index = c - 'a';
            int newBit = 1<<index;
            if ((bit & newBit) == newBit) {
                return false;
            }
            bit |= newBit;
        }
        return true;
    }
};

作者：bmyxb
```

find函数
```c++
class Solution {
public:
    bool isUnique(string astr) {
        string str;
        for(int i=0;i<astr.size();++i){
            if(str.find(astr[i]) != string::npos) return false;
            str.push_back(astr[i]);
        }
        return true;
    }
};

作者：oath-7
```

set性质
```c++
class Solution {
public:
    bool isUnique(string astr) {
        //利用set，hash_map
        if(astr.size() == 0)
            return true;
        set<char> sc;
        for(int j = 0; j < astr.size(); ++j)
        {
            if(sc.insert(astr[j]).second == false)
                return false;
            else
                sc.insert(astr[j]);
        }
        return true;
    }
};

作者：lee-355
```

vector哈希表
```c++
class Solution {
public:
    bool isUnique(string astr) {
        vector<char> m(128, 0);
        for (auto c: astr) {
            if (m[c - 'a'] == 0) {
                m[c-'a'] =1;
            } else {
                return false;
            }
        }
        return true;
    }
};

作者：tjuyzl
```

map
```c++
class Solution {
public:
    bool isUnique(string astr) {
        map<char,int> flag;
        int length = astr.length();
        for (int i = 0; i < length; i++)
        {
            flag[astr[i]] = flag[astr[i]] + 1;
            if (flag[astr[i]] != 1){
                return false;
            }
        }
        return true;
    }
};

作者：andre-12
```

排序（第一次直到sort可以对字符数组排序）
```c++
class Solution {
public:
    bool isUnique(string astr) {
        if(astr.size()==0)return 1;
        sort(astr.begin(),astr.end());
        for(int i=1;i<astr.size();++i)
        {
            if(astr[i]==astr[i-1])return 0;
        }
        return 1;
    }
};

作者：Ai6jgmdfab
