---
title: "Codeforces Round 1065 (Div. 3) 리뷰"
date: 2025-11-21
---

# A. Shizuku Hoshikawa and Farm Legs

저는 완전탐색으로 풀었지만, 에디토리얼 풀이는 다음과 같습니다.

1. $n$이 홀수인 경우: 가능한 구성 없음.

2. $n$이 짝수인 경우: 소가 가진 다리 수를 먼저 생각해봅시다. 이 값은 $n$ 이하의 4의 배수입니다. 나머지 다리는 닭의 다리가 됩니다. 따라서 답은 $n$ 이하의 음이 아닌 4의 배수의 개수이고, 이는 $\left\lfloor 4n \right\rfloor + 1$입니다.
