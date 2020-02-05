# 验证外星语词典
***
#### 2020.02.05

### 问题
>某种外星语也使用英文小写字母，但可能顺序 order 不同。字母表的顺序（order）是一些小写字母的排列。
给定一组用外星语书写的单词 words，以及其字母表的顺序 order，只有当给定的单词在这种外星语中按字典序排列时，返回 true；否则，返回 false。

### 示例
>输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
输出：true
解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。

>输入：words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
输出：false
解释：在该语言的字母表中，'d' 位于 'l' 之后，那么 words[0] > words[1]，因此单词序列不是按字典序排列的。

>输入：words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
输出：false
解释：当前三个字符 "app" 匹配时，第二个字符串相对短一些，然后根据词典编纂规则 "apple" > "app"，因为 'l' > '∅'，其中 '∅' 是空白字符，定义为比任何
其他字符都小（更多信息）。

>1.1 <= words.length <= 100
 2.1 <= words[i].length <= 20
 3.order.length == 26
 4.在 words[i] 和 order 中的所有字符都是英文小写字母。
 
 ### 思路
 >把所有的字母替换成正常的26个字母顺序进行比较。
 
 ### 代码
 ```c
 bool isAlienSorted(char ** words, int wordsSize, char * order){
    char map[26]={0};
    for(int i=0;i<strlen(order);i++){
        map[i]=order[i]+(i+'a');
    }
    int j;
    for(int i=0;i<wordsSize;i++){
        for(j=0;j<strlen(words[i]);j++);{
            for(int k=0;k<26;k++){
                if(words[i][j]==order[k]){
                    words[i][j]=map[k]-order[k];
                }
            }
        }
    }
    for(int i=0;i<wordsSize-1;i++){
            if(words[i][0]<words[i+1][0]){
                continue;
            }
            else if(words[i][0]>words[i+1][0]){
                return 0;
            }
            else{
                if(strcmp(words[i],words[i+1])>0)
                return 0;
            }
    }
    return 1;
}
```

### 分析
 - 这道题我的思路首先想到的时将字母表替换，然后将所有的字母按已更换的正常顺序的字母替换并进行比较，其中用到的strcmp这一函数需要充分理解其含义。对字母进行
   逐一比较。
 - 而这道题的困难在于充分理解strcmp的含义以及字母替换的手段。字母替换利用构造新的字符组map以将单词中的字母按规律替换。
 - 但是很显然用到了大量的循环，对内存和时间存在一定量的牺牲。网友的思路有与我相同的，但更高明的则是map和hash表的解法，贴上。
 
 map解法：
 ```c++
 class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        map<char, int> oreder_map;
        for(int i = 0; i < order.length(); i ++) {
            order_map[order[i]] = i;
        }
        for(int j = 1; j < words.size(); j ++) {
            for(int i = 0; i < min(words[j - 1].length(), words[j].length()); i ++) {
                if(order_map[words[j - 1][i]] > order_map[words[j][i]]) {
                    return false;
                }
                else if(order_map[words[j - 1][i]] == order_map[words[j][i]]) {
                    if((i + 1 == words[j].length()) && (i + 1 < words[j - 1].length())) {
                        return false;
                    }
                    continue;
                }
                else {
                    break;
                }
            }
        }
        
        return true;
    }
};

作者：jiangzhiming
```
hash表解法：
```c++
class Solution {
    unordered_map<char, int> hash;

    bool checkOrder(string& a, string& b) {
        int len = min(a.size(), b.size()); // 取公共长度
        int i = 0;
        while (i < len && a[i] == b[i]) ++i; // 跳过前面字母相同的部分 
        if (i < len) return hash[a[i]] < hash[b[i]]; // 如果公共部分存在不相同的字母，则比较位置关系

        // 如果不存在公共部分，则比较单词长度。例如：apple，app
        return a.size() <= b.size();
    }
public:
    bool isAlienSorted(vector<string>& words, string order) {
        if (words.empty() || order.empty()) return false;

        // 将字母位置存入hash表，节省后续查找时间
        for (int i = 0; i < order.size(); ++i) hash[order[i]] = i;

        // 一组数组中，如果所有相邻的单词有序，那么整个数组也是有序的
        for (int i = 1; i < words.size(); ++i)
            if (!checkOrder(words[i - 1], words[i])) return false;
        
        return true;
    }
};

作者：2020-0202
