***
#### 题目
##### 589. N叉树的前序遍历
#### 地址
##### https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/
#### 方法一：递归解法
##### 复杂度分析
- 时间复杂度：O(n)，其中 n 是二叉树的节点数。每一个节点恰好被遍历一次。
- 空间复杂度：O(n)，为递归过程中栈的开销，平均情况下为 O(log(n))，最坏情况下树呈现链状，为 O(n)。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func preorder(root *Node) []int {
    ret := make([]int, 0)
    dfs(root, &ret)
    return ret
}

func dfs(node *Node, ret *[]int) {
    if node == nil {
        return
    }

    *ret = append(*ret, node.Val)
    for _, child := range node.Children {
        dfs(child, ret)
    }
}
```
#### 方法二：迭代解法（栈）
##### 复杂度分析
- 时间复杂度：时间复杂度：O(M)，其中 M 是 N 叉树中的节点个数。每个节点只会入栈和出栈各一次。
- 空间复杂度：O(M)。在最坏的情况下，这棵 N 叉树只有 2 层，所有第 2 层的节点都是根节点的孩子。将根节点推出栈后，需要将这些节点都放入栈，共有 M−1 个节点，因此栈的大小为 O(M)。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func preorder(root *Node) []int {
    ret := make([]int, 0)
    if root == nil {
        return ret
    }

    stack := []*Node{root}
    for len(stack) > 0 {
        topNode := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        ret = append(ret, topNode.Val)

        for i := len(topNode.Children) - 1; i >= 0; i-- {
            stack = append(stack, topNode.Children[i])
        }
    }

    return ret
}
```
***
#### 题目
##### 590. N叉树的后序遍历
#### 地址
##### https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/
#### 方法一：递归解法
##### 复杂度分析
- 时间复杂度：O(n)，其中 n 是二叉树的节点数。每一个节点恰好被遍历一次。
- 空间复杂度：O(n)，为递归过程中栈的开销，平均情况下为 O(log(n))，最坏情况下树呈现链状，为 O(n)。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func postorder(root *Node) []int {
    ret := make([]int, 0)
    dfs(root, &ret)
    return ret
}

func dfs(node *Node, ret *[]int) {
    if node == nil {
        return
    }

    for _, child := range node.Children {
        dfs(child, ret)
    }
    *ret = append(*ret, node.Val)
}
```
#### 方法二：迭代解法（栈）
##### 复杂度分析
- 时间复杂度：时间复杂度：O(M)，其中 M 是 N 叉树中的节点个数。每个节点只会入栈和出栈各一次。
- 空间复杂度：O(M)。在最坏的情况下，这棵 N 叉树只有 2 层，所有第 2 层的节点都是根节点的孩子。将根节点推出栈后，需要将这些节点都放入栈，共有 M−1 个节点，因此栈的大小为 O(M)。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func postorder(root *Node) []int {
    ret := make([]int, 0)
    if root == nil {
        return ret
    }

    stack := []*Node{root}
    for len(stack) > 0 {
        topNode := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        ret = append(ret, topNode.Val)
        stack = append(stack, topNode.Children...)
    }
    reverse(ret)

    return ret
}

func reverse(a []int) {
    for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
        a[i], a[j] = a[j], a[i]
    }
}
```
***
#### 题目
##### 429. N 叉树的层序遍历
#### 地址
##### https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/
#### 方法一：利用队列实现广度优先搜索
##### 复杂度分析
- 时间复杂度：O(n)。n 指的是节点的数量。
- 空间复杂度：O(n)。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func levelOrder(root *Node) [][]int {
    ret := make([][]int, 0)
    bfs(root, &ret)
    return ret
}

func bfs(root *Node, ret *[][]int) {
    if root == nil {
        return
    }

    queue := []*Node{root}
    for len(queue) > 0 {
        n := len(queue)
        level := make([]int, n)

        for i := 0; i < n; i++ {
            headNode := queue[0]
            queue = queue[1:]
            level[i] = headNode.Val
            queue = append(queue, headNode.Children...)
        }
        *ret = append(*ret, level)
    }
}
```
#### 方法二：递归
##### 复杂度分析
- 时间复杂度：O(n)。n 指的是节点的数量
- 空间复杂度：正常情况 O(log(n))，最坏情况 O(n)。运行时在堆栈上的空间。
##### Golang实现
``` go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func levelOrder(root *Node) [][]int {
    ret := make([][]int, 0)
    if root == nil {
        return ret
    }

    dfs(root, 0, &ret)
    return ret
}

func dfs(node *Node, level int, ret *[][]int) {
    if len(*ret) <= level {
        *ret = append(*ret, []int{})
    }
    (*ret)[level] = append((*ret)[level], node.Val)

    for _, child := range node.Children {
        dfs(child, level+1, ret)
    }
}
```
***