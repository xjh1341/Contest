## LCCUP2020秋季编程大赛个人赛

[比赛链接](https://leetcode-cn.com/contest/season/2020-fall/problems/nGK0Fy/)

### 1. 速算机器人

**题解：**

直接模拟

**代码：**

```c++
class Solution {
public:
    int calculate(string s) {
        int x = 1, y = 0;
        for(int i = 0; i < s.size(); i++) {
            if(s[i] == 'A') {
                x = 2 * x + y;
            } else {
                y = 2 * y + x;
            }
        }
        return x + y;
    }
};
```

### 2. 早餐组合

**题解：**

使用二分计算出每个主食能搭配的饮料个数

**代码：**

```c++
class Solution {
public:
    const int mod = 1e9 + 7;
    
    int breakfastNumber(vector<int>& staple, vector<int>& drinks, int x) {
        sort(drinks.begin(), drinks.end());
        int n = drinks.size();
        long long sum = 0;
        for(int i = 0; i < staple.size(); i++) {
            int left = x - staple[i];
            int l = 0, r = n - 1;
            while(l <= r) {
                int mid = (r + l) / 2;
                if(drinks[mid] <= left) {
                    l = mid + 1;
                } else if(drinks[mid] > left) {
                    r = mid - 1;
                }
            }
            sum += l;
            sum %= mod;
        }
        return sum;
    }
};
```

### 3. 秋叶收藏集

**题解：**

dp,红黄蓝三种状态分别用012表示

**代码：**

```c++
class Solution {
public:
    int minimumOperations(string leaves) {
        int n = leaves.size();
        vector<vector<int>> dp(3, vector<int>(n, 0));

        for(int i = 0; i < n; i++) {
            if(i < 1) {
                dp[0][i] = (leaves[i] == 'y');
                dp[1][i] = dp[0][i];
            } else {
                dp[0][i] = dp[0][i - 1] + (leaves[i] == 'y');
                dp[1][i] = min(dp[0][i - 1] + (leaves[i] != 'y'), dp[1][i - 1] + (leaves[i] != 'y'));
            }

            if(i < 2) {
                dp[2][i] = dp[1][i];
            } else {
                dp[2][i] = min(dp[1][i - 1] + (leaves[i] == 'y'), dp[2][i - 1] + (leaves[i] == 'y'));
            }
        }

        return dp[2].back();
    }
};
```

### 4. 快速公交

**题解：**

**代码：**

```c++

class Solution {
public:
    typedef long long ll;
    const int mod = 1e9 + 7;
    unordered_map<int, long long> mp;
    ll inc;
    ll dec;
    vector<int> jump;
    vector<int> cost;

    int busRapidTransit(int target, int inc, int dec, vector<int>& jump, vector<int>& cost) {
        this->inc = inc;
        this->dec = dec;
        this->jump = jump;
        this->cost = cost;
        return dfs(target) % mod;
    }

    ll dfs(int cur) {
        if(cur == 0) {
            return 0;
        }
        if(cur == 1) {
            return inc;
        }
        if(mp.count(cur)) {
            return mp[cur];
        }

        ll ans = inc * cur;
        int n = jump.size();
        for(int i = 0; i < n; i++) {
            int step1 = cur % jump[i];
            int A = cur / jump[i];
            if(cur > step1) {
                ans = min(ans, step1 * inc + cost[i] + dfs(A));
            }
            if(step1) {
                int step2 = jump[i] - step1;
                A++;
                ans = min(ans, step2 * dec + cost[i] + dfs(A));
            }
        }
        mp[cur] = ans;
        return ans;
    }
};
```

### 5. 追逐游戏

**题解：**

**代码：**

```C++

class Solution {
public:
    int n, loop = 0;
    vector<vector<int>> adj;
    vector<int> depth, parent;
    vector<bool> in_loop;

    void dfs(int u, int p) {
        parent[u] = p;
        depth[u] = depth[p] + 1;
        for(int v : adj[u]) {
            if(v == p) {
                continue;
            }
            if(!depth[v]) {
                dfs(v, u);
            } else if(depth[v] < depth[u]) {
                int cu = u;
                while(cu != v) {
                    in_loop[cu] = 1;
                    loop++;
                    cu = parent[cu];
                }
                in_loop[v] = true;
                loop++;
            }
        } 
    }

    vector<int> bfs(int u, bool detect_loop) {
        vector<int> dist(n + 1, INT_MAX);
        queue<int> q;
        dist[u] = 0;
        q.push(u);
        while(!q.empty()) {
            int x = q.front();
            q.pop();
            if(detect_loop && in_loop[x]) {
                return {x, dist[x]};
            }
            for(int y : adj[x]) {
                if(dist[y] > dist[x] + 1) {
                    dist[y] = dist[x] + 1;
                    q.push(y);
                }
            }
        }
        return dist;
    }

    int chaseGame(vector<vector<int>>& edges, int startA, int startB) {
        n = edges.size();
        adj = vector<vector<int>>(n + 1);
        for(auto& v : edges) {
            adj[v[0]].push_back(v[1]);
            adj[v[1]].push_back(v[0]);
            if((v[1] == startA && v[0] == startB) || (v[0] == startA && v[1] == startB)) {
                return 1;
            }
        }

        depth.resize(n + 1);
        parent.resize(n + 1);
        in_loop.resize(n + 1);
        dfs(1, 0);

        vector<int> da = bfs(startA, false);
        vector<int> db = bfs(startB, false);

        if(loop >= 4) {
            vector<int> qb = bfs(startB, 1);
            if(qb[1] + 1 < da[qb[0]]) {
                return -1;
            }
        }

        int ans = 0;
        for(int i = 1; i <= n; i++) {
            if(da[i] > db[i] + 1) {
                ans = max(ans, da[i]);
            }
        }
        return ans;
    }
};
```

