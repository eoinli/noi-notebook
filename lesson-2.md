---
description: 了解计算机存储的基本知识，学习 C++ 基本数据类型有关内容
---

# Lesson 2

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、位

**位**（Bit），是计算机存储的基本单位。每一位中可能有 0 和 1 两种情况。8 位等于 1 字节（Byte）。因此 1 字节内可存储最大 $$2^{7}$$ 的值（首位保留为数字的正负符号位）。

## 二、C++ 基本数据类型

### 1.1 整型

**整型**（Integer），即整数。C++ 中若要声明整型变量，可以使用三种关键字：`short`、`int` 和 `long long`。其中：

* `short` 可以表示最大占用 2 个字节（$$\pm 2^{15}$$ 即 $$\pm 32768$$）的整数；
* `int` 可以表示最大占用 4 个字节（$$\pm 2^{31}$$ 即约等于 $$\pm 2.1 \times 10^{9}$$）的整数；
* `long long` 可以表示最大 8 个字节（$$\pm 2^{63}$$）的整数。

另外，可以通过 `unsigned` 关键字去掉占用 1 位的正负符号，默认定义正数。这样用上述三个关键字定义的变量可包含的正数范围就会分别翻 1 倍。

### 1.2 浮点数

**浮点数**（Floating point），即小数。C++ 中若要声明浮点数变量，可以使用两种关键字：`float` 和 `double`。其中：

* `float` 表示**单精度浮点数**，定义的变量占用 4 个字节，可以表示最大 7 位小数；
* `double` 表示**双精度浮点数**，定义的变量占用 8 个字节，可以表示最大 15 位小数。

**应用**

```cpp
#include<iostream>
using namespace std;

int main(){
	double m, n, area;
	cin >> m >> n;
	area = m*m + n*n - m*(m+n)/2 - n*n/2;
	cout << area;
	return 0;
}
```

#### 1.2.1 保留指定位数的浮点数

设置包含 `iomanip` 包，并在输出时按如下代码的方式设置 `fixed` 关键字和 `setprecision()`来设置精确到小数的位数。`[OUTPUT]` 表示要输出的内容。

```cpp
// 设置iomanip头文件
#include<iomanip>
// 设置精确到小数点后六位
cout << fixed << setprecision(6) << [OUTPUT];
```

这样，我们就可以将 1.2 的应用中的代码优化为——

```cpp
#include<iostream>
#include<iomanip>
using namespace std;

int main(){
	double m, n, area;
	cin >> m >> n;
	area = m*m + n*n - m*(m+n)/2 - n*n/2;
	cout << fixed << setprecision(5) << area;
	return 0;
}
```

### 1.3 字符型

C++ 中若要声明字符型变量，可以使用 `char` 关键字。注意，与 Python 中不同的是，C 和 C++ 中的**字符串均使用双引号** `""` 引用起来，而**单个字符用单引号** `''`。

{% hint style="warning" %}
字符串与字符不是同一种东西，请勿混淆。
{% endhint %}

**应用**

```cpp
#include<iostream>
using namespace std;

int main(){++
	char i;
	cin >> i;
	cout << "    " << i << endl;
	cout << "   " << i << i << i << endl;
	cout << "  " << i << i << i << i << i << endl;
	cout << " " << i << i << i << i << i << i << i << endl;
	cout << i << i << i << i << i << i << i << i << i;
	return 0;
}
```

#### 1.3.1 ASCII 代码

**ASCII**（**American Standard Code for Information Interchange**），即**美国信息交换标准代码**。这个系统中，共包含 128 个字符，其中最高位保留为符号位。

**应用**

以下代码的运行结果可以实现这样的效果：当输入一个大写英文字母时，返回相应的小写英文字母。在 ASCII 中，英文大写字母的代码均为其相应小写字母代码减去 32 的结果。

```cpp
#include<iostream>
using namespace std;

int main(){
	char a, b;
    // 输入大写字母并赋值给a
	cin >> a;
    // 计算时，a被强制转化为ASCII码并加上32赋值给b
	b = a + 32;
    // 由于b的数据类型为字符，则输出b
	cout << b;
	return 0;
}
```

### 1.4 布尔型

**布尔型**（Boolean）变量只包含 `true` 和 `false` 两种值，在 C++ 中通过 `bool` 关键字定义。

{% hint style="info" %}
注意，C 语言中并没有布尔型变量，取而代之地，表示 `false` 的可以是 `0` 和 `null`，其余全视为 `true`。布尔型变量是 C++ 中引入的。
{% endhint %}

## 三、容器的大小

`sizeof()` 函数可以获取作为容器的参数的容量大小。
