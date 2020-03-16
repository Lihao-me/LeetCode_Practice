# 用户分组
***
#### 2020.03.16

### 问题
>有 n 位用户参加活动，他们的 ID 从 0 到 n - 1，每位用户都 恰好 属于某一用户组。给你一个长度为 n 的
数组 groupSizes，其中包含每位用户所处的用户组的大小，请你返回用户分组情况（存在的用户组以及每个组中用户的 ID）。
你可以任何顺序返回解决方案，ID 的顺序也不受限制。此外，题目给出的数据保证至少存在一种解决方案。

### 示例
>输入：groupSizes = [3,3,3,3,3,1,3]     
输出：[[5],[0,1,2],[3,4,6]]        
解释：      
其他可能的解决方案有 [[2,1,6],[5],[0,4,3]] 和 [[5],[0,6,2],[4,3,1]]。     

>输入：groupSizes = [2,1,3,3,3,2]     
输出：[[1],[0,5],[2,3,4]]    

### 提示：   
1.groupSizes.length == n
2.1 <= n <= 500
3.1 <= groupSizes[i] <= n

### 代码
```c++
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        map<int,vector<int>>m;
        vector<vector<int>>res;
        for(int i=0;i<groupSizes.size();i++){
            m[groupSizes[i]].push_back(i);
            if(m[groupSizes[i]].size()==groupSizes[i])
            {
                res.push_back(m[groupSizes[i]]);
                m[groupSizes[i]]={};
            }
        }
        return res;

    }
};
```

### 分析
 - 执行用时：24 ms，内存消耗：14.3 MB。我的提交执行用时已经战胜 78.37 % 的 cpp 提交记录。本来依然是准备再申请一个二维数组进行暴力求解，但是还是想
   学习一下大家都是怎么做的。
 - 然后又一次看到了map的神奇之处，它非常舒适地储存了我们想储存地信息，非常方便。
 
哈希映射
对于两个用户 x 和 y，如果 groupSize[x] != groupSize[y]，它们用户组的大小不同，那么它们一定不在同一个用户组中。因此我们可以首先对所有的用户进行一
次【粗分组】，用一个哈希映射（HashMap）来存储所有的用户。哈希映射中键值对为 (gsize, users)，其中 gsize 表示用户组的大小，users 表示满足用户组大小
为 gsize，即 groupSize[x] == gsize 的所有用户。这样以来，我们就把所有用户组大小相同的用户都暂时放在了同一个组中。
在进行了【粗分组】后，我们可以将每个键值对 (gsize, users) 中的 users 进行【细分组】。由于题目保证了给出的数据至少存在一种方案，因此我们的【细分组】
可以变得很简单：只要每次从 users 中取出 gsize 个用户，把它们放在一个组中就可以了。在进行完所有的【细分组】后，我们就得到了一种满足条件的分组方案。
```c++
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        unordered_map<int, vector<int>> groups;
        for (int i = 0; i < groupSizes.size(); ++i) {
            groups[groupSizes[i]].push_back(i);
        }

        vector<vector<int>> ans;
        for (auto group = groups.begin(); group != groups.end(); ++group) {
            const int& gsize = group->first;
            vector<int>& users = group->second;
            for (auto iter = users.begin(); iter != users.end(); iter = next(iter, gsize)) {
                vector<int> dummy(iter, next(iter, gsize));
                ans.push_back(dummy);
            }
        }
        return ans;
    }
};

作者：LeetCode-Solution
```

贪心
```c++
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        vector<vector<int>> res;
        unordered_map<int, int> um;
        for (int i = 0; i < groupSizes.size(); ++i) {
            if (um.find(groupSizes[i]) == um.end() || res[um[groupSizes[i]]].size() >= groupSizes[i]) {
                um[groupSizes[i]] = res.size();
                res.push_back({i});
            }
            else if (res[um[groupSizes[i]]].size() < groupSizes[i]) {
                res[um[groupSizes[i]]].push_back(i);
            }
        }
        return res;
    }
};

作者：charon____
