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
二分，二维前缀和

<a href="#3c">c++</a>

<a href="#3p">python</a>

<a href="#3g">go</a>

# 4017. 数组中的峰值 II


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

```

<h2 id="3p">3p</h2>

```python

```

<h2 id="3g">3g</h2>

```go

```

<h2 id="4c">4c</h2>

```c++

```

<h2 id="4p">4p</h2>

```python

```

<h2 id="4g">4g</h2>

```go

```