---
title: "Codeforces Round 1065 (Div. 3) 리뷰"
date: 2025-11-21
---

# A. Shizuku Hoshikawa and Farm Legs

저는 완전탐색으로 풀었지만, 에디토리얼 풀이는 다음과 같습니다.

$n$이 홀수인 경우, 가능한 구성은 없습니다.

$n$이 짝수인 경우, 소의 수가 닭의 수를 결정합니다.  
답은 $n$ 이하의 음이 아닌 4의 배수의 개수이고, 이는 $\left\lfloor 4n \right\rfloor + 1$입니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    cout << (n & 1 ? 0 : n / 4 + 1) << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```
