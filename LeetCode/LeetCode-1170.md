# LeetCode-1170(1432)

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230610104927.png)

## Sample

### Input

```
queries = ["bbb","cc"], words = ["a","aa","aaa","aaaa"]
```

### Output

```
[1,2]
```

### Explain

```
第一个查询 f("bbb") < f("aaaa")，第二个查询 f("aaa") 和 f("aaaa") 都 > f("cc")。
```

### 数据范围

- 1 <= queries.length <= 2000
- 1 <= words.length <= 2000
- 1 <= queries[i].length, words[i].length <= 10
- queries[i][j]、words[i][j] 都由小写英文字母组成

## 思路

这题有几个必要条件得获取
- `words`中每个单词`word`的`f(word)`的值得知道
- `queries`中每个单词`query`的`f(query)`的值也得知道

这两个条件大家的时间复杂度应该都是一样的，$O(nm)$，其中，`n`是字符串数组的长度，`m`是字符串长度。

那么我们获取上面的之后应该怎么快速的获得`query`对应的值呢？有一种暴力解法就是每获取一个`query`的`f`值，我们就对比一边所有的`f(word)`的值，然后输出结果，但是这样的事件复杂度为$O(n^2)$，时间复杂度偏高。

但是我们可以换个思路来做这道题。如果`f(words)`是有序的，这道题就可以变成查询每个`f(query)`插入`f(words)`中的位置，那么我们一般怎么求位置呢？用二分，我们可以用二分的方法来确定`f(query)`对应的位置。根据这样的方法，排序的时间复杂度为$O(nlogn)$，二分的时间复杂度为$O(nlogn)$，总的时间复杂度为$O(nlogn)$

## 代码

```C++
class Solution {
public:
    int f(string s){
        int res = 1, idx = 0;
        for(int i = 1; i < s.size(); i++){
            if(s[i] == s[idx]) res++;
            else if(s[i] < s[idx]){
                res = 1;
                idx = i;
            }
        }
        return res;
    }

    int binary_search(vector<int> &arr, int target){
        int left = -1, right = arr.size();
        while(left + 1 < right){
            int mid = (left + right) >> 1;
            if(arr[mid] <= target) left = mid;
            else right = mid;
        }
        return left + 1;
    }

    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
        int len_queries = queries.size(), len_words = words.size();
        vector<int> res(len_queries, 0);
        vector<int> f_queries(len_queries, 0), f_words(len_words, 0);
        for(int i = 0; i < len_queries; i++) f_queries[i] = f(queries[i]);
        for(int i = 0; i < len_words; i++) f_words[i] = f(words[i]);

        sort(f_words.begin(), f_words.end());
        for(int i = 0; i < len_queries; i++){
            res[i] = len_words - binary_search(f_words, f_queries[i]);
        }

        return res;
    }
};
```
