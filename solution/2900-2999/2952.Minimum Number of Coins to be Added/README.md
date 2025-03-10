---
comments: true
difficulty: 中等
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2900-2999/2952.Minimum%20Number%20of%20Coins%20to%20be%20Added/README.md
rating: 1784
tags:
    - 贪心
    - 数组
    - 排序
---

# [2952. 需要添加的硬币的最小数量](https://leetcode.cn/problems/minimum-number-of-coins-to-be-added)

[English Version](/solution/2900-2999/2952.Minimum%20Number%20of%20Coins%20to%20be%20Added/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给你一个下标从 <strong>0 </strong>开始的整数数组 <code>coins</code>，表示可用的硬币的面值，以及一个整数 <code>target</code> 。</p>

<p>如果存在某个 <code>coins</code> 的子序列总和为 <code>x</code>，那么整数 <code>x</code> 就是一个 <strong>可取得的金额 </strong>。</p>

<p>返回需要添加到数组中的<strong> 任意面值 </strong>硬币的 <strong>最小数量 </strong>，使范围 <code>[1, target]</code> 内的每个整数都属于 <strong>可取得的金额</strong> 。</p>

<p>数组的 <strong>子序列</strong> 是通过删除原始数组的一些（<strong>可能不删除</strong>）元素而形成的新的 <strong>非空</strong> 数组，删除过程不会改变剩余元素的相对位置。</p>

<p>&nbsp;</p>

<p><strong class="example">示例 1：</strong></p>

<pre>
<strong>输入：</strong>coins = [1,4,10], target = 19
<strong>输出：</strong>2
<strong>解释：</strong>需要添加面值为 2 和 8 的硬币各一枚，得到硬币数组 [1,2,4,8,10] 。
可以证明从 1 到 19 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 2 。
</pre>

<p><strong class="example">示例 2：</strong></p>

<pre>
<strong>输入：</strong>coins = [1,4,10,5,7,19], target = 19
<strong>输出：</strong>1
<strong>解释：</strong>只需要添加一枚面值为 2 的硬币，得到硬币数组 [1,2,4,5,7,10,19] 。
可以证明从 1 到 19 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 1 。</pre>

<p><strong class="example">示例 3：</strong></p>

<pre>
<strong>输入：</strong>coins = [1,1,1], target = 20
<strong>输出：</strong>3
<strong>解释：</strong>
需要添加面值为 4 、8 和 16 的硬币各一枚，得到硬币数组 [1,1,1,4,8,16] 。 
可以证明从 1 到 20 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 3 。</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coins.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coins[i] &lt;= target</code></li>
</ul>

## 解法

### 方法一：贪心 + 构造

我们不妨假设当前需要构造的金额为 $s$，且我们已经构造出了 $[0,...,s-1]$ 内的所有金额。如果此时有一个新的硬币 $x$，我们把它加入到数组中，可以构造出 $[x, s+x-1]$ 内的所有金额。

接下来，分类讨论：

-   如果 $x \le s$，那么我们可以将上面两个区间合并，得到 $[0, s+x-1]$ 内的所有金额。
-   如果 $x \gt s$，那么我们就需要添加一个面值为 $s$ 的硬币，这样可以构造出 $[0, 2s-1]$ 内的所有金额。然后我们再考虑 $x$ 和 $s$ 的大小关系，继续分析。

因此，我们将数组 $coins$ 按照升序排序，然后从小到大遍历数组中的硬币。对于每个硬币 $x$，我们进行上述分类讨论，直到 $s > target$ 为止。

时间复杂度 $O(n \times \log n)$，空间复杂度 $O(\log n)$。其中 $n$ 是数组的长度。

<!-- tabs:start -->

```python
class Solution:
    def minimumAddedCoins(self, coins: List[int], target: int) -> int:
        coins.sort()
        s = 1
        ans = i = 0
        while s <= target:
            if i < len(coins) and coins[i] <= s:
                s += coins[i]
                i += 1
            else:
                s <<= 1
                ans += 1
        return ans
```

```java
class Solution {
    public int minimumAddedCoins(int[] coins, int target) {
        Arrays.sort(coins);
        int ans = 0;
        for (int i = 0, s = 1; s <= target;) {
            if (i < coins.length && coins[i] <= s) {
                s += coins[i++];
            } else {
                s <<= 1;
                ++ans;
            }
        }
        return ans;
    }
}
```

```cpp
class Solution {
public:
    int minimumAddedCoins(vector<int>& coins, int target) {
        sort(coins.begin(), coins.end());
        int ans = 0;
        for (int i = 0, s = 1; s <= target;) {
            if (i < coins.size() && coins[i] <= s) {
                s += coins[i++];
            } else {
                s <<= 1;
                ++ans;
            }
        }
        return ans;
    }
};
```

```go
func minimumAddedCoins(coins []int, target int) (ans int) {
	slices.Sort(coins)
	for i, s := 0, 1; s <= target; {
		if i < len(coins) && coins[i] <= s {
			s += coins[i]
			i++
		} else {
			s <<= 1
			ans++
		}
	}
	return
}
```

```ts
function minimumAddedCoins(coins: number[], target: number): number {
    coins.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0, s = 1; s <= target; ) {
        if (i < coins.length && coins[i] <= s) {
            s += coins[i++];
        } else {
            s <<= 1;
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- end -->
