# 比较版本号
***
#### 2020.03.07

### 问题
>比较两个版本号 version1 和 version2。
如果 version1 > version2 返回 1，如果 version1 < version2 返回 -1， 除此之外返回 0。
你可以假设版本字符串非空，并且只包含数字和 . 字符。
 . 字符不代表小数点，而是用于分隔数字序列。
例如，2.5 不是“两个半”，也不是“差一半到三”，而是第二版中的第五个小版本。
你可以假设版本号的每一级的默认修订版号为 0。例如，版本号 3.4 的第一级（大版本）和第二级（小版本）修订号分别为 3 和 4。其第三级和第四级修订号均为 0。

### 示例
>输入: version1 = "0.1", version2 = "1.1"
输出: -1

>输入: version1 = "1.0.1", version2 = "1"
输出: 1

>输入: version1 = "7.5.2.4", version2 = "7.5.3"
输出: -1

>输入：version1 = "1.01", version2 = "1.001"
输出：0
解释：忽略前导零，“01” 和 “001” 表示相同的数字 “1”。

>输入：version1 = "1.0", version2 = "1.0.0"
输出：0
解释：version1 没有第三级修订号，这意味着它的第三级修订号默认为 “0”。

>提示：
1.版本字符串由以点 （.） 分隔的数字字符串组成。这个数字字符串可能有前导零。
2.版本字符串不以点开始或结束，并且其中不会有两个连续的点。

### 思路
>依次对相同位级的数比较，先较大的则符合。

### 代码
```c
int compareVersion(char * version1, char * version2){
    
    for(int i=0,j=0;i<strlen(version1)||j<strlen(version2);){
        int v1=0,v2=0;
        while(i<strlen(version1)&&version1[i]!='.'){
            v1=v1*10+(version1[i]-'0');
            i++;
        }
        i++;
        while(j<strlen(version2)&&version2[j]!='.'){
            v2=v2*10+(version2[j]-'0');
            j++;
        }
        j++;
        if(v1>v2)
        return 1;
        else if(v1<v2)
        return -1;
    }
return 0;
}
```

### 分析
 - 执行用时 ：4 ms, 在所有 C 提交中击败了64.77%的用户内存消耗 :6.8 MB, 在所有 C 提交中击败了51.95%的用户。思路还是比较好想的。
 - c++里的stringstream也很好。
 
```c++
class Solution {
public:
    int compareVersion(string version1, string version2) {
        char c;
        int v1,v2;
        istringstream its1(version1);
        istringstream its2(version2);
        
        while(bool(its1>>v1) + bool(its2>>v2)){
            if(v1>v2) return 1;
            if(v1<v2) return -1;
            
            v1=0;
            v2=0;
            its1>>c;
            its2>>c;
            
        }
        
        return 0;
    }
};

作者：victoriacck
