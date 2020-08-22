# 键值映射
***
#### 2020.08.22

### 问题
>实现一个 MapSum 类里的两个方法，insert 和 sum。           
对于方法 insert，你将得到一对（字符串，整数）的键值对。字符串表示键，整数表示值。如果键已经存在，那么原来的键值对将被替代成新的键值对。                      
对于方法 sum，你将得到一个表示前缀的字符串，你需要返回所有以该前缀开头的键的值的总和。                                       
                             
### 示例
```
输入: insert("apple", 3), 输出: Null               
输入: sum("ap"), 输出: 3                 
输入: insert("app", 2), 输出: Null            
输入: sum("ap"), 输出: 5                   
```

### 代码
```cpp
class MapSum {
public:
    /** Initialize your data structure here. */
    map<string,int>hash;
    MapSum() {

    }
    
    void insert(string key, int val) {
        hash[key]=val;

    }
    
    int sum(string prefix) {
        int ans=0;
        for(auto v:hash)
        {
            if(v.first.find(prefix)==0)
            ans += v.second;
        }
        return ans;
    }
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */
```
 
### 分析
  - 执行用时：8 ms, 在所有 C++ 提交中击败了37.33%的用户内存消耗：8.2 MB, 在所有 C++ 提交中击败了51.83%的用户。
  - 直接使用hash表+map只能说是一种非常取巧的方法，看到一些人用dfs和前缀树等方法还是觉得有很多地方有待发掘和学习。
  
### 优解
#### 1.前缀树
```c++
class MapSum {
private:
    bool isEnd; // 是否为最后一个字母
    MapSum* next[26]; // 字母表
    int value; // 若为最后一个字母，其对应的值
    
    // 深度优先遍历算法
    int dfs(MapSum* root) { 
        if(!root) return 0; // 递归基：如果当前访问的MapSum为空，则直接返回0
        
        int res = 0;
        if(root->isEnd) res += root->value; // 若当前节点不为空且isEnd，则加上其值
        for(MapSum* cur : root->next) { // 再遍历当前节点的next数组中所有的MapSum
            res += dfs(cur);
        } 

        return res;
    }
public:
    /** Initialize your data structure here. */
    MapSum() {
        isEnd = false;
        memset(next, 0, sizeof(next));
        value = 0;
    }
    
    void insert(string key, int val) {
        MapSum* node = this;
        for(char ch : key) {
            if(node->next[ch - 'a'] == NULL) {
                node->next[ch - 'a'] = new MapSum();
            }
            node = node->next[ch - 'a'];
        }
        node->isEnd = true;
        node->value = val; // 相比较正常的前缀树，只是新增了一个value属性
    }
    
    int sum(string prefix) {
        MapSum* node = this;
        for(char ch : prefix) {
            if(node->next[ch - 'a'] == NULL) return 0;
            node = node->next[ch - 'a'];
        }
        return dfs(node);
    }
};

作者：xiao-xiao-he-miao
链接：https://leetcode-cn.com/problems/map-sum-pairs/solution/qian-zhui-shu-si-xiang-by-xiao-xiao-he-miao/
来源：力扣（LeetCode）
```

#### 2.字典树+dfs
```c++
class Trie {
private:
    struct TrieNode {
        TrieNode *son[26];
        int num;
    };
    TrieNode *root = new TrieNode();

public:
    void insert(string s, int num) {
        auto node = root;
        for (int i =  0; i < s.size(); i ++) {
            auto t = s[i] - 'a';
            if (!(node -> son[t])) 
                node -> son[t] = new TrieNode();
            node = node -> son[t];
        }
        node -> num = num;
    }

    int find(string s) {
        TrieNode *node = root;
        int res = 0;
        for (int i = 0; i < s.size(); i ++) {
            auto t = s[i] - 'a';
            if (!node -> son[t]) return res;
            node = node -> son[t];
        }
        res += dfs(node);
        return res;
    }

    int dfs(TrieNode *node) {
        int res = 0;
        res += node -> num;
        for (int i = 0; i < 26; i ++) 
            if (node -> son[i]) 
                res += dfs(node -> son[i]);
        return res;
    }
};

class MapSum {
private: 
    Trie *trie = new Trie();
public:
    /** Initialize your data structure here. */
    MapSum() {
    }
    
    void insert(string key, int val) {
        trie -> insert(key, val);
    }
    
    int sum(string prefix) {
        return trie -> find(prefix);
    }
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */

作者：sjy001
链接：https://leetcode-cn.com/problems/map-sum-pairs/solution/c-trie-tree-by-jerrysu/
来源：力扣（LeetCode）
```

#### 3.unordered_map
```cpp
struct TrieNode
{
    bool end; // 标识字符串结尾 val是否有效
    int val;
    unordered_map<char, TrieNode*> map;

    TrieNode() : end(false) {}
};


class MapSum {
public:
    /** Initialize your data structure here. */
    MapSum() {

        m_root = new TrieNode;
    }

    void insert(string key, int val) {

        if (key.empty())
        {
            return;
        }

        TrieNode* cur = m_root;

        for (int i = 0; i < key.size(); ++i)
        {
            auto iter = cur->map.find(key.at(i));
            if (iter == cur->map.end())
            {
                cur->map[key.at(i)] = new TrieNode;
            }

            cur = cur->map[key.at(i)];
        }

        cur->end = true;
        cur->val = val;
    }

    int sum(string prefix) {

        int res = 0;

        if (prefix.empty())
        {
            return res;
        }

        TrieNode* cur = m_root;

        //1、先到达前缀处
        for (int i = 0; i < prefix.size(); ++i)
        {
            auto iter = cur->map.find(prefix.at(i));
            if (iter == cur->map.end())
            {
                return res;
            }

            cur = iter->second;
        }

        //2、所有以该前缀开头的 递归
        get_sum(cur, res);

        return res;
    }

private:

    void get_sum(TrieNode* node, int& res)
    {
        if (node == nullptr)
        {
            return;
        }

        if (node->end)
        {
            res += node->val;
        }

        for (const auto& p : node->map)
        {
            get_sum(p.second, res);
        }
    }


private:
    TrieNode* m_root;
};

作者：eric-345
链接：https://leetcode-cn.com/problems/map-sum-pairs/solution/677-jian-zhi-ying-she-by-eric-345/
来源：力扣（LeetCode）
```
