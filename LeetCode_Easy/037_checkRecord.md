# 学生出勤记录Ⅰ
***
#### 2020.02.28

### 问题
>给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符：
1.'A' : Absent，缺勤
2.'L' : Late，迟到
3.'P' : Present，到场
如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个连续的'L'(迟到),那么这个学生会被奖赏。
你需要根据这个学生的出勤记录判断他是否会被奖赏。

### 示例
>输入: "PPALLP"
输出: True

>输入: "PPALLL"
输出: False

### 思路
>当出现两个A或出现LLL时即不能获得奖赏。

### 代码
```c++
class Solution {
public:
    bool checkRecord(string s) {
        int a=0;
        for(int i=0;i<s.size();i++){
            if(s[i]=='A')
            a++;
            if(i+1<=s.size()-2&&s[i]=='L'&&s[i+1]=='L'&&s[i+2]=='L')
            return 0;
            }
        if(a<=1)
        return 1;
        else
        return 0;
    }
};
```

### 分析
 - 常规做法，一遍循环进行判断执行用时：8 ms击败11.30%，内存消耗：8.5 MB击败20.00%。但是时空上并不理想。可以考虑哈希表和find函数。而find函数就是查
   找第一次出现的字符串。

find函数
```c++
class Solution {
public:
    bool checkRecord(string s) {
        return s.find('A', s.find('A') + 1) == -1 && s.find("LLL") == -1;
    }
};

作者：klaxxi
```

哈希表
```c++
class Solution {
public:
    bool checkRecord(string s) {
        unordered_map<string,int> hash;
        if(s == "A"){
            return true;
        }
        for(int i =0;i<s.size();i++){
            string str = s.substr(i,3);
            string str1 = s.substr(i,1);
            hash[str1]++;
            hash[str]++;
        }
        if(hash["A"] <=1 && hash["LLL"]==0){
            return true;
        }
        return false;
    }
};

作者：183-3
