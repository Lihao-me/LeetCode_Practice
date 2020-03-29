# 最小绝对差
***
#### 2020.03.29

### 问题
>给你个整数数组 arr，其中每个元素都 不相同。            
请你找到所有具有最小绝对差的元素对，并且按升序的顺序返回。          

### 示例
>输入：arr = [4,2,1,3]               
输出：[[1,2],[2,3],[3,4]]                          

>输入：arr = [1,3,6,10,15]          
输出：[[1,3]]       

>输入：arr = [3,8,-10,23,19,-4,-14,27]            
输出：[[-14,-10],[19,23],[23,27]]         

### 提示
>1.2 <= arr.length <= 10^5             
2.-10^6 <= arr[i] <= 10^6           

### 思路
>先对数组进行排序，找到最小差值。最后再遍历一遍数组，讲符合最小差值的一对数记录。

### 代码
```c++
class Solution {
public:
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        sort(arr.begin(),arr.end());
        int min=arr[1]-arr[0];
        for(int i=1;i<arr.size();i++)
        {
            if(arr[i]-arr[i-1]<min)
            min=arr[i]-arr[i-1];
        }
        vector<vector<int>>res;
        for(int i=1;i<arr.size();i++)
        {
            if(arr[i]-arr[i-1]==min)
            res.push_back({arr[i-1],arr[i]});
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :300 ms, 在所有 C++ 提交中击败了5.06%的用户内存消耗 :30.8 MB, 在所有 C++ 提交中击败了5.56%的用户。今天击败的人确实有点少，但不
   得不承认这真的是我能想到得稍微简洁一点得方法了。
 - 但是题解中的思路和我都差不多，为什么用时差别这么大，想不通。
 
```c++
static auto pre =[] { std::ios::sync_with_stdio(false); cin.tie(nullptr); return nullptr;}();
class Solution {
public:
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        vector<vector<int>> result;
        int min = 1000001;
        sort(arr.begin(), arr.end());
        for (int i = 0; i < arr.size() - 1; i++) {
            int temp = arr[i + 1] - arr[i];
            if (temp < min) {
                min = temp;
                result = { { arr[i], arr[i + 1] } };
            } else if (temp == min) {
                result.emplace_back(vector<int>{ arr[i], arr[i + 1] });
            }
        }
        return result;
    }
};

作者：klaxxi
