# Q1. 最近的可用无人机
模拟

<a href="#1c">c++</a>

<a href="#1p">python</a>

<a href="#1g">go</a>

# Q2. 交通灯的最大等待时间
贪心

<a href="#2c">c++</a>

<a href="#2p">python</a>

<a href="#2g">go</a>

# Q3. 工位的最大间隔
贪心

<a href="#3c">c++</a>

<a href="#3p">python</a>

<a href="#3g">go</a>

# Q4. 电梯请求 III
状压DP

<a href="#4c">c++</a>

<a href="#4p">python</a>

<a href="#4g">go</a>

# 代码

<h2 id="1c">1c</h2>

```c++
class Solution {
public:
    int nearestDrone(vector<vector<int>>& drones, vector<int>& target) {
        int n = drones.size();
        int minn = 1e9, idx = -1;
        for (int j = 0; j < n; j++) {
            int c = abs(drones[j][0] - target[0]) + abs(drones[j][1] - target[1]);
            if (c <= drones[j][2]) {
                if (minn > c) {
                    minn = c;
                    idx = j;
                }
            }
        }
        return idx;
    }
};
```

<h2 id="1p">1p</h2>

```python
class Solution:
    def nearestDrone(self, drones: list[list[int]], target: list[int]) -> int:
        n = len(drones)
        minn = 10**9
        idx = -1
        for j in range(n):
            c = abs(drones[j][0] - target[0]) + abs(drones[j][1] - target[1])
            if c <= drones[j][2]:
                if minn > c:
                    minn = c
                    idx = j
        return idx
```

<h2 id="1g">1g</h2>

```go
func nearestDrone(drones [][]int, target []int) int {
    abs := func(x int) int {
        if x < 0 {
            return -x
        }
        return x
    }

    n := len(drones)
    minn := 1000000000
    idx := -1

    for j := 0; j < n; j++ {
        dist := abs(drones[j][0]-target[0]) + abs(drones[j][1]-target[1])
        if dist <= drones[j][2] && dist < minn {
            minn = dist
            idx = j
        }
    }
    return idx
}
```

<h2 id="2c">2c</h2>

```c++
class Solution {
public:
    int minPenalty(int period, vector<int>& lights, vector<int>& arrivalTime) {
        int n = lights.size();
        int m = arrivalTime.size();
        for (int i = 0; i < m; i++) {
            arrivalTime[i] %= period;
        }
        sort(arrivalTime.begin(), arrivalTime.end());
        sort(lights.begin(), lights.end());
        int ans = 0;
        for (int i = 0; i < m; i++) {
            if (arrivalTime[i] < lights[n - 1]) {
                continue;
            }
            ans = max(ans, period - arrivalTime[i]);
        }
        return ans;
    }
};
```

<h2 id="2p">2p</h2>

```python
class Solution:
    def minPenalty(self, period: int, lights: list[int], arrivalTime: list[int]) -> int:
        n = len(lights)
        m = len(arrivalTime)
        for i in range(m):
            arrivalTime[i] %= period
        arrivalTime.sort()
        lights.sort()
        ans = 0
        max_light = lights[-1]
        for i in range(m):
            if arrivalTime[i] < max_light:
                continue
            ans = max(ans, period - arrivalTime[i])
        return ans
```

<h2 id="2g">2g</h2>

```go
import "sort"

func minPenalty(period int, lights []int, arrivalTime []int) int {
    n := len(lights)
    m := len(arrivalTime)
    for i := 0; i < m; i++ {
        arrivalTime[i] %= period
    }
    sort.Ints(arrivalTime)
    sort.Ints(lights)
    ans := 0
    maxLight := lights[n-1]
    for i := 0; i < m; i++ {
        if arrivalTime[i] < maxLight {
            continue
        }
        if period-arrivalTime[i] > ans {
            ans = period - arrivalTime[i]
        }
    }
    return ans
}
```

<h2 id="3c">3c</h2>

```c++
class Solution {
public:
    int maximumGap(string skill, string station) {
        int n = skill.size();
        int m = station.size();
        vector<int> pre(n), suf(n);
        for (int i = 0, j = 0; i < m && j < n; i++) {
            if (station[i] == skill[j]) {
                pre[j] = i;
                j++;
            }
        }
        for (int i = m - 1, j = n - 1; i >= 0 && j >= 0; i--) {
            if (station[i] == skill[j]) {
                suf[j] = i;
                j--;
            }
        }
        int ans = 0;
        for (int i = 0; i < n - 1; i++) {
            ans = max(ans, suf[i + 1] - pre[i]);
        }
        return ans;
    }
};
```

<h2 id="3p">3p</h2>

```python
class Solution:
    def maximumGap(self, skill: str, station: str) -> int:
        n = len(skill)
        m = len(station)
        pre = [0] * n
        suf = [0] * n
        j = 0
        for i in range(m):
            if j < n and station[i] == skill[j]:
                pre[j] = i
                j += 1
        j = n - 1
        for i in range(m - 1, -1, -1):
            if j >= 0 and station[i] == skill[j]:
                suf[j] = i
                j -= 1
        ans = 0
        for i in range(n - 1):
            ans = max(ans, suf[i + 1] - pre[i])
        return ans
```

<h2 id="3g">3g</h2>

```go
func maximumGap(skill string, station string) int {
    n := len(skill)
    m := len(station)
    pre := make([]int, n)
    suf := make([]int, n)
    j := 0
    for i := 0; i < m && j < n; i++ {
        if station[i] == skill[j] {
            pre[j] = i
            j++
        }
    }
    j = n - 1
    for i := m - 1; i >= 0 && j >= 0; i-- {
        if station[i] == skill[j] {
            suf[j] = i
            j--
        }
    }
    ans := 0
    for i := 0; i < n-1; i++ {
        diff := suf[i+1] - pre[i]
        if diff > ans {
            ans = diff
        }
    }
    return ans
}
```

<h2 id="4c">4c</h2>

```c++
class Solution {
public:
    long long elevatorRequests(int n, int start, vector<vector<int>>& requests) {
        int m = requests.size();
        vector<vector<long long>> dp(1 << m, vector<long long>(m, 1e18));
        for (int i = 0; i < m; i++) {
            dp[1 << i][i] = max((long long)abs(start - requests[i][1]), (long long)requests[i][0]);
        }
        for (int i = 1; i < (1 << m); i++) {
            for (int j = 0; j < m; j++) {
                if (i >> j & 1) {
                    for (int k = 0; k < m; k++) {
                        if (i >> k & 1) {
                            continue;
                        }
                        long long c = max((long long)requests[k][0], (long long)abs(requests[j][1] - requests[k][1]) + dp[i][j]);
                        dp[i | (1 << k)][k] = min(dp[i | (1 << k)][k], c);
                    }
                }
            }
        }
        long long ans = 1e18;
        for (int i = 0; i < m; i++) {
            ans = min(ans, dp[(1 << m) - 1][i]);
        }
        return ans;
    }
};
```

<h2 id="4p">4p</h2>

```python
class Solution:
    def elevatorRequests(self, n: int, start: int, requests: list[list[int]]) -> int:
        m = len(requests)
        INF = 10**18
        dp = [[INF] * m for _ in range(1 << m)]
        for i in range(m):
            dp[1 << i][i] = max(abs(start - requests[i][1]), requests[i][0])
        for mask in range(1, 1 << m):
            for j in range(m):
                if mask >> j & 1:
                    for k in range(m):
                        if mask >> k & 1:
                            continue
                        cost = max(requests[k][0], abs(requests[j][1] - requests[k][1]) + dp[mask][j])
                        if cost < dp[mask | (1 << k)][k]:
                            dp[mask | (1 << k)][k] = cost
        ans = INF
        full = (1 << m) - 1
        for i in range(m):
            if dp[full][i] < ans:
                ans = dp[full][i]
        return ans
```

<h2 id="4g">4g</h2>

```go
func elevatorRequests(n int, start int, requests [][]int) int64 {
    m := len(requests)
    const INF int64 = 1e18
    dp := make([][]int64, 1<<m)
    for i := 0; i < 1<<m; i++ {
        dp[i] = make([]int64, m)
        for j := 0; j < m; j++ {
            dp[i][j] = INF
        }
    }
    for i := 0; i < m; i++ {
        a := absInt(start - requests[i][1])
        b := requests[i][0]
        if int64(a) > int64(b) {
            dp[1<<i][i] = int64(a)
        } else {
            dp[1<<i][i] = int64(b)
        }
    }
    for mask := 1; mask < (1 << m); mask++ {
        for j := 0; j < m; j++ {
            if (mask>>j)&1 == 0 {
                continue
            }
            for k := 0; k < m; k++ {
                if (mask>>k)&1 == 1 {
                    continue
                }
                dist := absInt(requests[j][1] - requests[k][1])
                cost := int64(dist) + dp[mask][j]
                if int64(requests[k][0]) > cost {
                    cost = int64(requests[k][0])
                }
                nextMask := mask | (1 << k)
                if cost < dp[nextMask][k] {
                    dp[nextMask][k] = cost
                }
            }
        }
    }
    full := (1 << m) - 1
    ans := INF
    for i := 0; i < m; i++ {
        if dp[full][i] < ans {
            ans = dp[full][i]
        }
    }
    return ans
}

func absInt(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```