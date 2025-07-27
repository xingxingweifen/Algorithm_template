### 埃式筛模板
```cpp
int mx = 1e6;
vector<int> f(mx + 1, 1);
// 处理完成之后对于i f[i] = 1代表是质数，否则不是质数
auto init = []()->int{
    f[1] = 0;
    for (int i = 2; i <= mx; ++i){
        if (f[i] == 1){
            for (int j = i + i; j <= mx; j += i){
                f[j] = 0;
            }
        }
    }
    return 0;
}();
```
### 分解质因数模板
分解质因数
```cpp
#include <bits/stdc++.h>
using namespace std;

// 分解质因数
vector<int> primerFactor(int n){
    vector<int> res;
    for (int i = 2; i * i <= n; ++i){
        while (n % i == 0){
            res.push_back(i);
            n /= i;
        }
    }
    if (n > 1) res.push_back(n);
    return res;
}

int main(){
    ostream_iterator<int> out(cout, " ");
    auto ret = primerFactor(1001);
    copy(ret.begin(), ret.end(), out);

    return 0;
}

// 结果为 7 11 13
```