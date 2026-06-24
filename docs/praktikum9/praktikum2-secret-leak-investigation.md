# Praktikum 2 - Secret Leak Investigation & Remediation

## Tujuan

Melakukan investigasi kebocoran secret pada repository Git, menganalisis dampaknya, serta menentukan langkah remediasi yang tepat untuk mengurangi risiko keamanan.

---

## Setup Repository

### Riwayat Commit

![Git History](../../screenshots/p9/p2/git-history.png)

Repository sengaja dibuat memiliki beberapa commit yang mengandung secret untuk mensimulasikan kasus kebocoran kredensial pada lingkungan produksi.

---

## Tugas 2.1 - Forensik dengan Gitleaks

### Menjalankan Gitleaks

![Gitleaks Command](../../screenshots/p9/p2/gitleaks-command.png)

Tool Gitleaks digunakan untuk mendeteksi secret yang tersimpan pada source code maupun riwayat commit Git.

### Hasil Pemindaian

![Gitleaks Result](../../screenshots/p9/p2/gitleaks-result.png)

### Analisis

#### Berapa Total Secret yang Ditemukan?

Berdasarkan isi repository, secret yang kemungkinan terdeteksi meliputi:

- Database Password
- JWT Secret
- AWS Access Key
- AWS Secret Key
- Stripe Live API Key
- Redis Password
- Bearer Token

Total secret yang ditemukan diperkirakan lebih dari satu karena beberapa secret muncul kembali pada commit yang berbeda.

#### Commit Mana Saja yang Mengandung Secret?

##### Commit 1

Mengandung:

- Database Password
- JWT Secret
- AWS Access Key
- AWS Secret Key
- Stripe API Key

##### Commit 3

Mengandung:

- Fallback Bearer Token

##### Commit 4

Mengandung:

- Database Password
- JWT Secret
- Redis Password

#### Apakah Commit Kedua Menghilangkan Secret?

Tidak.

Commit kedua hanya menghapus secret dari versi file terbaru (HEAD), tetapi secret tetap tersimpan di seluruh riwayat Git sebelumnya.

Selama history masih ada, attacker tetap dapat mengakses secret menggunakan:

```bash
git log
git show
git checkout
```

atau melalui tool forensik seperti Gitleaks.

---

## Tugas 2.2 - Klasifikasi Risiko

| Secret | Jenis | Jika Dieksploitasi | Urgensi Rotasi |
|----------|----------|----------|----------|
| DB_PASSWORD | Database Credential | Attacker dapat mengakses database transaksi dan data pelanggan | Sangat Tinggi |
| JWT_SECRET | Signing Key | Attacker dapat membuat token JWT palsu dan login sebagai user lain | Sangat Tinggi |
| AWS Access Key | Cloud Credential | Attacker dapat mengakses resource cloud perusahaan | Sangat Tinggi |
| AWS Secret Key | Cloud Credential | Dapat digunakan bersama access key untuk mengambil alih akun cloud | Sangat Tinggi |
| Stripe API Key | Payment Credential | Attacker dapat melakukan operasi terhadap sistem pembayaran | Sangat Tinggi |
| Redis Password | Cache Credential | Attacker dapat membaca atau memodifikasi data cache | Tinggi |
| Bearer Token | Service Token | Attacker dapat mengakses service internal tanpa autentikasi tambahan | Tinggi |

### Dampak Terburuk

Pada sistem fintech, kombinasi beberapa secret tersebut dapat menyebabkan:

- Kebocoran data pelanggan.
- Pemalsuan transaksi.
- Pengambilalihan akun pengguna.
- Penyalahgunaan layanan cloud.
- Gangguan operasional sistem pembayaran.

---

## Tugas 2.3 - Remediasi yang Benar

### Rewrite History

![Rewrite History](../../screenshots/p9/p2/rewrite-history-command.png)

Repository dapat dibersihkan menggunakan:

- git filter-branch
- BFG Repo Cleaner

### Apakah Secret Langsung Hilang?

Tidak selalu.

Meskipun history telah dibersihkan, secret mungkin masih:

- Tersimpan pada clone developer lain.
- Tersimpan pada backup repository.
- Tersimpan pada cache Git hosting.
- Sudah terlanjur diakses pihak lain.

### Risiko Rewrite History

Jika repository sudah digunakan banyak developer:

- Semua developer harus melakukan re-clone atau reset repository.
- Potensi konflik branch meningkat.
- Commit hash berubah.

Karena itu rewrite history harus direncanakan dengan hati-hati.

### Mengapa Rotasi Tetap Wajib?

Karena tidak ada jaminan bahwa secret belum pernah dilihat atau dicuri.

Prinsip keamanan yang digunakan adalah:

> Anggap semua secret yang pernah bocor sudah diketahui attacker.

Oleh karena itu seluruh secret harus diganti meskipun history sudah dibersihkan.

---

## Tugas 2.4 - Pre-commit Hook

### Implementasi Hook

![Pre Commit Hook](../../screenshots/p9/p2/precommit-hook.png)

Pre-commit hook digunakan untuk mencegah developer melakukan commit yang mengandung secret.

---

### Upaya Bypass

| Metode | Berhasil | Penjelasan |
|----------|----------|----------|
| git commit --no-verify | Ya | Melewati seluruh hook lokal |
| Menghapus file hook | Ya | Hook hanya berada pada mesin developer |
| Commit melalui tool lain | Ya | Beberapa tool dapat mengabaikan hook lokal |

---

### Mengapa Hook Bisa Dilewati?

Karena pre-commit hook merupakan mekanisme client-side.

Artinya kontrol berada pada komputer developer dan tidak dipaksa oleh server Git.

---

### Cara Memperkuat

Beberapa langkah yang dapat dilakukan:

1. Menambahkan secret scan pada CI/CD.
2. Menjalankan Gitleaks pada pipeline.
3. Menambahkan branch protection.
4. Melakukan code review wajib.
5. Menggunakan repository ruleset.

---

### Implikasi Keamanan Tim

Pre-commit hook tidak boleh menjadi satu-satunya mekanisme perlindungan.

Jika seorang developer menonaktifkan hook, secret masih dapat masuk ke repository.

Karena itu diperlukan kontrol tambahan pada sisi server dan pipeline CI/CD.

---

## Kesimpulan

Memindahkan secret ke environment variable tidak otomatis menyelesaikan masalah apabila secret pernah tersimpan di repository Git. History repository harus dibersihkan, seluruh secret harus dirotasi, dan organisasi perlu menerapkan kontrol berlapis seperti Gitleaks, pre-commit hook, serta secret scanning pada CI/CD untuk mencegah kejadian serupa di masa depan.
