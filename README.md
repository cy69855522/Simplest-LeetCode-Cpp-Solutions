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

## [116. Populating Next Right Pointers in Each Node 递归](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
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
## [117. Populating Next Right Pointers in Each Node II 递归](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
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
        if(root and (root->left or root->right)){
            if(root->left and root->right) root->left->next = root->right;
            
            Node *node = root->right ? root->right : root->left;
            Node *head = root->next;
            while (head and not (head->left or head->right)){
                head = head->next;
            }
            node->next = head ? (head->left ? head->left : head->right) : nullptr;
            
            connect(root->right);
            connect(root->left);
        }
        return root;
    }
};
```
- 对于任意一次递归，只考虑如何设置子节点的 next 属性,分为三种情况：
	- 没有子节点：直接返回
	- 有一个子节点：将这个子节点的 `next` 属性设置为同层的下一个节点，即为 `root.next` 的最左边的一个节点，如果 `root.next` 没有子节点，则考虑 `root.next.next`，依次类推
	- 有两个节点：左子节点指向右子节点，然后右子节点同第二种情况的做法
- 注意递归的顺序需要从右到左
## [236. Lowest Common Ancestor of a Binary Tree 递归](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode *l = root->left ? lowestCommonAncestor(root->left, p, q) : nullptr;
        TreeNode *r = root->right ? lowestCommonAncestor(root->right, p, q) : nullptr;
        return (root == p or root == q or (l and r)) ? root : l ? l : r;
    }
};
```
- 递归全部节点，p 的祖先节点全部返回 p，q 的祖先节点全部返回 q，如果它同时是俩个节点的最近祖先，那么返回自身，否则返回 nullptr
## [461. Hamming Distance 异或](https://leetcode.com/problems/hamming-distance/submissions/)
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return bitset<32>(x ^ y).count();
    }
};
```
## [1108. Defanging an IP Address 逆遍历](https://leetcode.com/problems/defanging-an-ip-address/)
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
