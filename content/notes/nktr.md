---
title: "牛客tracker记录"
date: 2025-11-15
draft: false
hideToc: true
tags: ["牛客竞赛"]
---

#### 2025-11-16 |模拟|枚举|状态变化|位运算|
``` cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define all(a) a.begin(), a.end()
const int inf = 1e9 + 7;

array<int, 8> dx = {1, -1, -1, 1, 1, -1, 0, 0};
array<int, 8> dy = {1, -1, 1, -1, 0, 0, 1, -1};

constexpr int N = 5;
using Grid = array<array<char, N>, N>;
using Count = array<array<int, N>, N>;

bool check(ll x, const Grid& c) {
	for (int i = 1; i < N; i++) {
		for (int j = 1; j < N; j++) {
			int w = (i - 1) * (N - 1) + (j - 1); // 将映射为一维数组的下标
			int sta = (x >> w) & 1; // 获取当前格子的状态
			if (sta == 1 && c[i][j] >= '0' && c[i][j] <= '9') {
				return false; // 格子是雷并且是一个数字格子（不合法）
			}

			// 合法时判断数字格子周围的雷数
			if (c[i][j] >= '0' && c[i][j] <= '9') {
				int cur = c[i][j] - '0';
				int cnt = 0;
				for (int k = 0; k < 8; k++) {
					int nx = i + dx[k];
					int ny = j + dy[k];
					if (nx < 1 || ny < 1 || nx >= N || ny >= N) {
						continue;
					} // 出界
					int w2 = (nx - 1) * (N - 1) + (ny - 1);
					int sta2 = (x >> w2) & 1;
					cnt += sta2;
				}
				// 周围雷的数量和格子显示的数字不一样
				if (cnt != cur) {
					return false;
				}
			}
		}
	}
	return true;
}

int main() {
	cin.tie(0)->sync_with_stdio(0);

	Grid c;
	for (int i = 1; i < N; i++) {
		for (int j = 1; j < N; j++) {
			cin >> c[i][j];
		}
	}

	constexpr int range = 1 << 16;
	Count cnt = {};

	for (int i = 0; i < range; i++) {
		if (check(i, c)) {
			for (int a = 0; a < N; a++) {
				for (int b = 1; b < N; b++) {
					int w = (a - 1) * (N - 1) + (b - 1);
					int sta = (i >> w) & 1;
					if (sta) cnt[a][b] |= 1;
					else cnt[a][b] |= 2;
				}
			}
		}
	}

	for (int i = 1; i < N; i++) {
		for (int j = 1; j < N; j++) {
			if (c[i][j] >= '0' && c[i][j] <= '9') {
				cout << c[i][j];
			} else {
				if (cnt[i][j] == 3) {
					cout << ".";
				} else if (cnt[i][j] == 2) {
					cout << "O";
				} else {
					cout << "X";
				}
			}
		}
		cout << "\n";
	}


	return 0;
}

```

#### 2025-11-15 |逆元|组合数Comb|快速幂|

``` cpp

#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define all(a) a.begin(), a.end()

const ll MOD = 998244353;

const int N = 2000000;
vector<ll> a(N); 

ll qpow(ll x, ll y) {
	ll s = 1;
	x %= MOD;
	while (y > 0) {
		if (y & 1) s = s * x % MOD;
		x = x * x % MOD;
		y >>= 1;
	}
	return s;
}

void init() {
	a[1] = 1;
	ll s = 1;
	for (ll i = 2; i < N; i++) {
		ll cnt = 0;
		s = i;
		while (s % 2 == 0) {
			s /= 2;
			cnt++;
		}
		a[i] = a[i - 1] + cnt;
	}	
}

void solve() {
	ll n;
	cin >> n;
	ll x = a[n];
	ll ans = qpow(x, MOD - 2);
	cout << ans << " ";
}
int main() {	
	cin.tie(0)->sync_with_stdio(0);	
	int t;
	cin >> t;
	init();
	while (t--) {
		solve();
	}
	return 0;
}

```