## 《剑指offer》题解

> 《剑指Offer》第二版所有题目，共76道题目    题目来源：[Acwing](https://www.acwing.com/problem/search/1/?csrfmiddlewaretoken=S5lzkLHkKv13snNnUJUOL5GoHelfgamZ8nZzpmpLKjDjzHawX6aSvM1bd9dMw6f2&search_content=%E5%89%91%E6%8C%87Offer)
>

## 😜[简单题]

### 63. 字符串中第一个只出现一次的字符

```c
class Solution {
public:
    unordered_map<char, int> m;
    char firstNotRepeatingChar(string s) {
        char res = '#';
        for (auto c : s) m[c]++;
        
        for (auto c : s)
            if (m[c] == 1) 
                return c;
        
        return res;
    }
};
```

### 66. 两个链表的第一个公共结点   

> 思路：将两个链表同时向后移动一步，当到达终点时，指向另一链表的开头，这样最多遍历两次就能找到公共结点

```c
class Solution {
public:
    ListNode *findFirstCommonNode(ListNode *headA, ListNode *headB) {
        auto p = headA, q = headB;
        while (p != q) {
            if (p) p = p->next;
            else p = headB;
            if (q) q = q->next;
            else q = headA;
        }
        return q;
    }
};      
```

### 67.数字在排序数组中出现的次数   

> 思路：二分

```c
class Solution {
public:
    int getNumberOfK(vector<int>& nums , int k) {
        if (nums.empty()) return 0;
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < k) l = mid + 1;
            else r = mid;
        }
        
        int left = l;
        if (nums[l] != k) return 0;
        
        l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= k) l = mid;
            else r = mid - 1;
        }
        return r - left + 1;
    }
};
```

### 68. 0到n-1中缺失的数字

> 思路：二分  （当区间内的所有nums[i] == i时，则缺失的数字为n） 

```c
class Solution {
public:
    int getMissingNumber(vector<int>& nums) {
        if (nums.empty()) return 0;
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] != mid) r = mid;
            else l = mid + 1;
        }
        if (nums[r] == r) r++;
        return r;
    }
};
```

### 69.数组中数值和下标相等的元素     

> 思路：二分    

```c
class Solution {
public:
    int getNumberSameAsIndex(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] - mid >= 0) r = mid;
            else l = mid + 1;
        }
        if (nums[r] == r) return r;
        return -1;
    }
};
```

### 70.二叉搜索树的第k个结点

```c
class Solution {
public:
    TreeNode* res;
    TreeNode* kthNode(TreeNode* root, int k) {
        dfs(root, k);
        return res;
    }
    void dfs(TreeNode* root, int &k) {
        if (!root) return;
        dfs(root->left, k);
        k--;
        if (!k) res = root;
        if (k >= 0) 
            dfs(root->right, k);
    }
};
```

### 71.二叉树的深度

* 递归
```c
class Solution {
public:
    int treeDepth(TreeNode* root) {
        if (!root) return 0;
        int left = treeDepth(root->left);
        int right = treeDepth(root->right);
        return max(left, right) + 1;
    }
};
```

* 非递归

```c
class Solution {
public:
    int treeDepth(TreeNode* root) {
        if (!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int count = 0;
        while (!q.empty()) {
        int len = q.size();
        for (int i = 0; i < len; i++) {
        auto p = q.front();
        q.pop();
        if (p->left) q.push(p->left);
        if (p->right) q.push(p->right);
        }
        count++;
        }
        return count;
    }
};
```

  

### 72.平衡二叉树 

```c
class Solution {
public:
    bool ans = true;
    bool isBalanced(TreeNode* root) {
        dfs(root);
        return ans;
    }
    int dfs(TreeNode* root) {
        if (!root) return 0;
        int l = dfs(root->left);
        int r = dfs(root->right);
        if (abs(l - r) > 1) ans = false;
        return max(l, r) + 1;
    }
};
```

### 75. 和为S的两个数字

```c
class Solution {
public:
    vector<int> findNumbersWithSum(vector<int>& nums, int target) {
        unordered_set<int> hash;
        for (auto x: nums) {
            if (hash.count(target - x)) return {target - x, x};
            else
                hash.insert(x);
        }
        return {};
    }
};
```

### 78. 左旋转字符串

```c
class Solution {
public:
    string leftRotateString(string str, int n) {
        reverse(str.begin(), str.end());
        reverse(str.begin(), str.begin() + str.length() - n);
        reverse(str.begin() + str.length() - n, str.end());
        return str;
    }
};
```

### 80. 骰子的点数

### 81. 扑克牌的顺子

```c
class Solution {
public:
    bool isContinuous( vector<int> nums ) {
        if (!nums.size()) return false;
        sort(nums.begin(), nums.end());
        int k = 0;
        while (!nums[k]) k++;
        for (int i = k + 1; i < nums.size(); i++)
            if (nums[i] == nums[i - 1])
                return false;
        return nums.back() - nums[k] <= 4;
    }
};
```

### 82.圆圈中最后剩下的数字   

* 链表模拟

```c
  #include <list>
  class Solution {
  public:
      int lastRemaining(int n, int m){
           list<int> nums;
           for (int i = 0; i < n; i++) nums.push_back(i);
           auto it = nums.begin();
           int k = m - 1;
           while (nums.size() > 1) {
               while (k--) {
                  it++;
                  if (it == nums.end()) it = nums.begin();
               }
               it = nums.erase(it);
               if (it == nums.end()) it = nums.begin();
               k = m - 1;
           }
           return nums.front();
      }
  };
```

* 递归

```c
class Solution {
public:
    int lastRemaining(int n, int m){
       if (n == 1) return 0;
       return (lastRemaining(n - 1, m) + m) % n;
    }
};
```

### 83.股票的最大利润

```c
class Solution {
public:
    int maxDiff(vector<int>& nums) {
        if (!nums.size()) return 0;
        int res = 0;
        for (int i = 0, minv = nums[0]; i < nums.size(); i++) {
            res = max(res, nums[i] - minv);
            minv = min(minv, nums[i]);
        }
        return res;
    }
};
```

### 84. 求1+2+…+n

```c
class Solution {
public:
    int getSum(int n) {
        int res = n;
        n > 0 && (res += getSum(n - 1));
        return res;
    }
};
```

### 

