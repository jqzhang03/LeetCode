# 4010. 数对的最大强度
模拟

<a href="1c">c++</a>

<a href="1p">python</a>

<a href="1g">go</a>

# 4011. 按奇偶比统计子数组 I
前缀和，暴力

<a href="#2c">c++</a>

<a href="#2p">python</a>

<a href="#2g">go</a>

# 4012. 统计每个班次结束后的未完成任务数
二分，前缀和

<a href="#3c">c++</a>

<a href="#3p">python</a>

<a href="#3g">go</a>

# 4013. 按奇偶比统计子数组 II
树状数组

<a href="#4c">c++</a>

<a href="#4p">python</a>

<a href="#4g">go</a>

# 代码

<h2 id="1c">1c</h2>

```c++
class Solution {
public:
    long long maxPairStrength(vector<int>& nums) {
        long long ans = 0;
        int sz = nums.size();
        for (int i = 0; i < sz; i++) {
            for (int j = i + 1; j < sz; j++) {
                int g = gcd(nums[i], nums[j]);
                ans = max(ans, 1LL * nums[i] / g * (nums[j] / g));
            }
        }
        return ans;
    }
    int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
};
```

<h2 id="1p">1p</h2>

```python
class Solution:
    def maxPairStrength(self, nums: list[int]) -> int:
        ans = 0
        sz = len(nums)
        for i in range(sz):
            for j in range(i + 1, sz):
                g = math.gcd(nums[i], nums[j])
                ans = max(ans, nums[i] // g * nums[j] // g)
        return ans
```

<h2 id="1g">1g</h2>

```go
func maxPairStrength(nums []int) int64 {
    sz := len(nums)
    var ans int64 = int64(0)
    for i := 0; i < sz; i++ {
        for j := i + 1; j < sz; j++ {
            g := gcd(int64(nums[i]), int64(nums[j]))
            ans = max(ans, int64(nums[i]) / g * int64(nums[j]) / g)
        }
    }
    return ans
}
func gcd(a, b int64) int64 {
    if b == int64(0) {
        return a
    } else {
        return gcd(b, a % b)
    }
}
```

<h2 id="2c">2c</h2>

```c++
class Solution {
public:
    int countRatioSubarrays(vector<int>& nums, int a, int b) {
        int sz = nums.size();
        int ans = 0;
        for (int i = 0; i < sz; i++) {
            if (nums[i] & 1) {
                nums[i] = a;
            } else {
                nums[i] = -b;
            }
        }
        for (int i = 1; i < sz; i++) {
            nums[i] = nums[i] + nums[i - 1];
        }
        for (int i = 0; i < sz; i++) {
            for (int j = i; j < sz; j++) {
                int l, r;
                r = nums[j];
                if (i == 0) {
                    l = 0;
                } else {
                    l = nums[i - 1];
                }
                if (r - l >= 0) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```

<h2 id="2p">2p</h2>

```python
class Solution:
    def countRatioSubarrays(self, nums: list[int], a: int, b: int) -> int:
        ans = 0
        sz = len(nums)
        for i in range(sz):
            nums[i] = a if nums[i] % 2 == 1 else -b
            nums[i] += nums[i - 1] if i > 0 else 0
        for i in range(sz):
            for j in range(i, sz):
                r = nums[j]
                l = nums[i - 1] if i > 0 else 0
                if r - l >= 0:
                    ans += 1

        return ans
```

<h2 id="2g">2g</h2>

```go
func countRatioSubarrays(nums []int, a int, b int) int {
    ans := 0
    sz := len(nums)
    for i := 0; i < sz; i++ {
        if nums[i] % 2 == 1 {
            nums[i] = a
        } else {
            nums[i] = -b
        }
        if i != 0 {
            nums[i] += nums[i - 1]
        }
    }
    for i := 0; i < sz; i++ {
        for j := i; j < sz; j++ {
            r := nums[j]
            var l int = 0
            if i != 0 {
                l = nums[i - 1]
            }
            if r - l >= 0 {
                ans++
            }
        }
    }
    return ans
}
```

<h2 id="3c">3c</h2>

```c++
class Solution {
public:
    vector<int> countTasks(vector<int>& tasks, vector<int>& shifts) {
        int n = tasks.size(), m = shifts.size();
        vector<long long> sum(n);
        sum[0] = tasks[0];
        vector<int> ans(m);
        for (int i = 1; i < n; i++) {
            sum[i] = sum[i - 1] + tasks[i];
        }
        int l = 0;
        long long pre = 0;
        for (int i = 0; i < m; i++) {
            if (shifts[i] >= sum[n - 1] - pre) {
                l = 0;
                pre = 0;
                ans[i] = 0;
            } else {
                l = upper_bound(sum.begin(), sum.end(), pre + shifts[i]) - sum.begin();
                pre += shifts[i];
                ans[i] = n - l;
            }
        }
        return ans;
    }
};
```

<h2 id="3p">3p</h2>

```python
class Solution:
    def countTasks(self, tasks: List[int], shifts: List[int]) -> List[int]:
        n = len(tasks)
        m = len(shifts)
        sum = [0] * n
        sum[0] = tasks[0]
        for i in range(1, n):
            sum[i] = sum[i - 1] + tasks[i]
        l = 0
        pre = 0
        ans = [0] * m
        for i in range(m):
            if (shifts[i] >= sum[n - 1] - pre):
                l = 0
                pre = 0
                ans[i] = 0
            else:
                l = bisect.bisect_right(sum, pre + shifts[i])
                pre += shifts[i]
                ans[i] = n - l
        return ans
```

<h2 id="3g">3g</h2>

```go
func countTasks(tasks []int, shifts []int) []int {
    n := len(tasks)
    m := len(shifts)
    sum := make([]int64, n)
    sum[0] = int64(tasks[0])
    for i := 1; i < n; i++ {
        sum[i] = sum[i - 1] + int64(tasks[i])
    }
    ans := make([]int, m)
    l := 0
    var pre int64 = int64(0)
    for i := 0; i < m; i++ {
        if int64(shifts[i]) >= sum[n - 1] - pre {
            pre = int64(0)
            l = 0
            ans[i] = 0
        } else {
            l = sort.Search(len(sum), func(j int) bool {
                return sum[j] > pre + int64(shifts[i])
            })
            pre += int64(shifts[i])
            ans[i] = n - l
        }
    }
    return ans
}
```

<h2 id="4c">4c</h2>

```c++
template <typename T>
struct Fenwick {
    const int n;
    vector<T> a;
    Fenwick(int n) : n(n), a(n + 1) {}
    void add(int x, T v) {
        for (int i = x; i <= n; i += i & -i) {
            a[i] += v;
        }
    }
    T sum(int x) {
        T ans = 0;
        for (int i = x; i > 0; i -= i & -i) {
            ans += a[i];
        }
        return ans;
    }
    T rangeSum(int l, int r) {
        return sum(r) - sum(l - 1);
    }
    void rangeAdd(int l, int r, T x) {
        add(l, x);
        add(r + 1, -x);
    }
};
class Solution {
public:
    long long countRatioSubarrays(vector<int>& nums, int a, int b) {
        int n = nums.size();
        vector<long long> sum(n + 1);
        vector<long long> z;
        for (int i = 0; i < n; i++) {
            if (nums[i] % 2 == 1) {
                nums[i] = a;
            } else {
                nums[i] = -b;
            }
        }
        for (int i = 1; i <= n; i++) {
            sum[i] = sum[i - 1] + nums[i - 1];
            z.emplace_back(sum[i]);
        }
        z.emplace_back(0);
        sort(z.begin(), z.end());
        z.erase(unique(z.begin(), z.end()), z.end());
        for (int i = 0; i <= n; i++) {
            sum[i] = lower_bound(z.begin(), z.end(), sum[i]) - z.begin() + 1;
        }
        Fenwick<int> fen(z.size());
        fen.add(sum[0], 1);
        long long ans = 0;
        for (int i = 1; i <= n; i++) {
            ans += fen.sum(sum[i]);
            fen.add(sum[i], 1);
        }
        return ans;
    }
};
```

<h2 id="4p">4p</h2>

```python
class Fenwick:
    def __init__(self, n: int):
        self.n = n
        self.a = [0] * (n + 1)
    def add(self, idx: int, val: int):
        i = idx
        while i <= self.n:
            self.a[i] += val
            i += i & -i
    def sum(self, idx: int) -> int:
        i = idx
        ans = 0
        while i > 0:
            ans += self.a[i]
            i -= i & -i
        return ans

class Solution:
    def countRatioSubarrays(self, nums: list[int], a: int, b: int) -> int:
        n = len(nums)
        sum = [0] * (n + 1)
        z = []
        for i in range(n):
            nums[i] = a if nums[i] % 2 == 1 else -b
        for i in range(1, n + 1):
            sum[i] = sum[i - 1] + nums[i - 1]
            z.append(sum[i])
        z.append(0)
        z = sorted(set(z))
        for i in range(0, n + 1):
            sum[i] = bisect.bisect_left(z, sum[i]) + 1
        fen = Fenwick(len(z))
        fen.add(sum[0], 1)
        ans = 0
        for i in range(1, n + 1):
            ans += fen.sum(sum[i])
            fen.add(sum[i], 1)
        return ans

```

<h2 id="4g">4g</h2>

```go
type Fenwick struct {
    n int
    a []int
}
func NewFenwick(n int) *Fenwick {
    return &Fenwick{
        n: n,
        a: make([]int, n + 1),
    }
}
func (f *Fenwick) add(idx, val int) {
    for i := idx; i <= f.n; i += i & -i {
        f.a[i] += val
    }
}
func (f *Fenwick) sum(idx int) int {
    res := 0
    for i := idx; i > 0; i -= i & -i {
        res += f.a[i]
    }
    return res
}
func countRatioSubarrays(nums []int, a int, b int) int64 {
    n := len(nums)
    sum := make([]int, n + 1)
    z := make([]int, 0)
    for i := 0; i < n; i++ {
        if nums[i] % 2 == 1 {
            nums[i] = a
        } else {
            nums[i] = -b
        }
    }
    for i := 1; i <= n; i++ {
        sum[i] = sum[i - 1] + nums[i - 1]
        z = append(z, sum[i])
    }
    z = append(z, 0)
    slices.Sort(z)
    z = slices.Compact(z)
    for i := 0; i <= n; i++ {
        sum[i] = sort.Search(len(z), func(idx int) bool {
            return sum[i] <= z[idx]
        }) + 1
    }
    fen := NewFenwick(len(z))
    fen.add(sum[0], 1)
    ans := int64(0)
    for i := 1; i <= n; i++ {
        ans += int64(fen.sum(sum[i]))
        fen.add(sum[i], 1)
    }
    return ans
}
```