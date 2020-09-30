## 第208场周赛

### [1. 文件夹操作日志搜集器](https://leetcode-cn.com/contest/weekly-contest-208/problems/crawler-log-folder/)

**题解**



**代码：**

```c++
class Solution {
public:
    int minOperations(vector<string>& logs) {
        int cnt = 0;
        for(auto& a : logs) {
            if(a == "../") {
                cnt--;
                if(cnt == -1) {
                    cnt = 0;
                }
            } else if(a != "./") {
                cnt++;
            }
        }
        return cnt;
    }
};
```

### [2. 经营摩天轮的最大利润](https://leetcode-cn.com/contest/weekly-contest-208/problems/maximum-profit-of-operating-a-centennial-wheel/)

**题解：**



**代码：**

```
class Solution {
public:
    int minOperationsMaxProfit(vector<int>& customers, int boardingCost, int runningCost) {
        int ans = -1, sum = 0;
        int maxsum = 0;
        int left = 0;
        int i;
        for(i = 0; i < customers.size(); i++) {
            left += customers[i];
            if(left < 4) {
                sum += (left *  boardingCost - runningCost);
                left = 0;
            } else {
                left -= 4;
                sum += (4 *  boardingCost - runningCost);
            }
            
            if(sum > maxsum) {
                maxsum = sum;
                ans = i + 1;
            }
        }
        i++;
        while(left) {
            if(left < 4) {
                sum += (left *  boardingCost - runningCost);
                left = 0;
            } else {
                left -= 4;
                sum += (4 *  boardingCost - runningCost);
            }
            if(sum > maxsum) {
                maxsum = sum;
                ans = i++;
            }
        }
        return maxsum > 0 ? ans : -1;
    }
};
```

### [3. 皇位继承顺序](https://leetcode-cn.com/contest/weekly-contest-208/problems/throne-inheritance/)

**题解：**

**代码：**

```
class ThroneInheritance {
public:
    ThroneInheritance(string kingName) {
        king = kingName;
        isalive[kingName] = 1;
    }
    
    void birth(string parentName, string childName) {
        getchildren[parentName].push_back(childName);
        isalive[childName] = 1;
    }
    
    void death(string name) {
        isalive[name] = 0;
    }
    
    vector<string> getInheritanceOrder() {
        order.clear();
        dfs(king);
        return order;
    }
    
    void dfs(string& name) {
        if(isalive[name]) {
            order.push_back(name);
        }
        for(auto& a : getchildren[name]) {
            dfs(a);
        }
    }
    
private:
    string king;
    unordered_map<string, vector<string>> getchildren;
    unordered_map<string, int> isalive;
    vector<string> order;
};
```

### [4. 最多可达成的换楼请求数目](https://leetcode-cn.com/contest/weekly-contest-208/problems/maximum-number-of-achievable-transfer-requests/)

**题解：**



**代码：**

```c++
class Solution {
public:
    int maximumRequests(int n, vector<vector<int>>& req) 
    {
        int ans = 0;
        int m = req.size();
        int lim = (1 << m);
        vector<int> delta(n);
        for(int state = 0; state < lim; state++) {
            for(int i = 0; i < n; i++) {
                delta[i] = 0;
            }
            int cnt = 0;
            for(int i = 0, k = 1; i < m; i++, k <<= 1) {
                if(state & k) {
                    delta[req[i][0]]++;
                    delta[req[i][1]]--;
                    cnt++;
                }
            }
            bool flag = 1;
            for(int i = 0; i < n; i++) {
                if(delta[i] != 0) {
                    flag = 0;
                    break;
                }
            }
            if(flag) {
                ans = max(ans, cnt);
            }
        }
        return ans;
    }
};
```

