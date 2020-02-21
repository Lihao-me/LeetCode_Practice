# 分割平衡字符串
***
#### 2020.02.21

### 问题
>在一个「平衡字符串」中，'L' 和 'R' 字符的数量是相同的。
给出一个平衡字符串 s，请你将它分割成尽可能多的平衡字符串。
返回可以通过分割得到的平衡字符串的最大数量。

### 示例
>输入：s = "RLRRLLRLRL"
输出：4
解释：s 可以分割为 "RL", "RRLL", "RL", "RL", 每个子字符串中都包含相同数量的 'L' 和 'R'。

>输入：s = "RLLLLRRRLR"
输出：3
解释：s 可以分割为 "RL", "LLLRRR", "LR", 每个子字符串中都包含相同数量的 'L' 和 'R'。

>输入：s = "LLLLRRRR"
输出：1
解释：s 只能保持原样 "LLLLRRRR".

>提示：
1 <= s.length <= 1000
s[i] = 'L' 或 'R'

### 思路
>查遍整个数组，当遇到R时加一，遇到L时减一，出现一次0则计一个平衡字符串。

### 代码
```c
int balancedStringSplit(char * s){
    int len=strlen(s),k=0,n=0;
    for(int i=0;i<len;i++){
        if(s[i]=='R')
        k+=1;
        else if(s[i]=='L')
        k-=1;
        if(i!=0&&k==0)
        n+=1;
    }
return n;
}
```

### 分析
 - 这道题可以通过信息的数字化轻松得到答案，而简单之处在R和L的数量相等。
 - 用时8ms，击败63.28%；内存6.8MB，击败19.80%。看过题解后发现可以用贪心法将时间简化,这位使用贪心算法的网友用时0ms，其思路很值得学习。也有C语言
   用户三行代码搞定，但可读性上显然大大折损。

三行代码
```c
int balancedStringSplit(char * s){
    int cnt=0,ans=0;
    for(int i=0; i<strlen(s); i++) cnt = s[i]=='R'? (cnt+1):(cnt-1),ans = cnt==0? (ans+1):ans;
    return ans;
}

作者：JUNHAO_
```

初看本题，第一疑问是这样的平衡字符串是不是一定可以分割成这样的n(>1)段，其中每一段都是平衡字符串。答案显然是否定的。那么第二个疑问就来了，取答案值
的时候分割成n(>0))段，这n段一定都是平衡字符串吗？也不一定。比如样例中"LLLLRRRR"可以分割成"LLL", "LR", "RRR"三段，但答案依然是1。这两个疑问解决
后，我们再来看这个题。考虑任意一种分割，那么一定有 #(分割出的各段有非平衡的) ≤ #(分割出的各段全都平衡) （其中#()代表分割结果中平衡字符串的个数），
由于本题只是求#()的最大值，因此不妨只考虑分割出的各段全都平衡的情况。为了分割出最多的平衡字符串，我们应该从左往右进行一次遍历，每遇到一个极小的平衡
字符串就计数一次。为了判断是否达到“平衡”，我们引入“平衡因子”a（类比LeetCode1111嵌套深度），深度增加时a++，深度下降时a--，当a为0时，说明又开始记录
一个新的平衡字符串了。另外由于这题R和L都可以放在左侧，因此需要另外增加一个布尔变量add，记录此时是深度增加还是深度下降。（1111中的括号匹配，遇到(就
是深度增加，遇到)就是深度下降，所以不用额外空间进行记录）
```c++
class Solution {
public:
	int balancedStringSplit(string s) {
		int cnt = 0;
		int a = 0;	// a: balanced factor
		bool add;
		for(int i = 0; i < s.length() - 1; i++) {
			if(!a) {		// if a == 0, start again (add is true for sure)
				if(s[i] == s[i + 1])	add = true;
				else add = false;
				a++;
				cnt++;
			}
			else {	// if a != 0
				if(add) {	// if is still increasing
					a++;
					if(s[i] == s[i + 1])	add = true;
					else	add = false;
				}
				else {
					a--;
					if(s[i] == s[i + 1])	add = false;
					else	add = true;
				}
			}
		}
		return cnt;
	}
};

作者：horizonlc-codingMyHero
