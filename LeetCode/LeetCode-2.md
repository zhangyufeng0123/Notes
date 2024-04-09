# 2. 两数相加

## 题面

![image-20230729223926822](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting@main/mini/202307292239841.png)

## Sample

### Input

```
l1 = [2,4,3], l2 = [5,6,4]
```



![img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting@main/mini/202307292240074.jpg)

### Output

```
[7,0,8]
```

### Explain

```
342 + 465 = 807.
```

### 数据范围

- 每个链表中的节点数在范围`[1,100]`内
- 0 $\le$ Node.val $\le$ 9
- 题目数据保证链表表示的数字不含前导零



## 思路

这道题我们可以把链表比做字符串，每次计算的时候都拿出两个字符串都拿出一个数进行相加操作，如果有进位我们就标记。当一边的数都取完时，我们就将进位标志与没有取完的数进行相加，直到另一边的数也取完。最后不要忘记检查如果两边的数都取完之后进位是否为1，如果为1，还需要加一个节点。

其实还有一种方法，那就是将两条链表上的数都取下来，用数组存储，然后对其进行相加操作，最后再创建一条新的链表存储数组上的数。



## 代码

```c++
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* root = new ListNode(0, l1), *p = root;
        int tmp = 0;
        while(l1 != nullptr && l2 != nullptr){
            l1->val += l2->val + tmp;
            tmp = l1->val / 10;
            l1->val %= 10;
            l1 = l1->next;
            l2 = l2->next;
            p = p->next;
        }
        if(l1 == nullptr){
            p->next = l2;
        }
        ListNode *t = p;
        p = p->next;
        while(p != nullptr){
            p->val += tmp;
            tmp = p->val / 10;
            p->val %= 10;
            p = p->next;
            t = t->next;
        }
        if(tmp == 1){
            ListNode *q = new ListNode(1);
            t->next = q;
        }
        return root->next;
    }
};
```

