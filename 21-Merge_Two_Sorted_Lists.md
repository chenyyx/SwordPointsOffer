## 1 - 题目

[LeetCode- No.21-合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例1:**

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_1.jpg" alt="21_merge_lists_1" style="zoom:50%;" />

```shell
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例2:**

```shell
输入：l1 = [], l2 = []
输出：[]
```

**示例3:**

```shell
输入：l1 = [], l2 = [0]
输出：[0]
```

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 < Node.val <= 100`
- `l1` 和 `l2`  均按 **非递减顺序** 排列

## 2 - 思路

思路参考 - [No.21-题解](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/yi-kan-jiu-hui-yi-xie-jiu-fei-xiang-jie-di-gui-by-/)

本题使用递归实现。

- 终止条件：两条链表分别名为 `l1` 和 `l2` ，当 `l1` 为空或 `l2` 为空时结束。
- 返回值：每一层调用都返回排序好的链表头。
- 如何递归：我们判断 `l1` 和 `l2` 头节点哪个更小，然后较小的节点的 `next` 指针指向**其余节点的合并结果。（调用递归）**

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_1.jpg" alt="21_merge_lists_py_1" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_2.jpg" alt="21_merge_lists_py_2" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_3.jpg" alt="21_merge_lists_py_3" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_4.jpg" alt="21_merge_lists_py_4" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_5.jpg" alt="21_merge_lists_py_5" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_6.jpg" alt="21_merge_lists_py_6" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_7.jpg" alt="21_merge_lists_py_7" style="zoom:50%;" />

<img src="/Users/chenyao/Desktop/复习/ml/SwordPointsOffer/imgs/21_merge_lists_py_8.jpg" alt="21_merge_lists_py_8" style="zoom:50%;" />

### Python 题解

#### py-1 不新建一个新的链表

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1: return l2  # 终止条件，直到两个链表都空
        if not l2: return l1
        if l1.val <= l2.val:  # 递归调用
            l1.next = self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1,l2.next)
            return l2
```

下面是本题的递归过程：

```shell
// (1, 1)：代表第一次进入递归函数，并且从第一个口进入，记录进入前链表的状态
merge(1,1): 1->4->5->null, 1->2->3->6->null
		// 因为 l1.val <= l2.val，所以 l1.next 是下面递归的结果
    merge(2,2): 4->5->null, 1->2->3->6->null
    	//因为 l1.val > l2.val，所以 l2.next 是下面递归的结果
    	merge(3,2): 4->5->null, 2->3->6->null
    		merge(4,2): 4->5->null, 3->6->null
    			merge(5,1): 4->5->null, 6->null
    				merge(6,1): 5->null, 6->null
    					merge(7): null, 6->null
    					return l2
    				l1.next --- 5->6->null, return l1
    			l1.next --- 4->5->6->null, return l1
    		l2.next --- 3->4->5->6->null, return l2
    	l2.next --- 2->3->4->5->6->null, return l2
    l2.next --- 1->2->3->4->5->6->null, return l2
l1.next --- 1->1->2->3->4->5->6->null, return l1
```



#### py-2 - 创建两个链表，一个负责保存头节点，一个负责记录比较后的结果

创建 2 个链表：

- result - 保存头节点
- cur - 记录比较后的结果，在每次比较后，将 cur.next 指向较小的结果。然后，将 cur 赋值为最后面的指针。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        result = cur = ListNode(0)
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        cur.next = l1 or l2
        return result.next
```



### C++ 题解

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL) {
            return l2;
        }
        if (l2 == NULL) {
            return l1;
        }
        if (l1->val <= l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
};
```

















