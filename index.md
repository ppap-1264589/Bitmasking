### FRIEND.cpp

``` c++
///Template created by PPAP_1264589
#include <bits/stdc++.h>
#define Task              "DRAFT"
#define ll                long long
#define pii               pair<int, int>
#define f                 first
#define s                 second
#define MP                make_pair
#define rollup(i,a,b)     for (int i = (a); i <= (b); i++)
#define rolldown(i,a,b)   for (int i = (a); i >= (b); i--)
#define bit(x, i)         ((x >> (i)) & 1)
using namespace std;

void setFile(){
    ios_base::sync_with_stdio(0), cin.tie(nullptr), cout.tie(nullptr);
    freopen (Task".inp", "r", stdin);
    freopen (Task".out", "w", stdout);
}

int n,q;
int S, F;
int c[16][16];
int dp[1 << 16]; //bieu dien trang thai nguoi i -> j va j -> i
map<int, pii> mm;
int sum = 0;

void in(){
    cin >> S >> F;
    cin >> n >> q;
    int u,v,w;
    memset(dp, -1, sizeof(dp));
    while (cin >> u >> v >> w){
        u--, v--;
        c[u][v] = c[v][u] += w;
        sum += w;
        int pr = (1 << u) + (1 << v);
        dp[pr] += w;
    }
}

void dp_bit(){
    rollup(x, 0, (1 << n)-1){
        rollup(i, 0, n-1){
            rollup(j, i+1, n-1){ 
                if (bit(x, i) && bit(x, j)){ 
                    int v = x &~ ((1 << i) | (1 << j));
                    if (dp[x] <= dp[v] + c[i][j]){
                        dp[x] = dp[v] + c[i][j];
                        mm[x] = MP(i+1, j+1);
                    }
                }
            }
        }
    }

    int s = dp[(1 << n) - 1];
    cout << (sum - s)*F + s*S;
}

void Trace(){
    int fi = (1 << n) - 1;
    while (fi){
        int a = mm[fi].f;
        int b = mm[fi].s;
        cout << a << " " << b << "\n";
        fi = fi - (1 << (a-1)) - (1 << (b-1));
    }
}

int main (){
    setFile();
    in();
    dp_bit();
    cout << endl;
    Trace();
    return 0;
}
```
