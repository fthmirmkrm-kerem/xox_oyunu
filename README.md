# xox_oyunu
kazanabilirsin.org.tr.com.tr
#include <iostream>
#include <vector>

// Tahtayı çizme fonksiyonu 
void tahta_ciz(const std::vector<char>& tahta) {
    std::cout << "\n";
    std::cout << " " << tahta[0] << " | " << tahta[1] << " | " << tahta[2] << " \n";
    std::cout << "---|---|---\n";
    std::cout << " " << tahta[3] << " | " << tahta[4] << " | " << tahta[5] << " \n";
    std::cout << "---|---|---\n";
    std::cout << " " << tahta[6] << " | " << tahta[7] << " | " << tahta[8] << " \n";
    std::cout << "\n";
}

// Kazanma durumunu kontrol etme fonksiyonu
bool kazanma_kontrol(const std::vector<char>& tahta, char oyuncu) {
    // Satır ve Sütun kontrolü
    for (int i = 0; i < 3; ++i) {
        // Satır (0, 1, 2), (3, 4, 5), (6, 7, 8)
        if (tahta[i * 3] == oyuncu && tahta[i * 3 + 1] == oyuncu && tahta[i * 3 + 2] == oyuncu) {
            return true;
        }
        // Sütun (0, 3, 6), (1, 4, 7), (2, 5, 8)
        if (tahta[i] == oyuncu && tahta[i + 3] == oyuncu && tahta[i + 6] == oyuncu) {
            return true;
        }
    }

    // Çapraz kontrol (Sol üstten sağ alta)
    if (tahta[0] == oyuncu && tahta[4] == oyuncu && tahta[8] == oyuncu) {
        return true;
    }

    // Çapraz kontrol (Sağ üstten sol alta)
    if (tahta[2] == oyuncu && tahta[4] == oyuncu && tahta[6] == oyuncu) {
        return true;
    }

    return false;
}

// Tahtanın dolu olup olmadığını kontrol etme fonksiyonu
bool tahta_dolu(const std::vector<char>& tahta) {
    for (char hucre : tahta) {
        if (hucre != 'X' && hucre != 'O') {
            return false; // Boş bir hücre var
        }
    }
    return true; // Tahta dolu (berabere)
}

int main() {
    // Tahta: 1'den 9'a kadar pozisyonları temsil eden karakterler
    std::vector<char> tahta = {'1', '2', '3', '4', '5', '6', '7', '8', '9'};
    char simdiki_oyuncu = 'X';
    int secim;
    bool oyun_bitmedi = true;

    std::cout << "XOX (Tic-Tac-Toe) Oyunu Basliyor!\n";

    while (oyun_bitmedi) {
        tahta_ciz(tahta);

        // Kullanıcıdan geçerli bir hamle alınana kadar döngü
        do {
            std::cout << "Oyuncu " << simdiki_oyuncu << ", bir pozisyon secin (1-9): ";
            std::cin >> secim;

            // Girişin geçerli olup olmadığını kontrol et
            if (std::cin.fail()) {
                std::cout << "Gecersiz giris! Lutfen bir sayi girin (1-9).\n";
                std::cin.clear();
                std::cin.ignore(10000, '\n');
                secim = -1; // Tekrar döngüye girmesi için
            } else if (secim < 1 || secim > 9 || tahta[secim - 1] == 'X' || tahta[secim - 1] == 'O') {
                std::cout << "Gecersiz secim! Lutfen bos bir hucre (1-9) secin.\n";
                secim = -1; // Tekrar döngüye girmesi için
            }
        } while (secim == -1);

        // Hamleyi yap
        tahta[secim - 1] = simdiki_oyuncu;

        // Kazanma kontrolü
        if (kazanma_kontrol(tahta, simdiki_oyuncu)) {
            tahta_ciz(tahta);
            std::cout << "Tebrikler! Oyuncu " << simdiki_oyuncu << " kazandi!\n";
            oyun_bitmedi = false;
        }
        // Beraberlik kontrolü
        else if (tahta_dolu(tahta)) {
            tahta_ciz(tahta);
            std::cout << "Oyun Berabere!\n";
            oyun_bitmedi = false;
        }
        // Sıra değiştirme
        else {
            simdiki_oyuncu = (simdiki_oyuncu == 'X') ? 'O' : 'X';
        }
    }

    return 0;
}
