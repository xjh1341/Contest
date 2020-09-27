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

秋日市集上，魔术师邀请小扣与他互动。魔术师的道具为分别写有数字 `1~N` 的 `N` 张卡牌，然后请小扣思考一个 `N` 张卡牌的排列 `target`。

魔术师的目标是找到一个数字 k（k >= 1），使得初始排列顺序为 `1~N` 的卡牌经过特殊的洗牌方式最终变成小扣所想的排列 `target`，特殊的洗牌方式为：

- 第一步，魔术师将当前位于 **偶数位置** 的卡牌（下标自 1 开始），保持 **当前排列顺序** 放在位于 **奇数位置** 的卡牌之前。例如：将当前排列 [1,2,3,4,5] 位于偶数位置的 [2,4] 置于奇数位置的 [1,3,5] 前，排列变为 [2,4,1,3,5]；
- 第二步，若当前卡牌数量小于等于 `k`，则魔术师按排列顺序取走全部卡牌；若当前卡牌数量大于 `k`，则取走前 `k` 张卡牌，剩余卡牌继续重复这两个步骤，直至所有卡牌全部被取走；

卡牌按照魔术师取走顺序构成的新排列为「魔术取数排列」，请返回是否存在这个数字 k 使得「魔术取数排列」恰好就是 `target`，从而让小扣感到大吃一惊。

**示例 1：**

> 输入：`target = [2,4,3,1,5]`
>
> 输出：`true`
>
> 解释：排列 target 长度为 5，初始排列为：1,2,3,4,5。我们选择 k = 2：
> 第一次：将当前排列 [1,2,3,4,5] 位于偶数位置的 [2,4] 置于奇数位置的 [1,3,5] 前，排列变为 [2,4,1,3,5]。取走前 2 张卡牌 2,4，剩余 [1,3,5]；
> 第二次：将当前排列 [1,3,5] 位于偶数位置的 [3] 置于奇数位置的 [1,5] 前，排列变为 [3,1,5]。取走前 2 张 3,1，剩余 [5]；
> 第三次：当前排列为 [5]，全部取出。
> 最后，数字按照取出顺序构成的「魔术取数排列」2,4,3,1,5 恰好为 target。

**示例 2：**

> 输入：`target = [5,4,3,2,1]`
>
> 输出：`false`
>
> 解释：无法找到一个数字 k 可以使「魔术取数排列」恰好为 target。

**提示：**

- `1 <= target.length = N <= 5000`
- 题目保证 `target` 是 `1~N` 的一个排列。

**代码：**

```c++
class Solution {
public:
    bool isMagic(vector<int>& target) {

    }
};
```

### 3. 数字游戏

小扣在秋日市集入口处发现了一个数字游戏。主办方共有 `N` 个计数器，计数器编号为 `0 ~ N-1`。每个计数器上分别显示了一个数字，小扣按计数器编号升序将所显示的数字记于数组 `nums`。每个计数器上有两个按钮，分别可以实现将显示数字加一或减一。小扣每一次操作可以选择一个计数器，按下加一或减一按钮。

主办方请小扣回答出一个长度为 `N` 的数组，第 `i` 个元素(0 <= i < N)表示将 `0~i` 号计数器 **初始** 所示数字操作成满足所有条件 `nums[a]+1 == nums[a+1],(0 <= a < i)` 的最小操作数。回答正确方可进入秋日市集。

由于答案可能很大，请将每个最小操作数对 `1,000,000,007` 取余。

**示例 1：**

> 输入：`nums = [3,4,5,1,6,7]`
>
> 输出：`[0,0,0,5,6,7]`
>
> 解释：
> i = 0，[3] 无需操作
> i = 1，[3,4] 无需操作；
> i = 2，[3,4,5] 无需操作；
> i = 3，将 [3,4,5,1] 操作成 [3,4,5,6], 最少 5 次操作；
> i = 4，将 [3,4,5,1,6] 操作成 [3,4,5,6,7], 最少 6 次操作；
> i = 5，将 [3,4,5,1,6,7] 操作成 [3,4,5,6,7,8]，最少 7 次操作；
> 返回 [0,0,0,5,6,7]。

**示例 2：**

> 输入：`nums = [1,2,3,4,5]`
>
> 输出：`[0,0,0,0,0]`
>
> 解释：对于任意计数器编号 i 都无需操作。

**示例 3：**

> 输入：`nums = [1,1,1,2,3,4]`
>
> 输出：`[0,1,2,3,3,3]`
>
> 解释：
> i = 0，无需操作；
> i = 1，将 [1,1] 操作成 [1,2] 或 [0,1] 最少 1 次操作；
> i = 2，将 [1,1,1] 操作成 [1,2,3] 或 [0,1,2]，最少 2 次操作；
> i = 3，将 [1,1,1,2] 操作成 [1,2,3,4] 或 [0,1,2,3]，最少 3 次操作；
> i = 4，将 [1,1,1,2,3] 操作成 [-1,0,1,2,3]，最少 3 次操作；
> i = 5，将 [1,1,1,2,3,4] 操作成 [-1,0,1,2,3,4]，最少 3 次操作；
> 返回 [0,1,2,3,3,3]。

**提示：**

- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^3`

**代码：**

```
class Solution {
public:
    vector<int> numsGame(vector<int>& nums) {

    }
};
```

### 4. 古董键盘

小扣在秋日市集购买了一个古董键盘。由于古董键盘年久失修，键盘上只有 26 个字母 **a~z** 可以按下，且每个字母最多仅能被按 `k` 次。

小扣随机按了 `n` 次按键，请返回小扣总共有可能按出多少种内容。由于数字较大，最终答案需要对 1000000007 (1e9 + 7) 取模。

**示例 1：**

> 输入：`k = 1, n = 1`
>
> 输出：`26`
>
> 解释：由于只能按一次按键，所有可能的字符串为 "a", "b", ... "z"

**示例 2：**

> 输入：`k = 1, n = 2`
>
> 输出：`650`
>
> 解释：由于只能按两次按键，且每个键最多只能按一次，所有可能的字符串（按字典序排序）为 "ab", "ac", ... "zy"

**提示：**

- `1 <= k <= 5`
- `1 <= n <= 26*k`

**代码：**

```
class Solution {
public:
    int keyboard(int k, int n) {

    }
};
```

### 5. 导航装置

小扣参加的秋日市集景区共有 NN 个景点，景点编号为 11~NN。景点内设有 N-1N−1 条双向道路，使所有景点形成了一个二叉树结构，根结点记为 `root`，景点编号即为节点值。

由于秋日市集景区的结构特殊，游客很容易迷路，主办方决定在景区的若干个景点设置导航装置，按照所在景点编号升序排列后定义装置编号为 1 ~ M。导航装置向游客发送数据，数据内容为列表 `[游客与装置 1 的相对距离,游客与装置 2 的相对距离,...,游客与装置 M 的相对距离]`。由于游客根据导航装置发送的信息来确认位置，因此主办方需保证游客在每个景点接收的数据信息皆不相同。请返回主办方最少需要设置多少个导航装置。

**示例 1：**

> 输入：`root = [1,2,null,3,4]`
>
> 输出：`2`
>
> 解释：在景点 1、3 或景点 1、4 或景点 3、4 设置导航装置。
>
> ![image.png](https://pic.leetcode-cn.com/1597996812-tqrgwu-image.png)

**示例 2：**

> 输入：`root = [1,2,3,4]`
>
> 输出：`1`
>
> 解释：在景点 3、4 设置导航装置皆可。
>
> ![image.png](https://pic.leetcode-cn.com/1597996826-EUQRyz-image.png)

**提示：**

- `2 <= N <= 50000`
- 二叉树的非空节点值为 `1~N` 的一个排列。

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

秋日市集上有个奇怪的黑盒，黑盒的主视图为 n*m 的矩形。从黑盒的主视图来看，黑盒的上面和下面各均匀分布有 m 个小孔，黑盒的左面和右面各均匀分布有 n 个小孔。黑盒左上角小孔序号为 0，按顺时针编号，总共有 2*(m+n) 个小孔。每个小孔均可以打开或者关闭，初始时，所有小孔均处于关闭状态。每个小孔上的盖子均为镜面材质。例如一个 2*3 的黑盒主视图与其小孔分布如图所示:

![image.png](https://pic.leetcode-cn.com/1598951281-ZCBrif-image.png)

店长告诉小扣，这里是「几何学的快问快答」，店长可能有两种操作：

- `open(int index, int direction)` - 若小孔处于关闭状态，则打开小孔，照入光线；否则直接照入光线；
- `close(int index)` - 关闭处于打开状态小孔，店长保证不会关闭已处于关闭状态的小孔；

其中：

- `index`： 表示小孔序号
- `direction`：`1` 表示光线沿 y=xy=x 方向，`-1` 表示光线沿 y=-xy=−x 方向。

![image.png](https://pic.leetcode-cn.com/1599620810-HdOlMi-image.png)

当光线照至边界时：若边界上的小孔为开启状态，则光线会射出；否则，光线会在小孔之间进行反射。特别地：

1. 若光线射向未打开的拐角（黑盒顶点），则光线会原路反射回去；
2. 光线自拐角处的小孔照入时，只有一种入射方向（如自序号为 0 的小孔照入方向只能为 `-1`）

![image.png](https://pic.leetcode-cn.com/1598953840-DLiAsf-image.png)

请帮助小扣判断并返回店长每次照入的光线从几号小孔射出。

**示例 1：**

> 输入：
> `["BlackBox","open","open","open","close","open"]`
> `[[2,3],[6,-1],[4,-1],[0,-1],[6],[0,-1]]`
>
> 输出：`[null,6,4,6,null,4]`
>
> 解释：
> BlackBox b = BlackBox(2,3); // 新建一个 2x3 的黑盒
> b.open(6,-1) // 打开 6 号小孔，并沿 y=-x 方向照入光线，光线至 0 号小孔反射，从 6 号小孔射出
> b.open(4,-1) // 打开 4 号小孔，并沿 y=-x 方向照入光线，光线轨迹为 4-2-8-2-4，从 4 号小孔射出
> b.open(0,-1) // 打开 0 号小孔，并沿 y=-x 方向照入光线，由于 6 号小孔为开启状态，光线从 6 号小孔射出
> b.close(6) // 关闭 6 号小孔
> b.shoot(0,-1) // 从 0 号小孔沿 y=-x 方向照入光线，由于 6 号小孔为关闭状态，4 号小孔为开启状态，光线轨迹为 0-6-4，从 4 号小孔射出

**示例 2：**

> 输入：
> `["BlackBox","open","open","open","open","close","open","close","open"]`
> `[[3,3],[1,-1],[5,1],[11,-1],[11,1],[1],[11,1],[5],[11,-1]]`
>
> 输出：`[null,1,1,5,1,null,5,null,11]`
>
> 解释：
>
> ![image.png](https://pic.leetcode-cn.com/1599204202-yGDMVk-image.png)
>
> BlackBox b = BlackBox(3,3); // 新建一个 3x3 的黑盒
> b.open(1,-1) // 打开 1 号小孔，并沿 y=-x 方向照入光线，光线轨迹为 1-5-7-11-1，从 1 号小孔射出
> b.open(5,1) // 打开 5 号小孔，并沿 y=x 方向照入光线，光线轨迹为 5-7-11-1，从 1 号小孔射出
> b.open(11,-1) // 打开 11 号小孔，并沿逆 y=-x 方向照入光线，光线轨迹为 11-7-5，从 5 号小孔射出
> b.open(11,1) // 从 11 号小孔沿 y=x 方向照入光线，光线轨迹为 11-1，从 1 号小孔射出
> b.close(1) // 关闭 1 号小孔
> b.open(11,1) // 从 11 号小孔沿 y=x 方向照入光线，光线轨迹为 11-1-5，从 5 号小孔射出
> b.close(5) // 关闭 5 号小孔
> b.open(11,-1) // 从 11 号小孔沿 y=-x 方向照入光线，光线轨迹为 11-1-5-7-11，从 11 号小孔射出

**提示：**

- `1 <= n, m <= 10000`
- `1 <= 操作次数 <= 10000`
- `direction` 仅为 `1` 或 `-1`
- `0 <= index < 2*(m+n)`

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
