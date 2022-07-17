---
description: 巩固一维数组的知识，学习使用二维数组
---

# Lesson 12

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、复习

### 1.1 校门外的树

#### 题目描述

某校大门外长度为 L 的马路上有一排树，每两棵相邻的树之间的间隔都是 1 米。我们可以把马路看成一个数轴，马路的一端在数轴 0 的位置，另一端在 L 的位置；数轴上的每个整数点，即 0、1、2、……、L，都种有一棵树。

由于马路上有一些区域要用来建地铁。这些区域用它们在数轴上的起始点和终止点表示。已知任一区域的起始点和终止点的坐标都是整数，区域之间可能有重合的部分。现在要把这些区域中的树（包括区域端点处的两棵树）移走。你的任务是计算将这些树都移走后，马路上还有多少棵树。

#### 格式与样例

* 输入：第一行有两个整数，分别表示马路的长度 L 和区域的数目 m；接下来 m 行，每行两个整数 u、v，表示一个区域的起始点和终止点的坐标。
* 输出：一行一个整数，表示将这些树都移走后，马路上剩余的树木数量。

样例输入

```
500 3
150 300
100 200
470 471
```

样例输出

```
298
```

#### 数据范围与提示

* 对于 20% 的数据，保证区域之间没有重合的部分。
* 对于 100% 的数据，保证 $$1 \leq L \leq 10^4$$，$$1 \leq m \leq 100$$，$$0 \leq u \leq v \leq L$$。

#### 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int l, m;
	cin >> l >> m; // 输入路的总长及修地铁的段数
	int cnt = 0, tree[10005] = {0}; // 定义计数器、和用于记录每一棵树的数组
	
	for (int i = 0; i < m; i++) {
		int u, v;
		cin >> u >> v; // 输入每次修地铁所需要移除的树木的上下界
		for (int j = u; j <= v; j++) 
			if (!tree[j]) // 判断是否还在（0）
				tree[j] = 1; // 设定为已移除（1）
        // 这里也可以直接设定为将上下界之间的所有树木都记为已移除（1）
	}
	
	for (int i = 0; i <= l; i++) 
		if (!tree[i]) 
			cnt++; // 对还在的树木（0）计数
    
	cout << cnt; // 输出结果
	
	return 0;
}
```

### 1.2 约瑟夫问题

#### 题目描述

n 个人围成一圈，从第一个人开始报数，数到 m 的人出列，再由下一个人重新从 1 开始报数，数到 m 的人再出圈，依次类推，直到所有的人都出圈，请输出依次出圈人的编号。

#### 格式与样例

* 输入：两个整数 n、m。
* 输出：一行 n 个整数，按顺序输出每个出圈人的编号。

样例输入

```
10 3
```

样例输出

```
3 6 9 2 7 1 8 5 10 4
```

#### 数据范围与提示

$$1 \le m, n \le 100$$。

#### 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n, m, t = 0, s = 0;
	cin >> n >> m;
	int p[10005] = {0};
    
	while(n) {
		t++; // t作为循环变量，用来枚举每名士兵的情况
		if (t == n + 1) t = 1; // 当一个轮回结束之后，重新开始一个轮回
		if (!p[t]) s++; // 如果数组中的第t名士兵记为0，则继续报数
		if (s == m) { // 如果报到了m
			s = 0; // 则重新开始一轮报数
			cout << t << " "; // 并输出t
			p[t] = 1; // 记第t名士兵为已出列
			n--; // 减少循环人数
		}
	}
	
	return 0;
}
```

### 1.3 数组最大值

#### 题目描述

输入 n 个整数，存放在数组 a\[1] 至 a\[n] 中，输出最大数所在位置（$$n \ge 1000$$）。

#### 格式与样例

* 输入：第一行，数的个数 n；第二行，n 个正整数，每个数在 $$2^{32}-1$$ 之内。
* 输出：最大数所在位置。

#### 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n, maxn = -1, sub;
	cin >> n;
	int a[1005] = {0};
	
	for (int i = 0; i < n; i++) {
		cin >> a[i];
		if (a[i] > maxn) sub = i+1, maxn = a[i];
	}
	
	cout << sub;
	return 0;
}
```

## 二、数组（二）

### 2.1 二维数组

**应用一**

已知一个 $$n \times n$$ 的正方形正整数矩阵，输出这个矩阵中每一个元素加上 10 之后形成的新矩阵。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	int rect[n][n];
	
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			cin >> rect[i][j];
			rect[i][j] += 10;
		}
	}
	
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			cout << rect[i][j] << " ";
		}
		cout << endl;
	}
	return 0;
}
```

**应用二**

已知一个包含 10 个元素的数组，以 `A[元素位置] = 数组` 的格式将小于等于 10 的数组元素输出出来，保留一位小数。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	double a[15] = {0};
	for (int i = 0; i < 10; i++) {
		cin >> a[i];
	}
	for (int i = 0; i < 10; i++) {
		if (a[i] <= 10) {
			cout << "A[" << i << "] = " << fixed << setprecision(1) << a[i] << endl;
		}
	}
	return 0;
}
```

#### 2.1.1 矩阵中的各位置坐标

```
(i-1, j-1)    (i-1,  j )    (i-1, j+1)
( i , j-1)    ( i ,  j )    ( i , j+1)
(i+1, j-1)    (i+1,  j )    (i+1, j+1)
```

**应用一**

输出从第一行到指定行数的杨辉三角。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int n;
	cin >> n;
	int a[25][25] = {0};
	
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= i; j++) {
			if (i == 1 || i == 2 || j == 1 || j == i) {
				a[i][j] = 1;
				cout << a[i][j] << " ";
				continue;
			}
			a[i][j] = a[i-1][j-1] + a[i-1][j];
			cout << a[i][j] << " ";
		}
		cout << endl;
	}
	return 0;
}
```

**应用二**

给定一个 5×5 二维数组，输出指定行的所有元素的平均值。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main () {
	int m[10][10] = {0}, row, ans = 0;
	
	for (int i = 1; i <= 5; i++) {
		for (int j = 1; j <= 5; j++) {
			cin >> m[i][j];
		}
	}
	
	cin >> row;
	
	for (int i = 1; i <= 5; i++) {
		ans += m[row][i];
	}
	
	cout << ans / 5;
	return 0;
}
```

## 三、思考题

给定 $$N\times N$$ 的二维数组。

1. 求数组矩阵主对角线（左上到右下）上的元素之和；
2. 求数组矩阵副对角线（右上到左下）上的元素之和；
3. 求数组矩阵主对角线的右上半部分（不包含对角线上的元素本身）的元素之和。
