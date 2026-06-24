# Praktikum 4 - Stateless Session Review

## Tujuan

Menganalisis penggunaan session pada lingkungan cloud dan distributed system.

---

## Hasil Review

| Session Driver | Cocok untuk Cloud | Risiko                                         |
| -------------- | ----------------- | ---------------------------------------------- |
| file           | Tidak             | Session hilang ketika request berpindah server |
| database       | Ya                | Beban database meningkat                       |
| redis          | Ya                | Memerlukan infrastruktur Redis                 |
| cookie         | Ya                | Ukuran data terbatas                           |

---

## Analisis

### File Session

Kelebihan:

* Mudah digunakan

Kekurangan:

* Tidak cocok untuk multiple server

---

### Database Session

Kelebihan:

* Shared storage

Kekurangan:

* Menambah beban database

---

### Redis Session

Kelebihan:

* Cepat
* Shared session

Kekurangan:

* Memerlukan Redis server

---

### Cookie Session

Kelebihan:

* Stateless

Kekurangan:

* Kapasitas terbatas

---

## Pertanyaan Diskusi

### Apa yang terjadi jika aplikasi berjalan pada dua server?

User dapat kehilangan session jika request berpindah server.

---

### Apakah session tetap tersedia?

Tergantung driver yang digunakan.

File session biasanya gagal pada arsitektur multi-server.

---

### Driver Terbaik untuk Cloud

Redis merupakan pilihan yang paling umum karena:

* Cepat
* Shared
* Mudah diskalakan

---

## Kesimpulan

Session berbasis Redis lebih cocok digunakan pada deployment cloud modern dibandingkan file session lokal.
