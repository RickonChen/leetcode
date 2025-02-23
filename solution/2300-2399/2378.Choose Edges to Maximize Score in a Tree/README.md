# [2378. Choose Edges to Maximize Score in a Tree](https://leetcode.cn/problems/choose-edges-to-maximize-score-in-a-tree)

[English Version](/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>You are given a <strong>weighted</strong> tree consisting of <code>n</code> nodes numbered from <code>0</code> to <code>n - 1</code>.</p>

<p>The tree is <strong>rooted</strong> at node <code>0</code> and represented with a <strong>2D</strong> array <code>edges</code> of size <code>n</code> where <code>edges[i] = [par<sub>i</sub>, weight<sub>i</sub>]</code> indicates that node <code>par<sub>i</sub></code> is the <strong>parent</strong> of node <code>i</code>, and the edge between them has a weight equal to <code>weight<sub>i</sub></code>. Since the root does <strong>not</strong> have a parent, you have <code>edges[0] = [-1, -1]</code>.</p>

<p>Choose some edges from the tree such that no two chosen edges are <strong>adjacent</strong> and the <strong>sum</strong> of the weights of the chosen edges is maximized.</p>

<p>Return <em>the <strong>maximum</strong> sum of the chosen edges</em>.</p>

<p><strong>Note</strong>:</p>

<ul>
	<li>You are allowed to <strong>not</strong> choose any edges in the tree, the sum of weights in this case will be <code>0</code>.</li>
	<li>Two edges <code>Edge<sub>1</sub></code> and <code>Edge<sub>2</sub></code> in the tree are <strong>adjacent</strong> if they have a <strong>common</strong> node.
	<ul>
		<li>In other words, they are adjacent if <code>Edge<sub>1</sub></code> connects nodes <code>a</code> and <code>b</code> and <code>Edge<sub>2</sub></code> connects nodes <code>b</code> and <code>c</code>.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/images/treedrawio.png" style="width: 271px; height: 221px;" />
<pre>
<strong>Input:</strong> edges = [[-1,-1],[0,5],[0,10],[2,6],[2,4]]
<strong>Output:</strong> 11
<strong>Explanation:</strong> The above diagram shows the edges that we have to choose colored in red.
The total score is 5 + 6 = 11.
It can be shown that no better score can be obtained.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/images/treee1293712983719827.png" style="width: 221px; height: 181px;" />
<pre>
<strong>Input:</strong> edges = [[-1,-1],[0,5],[0,-6],[0,7]]
<strong>Output:</strong> 7
<strong>Explanation:</strong> We choose the edge with weight 7.
Note that we cannot choose more than one edge because all edges are adjacent to each other.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>par<sub>0</sub> == weight<sub>0</sub> == -1</code></li>
	<li><code>0 &lt;= par<sub>i</sub> &lt;= n - 1</code> for all <code>i &gt;= 1</code>.</li>
	<li><code>par<sub>i</sub> != i</code></li>
	<li><code>-10<sup>6</sup> &lt;= weight<sub>i</sub> &lt;= 10<sup>6</sup></code> for all <code>i &gt;= 1</code>.</li>
	<li><code>edges</code> represents a valid tree.</li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

**方法一：树形 DP**

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def maxScore(self, edges: List[List[int]]) -> int:
        def dfs(i):
            a = b = 0
            t = 0
            for j in g[i]:
                x, y = dfs(j)
                a += y
                b += y
                t = max(t, x - y + g[i][j])
            b += t
            return a, b

        g = defaultdict(lambda: defaultdict(lambda: -inf))
        for i, (p, w) in enumerate(edges[1:], 1):
            g[p][i] = w
        return max(dfs(0))
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    private Map<Integer, Integer>[] g;

    public long maxScore(int[][] edges) {
        int n = edges.length;
        g = new Map[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            g[i] = new HashMap<>();
        }
        for (int i = 1; i < n; ++i) {
            int p = edges[i][0], w = edges[i][1];
            g[p].put(i, w);
        }
        return dfs(0)[1];
    }

    private long[] dfs(int i) {
        long a = 0, b = 0;
        long t = 0;
        for (int j : g[i].keySet()) {
            long[] s = dfs(j);
            a += s[1];
            b += s[1];
            t = Math.max(t, s[0] - s[1] + g[i].get(j));
        }
        b += t;
        return new long[] {a, b};
    }
}
```

### **C++**

```cpp
using ll = long long;

class Solution {
public:
    vector<unordered_map<int, int>> g;

    long long maxScore(vector<vector<int>>& edges) {
        int n = edges.size();
        g.resize(n + 1);
        for (int i = 1; i < n; ++i) {
            int p = edges[i][0], w = edges[i][1];
            g[p][i] = w;
        }
        return dfs(0).second;
    }

    pair<ll, ll> dfs(int i) {
        ll a = 0, b = 0;
        ll s = 0;
        for (auto& [j, v] : g[i]) {
            auto t = dfs(j);
            a += t.second;
            b += t.second;
            s = max(s, t.first - t.second + v);
        }
        b += s;
        return {a, b};
    }
};
```

### **Go**

```go
func maxScore(edges [][]int) int64 {
	n := len(edges)
	g := make([]map[int]int, n+1)
	for i := range g {
		g[i] = make(map[int]int)
	}
	for i := 1; i < n; i++ {
		p, w := edges[i][0], edges[i][1]
		g[p][i] = w
	}
	var dfs func(i int) []int
	dfs = func(i int) []int {
		a, b := 0, 0
		s := 0
		for j, v := range g[i] {
			t := dfs(j)
			a += t[1]
			b += t[1]
			s = max(s, t[0]-t[1]+v)
		}
		b += s
		return []int{a, b}
	}
	return int64(dfs(0)[1])
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

### **TypeScript**

```ts

```

### **...**

```


```

<!-- tabs:end -->
