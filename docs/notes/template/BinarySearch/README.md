# 二分查找算法

## 简介
二分查找是计算机科学中最基本、最有用的算法之一，在基础算法的学习中是非常重要的。
二分查找的最基本问题是在有序数组里查找一个特定的元素。

## 应用

- 在半有序（旋转有序或者是山脉）数组里查找元素；
- 确定一个有范围的整数
- 需要查找的目标元素满足某个特定的性质。

## 模板

### 模板一
把待搜索区间的左边界设置为`left`,右边界下标设置为`right`。

把待搜索区间 `[left, right]` 分为 3 个部分：
- `mid` 位置（只有 1 个元素）；
- `[left, mid - 1]` 里的所有元素；
- `[mid + 1, right]` 里的所有元素；

于是，二分查找就是不断地在区间 `[left, right]` 里根据 `left` 和 `right` 的中间位置 `mid = (left + right) / 2` 的元素大小，
也就是看 `nums[mid]` 与 `target` 的大小关系：
- 如果 `nums[mid] == target` ，返回 `mid`；
- 如果 `nums[mid] > target` ，由于数组有序，`mid` 以及 `mid` 右边的所有元素都大于 `target`，目标元素一定在区间 `[left, mid - 1]` 里，因此设置 `right = mid - 1`；
- 如果 `nums[mid] < target` ，由于数组有序，`mid` 以及 `mid` 左边的所有元素都小于 `target`，目标元素一定在区间 `[mid + 1, right]` 里，因此设置 `left = mid + 1`。

```java
class Solution {

    public int search(int[] nums, int target) {
        // 特殊用例判断
        int len = nums.length;
        if (len == 0) {
            return -1;
        }
        // 在 [left, right] 区间里查找 target
        int left = 0;
        int right = len - 1;
        while (left <= right) {
            // 为了防止 left + right 整形溢出，写成如下形式
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                // 下一轮搜索区间：[left, mid - 1]
                right = mid - 1;
            } else {
                // 此时：nums[mid] < target
                // 下一轮搜索区间：[mid + 1, right]
                left = mid + 1;
            }
        }
        return -1;
    }
}

```

**注意事项：**

1. 思考策略是：永远去想下一轮目标元素应该在哪个区间里：
- 如果目标元素在区间 `[left, mid - 1]` 里，就需要设置设置 `right = mid - 1`；
- 如果目标元素在区间 `[mid + 1, right]` 里，就需要设置设置 `left = mid + 1`；

2. 二分查找算法是典型的「减治思想」的应用，我们使用二分查找将待搜索的区间逐渐缩小，以达到「缩减问题规模」的目的；
3. 循环可以继续的条件是 `while (left <= right)`，特别地，当 `left == right` 即当待搜索区间里只有一个元素的时候，查找也必须进行下去；
4. `int mid = (left + right) / 2;` 在 `left + right` 整形溢出的时候，`mid` 会变成负数，回避这个问题的办法是写成 `int mid = left + (right - left) / 2;`