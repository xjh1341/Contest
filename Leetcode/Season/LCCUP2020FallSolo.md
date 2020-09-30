## LCCUP2020秋季编程大赛个人赛

[比赛链接](https://leetcode-cn.com/contest/season/2020-fall/problems/nGK0Fy/)

### 1. 速算机器人

**提示：**

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

**提示：**



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

**提示：**



**代码：**

```c++
class Solution {
public:
    int minimumOperations(string leaves) {
        //to do
    }
};
```

### 4. 快速公交

**提示：**



**代码：**

```c++
class Solution {
public:
    int busRapidTransit(int target, int inc, int dec, vector<int>& jump, vector<int>& cost) {

    }
};
```

### 5. 追逐游戏

**代码：**

```C++
class Solution {
public:
    int chaseGame(vector<vector<int>>& edges, int startA, int startB) {

    }
};
```

