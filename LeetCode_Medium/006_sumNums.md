# 求1+2+...+n
***
#### 2020.03.13

### 问题
>求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

### 示例
>输入: n = 3   
输出: 6   

>输入: n = 9   
输出: 45   

### 提示
>1 <= n <= 10000

### 代码
```c++
class Solution {
public:
    int sumNums(int n) {
        n&&(n+=sumNums(n-1));
        return n;

    }
};
```

### 分析
 - 这道题如果不加这些限制条件本身甚至就不能算一道题，但是加了这些条件就具有了一定难度。而将其难度提升至中档我觉得就在于“短路”这一思想的植入。不能使用
   各种常见的语法语句的确是一个致命的打击。不能用循环算法和四则运算法则好说，我可以递归，但是没有了判断语句则意味着无法终止递归进程。评论区一位网友
   也是很会补刀：“答应我，别再if/else走天下了好吗？”不得不承认从当初开始学C语言到如今，真的是几乎离不开这些判断语句，但是我们的进步也正是从这样的叩
   问中领悟与习得。不应该只在简单的地方原地踏步，而应积极地学习，积极地挑战。
 - 这里就要介绍一下什么是“短路”了：&& 的短路特性
   “&&”：（可代替if）    
   左边是false，则不执行右边表达式;    
   左边是true，则执行右边表达式。    
   “||”：    
   左边为true，则不执行右边表达式；   
   左边为false，则执行右边表达式。    
   而它也是可以用来替代if/while的利器。
 - 执行用时 :0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗 :7.9 MB, 在所有 C++ 提交中击败了100.00%的用户。
   也有用户通过类构造函数，这个做法就让初学Cpp的我叹为观止了。也有goto lebel的解法。更有甚者用了位运算。小小的一道题也是大有乾坤呐。这个位运算的解法
   真的是闻所未闻，值得咂摸。
   
```c++
class ConSum     //用构造函数实现
{
public:
	ConSum()      //利用创建n个对象来调n次构造函数
	{
		n++;
		sum += n;
	}
	static int GetSum()
	{
		return sum;
	}
	static int n;   //用静态数据存储才可达到累加效果
	static int sum;
};

int ConSum::n = 0;       //初始化静态成员
int ConSum::sum = 0;
class Solution {
public:
    int sumNums(int n) {
        ConSum::n = 0;       //初始化静态成员
        ConSum::sum = 0;
        ConSum *a = new ConSum[n];
	return ConSum::GetSum();   //访问静态成员函数必须用域作用符
    }
};

作者：vimday
```
用数组保存goto labels，然后用关系表达式的值作为数组的下标访问label并跳转。
类似的，还可以用数组保存函数指针并不断地通过函数调用来完成计算。
时间复杂度 O(n)
空间复杂度 O(1)
```c++
class Solution {
public:
    int sumNums(int n) {
        int r = 0;
        void* label[] = {&&end, &&repeat};
    repeat:
        r += n;
        n--;
        goto *label[n > 0];
    end:
        return r;
    }
};

作者：li-fei-11
```

位运算
```c
int sumNums(int n){
    char a[n][n+1];
    return sizeof(a) >> 1;      // n(n+!) / 2
}

作者：mokeeqian
