## LCCUP2020秋季编程大赛团队赛

[比赛链接](https://leetcode-cn.com/contest/season/2020-fall/problems/ccw6C7/)

### 1. 黑白方格画

小扣注意到秋日市集上有一个创作黑白方格画的摊位。摊主给每个顾客提供一个固定在墙上的白色画板，画板不能转动。画板上有 `n * n` 的网格。绘画规则为，小扣可以选择任意多行以及任意多列的格子涂成黑色，所选行数、列数均可为 0。

小扣希望最终的成品上需要有 `k` 个黑色格子，请返回小扣共有多少种涂色方案。

注意：两个方案中任意一个相同位置的格子颜色不同，就视为不同的方案。

**示例 1：**

> 输入：`n = 2, k = 2`
>
> 输出：`4`
>
> 解释：一共有四种不同的方案：
> 第一种方案：涂第一列；
> 第二种方案：涂第二列；
> 第三种方案：涂第一行；
> 第四种方案：涂第二行。

**示例 2：**

> 输入：`n = 2, k = 1`
>
> 输出：`0`
>
> 解释：不可行，因为第一次涂色至少会涂两个黑格。

**示例 3：**

> 输入：`n = 2, k = 4`
>
> 输出：`1`
>
> 解释：共有 2*2=4 个格子，仅有一种涂色方案。

**限制：**

- `1 <= n <= 6`
- `0 <= k <= n * n`

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

### 2. 魔术排列

**提示：**

**代码：**

```c++
class Solution {
public:
    bool isMagic(vector<int>& target) {

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

