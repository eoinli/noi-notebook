---
description: 分析学习使用二维数组进行图形变换和输出
---

# Lesson 13

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、数组（三）

### 1.1 基本

#### 1.1.1 求相同值

给定一个 $$n\times m$$ 的整数二维数组以及一个整数 x，求这个二维数组中与 x 相同的数的个数。（$$n, m \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	
	int n, m, a[105][105] = {0}, num, cnt = 0;
	
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			cin >> a[i][j];

	cin >> num;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			if (a[i][j] == num) cnt++;
	
	cout << cnt;

	return 0;
}
```

#### 1.1.2 交换列

给定一个 $$r\times c$$ 的整数二维数组，交换所形成矩阵的指定两列的值，输出交换后的二维数组。（$$r, c \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int r, c, a[105][105] = {0}, sc1, sc2;
	
	cin >> r >> c;
	for (int i = 1; i <= r; i++)
		for (int j = 1; j <= c; j++)
			cin >> a[i][j];
	
	cin >> sc1 >> sc2;
	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			if (j == sc1) cout << a[i][sc2] << " ";
			else if (j == sc2) cout << a[i][sc1] << " ";
			else cout << a[i][j] << " ";
            // 此处也可以直接使用swap(a[i][sc1], a[i][sc2])实现效果
		}
		cout << endl;
	}
	
	return 0;
}
```

#### 1.1.3 交换行

给定一个 $$r\times c$$ 的整数二维数组，交换数组所形成矩阵的指定两行的值，输出交换后的二维数组。（$$r, c \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int r, c, a[105][105] = {0}, sr1, sr2;
	
	cin >> r >> c;
	for (int i = 1; i <= r; i++)
		for (int j = 1; j <= c; j++)
			cin >> a[i][j];
	
	cin >> sr1 >> sr2;
	for (int i = 1; i <= c; i++) swap(a[sr1][i], a[sr2][i]);
	
	for (int i = 1; i <= r; i++){
		for (int j = 1; j <= c; j++)
			cout << a[i][j] << " ";
		cout << endl;
	}
	
	return 0;
}
```

#### 1.1.4 求边缘元素和

给定一个 $$r\times c$$ 的整数二维数组，计算并输出位于数组所形成矩阵的边缘元素之和。（$$r, c \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int r, c, sum = 0, a[105][105];
	
	cin >> r >> c;
	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			cin >> a[i][j];
			if (i == 1 || i == r || j == 1 || j == c)
                sum += a[i][j];
		}
	}
	
	cout << sum;
	
	return 0;
}
```

#### 1.1.5 同位置元素相加

给定两个 $$r\times c$$ 的整数二维数组，计算两个数组各同位置元素值的和并按 $$r\times c$$ 的格式输出。（$$r, c \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int r, c, a[105][105] = {0}, b[105][105] = {0}, sum;
int main() {
	
	cin >> r >> c;
	for (int i = 1; i <= r; i++)
		for (int j = 1; j <= c; j++)
			cin >> a[i][j];
	for (int i = 1; i <= r; i++)
		for (int j = 1; j <= c; j++)
			cin >> b[i][j];
	
	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			sum = a[i][j] + b[i][j];
			cout << sum << " ";
		}
		cout << endl;
	}
	
	return 0;
}
```

#### 1.1.6 求图片相似度

给定两幅 $$m\times n$$ 黑白像素图像，每一个像素只可能有 0 或 1 两个值，分别代表这个像素是白色或者黑色。如果这两幅图像的同一个位置的像素值是相同的，就说两幅图在这个像素上相似。求两张图片的总相似程度（用百分数表示，四舍五入到百分数的小数点后 2 位，不用写百分号 %）。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int m, n, a[105][105] = {0}, b[105][105] = {0};
	
	cin >> m >> n;
	for (int i = 1; i <= m; i++)
		for (int j = 1; j <= n; j++)
			cin >> a[i][j];
	for (int i = 1; i <= m; i++)
		for (int j = 1; j <= n; j++)
			cin >> b[i][j];
	
	int tot = m*n, valid = 0;
	
	for (int i = 1; i <= m; i++)
		for (int j = 1; j <= n; j++)
			if (a[i][j] == b[i][j]) valid++;
			
	double rate = valid*1.0/tot*100;
	cout << fixed << setprecision(2) << rate;
	
	return 0;
}
```

### 1.2 对角线

#### 1.2.1 求主对角线以上元素和

给定一个 $$n\times n$$ 的整数二维数组，计算并输出位于数组所形成矩阵的主对角线（左上到右下）以上部分的（不含主对角线）元素之和。（$$n \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n, sum = 0, a[105][105] = {0};
	
	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			cin >> a[i][j];
			if (j > i)
				sum += a[i][j];
		}
	}
	
	cout << sum;
	
	return 0;
}
```

#### 1.2.2 求副对角线以上元素和

给定一个 $$n\times n$$ 的整数二维数组，计算并输出位于数组所形成矩阵的副对角线（右上到左下）以上部分的（不含副对角线）元素之和。（$$n \le 100$$）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n, sum = 0, a[105][105] = {0};

	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			cin >> a[i][j];
			if (j <= n - i)
				sum += a[i][j];
		}
	}

	cout << sum;

	return 0;
}
```

#### 1.2.3 求主、副对角线右侧夹角中元素和

给定一个 $$n\times n$$ 的整数二维数组，计算并输出位于数组所形成矩阵的主、副对角线所形成的右侧夹角中的（不含主、副对角线）元素之和。（$$n \le 100$$）

举例来说，在以下 5 × 5 正方形矩阵中，标有 `a` 或 `c` 的元素在主对角线上，标有 `b` 或 `c` 的元素在副对角线上，则标有 `o` 的元素是需要计算和的元素。

```
a - - - b
- a - b o
- - c o o
- b - a o
b - - - a
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n, sum = 0, a[105][105] = {0};

	cin >> n;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			cin >> a[i][j];
			if (j > i && i+j > n+1)
				sum += a[i][j];
		}
	}

	cout << sum;

	return 0;
}
```

### 1.3 旋转

#### 1.3.1 顺时针旋转 90°

给定一个 $$n\times n$$ 的整数二维数组，输出该数组按顺时针方向旋转 90° 后的结果。（$$n \le 100$$）

**分析**

```
1,1  1,2  1,3       3,1  2,1  1,1
2,1  2,2  2,3  -->  3,2  2,2  1,2
3,1  3,2  3,3       3,3  2,3  1,3
```

在以上矩阵中，`n` 的值为 3，则对于元素 `[x,y]`，它处在新数组的位置为 `[3+1-y,x]`。

**解答**

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, a[105][105] = {0}, b[105][105] = {0};
int main() {
	
	cin >> n;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++)
			cin >> a[i][j];
	
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++) 
			b[i][j] = a[n+1-j][i];
	
	for (int i = 1; i <= n; i++){
		for (int j = 1; j <= n; j++)
			cout << b[i][j] << " ";
		cout << endl;
	}
	
	return 0;
}
```

#### 1.3.2 逆时针旋转 90°

给定一个 $$n\times n$$ 的整数二维数组，输出该数组按逆时针方向旋转 90° 后的结果。（$$n \le 100$$）

**分析**

```
1,1  1,2  1,3       1,3  2,3  3,3
2,1  2,2  2,3  -->  1,2  2,2  3,2
3,1  3,2  3,3       1,1  2,1  3,1
```

在以上矩阵中，`n` 的值为 3，则对于元素 `[x,y]`，它处在新数组的位置为 `[y,3+1-x]`。

**解答**

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, a[105][105] = {0}, b[105][105] = {0};
int main() {
	
	cin >> n;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++)
			cin >> a[i][j];
	
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++) 
			b[i][j] = a[j][n+1-i];
	
	for (int i = 1; i <= n; i++){
		for (int j = 1; j <= n; j++)
			cout << b[i][j] << " ";
		cout << endl;
	}

	return 0;
}
```
