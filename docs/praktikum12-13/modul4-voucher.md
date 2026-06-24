# Modul 4 - Voucher & Promo System

## Tujuan

Mencegah penyalahgunaan voucher melalui mekanisme keamanan yang tepat.

---

## Permasalahan Awal

Masalah yang ditemukan:

- Voucher dapat digunakan ganda.
- Tidak ada idempotency.
- Kuota voucher tidak dikontrol dengan baik.

## Kode Awal

![Insecure Voucher](../../screenshots/sbd/modul4/insecure-voucher.png)

---

## Risiko Keamanan

Dampak yang mungkin terjadi:

- Double redemption.
- Penyalahgunaan promosi.
- Kerugian finansial.

---

## Pessimistic Locking

![Pessimistic Locking](../../screenshots/sbd/modul4/locking-implementation.png)

Lock digunakan untuk mencegah dua transaksi menggunakan voucher yang sama secara bersamaan.

---

## Idempotency

![Idempotency](../../screenshots/sbd/modul4/idempotency-demo.png)

Request yang sama tidak boleh menghasilkan efek bisnis lebih dari satu kali.

---

## Kesimpulan

Pessimistic locking dan idempotency merupakan mekanisme penting untuk mencegah race condition pada sistem promosi.
