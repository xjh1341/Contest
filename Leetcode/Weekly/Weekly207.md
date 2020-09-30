## 第207场周赛

### [1. 重新排列单词间的空格](https://leetcode-cn.com/contest/weekly-contest-207/problems/rearrange-spaces-between-words/)

**题解**

​		遍历一遍，记录空格个数和单词个数，然后计算单词之间填充的空格和剩下的空格

**代码：**

```c++
class Solution {
public:
    string reorderSpaces(string text) {
        int cnt1 = 0;
        vector<string> vs;
        string temp;
        for(int i = 0; i < text.size(); i++) {
            if(text[i] == ' ') {
                cnt1++;
                if(temp.size()) {
                    vs.push_back(temp);
                    temp.clear();
                }
            } else {
                temp += text[i];
            }
        }
        if(temp.size()) {
            vs.push_back(temp);
            temp.clear();
        }
        int n = vs.size();
        int d = 0;
        if(n > 1) {
            d = cnt1 / (n - 1);
        }
        string ans = vs[0];
        for(int i = 1; i < vs.size(); i++) {
            for(int j = 0; j < d; j++) {
                ans += " ";
                cnt1--;
            }
            ans += vs[i];
        }
        while(cnt1--) {
            ans += " ";
        }
        return ans;
    }
};
```

### [2. 拆分字符串使唯一子字符串的数目最大](https://leetcode-cn.com/contest/weekly-contest-207/problems/split-a-string-into-the-max-number-of-unique-substrings/)


**题解**

​		每两个相邻字符之间有两种选择，拆开或者不拆。回溯法遍历，用set检查不合法的情况，并存入合法的子字符串

**代码：**

```c++
class Solution {
public:
    int cnt = 0;
    void dfs(int left, int index, string& s) {
        if(index == s.size()) {
            int n = st.size();
            cnt = max(cnt, n);
            return;
        }
        string temp = s.substr(left, index - left + 1);
        if(!st.count(temp)) {
            st.insert(temp);
            dfs(index + 1, index + 1, s);
            st.erase(temp);
        }
        dfs(left, index + 1, s);
    }
    
    int maxUniqueSplit(string s) {
        dfs(0, 0, s);
        return cnt; 
    }
    
private:
    unordered_set<string> st;
};
```

### [3. 矩阵的最大非负积](https://leetcode-cn.com/contest/weekly-contest-207/problems/minimum-cost-to-connect-two-groups-of-points/)


**题解**

​		简单的dp，当前位置的取值根据上边或左边的值，注意乘积最大值需要保留最小值，最后取余

**代码：**

```c++
class Solution {
public:
    const int mod = 1e9 + 7;
    int maxProductPath(vector<vector<int>>& grid) {
        int row = grid.size();
        int col = grid[0].size();
        long long dp1[16][16] = {0};
        long long dp2[16][16] = {0};
        dp1[0][0] = grid[0][0];
        dp2[0][0] = grid[0][0];
        for(int j = 1; j < col; j++) {
            dp1[0][j] = dp1[0][j - 1] * grid[0][j];
            dp2[0][j] = dp2[0][j - 1] * grid[0][j];
        }
        for(int i = 1; i < row; i++) {
            dp1[i][0] = dp1[i - 1][0] * grid[i][0];
            dp2[i][0] = dp2[i - 1][0] * grid[i][0];
        }
        for(int i = 1; i < row; i++) {
            for(int j = 1; j < col; j++) {
                long long n1 = dp1[i - 1][j] * grid[i][j];
                long long n2 = dp2[i - 1][j] * grid[i][j];
                long long n3 = dp1[i][j - 1] * grid[i][j];
                long long n4 = dp2[i][j - 1] * grid[i][j];
                
                dp1[i][j] = max(n1, n2);
                dp1[i][j] = max(dp1[i][j], n3);
                dp1[i][j] = max(dp1[i][j], n4);
                
                dp2[i][j] = min(n1, n2);
                dp2[i][j] = min(dp2[i][j], n3);
                dp2[i][j] = min(dp2[i][j], n4);
            }
        }
        if(dp1[row - 1][col - 1] < 0) {
            return -1;
        }
        return dp1[row - 1][col - 1] % mod;
    }
};
```

### [4. 连通两组点的最小成本](https://leetcode-cn.com/contest/weekly-contest-207/problems/minimum-cost-to-connect-two-groups-of-points/)


**题解**

​		数据范围在12以内，应该是O(2^n)的复杂度，采用状压；

​		对于第一组中的每个点，第一种做法是直接连一条边，第二种做法是连接若干个当前还没有连通的点，然后更新最小花费状态；

**代码：**

```c++
class Solution {
public:
    int connectTwoGroups(vector<vector<int>> &cost) {
        int size1 = cost.size(), size2 = cost[0].size(), stateNum = 1 << size2;    //stateNum为第二组总的状态数+1
        vector<int> dp(stateNum, INT_MAX);                                         //dp数组初始化为很大的数
        dp[0] = 0;                                                                 //初始状态
        for (int i = 0; i < size1; ++i) {                                          //迭代每一行
            vector<int> temp(stateNum, INT_MAX);                                   //滚动数组
            for (int state = 0; state < stateNum; ++state) {                       //枚举所有状态
                if (dp[state] == INT_MAX) continue;                                //若状态不可达，continue
                for (int j = 0; j < size2; ++j) {                                  //方案一：任选一条边相连
                    int nextState = state | (1 << j);                              //相连后到达的状态
                    temp[nextState] = min(temp[nextState], dp[state] + cost[i][j]);//更新最小花费
                }
                int flipState = (stateNum - 1) ^ state;                                          //方案二：连接若干未连接的边，使用异或进行位反转得到所有未连接的边
                for (int subState = flipState; subState; subState = flipState & (subState - 1)) {//枚举未连接的边的子集
                    int sum = 0;                                                                 //记录花费
                    for (int k = 0; k < size2; ++k)                                              //枚举size2
                        if (subState & (1 << k)) sum += cost[i][k];                              //若子集中存在该边，则更新花费
                    int nextState = state | subState;                                            //相连后到达的状态
                    temp[nextState] = min(temp[nextState], dp[state] + sum);                     //更新最小花费
                }
            }
            dp = move(temp);//滚动数组
        }
        return dp.back();//返回结果
    }
};
```