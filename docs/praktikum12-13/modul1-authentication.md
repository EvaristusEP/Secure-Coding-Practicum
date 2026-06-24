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

### Screenshot Kode Awal

![Insecure Authentication](../../screenshots/sbd/modul1/insecure-auth.png)

---

## Perbaikan yang Dilakukan

### LoginAttempt Model

Mencatat:
- User
- IP Address
- Waktu login
- Status berhasil/gagal

### Screenshot LoginAttempt

![Login Attempt Model](../../screenshots/sbd/modul1/login-attempt-model.png)

---

### Session Ownership & Lockout

### Screenshot Implementasi Secure

![Secure Authentication](../../screenshots/sbd/modul1/secure-auth.png)

---

## Kesimpulan

Autentikasi tidak hanya memverifikasi password, tetapi juga harus mampu mengendalikan percobaan login dan memastikan kepemilikan session.
