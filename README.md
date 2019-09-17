# 🐱‍👤 Simplest-LeetCode-Cpp-Solutions
Leet Code 刷题笔记 - - 不求最快最省，但求最简最优雅 ✒，Simpler is better here.

# 前言
- 如有更简解法，欢迎 fork 或 issue。
- 为了快速找到题目可以按 [**Ctrl键 + F键**] 输入题目序号或名字定位。
- 欢迎加入**QQ交流群**：902025048 [∷二维码](QR.png) 群内提供更多相关资料~

# 推荐
- 🐍 [ LeetCode最短Python题解 ](https://github.com/cy69855522/Shortest-LeetCode-Python-Solutions)

# 题库解析
此专栏追求代码的**精简**和**技巧性**，默认已看过题目，🤖 没看过的话点标题可以跳转链接，一起体验炫酷的 C++

## [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() {}

    Node(int _val, Node* _left, Node* _right, Node* _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
class Solution {
public:
    Node* connect(Node* root) {
        if(root and root->left){
            root->left->next = root->right;
            
            if(root->next) root->right->next = root->next->left;
            
            connect(root->left);
            connect(root->right);
        }
        return root;
    }
};
```
- 对于任意一次递归，只需要考虑如何设置子节点的 next 属性：
	- 将左子节点连接到右子节点
	- 将右子节点连接到 `root.next` 的左子节点
	- 递归左右节点
## [1108. Defanging an IP Address](https://leetcode.com/problems/defanging-an-ip-address/)
```cpp
class Solution {
public:
    string defangIPaddr(string address) {
        for(int i = address.size(); i >= 0; i--){
            if(address[i] == '.'){
                address.replace(i, 1, "[.]");
            }
        }
        return address;
    }
};
```
- 逆遍历字符串并替换 `.` 为 `[.]`
- 本题若正遍历，每次替换完，下一个字符会变成 `.`，进入死循环
