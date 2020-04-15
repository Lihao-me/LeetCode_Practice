# 宝石与石头
***
#### 2020.04.15

### 问题
> 给定字符串J 代表石头中宝石的类型，和字符串 S代表你拥有的石头。 S 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少
是宝石。J 中的字母不重复，J 和 S中的所有字符都是字母。字母区分大小写，因此"a"和"A"是不同类型的石头。

### 示例
>输入: J = "aA", S = "aAAbbbb"              
输出: 3                  

>输入: J = "z", S = "ZZ"                   
输出: 0                      

### 注意
1.S 和 J 最多含有50个字母。               
2.J 中的字符不重复。             

### 代码
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        int num=0;
        for(int i=0;i<S.size();i++)
        {
            for(int j=0;j<J.size();j++)
            {
                if(S[i]==J[j]){
                num++;
             
                break;}
            }
        }
        return num;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :6.1 MB, 在所有 C++ 提交中击败了100.00%的用户。基础题，没啥好说的，时间
   复杂度O(n^2)。
   
哈希表
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S)
    {
        unordered_map<char, bool> mp;
        int sum = 0;
        for (auto j : J)
            mp[j] = 1;
        for (auto s : S)
            if (mp[s]) sum++;
        return sum;
    }
};

作者：int-myheart
```

strchar
```c
int numJewelsInStones(char * J, char * S){
    int i,num=0;
    for(i=0;S[i]!='\0';i++){
        if(strchr(J,S[i])) //若S[i]在J中则返回一个S[i]所在位置的指针
            num++;
    }
    
    return num;
}

作者：3989364
```

set+map
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
    	int num_jewels = 0;

    	map<char, int> record;
    	set<char> s_self_stone;
    	for(size_t i=0;i<S.size();++i){
    		record[S[i]]++;
    		s_self_stone.insert(S[i]);
    	}
    	for(size_t i=0;i<s_self_stone.size();++i){
    		auto index = s_self_stone.begin();
    		std::advance(index, i);
    		auto ind = std::find(J.begin(), J.end(), *index);
    		if(ind != J.end()){
    			num_jewels += record[*index];
    		}
    	}
    	return num_jewels;
    }
};

作者：user2473E
```

find
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        int num=0;
        for(auto i:S)
        {
            if(J.find(i)!=J.npos)
                num++;
        }
        return num;
    }
};

作者：a-pan-jia-de-shi-tou
```

multiset
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        multiset<char> stone;
        for(auto j:J)
            stone.insert(j);
        int num=0;
        for(auto i:S)
        {
            num+=stone.count(i);
        }
        return num;
    }
};

作者：a-pan-jia-de-shi-tou
```
