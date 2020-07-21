# 转变日期格式
***
#### 2020.07.21

### 问题
>给你一个字符串 date ，它的格式为 Day Month Year ，其中：           
	Day 是集合 {"1st", "2nd", "3rd", "4th", ..., "30th", "31st"} 中的一个元素。            
	Month 是集合 {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"} 中的一个元素。                           
	Year 的范围在 [1900, 2100] 之间。                     
请你将字符串转变为 YYYY-MM-DD 的格式，其中：                   
	YYYY 表示 4 位的年份。                            
	MM 表示 2 位的月份。                         
	DD 表示 2 位的天数。                                   

### 示例
>输入：date = "20th Oct 2052"                
输出："2052-10-20"                     

>输入：date = "6th Jun 1933"                 
输出："1933-06-06"                         

>输入：date = "26th May 1960"                 
输出："1960-05-26"                                      

### 提示
>	给定日期保证是合法的，所以不需要处理异常输入。   

### 代码
```c++
class Solution {
public:
    string reformatDate(string date) {
        unordered_map<string,string>smonth={{"Jan","01"},{ "Feb","02"},{ "Mar","03"},{ "Apr","04"},{ "May","05"},{ "Jun","06"},{ "Jul","07"},{ "Aug","08"},{ "Sep","09"},{ "Oct","10"},{ "Nov","11"},{ "Dec","12"}};
        stringstream ss(date);
        string day,month,year;
        ss>>day>>month>>year;
        string Smonth=smonth[month];
        day.pop_back();
        day.pop_back();
        if(day.size()==1)
        day='0'+day;
        return year+'-'+Smonth+'-'+day;
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了56.47%的用户内存消耗：6.2 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题按照我们传统的思路，可能先想到的是以已知字符串的空格为切入点，依次将年月日提取并进行转换。但是由于“日”可能为个位数，也可能为十位数，这就为字符的提取增加了一些负担。
 - 参考了官方题解，发现了<sstream>下的stringstream用法，这个知识点是原来没有接触过的，由于将已知字符串以流的形式赋值，可以很巧妙也很方便地将其进行提取。而对于这一知识点的详细介绍可见：
   [stringstream常见用法](https://blog.csdn.net/liitdar/article/details/82598039)
 - 但是在时间消耗上做的并不是很好。显然是有更好的办法。
 
### 优解
#### 1.0ms范例
```c++
class Solution {
public:
    string reformatDate(string date) {
        int  day, month=0, year;
        char month_s[4];
        sscanf(date.c_str(), "%d%*s %s %d", &day, month_s, &year);
        string months[]={"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"};
        for(int i=0;i<12;i++)
        {
            if(months[i]==month_s)
            {
                month = i+1;
                break;
            }
        }
        char res[20];
        sprintf(res, "%04d-%02d-%02d", year, month, day);
        return res;
    }
};
```

#### 2.哈希表和substr(双百)
```c++
class Solution {
public:
    string reformatDate(string date) {
        unordered_map<string,string> hash{{"Jan","01"},{"Feb","02"},{"Mar","03"},{"Apr","04"},{"May","05"},{"Jun","06"},{"Jul","07"},{"Aug","08"},{"Sep","09"},{"Oct","10"},{"Nov","11"},{"Dec","12"}};
        int idx1=date.find(' ',0);
        int idx2=date.find(' ',idx1+1);
        string res=date.substr(idx2+1)+"-"+hash[date.substr(idx1+1,idx2-idx1-1)]+"-";
        if(idx1==3) res+="0"+date.substr(0,idx1-2);
        else res+=date.substr(0,idx1-2);
        return res;
    }
};

作者：ke-ai-de-xiao-li-yu
链接：https://leetcode-cn.com/problems/reformat-date/solution/ha-xi-biao-he-find-substr-by-ke-ai-de-xiao-li-yu/
```
