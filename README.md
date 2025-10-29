Nomor 1

    #include <iostream>
    #include <vector>
    #include <sstream>
    using namespace std;

    long long angkadibalik(long long num) {
        bool tanda_negatif = num < 0;
        if (tanda_negatif) num = -num;

        long long rev = 0;
        while (num > 0) {
            rev = rev * 10 + (num % 10);
            num /= 10;
        }

        return tanda_negatif ? -rev : rev;
    }

    int main() {
        string input;
        getline(cin, input);

        stringstream ss(input);
        vector<long long> arr;
        long long num;

        while (ss >> num) arr.push_back(num);

        long long total = 0;
        for (int i = 0; i < (int)arr.size(); i++) {
            long long hasil = (i % 2 == 0) ? angkadibalik(arr[i]) : arr[i];
            total += hasil;
        }

        cout << total;
        return 0;
    }
Nomor 2

    #include <iostream>
    #include <string>
    #include <vector>
    #include <cctype>
    using namespace std;

    // Fungsi untuk membalikkan string (tanpa reverse bawaan)
    string manualReverse(const string& s) {
        string rev = "";
        for (int i = s.length() - 1; i >= 0; --i)
            rev += s[i];
        return rev;
    }

    // Hilangkan huruf vokal dari kata
    string hilangkanvokal(const string& s) {
        string result = "";
        for (char c : s) {
            char lower = tolower(c);
            if (lower != 'a' && lower != 'e' && lower != 'i' && lower != 'o' && lower != 'u')
                result += c;
        }
        return result;
    }

    // Encode: kata asli → sandi
    string encodekata(const string& word) {
        int asciiVal = (int)word[0]; // simpan ASCII huruf pertama
        string asciiStr = to_string(asciiVal);
        string noVowel = hilangkanvokal(word);
        string reversed = manualReverse(noVowel);

        // letakkan ASCII di tengah
        string result = reversed.substr(0, reversed.length() / 2) + asciiStr +
            reversed.substr(reversed.length() / 2);

        return result;
    }

    // Decode: sandi → kemungkinan kata asli
    vector<string> decodekata(const string& cipher) {
        vector<string> results;
        string num = "";
        for (char c : cipher) {
            if (isdigit(c))
                num += c;
        }

        if (num.empty()) {
            cout << "Sandi tidak mengandung kode ASCII!" << endl;
            return results;
        }

        size_t pos = cipher.find(num);
        string left = cipher.substr(0, pos);
        string right = cipher.substr(pos + num.length());
        string combined = left + right;

        // Balik lagi huruf yang dibalik
        string reversed = manualReverse(combined);

        // Ubah angka ke karakter
        char firstChar = (char)stoi(num);

        // Bentuk kata tanpa vokal
        string baseWord = firstChar + reversed;
        results.push_back(baseWord);

        // Tambahkan kemungkinan kata dengan vokal (sederhana)
        string vowels = "aeiou";
        for (char v : vowels) {
            for (int i = 1; i < (int)baseWord.length(); i++) {
                string candidate = baseWord.substr(0, i) + v + baseWord.substr(i);
                results.push_back(candidate);
            }
        }

        return results;
    }

    int main() {
        cout << "=== Mesin Lost and Found ===\n";
        cout << "1. Encode kata asli\n";
        cout << "2. Decode sandi\n";
        cout << "Pilih mode (1/2): ";

        int choice;
        cin >> choice;
        cin.ignore();

        if (choice == 1) {
            string word;
            cout << "\nMasukkan kata asli: ";
            getline(cin, word);
            cout << "Hasil encode: " << encodekata(word) << endl;
        }
        else if (choice == 2) {
            string cipher;
            cout << "\nMasukkan sandi: ";
            getline(cin, cipher);
            vector<string> decoded = decodekata(cipher);

            cout << "\nKemungkinan kata asli (tanpa dan dengan vokal):\n";
            for (string w : decoded)
                cout << "- " << w << endl;
        }
        else {
            cout << "Pilihan tidak valid!\n";
        }

        return 0;
    }
Nomor 3

    #include <iostream>
    #include <string>
    using namespace std;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);

        const int durasiHijau = 20;
        const int durasiKuning = 3;
        const int durasiMerah = 80;
        const int total = durasiHijau + durasiKuning + durasiMerah; // 103
        const int startOffset = 45; // pada detik ke-45 APILL mulai menunjukkan KUNING

        cout << "Masukkan detik (bisa banyak, pisah pakai spasi atau newline). Ctrl+D/Ctrl+Z untuk selesai.\n";

        long long t;
        while (cin >> t) {
            int pos = (int)((((t - startOffset) % total) + total) % total);

            string warna;
            if (pos < durasiKuning) warna = "Kuning";
            else if (pos < durasiKuning + durasiMerah) warna = "Merah";
            else warna = "Hijau";

            cout << t << " -> " << warna << '\n';
        }

        return 0;
    }
