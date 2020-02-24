# 猜数字
***
#### 2020.02.24

### 问题
>小A 和 小B 在玩猜数字。小B 每次从 1, 2, 3 中随机选择一个，小A 每次也从 1, 2, 3 中选择一个猜。他们一共进行三次这个游戏，请返回 小A 猜对了几次？
输入的guess数组为 小A 每次的猜测，answer数组为 小B 每次的选择。guess和answer的长度都等于3。

### 示例
>输入：guess = [1,2,3], answer = [1,2,3]
输出：3
解释：小A 每次都猜对了。

>输入：guess = [2,2,3], answer = [3,2,1]
输出：1
解释：小A 只猜对了第二次。

>限制：
1.guess的长度 = 3
2.answer的长度 = 3
3.guess的元素取值为 {1, 2, 3} 之一。
4.answer的元素取值为 {1, 2, 3} 之一。

### 思路
>将两组数对应依次比较，相等时计一次。

### 代码
```c
int game(int* guess, int guessSize, int* answer, int answerSize){
    guessSize=answerSize;
    int sum=0;
    for(int i=0;i<guessSize;i++){
        if(guess[i]==answer[i])
        sum++;
    }
    return sum;
}
```

### 分析
 - 这道题很简单，一次循环搞定。用时4ms，击败60.82%；内存2.8MB，击败25.97%。
 - 一位网友的异或解法值得学习。
 
```c++
class Solution {
public:
    int game(vector<int>& guess, vector<int>& answer) {
        return !(guess[0]^answer[0]) + !(guess[1]^answer[1]) + !(guess[2]^answer[2]);
    }
};

作者：ssln
