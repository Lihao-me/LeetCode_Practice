### 总持续时间能被60整除的歌曲
***
#### 2020.05.12

### 问题
>在歌曲列表中，第 i 首歌曲的持续时间为 time[i] 秒。               
返回其总持续时间（以秒为单位）可被 60 整除的歌曲对的数量。形式上，我们希望索引的数字 i 和 j 满足  i < j
且有 (time[i] + time[j]) % 60 == 0。

### 示例
>输入：[30,20,150,100,40]          
输出：3                        
解释：这三对的总持续时间可被 60 整数：                
(time[0] = 30, time[2] = 150): 总持续时间 180               
(time[1] = 20, time[3] = 100): 总持续时间 120                    
(time[1] = 20, time[4] = 40): 总持续时间 60                      

>输入：[60,60,60]                    
输出：3                     
解释：所有三对的总持续时间都是 120，可以被 60 整数。                                    

### 提示
>1.1 <= time.length <= 60000                   
2.1 <= time[i] <= 500         

### 代码
```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        int num=0;
        for(int i=0;i<time.size();i++)
        {
            for(int j=i+1;j<time.size();j++)
            {
                if((time[i]+time[j])%60==0)
                {
                    num++;
                }
            }
        }
        return num;
    }
};
```

```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        int num=0;
        if(time.size()<2)
        return 0;

        vector<int>a(60,0);

        for(int i=time.size()-1;i>=0;--i)
        {
         int remain=time[i]%60;
         num += a[(60-remain)%60];
         ++a[remain];
        }
        return num;
    }
};
```

### 分析
 - 执行用时 :32 ms, 在所有 C++ 提交中击败了78.44%的用户内存消耗 :9.5 MB, 在所有 C++ 提交中击败了100.00%的用户。第一个代码超时了，暴力解法
   时间复杂度O(n^2)，实在想不出来了。参考了网友的哈希表的解法。不得不说很妙。
   
双指针
```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        sort(time.begin(), time.end());
        vector<int> sum = {60, 120, 180, 240, 300, 360, 420, 480, 540, 600, 660, 720, 780, 840, 900, 960};
        int count = 0;
        for (int s : sum) {
            int begin = 0;
            int end = (int) time.size() - 1;
            while (begin < end) {
                if (time[begin] + time[end] < s) {
                    begin ++;
                } else if (time[begin] + time[end] > s) {
                    end --;
                } else {
                    if (time[begin] != time[end]) {
                        int b_size = 1;
                        int e_size = 1;
                        while (begin < end && time[begin] == time[begin + 1]) {
                            begin ++;
                            b_size ++;
                        }
                        while (begin < end && time[end] == time[end - 1]) {
                            end --;
                            e_size ++;
                        }
                        count += b_size * e_size;
                    } else {
                        int b_size = 1;
                        while (begin < end && time[begin] == time[begin + 1]) {
                            begin ++;
                            b_size ++;
                        }
                        count += b_size * (b_size - 1) / 2;
                    }
                    begin ++;
                    end --;
                }
            }
        }
        return count;
    }    
};

作者：lemuria
```

map
```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        int res=0;
        map<int,int> mp;
        for(int i=0;i<time.size();i++)
            mp[time[i]%60]++;
        for(map<int,int>::iterator it=mp.begin();it!=mp.end();it++)
            if(it->first<=30)
                if(it->first==30||it->first==0)
                    res+=it->second*(it->second-1)/2;
                else
                    res+=it->second*mp[60-it->first];
        return res;
    }
};

作者：wandore
```

hash map
```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        //edge case
        if(time.size() <= 1) return 0;

        int res = 0;
        unordered_map<int, int> record;
        for(auto& t:time) t = t % 60;
        for(auto t:time){
            int residue = (60 - t) % 60;
            if(record.find(residue) != record.end()){
                res += record[residue];
            }
            record[t] += 1;
        }
        return res;
    }
};

作者：Daping
