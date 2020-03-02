### 一年中的第几天
***
#### 2020.03.02

### 问题
>给你一个按 YYYY-MM-DD 格式表示日期的字符串 date，请你计算并返回该日期是当年的第几天。
通常情况下，我们认为 1 月 1 日是每年的第 1 天，1 月 2 日是每年的第 2 天，依此类推。每个月的天数与现行公元纪年法（格里高利历）一致。

### 示例
>输入：date = "2019-01-09"
输出：9

>输入：date = "2019-02-10"
输出：41

>输入：date = "2003-03-01"
输出：60

>输入：date = "2004-03-01"
输出：61

>提示：
1.date.length == 10
2.date[4] == date[7] == '-'，其他的 date[i] 都是数字。
3.date 表示的范围从 1900 年 1 月 1 日至 2019 年 12 月 31 日。

### 思路
>将该日期前直到1月1日的每月天数累加并加上该日期显示的天数即为总天数。

### 代码
```c++
class Solution {
public:
    int dayOfYear(string date) {
        int year=0,month=0,day=0;
        int month_array[12]={31,28,31,30,31,30,31,31,30,31,30,31};
        for(int i=0;i<4;i++)
        year=year*10+date[i]-48;
        for(int i=5;i<7;i++)
        month=month*10+date[i]-48;
        for(int j=8;j<10;j++)
        day=day*10+date[j]-48;

        if((year%4==0&&year%100!=0)||year%400==0)
        month_array[1]=29;
        for(int i=0;i<month-1;i++){
            day=day+month_array[i];
        }
        return day;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :8.3 MB, 在所有 C++ 提交中击败了15.56%的用户。时间上击败100%的用户，还是很高兴
   的。本来以为循环耗时比较大，事实上大家的算法则更为复杂。
 - 下面介绍一种普遍的split的解法。
 
```c++
class Solution {
public:
    int dayOfYear(string date)
    {
        vector<string> ymd = split(date, '-');
        int y = stoi(ymd[0]), m = stoi(ymd[1]), d = stoi(ymd[2]);
        int months[] = {31, 28, 31 ,30, 31, 30, 31, 31, 30, 31, 30, 31};
        if (isLeap(y)) months[1] = 29;
        int sum = d;
        for (int i = 0; i < m - 1; i++)
            sum += months[i];
        return sum;
    }
    bool isLeap(int y)
    {
        if (y % 4 == 0 && y % 100) return 1;
        if (y % 400 == 0) return 1;
        return 0;
    }
    vector<string> split(string str, char ch)
    {
        vector<string> subs;
        int length = 0;
        for (int i = -1, k = 0; k < str.length(); )
        {
            while (str[k] == ch) k++;
            i++;
            subs.push_back("");
            while (str[k] != ch && k < str.length())
            {
                subs[i] += str[k];
                k++;
            }
            length = i + 1;
        }
        return subs;
    }
};

作者：int-myheart
