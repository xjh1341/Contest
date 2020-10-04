## 第209场周赛

### [1. 特殊数组的特征值](https://leetcode-cn.com/contest/weekly-contest-209/problems/special-array-with-x-elements-greater-than-or-equal-x/)

**题解**

x的取值范围从0到n，先排序，然后枚举x，通过二分找出大于x的个数，若大于等于x则输出

**代码：**

```c++
class Solution {
public:
    int specialArray(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        for(int i = 0; i <= n; i++) {
            int cnt = nums.end() - lower_bound(nums.begin(), nums.end(), i);
            if(cnt >= i) {
                return cnt;
            }
        }
        return -1;
    }
};
```

### [2. 奇偶树](https://leetcode-cn.com/contest/weekly-contest-209/problems/even-odd-tree/)

**题解：**

树的层序遍历的变种

**代码：**

```c++
class Solution {
public:
    bool isEvenOddTree(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        int flag = 1;
        while(!q.empty()) {
            int size = q.size();
            vector<int> v;
            while(size--) {
                TreeNode* node = q.front();
                q.pop();
                if(node->val % 2 != flag) {
                    return 0;
                }
                v.push_back(node->val);
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            for(int i = 1; i < v.size(); i++) {
                if(flag == 1 && v[i] <= v[i - 1]) {
                    return 0;
                } else if(flag == 0 && v[i] >= v[i - 1]) {
                    return 0;
                }
            }
            flag = flag == 1 ? 0 : 1;
        }
        return 1;
    }
};
```

### [3.可见点的最大数目](https://leetcode-cn.com/contest/weekly-contest-209/problems/maximum-number-of-visible-points/)

**题解：**

保存和原点重合的点，根据dx和dy计算夹角，因为角度首尾相接，将整个数组加上360接到原数组后，然后使用滑动窗口找出覆盖最多的区间，注意使用double存储角度，否则可能存在误差；

**代码：**

```c++
class Solution {
public:
    int visiblePoints(vector<vector<int>>& points, int angle, vector<int>& location) {
        vector<double> angles;
        int flag = 0;
        for(auto& p : points) {
            double dy = p[1] - location[1];
            double dx = p[0] - location[0];
            if(dx == 0 && dy == 0) {
                flag++;
            } else {
                double ang = atan2(dy, dx) * 180 / 3.14159;
                angles.push_back(ang);
            }
        }
        sort(angles.begin(), angles.end());
        int n = angles.size();
        for(int i = 0; i < n; i++) {
            angles.push_back(angles[i] + 360);
        }
        int ans = 0;
        int i = 0, j = 0;
        while(j < angles.size()) {
            if(angles[j] - angles[i] <= angle + 1e-5) {
                j++;
                ans = max(ans, j - i);
            } else {
                i++;
            }
        }
        return ans + flag;
    }
};
```

### [4. 使整数变为 0 的最少操作次数](https://leetcode-cn.com/contest/weekly-contest-209/problems/minimum-one-bit-operations-to-make-integers-zero/)

**题解：**

本质是求解n是第几个格雷码，格雷码的生成顺序；

- 第0项为0；
- 第一项改变最右边的位元；
- 第二项改变右起第一个为1的位元的左边位元
- 重复第一项和第二项

**代码：**

```c++
class Solution {
public:
    int minimumOneBitOperations(int n) {
        int ans = 0;
        while (n) {
            ans ^= n;
            n >>= 1;    
        } 
        return ans;
    }
};
```

