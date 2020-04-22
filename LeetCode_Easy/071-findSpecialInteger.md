# 有序数组中出现次数超过25%的元素
***
#### 2020.04.22

### 问题
>给你一个非递减的 有序 整数数组，已知这个数组中恰好有一个整数，它的出现次数超过数组元素总数的 25%。                 
请你找到并返回这个整数                          

### 示例
>输入：arr = [1,2,2,6,6,6,6,7,10]                           
输出：6                            
     
### 提示
1.1 <= arr.length <= 10^4            
2.0 <= arr[i] <= 10^5              

### 代码
```c++
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
        int n=arr.size();
        int ans=arr[0],cns=0;
        for(int i=0;i<arr.size();i++)
        {
            if(arr[i]==ans){
            cns++;
            if(cns*4>n)
            return arr[i];
            }
            else{
            ans=arr[i];
            cns=1;}
        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :28 ms, 在所有 C++ 提交中击败了33.63%的用户内存消耗 :12 MB, 在所有 C++ 提交中击败了33.33%的用户。这种低效的思路看起来很简单。
   关键是学会使用高效的算法。

二分查找
```c++
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
        int n = arr.size();
        int span = n / 4 + 1;
        for (int i = 0; i < n; i += span) {
            auto iter_l = lower_bound(arr.begin(), arr.end(), arr[i]);
            auto iter_r = upper_bound(arr.begin(), arr.end(), arr[i]);
            if (iter_r - iter_l >= span) {
                return arr[i];
            }
        }
        return -1;
    }
};

作者：LeetCode-Solution
```

map
```c++
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
    	map<int, size_t> record;
    	for(size_t i=0;i<arr.size();++i){
    		record[arr[i]]++;
    	}
    	vector<pair<int, size_t>> v_record(record.begin(), record.end());
    	for(auto& iter:v_record){
    		if(4*iter.second > arr.size()) return iter.first;
    	}
    	return -1;
    }
};

作者：user2473E
```

双指针
```c++
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
        int i = 0, j = 0;
        int n = arr.size();
        int limit = n >> 2;
        int count = 0;
        // 看似两个while，实际因为双指针，对每个元素只会遍历一次
        while(i < n){
            while(j < n && arr[i] == arr[j]){
                count++;
                j++;
            }
            if(count > limit){
                return arr[i];
            }else{
                i = j + 1;
                j = i;
                count = 0;
            }
        }
        return arr[i];
    }
};

作者：aspenstarss
