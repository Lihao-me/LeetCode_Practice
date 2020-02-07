# 3的幂
***
#### 2020.02.03

### 问题
>给定一个整数，写一个函数来判断它是否是 3 的幂次方。

### 示例
>输入: 27
输出: true

>输入: 0
输出: false

>输入: 9
输出: true

>输入: 45
输出: false

### 进阶
>你能不使用循环或者递归来完成本题吗？

### 代码
```c
bool isPowerOfThree(int n){
    if(n==3||n==1)
    return 1;

    if(n!=0&&n%3==0)
    return isPowerOfThree(n/3);
    else
    return 0;

}
```

### 分析
 - 这道题本身还是非常简单的，递归、循环是可以很容易想出来的，我之所以选择这道题是看上了它的进阶部分，如何不使用循环和递归做出来就有一些难度了。我也尝
   试了一些方法，但很显然是需要循环或递归的。
 - 看到一位网友的分析，int范围内的3的幂才19个，硬编码即可做出，不得不说确实是一个投机取巧的好方法呐。
 - 但是看了官方的解法感觉学好数学还是很重要呀。官方的非循环非递归解法无一不是用到了与3的幂次方有关的数学公式或特性，还是有点小高深呐。不过显然中文版
   网页是直接从原英文版机译过来，词汇表述并不专业，但也不影响理解。
 
 非递归非循环之“硬编码”：
 ```c++
 class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n == 1 || n == 3 || n == 9 || n == 27 || n == 81 || n == 243 || n == 729 || n == 2187 || n == 6561 || n == 19683 || n == 59049 || n == 177147 || n == 531441 || n == 1594323 || n == 4782969 || n == 14348907 || n == 43046721 || n == 129140163 || n == 387420489 || n == 1162261467) {
            return true;
        } else {
            return false;
        }
    }
};

作者：scut_dell
```

Java官方解法：
 - 方法一：循环迭代
找出数字 n 是否是数字 b 的幂的一个简单方法是，n%3  只要余数为 0，就一直将 n 除以 b。
n=bx n=b×b×…×b\begin{aligned}
n &= b^x \ n &= b \times b \times \ldots \times b
\end{aligned}
n​=bx n​=b×b×…×b​
因此，应该可以将 n 除以 b  x 次，每次都有 0 的余数，最终结果是 1。
```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        if (n < 1) {
            return false;
        }

        while (n % 3 == 0) {
            n /= 3;
        }

        return n == 1;
    }
}
```
注意我们需要一个警卫来检查那个 n！=0，否则 while 循环将永远不会结束。对于负数，该算法没有意义，因此我们也将包括该保护。
复杂度分析
时间复杂度：O(log⁡b(n))O(\log_b(n))O(logb​(n))，在我们的例子中是 O(log⁡n)O(\log n)O(logn)。除数是用对数表示的。
空间复杂度：O(1)O(1)O(1)，没有使用额外的空间。

 - 方法二：基准转换
在基数 10 中，10 的所有幂都从数字 1 开始，然后只跟 0（例如10、100、1000）。其他基地及其各自的权力也是如此。例如，在基数 2 中，10210 _2102​、1002100 _21002​ 和 100021000 _210002​ 分别表示为  2102_{10}210​, 4104_{10}410​ 和 8108_{10}810​。因此，如果我们把我们的数转换成基3，并且表示形式是 100…0，那么这个数就是3的幂。
证明 ：
给定以 3 为底的数字表示为数组 s，第 0 位开始为有效数字，从 3 为底转换为 10 为底的公式为：
∑i=0len(s)−1s[i]∗3i\sum_{i=0}^{len(s) - 1} s[i] * 3^{i}
i=0∑len(s)−1​s[i]∗3i
因此，只有一个数字 1，其余的都是 0，这意味着这个数字是 3 的幂。
实现：
我们所要做的就是将数字转换为以3为底的基数 ，并检查它是否为前导1，后跟所有 0。
两个内置的Java函数将帮助我们前进。
 ```java
 String baseChange = Integer.toString(number, base);
 ```
 上面的代码将 number 转换以 base 为底的基数，并以字符串形式返回结果。例如，integer.toString（5，2）=“101” 和 integer.toString（5，3）=“12”。
 ```java
 boolean matches = myString.matches("123");
 ```
 上面的代码检查字符串中是否存在特定的正则表达式。例如，如果字符串 mystring 中存在子字符串 “123”，上面的内容将返回 true。
 ```java
 boolean powerOfThree = baseChange.matches("^10*$")
 ```
 我们将使用上面的正则表达式来检查字符串是否以1 ^1 开头，后跟 0 或 多个 0 0* 并且不包含任何其他值 $。
 ```java
 public class Solution {
    public boolean isPowerOfThree(int n) {
        return Integer.toString(n, 3).matches("^10*$");
    }
}
```
复杂度分析
时间复杂度：O(log⁡3n)O(\log_3n)O(log3​n)。
假设：
Integer.toString() - 基转换通常是作为一个重复的除法来实现的。复杂性应该类似于我们的方法 1:O（ log3n）O（\ log_3n）O（ log3​n）的复杂性。
String.matches() - 方法迭代整个字符串。n 以 3 为基数表示的位数是O（log⁡3n）O（\log_3n）O（log3​n）。
空间复杂度：O(log⁡3n)O(\log_3n)O(log3​n)。我们使用两个附加变量。
以 3 为基数表示数字的字符串（大小为 log⁡3n\log_3nlog3​n）
正则表达式的字符串（常量大小）

 - 方法三：运算法
我们可以用下面的数学公式
n=3i i= log3(n) i= logb(n) logb(3)n = 3^i \ i = \ log_3(n) \ i = \frac{\ log_b(n)}{\ log_b(3)}
n=3i i= log3​(n) i= logb​(3) logb​(n)​
若 n 是 3 的幂则 i 是整数。在 Java 中，我们通过取小数部分（利用 % 1）来检查数字是否是整数，并检查它是否是 0。
```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return (Math.log10(n) / Math.log10(3)) % 1 == 0;
    }
}
```
常见的陷阱 :
这个解决方案是有问题的，因为我们开始使用 double s，这意味着我们会遇到精度错误。说明在比较双精度数时不应使用 ==。这是因为 Math.log10(n)/Math.log10(3) 的结果可能是 5.0000001 或 4.9999999。使用 Math.log() 函数而不是Math.log10() 可以观察到这种效果。
为了解决这个问题，我们需要将结果与 epsilon 进行比较。
```java
return (Math.log(n) / Math.log(3) + epsilon) % 1 <= 2 * epsilon;
```
复杂度分析
时间复杂度：UnknownUnknownUnknown。这里主要消耗时间的运算是 Math.log，它限制了我们算法的时间复杂性。实现依赖于我们使用的语言和编译器 。
空间复杂度： O(1)O(1)O(1)，我们没有使用任何额外的内存。epsilon 变量可以是内联的。

 - 方法四：整数限制
一个重要的信息可以从函数名中推导出来。
```java
public boolean isPowerOfThree(int n)
```
我们可以看出 n 的类型是  int。在 Java 中说明了该变量是四个字节，他的最大值为 2147483647。有三种方法可以计算出该最大值。

Google
System.out.println(Integer.MAX_VALUE);
MaxInt = 2322−1\frac{ 2^{32} }{2} - 12232​−1 ,因为我们使用 32 位来表示数字，所以范围的一半用于负数，0 是正数的一部分。

知道了 n 的限制，我们现在可以推断出 n 的最大值，也就是 3 的幂，是 1162261467。我们计算如下：
3⌊log⁡3MaxInt⌋=3⌊19.56⌋=319=11622614673^{\lfloor{}\log_3{MaxInt}\rfloor{}} = 3^{\lfloor{}19.56\rfloor{}} = 3^{19} = 1162261467
3⌊log3​MaxInt⌋=3⌊19.56⌋=319=1162261467
因此，我们应该返回 true 的 n 的可能值是 303^030，313^131…3193 ^ {19}319。因为 3 是质数，所以 3193^{19}319 的除数只有 303^030，313^131…3193 ^{19}319，因此我们只需要将 3193^{19}319 除以 n。若余数为 0 意味着 n 是 3193^{19}319 的除数，因此是 3 的幂。
```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```
复杂度分析
时间复杂度：
O(1)O(1)
O(1)。我们只做了一次操作。
空间复杂度： 
O(1)O(1)
O(1)，没有使用额外空间。
