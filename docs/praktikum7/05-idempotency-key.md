# 05. Idempotency Key

## Tujuan

Mencegah transaksi yang sama diproses lebih dari satu kali.

## Implementasi

![Idempotency Code](../../screenshots/p7/05/idempotency-code.png)

## Hasil Eksekusi

![Idempotency Output](../../screenshots/p7/05/idempotency-output.png)

## Analisis

Sistem menyimpan transaction ID yang telah diproses. Ketika callback yang sama dikirim kembali, transaksi langsung ditolak sehingga saldo tidak bertambah dua kali.

## Kesimpulan

Idempotency Key sangat penting pada sistem pembayaran untuk menghindari transaksi ganda.
