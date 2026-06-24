# Praktikum 5 - Resilience Design Review

## Tujuan

Menganalisis ketahanan aplikasi ketika dependency utama mengalami gangguan.

---

## Tabel Analisis

| Failure Scenario     | Dampak                      | Keputusan          | Mitigasi                             |
| -------------------- | --------------------------- | ------------------ | ------------------------------------ |
| Database Down        | Login dan transaksi gagal   | Fail Closed        | Database replica, backup, monitoring |
| Redis Down           | Session dan cache terganggu | Fail Open sebagian | Fallback cache                       |
| SMTP Down            | Email gagal terkirim        | Retry Queue        | Retry dan monitoring                 |
| Third Party API Down | Integrasi gagal             | Fail Open terbatas | Timeout dan fallback                 |
| Storage Full         | Upload gagal                | Fail Closed        | Monitoring kapasitas storage         |

---

## Analisis

### Database Down

Dampak:

* Login gagal
* Data tidak dapat disimpan

Risiko:

* Gangguan operasional

Mitigasi:

* Replica database
* Backup
* Monitoring

---

### Redis Down

Dampak:

* Session terganggu
* Cache hilang

Mitigasi:

* Fallback cache
* High availability Redis

---

### SMTP Down

Dampak:

* Email gagal dikirim

Mitigasi:

* Queue
* Retry policy

---

### Third Party API Down

Dampak:

* Fitur tertentu tidak berjalan

Mitigasi:

* Circuit Breaker
* Timeout
* Fallback Response

---

### Storage Full

Dampak:

* Upload gagal

Mitigasi:

* Monitoring kapasitas
* Alert otomatis

---

## Failure Scenario Paling Berbahaya

Database Down.

Alasan:

* Seluruh proses bisnis bergantung pada database.
* Dapat menghentikan operasi aplikasi secara total.

---

## Rekomendasi

* Monitoring aktif
* Retry policy
* Circuit breaker
* Backup berkala
* Disaster recovery plan

---

## Kesimpulan

Sistem yang baik bukan sistem yang tidak pernah gagal, tetapi sistem yang mampu gagal dengan aman dan tetap menjaga keamanan data pengguna.
