利用十进制数存储任意进制，并且转化为目标进制数。  
以3进制为例。  
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int m = 5; // 代表最长为5位的三进制数
    vector<int> pow(m);
    pow[0] = 1;

    for (int i = 1; i < m; ++i){
        pow[i] = pow[i - 1] * 3;
    }
    // 预处理所有从0 ~ 3^m - 1的三进制数
    for (int i = 0; i < pow[m - 1] * 3; ++i){
        int n = i;
        string s = "";
        for (int j = m - 1; j >= 0; --j){
            int x = n / pow[j] % 3;
            s += to_string(x);
        }
        cout << s << endl;
    }

    return 0;
}
```