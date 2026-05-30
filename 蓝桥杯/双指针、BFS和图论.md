---
date: 2026-05-25
---
# 双指针、BFS和图论
## 双指针
双指针只是建立在暴力上的优化，所以先搞明白暴力怎么写
## bfs
有个判重数组别忘了
![[Pasted image 20260528222658.png]]
### 经典题
![[Pasted image 20260529000418.png]]
```cpp
#include <bits/stdc++.h>
#define x first
#define y second
using namespace std;
typedef pair<int, int> PII;
int n, m;
const int N = 210;
char g[N][N];
int dist[N][N];
int dx[4] = {-1, 1, 0, 0}, dy[4] = {0, 0, -1, 1};

int bfs(PII start, PII end) {
    queue<PII> q;
    memset(dist, -1, sizeof dist);
    dist[start.x][start.y] = 0;
    q.push(start);
    while (q.size()) {
        PII cur = q.front();
        q.pop();
        // 判断cur是不是目标
        if (end == make_pair(cur.x, cur.y)) return dist[cur.x][cur.y];
        // 不是目标，后续入队
        for (int i = 0; i < 4; i ++) {
            int next_x = cur.x + dx[i];
            int next_y = cur.y + dy[i];
            if (next_x < 0 || next_y < 0 || next_x >= n || next_y >= m) continue;
            if (g[next_x][next_y] == '#') continue;
            if (dist[next_x][next_y] != -1) continue;
            dist[next_x][next_y] = dist[cur.x][cur.y] + 1;
            q.push({next_x, next_y});
        }
    }
    return -1;
}

int main() {
    int round;
    cin >> round;
    while (round --) {
        cin >> n >> m;
        for (int i = 0; i < n; i ++) cin >> g[i];
        PII start, end;
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < m; j ++) {
                if (g[i][j] == 'S') start = {i, j};
                else if (g[i][j] == 'E') end = {i, j};
            }
        }
        int distance = bfs(start, end);
        if (distance == -1) cout << "oop!" << endl;
        else cout << distance << endl;
    }
    
    return 0;
}
```
## 图论(环)
### 经典题
![[Pasted image 20260530221036.png]]
![[Pasted image 20260529104032.png]]
![[Pasted image 20260529104748.png]]
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 10010;
int bottle[N];
int state[N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i ++) cin >> bottle[i];
    int cnt = 0;
    for (int i = 1; i <= n; i ++) {
        if (state[i]) continue;
        int j = bottle[i];
        while (!state[j]) {
            state[j] = true;
            j = bottle[j];
        }
        cnt ++;
    }
    cout << n - cnt << endl;
    return 0;
}
```