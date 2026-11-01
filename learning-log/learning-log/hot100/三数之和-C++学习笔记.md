# 15. 三数之和：C++ 学习笔记与面试手撕

## 题目要做什么

从数组中选出**三个不同位置**的数，使它们的和为 `0`。返回所有**不重复的数字组合**；相同数字可以出现两次，但必须来自不同位置。

```text
输入：[-1, 0, 1, 2, -1, -4]
答案：[[-1, -1, 2], [-1, 0, 1]]
```

答案按数字组合去重，而不是按下标组合去重。

## 一、先把三数之和变成两数之和

先固定一个数 `nums[i]`，再从它后面的元素中找两个数，使它们的和为 `-nums[i]`。

例如固定 `-2` 时，另外两个数需要加起来等于 `2`；`0 + 2` 和 `1 + 1` 都可能满足条件。

## 二、为什么先排序

排序后，数字从小到大排列。固定 `nums[i]` 后，令 `left = i + 1`、`right = n - 1`：

- `nums[left] + nums[right] < -nums[i]`：和太小，`left++`，寻找更大的数。
- `nums[left] + nums[right] > -nums[i]`：和太大，`right--`，寻找更小的数。
- 和正好等于 `-nums[i]`：记录这一组，两个指针都移动。

排序还让相同数字挨在一起，便于去重。

## 三、去重有两处

### 固定的数去重

```cpp
if (i > 0 && nums[i] == nums[i - 1]) {
    continue;
}
```

如果当前固定数和上一次相同，跳过这轮，避免重复找出同一批三元组。

### 找到答案后，左右指针去重

正确顺序是：**保存当前答案 → 左右指针各移动一次 → 跳过相邻重复值**。

```cpp
ans.push_back({nums[i], nums[left], nums[right]});
left++;
right--;

while (left < right && nums[left] == nums[left - 1]) {
    left++;
}
while (left < right && nums[right] == nums[right + 1]) {
    right--;
}
```

`nums[left - 1]` 和 `nums[right + 1]` 分别是指针刚刚离开的值。用 `while` 是为了连续跳过多个相同数字。

例如排序后是 `[-2, 0, 0, 2, 2]`，固定 `-2` 时，`[-2, 0, 2]` 只能加入答案一次。

没找到答案时，和太小只移动左指针、和太大只移动右指针；此时不专门跳过重复值也能保证正确。

## 四、完整 C++ 代码

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int n = nums.size();

        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = n - 1;
            int need = -nums[i];

            while (left < right) {
                int sum = nums[left] + nums[right];

                if (sum < need) {
                    left++;
                } else if (sum > need) {
                    right--;
                } else {
                    ans.push_back({nums[i], nums[left], nums[right]});
                    left++;
                    right--;

                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right + 1]) {
                        right--;
                    }
                }
            }
        }

        return ans;
    }
};
```

## 五、这次写代码时的易错点

| 易错写法 | 问题 | 正确做法 |
| --- | --- | --- |
| 不排序就按和的大小移动指针 | 无法保证移动方向正确 | 先 `sort(nums.begin(), nums.end())` |
| 找到一组就 `return` | 会漏掉其他三元组 | 用 `ans.push_back(...)`，最后统一返回 |
| 把 `need` 当作答案里的第三个数 | `need` 是后两个数的**目标和** | 保存 `{nums[i], nums[left], nums[right]}` |
| 找到后不移动指针 | 可能陷入死循环 | `left++`、`right--` |
| 记录答案前先跳过重复值 | 可能改变刚找到的三元组 | 先记录，再移动和去重 |
| 右指针移动后与 `nums[right - 1]` 比 | 比较方向错了 | 与刚离开的 `nums[right + 1]` 比 |
| 把 `return ans` 放在循环里 | 第一轮就结束 | 放在所有循环之后 |

## 六、复杂度

排序是 `O(n log n)`；外层固定一个数，内层双指针最多扫描一遍，合计时间复杂度为 **`O(n²)`**。双指针部分的额外空间为 `O(1)`；排序所用空间取决于实现，答案空间另计。

## 七、面试手撕时怎么说

可以按下面的顺序讲，不需要逐字背诵：

> “题目需要找所有和为零、按数字组合去重的三元组。最直接的方法是枚举三个下标，但时间复杂度是 `O(n³)`。我准备先排序，再固定第一个数，把问题变成从后面找两个数，使它们的和等于固定数的相反数。”

> “对后面的区间使用左右双指针。两数之和小于目标时，移动左指针以增大和；大于目标时，移动右指针以减小和。相等时保存答案，再移动两个指针。”

> “为了避免重复答案，我会跳过重复的固定数；每次找到答案后，也跳过左右两侧连续的重复值。这样每个固定数对应的区间只扫描一遍，总时间是 `O(n²)`。最后用含重复数字的例子检查去重。”

面试时可以重点解释：为什么排序后指针移动有方向，以及为什么要分两处去重。

## 八、复习时自测

1. 固定 `nums[i]` 后，另外两个数的目标和是多少？
2. 和太小时移动哪个指针？为什么？
3. 找到一组后，为什么不能直接 `return`？
4. 外层去重和左右指针去重各解决什么问题？
5. 对 `[-2, 0, 0, 2, 2]`，答案应出现几次？
