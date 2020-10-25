## 第211场周赛

### 1. [两个相同字符之间的最长子字符串](https://leetcode-cn.com/problems/largest-substring-between-two-equal-characters/)

**题解**

使用map存储第一次出现的位置

**代码：**

```c++
class Solution {
public:
    int maxLengthBetweenEqualCharacters(string s) {
        map<char, int> mp;
        int maxlen = -1;
        for(int i = 0; i < s.size(); i++) {
            if(mp.count(s[i])) {
                maxlen = max(maxlen, i - mp[s[i]] - 1);
            } else {
                mp[s[i]] = i;
            }
        }
        return maxlen;
    }
};
```

### 2. [执行操作后字典序最小的字符串](https://leetcode-cn.com/problems/lexicographically-smallest-string-after-applying-operations/)

**题解：**



**代码：**

```c++

```

### 3.[无矛盾的最佳球队](https://leetcode-cn.com/problems/best-team-with-no-conflicts/)

**题解：**

排序 + 动态规划；

先按照age和score从大到小排序，dp[i]表示最后一个队员是i的最大分数，可以从0 - i - 1范围枚举上一个球员，只需要保证i的分数大于等于该球员分数即可满足条件；

**代码：**

```c++
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        int n = ages.size();
        vector<pair<int, int>> vp;
        for(int i = 0; i < n; i++) {
            vp.push_back({ages[i], scores[i]});
        }
        sort(vp.begin(), vp.end());
        vector<int> dp(n);
        int ans = 0;
        for(int i = 0; i < n; i++) {
            dp[i] = vp[i].second;
            for(int j = 0; j < i; j++) {
                if(vp[i].second >= vp[j].second) {
                    dp[i] = max(dp[i], dp[j] + vp[i].second);
                }
            }
            ans = max(ans, dp[i]);
            cout << vp[i].first << " " << vp[i].second << " " << dp[i] << endl;
        }
        return ans;
    }
};
```

### 4. [带阈值的图连通性](https://leetcode-cn.com/problems/graph-connectivity-with-threshold/)

**题解：**

通过枚举因子，连接他们的倍数，使用并查集来实现连边和查询工作

**代码：**

```c++
class UnionFind {
private:
    int m_n;
    vector<int> m_parent;

public:
    UnionFind(int n) {
        m_n = n;
        m_parent.resize(n);
        for(int i = 0; i < n; i++) {
            m_parent[i] = i;
        }
    }

    int find(int idx) {
        return m_parent[idx] == idx ? idx : m_parent[idx] = find(m_parent[idx]);
    }

    void connect(int a, int b) {
        int fa = find(a), fb = find(b);
        if(fa != fb) {
            m_parent[fb] = fa;
        }
    }
};

class Solution {
public:
    vector<bool> areConnected(int n, int threshold, vector<vector<int>>& queries) {
        UnionFind uf(n + 1);
        vector<bool> ans;
        for(int i = threshold + 1; i <= n; i++) {
            for(int j = 2 * i; j <= n; j += i) {
                uf.connect(i, j);
            }
        }
        for(auto& q : queries) {
            int f1 = uf.find(q[0]);
            int f2 = uf.find(q[1]);
            ans.emplace_back(f1 == f2);
        }
        return ans;
    }
};
```

