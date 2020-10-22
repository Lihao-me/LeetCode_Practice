# 划分字母区间
***
#### 2020.10.23

### 问题
>字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

示例 1：
```
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
 ```

提示：
- S的长度在[1, 500]之间。
- S只包含小写字母 'a' 到 'z' 。
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-labels
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

### 代码
```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        vector<int>res;
        int start=0,end=0;
        map<char,int>mymap;
        for(int i=0;i<S.size();i++)//记录每一个字母最后出现的位置
        {
            mymap[S[i]]=i;
        }
        for(int i=0;i<S.size();i++)
        {
            end=max(mymap[S[i]],end);
            if(i==end)//判断是否为最后出现的位置
            {
                res.push_back(end-start+1);
                start=i+1;
            }
        }
        return res;
    }
};
```

### 分析
 - 执行用时：12 ms, 在所有 C++ 提交中击败了44.94%的用户；内存消耗：6.9 MB, 在所有 C++ 提交中击败了20.97%的用户。     
 - 开始的时候是没有什么头绪的，看了评论才发现可以使用map来进行记录，可以说很巧妙的方法了。只需要记录最后出现的位置就可以巧妙地得出最后一位字符串地长度。   
 - 但是用时和存耗都十分不乐观，还需继续提升代码效率。
 
### 优解
#### 1.用时0ms范例
```c++
class Solution {
public:
    vector<int> partitionLabels(string S){
        if(!S.size()) return {};
        int t[26] = {0};
        for(int i = 0; i < S.size(); i ++)
            t[S[i] - 'a'] = i;

        vector<int> res;
        int k = 0;
        for(int i = 0; i < S.size();){
            int j = i;
            while(j <= t[S[i] - 'a'] && t[S[j] - 'a'] <= t[S[i] - 'a']) j ++;
            if(j == t[S[i] - 'a'] + 1){
                res. push_back(t[S[i] - 'a'] - k + 1);
                k = j;   
            }
            i = j;
        }
        return res;
    }
};
```
 - 解法和我的相似，但是他使用的数组,但是用时很短。
 
 #### 2.内存最少6532kb范例
 ```c++
 class Solution {
public:
    vector<int> partitionLabels(string S) {
        int n = S.length();
        vector<int>result;
        int letter[26]={0};
        for (int i=0;i<n;i++)
        {
            int t = S[i] - 'a';
            letter[t]++;
        }
        int quick = 0, slow = 0;
        for (int i = 0; i < n; i++)
        {
            slow++;
            int t = S[i] - 'a';
            if (letter[t] != 0)
            {
                quick += letter[t];
                letter[t] = 0;
            }
            if (quick == slow)
            {
                result.push_back(slow);
                quick = 0;
                slow = 0;
            }
        }
        return result;
    }
};
```
 - 和上一个题解差不多的思路，用的快慢指针，这种应该比map的内存分配确实少一些。
 
#### 3.栈与哈希表
```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        int size = S.size();
        stack<char> st;
        vector<int> ans;
        unordered_map<char, int> hash;
        for(int i = 0; i < size; i++){
            hash[S[i]] = i;
        }
        int left = 0, right = 0;
        for(int i = 0; i < size; i++){
            right = max(right, hash[S[i]]);
            if(i == right){
                ans.push_back(right - left + 1);
                left = right + 1;
            }
        }
        return ans;
    }
};

作者：lljj5464
链接：https://leetcode-cn.com/problems/partition-labels/solution/hua-fen-zi-mu-qu-jian-cha-xi-biao-liang-chong-bian/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 4.并查集
>解题思路
看上去又是一道动态规划，但读完题想了想，这貌似是一道UnionFindSet的题目。
题目中说到S中只会出现小写字母，那么我们可以直接开一个大小为26的vector，用pair<int, int>这个数据结构来存储某个字母第一次出现和最后一次出现的index，最初始都设置为500(因为最长为500，index最大值为499)。
第一次遍历，更新字符的起始index和中止index。
然后按照每个字符的起始index的大小顺序从小到大对charVec进行排序。
第二次遍历，将start和end初始化为S[0]的起始index和中止index。如果发现当前遍历的字符与区间[start, end]有交集，就把二者合并。如果没有交集，就把之前那一段push进results，然后更新start和end。(这里中止循环的逻辑判断有点绕，但是看代码应该能懂)

代码
```c++
class Solution {
public:
    vector<int> partitionLabels(string S)
    {
        int i, length = S.length();
        vector<pair<int, int>> charVec(26, make_pair(500, 500));
        for(i = 0; i < length; i++) {
            int index = S[i] - 'a';
            if(charVec[index].first == 500) {
                charVec[index].first = i;
                charVec[index].second = i;
            }
            else charVec[index].second = i;
        }
        sort(charVec.begin(), charVec.end(), sortFun);
        int start = charVec[0].first, end = charVec[0].second;
        vector<int> results(0);
        for(i = 1; i < charVec.size(); i++) {
            if(end > charVec[i].first) end = (end > charVec[i].second ? end : charVec[i].second);
            else {
                results.push_back(end - start + 1);
                start = charVec[i].first;
                end = charVec[i].second;
                if(charVec[i].second == 500) break;
            }
        }
        if(i == charVec.size()) results.push_back(end - start + 1);
        return results;
    }

    static bool sortFun(const pair<int, int>& character_1, const pair<int, int>& character_2)
    {
        return character_1.first < character_2.first;
    }
};

作者：wakawaka
链接：https://leetcode-cn.com/problems/partition-labels/solution/cbing-cha-ji-unionfindsetqiu-jie-by-wakawaka/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
