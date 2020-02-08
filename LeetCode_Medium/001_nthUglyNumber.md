# 丑数Ⅱ
***
#### 2020.02.08

### 问题
>编写一个程序，找出第 n 个丑数。
丑数就是只包含质因数 2, 3, 5 的正整数。

### 示例
>输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

>说明：
1.1 是丑数。
2.n 不超过1690。

### 代码
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int>res(n+1);
        res[1]=1;
        int index2=1,index3=1,index5=1;
        for(int i=2;i<=n;i++){
            res[i]=min(min(res[index2]*2,res[index3]*3),res[index5]*5);
            while(res[index2]*2<=res[i]) index2=index2+1;
            while(res[index3]*3<=res[i]) index3=index3+1;
            while(res[index5]*5<=res[i]) index5=index5+1;
        }
        return res[n];
        
    }
};
```

### 分析
 - 这道题也是我做的第一道中档题，初看题时觉得挺简单的，仅仅是在昨天“丑数Ⅰ”上的一个拓展，但如我所料真的就没这么简单，我依然是按我们的初级方法，选择了
   C语言，试图用循环暴力解出来，但还是超时了，代码如下。
 ```c
   int nthUglyNumber(int n){
    int num=1,i=0;
    for(num=1;i<n;num++){
        int text=num;
        while(1){
            if(text==1){
                i=i+1;
                break;   
            }
            else if(text%2==0){
                text=text/2;
                continue;
            }
            else if(text%3==0){
                text=text/3;
                continue;
            }
            else if(text%5==0){
                text=text/5;
                continue;
            }
            else
            break;
        }
    }
    return num-1;
 }
 ```
 
  当运行到第1352时就不行了。很显然暴力法行不通，只能寻求其他的方法了。
- 查看了网友们各式各样的解法，使用C语言的真的是极少啊。唯一找到的一个自称为“新手小白”C用户使用的是三队列的方法，代码还是有一点复杂的，由于对队列
  的认识也仅仅停留在初级阶段，所以我也开始尝试网友们所说的c++的方法。至于队列，看博客上的解析则需要从数据结构其存储结构进行理解。看了之后对其运行
  模式上有了了解，但对实际运用上还是不太懂。还是需要从习题中了解练习了。https://www.cnblogs.com/kubixuesheng/p/4104802.html
```c
  首先，建立三个队列，作为2、3、5的待乘队列，各自插入元素 1 ；
  每一轮操作时，计算三个队列的首位元素与各自乘数的积，取三者的最小值作为第 i 个丑数，并把这个最小值插入到三个队列的队尾;
  同时，对被取到最小值的队列进行弹出操作；如此循环 n-1 次，可以计算出第n个丑数。
  int nthUglyNumber(int n)
{
// 计算三个数中的最小值
int Min(int a, int b, int c)
{
int result = a;
if (b < result)
result = b;
if (c < result)
result = c;
return result;
};
// 定义队列结点
struct Node
{
    int Value;
    struct Node *Next;
};

// 定义队列结构体，包含首位元素与乘数的乘积（动态更新）、队列头指针、队尾指针
struct Queue
{
    int Front;
    struct Node *Header;
    struct Node *Tail;
};

// 队列初始化，将 1 插入队列中
void InitQueue(struct Queue *myQueue, int multiplier)
{
    struct Node *myHeader = malloc(sizeof(struct Node));
    struct Node *myTail = malloc(sizeof(struct Node));
    myHeader->Value = 0;
    myHeader->Next  = myTail;
    myTail->Value   = 1;
    myTail->Next    = NULL;
    myQueue->Front  = multiplier;
    myQueue->Header = myHeader;
    myQueue->Tail   = myTail;
};

// 队列的插入操作
void Push(struct Queue *myQueue, int value)
{
    struct Node *temp = malloc(sizeof(struct Node));
    temp->Value = value;
    temp->Next  = NULL;
    myQueue->Tail->Next = temp;
    myQueue->Tail = temp;
};

// 队列的弹出操作
void Pop(struct Queue *myQueue, int multiplier)
{
    if (myQueue->Front != 0)
    {
        struct Node *temp = myQueue->Header->Next;
        myQueue->Header->Next = temp->Next;
        if (temp->Next == NULL)
            myQueue->Front = 0;
        else
            myQueue->Front  = temp->Next->Value * multiplier;
        free(temp);
    }
};

// 定义三个待乘队列
struct Queue *myQueue2 = malloc(sizeof(struct Queue));
struct Queue *myQueue3 = malloc(sizeof(struct Queue));
struct Queue *myQueue5 = malloc(sizeof(struct Queue));

// 三个队列初始化
InitQueue(myQueue2, 2);
InitQueue(myQueue3, 3);
InitQueue(myQueue5, 5);

int count  = 1;  // 计数器
int result = 1;  // 待返回的结果

while (count < n)
{
    result = Min(myQueue2->Front, myQueue3->Front, myQueue5->Front);
    count++;
    Push(myQueue2, result);
    Push(myQueue3, result);
    Push(myQueue5, result);
    if (myQueue2->Front == result)
        Pop(myQueue2, 2);
    if (myQueue3->Front == result)
        Pop(myQueue3, 3);
    if (myQueue5->Front == result)
        Pop(myQueue5, 5);
}
return result;
};
作者：yi-cuo-dou
```
- 而c++也是主要有暴力求解、优先队列和动态规划三种解法。我看了几种解法，认为动态规划是我现有程度下比较好理解的。一个答主也是从数学的规律中巧妙运用动态
  规划解出。对动态规划的理解上主要参考了这一篇，我认为总结得非常清晰也好理解：https://blog.csdn.net/misayaaaaa/article/details/71794620 同时也
  找到了相关得比较基础得典型问题，发现原来遇到得台阶问题、最短路径等都可以用动态规划得思想解决，也算是有很大收获了 
  https://blog.csdn.net/misayaaaaa/article/details/71940779
- 而网友提供得另两种也贴上以供学习。我也查找了相关算法的说明，对其有了基本的了解。
- 其中的暴力解法的思想和我的C是差不多的，但他之所以能行得通高明在其经验比我更为丰富，体现在对数据类型的定义上，对内核中栈、堆的理解上，以及较大数据的
  处理上避免了溢出。
  至于优先队列即其所谓的小顶堆，说明如下：https://blog.csdn.net/weixin_36888577/article/details/79937886 也算是有了初步的认识吧。
  
  1.暴力(brute force)
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> v;
        for (long long a=1;a<=INT_MAX;a=a*2)
            for (long long b=a;b<=INT_MAX;b=b*3)
                for (long long c=b;c<=INT_MAX;c=c*5)
                    v.push_back(c);
        sort(v.begin(),v.end());
        return v.at(n-1);
    }
};
```
2.优先队列(小顶堆)
优先队列/小顶堆/大顶堆
利用优先队列有自动排序的功能
每次取出队头元素，存入队头元素*2、队头元素*3、队头元素*5
但注意，像12这个元素，可由4乘3得到，也可由6乘2得到，所以要注意去重
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        priority_queue <double,vector<double>,greater<double> > q;
        double answer=1;
        for (int i=1;i<n;++i)
        {
            q.push(answer*2);
            q.push(answer*3);
            q.push(answer*5);
            answer=q.top();
            q.pop();
            while (!q.empty() && answer==q.top())
                q.pop();
        }
        return answer;
    }
};
```
还可以更进一步采用set来识别有无重复
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        priority_queue <double,vector<double>,greater<double> > q;
        set<int> s;
        s.insert(1);
        vector<int> mask({2,3,5});
        double answer=1;
        for (int i=1;i<n;++i)
        {
            for (int &j:mask)
                if (s.count(answer*j)==0)
                {
                    q.push(answer*j);
                    s.insert(answer*j);
                }
            answer=q.top();
            q.pop();
        }
        return answer;
    }
};
```
3.动态规划(三指针)
我们先模拟手写丑数的过程
1打头，1乘2 1乘3 1乘5，现在是{1,2,3,5}
轮到2，2乘2 2乘3 2乘5，现在是{1,2,3,4,5,6,10}
手写的过程和采用小顶堆的方法很像，但是怎么做到提前排序呢

小顶堆的方法是先存再排，dp的方法则是先排再存
我们设3个指针p_2,p_3,p_5
代表的是第几个数的2倍、第几个数3倍、第几个数5倍
动态方程dp[i]=min(dp[p_2]*2,dp[p_3]*3,dp[p_5]*5)
小顶堆是一个元素出来然后存3个元素
动态规划则是标识3个元素，通过比较他们的2倍、3倍、5倍的大小，来一个一个存
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> dp(n);
        dp.at(0)=1;
        int p_2,p_3,p_5;
        p_2=p_3=p_5=0;
        for (int i=1;i<n;++i)
        {
            dp.at(i)=min(min(2*dp.at(p_2),3*dp.at(p_3)),5*dp.at(p_5));
            if (dp.at(i)==2*dp.at(p_2))
                ++p_2;
            if (dp.at(i)==3*dp.at(p_3))
                ++p_3;
            if (dp.at(i)==5*dp.at(p_5))
                ++p_5;
        }
        return dp.at(n-1);
    }
};

作者：LZH_Yves
```
