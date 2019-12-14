# 👻 Simplest-LeetCode-Cpp-Solutions
Leet Code 刷题笔记 - - 不求最快最省，但求最简最优雅 ✒，Simpler is better here.

# 前言
- 如有更简解法，欢迎 fork 或 issue。
- 为了快速找到题目可以按 [**Ctrl键 + F键**] 输入题目序号或名字定位。
- 欢迎加入**QQ交流群**：902025048 [∷二维码](QR.png) 群内提供更多相关资料~

# 推荐
- 🐍 [ LeetCode最短Python题解 ](https://github.com/cy69855522/Shortest-LeetCode-Python-Solutions)
- 🐟 [ 初级算法 ](#初级算法)

# 题库解析
此专栏追求代码的**精简**和**技巧性**，默认已看过题目，🤖 没看过的话点标题可以跳转链接，一起体验炫酷的 C++

## [13. Roman to Integer 哈希表](https://leetcode.com/problems/roman-to-integer/)
```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<string, int> m = {{"I", 1}, {"IV", 3}, {"IX", 8}, {"V", 5}, {"X", 10}, {"XL", 30}, {"XC", 80}, {"L", 50}, {"C", 100}, {"CD", 300}, {"CM", 800}, {"D", 500}, {"M", 1000}};
        
        int r = m[s.substr(0, 1)];
        for(int i=1; i<s.size(); ++i){
            string two = s.substr(i-1, 2);
            string one = s.substr(i, 1);
            r += m[two] ? m[two] : m[one];
        }
        return r;
    }
};
```
- 构建一个哈希表记录所有罗马数字子串，注意长度为2的子串记录的值是（实际值-子串内左边罗马数字代表的数值）
- 遍历整个s的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值。整个过程时间复杂度为O(N)
- 举个例子，遍历经过IV的时候先记录I的对应值1再往前移动一步记录IV的值3，加起来正好是IV的真实值4
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
### [206. Reverse Linked List 双指针](https://leetcode.com/problems/reverse-linked-list/)
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
    ListNode* reverseList(ListNode* head) {
        ListNode *pre = NULL;
        while(head){
            ListNode *next = head -> next;
            head -> next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```
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
## [238. Product of Array Except Self 双指针](https://leetcode.com/problems/product-of-array-except-self/)
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n, 1);
        
        int l = 1;
        for(int i=0; i<n; ++i){
            res[i] *= l;
            l *= nums[i];
        }
        
        int r = 1;
        for(int j=n-1; j>=0; --j){
            res[j] *= r;
            r *= nums[j];
        }
        
        return res;
    }
};
```
- 本题利用双指针，新数组每个位置上的值应该等于数组左边所有数字的乘积 × 数组右边所有数字的乘积
- 1.初始化一个新的数组res（result），包含n个1
  2.初始化变量l（left）代表左边的乘积，从左到右遍历数组，每次都让新数组的值乘以它左边数字的乘积l，然后更新l。此时新数组里的所有数字就代表了nums数组中对应位置左边所有数字的乘积
  3.再从右往左做一遍同样的操作，最终`res[i] = 1 * nums中i左边所有数字的乘积 * nums中i右边所有数字的乘积`
## [268. Missing Number 数学](https://leetcode.com/problems/missing-number/)
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        
        int sum = 0;
        for(int i=0; i<n; ++i){
            sum += nums[i];
        }
        
        n++;
        return n * (n - 1) / 2 - sum;
    }
};
```
- 缺失数字 = 0 加到 n+1 的总和 - 数组中所有数字的总和
- 计算 0 加到 n+1 的总和，可利用等差数列求和公式，此题可理解为`总和 = (元素个数 / 2) * (首尾两数字之和)`
## [344. Reverse String 双指针](https://leetcode.com/problems/reverse-string/)
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = -1, j = s.size();
        while(++i < --j){
            swap(s[i], s[j]);
        }
    }
};
```
### [448. Find All Numbers Disappeared in an Array 哈希表](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(int i=0; i<nums.size(); ++i){
            nums[abs(nums[i])-1] = -abs(nums[abs(nums[i])-1]);
        }
        
        vector<int> r;
        for(int i=0; i<nums.size(); ++i){
            if(nums[i] > 0){
                r.push_back(i+1);
            }
        }
        return r;
    }
};
```
- 应题目进阶要求，O(N)时间，无额外空间，此解实际上是利用索引把数组自身当作哈希表处理
- 将 nums 中所有正数作为索引i，置 nums[i] 为负值。那么，仍为正数的位置即为（未出现过）消失的数字
    - 原始数组：[4,3,2,7,8,2,3,1]
    - 重置后为：[-4,-3,-2,-7,`8`,`2`,-3,-1]
    - 结论：[8,2] 分别对应的index为[5,6]（消失的数字）
## [461. Hamming Distance 异或](https://leetcode.com/problems/hamming-distance/submissions/)
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return bitset<32>(x ^ y).count();
    }
};
```
## [709. To Lower Case ASCII码](https://leetcode.com/problems/to-lower-case/)
```cpp
class Solution {
public:
    string toLowerCase(string str) {
        for(auto& c: str) c = c < 'a' ? c + 32 : c;
        return str;
    }
};
```
- 利用 ASCII 修改字符
## [973. K Closest Points to Origin 快速选择](https://leetcode.com/problems/k-closest-points-to-origin/)
```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        int p = pow(points[0][0], 2) + pow(points[0][1], 2);
        vector<vector<int>> l, m, r;
        for(int i=0; i<points.size(); ++i){
            int v = pow(points[i][0], 2) + pow(points[i][1], 2);
            if(v < p){
                l.push_back(points[i]);
            }else if(v == p){
                m.push_back(points[i]);
            }else{
                r.push_back(points[i]);
            }
        }
        
        if(K <= l.size()){
            return kClosest(l, K);
        }else if(K <= l.size() + m.size()){
            l.insert(l.end(), m.begin(), m.begin() + K - l.size());
            return l;
        }else{
            r = kClosest(r, K - l.size() - m.size());
            l.insert(l.end(), m.begin(), m.end());
            l.insert(l.end(), r.begin(), r.end());
            return l;
        }
    }
};
```
- 快速选择的一般流程，计算lmr，组合
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
# 探索
## 初级算法
### 数组
#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        return nums.size();
    }
};
```
- `unique` 函数可以将有序向量的重复项移至末尾，如 `1123445` → `1234514`，并返回**指向去重后容器中不重复序列的最后一个元素的下一个元素的迭代器**
#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        for(int i = 0; i + 1 < prices.size(); ++i) profit += max(prices[i + 1] - prices[i], 0);
        return profit;
    }
};
```
- 每次买完股票，第二天就出售掉
- 只要明天股票价格比今天高，今天就买入
- 买入和出售可以发生在同一天
#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        std::rotate(nums.begin(), nums.end() - k % nums.size(), nums.end());
    }
};
```
- 直接调用<algrorithm>中的函数，函数原型可参考[这篇博客](https://blog.csdn.net/li1615882553/article/details/83546763)
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        reverse(nums.begin(), nums.end() - k % nums.size());
        reverse(nums.end() - k % nums.size(), nums.end());
        reverse(nums.begin(), nums.end());
    }
};
```
- 三重反转
- 把一个数组的右边一部分移动到左边相当于：
	- 把左部分翻转
	- 把右部分翻转
	- 最后把整体翻转
#### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)
```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set s(nums.begin(), nums.end());
        return s.size() != nums.size();
    }
};
```
- 利用集合排除重复元素，然后对比尺寸
#### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        return accumulate(begin(nums), end(nums), 0, bit_xor<int>());
    }
};
```
- 这里用到了异或（xor），就是把数字的二进制形式按位计算，相同为1不同为0，然后返回位运算后的二进制代表的数字，那么相同的数字异或后为`0`
- `0`异或任何数都等于那个数，所以用`0`做起始值不会对结果有影响
- `accumulate`函数对整个序列进行递进式计算。每次计算都使用函数`bit_xor<int>`，以上次累积结果和当前数字作为参数，并将计算结果用于下次计算，有点递归的意思
#### [350. 两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)
```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& a, vector<int>& b) {
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        a.erase(set_intersection(a.begin(), a.end(), b.begin(), b.end(), a.begin()), a.end());
        return a;
    }
};
```
- 直接将两个排序后的向量视为集合，取`{x|x∈a∧x∈b}`即可
#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)
```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i=digits.size()-1, carry=1; i>-1; digits[i--] %= 10){
            digits[i] = digits[i] + carry;
            carry = digits[i] > 9;
        }
        if(!digits[0]) digits.insert(digits.begin(), 1);
        return digits;
    }
};
```
- O(N)时间 O(1)额外空间
- 从向量末尾开始逐个` + carry`，并计算是否进位
- 如果`for`循环之后`vector`首位为`0`，则代表最后一次计算需要进位
#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        for(int i=0, j=0; j<nums.size(); ++j) if(nums[j]) swap(nums[j], nums[i++]);
    }
};
```
- 指针`i`指向下一个不为`0`的数字应该出现的位置
- 指针`j`指向当前遍历到的数字
- 当`nums[j]`不为`0`，则将其与`nums[i]`交换，因为`j`是从左开始遍历，故保持了相对顺序
#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        for(int i = 0; i < nums.size(); m[target - nums[i - 1]] = i++){
            if(m.count(nums[i])) return {m[nums[i]], i};
        }
        return {-1, -1};
    }
};
```
- 哈希表解法，时间复杂度O(N)
- 遍历数组，每次都记录：`哈希表[target - 当前数字] = 当前数字的索引`，那么在往后的遍历中如果遇到map的键值包含当前数字的情况，说明之前的某个数字可以和当前数字加和为target，返回俩个数字的索引即可
- 写法上需要清楚C++中for循环的执行顺序：
    - ①初始化i为0
    - ②判断i是否小于数组长度，满足条件执行③，否则跳出循环
    - ③执行for语句对应的`{}`中的代码
    - ④执行`m[target - nums[i - 1]] = i++`
        - a.执行右边的表达式`i++`，记录结果为当前的`i`所拥有的值，并使得此时变量`i`的值递增一步
        - b.计算`target - nums[i - 1]`，此时`i`的值为`用于赋值的值 + 1`，所以`i - 1 = 用于赋值的值`
        - c.写入哈希表
        - 此行代码等同于`m[target - nums[i]] = i; i++;`
#### [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)
```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<unordered_set<int>> row(9), col(9), block(9);
        for(int i = 0; i < 9; ++i){
            for(int j = 0; j < 9; ++j){
                char cur = board[i][j];
                if(cur != '.'){
                    if(row[i].count(cur) | col[j].count(cur) | block[(i/3)*3 + j/3].count(cur))  return false;
                    row[i].insert(cur);
                    col[j].insert(cur);
                    block[(i/3)*3 + j/3].insert(cur);
                }
            }
        }
        return true;
    }
};
```
- 利用集合判断是否重复
#### [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        for (int i=0; i<matrix.size(); i++)
            for (int j=0; j<i; j++) swap(matrix[i][j], matrix[j][i]);
        
        for (auto& row: matrix) reverse(row.begin(), row.end());
    }
};
```
- 时间 O(N^2) 空间 O(1)
- 前俩行转置矩阵
- 最后一行做左右镜像
### 字符串
#### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)
```python
class Solution {
public:
    void reverseString(vector<char>& s) {
        reverse(s.begin(), s.end());
    }
};
```
#### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)
```cpp
class Solution {
public:
    int reverse(int x) {
        string s, m = x > 0 ? "2147483647" : "-2147483648";
        for(s = x >= 0 ? "" : "-"; x; x/=10) s += to_string(abs(x % 10));
        return s.size() == m.size() and s > m or s == "" ? 0 : stoi(s);
    }
};
```
- 先把数字反转并转为字符串`s`
- 字符串的比较结果 等价于 等长正整数的比较结果，我们可以利用这一点直接判断溢出
