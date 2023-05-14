# LeetCode-2441(1168分)

## 与对应负数同时存在的最大正整数

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230513235239.png)

## Sample

### Input
```
nums = [-1,2,-3,3]
```

### Output
```
3
```

### Explain
```
3 是数组中唯一一个满足题目要求的 k 。
```

### 提示
- `1 <= nums.length <= 1000`
- `-1000 <= nums[i] <= 1000`
- `nums[i] != 0`

## 思路

题目的意思简洁明了，让我们在数组中找到一个正整数，并且它的相反数也在这个数组内，用另一种话来讲，就是在一个数组中找到两个数使得这两个数相等。
那我们应该怎么找呢？
- 暴力点的方法就是用$O(n^2)$的时间复杂度用两层for循环找，数据范围是1000，不会超时。
- 次一点的方法就是用$O(nlogn)$的时间复杂度，先排序再利用双指针来寻找两个数相加为0
- 最后一个方法就是用$O(n)$的时间复杂度，利用哈希表来存储已经遍历过的数字，之后每遍历一个点的时候就判断它的相反数是否已经出现过即可


## 代码

```cpp
class Solution {
public:
    int findMaxK(vector<int>& nums) {
        unordered_map<int, int> hash;
        int res = -1;
        for(auto num : nums){
            if(hash[-1 * num]) res = max(res, abs(num));
            hash[num] = 1;
        }
        return res;
    }
};
```