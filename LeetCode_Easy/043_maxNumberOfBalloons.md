# “气球”的最大数量
***
#### 2020.03.12

### 问题
>给你一个字符串 text，你需要使用 text 中的字母来拼凑尽可能多的单词 "balloon"（气球）。
字符串 text 中的每个字母最多只能被使用一次。请你返回最多可以拼凑出多少个单词 "balloon"。

### 示例
>输入：text = "nlaebolko"    
输出：1   

>输入：text = "loonbalxballpoon"   
输出：2   

>输入：text = "leetcode"    
输出：0    

### 提示
>1.1 <= text.length <= 10^4
2.text 全部由小写英文字母组成

### 思路
>记录所有相关的字母的出现次数，则能保证得到完整单词的字母频次就是可以得到“气球”的最大数量。

### 代码
```c++
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        int b=0,a=0,l=0,o=0,n=0;
        for(int i=0;i<text.size();i++){
            switch(text[i]){
                case 'b':b++;break;
                case 'a':a++;break;
                case 'l':l++;break;
                case 'o':o++;break;
                case 'n':n++;break;
            }
        }
        l=l/2;
        o=o/2;
        int num=min(b,min(a,min(l,min(o,n))));
        return num;
        
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了93.00%的用户，内存消耗 :8.1 MB, 在所有 C++ 提交中击败了100.00%的用户。这道题本身也非常好思考，而且最后
   利用到了整型变量的特性。对于那些出现两次才能凑出一个单词的来说，整型恰恰能够将多余的舍余。这道题的简单之处也在于“气球”这一单词限制死了，试想若任意
   给一单词，问能得几个单词，这样得问题就复杂多了。
 - 看了网友得解答之后发现也没有更好得方法。相比较用哈希表，虽然时间上和空间上跟我消耗一样，但代码则简洁了许多，也很好理解。
 
```c++
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        unordered_map<char,int> hash;
        for(int i = 0;i < text.size();i++){
            hash[text[i]]++;
        }
        return min(min(min(min(hash['b'],hash['a']),hash['l']/2),hash['o']/2),hash['n']);
    }
};

作者：183-3
