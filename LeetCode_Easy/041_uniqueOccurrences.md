# 独一无二的出现次数
***
#### 2020.03.09

### 问题
>给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。
如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。

### 示例
>输入：arr = [1,2,2,1,1,3]  
输出：true  
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。  

>输入：arr = [1,2]  
输出：false  

>输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]  
输出：true  

### 提示
>1.1 <= arr.length <= 1000   
2.-1000 <= arr[i] <= 1000   

### 思路
>将数组由小到大排序，依次遍历，出现重复的数则记录一次。最后若所有数的次数都不相等则符合，否则不符合。

### 代码
```c++
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        int len=arr.size();
        if(len==1)
        return 1;
        int *num;
        num=new int[len]();
        int i=0;
        num[0]=1;
        sort(arr.begin(),arr.end());
        for(int j=1;j<len;j++){
            if(arr[j]==arr[j-1])
            num[i]++;
            else{
            i=i+1;
            num[i]++;}
        }
        sort(num,num+i+1);
        for(int k=1;k<=i;k++){
            if(num[k]==num[k-1]){
            return 0;}
        }
        return 1;
    }
};
```

### 分析
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :10.3 MB, 在所有 C++ 提交中击败了5.30%的用户。本来以为这种暴力的求解方法用了好几个
   循环会比较浪费时间，没想到时间上还比较可观，但内存上很不如意。而且也因为一些非常低级的下标错误一直运行不了，以后应当注意。
 - 在初始化new申请的动态数组时，从网上搜来的memset函数使用不当造成溢出也影响了正确答案的得出，对于memset的用法：
   https://blog.csdn.net/qq_44195656/article/details/92398606 。而其第三个参数是字节而非数组元素长度。
 -  另外一个值得学习的是依旧对hash和map没有熟练掌握，因为题解中的方法几乎是用这两种，而我却不能想到，所以暴力的解决方法也应当得到改善了。

hash表
去重集合没必要2000，因为长度最大1000(甚至强迫症自己去试最小多少)，都小，可直接申请，不会爆炸，后来发现hash申请为char也可以通过。
hash[i] > 0 && set[hash[i]] > 0:意思是hash表中出现次数大于0且set集合中出现次数也大于0，证明字数出现次数重复，立马返回false，否者就给set[出现次数]
赋值1。
即使if条件不满足，要么是hash[i]<=0,要么set[出现次数]<=0，两种情况都可向set[hash[i]]赋值1，0一直被重复赋值也没有影响
代码
```c
bool uniqueOccurrences(int* arr, int arrSize){
	char hash[2000] = {0}, set[1000] = {0};
	for (short i = 0; i < arrSize; i++) hash[arr[i]+1000]++;
	for (short i = 0; i < 2000; i++) {
		if (hash[i] > 0 && set[hash[i]] > 0) return false;
		else set[hash[i]] = 1;
	}
	return true;
}

作者：boille
```

map
```c++
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        map<int,int> count;
        for(int i = 0; i < arr.size(); i++){
            count[arr[i]]++;
        }
        set<int> cmp;
        for(auto i = count.begin(); i != count.end(); i++){
            if(cmp.find(i -> second) != cmp.end()){
                return false;
            }
            cmp.insert(i -> second);
        }
        return true;
    }
};

作者：epecker
