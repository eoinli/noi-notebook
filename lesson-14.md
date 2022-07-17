---
description: 学习定义常量、中断函数执行、给变量赋极值等常用代码，初步认识字符数组
---

# Lesson 14

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、定义常量

使用 `const` 关键字可以定义常量（Constant），即不会改变的量。

```cpp
const MAXN = 1e5 + 10;
```

`MAXN` 为常量名，`1e5` 为使用科学计数法表示的 100,000 即 $$1\times10^{5}$$（以此类推）。这行代码的结果就是定义了一个值为 100,010 的常量 `MAXN`。常量的使用和变量一致，直接写常量名即可。

## 二、中断函数执行

`return 0;` 语句可以中断包括主函数在内的所有函数的执行，即不再执行当前函数接下来的内容。它不仅可以使用于函数的末尾，也可以出现在函数的其他执行语句里。

```cpp
int main() {
    int a;
    cin >> a;
    if (a == 2) {
        cout << "a is 2";
        return 0;
    }
    cout << "a is not 2";
    return 0;
}
```

在以上的代码中，如果用户的输入值为 2，则会输出 `a is 2`，并且提前中断主函数的执行；如果不是 2，则会输出 `a is not 2`，之后中断主函数的执行。也就是说，如果 `a == 2` 成立，则程序不会再执行第一个 `return 0;` 后的函数内容。

## 三、变量的极值

可以使用头文件 `climits` 获取数据类型变量可定义的极值。

```cpp
#include<climits> // 设置头文件
int maxn = INT_MAX; // 将变量定义为int类型变量可定义的最大值
int minn = INT_MIN; // 将变量定义为int类型变量可定义的最小值
```

`INT_MAX` 和 `INT_MIN` 等都是 `climits` 头文件中定义好的常量。

## 四、字符数组

顾名思义，**字符数组**就是由字符作为元素组成的数组。

### 4.1 头文件 cstring

可以使用头文件 `cstring` 进行对字符数组的操作。

#### 4.1.1 获取字符数组长度

头文件 `cstring` 提供 `strlen()` 函数以获取字符数组长度，其参数为待获取长度的字符数组名。

```cpp
char c[1005];
cin >> c;
cout << strlen(c);
// 输入 asdfghjkl
// 输出 9
```

### 4.2 读取一整行内容

一般情况下使用 `cin` 设置输入时，会自动过滤掉空格。如果想要在不过滤空格的基础上获取一整行的输入内容，则需要使用 `cin.getline()`。这个函数有两个参数，依次是**需要获取到的字符数组变量名**和**为这个字符数组变量分配的空间**。

```cpp
char a[1005];
cin.getline(a, 1005); // 读取包括空格在内的整行字符，存入字符数组a，为a分配了1005的空间
```

**应用一**

给定一行字符串，分别统计该字符串中数字（0-9）、大写字母（A-Z）、小写字母（a-z）和其他字符（不包含空格）的个数。保证给定字符串中的所有字符都在 ASCII 可显示字符范围内。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	char c[1005];
	int num = 0, cap = 0, low = 0, oth = 0;
	cin.getline(c, 1005);
	for (int i = 0; i < strlen(c); i++) {
		if (c[i] >= '0' && c[i] <= '9') num++;
		else if (c[i] >= 'A' && c[i] <= 'Z') cap++;
		else if (c[i] >= 'a' && c[i] <= 'z') low++;
		else if (c[i] != ' ') oth++;
	}
	cout << num << " " << cap << " " << low << " " << oth;
	return 0;
}
```

**应用二**

给定一行字符串，将该字符串中的所有小写字母转为大写字母、大写字母转为小写字母，并输出。保证给定字符串中的所有字符都在 ASCII 可显示字符范围内。

```cpp
#include <bits/stdc++.h>
using namespace std;

char c[1005];

int main() {
	cin.getline(c, 1005);
	for (int i = 0; i < strlen(c); i++) {
		if (c[i] >= 'a' && c[i] <= 'z') cout << char(c[i] - 'a' + 'A');
		else if (c[i] >= 'A' && c[i] <= 'Z') cout << char(c[i] + 'a' - 'A');
		else cout << c[i];
	}
	
	return 0;
}
```

**应用三**

给定一行字符串，将该字符串中的所有小写字母转为大写字母、大写字母转为小写字母，同时将每个字母向后推移 n 位（以 n = 2 为例，a/A 推移为 C/c，b/B 推移为 D/d……，y/Y 推移为 A/a，z/Z 推移为 B/b）并输出。保证给定字符串中的所有字符都在 ASCII 可显示字符范围内。

```cpp
#include <bits/stdc++.h>
using namespace std;

char c[1005];
int n;

int main() {
	cin >> c >> n;
	
	for (int i = 0; i < strlen(c); i++) {
		if (c[i] >= 'a' && c[i] <= 'z') cout << char('A' + (c[i] - 'a' + n)%26);
		else if (c[i] >= 'A' && c[i] <= 'Z') cout << char('a' + (c[i] - 'A' + n)%26);
		else cout << c[i];
	}
	return 0;
}
```
