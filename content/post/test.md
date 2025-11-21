---
date: 2025-11-22T5:38:00+08:00
draft: false
---

``` cpp

#include <bits/stdc++.h>
using namespace std;

// 网络信息和节点边信息
struct Edge { int to, cap, rev; };
struct Graph { int u, v, w; };

int n, m, s, t, l, ans;
vector<vector<Edge>> g;
vector<int> level, iter;
vector<Graph> gra;
/*
参数说明 : 
    g : 存储网络图，实现残量网络
    gra : 存储边信息
    1. 在g中，每个节点维护自己能推的流和反向流信息
    2. bfs遍历g[u]，找剩余容量大于0的边
    3. dfs增广时，通过rev来快速找到对应的反向边进行更新
*/
void add_edge(int from, int to, int cap) {
    g[from].push_back({to, cap, (int)g[to].size()});
    g[to].push_back({from, 0, (int)g[from].size() - 1});
}

// bfs 遍历分层
bool bfs() {
    queue<int> q;
    fill(level.begin(), level.end(), -1);
    level[s] = 0;
    q.push(s);
    while(!q.empty()) {
        int v = q.front(); q.pop();
        for(auto &e: g[v]) {
            if(e.cap > 0 && level[e.to] < 0) {
                level[e.to] = level[v] + 1;
                q.push(e.to);
            }
        }
    }
    return level[t] != -1;
}
// dfs 搜索增广
int dfs(int v, int f) {
    if(v == t) return f;
    for(int &i = iter[v]; i < g[v].size(); i++) {
        Edge &e = g[v][i];
        if(e.cap > 0 && level[e.to] == level[v]+1) {
            int d = dfs(e.to, min(f, e.cap));
            if(d > 0) {
                e.cap -= d;
                g[e.to][e.rev].cap += d;
                return d;
            }
        }
    }
    return 0;
}

// 计算最大流
int maxflow() {
    int flow = 0;
    while(bfs()) {
        fill(iter.begin(), iter.end(), 0);
        int f;
        while((f = dfs(s, INT_MAX)) > 0) flow += f;
    }
    return flow;
}

// 清理图信息
void init_graph() {
    g.assign(n + 1, vector<Edge>());
    level.assign(n + 1, -1);
    iter.assign(n + 1, 0);
}

void solve() {
    cin >> n >> m;
    gra.resize(m + 1);
    for(int i = 1; i <= m; i++) cin >> gra[i].u >> gra[i].v >> gra[i].w;
    cin >> s >> t >> l;

    ans = 0;

    // 情况 1: w < l
    init_graph();
    for(int i = 1; i <= m; i++) {
        if(gra[i].w < l) {
            add_edge(gra[i].u, gra[i].v, 1);
            add_edge(gra[i].v, gra[i].u, 1);
        }
    }
    ans += maxflow();

    // 情况 2: w > l
    init_graph();
    for(int i = 1; i <= m; i++) {
        if(gra[i].w > l) {
            add_edge(gra[i].u, gra[i].v, 1);
            add_edge(gra[i].v, gra[i].u, 1);
        }
    }
    ans += maxflow();

    cout << ans << "\n";
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}




```