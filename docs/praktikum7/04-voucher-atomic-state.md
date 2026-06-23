# 04. Atomic Transition

## Tujuan

Mencegah Race Condition menggunakan Lock.

## Implementasi Lock

![Lock](../../screenshots/p7/04/lock-code.png)

## Hasil Eksekusi

![Atomic Output](../../screenshots/p7/04/atomic-output.png)

## Analisis

Lock memastikan hanya satu thread yang dapat menggunakan voucher pada satu waktu. Akibatnya hanya satu pengguna yang berhasil, sedangkan pengguna lainnya ditolak.

## Kesimpulan

Operasi penting yang mengubah state harus dijalankan secara atomik untuk menjaga konsistensi data.
