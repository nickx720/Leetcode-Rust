# Course Schedule II :star::star:
- 题目地址: [https://leetcode-cn.com/problems/course-schedule-ii](https://leetcode-cn.com/problems/course-schedule-ii)
- 执行时间: 4 ms 
- 内存消耗: 3.1 MB
- 通过日期: 2019-03-13 09:19

## 题目内容
<p>现在你总共有 <em>n</em> 门课需要选，记为 <code>0</code> 到 <code>n-1</code>。</p>

<p>在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: <code>[0,1]</code></p>

<p>给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。</p>

<p>可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 2, [[1,0]] 
<strong>输出: </strong><code>[0,1]</code>
<strong>解释:</strong> 总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 <code>[0,1] 。</code></pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> 4, [[1,0],[2,0],[3,1],[3,2]]
<strong>输出: </strong><code>[0,1,2,3] or [0,2,1,3]</code>
<strong>解释:</strong> 总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
     因此，一个正确的课程顺序是 <code>[0,1,2,3]</code> 。另一个正确的排序是 <code>[0,2,1,3]</code> 。
</pre>

<p><strong>说明:</strong></p>

<ol>
	<li>输入的先决条件是由<strong>边缘列表</strong>表示的图形，而不是邻接矩阵。详情请参见<a href="http://blog.csdn.net/woaidapaopao/article/details/51732947" target="_blank">图的表示法</a>。</li>
	<li>你可以假定输入的先决条件中没有重复的边。</li>
</ol>

<p><strong>提示:</strong></p>

<ol>
	<li>这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。</li>
	<li><a href="https://www.coursera.org/specializations/algorithms" target="_blank">通过 DFS 进行拓扑排序</a> - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。</li>
	<li>
	<p>拓扑排序也可以通过 <a href="https://baike.baidu.com/item/%E5%AE%BD%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2/5224802?fr=aladdin&fromid=2148012&fromtitle=%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2" target="_blank">BFS</a> 完成。</p>
	</li>
</ol>


## 解法
```rust
// Author: Netcan @ https://github.com/netcan/Leetcode-Rust
// Zhihu: https://www.zhihu.com/people/netcan

use std::iter;
impl Solution {
    pub fn find_order(num_courses: i32, prerequisites: Vec<Vec<i32>>) -> Vec<i32> {
        let mut graph = iter::repeat(Vec::<i32>::new()).take(num_courses as usize).collect::<Vec<Vec<i32>>>();
        let mut indegree = vec![0; num_courses as usize];

        // 建图
        for edge in &prerequisites {
            let (u, v) = (edge[0], edge[1]);
            graph[v as usize].push(u); // v->u
            indegree[u as usize] += 1;
        }

        // 入度为0的节点
        let mut S = Vec::new();
        for (node, &degree) in indegree.iter().enumerate() {
            if degree == 0 {
                S.push(node as i32);
            }
        }

        let mut order: Vec<i32> = Vec::new();
        while !S.is_empty() {
            let v = S.pop().unwrap();
            order.push(v);
            for &u in &graph[v as usize] {
                indegree[u as usize] -= 1;
                if indegree[u as usize] == 0 {
                    S.push(u);
                }
            }
        }

        // 图是否为空
        if (indegree.iter().all(|&e| e == 0)) {
            order
        } else {
            vec![]
        }
    }
}


```