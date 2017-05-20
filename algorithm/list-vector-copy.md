알고리즘 문제를 풀다보니 list, vector 간 copy 하는 경우에 1번 방법을 주로 써왔는데,

std::copy를 쓰는 방법은 어떻게 할까 궁금해서 찾아보았다.

# 1. 루프를 돌며 추가

```c++
for(auto value : list)
    {
        result.push_back(value);
    }
```

# 2. std::copy 사용

```c++
#include <algorithm>
#include <iterator>

    std::copy(vector.begin(), vector.end(), std::back_inserter(list));
```