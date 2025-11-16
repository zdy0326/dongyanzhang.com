---
title: "洛谷刷题记录"
date: 2025-11-16
draft: false
hideToc: true
tags: ["洛谷"]
---

#### P3379 ST表|DFS|倍增算法|图论|LCA

``` cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define all(a) a.begin(), a.end()
const int inf = 1e9 + 7;

const int N = 1e6 + 1;
int n, m, s;
vector<int> dep(N);
vector<int> e[N];
vector<vector<int>> f(N, vector<int>(33, 0));

void dfs(int u, int fa) {
	f[u][0] = fa;
	dep[u] = dep[fa] + 1;
	for (auto v : e[u]) {
		if (v != fa) {
			dfs(v, u);
		}
	}
}
void init() {
	for (int j = 1; (1 << j) <= n; j++) {
		for (int i = 1; i <= n; i++) {
			f[i][j] = f[f[i][j - 1]][j - 1];
		}
	}
}
int lca(int u, int v) {
	if (dep[u] < dep[v]) {
		swap(u, v);
	}
	for (int i = 22; i >= 0; i--) {
		if (dep[f[u][i]] >= dep[v]) {
			u = f[u][i];
		}
	}
	if (u == v) return u;
	for (int i = 22; i >= 0; i--) {
		if (f[u][i] != f[v][i]) {
			u = f[u][i];
			v = f[v][i];
		}
	}
	return f[u][0];
}
int main() {
	cin.tie(0)->sync_with_stdio(0);
	cin >> n >> m >> s;
	for (int i = 1; i <= n -1; i++) {
		int u, v;
		cin >> u >> v;
		e[u].push_back(v);
		e[v].push_back(u);
	}
	dfs(s, 0);
	init();
	for (int i = 1; i <= m; i++) {
		int u, v;
		cin >> u >> v;
		cout << lca(u, v) << "\n";
	}
	return 0;
}
```