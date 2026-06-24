# Modul 1 - Authentication & Login Security

## Tujuan

Menganalisis kelemahan desain autentikasi dan memperbaikinya menggunakan prinsip Security by Design.

---

## Permasalahan Awal

Beberapa masalah yang ditemukan:

- Tidak ada pencatatan login gagal.
- Tidak ada pembatasan percobaan login.
- Session tidak terikat dengan kepemilikan user.
- Potensi brute force attack.

## Kode Awal

![Insecure Authentication](../../screenshots/sbd/modul1/insecure-auth.png)

---

## Risiko Keamanan

Jika tidak ada mekanisme pembatasan login:

- Attacker dapat mencoba password berulang kali.
- Akun lebih mudah diambil alih.
- Sulit melakukan audit aktivitas login.

---

## LoginAttempt

![LoginAttempt](../../screenshots/sbd/modul1/login-attempt-model.png)

LoginAttempt digunakan untuk mencatat aktivitas autentikasi seperti user, alamat IP, waktu login, dan status berhasil atau gagal.

---

## Session Ownership & Lockout

![Secure Authentication](../../screenshots/sbd/modul1/secure-auth.png)

Perbaikan dilakukan dengan:

- Menambahkan lockout mechanism.
- Membatasi percobaan login.
- Mengikat session kepada user yang melakukan autentikasi.
- Mencatat aktivitas login untuk kebutuhan audit.

---

## Kesimpulan

Autentikasi tidak hanya memverifikasi password, tetapi juga harus mampu mengendalikan percobaan login dan memastikan kepemilikan session.
