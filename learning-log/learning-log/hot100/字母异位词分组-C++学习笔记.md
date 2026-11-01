# 字母异位词分组：C++ 学习笔记

## 题目要做什么

把由相同字母、相同出现次数构成的单词放进同一组。例如：

```text
输入： ["eat", "tea", "tan", "ate", "nat", "bat"]
分组： ["eat", "tea", "ate"], ["tan", "nat"], ["bat"]
```

只看单词长度和“有哪些字母”不够：`"aab"` 与 `"abb"` 长度相同，也都有 `a`、`b`，但各字母的出现次数不同。

## 核心思路：给同组单词造同一个键

把单词的字母排序：

```text
"eat" → "aet"
"tea" → "aet"
"ate" → "aet"
```

排序后的字符串相同，原单词就属于同一组。用哈希表保存：

```text
排序后的字符串 → 一组原单词
"aet" → ["eat", "tea", "ate"]
"ant" → ["tan", "nat"]
```

## 完整代码

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> groups;

        for (const string& word : strs) {
            string key = word;
            sort(key.begin(), key.end());
            groups[key].push_back(word);
        }

        vector<vector<string>> ans;
        for (const auto& entry : groups) {
            ans.push_back(entry.second);
        }

        return ans;
    }
};
```

## 逐行理解关键语法

### 1. 哈希表的类型

```cpp
unordered_map<string, vector<string>> groups;
```

- `unordered_map<键的类型, 值的类型>` 是哈希表。
- 键是 `string`，例如 `"aet"`。
- 值是 `vector<string>`，即一组字符串，例如 `{"eat", "tea"}`。
- `vector` 可以先理解成长度可变的数组。

### 2. 遍历输入

```cpp
for (const string& word : strs)
```

`strs` 是函数参数 `vector<string>& strs`，保存题目传入的所有单词。冒号可以读成“从 `strs` 中依次取出”；`word` 是在这句 `for` 里定义的变量，每轮代表当前单词。

三种常见写法：

| 写法 | 是否复制元素 | 是否能修改容器里的原元素 |
| --- | --- | --- |
| `string word` | 是 | 否 |
| `string& word` | 否 | 是 |
| `const string& word` | 否 | 否 |

这里只读取原单词，因此用 `const string&`。

### 3. 复制并排序

```cpp
string key = word;
sort(key.begin(), key.end());
```

`key` 是 `word` 的独立副本。排序 `key` 后，原单词 `word` 不变。例如 `word` 仍是 `"tea"`，`key` 变成 `"aet"`。

`begin()` 和 `end()` 是函数，必须带括号；`sort(key.begin, key.end)` 是错误写法。

### 4. 找到组并加入原单词

```cpp
groups[key].push_back(word);
```

分成两步理解：

1. `groups[key]`：用键 `key` 取出对应的 `vector<string>`。如果键不存在，`[]` 会自动创建一个空的 `vector<string>`。
2. `.push_back(word)`：把原单词放到这个数组末尾。

例如第一次处理 `"eat"`：

```text
groups["aet"] → 新建空数组 [] → 加入 "eat" → ["eat"]
```

再处理 `"tea"`：

```text
groups["aet"] → 找到已有的 ["eat"] → 加入 "tea" → ["eat", "tea"]
```

这里的 `[]` 是**按键取值**，不是按数字下标取元素。它与 `nums[0]` 外形相似，但 `groups["aet"]` 中的 `"aet"` 是哈希表的键。

### 5. 取出所有组

```cpp
for (const auto& entry : groups) {
    ans.push_back(entry.second);
}
```

`entry` 在 `for` 语句中定义，每轮是哈希表里的一个键值对。`auto` 让编译器推断它的类型；`const &` 表示只读取且不复制。

- `entry.first`：键，例如 `"aet"`。
- `entry.second`：值，例如 `{"eat", "tea", "ate"}`。

答案只需要各组原单词，所以把每个 `entry.second` 加入 `ans`。`ans` 的类型是 `vector<vector<string>>`，可以理解为“装着多个单词组的数组”。

## 这次写代码时的易错点

1. `sort` 的参数应写成 `key.begin()`、`key.end()`，不能漏掉 `()`。
2. `key` 定义在第一个 `for` 的 `{}` 内，出了这个范围就不能继续使用。
3. 哈希表里可能有多个组，不能只写一次 `ans.push_back(groups[key])`；要遍历整个 `groups`。
4. 函数返回类型是 `vector<vector<string>>`，最后要 `return ans;`。
5. 范围 `for` 后不要误加分号：`for (...) ;` 会产生空循环。

## 复杂度与复习提示

设有 `n` 个单词，每个单词的最长长度为 `k`。对每个单词排序需要 `O(k log k)`，总时间约为 **`O(n × k log k)`**；哈希表及分组占用额外空间约 **`O(n × k)`**。

复习时先不看代码，试着回答：

1. 为什么 `"eat"` 和 `"tea"` 能进入同一组？
2. 为什么要复制 `word` 给 `key`？
3. `groups[key]` 在键不存在时会发生什么？
4. `entry.first` 和 `entry.second` 分别是什么？
5. 为什么需要第二个 `for` 循环？
