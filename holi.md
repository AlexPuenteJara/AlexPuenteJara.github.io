$c^2=b^2+a^2$
  <!-- begin code -->
  <div class="collapsed code-title" type="button" data-toggle="collapse" data-target="#codeProblemcombinatorics" aria-expanded="false" aria-controls="collapseTwo">
  <!-- title -->
  <i class="fas fa-caret-right"></i> <p class="title">Code</p>
  </div>
  <div id="codeProblemcombinatorics" class="collapse">

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll comb (int n, int m) {
  if (m == 0) return 1;
  if (n == m) return 1;
  return comb(n - 1, m - 1) + comb(n - 1, m);
}
int main () {
  cout << comb(4, 2) << '\n';
  return (0);
}
```

  </div>
  <!-- ends code -->