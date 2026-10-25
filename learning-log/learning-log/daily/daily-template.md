# 2026-10-25（周日）

## 今日计划

> 从对应月份的 `plans/month-01/README.md` 里复制当天的任务

- [ ] 算法题：两数之和
- [ ] 学习：git用法哈希
- [ ] 练习：

## 今天做了什么

git python环境验证
用C++哈希写了两数之和的

## 遇到的问题和解决方法

| 问题 | 原因 | 解决方法 |
| 不理解哈希怎么用| 对语法不熟练|在Ai的帮助下学习了哈希的简单用法，本题是可以解决的 |
| | | |

## 今天学到的一个知识点

---

用自己的话写下来，以后复习面试题时用：
哈希表，unordered_map<int,int>name
第一个是键第二个是值

-# C++ 哈希表语法速记

力扣题里常用两种哈希容器：

- `unordered_map`：存 **键 → 值**，例如“数字 → 下标”。
- `unordered_set`：只存 **键**，适合判断某个数是否出现过。

## 1. `unordered_map`：键值对

```cpp
#include <unordered_map>
using namespace std;

unordered_map<int, int> seen;  // 数字 -> 下标
```

| 操作       | 语法                         | 含义                  |
| ---------- | ---------------------------- | --------------------- |
| 存入或更新 | `seen[x] = i;`               | 数字 `x` 对应下标 `i` |
| 判断存在   | `seen.find(x) != seen.end()` | 表里有 `x`            |
| 判断不存在 | `seen.find(x) == seen.end()` | 表里没有 `x`          |
| 取值       | `seen[x]`                    | 取得 `x` 对应的值     |
| 删除       | `seen.erase(x);`             | 删除键 `x`            |
| 元素数量   | `seen.size()`                | 表中键值对的数量      |

`find` 的常用写法：

```cpp
if (seen.find(x) != seen.end()) {
    // 找到了 x
}
```

> 注意：如果 `x` 不存在，`seen[x]` 会自动创建一个键值对。因此，判断是否存在时用 `find`；确认存在后再用 `seen[x]` 取值。

## 2. `unordered_set`：只记录是否出现

```cpp
#include <unordered_set>
using namespace std;

unordered_set<int> visited;

visited.insert(5);                     // 存入 5
if (visited.find(5) != visited.end()) { // 判断 5 是否存在
    // 找到了
}
visited.erase(5);                      // 删除 5
```

## 3. 面试题里怎么选

- 只关心“这个数出现过吗” → `unordered_set`。
- 还需要知道“它对应的下标、次数等信息” → `unordered_map`。

### 例子：两数之和

哈希表一开始是空的。遍历数组时，先在已经存入的数字中寻找配对；找不到再存当前数字。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> seen;  // 数字 -> 下标

        for (int i = 0; i < nums.size(); i++) {
            int need = target - nums[i];

            if (seen.find(need) != seen.end()) {
                return {seen[need], i};
            }

            seen[nums[i]] = i;
        }

        return {};
    }
};
```

`seen[nums[i]] = i;` 的意思是“记住当前数字及其下标”，供后面的数字查找。先查再存，也能避免当前元素和自己配对。

## 4. 复杂度

哈希表的查找、插入、删除平均为 **$O(1)$**。两数之和只遍历一次数组，平均时间复杂度为 **$O(n)$**，额外空间复杂度为 **$O(n)$**。

---

## 明天的任务

## 时间允许的话再刷一道

## 状态

- 学习时长： 2小时
- 完成度： /3
