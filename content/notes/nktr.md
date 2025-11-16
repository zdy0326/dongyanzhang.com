---
title: "牛客tracker记录"
date: 2025-11-15
draft: false
hideToc: true
tags: ["牛客竞赛"]
---

#### 2025-11-16
```

```

#### 2025-11-15

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