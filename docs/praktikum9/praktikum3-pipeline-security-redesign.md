# Praktikum 3 - Pipeline Security Redesign

## Tujuan

Melakukan audit terhadap pipeline CI/CD yang ada, mengidentifikasi kelemahan keamanan, dan mendesain pipeline yang lebih aman untuk proses deployment aplikasi.

---

## Pipeline Awal

![Original Pipeline](../../screenshots/p9/p3/original-pipeline.png)

Pipeline awal melakukan build dan deploy langsung ke production tanpa tahapan keamanan yang memadai.

---

# Tugas 3.1 - Audit Pipeline

## Temuan Keamanan

### 1. Deploy Langsung ke Production

Pipeline melakukan deployment langsung ke production setelah proses build selesai.

Risiko:

- Bug langsung masuk ke production.
- Tidak ada lingkungan staging untuk validasi.

---

### 2. Tidak Ada Secret Scanning

Pipeline tidak memeriksa apakah repository mengandung credential atau secret.

Risiko:

- Password dan API key dapat ikut terdeploy.

---

### 3. Tidak Ada SAST

Source code tidak diperiksa menggunakan Static Application Security Testing.

Risiko:

- Vulnerability masuk ke production tanpa terdeteksi.

---

### 4. Tidak Ada Dependency Scan

Dependency yang digunakan tidak diperiksa terhadap CVE.

Risiko:

- Library rentan tetap digunakan.

---

### 5. Build Menggunakan Skip Tests

```bash
mvn package -DskipTests
```

Risiko:

- Error aplikasi tidak terdeteksi sebelum deploy.

---

### 6. Tidak Ada Artifact Verification

Artifact hasil build tidak memiliki checksum atau tanda tangan digital.

Risiko:

- Sulit memastikan integritas file.

---

### 7. Tidak Ada Approval Manual

Deployment dapat berjalan otomatis tanpa persetujuan manusia.

Risiko:

- Kesalahan konfigurasi langsung mempengaruhi production.

---

### 8. Tidak Ada DAST

Tidak ada pengujian keamanan terhadap aplikasi yang sedang berjalan.

Risiko:

- Vulnerability runtime tidak terdeteksi.

---

### 9. Tidak Ada Environment Separation

Pipeline hanya mengenal production.

Risiko:

- Sulit melakukan validasi sebelum release.

---

## Kesimpulan Audit

Pipeline awal lebih berfokus pada kecepatan deployment dibandingkan keamanan sehingga berpotensi menimbulkan risiko tinggi pada sistem produksi.

---

# Tugas 3.2 - Desain Pipeline Baru

## Arsitektur Pipeline

![Secure Pipeline](../../screenshots/p9/p3/secure-pipeline-yaml.png)

Tahapan yang digunakan:

1. Secret Scan
2. SAST
3. Build & Unit Test
4. Dependency Scan
5. Package & Sign
6. Deploy Staging
7. DAST
8. Manual Approval
9. Deploy Production

---

## Contoh Secure Pipeline

```yaml
name: Secure Deploy

on:
  push:
    branches:
      - main

jobs:

  secret-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

  sast:
    needs: secret-scan
    runs-on: ubuntu-latest

  build-test:
    needs: sast
    runs-on: ubuntu-latest

  dependency-scan:
    needs: build-test
    runs-on: ubuntu-latest

  package-sign:
    needs: dependency-scan
    runs-on: ubuntu-latest

  deploy-staging:
    needs: package-sign
    runs-on: ubuntu-latest

  dast:
    needs: deploy-staging
    runs-on: ubuntu-latest

  approval:
    needs: dast
    runs-on: ubuntu-latest

  deploy-production:
    needs: approval
    runs-on: ubuntu-latest
```

Pipeline di atas menerapkan prinsip secure software delivery dengan beberapa lapisan validasi sebelum deployment ke production.

---

# Tugas 3.3 - Fail Fast vs Fail Safe

## Fail Fast

Prinsip Fail Fast menghentikan pipeline segera setelah ditemukan masalah.

Contoh:

- Secret ditemukan.
- Dependency Critical ditemukan.
- Unit Test gagal.

Keuntungan:

- Risiko tidak menyebar lebih jauh.
- Masalah ditemukan lebih cepat.

---

## Fail Safe

Prinsip Fail Safe mengizinkan proses tetap berjalan ketika tool pendukung mengalami masalah.

Contoh:

- Server scanner sedang maintenance.
- Tool timeout sementara.

Keuntungan:

- Delivery tidak selalu terblokir karena masalah tool.

---

## Implementasi pada Pipeline

Pipeline ini menggunakan:

### Fail Fast

Pada:

- Secret Scan
- SAST
- Dependency Scan
- Unit Test

Karena temuan pada tahap tersebut merupakan blocker.

### Fail Safe

Pada:

- Monitoring tambahan
- Reporting tambahan

Karena kegagalan tool tidak boleh selalu menghentikan seluruh proses bisnis.

---

# Tugas 3.4 - The Broken Window

## Skenario

Dependency Scan menemukan CVE baru dengan:

- CVSS 7.5 (High)
- Fix tersedia
- Digunakan pada 23 endpoint
- Besok ada demo investor

---

## Risiko Teknis

Jika vulnerability dibiarkan:

- Potensi eksploitasi meningkat.
- Dependency tetap digunakan pada seluruh endpoint terkait.
- Technical debt keamanan bertambah.

---

## Risiko Non Teknis

- Kehilangan kepercayaan investor.
- Pelanggaran kepatuhan keamanan.
- Kerusakan reputasi perusahaan.
- Potensi konsekuensi hukum apabila terjadi insiden.

---

## Cara Menyampaikan Keberatan

Sebagai engineer, keberatan harus disampaikan secara profesional.

Contoh:

> Berdasarkan hasil dependency scan, terdapat vulnerability High dengan patch yang sudah tersedia. Saya menyarankan mitigasi atau upgrade sebelum production karena risiko eksploitasi cukup signifikan.

Pendekatan ini fokus pada fakta dan risiko, bukan opini pribadi.

---

## Jika Keputusan Tetap Diambil

Hal yang perlu didokumentasikan:

- Hasil scan.
- CVSS score.
- Dependency yang terdampak.
- Tanggal keputusan.
- Penanggung jawab keputusan.

Dokumentasi penting untuk audit dan pelacakan risiko.

---

## Alternatif Solusi

Alternatif yang lebih aman:

1. Upgrade patch sebelum demo.
2. Deploy ke staging terlebih dahulu.
3. Terapkan compensating control.
4. Batasi akses endpoint terdampak.
5. Jadwalkan patch segera setelah demo.

Dengan pendekatan tersebut kebutuhan bisnis dan keamanan tetap dapat diakomodasi.

---

# Kesimpulan

Pipeline modern tidak hanya bertanggung jawab melakukan deployment, tetapi juga menjadi kontrol keamanan otomatis. Dengan menerapkan secret scan, SAST, dependency scan, DAST, approval manual, dan staging environment, risiko keamanan dapat dikurangi secara signifikan sebelum aplikasi mencapai production.
