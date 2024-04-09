# AcWing 4741.魔法百合井



## 题面

![image-20230722212959647](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting@main/mini/202307222129681.png)

## Sample

### Input

```
2
5
20
```

### Output

```
Case #1: 5
Case #2: 15
```

### Explain

```
对于 Case 1，井中一共有 5 朵百合花。
最佳方案是一个接一个的连续向井中投入 5 个硬币，这样我们可以一个接一个的得到 5 朵百合花。
一共需要花费 5 个硬币。
对于 Case 2，井中一共有 15 朵百合花。
```

### 数据范围

- 1 $$\le$$ T $$\le$$ 100
- 1 $$\le$$ L $$\le$$ $$10^5$$ 

## 想法

刚开始我的想法是dp，但是我一开始想的是n个硬币能买多少话，这样做比较麻烦，因为最后还需要利用二分来查找想要买m朵花需要几个硬币。

后面转换了思路，即，计算要买m朵花需要几个硬币。`dp[花的数量] = 硬币的数量`

有题目我们可以知道买花的方案主要有以下三个

1. 一个硬币换一朵花
2. 四个硬币记录当前有多少花
3. 二个硬币换上次记录的花的数量

由上面三个方案我们可以得知

1. 第三个方案一定得在第二个方案后面，如果没有第二个方案，那么第三个方案一直用二个硬币换0朵花
2. 第二个方案后面可以跟很多个第三个方案
3. 在第二个方案后面，第一个方案不影响第三个方案，因此可以穿插着来

## 代码

```c++
#include<iostream>
#include<algorithm>
#include<cstring>

const int INF = 1e9 + 7;
const int maxn = 1e5 + 10;
int dp[maxn];

void init(){
    memset(dp, INF, sizeof(dp));
    dp[0] = 0;
    for(int i = 1; i < maxn; i++){
        dp[i] = std::min(dp[i - 1] + 1, dp[i]);
        for(int j = i + i, k = 1; j < maxn; j += i, k++){	// 该for循环我们默认dp[i]的时候用了四个硬币记录当前的花树
            dp[j] = std::min(dp[j], dp[i] + 2 * k + 4);
        }
    }
}

int main(){
    init();
    int T, idx = 0;
    std::cin >> T;
    while(T--){
        int n;
        std::cin >> n;
        std::cout << "Case #" << ++idx << ": " << dp[n] << std::endl;
    }
    
    return 0;
}
```



