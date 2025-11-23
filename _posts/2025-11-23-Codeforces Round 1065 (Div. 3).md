---
title: "Codeforces Round 1065 (Div. 3)"
date: 2025-11-23
---

# A

완전탐색으로 해결했습니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        int cnt = 0;
        for (int chickens = 0; chickens <= 100; chickens++) {
            for (int cows = 0; cows <= 100; cows++) {
                if (2 * chickens + 4 * cows == n) cnt++;
            }
        }
        cout << cnt << '\n';
    }
}
```

# B

수식을 잘 정리하면 결국 $\lvert a_n - a_1 \rvert$의 최솟값을 구하는 문제입니다. 세 가지 경우로 나누어 생각해 볼 수 있습니다.

1. $a_n$, $a_1$ 둘 다 -1인 경우: 두 값을 같게 만들면 $\lvert a_n - a_1 \rvert$이 최솟값 0을 가집니다. 사전식 순서로 가장 앞에 오는 것을 출력해야 하므로 0, 0을 넣어줍니다.

2. $a_n$만 -1인 경우: $a_n$에 $a_1$ 값을 넣어줍니다.

3. $a_1$만 -1인 경우: $a_1$에 $a_n$ 값을 넣어줍니다.

나머지 $a_2 \cdots a_{n-1}$ 중 -1이 있다면 사실 아무 값을 넣어도 상관없습니다. 결과에 영향을 미치지 않는 항이기 때문이죠. 하지만 사전식 순서로 가장 앞에 오는 것을 출력해야 하므로 0을 넣어줍니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int intabs(int n) {
    if (n >= 0) return n;
    return -n;
}

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        vector<int> a(n + 1);
        for (int i = 1; i <= n; i++) {
            cin >> a[i];
            if ((1 < i && i < n) && a[i] == -1) a[i] = 0;
        }
        if (a[1] == -1 && a[n] == -1) {
            a[1] = 0;
            a[n] = 0;
        } else if (a[1] == -1) {
            a[1] = a[n];
        } else if (a[n] == -1) {
            a[n] = a[1];
        }
        cout << intabs(a[n] - a[1]) << '\n';
        for (int i = 1; i <= n; i++) cout << a[i] << ' ';
        cout << '\n';
    }
}
```

# C1

DP인 줄 알고 삽질했는데... XOR 연산의 성질을 잘 이해하는지 묻는 문제였습니다.

$2n$개 요소 전체를 XOR한 값이 0이라면, 무승부입니다. (Ajisai의 최종점수 = Mai의 최종점수)

$2n$개 요소 전체를 XOR한 값이 1이라면, $a_i \not= b_i$를 만족하는 $i$-th 턴 중, 가장 마지막 턴을 가져가는 사람이 이깁니다. $a_i \not= b_i$인 경우 $a_i$와 $b_i$를 swap하는 것은 결국 나의 최종점수와 상대의 최종점수를 바꾸는 것과 마찬가지이기 때문입니다.

$2n$개 요소 전체를 XOR한 값이 1이라는 것은, 전체에서 1이 홀수번 등장했다는 것을 의미합니다. 홀수 = 짝수 + 홀수이고, 1을 홀수개 가져간 사람이 자신의 최종점수가 1이 되어 승리합니다. 1의 개수의 짝홀성을 결정짓는 마지막 1을 누가 가져가느냐의 싸움입니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n), b(n);
    int x = 0;  // 0 ^ 0 = 0 and 0 ^ 1 = 1 -> x is set to a[0]
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        x ^= a[i];
    }
    for (int i = 0; i < n; i++) {
        cin >> b[i];
        x ^= b[i];
    }
    if (x == 0) {
        cout << "Tie" << '\n';
    } else {
        int i = n - 1;
        while (a[i] == b[i]) i--;
        cout << (i & 1 ? "Mai" : "Ajisai") << '\n';
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```

# C2

$2n$개 요소 전체를 XOR한 값의 비트 중, 가장 높은 1의 비트 인덱스를 k라고 합시다. 이 k번 비트 역시 전체에서 홀수번 등장합니다. 이 비트를 누가 한번 더 가져가느냐에 따라 승패가 결정됩니다. 나머지는 C1 문제와 본질적으로 동일합니다. 가장 높은 1의 비트 인덱스 구하기, k번 비트 확인하기 등 간단한 비트연산 코드를 작성해야 합니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int msb_pos(int x) {
    int p = -1;
    while (x > 0) {
        p++;
        x >>= 1;
    }
    return p;
}

void solve() {
    int n;
    cin >> n;
    vector<int> a(n), b(n);
    int x = 0;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        x ^= a[i];
    }
    for (int i = 0; i < n; i++) {
        cin >> b[i];
        x ^= b[i];
    }
    if (x == 0) {
        cout << "Tie" << '\n';
    } else {
        int msb = msb_pos(x);
        int i = n - 1;
        while (((a[i] >> msb) & 1) == ((b[i] >> msb) & 1)) i--;
        cout << (i & 1 ? "Mai" : "Ajisai") << '\n';
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```

# D

u, v가 트리의 간선을 이룬다면 다음 두 가지를 만족합니다.

- u < v
- p에서 u가 v보다 먼저 등장한다.

다음 두 배열을 정의합니다.

- pre[i] = "$p_1,\cdots,p_i$ 중 최솟값"
- suf[i] = "$p_i,\cdots,p_n$ 중 최댓값"

pre[i-1] > suf[i]인 i가 단 하나라도 존재하면 트리를 구성할 수 없습니다. ... 왜 그럴까요? 사실 이 부분을 많이 고민했는데, 생각보다 간단합니다. 트리에서 임의의 정점 2개를 선택하면, 두 정점 사이에는 반드시 경로가 존재해야 합니다. 그런데 만약 pre[i-1] > suf[i]인 i가 단 하나라도 있다면? { p[1],...,p[i-1] }, { p[i],...,p[n] } 두 정점 집합 사이에 간선이 생기지 않습니다. 왼쪽 집합의 모든 값이 오른쪽 집합의 모든 값보다 항상 크기 때문에, 두 집합 간 어떤 정점 쌍도 간선을 만들지 못합니다. "pre[i-1] > suf[i]인 i가 단 하나라도 존재하지 않을 것"은 트리구성의 필요조건입니다.

그렇다면 충분조건에 대해 생각해봅시다. 모든 i에 대해 pre[i-1] < suf[i]라면, 트리가 구성된다는 것을 어떻게 보장할 수 있을까요? { p[1],...,p[i-1] }, { p[i],...,p[n] } 두 집합에 대해, pre[i-1]은 p[i-1]보다 작고, 먼저 등장합니다. 마찬가지로 suf[i]는 p[i]보다 크고, 이후에 등장합니다. 따라서 p[i-1], pre[i-1] 사이 간선이 존재하고, suf[i], p[i] 사이 간선이 존재합니다. 그리고 모든 i에 대해 pre[i-1], suf[i] 사이 간선은 존재합니다. 따라서 모든 i에 대해 p[i-1]->p[i] 경로는 반드시 존재합니다. 즉 모든 정점이 서로 연결되어 있으므로, 스패닝 트리를 갖습니다.

\* 물론 p[i-1] = pre[i-1]이거나 p[i] = suf[i]인 경우도 있지만, 이러한 경우에도 달라지는 것은 없습니다.

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;

    vector<int> p(n);
    vector<int> pre(n);  // pre[i] = "the minimum of p_1,...,p_i"
    vector<int> suf(n);  // suf[i] = "the maximum of p_i,...,p_n"

    for (int i = 0; i < n; i++) cin >> p[i];

    // fill pre
    for (int i = 0; i < n; i++) {
        if (i == 0) {
            pre[i] = p[i];
        } else {
            pre[i] = min(pre[i - 1], p[i]);
        }
    }

    // fill suf
    for (int i = n - 1; i >= 0; i--) {
        if (i == n - 1) {
            suf[i] = p[i];
        } else {
            suf[i] = max(suf[i + 1], p[i]);
        }
    }

    // if there exists some i such that pre[i-1] > suf[i], then answer is "No".
    for (int i = 1; i < n; i++) {
        if (pre[i - 1] > suf[i]) {
            cout << "No" << '\n';
            return;
        }
    }
    cout << "Yes" << '\n';
    return;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```

---

# 후기

아직 많이 부족한 것 같습니다. 요즘 어과초를 재밌게 보고 있는데, 신기하게도 이번 div3 출제자분이 [토키와다이의 레일건](https://codeforces.com/profile/reirugan)이었습니다. 덕분에 즐겁게 참여했습니다!
