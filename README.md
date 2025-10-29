// Nomor 1
#include <iostream>
#include <vector>
using namespace std;

long long angka_terbalik(long long num) {
    if (num == 0) return 0;
    bool neg = num < 0;
    unsigned long long x = neg ? static_cast<unsigned long long>(-num) : static_cast<unsigned long long>(num);
    unsigned long long rev = 0;
    while (x > 0) {
        rev = rev * 10 + (x % 10);
        x /= 10;
    }
    long long res = static_cast<long long>(rev);
    return neg ? -res : res;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    if (!(cin >> n)) return 0;
    vector<long long> arr(n);
    for (int i = 0; i < n; ++i) cin >> arr[i];

    long long sum = 0;            
    for (int i = 0; i < n; ++i) {
        if (i % 2 == 0) arr[i] = angka_terbalik(arr[i]);
        sum += arr[i];
    }

    cout << sum << '\n';
    return 0;
}
