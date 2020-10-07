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

**题解：**

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

### [3. 数字游戏](https://leetcode-cn.com/contest/season/2020-fall/problems/5TxKeK/)

**题解：**

`nums[a]+1==nums[a+1]`可以转换为`nums[a]-a==nums[a+1]-(a+1)`

题意即使nums[i] - i 后的数组，变成全部元素相等最少需要的步数，元素相等且最少步数，既等于中位数；

求最小中位数可以使用两个优先队列，大顶堆存储较小的部分，小顶堆存储较大的部分；

默认较小的部分多一个元素，防止数据流为奇数个；

**代码：**

```c++
class Solution {
public:
    const int mod = 1e9 + 7;
    typedef long long ll;
    
    vector<int> numsGame(vector<int>& nums) {
        int n = nums.size();
        if(n == 1) {
            return {0};
        }
        priority_queue<int> low;
        priority_queue<int, vector<int>, greater<int>> high;
        ll lowSum = 0, highSum = 0;
        vector<int> ans(n);

        for(int i = 0; i < n; i++) {
            nums[i] -= i;
        }

        for(int i = 0; i < n; i++) {
            low.push(nums[i]);
            lowSum += nums[i];
            high.push(low.top());
            highSum += low.top();
            lowSum -= low.top();
            low.pop();
            if(high.size() > low.size()) {
                low.push(high.top());
                lowSum += high.top();
                highSum -= high.top();
                high.pop();
            }
            int mid;
            if(low.size() > high.size()) {
                mid = low.top();
            } else {
                mid = (low.top() + high.top()) / 2;
            }
            ll t = mid * low.size() - lowSum + highSum - mid * high.size();
            ans[i] = t % mod;
        }
        return ans;
    }
};
```

### [4. 古董键盘](https://leetcode-cn.com/contest/season/2020-fall/problems/Uh984O/)

**题解：**

分组背包问题，26组，每组有k次可选，求装满背包容量n的方案；

`dp[i][j]`表示使用了 i 个字母，构成序列长度为 j 的可能方案个数；

转移方程：使用第i个字母，往前i - 1个字母中所有可能插入c个该字母

`dp[i][j] = dp[i - 1][j - c] * comb[j][c]`

注意：使用long long存储组合数的时候也可能溢出；

**代码：**

```c++
class Solution {
public:
    const int mod = 1e9 + 7;
    typedef long long ll;

    int keyboard(int k, int n) {
        vector<vector<ll>> comb(n + 1, vector<ll>(n + 1));
        comb[0][0] = 1;
        for(int i = 1; i <= n; i++) {
            comb[i][0] = 1;
            for(int j = 1; j <= i; j++) {
                comb[i][j] = comb[i - 1][j] + comb[i - 1][j - 1];
                comb[i][j] %= mod;
            }
        }

        vector<vector<ll>> dp(27, vector<ll>(n + 1, 0));
        for(int i = 0; i <= 26; i++) {
            dp[i][0] = 1;
        }
        for(int i = 1; i <= 26; i++) {
            for(int j = 1; j <= n; j++) {
                for(int c = 0; c <= k; c++) {
                    if(j < c) continue;
                    dp[i][j] += dp[i - 1][j - c] * comb[j][c];
                }
                dp[i][j] %= mod;
            }
        }
        return dp[26][n];
    }
};
```

### [5. 导航装置](https://leetcode-cn.com/contest/season/2020-fall/problems/hSRGyL/)

**题解：**

树形dp  [参考其他大佬的题解](https://leetcode-cn.com/problems/hSRGyL/solution/rang-ren-tou-tu-de-shu-xing-dong-tai-gui-hua-by-he/)

**代码：**

```c++
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
    vector<int> dfs(TreeNode* root) {
        if(!root) return {0, 0, 0, 0};
        if(!root->left && !root->right) return {0, 1, 1, 1};
        vector<int> ans(4);
        if(root->left && root->right) {
            vector<int> l = dfs(root->left);
            vector<int> r = dfs(root->right);
            ans[3] = min(l[1] + r[3], min(l[3] + r[1], l[2] + r[2]));
            ans[2] = min(ans[3], min(l[0] + r[2], l[2] + r[0]));
            ans[1] = min(ans[3], min(l[0] + r[3], l[3] + r[0]));
            ans[0] = min(min(ans[1], ans[2]), min(l[0] + r[2], l[2] + r[0]));
        } else {
            ans = root->left ? dfs(root->left) : dfs(root->right);
            ans[1] = min(ans[0] + 1, ans[3]);
        }
        return ans;
    }

    int navigation(TreeNode* root) {
        vector<int> ans = dfs(root);
        return ans[1];
    }
};
```

### [6. 黑盒光线反射](https://leetcode-cn.com/contest/season/2020-fall/problems/IQvJ9i/)

**题解：**

预处理出所有的循环 [参考其他大佬的题解](https://leetcode-cn.com/problems/IQvJ9i/solution/yu-chu-li-chu-suo-you-de-zu-by-zerotrac2/)

**代码：**

```
class BlackBox {
private:
    // 存储从每个小孔以 y=x 方向射出时，所在的循环的 id 以及在循环中的 id
    vector<pair<int, int>> groupPos;
    // 存储从每个小孔以 y=-x 方向射出时，所在的循环的 id 以及在循环中的 id
    vector<pair<int, int>> groupNeg;
    // 存储每个循环的有序映射
    vector<map<int, int>> groupStats;

public:
    BlackBox(int n, int m) {
        int ptCount = (n + m) * 2;
        groupPos.assign(ptCount, {-1, -1});
        groupNeg.assign(ptCount, {-1, -1});
        for (int i = 0; i < ptCount; ++i) {
            // 如果不是左上角或者右下角的小孔，那么从 y=x 方向射出找循环
            if (i != 0 && i != m + n && groupPos[i].first == -1) {
                createGroup(n, m, i, 1);
            }
            // 如果不是左下角或者右上角的小孔，那么从 y=-x 方向射出找循环
            if (i != m && i != m * 2 + n && groupNeg[i].first == -1) {
                createGroup(n, m, i, -1);
            }
        }
    }

    void createGroup(int n, int m, int index, int direction) {
        int groupId = groupStats.size();
        int groupLoc = 0;
        groupStats.emplace_back();
        // 不断模拟光线的路径，直到走到一个已经遇见过的状态，这样就找到了一个循环
        while (!(direction == 1 && groupPos[index].first != -1) && !(direction == -1 && groupNeg[index].first != -1)) {
            if (direction == 1) {
                groupPos[index] = {groupId, groupLoc++};
                index = (n + m) * 2 - index;
            }
            else {
                groupNeg[index] = {groupId, groupLoc++};
                if (index <= m * 2) {
                    index = m * 2 - index;
                }
                else {
                    index = (m * 2 + n) * 2 - index;
                }
            }
            // 如果小孔不在角上，就改变方向
            if (index != 0 && index != m && index != m + n && index != m * 2 + n) {
                direction = -direction;
            }
        }
    }
    
    int open(int index, int direction) {
        // 插入二元组
        if (auto [groupId, groupLoc] = groupPos[index]; groupId != -1) {
            groupStats[groupId].emplace(groupLoc, index);
        }
        if (auto [groupId, groupLoc] = groupNeg[index]; groupId != -1) {
            groupStats[groupId].emplace(groupLoc, index);
        }

        // 查询
        auto [groupId, groupLoc] = (direction == 1 ? groupPos[index] : groupNeg[index]);
        auto& store = groupStats[groupId];
        if (auto iter = store.upper_bound(groupLoc); iter != store.end()) {
            return iter->second;
        }
        else {
            return store.begin()->second;
        }
    }
    
    void close(int index) {
        // 删除二元组
        if (auto [groupId, groupLoc] = groupPos[index]; groupId != -1) {
            groupStats[groupId].erase(groupLoc);
        }
        if (auto [groupId, groupLoc] = groupNeg[index]; groupId != -1) {
            groupStats[groupId].erase(groupLoc);
        }
    }
};

```

