# Praktikum 5 - Password Reset Token Leak

## Tujuan

Mempelajari risiko kebocoran token reset password dan penerapan secure by design.

---

## Versi Insecure

### Screenshot Kode

![Insecure Code](../screenshots/p5/insecure-code.png)

### Screenshot Output

![Insecure Output](../screenshots/p5/insecure-output.png)

### Analisis

Masalah yang ditemukan:

1. Token disimpan dalam bentuk plaintext.
2. Token dicetak ke log.
3. Token tidak memiliki masa berlaku.
4. Token dapat digunakan berulang kali.
5. Tidak terdapat user binding.

---

## Versi Secure

### Screenshot Kode

![Secure Code](../screenshots/p5/secure-code.png)

### Screenshot Output

![Secure Output](../screenshots/p5/secure-output.png)

### Perbaikan

- Menggunakan secure random token.
- Menyimpan hash token.
- Menambahkan expiry time.
- Menerapkan read-once token.
- Menggunakan user binding.

---

## Test Token Reuse

![Token Reuse Test](../screenshots/p5/token-reuse-test.png)

### Hasil Pengujian

Output:

Expected rejection: Token tidak valid

### Analisis

Token yang sudah digunakan tidak dapat digunakan kembali sehingga mencegah replay attack.

---

## Test Wrong User Binding

![Wrong User Binding Test](../screenshots/p5/wrong-user-binding-test.png)

### Hasil Pengujian

Output:

Expected rejection: Token tidak dimiliki user ini

### Analisis

## Analisis Praktikum 5

Pada implementasi insecure ditemukan beberapa kelemahan keamanan. Token reset password disimpan dalam bentuk plaintext dan dicetak ke log sehingga berpotensi bocor kepada pihak yang memiliki akses terhadap sistem. Selain itu token tidak memiliki masa berlaku dan dapat digunakan berulang kali.

Hasil pengujian menunjukkan bahwa token yang sama masih dapat digunakan untuk mengganti password lebih dari satu kali. Kondisi ini memungkinkan penyerang mengambil alih akun apabila token berhasil diperoleh.

Pada implementasi secure dilakukan beberapa perbaikan, yaitu penggunaan secure random token, penyimpanan hash token, penerapan masa berlaku token, mekanisme read-once token, serta user binding. Hasil pengujian menunjukkan bahwa token yang sudah digunakan akan ditolak dan token milik pengguna lain tidak dapat digunakan untuk mengakses akun yang berbeda.

Berdasarkan hasil praktikum dapat disimpulkan bahwa token reset password tidak boleh diperlakukan sebagai string biasa. Token harus memiliki lifecycle, ownership, expiry, dan aturan penggunaan yang jelas agar tidak dapat disalahgunakan.

## Kesimpulan

Token reset password tidak boleh diperlakukan sebagai string biasa. Token harus memiliki lifecycle, ownership, dan aturan penggunaan yang jelas.
