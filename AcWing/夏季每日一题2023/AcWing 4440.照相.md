# AcWing 4440.照相

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230615231310.png)

## Sample

### Input
```
14
GGGHGHHGHHHGHG
```

### Output

```
1
```

### Explain

```
在这个例子中，只需反转由前六头奶牛组成的前缀即可。
   GGGHGHHGHHHGHG （反转前）
-> HGHGGGHGHHHGHG （反转后）
在反转之前，四头更赛牛处于偶数位置。
反转后，六头更赛牛处于偶数位置。不可能使得超过六头更赛牛处于偶数位置
```

### 数据范围

2≤N≤2×$10^5$ ，N 为偶数

## 思路

首先我们得注意到题目中所给的条件。
1. 使得尽可能多的更塞牛处于队列中的偶数位置（即，使得尽可能多的`G`在字符串的奇数位置）
2. 能让他的奶牛的**偶数**长的**前缀**进行反转

由上我们可以得知，相邻的两个牛是彼此绑定的，比如样例中的`GGGHGHHGHHHGHG`，我们可以划分为`GG|GH|GH|HG|HH|HG|HG`，可以看到两种牛的组合有四种，分别是`GG`、`HH`、`GH`、`HG`，其中有两种反转不影响结果，即`GG`和`HH`。那为了达到目的，我们需要让相邻的组合除了是`GG`和`HH`之外都是`HG`。因此我们得将`GH`都进行翻转。

既然知道了这个，我们还得判断他怎么翻转。首先我们得知道，翻转的时候每一个位置都会发生变化，如果前`n`组的组合不一致，那么翻转之后也不一致，因此我们在翻转的时候得确保前`n`组的组合一致。

如果样例是`HGGH`我们如果要翻转使得更多的`G`在偶数位置，那么我们首先得翻转`HG`使得要翻转的组合是一致的。

由上我们可以得知翻转的特性。那么什么时候需要翻转呢？那就是相邻两组组合不一致，那么我们就要将前面的组合进行翻转变成使其变成和后面的组合一致。这样翻转就能使得字符串所有的牛的组合是一样的，最后如果组合都是`GH`，那么我们还需要对其翻转一次，使得其变成`HG`

## 代码

```cpp
#include<iostream>
#include<string>

using namespace std;

int main(){
    int n;
    string str;
    cin >> n;
    cin >> str;
    int tmp = -1, res = -1;
    for(int i = 0; i < n; i += 2){
        if(str[i] == 'G' && str[i + 1] == 'H'){
            if(tmp == -1){
                tmp = 0;
                res = 0;
            }else if(tmp == 1){
                tmp = 0;
                res++;
            }
        }else if(str[i] == 'H' && str[i + 1] == 'G'){
            if(tmp == -1){
                tmp = 1;
                res = 0;
            }else if(tmp == 0){
                tmp = 1;
                res++;
            }
        }
    }
    if(tmp == 0) res++;
    cout << res << endl;
    return 0;
}
```