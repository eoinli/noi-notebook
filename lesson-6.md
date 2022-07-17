---
description: 了解 C++ 中对变量命名的一般规范，学习循环结构程序的更多语句用法
---

# Lesson 6

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、变量的命名规范

在开发项目时，为了保持代码的可读性，常用见名知义的规则，让代码简单易懂。但是在算法竞赛中，代码应当保持简洁而不复杂，这对节省时间很有帮助。算法中常用的变量名通常仅用一个或几个字母表示，列表如下：

| 变量名                       | 常用意义    |
| ------------------------- | ------- |
| `a`、`b`、`c`、`d`           | 普通变量    |
| `x`、`y`、`z`               | 普通变量或坐标 |
| `h`                       | 高度      |
| `i`、`j`、`k`               | 循环变量    |
| `l`、`len`                 | 长度      |
| `l`、`r`                   | 左右      |
| `m`、`n`                   | 数量      |
| `r`、`c`                   | 行、列     |
| `sum`、`ans`、`cnt`、`count` | 计数器、累加器 |
| `t`、`tmp`、`temp`          | 临时变量    |
| `e`、`v`                   | 边、点     |
| `flag`                    | 标志      |

## 二、循环结构（二）

### 2.1 while 语句

```cpp
while (判断条件) {
    // 执行语句
}
```

C++ 中的 while 循环的语法与 Python 中相似，不过判断条件被小括号框了起来，循环体被大括号框了起来。while 循环的运行规则是，满足判断条件则执行循环语句；循环结束后，如果满足判断条件，则进行下一次循环，否则跳出循环。通常来说，for 循环相对于 while 循环用的更多，但在循环过程中不需要用到循环变量值的时候，相对于较冗长的 for 循环，使用 while 循环可以减少代码量、提高可读性。

![while 循环运行流程图](<.gitbook/assets/image (4).png>)

**应用**

输入 n 和 m（$$n \leq m$$），求两者之间（包含 n 和 m）7 的倍数和。请使用 while 循环语句。

```cpp
#include <iostream>
using namespace std;
int main() {
	int m, n, ans=0;
	cin >> n >> m;
	int i = n;
	while (i <= m) {
		if (i%7==0) ans += i;
		i++;
	}
	cout << ans;
	return 0;
}
```

**复习**

输入 n 和 m（$$n \le m$$），求两者之间（包含 n 和 m）第一个能被 7 整除的数的和。请使用 for 循环语句。

```cpp
#include <iostream>
using namespace std;
int main() {
	int m, n, ans=0;
	bool flag=1;
	cin >> n >> m;
	for (int i = n; i <= m; i++) {
		if (i%7==0) {
			if (flag) {
				ans = i;
				flag = 0;
			}
		}
	}
	cout << ans;
	return 0;
}
```

### 2.2 break 关键字

在上面的复习例中，为了判断第一个倍数花了很多工夫，如果我们能在判断到第一个可以被 7 整除的数时就**跳出循环结构**，既可以减小代码的长度，也可以减短程序运行的时间。`break` 关键字的用途就是实现这一点。在循环中，`break` 关键字表示跳出当前循环结构，不再执行循环结构中接下来的内容。由此，上面复习中的代码可以优化为——

```cpp
#include <iostream>
using namespace std;
int main() {
	int m, n, ans=0;
	cin >> n >> m;
	for (int i = n; i <= m; i++) {
		if (i%7==0) {
			ans = i;
			break;
		}
	}
	cout << ans;
	return 0;
}
```

### 2.3 continue 关键字

有时候需要在循环中**跳过某一次循环**，这就用到了 `continue` 关键字，它的用途在于跳过这一次循环，不再执行这一次循环中接下来的内容，但下一次循环继续运行。

**应用**

逢 4 过：输入 n 和 m（$$n \le m$$），输出两者之间（包含 n 和 m）不是 4 的倍数的数，用空格分隔。

```cpp
#include <iostream>
using namespace std;
int main() {
	int m, n;
	cin >> n >> m;
	for (int i = n; i <= m; i++) {
		if (i%4==0) continue;
		else cout << i << " ";
	}
	return 0;
}
```

### 2.4 综合运用

**应用一**

输入一个正整数，将该数字倒序输出。

```cpp
#include <iostream>
using namespace std;
int main() {
	int n;
	cin >> n;
	while (n) {
		cout << n%10;
		n /= 10;
	}
	return 0;
}
```

**应用二**

循环嵌套：输入 n 和 m（$$n \le m$$），输出两者之间（包含 n 和 m）所有数中包含 2 的个数。

```cpp
#include <iostream>
using namespace std;
int main() {
	int n, m, ans = 0;
	cin >> n >> m;
	for (int i = n; i <= m; i++) {
		for (int j = i; j; j /= 10) {
			if (j % 10 == 2) ans++;
		}
	}
	cout << ans;
	return 0;
}
```
