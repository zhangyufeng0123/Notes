# LeetCode-1171(1782)

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230611223436.png)

## Sample

### Input

```
head = [1,2,-3,3,1]
```

### Output

```
[3,1]
```

### Explain

```
答案 [1,2,1] 也是正确的。
```

### 数据范围

- 给你的链表中可能有 1 到 1000 个节点。
- 对于链表中的每个节点，节点的值：-1000 <= node.val <= 1000.

## 思路

我们怎么快速找到一段总和为`0`的连续区间呢？一般的做法就是用两个`for`循环遍历数组，然后计算，但是这样的时间复杂度为$O(n^2)$，当数组长度过大时，就会导致时间超限。那么我们可以怎么做呢？

我们可以利用前缀和的思想，在一个前缀和的数组中，如果数组中有两个数相等，索引分别是`first`,`last`，那么在`(first, last]`这个区间内的所有数之和为`0`。这是为什么呢？

假设现在有一个前缀和数组`num`，`num[first] = num[right]`，`num[first]`代表的是`[0, first]`区间的数之和，`num[last]`代表的是`[0, last]`区间的数之和。那么`num[last] - num[first]`代表的就是区间`(first, last]`的数之和，由前面我们可以得到`num[first] = num[last]`，即，区间`(first, last]`的数之和为`0`

因此我们可以通过前缀和的思想，再加上哈希来存储前缀和数组中每一个值的最远位置。这样的时间复杂度为$O(n)$

当然，快速找到区间只是第一步，接下来就是下一步。

为了满足返回最终结果链表中连续的数之和不为`0`，我们可以利用贪心的想法。

我们从头遍历前缀和数组，如果这个数值不唯一，那么我们就获取索引值最大的那个值，然后将两个值之间的数都给删除，删除这些数并不会影响前缀和的性质，为什么呢？参考前面，这个区间的数之和为`0`，所以并不影响。

删掉这些数字之后我们下次寻找就不在删掉的区间中遍历了，因为如果在这个区间中遍历寻找前缀和数组时，如果找到并进行删除操作，那么删除的数组可能会有重叠部分。

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230611230552.png)

我们要删除橙色部分，但是最后的操作删的其实是蓝色的部分，但是橙色部分的和为`0`，这是确定的，但是蓝色部分的和呢？我们并不知道，因此我们当我们删除一段区间之后，我们不用再遍历这个区间内的前缀和，直接从删除区间的末尾进行遍历。

根据这样的操作，最后留下来的数就不会形成连续和为`0`的情况

::: tip 注意
刚开始我们首先得确定从数组起点开始的前缀和为`0`的区间，并对其进行删除操作。否则当遇上输入为`[1, -1]`时，就会出现与结果不一样的答案。
:::

## 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        ListNode* root = new ListNode(0, head), *p = root;
        vector<int> num;
        num.emplace_back(head->val);
        head = head->next;
        while(head != NULL){
            num.emplace_back(num.back() + head->val);
            head = head->next;
        }
        unordered_map<int, int> hash;
        int len = num.size();
        for(int i = 0; i < len; i++){
            hash[num[i]] = i;
        }
        int idx = -1;
        if(hash.count(0)) idx = hash[0];
        for(int i = 0; i <= idx; i++){
            num[i] = 0;
        }
        for(int i = idx + 1; i < len; i++){
            if(hash[num[i]] != i){
                for(int j = i + 1; j <= hash[num[i]]; j++){
                    num[j] = 0;
                }
                i = hash[num[i]];
            }
        }
        ListNode *q = p->next;
        p->next = NULL;
        for(int i = 0; i < len; i++){
            if(q->val && num[i]){
                ListNode *t = new ListNode(q->val);
                p->next = t;
                p = t;
            }
            q = q->next;
        }
        return root->next;
    }
};
```
