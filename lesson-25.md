---
description: 理解埃氏筛法的思想、有关约数的定理并学习实现对应效果的代码
---

# Lesson 25

## 一、埃氏筛及其应用

### 1.1 埃氏筛

**埃拉托斯特尼筛法**（sieve of Eratosthenes），简称**埃氏筛**，是用来筛选一定范围内的质数的一种算法，被广泛运用。其原理是，从 2 开始，将每个质数的各个倍数标记成合数。埃氏筛是列出小质数最有效的方法之一。

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e7+5;
int n;
bool a[MAXN]; // 用于标记是否是合数

void range_prime(int x) {
    for (int i = 2; i <= x/i; i++) {
        if (a[i]) continue; // 如果标记过则为合数，因此要跳过
        for (int j = i+i; j <= x; j+=i) // 否则为质数，因此遍历到末尾
            a[j] = 1; // 将倍数全部标记为合数
    }
}

int main() {
    
    cin >> n;
    range_prime(n);
    for (int i = 2; i <= n; i++)
        if (!a[i]) // 输出所有剩下的质数
            cout << i << " ";
    
	return 0;
}
```

### 1.2 分解质因数

可以利用埃氏筛的思想实现分解质因数的效果。

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;

void divide(int x) {
    for (int i = 2; i <= x/i; i++) {
        if (!(x%i)) { // 如果能被整除
            while(!(x%i)) x /= i; // 则不断往下整除，直到不能再被整除
            cout << i << " "; // 此时输出质因子
        }
    }
    if (x > 1) cout << x << " "; // 如果质因子大于sqrt(x)则直接输出
}

int main() {
    
    cin >> n;
    divide(n);

	return 0;
}
```

试分析以上代码的原理。

可以通过一个例子来理解这段代码：假设我们输入的数是 18，遍历从 2 开始，$18\div 2=9$，不能继续往下整除，则输出质因子 2；接着遍历 3，$9\div 3 = 3$，继续往下整除 $3\div 3 = 1$，不能继续往下整除，则输出质因子 3；再往后遍历，无论遍历到多少都无法整除，所以得出最终的结果为 2 和 3。

对于一个数的质因子大于其算术平方根的情况，我们可以这样理解：一般地，一个数的质因数有两种情况：

1. 全部都小于或等于这个数的算术平方根；
2. 只有一个数大于，其他都小于或等于这个数的算术平方根。

第二条中所描述的这一现象的原因是，设这个数为 n，则当 n 有两个或以上个数的质因数大于 $\sqrt{n}$ 时，则这些质因数的乘积会大于 n 本身。而我们知道，一个数的任意两个质因数的乘积是不会大于这个数本身的，因此一个数不可能有两个或以上个数的质因数大于该数的算术平方根。

### 1.3 求约数个数

对于一个正整数 N（$$N\ne 1$$），它可以被分解为 $$P^{c_{1}}_{1} \times P^{c_{2}}_{2} \times P^{c_{3}}_{3} \times \dots \times P^{c_{k}}_{k}$$，其中每一个 P 都是该数的一个质因子，每一个 c 都是当前质因子的指数，那么 N 的约数个数满足——

$$
(c_{1} + 1) \times (c_{2} + 2) \times (c_{3} + 3) \times \dots \times (c_{k} + 1)
$$

```cpp
#include <bits/stdc++.h>
using namespace std;

pair <int, int> a[10005]; // 绑定两个int类型的数据到一个元素中
int cnt, n, ans = 1;

int main() {
    
    cin >> n;
    for (int i = 2; i <= n/i; i++) {
        if (!(n%i)) { // 如果n|i
            a[cnt].first = i; // 则将底数赋值为i
            while (!(n%i)) { // 如果n|i则循环
                a[cnt].second++; // 指数加1
                n /= i; // 自取商
            }
            cnt++; // 底数加1
        }
    }
    // 如果质因子大于sqrt(x)则将底数赋值为n，指数赋值为1
    if (n > 1) a[cnt].first = n, a[cnt].second = 1;
    for (int i = 0; i < cnt; i++)
        ans *= a[i].second+1; // 结果自乘以指数+1的值
    cout << ans;
	return 0;
}
```

### 1.4 求约数之和

对于一个正整数 N（$$N\ne 1$$），它可以被分解为 $$P^{c_{1}}_{1} \times P^{c_{2}}_{2} \times P^{c_{3}}_{3} \times \dots \times P^{c_{k}}_{k}$$，其中每一个 P 都是该数的一个质因子，每一个 c 都是当前质因子的指数，那么 N 的约数之和满足——

$$
(P^{0}_{1} + P^{1}_{1} + \dots + P^{c_{1}}_{1}) \times (P^{0}_{2} + P^{1}_{2} + \dots + P^{c_{2}}_{2}) \times (P^{0}_{3} + P^{1}_{3} + \dots + P^{c_{3}}_{3}) \times \dots \times (P^{0}_{k} + P^{1}_{k} + \dots + P^{c_{k}}_{k})
$$
