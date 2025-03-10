---
comments: true
difficulty: 中等
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0700-0799/0789.Escape%20The%20Ghosts/README.md
tags:
    - 数组
    - 数学
---

# [789. 逃脱阻碍者](https://leetcode.cn/problems/escape-the-ghosts)

[English Version](/solution/0700-0799/0789.Escape%20The%20Ghosts/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>你在进行一个简化版的吃豆人游戏。你从 <code>[0, 0]</code> 点开始出发，你的目的地是&nbsp;<code>target = [x<sub>target</sub>, y<sub>target</sub>]</code> 。地图上有一些阻碍者，以数组 <code>ghosts</code> 给出，第 <code>i</code> 个阻碍者从&nbsp;<code>ghosts[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>&nbsp;出发。所有输入均为 <strong>整数坐标</strong> 。</p>

<p>每一回合，你和阻碍者们可以同时向东，西，南，北四个方向移动，每次可以移动到距离原位置 <strong>1 个单位</strong> 的新位置。当然，也可以选择 <strong>不动</strong> 。所有动作 <strong>同时</strong> 发生。</p>

<p>如果你可以在任何阻碍者抓住你 <strong>之前</strong> 到达目的地（阻碍者可以采取任意行动方式），则被视为逃脱成功。如果你和阻碍者 <strong>同时</strong> 到达了一个位置（包括目的地）&nbsp;<strong>都不算</strong>&nbsp;是逃脱成功。</p>

<p>如果不管阻碍者怎么移动都可以成功逃脱时，输出 <code>true</code> ；否则，输出 <code>false</code> 。</p>
&nbsp;

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>ghosts = [[1,0],[0,3]], target = [0,1]
<strong>输出：</strong>true
<strong>解释：</strong>你可以直接一步到达目的地 (0,1) ，在 (1, 0) 或者 (0, 3) 位置的阻碍者都不可能抓住你。 
</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>ghosts = [[1,0]], target = [2,0]
<strong>输出：</strong>false
<strong>解释：</strong>你需要走到位于 (2, 0) 的目的地，但是在 (1, 0) 的阻碍者位于你和目的地之间。 
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>ghosts = [[2,0]], target = [1,0]
<strong>输出：</strong>false
<strong>解释：</strong>阻碍者可以和你同时达到目的地。 
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 &lt;= ghosts.length &lt;= 100</code></li>
	<li><code>ghosts[i].length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>同一位置可能有 <strong>多个阻碍者</strong> 。</li>
	<li><code>target.length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>target</sub>, y<sub>target</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

## 解法

### 方法一：曼哈顿距离

对于任意一个阻碍者，如果它到目的地的曼哈顿距离小于等于你到目的地的曼哈顿距离，那么它就可以在你到达目的地之前抓住你。因此，我们只需要判断所有阻碍者到目的地的曼哈顿距离是否都大于你到目的地的曼哈顿距离即可。

时间复杂度 $O(n)$，空间复杂度 $O(1)$。其中 $n$ 为阻碍者的数量。

<!-- tabs:start -->

```python
class Solution:
    def escapeGhosts(self, ghosts: List[List[int]], target: List[int]) -> bool:
        tx, ty = target
        return all(abs(tx - x) + abs(ty - y) > abs(tx) + abs(ty) for x, y in ghosts)
```

```java
class Solution {
    public boolean escapeGhosts(int[][] ghosts, int[] target) {
        int tx = target[0], ty = target[1];
        for (var g : ghosts) {
            int x = g[0], y = g[1];
            if (Math.abs(tx - x) + Math.abs(ty - y) <= Math.abs(tx) + Math.abs(ty)) {
                return false;
            }
        }
        return true;
    }
}
```

```cpp
class Solution {
public:
    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) {
        int tx = target[0], ty = target[1];
        for (auto& g : ghosts) {
            int x = g[0], y = g[1];
            if (abs(tx - x) + abs(ty - y) <= abs(tx) + abs(ty)) {
                return false;
            }
        }
        return true;
    }
};
```

```go
func escapeGhosts(ghosts [][]int, target []int) bool {
	tx, ty := target[0], target[1]
	for _, g := range ghosts {
		x, y := g[0], g[1]
		if abs(tx-x)+abs(ty-y) <= abs(tx)+abs(ty) {
			return false
		}
	}
	return true
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

```ts
function escapeGhosts(ghosts: number[][], target: number[]): boolean {
    const [tx, ty] = target;
    for (const [x, y] of ghosts) {
        if (Math.abs(tx - x) + Math.abs(ty - y) <= Math.abs(tx) + Math.abs(ty)) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- end -->
