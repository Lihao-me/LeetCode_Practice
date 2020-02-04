# 卡牌分组
***
#### 2020.02.04

### 问题
>给定一副牌，每张牌上都写着一个整数。
此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：
-	每组都有 X 张牌。
-	组内所有的牌上都写着相同的整数。
仅当你可选的 X >= 2 时返回 true。

### 示例
>输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]

>输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。

>输入：[1]
输出：false
解释：没有满足要求的分组。

>输入：[1,1]
输出：true
解释：可行的分组是 [1,1]

>输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]

>提示：
1.1 <= deck.length <= 10000
2.0 <= deck[i] < 10000

### 代码
```c++
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int,int> numbers;//记录每个数字出现的个数
        for(auto i:deck){
            numbers[i]++;
        }
        int gcd=0;
        for(auto i:numbers){//记录当前的最大公约数
            gcd=__gcd(gcd,i.second);
            if(gcd<2){//最大公约数小于2则无法分组
                return 0;
            }
        }
        return 1;
    }
};
```

### 分析
 - 看到这道题首先想到的还是使用C语言进行编写，按照常规的暴力求解的思路无非是记录所有的数字出现的次数然后求出最大公约数判断是否能将其分组。但是这个过
   程只能按照老方法申请一个足够大的数组进行操作，这无疑是对内存的一个浪费。所以我仔细翻看题解看网友们有没有更好的方法。然而令我吃惊的是46个题解竟然
   没有一个使用C语言。
 - 而C++中提到最多的就是Map与unordered_map。所以我决定借此机会理解一下Map。通过欣赏网友的代码，也是半学习半理解得写出了用此方法的代码。由于是第一次
   接触，代码中的许多用法是未曾见过的。整个程序单看是看不懂的，也是查阅了许多相关文章得以了解所谓的Map究竟是什么。而对于上述代码的生疏之处在这里一一
   说明，也算是提前的认识与学习。
 - 上述代码使用unordered_map实现直接访问各个元素，而unordered_map内部事实上是由哈希桶组成的，上述代码中定义的int型函数就是可以实现记录每个数字出现
   个数的变量，而其下还有许多的扩展功能，详见网页https://blog.csdn.net/fcku_88/article/details/88353431 。
 - 第一次见到auto i:v的用法也是不知所云呐，原来这是c++11的新特性，范围for，相当于java的for each。v是一个可遍历的容器或流，比如vector类型，i就用来
   在遍历过程中获得容器里的每一个元素。
   例如：vector<int> v={1,2,3,4};
   for(auto i:v)
   cout<<i;
   结果就是1234 
   这种用法也使代码大大简化。
 - __gcd亦是一个陌生的函数,他也是algorithm头文件下的一个直接计算最大公约数的函数，这个函数也是大大提高了代码的简洁性。
 - 值得说明的是，i.second的用法也是第一次见到，原谅我孤陋寡闻，但我觉得实在是太好用，所以自己的代码上照搬了许多他人使用的工具。而.second顾名思义就是
   下一个元素的意思了，使用起来确实比用for循环一个一个比较起来简便得多。
 - 代码中陌生函数、工具功能的清晰也让代码更加清楚起来。但是我的疑问也随之产生，unordered_map和map是否一样？而答案是不一样的：前者不会根据key进行排序，‘
   而后者内部的元素则是有序的，详见网页https://www.cnblogs.com/NeilZhang/p/5724996.html 。
 - 而map则是一个头文件，其下的函数也是功能强大。https://blog.csdn.net/sevenjoin/article/details/81943864
 - 可以看出C语言虽然是一个开发性的语言，但是C++却在它的功能性上让人眼前一亮，编码时也感到更加舒服。虽然代码已经看似简洁，但这些函数与变量的内核并未被
   我真正了解，这也是以后有待学习的。同时也能解释为什么内存上9.9MB仅打败cpp用户的22.86%，时间上28ms仅击败17.81%。
 - 我所使用的主要时unordered_map的算法，那就贴上正宗的Map方法。
 
 ```c++
 class Solution {
public:
	int ys(int a,int b){//求最大公约数 
		int tmp;
		do{
			tmp=a%b;
			a=b;
			b=tmp;
		}while(tmp);
		return a;
	}
    bool hasGroupsSizeX(vector<int>& deck) {
    	if(deck.size()<=1)
    		return 0;
        map<int,int> m;
        map<int,int>::iterator it1,it2;
		for(int i=0;i<deck.size();i++)
			m[deck[i]]++;
		int tmp,min=10000;
		for(it1=m.begin(),it2=m.begin(),it2++;it2!=m.end();it1++,it2++){
			tmp=ys(it1->second,it2->second);
			if(tmp<min)
				min=tmp;	
		}
		return min>=2;
    }
};

作者：loyal-6
