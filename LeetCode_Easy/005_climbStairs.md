# 爬楼梯
***
#### 2020.01.27

### 问题
>假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

### 示例
>输入： 2
>输出： 2
>解释： 有两种方法可以爬到楼顶。
>1.  1 阶 + 1 阶
>2.  2 阶

>输入： 3
>输出： 3
>解释： 有三种方法可以爬到楼顶。
>1.  1 阶 + 1 阶 + 1 阶
>2.  1 阶 + 2 阶
>3.  2 阶 + 1 阶

### 代码
```c
int climbStairs(int n){
    int s,s_1,s_2;
    s_1=1;
    s_2=2;
    if(n==1){
        return 1;
    }
    else if(n==2){
        return 2;
    }
    else{
        for(int i=3;i<=n;i++)
        {
            s=s_1+s_2;
            s_1=s_2;
            s_2=s;
        }
        return s;
    }
}
```

### 分析
 - 初看这道题还是有点麻烦的，首先想到的是把所有可能的情况一一枚举然后得到总的数量，但是工作量非常大。当把一阶、二阶、三阶等的情况先大致列举一下，就可    以很容易发现这就是斐波那契数列的实际应用问题。
 - 对于斐波那契数列是我们了解递归初期一个非常典型且简单的例子，所以实际应用问题下用递归就可以迎刃而解。
 - 当然，递归是一个简便的解法，不用递归便是循环解法了。今天就贴上循环的一个不错的解法。
 ```c
 int climbStairs(int n){
    if(n==0){return 0;}
	if(n==1){return 1;}
    if(n==2){return 2;}
	int a=1;int b=2;int c=0;
	n=n-2;
	while(n--){
		c=a+b;
		a=b;
		b=c;
	}
	return c;
}
作者：world-16
