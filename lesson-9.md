---
description: 学会关注题目的特殊情况并特殊对待，进一步练习使用多重循环
---

# Lesson 9

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、特判

在第五讲中，已经完成过题目“苹果和虫子”的代码如下所示：

```cpp
#include <iostream>
using namespace std;

int main() {
	int n, x, y, ans = 0;
	cin >> n >> x >> y;
	ans = n - y/x;
	if (y%x != 0) ans -= 1;
	cout << ans;
	return 0;
}
```

但是，如果计算之后的 `ans` 值变为了小数，也就是虫子把所有苹果吃完之后又过了几个小时，程序运行的结果会输出负数，而这是不符合实际情况的，因此针对此种情况需要进行**特判**。改进后的代码如下：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int x, y, z, ans = 0;
	cin >> x >> y >> z;
	ans = x - z/y;
	if (z/y) ans--;
	if (ans < 0) ans = 0; // 对ans小于0的情况进行特判
	cout << ans;
	return 0;
}
```

在题目中，常常会出现需要特判的情况，因此需要多加注意才能够取得 AC。

## 二、多重循环应用（二）

### 2.1 空心与实心矩阵

**写法一**

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int h, w;
    char s;
    bool t;
    cin >> h >> w >> s >> t;
    // 实心
    if (t) {
    	for (int i = 1; i <= h; i++) {
    		for (int j = 1; j <= w; j++) {
    			cout << s;
			}
			cout << endl;
		}
    // 空心
	} else {
		for (int i = 1; i <= h; i++) {
			for (int j = 1; j <= w; j++) {
                // 满足输出s时的情况判断
				if (i == 1 || i == h || j == 1 || j == w) cout << s;
				else cout << " ";
			}
			cout << endl;
		}
	}
    return 0;
}
```

**写法二**

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int h, w;
    char s;
    bool t;
    cin >> h >> w >> s >> t;
    for (int i = 1; i <= h; i++) {
    	for (int j = 1; j <= w; j++) {
            // 满足输出s时的情况判断
    		if (i == 1 || i == h || j == 1 || j == w) cout << s;
    		else {
                // 判断实心或空心
    			if (t) cout << s;
    			else cout << " ";
			}
		}
		cout << endl;
	
    return 0;
}
```

### 2.2 半金字塔

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= i; j++) {
			cout << "*";
		}
		cout << endl;
	}
	return 0;
}
```

### 2.3 完整金字塔

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n-i; j++) cout << " ";
		for (int j = 1; j <= 2*i-1; j++) cout << "*";
		cout << endl;
	}
	return 0;
}
```

### 2.4 反向半金字塔

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n-i; j++) cout << " ";
		for (int j = 1; j <= i; j++) cout << "*";
		cout << endl;
	}
	return 0;
}
```

### 2.5 菱形

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n-i; j++) cout << " ";
		for (int j = 1; j <= 2*i-1; j++) cout << "*";
		cout << endl;
	}
	for (int i = 1; i < n; i++) {
		for (int j = 1; j <= i; j++) cout << " ";
		for (int j = 1; j <= (n-i)*2-1; j++) cout << "*";
		cout << endl;
	}
	return 0;
}
```
