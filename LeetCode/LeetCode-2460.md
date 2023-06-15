# LeetCode-2460(1224)

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230605210512.png)

## Sample

### Input

```
nums = [1,2,2,1,1,0]
```

### Output

```
[1,4,2,0,0,0]
```

### Explain

```
执行以下操作：
- i = 0: nums[0] 和 nums[1] 不相等，跳过这步操作。
- i = 1: nums[1] 和 nums[2] 相等，nums[1] 的值变成原来的 2 倍，nums[2] 的值变成 0 。数组变成 [1,4,0,1,1,0] 。
- i = 2: nums[2] 和 nums[3] 不相等，所以跳过这步操作。
- i = 3: nums[3] 和 nums[4] 相等，nums[3] 的值变成原来的 2 倍，nums[4] 的值变成 0 。数组变成 [1,4,0,2,0,0] 。
- i = 4: nums[4] 和 nums[5] 相等，nums[4] 的值变成原来的 2 倍，nums[5] 的值变成 0 。数组变成 [1,4,0,2,0,0] 。
执行完所有操作后，将 0 全部移动到数组末尾，得到结果数组 [1,4,2,0,0,0] 。
```

### 提示

- 2 <= nums.length <= 2000
- 0 <= nums[i] <= 1000

## 做法

模拟。我们开辟一个新的初始化为`0`的数组进行存储需要返回的数值。我们在`nums`数组中每遍历到相同的邻居时，我们就将它存储到目标数组中，并对目标数组的索引进行`+1`操作。最后我们再将`nums`数组中的最后一位添加到目标数组中，这个时候不用考虑该数字是否为0。
- 如果这个数字为0，将它加入末尾之后不影响末尾都是0的要求
- 如果这个数字不为0，它也不影响末尾都是0的要求

## 代码

```C++
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int idx = 0, len = nums.size();
        vector<int> arr(len, 0);
        for(int i = 0; i < len - 1; i++){
            if(nums[i] == 0) continue;
            else if(nums[i] == nums[i + 1]){
                nums[i + 1] = 0;
                arr[idx++] = nums[i] * 2;
            }else {
                arr[idx++] = nums[i];
            }
        }
        arr[idx] = nums.back();
        return arr;
    }
};
```

```C++
// O(1)的空间复杂度
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int idx = 0, len = nums.size();
        for(int i = 0; i < len - 1; i++){
            if(nums[i] == nums[i + 1]){
                nums[i] *= 2;
                nums[i + 1] = 0;
            }
        }
        
        for(int i = 0; i < len; i++){
            if(nums[i]) nums[idx++] = nums[i];
        }
        for(int i = idx; i < len; i++){
            nums[i] = 0;
        }
        return nums;
    }
};
```
