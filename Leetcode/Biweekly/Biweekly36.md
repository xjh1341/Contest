## 第36场双周赛

### [1. 设计停车系统](https://leetcode-cn.com/contest/biweekly-contest-36/problems/design-parking-system/)

**题解**

水题

**代码：**

```c++
class ParkingSystem {
public:
    ParkingSystem(int big, int medium, int small) {
        m_left.resize(4);
        m_left[1] = big;
        m_left[2] = medium;
        m_left[3] = small;
    }
    
    bool addCar(int carType) {
        if(m_left[carType] > 0) {
            m_left[carType]--;
            return 1;
        }
        return 0;
    }
private:
    vector<int> m_left;
};
```

### [2. 警告一小时内使用相同员工卡大于等于三次的人](https://leetcode-cn.com/contest/biweekly-contest-36/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period/)

**题解：**

记录每个员工的签到时间，排序之后遍历一下，如果处于一个小时之内则警告，时间可以转换为int值，方便处理

**代码：**

```c++
class Solution {
public:
    vector<string> alertNames(vector<string>& keyName, vector<string>& keyTime) {
        int n = keyName.size();
        for(int i = 0; i < n; i++) {
            mp[keyName[i]].push_back(keyTime[i]);
        }
        vector<string> ans;
        for(auto& m : mp) {
            auto& v = m.second;
            sort(v.begin(), v.end());
            int len = v.size();
            for(int i = 0; i < len - 2; i++) {
                int h1 = (v[i][0] - '0') * 10 + (v[i][1] - '0');
                int m1 = (v[i][3] - '0') * 10 + (v[i][4] - '0');
                int h2 = (v[i + 2][0] - '0') * 10 + (v[i + 2][1] - '0');
                int m2 = (v[i + 2][3] - '0') * 10 + (v[i + 2][4] - '0');
                if((h1 == h2) || (h1 + 1 == h2 && m1 >= m2)) {
                    ans.push_back(m.first);
                    break;
                }
            }
        }
        return ans;
    }
    
    map<string, vector<string>> mp;
};
```

### [3. 给定行和列的和求可行矩阵](https://leetcode-cn.com/contest/biweekly-contest-36/problems/find-valid-matrix-given-row-and-column-sums/)

**题解：**

贪心的方法，遍历填充行和列最小的值，待证明

**代码：**

```c++
class Solution {
public:
    vector<vector<int>> restoreMatrix(vector<int>& rowSum, vector<int>& colSum) {
        int row = rowSum.size();
        int col = colSum.size();
        vector<vector<int>> ans(row, vector<int>(col));
        int i = 0, j = 0;
        while(i < row && j < col) {
            int t = min(rowSum[i], colSum[j]);
            ans[i][j] += t;
            rowSum[i] -= t;
            colSum[j] -= t;
            if(rowSum[i] == 0) i++;
            if(colSum[j] == 0) j++;
        }
        return ans;
    }
};
```

### [4. 找到处理最多请求的服务器](https://leetcode-cn.com/contest/biweekly-contest-36/problems/find-servers-that-handled-most-number-of-requests/)

**题解：**

优先队列

**代码：**

```c++

```