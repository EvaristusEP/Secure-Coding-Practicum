# Modul 3 - E-Wallet System

## Tujuan

Menerapkan aturan domain pada sistem dompet digital.

---

## Permasalahan Awal

Masalah yang ditemukan:

- Saldo dapat menjadi negatif.
- Tidak ada batas transaksi harian.
- Race condition memungkinkan double spending.

## Kode Awal

![Insecure Wallet](../../screenshots/sbd/modul3/insecure-wallet.png)

---

## Risiko Keamanan

Dampak yang mungkin terjadi:

- Kerugian finansial.
- Saldo tidak konsisten.
- Penyalahgunaan sistem.

---

## Domain Rules

![Domain Rules](../../screenshots/sbd/modul3/domain-rules.png)

Aturan utama:

- Saldo tidak boleh negatif.
- Transfer harus divalidasi.
- Daily limit harus diterapkan.

---

## Transaction Safety

![Transaction Safety](../../screenshots/sbd/modul3/transaction-safety.png)

Operasi keuangan harus dilakukan secara atomik agar tidak terjadi inkonsistensi data akibat race condition.

---

## Kesimpulan

Aturan domain harus ditempatkan pada model bisnis, bukan hanya pada controller.
