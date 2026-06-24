# Praktikum 1 - Fail Open vs Fail Closed

## Tujuan

Memahami perbedaan pendekatan Fail Open dan Fail Closed dalam sistem autentikasi serta dampaknya terhadap keamanan aplikasi.

---

## Source Code Awal

![Source Code](../../screenshots/p10/p1/source-code.png)

Kode awal mengembalikan nilai `true` ketika proses verifikasi token mengalami exception.

---

## Identifikasi Masalah

Pada method berikut:

```php
catch (Exception $e) {
    return true;
}
```

Sistem tetap memberikan akses meskipun proses validasi token gagal.

Hal ini termasuk implementasi Fail Open yang tidak aman untuk fitur autentikasi.

---

## Risiko Keamanan

Jika service autentikasi gagal:

* User tanpa token valid tetap memperoleh akses.
* Penyerang dapat melewati proses autentikasi.
* Area admin dapat diakses tanpa izin.

---

## Implementasi Fail Closed

```php
catch (Exception $e) {
    return false;
}
```

Dengan pendekatan ini akses akan ditolak ketika terjadi kegagalan validasi.

---

## Trade-Off

Keuntungan:

* Lebih aman.
* Mencegah akses tidak sah.

Kekurangan:

* User sah dapat ditolak ketika dependency sedang bermasalah.
* Availability sedikit berkurang.

---

## Analisis

| Fitur              | Dependency Gagal       | Keputusan           |
| ------------------ | ---------------------- | ------------------- |
| Login Admin        | Auth Service           | Fail Closed         |
| Pembayaran         | Payment Service        | Fail Closed         |
| Rekomendasi Produk | Recommendation Service | Fail Open           |
| Email Notifikasi   | SMTP Service           | Retry melalui Queue |

---

## Kesimpulan

Fitur yang berhubungan dengan autentikasi, otorisasi, dan transaksi wajib menggunakan pendekatan Fail Closed untuk menjaga keamanan sistem.
