# 283. 移动零：C++ 学习笔记

## 题目要求

把数组中的所有 `0` 移到末尾，同时保持非零元素原来的相对顺序。必须直接修改原数组，不能复制出另一个数组作为结果。

```text
输入： [0, 1, 0, 3, 12]
修改后：[1, 3, 12, 0, 0]
```

## 思路一：非零元素前移，再补零

用 `read` 遍历整个数组，用 `write` 记录下一个非零元素该放的位置。

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int write = 0;

        for (int read = 0; read < nums.size(); read++) {
            if (nums[read] != 0) {
                nums[write] = nums[read];
                write++;
            }
        }

        while (write < nums.size()) {
            nums[write] = 0;
            write++;
        }
    }
};
```

第一轮按原顺序把非零元素写到前面；第二轮把剩余位置填为 `0`。例如 `[0, 1, 0, 3, 12]`，第一轮结束时前面已经是 `[1, 3, 12, ...]`，此时 `write == 3`，再把下标 3、4 写成零。

## 思路二：双指针交换

同样用 `read` 查找非零元素、`write` 指向该元素应放的位置。找到非零元素后，交换两个位置的值。

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int write = 0;

        for (int read = 0; read < nums.size(); read++) {
            if (nums[read] != 0) {
                swap(nums[write], nums[read]);
                write++;
            }
        }
    }
};
```

这次不需要最后补零，因为交换会逐步把前面的零换到后面。非零元素仍按 `read` 遍历到的顺序排列。

### `swap` 在做什么

```cpp
swap(nums[write], nums[read]);
```

它交换数组中两个位置的**值**，不是交换两个下标。例如 `nums = [0, 1, 0, 3]`，当 `write = 0`、`read = 1` 时，交换 `nums[0]` 与 `nums[1]`，得到 `[1, 0, 0, 3]`。如果两个下标相同，就是与自己交换，数组不变。

## 为什么没有 `return`

函数签名是：

```cpp
void moveZeroes(vector<int>& nums)
```

- `void`：函数不返回结果。
- `vector<int>& nums`：`nums` 是引用，函数直接修改传进来的原数组。

因此不需要写 `return nums;`。函数结束后，力扣会检查原数组是否已经修改正确。

## 为什么不反复删除零

对 `vector` 中间的元素使用 `erase`，后面的元素会前移；边遍历边删除还容易跳过连续的零。用双指针写入或交换，逻辑更直接。

## 复杂度

两种方法都只遍历数组，时间复杂度为 **`O(n)`**，额外空间复杂度为 **`O(1)`**。

## 复习时自测

1. `read` 和 `write` 分别负责什么？
2. 为什么“写入再补零”需要第二轮，而“交换”不需要？
3. `swap(nums[write], nums[read])` 交换的是什么？
4. 为什么这题不写 `return nums;`？
5. 手动模拟 `[0, 0, 1, 0, 2]` 的交换过程。
