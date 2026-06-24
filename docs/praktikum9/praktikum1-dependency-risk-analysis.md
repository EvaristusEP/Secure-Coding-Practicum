# Praktikum 1 - Dependency Risk Analysis

## Tujuan

Melakukan audit dependency pada aplikasi Spring Boot untuk mengidentifikasi kerentanan keamanan yang dapat berdampak terhadap sistem fintech.

---

## Setup Project

### Dependency Awal

![Dependencies](../../screenshots/p9/p1/pom-dependencies.png)

Project menggunakan Spring Boot 2.3.1.RELEASE dengan beberapa dependency tambahan yang sebagian besar sudah cukup lama dan berpotensi memiliki kerentanan yang telah dipublikasikan.

---

## Tugas 1.1 - Scan & Inventarisasi

### Menjalankan OWASP Dependency Check

![Dependency Check Command](../../screenshots/p9/p1/dependency-check-command.png)

Tool OWASP Dependency Check digunakan untuk memindai seluruh dependency dan mencocokkannya dengan database CVE yang diketahui.

### Ringkasan Hasil

![Dependency Check Summary](../../screenshots/p9/p1/dependency-check-summary.png)

### Inventarisasi Dependency Berisiko

| Library | Versi | Risiko Umum |
|----------|--------|-------------|
| Spring Boot | 2.3.1.RELEASE | Banyak dependency transitif sudah usang |
| Log4j Core | 2.14.0 | Rentan terhadap Log4Shell (RCE) |
| Jackson Databind | 2.9.8 | Kerentanan deserialisasi |
| JJWT | 0.9.0 | Dependency lama dan tidak aktif diperbarui |
| Commons Collections | 3.2.1 | Unsafe deserialization |
| PostgreSQL Driver | 42.2.5 | Versi lama dengan berbagai perbaikan keamanan yang belum tersedia |

> Catatan: Nomor CVE, severity, dan CVSS score akan disesuaikan setelah hasil OWASP Dependency Check tersedia.

---

## Tugas 1.2 - Analisis Dampak

### 1. Exploitability

#### Log4j

Kerentanan Log4Shell memungkinkan attacker mengirim payload khusus yang diproses oleh mekanisme logging. Dalam beberapa konfigurasi, serangan dapat dilakukan tanpa autentikasi dan berasal langsung dari internet.

#### Jackson Databind

Kerentanan deserialisasi memungkinkan objek berbahaya diproses oleh aplikasi ketika menerima data JSON dari pengguna atau sistem eksternal.

#### Commons Collections

Library ini memiliki riwayat gadget chain yang sering digunakan dalam serangan deserialisasi Java untuk memperoleh Remote Code Execution.

---

### 2. Blast Radius

Apabila salah satu kerentanan berhasil dieksploitasi pada sistem fintech, dampak terburuk yang mungkin terjadi adalah:

- Akses tidak sah ke database transaksi.
- Kebocoran data pelanggan.
- Manipulasi saldo atau riwayat pembayaran.
- Pengambilalihan server aplikasi.
- Penyalahgunaan kredensial internal.
- Gangguan operasional layanan pembayaran.

Dampak tersebut dapat menyebabkan kerugian finansial dan reputasi perusahaan.

---

### 3. Prioritas Perbaikan

Jika hanya dua dependency yang dapat diperbaiki hari ini, prioritas utama adalah:

#### Prioritas 1 - Log4j

Alasan:

- Risiko Remote Code Execution.
- Dapat dieksploitasi dari jaringan.
- Dampaknya sangat tinggi.
- Sudah menjadi target eksploitasi massal di dunia nyata.

#### Prioritas 2 - Jackson Databind

Alasan:

- Digunakan pada hampir seluruh proses serialisasi JSON.
- Berinteraksi langsung dengan data eksternal.
- Memiliki banyak riwayat CVE terkait deserialisasi.

---

## Tugas 1.3 - Upgrade Plan

### Tahap 1

Upgrade:

- Log4j
- Jackson

Kedua library memiliki risiko keamanan paling tinggi sehingga harus diprioritaskan.

### Tahap 2

Upgrade:

- PostgreSQL Driver
- JJWT

Perubahan pada tahap ini relatif lebih aman dibanding upgrade framework utama.

### Tahap 3

Upgrade:

- Spring Boot Parent

Spring Boot mempengaruhi banyak dependency lain sehingga berpotensi menghasilkan breaking change.

### Risiko Breaking Change

Dependency yang paling berisiko untuk langsung diupgrade adalah:

#### Spring Boot

Karena:

- Mengubah banyak dependency transitif.
- Dapat mempengaruhi konfigurasi aplikasi.
- Dapat mengubah perilaku framework.

#### Jackson

Perubahan versi besar dapat mempengaruhi format serialisasi JSON yang digunakan client.

---

## Tugas 1.4 - Tantangan Lanjut

### Kasus

Setelah seluruh dependency diperbarui, tim QA menemukan bahwa response JSON kini memiliki field tambahan yang sebelumnya tidak muncul.

### Kemungkinan Penyebab

Library yang paling mungkin menjadi penyebab adalah:

#### Jackson Databind

Karena Jackson bertanggung jawab terhadap serialisasi dan deserialisasi objek JSON.

Perubahan versi dapat menyebabkan:

- Property baru ikut diserialisasi.
- Perubahan default configuration.
- Perubahan perilaku annotation.

### Cara Mengisolasi Penyebab

Langkah yang dapat dilakukan:

1. Membandingkan hasil serialisasi sebelum dan sesudah upgrade.
2. Menjalankan regression test khusus endpoint yang terdampak.
3. Mengupgrade dependency satu per satu.
4. Menggunakan branch terpisah untuk setiap upgrade.
5. Memanfaatkan Git bisect untuk menemukan perubahan yang menyebabkan bug.

Dengan pendekatan ini tim tidak perlu melakukan rollback seluruh dependency sekaligus.

---

## Kesimpulan

Dependency audit merupakan langkah penting dalam secure software delivery. Beberapa library yang digunakan pada project ini telah menggunakan versi lama yang memiliki riwayat kerentanan serius. Prioritas utama adalah memperbarui dependency dengan risiko tertinggi seperti Log4j dan Jackson, kemudian melakukan pengujian bertahap untuk meminimalkan risiko regresi pada aplikasi.
