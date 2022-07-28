---
description: 通过例题进一步复习巩固深、广度优先搜索算法的原理与应用
---

# Lesson 31

## 一、广度优先搜索（二）

### 1.1 迷宫（二）

在[第 29 讲的第 1.2 节](lesson-29.md#1.2-mi-gong)中，我们使用深搜完成了这道题。现在我们尝试使用广搜实现程序效果。上节课学习初步学习广搜时，我们是直接把搜索代码放在主函数里的，而这里我们希望把搜索代码放在一个独立的函数中。

为了方便比较，深搜与广搜的代码都在下方列出。

```cpp
# include <iostream>
# define N 105
using namespace std;

char map[N][N]; // 存字符迷宫

// sx、sy和ex、ey分别是起始点和终点的坐标
// hh和tt存储队首与队尾元素
int n, m, sx, sy, ex, ey, hh, tt;
// b数组存储当前元素是否走过
// flag用于标记是否到达了终点
bool b[N][N], flag;
// 分别存储能走的格子
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};

 // 分别存储队列以及临时变量
struct node {
    int x, y;
} que[N*N], t;

// 深搜
void dfs(int x, int y) {
    if (flag) return; // 如果已经到达终点则终止程序
    if (x == ex && y == ey) { 
        flag = 1;
        return; // 如果已经到达终点，标记后终止程序
    }
    for (int i = 0; i < 4; i++) { // 遍历四种走法
        int tx = x+dx[i], ty = y+dy[i];
        // 分别存储下一个格子的坐标
        if (map[tx][ty] == '.'
            && tx >= 0 && tx < n
            && ty >= 0 && ty < n
            && !b[tx][ty]) { // 判断下一个格子能不能走
            b[tx][ty] = 1; // 标记为已走过
            dfs(tx, ty); // 从下一格开始搜索
            b[tx][ty] = 0; // 回溯
        }
    }
    return;
}

// 广搜
void bfs(int x, int y) {
    que[tt].x = x, que[tt].y = y, tt++; // 初始化首元素入队
    while (hh < tt) { // 如果队列不为空
        t = que[hh++]; // 暂存队首元素并将队首元素后移一位
        if (t.x == ex && t.y == ey) {
            flag = 1;
            return;
        }
        for (int i = 0; i < 4; i++) {
            int tx = t.x+dx[i], ty = t.y+dy[i];
            if (map[tx][ty] == '.'
                && tx >= 0 && tx < n
                && ty >= 0 && ty < n
                && !b[tx][ty]) {
                b[tx][ty] = 1;
                // 将下一个格子加入队
                que[tt].x = tx, que[tt].y = ty, tt++;
            }
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
                cin >> map[i][j]; // 输入地图
        cin >> sx >> sy >> ex >> ey;
        flag = 0; // 每张地图进行初始化
        b[sx][sy] = 1; // 初始化起始点为已走过
        dfs(sx, sy); // 使用深搜
        bfs(sx, sy); // 使用广搜
        if (flag) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```

### 1.2 马的遍历

你可以在[洛谷（P1443）](https://www.luogu.com.cn/problem/P1443)等 OJ 上找到本题。

#### 题目描述

有一个 $$n \times m$$ 的棋盘，在某个点 $$(x, y)$$ 上有一个马，要求你计算出马到达棋盘上任意一个点最少要走几步。

#### 输入格式

输入只有一行四个整数，分别为 n、m、x、y。

#### 输出格式

一个 $$n \times m$$ 的矩阵，代表马到达某个点最少要走几步（左对齐，宽 5 格，不能到达则输出 -1）。

#### 样例

**样例输入**

```
3 3 1 1
```

**样例输出**

```
0    3    2    
3    -1   1    
2    1    4
```

#### 提示

对于全部的测试点，保证 $$1 \leq x \leq n \leq 400$$，$$1 \leq y \leq m \leq 400$$。

#### 分析

在中国象棋中，马的移动方式是从一个 $$1\times 2$$ 矩形的其中一个顶点走到位于矩形对角位置的顶点。所以，马移动一次有 8 种走法，如下图所示。

![马的八种走法](https://s2.loli.net/2022/07/28/rLCDXGZSRg2hVPv.png)

在之前的题目中，我们使用 `book` 数组来记录一个格子有没有被走过。但是由于本题题目要求有变，所以 `book` 数组的功能得到了拓展：我们会使用它来记录走到每一格点的步数。此外，由于题目要求带特殊格式输出，使用 C 语言风格输出会比输入输出流来得简便。

#### 解答

```cpp
# include <iostream>
# include <cstdio>
# include <cstring> // 初始化数组需要用到
# define N 405
using namespace std;

int n, m, x, y, hh, tt;
int bk[N][N]; // 用于记录步数

// 八种走法的坐标变化
int dx[8] = {-1,-2,-2,-1, 1, 2, 2, 1};
int dy[8] = {2, 1, -1,-2, 2, 1,-1,-2};

// 分别存储队列与临时变量
struct node {
    int x, y, step;
} q[N*N], t;

void bfs(int x, int y) {
    // 初始化首元素入队
    q[tt].x = x, q[tt].y = y, q[tt].step = 0, tt++;
    // 如果队列不为空
    while (hh < tt) {
        // 暂存队首元素并将队首元素后移一位
        t = q[hh++];
        // 遍历八种走法
        for (int i = 0; i < 8; i++) {
            // 分别存储下一个格点的坐标
            int tx = t.x+dx[i], ty = t.y+dy[i];
            // 判断下一个格点能不能走
            if (bk[tx][ty] == -1 && tx >= 1 && tx <= n && ty >= 1 && ty <= m) {
                // 如果可以则增加队首元素的步数
                bk[tx][ty] = t.step+1;
                // 将下一个格点加入队并增加对应步数
                q[tt].x = tx, q[tt].y = ty, q[tt].step = t.step+1, tt++;
            }
        }
    }
}

int main () {
    cin >> n >> m >> x >> y;
    /**************************************
     初始化数组值（只能初始化为0或-1），用法如下：
     memset(array, number, sizeof(array));
     
     其中array为数组名称；
     number为需要初始化的数值（仅可以为0或-1）。
     
     使用memset()函数需要cstring头文件。
    ***************************************/
    // 将数组初始化为-1
    memset(bk, -1, sizeof(bk));
    // 将马的位置的步数设为0
    bk[x][y] = 0;
    // 广搜
    bfs(x, y);

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++)
            // 按照左对齐补满5格的格式输出数据
            printf("%-5d", bk[i][j]);
        // 记得换行
        printf("\n");
    }
    return 0;
}
```

### 1.3 奇怪的电梯

你可以在[洛谷（P1135）](https://www.luogu.com.cn/problem/P1135)等 OJ 上找到本题。

#### 题目描述

呵呵，有一天我做了一个梦，梦见了一种很奇怪的电梯。大楼的每一层楼都可以停电梯，而且第 i 层楼（$$1 \le i \le N$$）上有一个数字 $$K_i$$（$$0 \le K_i \le N$$）。电梯只有四个按钮：开，关，上，下。上下的层数等于当前楼层上的那个数字。当然，如果不能满足要求，相应的按钮就会失灵。例如： `3` `3 1 2 5` 代表了 $$K_i$$（$$K_1=3$$，$$K_2=3$$，……），从 1 楼开始。在 1 楼，按“上”可以到 4 楼，按“下”是不起作用的，因为没有 -2 楼。那么，从 A 楼到 B 楼至少要按几次按钮呢？

#### 输入格式

共二行。

第一行为三个用空格隔开的正整数，表示 N、A、B（$$1 \le N \le 200$$，$$1 \le A, B \le N$$）。

第二行为 N 个用空格隔开的非负整数，表示 $$K_i$$。

#### 输出格式

一行，即最少按键次数，若无法到达，则输出 `-1`。

#### 样例

**样例输入**

```
5 1 5
3 3 1 2 5
```

**样例输出**

```
3
```

#### 提示

对于 100% 的数据，$$1 \le N \le 200$$，$$1 \le A, B \le N$$，$$0 \le K_i \le N$$。

#### 分析

这道题的数组不再是二维，也就意味着不需要二元坐标来表示位置。这里使用之前讲过的 `pair` 来封装队列。同时，需要注意，如果起始楼层与终点楼层相一致，不能输出 `-1` 而应该为 `0`。

#### 解答

```cpp
# include <iostream>
# define N 205
using namespace std;

int n, a, b, k[N], cnt, hh, tt, flag, tx;
bool bk[N];
// 两个元素依次是当前的楼层与所花的步数
pair<int,int> q[N], t;

void bfs (int start) {
    // 初始化首元素入队
    q[tt].first = start, q[tt].second = 0, tt++;
    // 如果队列不为空
    while (hh < tt) {
        // 暂存队首元素并将队首元素后移一位
        t = q[hh++];
        // 如果到达终点楼层
        if (t.first == b) {
            // 记录当前所花步数
            flag = t.second;
            // 终止函数执行
            return;
        }
        for (int i = 0; i < 2; i++) {
            // 分上楼与下楼两种走法
            // 楼层变化的量为当前层输入的数字
            if (!i) tx = t.first + k[t.first];
            else tx = t.first - k[t.first];
            // 如果可以去目标楼层
            if (!bk[tx] && tx >= 1 && tx <= n) {
                // 标记目标楼层为已到过
                bk[tx] = 1;
                // 将下一个楼层加入队并增加所花步数
                q[tt].first = tx, q[tt].second = t.second+1, tt++;
            }
        }
    }
}

int main () {
    
    cin >> n >> a >> b;
    for (int i = 1; i <= n; i++) cin >> k[i];
    // 调用广搜
    bfs(a);
    // 特判起等于终的情况
    if (a == b) cout << 0;
    // 若flag有值则输出
    else if (flag) cout << flag;
    // 若flag无值则输出-1
    else cout << -1;
    
    return 0;
}
```

## 二、深度优先搜索（三）

### 2.1 独立水域

有一片 $$n\times n$$ 大小的矩阵沼泽，大小各异陆地（1）与水潭（0）分散在这片沼泽中。如果我们把垂直和水平方向上相连通的水潭视为同一个独立的水域，那么请求出这片沼泽内有多少个独立的水域。

#### 分析

假设给出的输入数据是这样的：

```
5
0 1 1 0 0
1 1 0 0 1
0 0 1 0 1
0 1 1 1 0
1 0 0 0 1
```

那么就可以画出下面这张图片（图片需要联网）：

![输入数据形成的矩阵](https://s2.loli.net/2022/07/28/G9tJumHTihcOfAl.png)

如题目描述，垂直方向或水平方向上连在一起的 0 视为一片独立水域，那么上图中不同颜色的部分就是不同的独立水域，共有 5 个不同的水域，所以应当输出 5。

那么这个效果的代码应该怎样写呢？使用深搜或者广搜都可以实现效果，鉴于深搜的思路比较易懂，我们以深搜为例进行分析。要有独立水域，就肯定需要至少一个 0。那么，可以遍历每一个格子，如果发现一个 0，先将水域数量加 1，然后搜索这个 0 旁边有没有相连的其他 0。把相连的所有 0 都记为已经遍历过，这样下次碰到这个 0 时不会重复遍历——**这时是不需要回溯标记的**。这样就可以得出水域的总数量。

#### 解答

```cpp
# include <iostream>
# define N 105
using namespace std;

int n, a[N][N], cnt;
bool bk[N][N];
int dx[4] = {0, 1, 0, -1};
int dy[4] = {1, 0, -1, 0};

void dfs(int x, int y) {
    // 没有终点，所以不用写判断终点的代码
    // 分别遍历四种走法
    for (int i = 0; i < 4; i++) {
        // 存储下一个格子的坐标
        int tx = x+dx[i], ty = y+dy[i];
        // 如果能走且没有越界
        if (!a[tx][ty] && !bk[tx][ty] &&
            tx >= 1 && tx <= n &&
            ty >= 1 && ty <= n) {
            	// 将该格子标记为已走过
                bk[tx][ty] = 1;
            	// 搜索下一格
                dfs(tx, ty);
        }
    }
    return;
}

int main () {
    
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            cin >> a[i][j];
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            // 如果这个格子为0且没有被遍历过
            if (!a[i][j] && !bk[i][j]) {
                // 标记为已遍历
                bk[i][j] = 1;
                // 计数
                cnt++;
                // 搜索有没有连着的格子
                dfs(i, j);
            }
        }
    }
    cout << cnt;
    
    return 0;
}
```
