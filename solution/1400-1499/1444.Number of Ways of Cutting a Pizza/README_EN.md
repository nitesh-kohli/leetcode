---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/README_EN.md
rating: 2126
tags:
    - Memoization
    - Array
    - Dynamic Programming
    - Matrix
---

# [1444. Number of Ways of Cutting a Pizza](https://leetcode.com/problems/number-of-ways-of-cutting-a-pizza)

[中文文档](/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/README.md)

## Description

<p>Given a rectangular pizza represented as a <code>rows x cols</code>&nbsp;matrix containing the following characters: <code>&#39;A&#39;</code> (an apple) and <code>&#39;.&#39;</code> (empty cell) and given the integer <code>k</code>. You have to cut the pizza into <code>k</code> pieces using <code>k-1</code> cuts.&nbsp;</p>

<p>For each cut you choose the direction: vertical or horizontal, then you choose a cut position at the cell boundary and cut the pizza into two pieces. If you cut the pizza vertically, give the left part of the pizza to a person. If you cut the pizza horizontally, give the upper part of the pizza to a person. Give the last piece of pizza to the last person.</p>

<p><em>Return the number of ways of cutting the pizza such that each piece contains <strong>at least</strong> one apple.&nbsp;</em>Since the answer can be a huge number, return this modulo 10^9 + 7.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/images/ways_to_cut_apple_1.png" style="width: 500px; height: 378px;" /></strong></p>

<pre>
<strong>Input:</strong> pizza = [&quot;A..&quot;,&quot;AAA&quot;,&quot;...&quot;], k = 3
<strong>Output:</strong> 3 
<strong>Explanation:</strong> The figure above shows the three ways to cut the pizza. Note that pieces must contain at least one apple.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> pizza = [&quot;A..&quot;,&quot;AA.&quot;,&quot;...&quot;], k = 3
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> pizza = [&quot;A..&quot;,&quot;A..&quot;,&quot;...&quot;], k = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= rows, cols &lt;= 50</code></li>
	<li><code>rows ==&nbsp;pizza.length</code></li>
	<li><code>cols ==&nbsp;pizza[i].length</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>pizza</code> consists of characters <code>&#39;A&#39;</code>&nbsp;and <code>&#39;.&#39;</code> only.</li>
</ul>

## Solutions

### Solution 1

<!-- tabs:start -->

```python
class Solution:
    def ways(self, pizza: List[str], k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if k == 0:
                return int(s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0)
            ans = 0
            for x in range(i + 1, m):
                if s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0:
                    ans += dfs(x, j, k - 1)
            for y in range(j + 1, n):
                if s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0:
                    ans += dfs(i, y, k - 1)
            return ans % mod

        mod = 10**9 + 7
        m, n = len(pizza), len(pizza[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(pizza, 1):
            for j, c in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + int(c == 'A')
        return dfs(0, 0, k - 1)
```

```java
class Solution {
    private int m;
    private int n;
    private int[][] s;
    private Integer[][][] f;
    private final int mod = (int) 1e9 + 7;

    public int ways(String[] pizza, int k) {
        m = pizza.length;
        n = pizza[0].length();
        s = new int[m + 1][n + 1];
        f = new Integer[m][n][k];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = pizza[i - 1].charAt(j - 1) == 'A' ? 1 : 0;
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
            }
        }
        return dfs(0, 0, k - 1);
    }

    private int dfs(int i, int j, int k) {
        if (k == 0) {
            return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int ans = 0;
        for (int x = i + 1; x < m; ++x) {
            if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                ans = (ans + dfs(x, j, k - 1)) % mod;
            }
        }
        for (int y = j + 1; y < n; ++y) {
            if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                ans = (ans + dfs(i, y, k - 1)) % mod;
            }
        }
        return f[i][j][k] = ans;
    }
}
```

```cpp
class Solution {
public:
    int ways(vector<string>& pizza, int k) {
        const int mod = 1e9 + 7;
        int m = pizza.size(), n = pizza[0].size();
        vector<vector<vector<int>>> f(m, vector<vector<int>>(n, vector<int>(k, -1)));
        vector<vector<int>> s(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = pizza[i - 1][j - 1] == 'A' ? 1 : 0;
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
            }
        }
        function<int(int, int, int)> dfs = [&](int i, int j, int k) -> int {
            if (k == 0) {
                return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int ans = 0;
            for (int x = i + 1; x < m; ++x) {
                if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                    ans = (ans + dfs(x, j, k - 1)) % mod;
                }
            }
            for (int y = j + 1; y < n; ++y) {
                if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                    ans = (ans + dfs(i, y, k - 1)) % mod;
                }
            }
            return f[i][j][k] = ans;
        };
        return dfs(0, 0, k - 1);
    }
};
```

```go
func ways(pizza []string, k int) int {
	const mod = 1e9 + 7
	m, n := len(pizza), len(pizza[0])
	f := make([][][]int, m)
	s := make([][]int, m+1)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, k)
			for h := range f[i][j] {
				f[i][j][h] = -1
			}
		}
	}
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1]
			if pizza[i-1][j-1] == 'A' {
				s[i][j]++
			}
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		if k == 0 {
			if s[m][n]-s[m][j]-s[i][n]+s[i][j] > 0 {
				return 1
			}
			return 0
		}
		ans := 0
		for x := i + 1; x < m; x++ {
			if s[x][n]-s[x][j]-s[i][n]+s[i][j] > 0 {
				ans = (ans + dfs(x, j, k-1)) % mod
			}
		}
		for y := j + 1; y < n; y++ {
			if s[m][y]-s[m][j]-s[i][y]+s[i][j] > 0 {
				ans = (ans + dfs(i, y, k-1)) % mod
			}
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, 0, k-1)
}
```

```ts
function ways(pizza: string[], k: number): number {
    const mod = 1e9 + 7;
    const m = pizza.length;
    const n = pizza[0].length;
    const f = new Array(m).fill(0).map(() => new Array(n).fill(0).map(() => new Array(k).fill(-1)));
    const s = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            const x = pizza[i - 1][j - 1] === 'A' ? 1 : 0;
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
        }
    }
    const dfs = (i: number, j: number, k: number): number => {
        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }
        if (k === 0) {
            return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
        }
        let ans = 0;
        for (let x = i + 1; x < m; ++x) {
            if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                ans = (ans + dfs(x, j, k - 1)) % mod;
            }
        }
        for (let y = j + 1; y < n; ++y) {
            if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                ans = (ans + dfs(i, y, k - 1)) % mod;
            }
        }
        return (f[i][j][k] = ans);
    };
    return dfs(0, 0, k - 1);
}
```

<!-- tabs:end -->

<!-- end -->
