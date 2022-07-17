---
description: 认识结构体并学习其用法，学习基本排序算法的使用
---

# Lesson 16

{% hint style="info" %}
请注意，在本页面所显示的代码段中，可能出现行内缩进过多或过少等现象，这是不同笔记软件和集成开发环境所预设的制表符长度不同所导致的，属正常现象。
{% endhint %}

## 一、结构体

### 1.1 结构体的定义

**结构体**（Structure），是由一系列具有相同类型或不同类型的数据构成的数据集合，是一种比数组更加灵活的数据格式，用以描述特定对象的各项属性，也可以简单被看作把多种数据类型打包起来形成一个新的数据类型。我们可以通过自定义的结构体来描述某一实物对象的各种属性，而不必局限于有限的、固定的数据类型。

### 1.2 结构体的语法

#### 1.2.1 定义结构体与声明结构体变量的语法

`struct` 关键字可以用来定义一个结构体以及声明结构体变量，具体语法如下：

```cpp
struct 类型名 {
    数据类型1 成员变量1;
    数据类型2 成员变量2, 成员变量3;
} 结构体变量名; // 如果不需要提前声明结构体变量，可以不写结构体变量名

struct 已经定义过的类型名 结构体变量名 // 也可以不写struct
```

**应用**

```cpp
// ...
struct wuyuchengzaiwanshouzhi {
    string name;
    int score;
} c, d[105];

struct wuyuchengzaiwanshouzhi a, b;
wuyuchengzaiwanshouzhi e, f;
```

#### 1.2.2 调用结构变量的语法

```cpp
结构体变量名.成员变量名
```

**应用一**

```cpp
// 上接1.2.1应用部分代码
cin >> a.name >> a.score;
// 输入：wyczwsz 80
a.name // 值为字符串wyczwsz
a.score // 值为整数80
```

**应用二**

给定一个数字 n，接下来每行输入一个同学的名字和考试成绩，请按照同学们的考试成绩从小到大排序并输出结果。

```cpp
#include <bits/stdc++.h>
using namespace std;

struct node { // 定义结构体node
	string name; // 添加类型为string的属性name
	int score; // 添加类型为int的属性score
} stu[105]; // 声明类型为node的数组变量stu，分配空间给105个元素

bool cmp(node a, node b) { // 定义返回bool类型值的排序函数cmp，需要传入两个node类型的参数
	return (a.score < b.score); // 如果前者比后者大则返回false表示需要调换顺序
}

int main() {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++)
		cin >> stu[i].name >> stu[i].score; // 输入stu中下标为i的变量的两个属性
	sort(stu+1, stu+n+1, cmp); // stu表示stu数组的起始地址，这里是0，对stu数组中的所有元素排序
	for (int i = 1; i <= n; i++)
		cout << stu[i].name << " " << stu[i].score << endl; // 输出结果
	return 0;
}
```

在上题基础上，将相同成绩的学生按照输入的顺序排序并输出结果。

```cpp
#include <bits/stdc++.h>
using namespace std;

struct node {
	string name;
	int score, id; // 新增id属性
} stu[105];

bool cmp(node a, node b) {
	if (a.score != b.score) return a.score < b.score; // 如果分数不相同则正常排序
	return a.id < b.id; // 如果相同则按照输入顺序id排序
}

int main() {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> stu[i].name >> stu[i].score;
		stu[i].id = i; // 通过i给每一个元素设定输入顺序的属性
	}
	sort(stu+1, stu+n+1, cmp);
	for (int i = 1; i <= n; i++)
		cout << stu[i].name << " " << stu[i].score << endl;
	return 0;
}
```

## 二、排序

头文件 `algorithm` 中的函数 `sort()` 可以用于对数组元素进行排序。其具体应用见 1.2.2 节的应用二部分代码。它一般有三个参数：

```cpp
#include<algorithm>
sort(begin, end, cmp);
```

* `begin` 参数为数组中要排序的起始位置（下标）；
* `end` 参数为数组中要排序的结束位置（下标）；
* `cmp` 参数可选，为自定义的排序规则，是一个已经定义好的函数，如果不填则排序方式默认为升序。
