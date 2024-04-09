# 接雨水

## 题面

![image-20230723165320412](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting@main/mini/202307231653443.png)

## Sample

### Input

```
height = [0,1,0,2,1,0,1,3,2,1,2,1]
```

### Output

```
6
```

### Explain

```
上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。
```

### 数据范围

- `n == height.length`
- `1 $\le$ n $\le$ 2 * $10^4$ 

## 思路

一个洼地肯定是由两侧高和中间低组合而成，因此我们要计算块地能存多少雨水，我们就得知道这块地的两侧能搭多少高。因此我们用两个数组来维护从左到右的最大高度`left`以及从右到左的最大高度`right`。`left`的位置代表着从左到`index`中最高的高度，`right`也是如此。这样我们就能知道一个洼地的左右两侧高度了。

获取到两侧高度之后我们只要再获取中间的高度，我们就能求得储水量。因为木桶原理，我们要求的是两侧高度的较低处与中间的差值。

## 代码

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int len = height.size();
        vector<int> left(height), right(height);
        for(int i = 1; i < len; i++){
            left[i] = max(left[i - 1], left[i]);
        }
        for(int i = len - 2; i >= 0; i--){
            right[i] = max(right[i + 1], right[i]);
        }
        int res = 0;
        for(int i = 0; i < len; i++){
            res += min(left[i], right[i]) - height[i];
        }
        return res;
    }
};
```



