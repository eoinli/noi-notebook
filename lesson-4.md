---
description: 学习一些简单算法，进一步认识运算符与程序结构
---

# Lesson 4

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、一些数学函数

要使用这些数学函数，需要引入头文件 `cmath`。这里列举了三个常用的数学函数。

* `pow(n, m)` 的值为 $$n^{m}$$，即 n 的 m 次幂；
* `sqrt(n)` 的值为 $$\sqrt{n}$$，即 n 的算术平方根；
* `abs(n)` 的值为 $$\lvert a \rvert$$，即 a 的绝对值。

## 二、读取字符

可以通过 `getchar()` 函数获取任意输入的字符值，包括 `cin` 一般会忽略掉的空格和换行。

## 三、交换变量值

有时，我们需要交换两个变量的值。主要有三种方法来实现这个效果。

### 3.1 借助另一个变量

假设按照如下方法尝试交换两个变量 `a` 和 `b` 的值。

```cpp
int a, b;
cin >> a >> b;
a = b;
b = a;
cout << a << b;
```

这样的方法是不能正确实现效果的，而会输出两个相同的值。这是因为我们使用 `a = b` 的时候，`a` 的值已经与 `b` 一致了，再进行 `b = a` 赋值后，二者的值一定是相等的。如果我们引入另一个变量，将 `a` 的值先保存到这个变量中，再与 `b` 进行增减计算，就能实现效果。

```cpp
#include <iostream>
using namespace std;
int main(){
	int a, b, t;
	cin >> a >> b;
	t = a;
	a = b;
	b = t;
	cout << a << " " << b;
	return 0;
}
```

### 3.2 用同一变量增减

尝试不使用第三个变量实现效果。

```cpp
#include <iostream>
using namespace std;
int main(){
	int a, b;
	cin >> a >> b;
	// 请完成这里的执行语句
	cout << a << " " << b;
	return 0;
}
```

假设我们输入的整数为 3 和 4，那么首先将 a = 3 与 b = 4 之和赋值给 a = 7，再把 a = 7 减去 b = 4 的差赋值给 b = 3，b 就成功被赋值为 a 的初始值即 3 了；如果接着再用 a = 7 减去 b = 3 的差赋值给 a = 4，a 也被成功赋值给 b 的初始值即 4 了。具体可参见下表。

| 操作          | 操作后 a 的值      | 操作后 b 的值      |
| ----------- | ------------- | ------------- |
| （初始）        | x             | y             |
| `a = a + b` | x + y         | y             |
| `b = a - b` | x + y         | x + y - y = x |
| `a = a - b` | x + y - x = y | x             |

```cpp
#include <iostream>
using namespace std;
int main(){
	int a, b;
	ccin >> a >> b;
	a += b;
	b = a - b;
	a = a - b;
	cout << a << " " << b;
	return 0;
}
```

### 3.3 使用函数

我们也可以使用 `swap(a, b)` 解决问题，该函数的作用是将 a 与 b 的值交换。

```cpp
#include <iostream>
using namespace std;
int main(){
	int a, b;
	cin >> a >> b;
	swap(a, b);
	cout << a << " " << b;
	return 0;
}
```

## 四、运算符（二）

### 4.1 关系运算符（二）

#### 4.1.1 多重条件判断语句

```cpp
if (判断条件) {
    // 执行语句
} else if (判断条件) {
    // 执行语句
} else {
    // 执行语句
}
```

以上语句表示，在整个判断语句中，只能满足一个判断条件，并执行相应语句。各判断部分是**互斥**的。`else if` 的数量可以根据实际情况增加或减少。它与以下语句不同——

```cpp
if (判断条件) {
    // 执行语句
}
if (判断条件) {
    // 执行语句
}
if (判断条件) {
    // 执行语句
}
```

这段语句中的三个判断部分各司其职，可以同时满足多个条件，是**并列**关系。

**应用一**

输入农历月份，输出季节。

```cpp
#include <iostream>
using namespace std;
int main(){
	int season;
	cin >> season;
	if (season <= 3) cout << "Spring";
	else if (season <= 6) cout << "Summer";
	else if (season <= 9) cout << "Fall/Autumn";
	else if (season <= 12) cout << "Winter";
	return 0;
}
```

### 4.2 逻辑运算符

C++ 中的逻辑运算符包含常见的 `&&`、`||` 和 `!`，分别表示与、或和非。

* `&&` 表示左右两边的两个条件必须全部成立则为 `True`，否则则为 `False`；
* `||` 表示左右两边的两个条件必须至少有一个成立则为 `True`，否则则为 `False`；
* `!` 表示其后的条件取反（若原有条件为 `True` 则变为 `False`，反之亦然）。

**应用一**

给定语数外三科成绩，若均大于 90 分则输出 `Excellent!`，否则输出 `Come on!`。

```cpp
#include <iostream>
using namespace std;
int main() {
	int chi, math, eng;
	cin >> chi >> math >> eng;
	if (chi > 90 && math > 90 && eng > 90) cout << "Excellent!";
	else cout << "Come on!";
	return 0;
}
```

**应用二**

给定语数两科成绩，若其中**恰有一门不合格**则输出 `Strive harder!`，否则输出 `Fine!`。

```cpp
#include <iostream>
using namespace std;
int main() {
	int chi, math;
	cin >> chi >> math;
	if (chi < 60 && math >= 60 || chi >= 60 && math < 60) cout << "Strive harder!";
	else cout << "Fine!";
	return 0;
}
```

**应用三**

判断所给年份是否为闰年（四年一闰，一百年不闰，四百年再闰，年份值为大于 1582 的整数）。

```cpp
#include <iostream>
using namespace std;
int main(){
	int year;
	cin >> year;
	if (year%4 == 0 && year&100!=0 || year%400 == 0) cout << "Leap Year";
	else cout << "Normal Year";
    return 0;
}
```

#### 4.2.1 逻辑运算符的优先级

在 `&&`、`||` 和 `!` 中，程序会最先运算 `!`，其次 `&&`，最后 `||`。
