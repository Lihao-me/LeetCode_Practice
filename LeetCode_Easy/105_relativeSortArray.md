# 数组的相对排序
***
#### 2020.07.09

### 问题
>给你两个数组，arr1 和 arr2，     
	1.arr2 中的元素各不相同                     
	2.arr2 中的每个元素都出现在 arr1 中                             
对 arr1 中的元素进行排序，使 arr1 中项的相对顺序和 arr2 中的相对顺序相同。未在 arr2 中出现过的元素需要按照升序放在 arr1 的末尾。                                               

### 示例
>输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]                     
输出：[2,2,2,1,4,3,3,9,6,7,19]                  

### 提示
>1.arr1.length, arr2.length <= 1000                
2.0 <= arr1[i], arr2[i] <= 1000              
3.arr2 中的元素 arr2[i] 各不相同              
4.arr2 中的每个元素 arr2[i] 都出现在 arr1 中        

### 代码
```c++
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        int flag=0;
        for(int i=0;i<arr2.size();i++)
        {
            for(int j=0;j<arr1.size();j++)
            {
                if(arr1[j]==arr2[i])
                {
                    swap(arr1[flag],arr1[j]);
                    flag++;
                }
            }
        }
        sort(arr1.begin()+flag,arr1.end());
        return arr1;
    }
};
```

### 分析
 - 执行用时：8 ms, 在所有 C++ 提交中击败了54.75%的用户内存消耗：7.7 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题开始是没有什么思路的，但是既然是一个排序问题，就很容易往我们常见的排序方法上想，而给定的元素顺序是不规律的，所以很容易想到冒泡排序。这种方法的实现途径就和冒泡排序很像。但是嵌套
   的两层循环也注定了它的时间复杂度不是最简单的。
 - 而用时最短0ms的是桶排序的方法，感觉很巧妙，但是还需要时间来理解。
 
### 优解
#### 1.桶排序
```c++
class Solution { //桶排序
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2)
    {
        int a[1001] = { 0 }; //桶
        for (int j = 0; j < arr1.size(); j++)
        {
            if (j < arr2.size())
                a[arr2[j]] += 1001; //以1001为分解线，
            a[arr1[j]] += 1;
        }
        //最后a[arr2[j]]>=1001+1    (1)
        int tmp = 0;
        for (int i = 0; i < arr2.size(); i++)
        {
            while (a[arr2[i]]-- > 1001 ) //由（1）a[arr2[i]]>=1001 （2）
                arr1[tmp++] = arr2[i];
        }
        for (int i = 0; i < 1001; i++)
            while (a[i] > 0 && a[i]-- < 1000)//由（2）a[arr2[i]]>=1000
                arr1[tmp++] = i;
        return arr1;
    }
};

作者：lin-xi-zhu-w
链接：https://leetcode-cn.com/problems/relative-sort-array/solution/tong-pai-xu-ke-0ms-by-lin-xi-zhu-w/
```

#### 2.map
```c++
class Solution {
public:
	vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
		vector<int> res;
		map<int, int> m;//<num,count>;
		for (auto n: arr1) ++m[n];
		for (auto a : arr2) {
			while (m[a]--) res.push_back(a);
            m.erase(a);
		}
		for (map<int, int>::iterator it = m.begin(); it != m.end(); ++it) {
			while ((it->second)--) res.push_back(it->first);
		}
		return res;
	}
};

作者：cyhuang1995
链接：https://leetcode-cn.com/problems/relative-sort-array/solution/xi-guan-xing-xiang-dao-yong-mapbiao-shi-shu-zi-yu-/
```

#### 3.使用lambda表达式
```c++
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        unordered_map<int, int> dic;
        for (auto n : arr1) {
            dic[n] = 1001 + n;
        }
        for (int i = 0; i < arr2.size(); i++) {
            dic[arr2[i]] = i;
        }
        sort(arr1.begin(), arr1.end(), [dic](const int& a, const int& b) {
            return dic.at(a) < dic.at(b);
        });
        return arr1;
    }

作者：ikaruga
链接：https://leetcode-cn.com/problems/relative-sort-array/solution/relative-sort-array-by-ikaruga/
```
