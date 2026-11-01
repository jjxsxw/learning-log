# 最长连续序列：C++ 学习笔记

## 题目要做什么

给定一个未排序的整数数组，找出数字连续的最长序列的长度。数字在原数组中不必相邻。

```text
nums = [100, 4, 200, 1, 3, 2]
最长连续序列：1, 2, 3, 4
答案：4
```

题目要求时间复杂度为 `O(n)`。排序后扫描容易想到，但排序需要 `O(n log n)`；要达到题目要求，可以使用哈希集合。

## 为什么用 `unordered_set`

这题只需要判断“某个整数是否存在”，不用保存它的下标或其他信息，所以用 `unordered_set<int>`。集合还会自动去重：`[1, 2, 2, 3]` 放入集合后只剩 `{1, 2, 3}`，连续序列长度是 3。

```cpp
unordered_set<int> seen;

for (int x : nums) {
    seen.insert(x);
}
```

`insert(x)` 把 `x` 放进集合。判断是否存在：

```cpp
seen.find(x) != seen.end()  // 找到了 x
seen.find(x) == seen.end()  // 没找到 x
```

`find(x)` 返回一个指向元素的迭代器；没找到时返回 `seen.end()`。`end()` 是末尾之后的特殊位置，不是最后一个元素。

集合还有更短的判断方式：`seen.count(x) == 1` 表示存在，`seen.count(x) == 0` 表示不存在。

## 核心思路：只从起点向后数

对于集合里的数 `c`：

1. 如果 `c - 1` **存在**，`c` 不是这段连续序列的起点，跳过。
2. 如果 `c - 1` **不存在**，`c` 是起点；接着查找 `c + 1`、`c + 2`……，直到下一个数不存在。
3. 记录这段长度，并更新最长长度。

例如集合 `{1, 2, 3, 100}`：

| 当前数 | 前一个数是否存在 | 操作 |
| --- | --- | --- |
| 1 | 0 不存在 | 从 1 开始，数到 3，长度为 3 |
| 2 | 1 存在 | 跳过，不重复计数 |
| 3 | 2 存在 | 跳过，不重复计数 |
| 100 | 99 不存在 | 从 100 开始，长度为 1 |

集合的遍历顺序不固定；表格只是为了说明逻辑。建立集合后遍历 `seen`，而不是再次遍历原数组，可以避免重复数字导致同一段序列被反复统计。

## 完整代码

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> seen;

        for (int x : nums) {
            seen.insert(x);
        }

        int ans = 0;

        for (int c : seen) {
            if (seen.find(c - 1) == seen.end()) {
                int current = c;
                int length = 1;

                while (seen.find(current + 1) != seen.end()) {
                    current++;
                    length++;
                }

                ans = max(ans, length);
            }
        }

        return ans;
    }
};
```

## 三个变量分别做什么

- `current`：当前数到哪里了；找到下一个连续数时加一。
- `length`：当前这段连续序列的长度；每发现一个下一个数就加一，遇到新起点时重新设为 1。
- `ans`：到目前为止找到的最长长度；在外层循环之前设为 0，每数完一段后执行 `ans = max(ans, length)`。

例如已经数出长度 3 的序列，`ans = 3`。后来数出长度 5，`ans` 更新为 5；再数出长度 2，`ans` 仍是 5。

## 这次写代码时的易错点

| 写法或问题 | 正确做法 | 原因 |
| --- | --- | --- |
| `for (int c : x)` | `for (int c : seen)` | `x` 是单个整数；`seen` 才是要遍历的集合。 |
| `length = 1`，未声明变量 | `int length = 1` | 首次使用局部变量时需要声明类型。 |
| 用 `if` 查下一个数 | 用 `while` | 需要不断查找，直到连续序列结束。 |
| `unordered_set<int> ans` | `int ans = 0` | 答案是“最长长度”，类型为整数。 |
| `max(length)` | `ans = max(ans, length)` | `max` 比较两个值，结果要存回 `ans`。 |
| 数完后没有返回 | `return ans` | 函数返回类型是 `int`。 |

## 复杂度

设不同数字的个数为 `n`。建集合和查找的平均时间为 `O(n)`：每段连续序列只从起点向后数一次。额外空间为 `O(n)`。

## 复习时自测

1. 为什么 `c - 1` 存在时，可以跳过 `c`？
2. 为什么这里用 `unordered_set`，而两数之和用 `unordered_map`？
3. 为什么查下一个数用 `while`？
4. `length` 和 `ans` 分别应该在哪里初始化、在哪里更新？
5. 对 `[1, 2, 2, 3, 100]`，集合里有什么，最终答案是多少？
