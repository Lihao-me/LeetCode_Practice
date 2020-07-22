# 数组序号转换
***
#### 2020.07.22

### 问题
>给你一个整数数组 arr ，请你将数组中的每个元素替换为它们排序后的序号。 
序号代表了一个元素有多大。序号编号的规则如下：                  
	序号从 1 开始编号。                     
	一个元素越大，那么序号越大。如果两个元素相等，那么它们的序号相同。                
	每个数字的序号都应该尽可能地小。                                    

### 示例
>输入：arr = [40,10,20,30]                 
输出：[4,1,2,3]                            
解释：40 是最大的元素。 10 是最小的元素。 20 是第二小的数字。 30 是第三小的数字。                        

>输入：arr = [100,100,100]                     
输出：[1,1,1]                  
解释：所有元素有相同的序号。                  

>输入：arr = [37,12,28,9,100,56,80,5,12]                   
输出：[5,3,4,2,8,6,7,1,3]                            
                                       
### 提示
>	0 <= arr.length <= 10^5                      
	-10^9 <= arr[i] <= 109^                   

### 代码
```c++
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        map<int,vector<int>>mymap;
        for(int i=0;i<arr.size();i++)
        mymap[arr[i]].emplace_back(i);

        int flag=1;
        for(auto x:mymap)
        {
            for(auto y:x.second)
            arr[y]=flag;

            flag++;
        }
        return arr;

    }
};
```

### 分析
 - 执行用时：312 ms, 在所有 C++ 提交中击败了49.87%的用户内存消耗：51.4 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题的关键在于记录排序后的数组的元素对应顺序。而我开始也是忽略了map的自动排序功能。事实上这个容器的排序功能是非常方便的。mymap.second存放着以mymap.first为数值的下标。
 
### 优解
#### 1.桶排序(100ms最短用时范例)
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        int len = arr.size();
        if(!len)
            return vector<int>();
        // 桶排序
        int maxn = INT_MIN;
        int minn = INT_MAX;
        for(int n : arr) {
            if(n > maxn) {
                maxn = n;
            }
            if(n < minn) {
                minn = n;
            }
        }
        int blen = maxn-minn+1;
        int* bucket = new int[blen];
        for(int i=0;i<blen;i++)
            bucket[i] = 0;
        for(int n : arr) {
            bucket[n-minn] = 1; // 这题坑，1,1,...,1,3那么3还是排第二
        }
        // 前缀和求每个元素前面有多少元素
        int* sumn = new int[blen];
        sumn[0] = 1; // 序号从1开始编号
        for(int i=1;i<blen;i++) {
            sumn[i] = sumn[i-1] + bucket[i-1];
        }
        // 取结果
        vector<int> ans(len);
        for(int i=0;i<len;i++) {
            ans[i] = sumn[arr[i]-minn];
        }
        return ans;
    }
};
```

#### 2.vector
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        if (arr.empty()) return {};
        vector<pair<int, int>> ais;
        for (int i = 0; i < arr.size(); ++i)
            ais.emplace_back(arr[i], i);
        sort(ais.begin(), ais.end());
        
        vector<int> ans(arr.size());
        int rank = 1, pre = ais[0].first;
        for (auto ai : ais) {
            if (ai.first == pre) 
                ans[ai.second] = rank;
            else {
                ans[ai.second] = ++rank;
                pre = ai.first;
            }
        }
        return ans;
    }
};

作者：Gary_coding
链接：https://leetcode-cn.com/problems/rank-transform-of-an-array/solution/c-zhong-gui-zhong-ju-de-192msjie-fa-vectormaper-fe/
```

#### 3.unique+二分查找
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<int> ans = arr;
        sort(arr.begin(), arr.end());
        int len = unique(arr.begin(), arr.end()) - arr.begin();
        for(int& a : ans)
            a = lower_bound(arr.begin(), arr.begin() + len, a) - arr.begin() + 1;
        return ans;
    }
};

作者：Gary_coding
链接：https://leetcode-cn.com/problems/rank-transform-of-an-array/solution/c-zhong-gui-zhong-ju-de-192msjie-fa-vectormaper-fe/
```

#### 4.set+map+vector
```c++
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        set<int>tmp;
        map<int,int>ans;
        vector<int>res;
        for(auto v:arr)tmp.insert(v);//现在tmp内的元素都是有序的啦,而且还实现了去重的功能
        int index=1;
        for(auto v:tmp)ans[v]=index++;//ans的第二个索引存放的是数据的顺序。
        for(auto v:arr)res.push_back(ans[v]);
        return res;
    }
};

作者：zb121
链接：https://leetcode-cn.com/problems/rank-transform-of-an-array/solution/c-map-by-zb121/
```

#### 5.哈希表
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<int> res;
        set<int> st(arr.begin(),arr.end());
        unordered_map<int,int> hash;
        int order=1;
        for(auto it:st)
        {
            hash[it]=order;
            order++;    
        }
        for(int i=0;i<arr.size();i++)
        {
            res.push_back(hash[arr[i]]);
        }
        return res;
    }
};

作者：ke-ai-de-xiao-li-yu
链接：https://leetcode-cn.com/problems/rank-transform-of-an-array/solution/li-yong-ji-he-he-ha-xi-biao-shi-xian-by-ke-ai-de-x/
```
