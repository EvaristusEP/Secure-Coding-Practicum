# Modul 1 - Authentication & Login Security

## Tujuan

Menganalisis kelemahan desain autentikasi dan memperbaikinya menggunakan prinsip Security by Design.

---

## Permasalahan Awal

Beberapa masalah yang ditemukan:

* Tidak ada pencatatan login gagal.
* Tidak ada pembatasan percobaan login.
* Session tidak terikat dengan kepemilikan user.
* Potensi brute force attack.

---

## Risiko Keamanan

Jika tidak ada mekanisme pembatasan login:

* Attacker dapat mencoba password berulang kali.
* Akun lebih mudah diambil alih.
* Sulit melakukan audit aktivitas login.

---

## Perbaikan yang Dilakukan

### LoginAttempt Model

Mencatat:

* User
* IP Address
* Waktu login
* Status berhasil/gagal

### Lockout Mechanism

Akun dikunci sementara setelah beberapa kali gagal login.

### Session Ownership

Session harus selalu terikat dengan user yang melakukan login.

---

## Hasil

Implementasi menghasilkan autentikasi yang lebih aman serta mengurangi risiko brute force dan session abuse.

---

## Kesimpulan

Autentikasi tidak hanya memverifikasi password, tetapi juga harus mampu mengendalikan percobaan login dan memastikan kepemilikan session.
