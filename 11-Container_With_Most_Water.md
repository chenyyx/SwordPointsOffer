## 1 - 题目

[No.11 - 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

给你 `n` 个非负整数 `a1，a2，...，an`，每个数代表坐标中的一个点 $(i, a_i)$ 。在坐标内画 `n` 条垂直线，垂直线 `i` 的两个端点分别为 $(i, a_i)$ 和 $(i, 0)$ 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

**示例1:**

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_1.jpg" alt="11_container_water_1" style="zoom:50%;" />

```shell
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例2:**

```shell
输入：height = [1,1]
输出：1
```

**示例3:**

```python
输入：height = [4,3,2,1,4]
输出：16
```

**示例4:**

```shell
输入：height = [1,2,1]
输出：2
```

**提示：**

- `n = height.length`
- $2 <= n <= 3 * 10^4$
- $0 <= height[i] <= 3 * 10^4$

## 2 - 思路

下面思路借鉴 [LeetCode 对应题目题解](https://leetcode-cn.com/problems/container-with-most-water/solution/container-with-most-water-shuang-zhi-zhen-fa-yi-do/)。

- 面积取决于**短板**。
  - 因此，即使长板往内移动时遇到更长的板，矩形的面积也不会改变；遇到更短的板时，面积会变小。
  - 因此，想要面积变大，只能让短板往内移动（因为移动方向固定了），当然也有可能让面积变得更小，但只有这样才存在让面积变大的可能性。
- 无论是移动短板或者长板，我们都只关注移动后的新短板会不会变长，而每次移动的木板都只有三种情况，比原短板短，比原短板长，与原短板相等；如向内移动长板，对于新的木板：
  - 1.比原短板短，则新短板更短。
  - 2.与原短板相等或者比原短板长，则新短板不变。
  - 所以，向内移动长板，一定不能使新短板变长。
- **算法流程：** 设置双指针 $i, j$ 分别位于容器壁两端，根据规则移动指针，并且更新面积最大值 `res` ，直到 `i==j` 时返回 `res` 。

- **指针移动规则与证明：** 每次选定围成水槽两板高度 `h[i], h[j]` 中的短板，向中间收窄 1 格。以下证明：
  - 设每一状态下水槽面积为 $S(i, j),(0<=i<j<n)$ ，由于水槽的实际高度由两板中的短板决定，则可得面积公式 $S(i, j) = min(h[i], h[j]) \times (j-i)$ 。
  - 在每一个状态下，无论长板或短板收窄 1 格，都会导致水槽 **底边宽度 - 1** ：
    - 若向内移动短板，水槽的短板 $min(h[i], h[j])$ 可能变大，因此水槽面积 $S(i, j)$ 可能增大。
    - 若向内移动长板，水槽的短板 $min(h[i], h[j])$ 不变或变小，下个水槽的面积一定小于当前水槽面积。
  - 因此，向内收窄短板可以获取面积最大值。换个角度理解：
    - 若不指定移动规则，所有移动出现的 $S(i, j)$ 的状态数为 $C(n, 2)$ ，即暴力枚举出所有状态。
    - 在状态 $S(i, j)$ 下向内移动短板至 $S(i+1, j)$ （假设 $h[i] < h[j]$），则相当于消去了 $S(i, j-1), S(i, j-2)..., S(i, i+1)$ 状态集合。而所有消去状态的面积一定 $<= S(i, j)$ ：
      - 短板高度：相比 $S(i, j)$ 相同或更短 （$<= h[i]$）
      - 底边宽度：相比 $S(i, j)$ 更短。
    - 因此**所有消去的状态的面积都**  $<S(i, j)$ 。通俗的讲，我们每次向内移动短板，所有的消去状态都**不会导致丢失面积最大值** 。

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_1.jpg" alt="11_container_water_py_1" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_2.jpg" alt="11_container_water_py_2" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_3.jpg" alt="11_container_water_py_3" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_4.jpg" alt="11_container_water_py_4" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_5.jpg" alt="11_container_water_py_5" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_6.jpg" alt="11_container_water_py_6" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_7.jpg" alt="11_container_water_py_7" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/11_container_water_py_8.jpg" alt="11_container_water_py_8" style="zoom:50%;" />

### Python 题解

```python
class Solution(object):
    def maxArea(self, height):
        i, j, res = 0, len(height) - 1, 0
        while i < j:
            if height[i] < height[j]:
                res = max(res, height[i] * (j - i))
                i += 1
            else:
                res = max(res, height[j] * (j - i))
                j -= 1
        return res
```



### C++ 题解

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int area_tmp = 0;
        int area_max = 0;
        while (left < right) {
            area_tmp = (right - left) * (height[left] < height[right] ? height[left] : height[right]);
            if (area_tmp > area_max) {
                area_max = area_tmp;
            }
            if (height[left] < height[right]) {
                left++;
            }
            else {
                right--;
            }
        }
        return area_max;
    }
};
```

