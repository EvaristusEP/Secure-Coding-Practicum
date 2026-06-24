# 7C. Race Condition

## Tujuan

Memahami risiko Race Condition pada sistem yang diakses secara bersamaan.

## Implementasi

![Voucher Service](../../screenshots/p7/03/voucher-service.png)

## Kode Race Condition

![Race Condition](../../screenshots/p7/03/race-condition-code.png)

## Hasil Eksekusi

![Race Condition Output](../../screenshots/p7/03/race-condition-output.png)

## Analisis

Dua thread melakukan pengecekan voucher secara bersamaan. Karena tidak ada mekanisme sinkronisasi, kedua pengguna berhasil menggunakan voucher yang sama.

## Kesimpulan

Race Condition dapat menyebabkan inkonsistensi data dan pelanggaran aturan bisnis.
