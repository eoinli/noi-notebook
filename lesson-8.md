---
description: 利用多种程序结构学习判断质数的常用算法，初步学习多重循环
---

# Lesson 8

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、判断质数

质数（prime number）或素数，指的是**因数只有 1 和本身**的**非 0 非 1** 的整数。基于这一点，有两种方法来判断一个给定的整数是不是质数。

### 1.1 基本算法

```cpp
#include <iostream>
using namespace std;

int main() {

    int num;
    cin >> num;

    bool flag = true;
    // 判断是不是合数
    for (int i=2; i<num; i++){ 
        if (num % i == 0) { 
            flag = false;
            break;
        }
    }
	
    // 根据判断结果输出值
    if (flag)
        cout << "Y" <<endl;
    else
        cout << "N" <<endl;

    return 0;
}
```

在本例中，程序通过判断所输入数是否能被从 2 到 n-1 中任何一个整数整除，如果能则为合数，否则则为质数。

### 1.2 算法优化

如果所输入的数 n 存在非 1 且非自身的因数，那么这些因数会成对出现，或者等于 $$\sqrt{n}$$。举例而言，6 既等于 2 与 3 的乘积，也等于 3 和 2 的乘积。尽管这两个因数只是交换了一下位置，但在程序中被分别运算了两次（枚举到 2 的时候取余为 3，枚举到 3 的时候取余为 2）。为了减小计算机运行的负担，这两次运算可以被减小为一次运算。

假设我们输入的数为 100，当枚举到因数 4 的时候，取余结果为 25；枚举到因数 10 的时候，取余结果为 10；接着枚举到 25 的时候，取余结果为 4。这时会发现，所取余的结果与曾被枚举的数重复了。我们发现，为了避免重复，只需要枚举到 $$\sqrt{100}$$ 也就是 10 即可。以此类推可以发现，判断条件可以设为枚举到所输入数的算术平方根时为止。

由于程序中乘法运算相较于开方会更快，可以用循环变量的平方小于或等于所输入数的方法加快运行速度。

```cpp
#include <iostream>
using namespace std;

int main() {

    int num;
    cin >> num;

    bool flag = true; 
    // 修改判断条件
    for (int i=2; i*i<=num; i++){ 
        if (num % i == 0) { 
            flag = false;
            break;
        }
    }

    if (flag)
        cout << "Y" <<endl;
    else
        cout << "N" <<endl;

    return 0;
}
```

## 二、多重循环应用

### 2.1 正方形矩阵

**单重循环**

```cpp
#include <iostream>
using namespace std;

int main() {
	int n;
	cin >> n;
    // 总共输出n^2的字符
	for (int i = 1; i <= n*n; i++) {
		cout << n;
        // 判断是否应该换行
		if (i % n == 0) {
			cout << endl;
		}
	}
	return 0;
}
```

**双重循环**

```cpp
#include <iostream>
using namespace std;

int main() {
	int n;
	cin >> n;
    // 输出n行
	for (int i = 1; i <= n; i++){
        // 输出n列
		for (int j = 1; j <= n; j++) {
			cout << n;
            // 判断是否应该换行
			if (j == n) cout << endl;
		}
	}
	return 0;
}
```

### 2.2 一般矩阵

```cpp
#include <iostream>
using namespace std;

int main() {
	int n, m; char s;
	cin >> n >> m >> s;
    // 输出n行
	for (int i = 1; i <= n; i++){
        // 输出m列
		for (int j = 1; j <= m; j++) {
			cout << s;
            // 判断是否应该换行
			if (j == m) cout << endl;
		}
	}
	return 0;
}
```

## 三、思考题

已知正整数 n 是两个不同的质数的乘积，试求出两者中较大的那个质数。

```cpp
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    for (int i = 2; i <= n; i++) {
    	if (n % i == 0) { 
	        cout << n / i;
	        break;
	    }
	}
	
    return 0;
}
```

{% hint style="info" %}
根据**算术基本定理**（fundamental theorem of arithmetic），又称**正整数的唯一分解定理**，任何一个正整数数能且只能分解为一组质数的乘积。这一定理最早由欧几里得给出证明。
{% endhint %}

可知，若输入的数满足题目条件，它就只能分解为两个质数的乘积。所以在比它小且大于 1 的自然数中，只有那两个数能整除它，之间不可能再有任何合数或质数能整除它了，因为最小的能整除它的合数已经是他本身了。

同时，由于从小到大枚举的效率更高，且只用知道较小的质数，较大的质数就可直接通过整除得到。
