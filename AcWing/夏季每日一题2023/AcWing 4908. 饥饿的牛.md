# AcWing 4908. 饥饿的牛

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230610103819.png)

## Sample

### Input

```
2 5
1 10
5 10
```

### Output

```
5
```

### Explain

```
10捆干草在第 1
天早上被送到了牛棚，所以贝茜第 1∼5天都有干草吃。
```

### 数据范围

- 1 ≤ N ≤ $10^5$
- 1 ≤ T ≤ $10^{14}$
- 1 ≤ $d_i$ ≤ $10^{14}$
- 1 ≤ $b_i$ ≤ $10^9$


## 思路

看到时间有序，那么我们只要从前往后遍历即可，然后在遍历的时候维护当前干草最多能支撑牛吃到什么时候。

## 代码

```C++
#include<iostream>

using namespace std;

const int maxn = 1e5 + 10;

int main(){
    long long N, T;
    scanf("%lld %lld", &N, &T);
    
    long long res = 0, days = 0;
    
    while(N--){
        long long b, d;
        scanf("%lld %lld", &d, &b);
        if(d > days){   // 如果当前时间大于干草支撑的时间
            days = d + b - 1;   // -1是因为d这一天也要吃一份
        }else{
            days += b;
        }
        res += b;
        if(days >= T){  // 如果已经到T时间了，后面就没必要继续计算
            res -= (days - T);
            break;
        }
    }
    
    printf("%lld\n", res);
    
    return 0;
}
```
