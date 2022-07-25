---
description: 学习搜索算法中深度优先搜索的思想与代码写法
---

# Lesson 29

## 一、深度优先搜索

**深搜**（DFS），全称为**深度优先搜索**（depth-first search），是一种常用的搜索算法。其过程简要来说是，对每一个分支路径深入到不能再深入为止，且每个节点只能访问一次。深搜的核心是：**能深则深，不能深就退**。

下面通过几道例题学习深搜的用法。

### 1.1 矿中绝境

Jeffrey 在一年一度的采矿过程中，由于过于贪心，沉迷于挖矿无法自拔，被金钱蒙蔽了双眼，导致他在矿道里迷了路。假设 Jeffrey 正位于一个 $$n \times m$$ 矩阵迷宫的左上角 $$(1, 1)$$，迷宫的四周（除了出口处）全部被山体隔绝，矩阵中有障碍物（不能走）、空格（可以走）以及出口三种格子，分别用数字 1、0 和 2 表示。假设 Jeffrey 能够摸到障碍物并返回，且拥有超乎常人的记忆力与方向感。

#### 子任务一

请问 Jeffrey 是否有可能走出这个迷宫？如果有可能，输出 `Mission Accomplished!`。

**分析**

想要走出迷宫，我们的基本思路是这样的：

```cpp
// 是否到了终点
// 按序枚举每一种可能
	// 走一步：可以走且没有走过且在边界内
        // 做标记表示这里已经走过了
        // 走下一步（递归）
// 如果走到死路则返回
```

**解答**

```cpp
#include <iostream>
#define N 105
using namespace std;

int map[N][N]; int n, m;
bool b[N][N];
int dx[4] = {-1, 0, 1, 0}; // 上右左下四个方向走时x的变化
int dy[4] = {0, 1, 0, -1}; // 上右左下四个方向走时y的变化

void dfs(int x, int y) {
    if (map[x][y] == 2) { // 如果当前位置是终点
        cout << "Mission Accomplished!";
        return; // 直接终止函数
    }
    for (int i = 0; i < 4; i++) {
        int tx = x+dx[i], ty = y+dy[i]; // 保存四种走法时的坐标变化
        if (map[tx][ty] != 1 && tx >= 1 && tx <= n && ty >= 1 && ty <= m && !b[tx][ty]) { // 若能走
            b[tx][ty] = 1; // 把下一格标记为已走过
            dfs(tx, ty); // 走上下一格
            b[tx][ty] = 0; // 取消标记（不会导致走回头路）
        }
    }
    return; // 终止函数执行
}

int main () {
    
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            cin >> map[i][j];
    
    b[1][1] = 1; // 初始化起点格为已走过
    dfs(1, 1); // 从起点格开始走
    
    return 0;
}
```

#### 子任务二

请问 Jeffrey 有多少种方法走出这个迷宫？

**分析**

在子任务一的代码的基础上，如果在判断终点时不终止程序，让它继续运行，接着定义一个计数器变量来统计可行走法数量，每走到终点一次就计一次数，这样就可以统计出总共的走法数量。

**解答**

```cpp
//...
int map[N][N]; int n, m, cnt;
//...

void dfs(int x, int y) {
    if (map[x][y] == 2) cnt++;
    //...
}

int main () {
    //...
    cout << cnt;
    return 0;
}
```

#### 子任务三

请问 Jeffrey 是否有可能走出这个迷宫？如果有可能，输出 `Mission Accomplished!`；否则输出 `Mission Impossible!`。

**分析**

在子任务一的代码的基础上，增加一个标记变量 `flag` 来存储是否已经走出迷宫，一来可以节省循环的次数，二来也利于实现任务效果。

**解答**

```cpp
//...
bool b[N][N], flag;
//...

void dfs(int x, int y) {
    if (flag) return; // 如果已经成功则不再继续执行函数
    if (map[x][y] == 2) { 
        flag = 1; // 标记成功
        return;
    }
    //...
}

int main () {
    //...
    if (!flag) cout << "Mission Impossible!";
    else cout << "Mission Accomplished!";
    return 0;
}
```

### 1.2 迷宫

你可以在 [OpenJudge（百练 2790）](http://bailian.openjudge.cn/practice/2790/)上找到本题。

#### 题目描述

一天 Extense 在森林里探险的时候不小心走入了一个迷宫，迷宫可以看成是由 $$n \times n$$ 的格点组成，每个格点只有 2 种状态，`.` 和 `#`，前者表示可以通行，后者表示不能通行。同时当 Extense 处在某个格点时，他只能移动到上下左右四个方向之一的相邻格点上，Extense 想要从点 A 走到点 B，问在不走出迷宫的情况下能不能办到。如果起点或者终点有一个不能通行（为 `#`），则看成无法办到。迷宫左上角的格点标记为 $$(0, 0)$$。

#### 输入格式

第 1 行是测试数据的组数 k，后面跟着 k 组输入。

每组测试数据的第 1 行是一个正整数 n（$$1 \le n \le 100$$），表示迷宫的规模是 $$n \times n$$ 的。接下来是一个 $$n \times n$$ 的矩阵，矩阵中的元素为 `.` 或者 `#`。再接下来一行是 4 个整数 `ha`、`la`、`hb`、`lb`，描述 A 处在第 `ha` 行, 第 `la` 列，B 处在第 `hb` 行，第 `lb` 列。注意到 `ha`、`la`、`hb`、`lb` 全部是从 0 开始计数的。

#### 输出格式

k 行，每行输出对应一个输入。能办到则输出 `YES`，否则输出 `NO`。

#### 样例

**样例输入**

```
2
3
.##
..#
#..
0 0 2 2
5
.....
###.#
..#..
###..
...#.
0 0 4 0
```

**样例输出**

```
YES
NO
```

#### 解答

```cpp
#include <iostream>
#define N 105
using namespace std;

char map[N][N]; int n, m, sx, sy, ex, ey;
bool b[N][N], flag;
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};

void dfs(int x, int y) {
    if (flag) return;
    if (x == ex && y == ey) {
        flag = 1;
        return;
    }
    for (int i = 0; i < 4; i++) {
        int tx = x+dx[i], ty = y+dy[i];
        if (map[tx][ty] == '.' && tx >= 0 && tx < n && ty >= 0 && ty < n && !b[tx][ty]) {
            b[tx][ty] = 1;
            dfs(tx, ty);
            b[tx][ty] = 0;
        }
    }
    return;
}

int main () {
    
    cin >> m;
    
    while (m--) {
        cin >> n;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                cin >> map[i][j];
        cin >> sx >> sy >> ex >> ey;
        flag = 0;
        b[sx][sy] = 1;
        dfs(sx, sy);
        if (!flag) cout << "NO" << endl;
        else cout << "YES" << endl;
    }
    return 0;
}
```
