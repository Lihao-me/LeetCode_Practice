# 二进制链表转整数
***
#### 2020.07.25

### 问题
>给你一个单链表的引用结点 head。链表中每个结点的值不是 0 就是 1。已知此链表是一个整数数字的二进制表示形式。
请你返回该链表所表示数字的 十进制值 。

### 示例
![图片](https://github.com/Lihao-me/Pictures/blob/master/graph-1.png)
>输入：head = [1,0,1]           
输出：5                         
解释：二进制数 (101) 转化为十进制数 (5)                                           

>输入：head = [0]                  
输出：0                     

>输入：head = [1]                                  
输出：1                            

>输入：head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]                 
输出：18880                  
                   
>输入：head = [0,0]                 
输出：0                               

### 提示               
>	链表不为空。                     
	链表的结点总数不超过 30。                        
	每个结点的值不是 0 就是 1。                  

### 代码
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int getDecimalValue(ListNode* head) {
        ListNode* temp=head;
        int res=0;
        while(temp!=nullptr)
        {
            res=res*2+temp->val;
            temp=temp->next;
        }
        return res;
    }
};
```

### 分析
 - 执行用时：4 ms, 在所有 C++ 提交中击败了51.31%的用户内存消耗：8.2 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 本题的标签虽然是链表，但是主要还是二进制数转化为十进制数的问题。我的解题方法和0ms范例也就差在位运算上。不得不承认，对位运算的掌握还不是很好。

### 优解
#### 1.位运算(0ms范例)
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int getDecimalValue(ListNode* head) {
        int sum = 0;
        ListNode *cur = head;
        while(cur != NULL)
        {
            sum = (sum << 1) + cur->val;
            cur = cur->next;
        }
        return sum;
    }
};
```

#### 2.简单思路
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int getDecimalValue(ListNode* head) {
        int num[30];
        int p=0;
        int res=0;
//使用一个数组先将数取出，p记录长度
        for(;head!=0;++p){           
                num[p]=head->val;
                head=head->next;
        }
        int m=p;
//将二进制转换为10进制，遍历数组，=1的位置 用 2的m-1 次方转换为10进制
        for(;m>0;m--){
//p-m是从num[0]开始遍历
            if(num[p-m]==1){
                res=res+pow(2,m-1);

            }
        }
        return res;
    }
};

作者：feng-xie-1i
```
