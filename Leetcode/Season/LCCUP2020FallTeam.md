## LCCUP2020秋季编程大赛团队赛

### [1. 黑白方格画](https://leetcode-cn.com/contest/season/2020-fall/problems/ccw6C7/)

**题解：**

数学题，使用c[i]记录组合数，从n中选i个，行选x格，列选y格，x和y满足

``x * (n - y) + y * n = k``

**代码：**

```C++
class Solution {
public:
    int paintingPlan(int n, int k) {
        if(k == n * n || k == 0) {
            return 1;
        } 
        vector<int> c(n, 1);
        for(int i = 1; i < n; i++) {
            c[i] = c[i - 1] * (n + 1 - i) / i;
        }
        int ans = 0;       
        for(int i = 0; i < n; i++) {
            int j = (k - i * n) / (n - i);
            if(j >= 0 && j * (n - i) == k - i * n) {
                ans += (c[i] * c[j]);
            }
        }
        return ans;
    }
};
```

### [2. 魔术排列](https://leetcode-cn.com/contest/season/2020-fall/problems/er94lq/)

**提示：**

可以直接通过target计算出k的值，然后进行模拟

**代码：**

```c++

class Solution {
public:
    bool isMagic(vector<int>& target) {
        int k = 0;
        int n = target.size();
        bool flag = 1;
        for(int i = 0; i < n; i++) {
            if(i < n / 2) {
                if(target[i] == 2 * (i + 1)) {
                    k++;
                } else {
                    flag = 0;
                    break;
                }
            } else {
                if(target[i] == 2 * (i - n / 2) + 1) {
                    k++;
                } else {
                    flag = 0;
                    break;
                }
            }
        }

        if(k == 0) return 0;
        vector<int> tmp(n);
        for(int i = 0; i < n; i++) {
            tmp[i] = i + 1;
        }
        vector<int> ans;
        vector<int> tmp1;
        int cnt = 0;
        while(ans.size() < n) {
            tmp1.clear();
            cnt = 0;
            for(int i = 1; i < tmp.size(); i+=2) {
                if(cnt < k) {
                    ans.push_back(tmp[i]);
                    cnt++;
                } else {
                    tmp1.emplace_back(tmp[i]);
                }
            }
            for(int i = 0; i < tmp.size(); i+=2) {
                if(cnt < k) {
                    ans.push_back(tmp[i]);
                    cnt++;
                } else {
                    tmp1.emplace_back(tmp[i]);
                }
            }
            tmp = tmp1;
        }
        return ans == target;
    }
};
```

### 3. 数字游戏

**提示：**



**代码：**

```
class Solution {
public:
    vector<int> numsGame(vector<int>& nums) {

    }
};
```

### 4. 古董键盘

**提示：**



**代码：**

```
class Solution {
public:
    int keyboard(int k, int n) {

    }
};
```

### 5. 导航装置

**提示：**

**代码：**

```
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
    int navigation(TreeNode* root) {

    }
};
```

### 6. 黑盒光线反射

> 

**提示：**

**代码：**

```
class BlackBox {
public:
    BlackBox(int n, int m) {

    }
    
    int open(int index, int direction) {

    }
    
    void close(int index) {

    }
};

/**
 * Your BlackBox object will be instantiated and called as such:
 * BlackBox* obj = new BlackBox(n, m);
 * int param_1 = obj->open(index,direction);
 * obj->close(index);
 */
```

