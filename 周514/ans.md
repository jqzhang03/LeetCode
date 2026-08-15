# 4014. 应用折扣后的最低总价
贪心

<a href="#1c">c++</a>

<a href="#1p">python</a>

<a href="#1g">go</a>

# 4015. 树的加权和
搜索

<a href="#2c">c++</a>

<a href="#2p">python</a>

<a href="#2g">go</a>

# 4016. 两个不重叠子正方形的最大面积
dp

<a href="#3c">c++</a>

<a href="#3p">python</a>

<a href="#3g">go</a>

# 4017. 数组中的峰值 II
线段树

<a href="#4c">c++</a>

<a href="#4p">python</a>

<a href="#4g">go</a>

# 代码

<h2 id="1c">1c</h2>

```c++
class Solution {
public:
    double minPrice(vector<int>& prices, vector<int>& discounts) {
        sort(discounts.begin(), discounts.end());
        sort(prices.begin(), prices.end());
        int n = prices.size(), m = discounts.size();
        double ans = 0.0;
        for (int i = n - 1, j = m - 1; i >= 0; i--) {
            if (j >= 0) {
                ans += (prices[i] * (100 - discounts[j--])) / 100.0;
            } else {
                ans += prices[i];
            }
        }
        return ans;
    }
};
```

<h2 id="1p">1p</h2>

```python
class Solution:
    def minPrice(self, prices: list[int], discounts: list[int]) -> float:
        prices.sort()
        discounts.sort()
        n = len(prices)
        m = len(discounts)
        j = m - 1
        ans = 0.
        for i in reversed(prices):
            if j >= 0:
                ans += (i * (100 - discounts[j])) / 100.0
                j -= 1
            else:
                ans += i
        return ans
```

<h2 id="1g">1g</h2>

```go
func minPrice(prices []int, discounts []int) float64 {
    slices.Sort(prices)
    slices.Sort(discounts)
    n := len(prices)
    m := len(discounts)
    var ans float64 = float64(0)
    for i, j := n - 1, m - 1; i >= 0; i-- {
        if j >= 0 {
            ans += float64(prices[i] * (100 - discounts[j])) / float64(100)
            j -= 1
        } else {
            ans += float64(prices[i])
        }
    }
    return ans
}
```

<h2 id="2c">2c</h2>

```c++
class Solution {
public:
    long long weightedSum(vector<int>& parent, vector<int>& nums) {
        int n = parent.size();
        vector<int> dep(n, -1);
        dep[0] = 1;
        vector<vector<int>> f(n);
        for (int i = 0; i < n; i++) {
            if (parent[i] == -1) {
                continue;
            }
            f[parent[i]].emplace_back(i);
        }
        queue<int> q;
        q.push(0);
        int h = 1;
        while(!q.empty()) {
            int u = q.front();
            q.pop();
            for (auto v : f[u]) {
                if (dep[v] == -1) {
                    dep[v] = dep[u] + 1;
                    q.push(v);
                    h = max(h, dep[v]);
                }
            }
        }
        long long ans = 0;
        for (int i = 0; i < n; i++) {
            ans += 1LL * nums[i] * (h - dep[i] + 1);
        }
        return ans;
    }
};
```

<h2 id="2p">2p</h2>

```python
class Solution:
    def weightedSum(self, parent: list[int], nums: list[int]) -> int:
        n = len(parent)
        f = [[] for _ in range(n)]
        dep = [-1] * n
        dep[0] = 1
        for i in range(n):
            if parent[i] == -1:
                continue
            f[parent[i]].append(i)
        q = deque()
        q.append(0)
        h = 1
        while len(q) > 0:
            u = q.popleft()
            for v in f[u]:
                if dep[v] == -1:
                    dep[v] = dep[u] + 1
                    q.append(v)
                    h = max(h, dep[v])
        ans = 0
        for i in range(n):
            ans += nums[i] * (h - dep[i] + 1)
        return ans
```

<h2 id="2g">2g</h2>

```go
func weightedSum(parent []int, nums []int) int64 {
    n := len(parent)
    f := make([][]int, n)
    for i := 0; i < n; i++ {
        if parent[i] != -1 {
            f[parent[i]] = append(f[parent[i]], i)
        }
    }
    q := []int{}
    q = append(q, 0)
    h := 1
    dep := make([]int, n)
    for i, _ := range(dep) {
        dep[i] = -1
    }
    dep[0] = 1
    for len(q) != 0 {
        u := q[0]
        q = q[1:]
        for _, v := range(f[u]) {
            if dep[v] == -1 {
                dep[v] = dep[u] + 1
                q = append(q, v)
                h = max(h, dep[v])
            }
        }
    }
    var ans int64 = int64(0)
    for i := 0; i < n; i++ {
        ans += int64(nums[i]) * int64(h - dep[i] + 1)
    }
    return ans
}
```

<h2 id="3c">3c</h2>

```c++
class Solution {
public:
    int maxArea(vector<vector<int>>& mat) {
        int n = mat.size(), m = mat[0].size();
        vector<vector<int>> mat1(m, vector<int> (n));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                mat1[j][i] = mat[i][j];
            }
        }
        auto f = [&](vector<vector<int>> mx) {
            n = mx.size(), m = mx[0].size();
            vector<vector<int>> dp(n, vector<int> (m)), dp1(n, vector<int> (m));
            vector<int> a(n), b(n);
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (mx[i][j]) {
                        if (i && j) {
                            dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                        } else {
                            dp[i][j] = 1;
                        }
                    }
                    if (i > 0) {
                        a[i] = max({a[i - 1], dp[i][j], a[i]});
                    } else {
                        a[i] = max(a[i], dp[i][j]);
                    }
                }
            }
            for (int i = n - 1; i >= 0; i--) {
                for (int j = m - 1; j >= 0; j--) {
                    if (mx[i][j]) {
                        if (i != n - 1 && j != m - 1) {
                            dp1[i][j] = min({dp1[i + 1][j + 1], dp1[i + 1][j], dp1[i][j + 1]}) + 1;
                        } else {
                            dp1[i][j] = 1;
                        }
                    }
                    if (i < n - 1) {
                        b[i] = max({b[i + 1], b[i], dp1[i + 1][j]});
                    } else {
                        b[i] = max(b[i], dp1[i][j]);
                    }
                }
            }
            int ans = 0;
            for (int i = 0; i <= n - 2; i++) {
                ans = max(ans, min(a[i], b[i]));
            }
            return ans * ans;
        };
        return max(f(mat), f(mat1));
    }
};
```

<h2 id="3p">3p</h2>

```python
class Solution:
    def maxArea(self, mat: List[List[int]]) -> int:
        n = len(mat)
        m = len(mat[0])
        mat1 = [[0] * n for _ in range(m)]
        for i in range(n):
            for j in range(m):
                mat1[j][i] = mat[i][j]
        return max(self.f(mat), self.f(mat1))

    def f(self, mat: List[List[int]]) -> int:
        n = len(mat)
        m = len(mat[0])
        dp = [[0] * m for _ in range(n)]
        dp1 = [[0] * m for _ in range(n)]
        a = [0] * n
        b = [0] * n
        for i in range(n):
            for j in range(m):
                if mat[i][j] == 1:
                    if i > 0 and j > 0:
                        dp[i][j] = min(dp[i - 1][j], min(dp[i - 1][j - 1], dp[i][j - 1])) + 1
                    else:
                        dp[i][j] = 1
                if i > 0:
                    a[i] = max(a[i], max(a[i - 1], dp[i][j]))
                else:
                    a[i] = max(a[i], dp[i][j])
        for i in range(n - 1, -1, -1):
            for j in range(m - 1, -1, -1):
                if mat[i][j] == 1:
                    if i != n - 1 and j != m - 1:
                        dp1[i][j] = min(dp1[i + 1][j + 1], min(dp1[i + 1][j], dp1[i][j + 1])) + 1
                    else:
                        dp1[i][j] = 1
                if i < n - 1:
                    b[i] = max(b[i + 1], max(b[i], dp1[i + 1][j]))
                else:
                    b[i] = max(b[i], dp1[i][j])
        ans = 0
        for i in range(n - 1):
            ans = max(ans, min(a[i], b[i]))
        return ans * ans
```

<h2 id="3g">3g</h2>

```go
func maxArea(mat [][]int) int {
    n := len(mat)
    m := len(mat[0])
    mat1 := make([][]int, m)
    for i := 0; i < m; i++ {
        mat1[i] = make([]int, n)
    }
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            mat1[j][i] = mat[i][j]
        }
    }
    return max(f(mat), f(mat1))
}

func f(mat [][]int) int {
    n := len(mat)
    m := len(mat[0])
    dp := make([][]int, n)
    dp1 := make([][]int, n)
    for i := 0; i < n; i++ {
        dp[i] = make([]int, m)
        dp1[i] = make([]int, m)
    }
    a := make([]int, n)
    b := make([]int, n)
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            if mat[i][j] == 1 {
                if i > 0 && j > 0 {
                    dp[i][j] = min(dp[i][j - 1], min(dp[i - 1][j - 1], dp[i - 1][j])) + 1
                } else {
                    dp[i][j] = 1
                }
            }
            if i > 0 {
                a[i] = max(a[i - 1], max(a[i], dp[i][j]))
            } else {
                a[i] = max(a[i], dp[i][j])
            }
        }
    }
    for i := n - 1; i >= 0; i-- {
        for j := m - 1; j >= 0; j-- {
            if mat[i][j] == 1 {
                if i != n - 1 && j != m - 1 {
                    dp1[i][j] = min(dp1[i + 1][j + 1], min(dp1[i + 1][j], dp1[i][j + 1])) + 1
                } else {
                    dp1[i][j] = 1
                }
            }
            if i < n - 1 {
                b[i] = max(b[i + 1], max(b[i], dp1[i + 1][j]))
            } else {
                b[i] = max(b[i], dp1[i][j])
            }
        }
    }
    ans := 0
    for i := 0; i < n - 1; i++ {
        ans = max(ans, min(a[i], b[i]))
    }
    return ans * ans
}
```

<h2 id="4c">4c</h2>

```c++
class Solution {
    struct node {
        long long len, pre, suf, cnt;
        bool flag;
    }seg[100010 << 2];
public:
    vector<long long> countOfPeaks(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        int q = queries.size();
        auto merge = [&](node &a, node &b, node &c) {
            a.len = b.len + c.len;
            a.flag = b.flag || c.flag;
            a.cnt = b.cnt + c.cnt + b.len * c.len - b.suf * c.pre;
            a.pre = b.flag ? b.pre : b.len + c.pre;
            a.suf = c.flag ? c.suf : c.len + b.suf;
        };
        auto build = [&](auto &&self, int i, int l, int r) -> void {
            if (l == r) {
                seg[i].len = seg[i].pre = seg[i].suf = 1;
                seg[i].cnt = 0;
                if (l > 0 && l < n - 1 && nums[l] > nums[l - 1] && nums[l] > nums[l + 1]) {
                    seg[i].flag = true;
                } else {
                    seg[i].flag = false;
                }
                return;
            }
            int mid = (l + r) >> 1;
            self(self, i << 1, l, mid);
            self(self, i << 1 | 1, mid + 1, r);
            merge(seg[i], seg[i << 1], seg[i << 1 | 1]);
        };
        auto update = [&](auto &&self, int i, int l, int r, int pos) -> void {
            if (l == r) {
                if (l > 0 && l < n - 1 && nums[l] > nums[l - 1] && nums[l] > nums[l + 1]) {
                    seg[i].flag = true;
                } else {
                    seg[i].flag = false;
                }
                return;
            }
            int mid = (l + r) >> 1;
            if (pos <= mid) {
                self(self, i << 1, l, mid, pos);
            } else {
                self(self, i << 1 | 1, mid + 1, r, pos);
            }
            merge(seg[i], seg[i << 1], seg[i << 1 | 1]);
        };
        auto query = [&](auto &&self, int i, int l, int r, int x, int y) -> node {
            if (x <= l && r <= y) {
                return seg[i];
            }
            int mid = (l + r) >> 1;
            if (y <= mid) {
                return self(self, i << 1, l, mid, x, y);
            } else {
                if (x > mid) {
                    return self(self, i << 1 | 1, mid + 1, r, x, y);
                } else {
                    node ans, ll = self(self, i << 1, l, mid, x, mid), rr = self(self, i << 1 | 1, mid + 1, r, mid + 1, y);
                    merge(ans, ll, rr);
                    return ans;
                }
            }
        };
        build(build, 1, 0, n - 1);
        vector<long long> ans;
        for (int i = 0; i < q; i++) {
            if (queries[i][0] == 1) {
                ans.push_back(query(query, 1, 0, n - 1, queries[i][1], queries[i][2]).cnt);
            } else {
                int idx = queries[i][1], val = queries[i][2];
                nums[idx] = val;
                if (idx - 1 >= 0) {
                    update(update, 1, 0, n - 1, idx - 1);
                }
                update(update, 1, 0, n - 1, idx);
                if (idx + 1 >= 0) {
                    update(update, 1, 0, n - 1, idx + 1);
                }
            }
        }
        return ans;
    }
};
```

<h2 id="4p">4p</h2>

```python

```

<h2 id="4g">4g</h2>

```go

```