# Tugas Akhir - Security Delivery Checklist

## Tujuan

Dokumen ini digunakan sebagai checklist keamanan sebelum melakukan release ke production. Checklist dirancang agar dapat digunakan oleh developer dalam waktu singkat namun tetap memberikan perlindungan yang memadai terhadap risiko keamanan.

---

# Kategori Release

## Hotfix Darurat

Contoh:

- Perbaikan bug production
- Service down
- Patch keamanan kritis

Fokus:

- Memastikan perbaikan aman
- Meminimalkan downtime
- Memastikan tidak muncul masalah baru

---

## Release Fitur Besar

Contoh:

- Fitur pembayaran baru
- Integrasi pihak ketiga
- Perubahan arsitektur

Fokus:

- Pengujian menyeluruh
- Validasi keamanan
- Analisis dampak bisnis

---

# BLOCKER

Release TIDAK BOLEH dilakukan apabila salah satu item berikut belum terpenuhi.

## Source Code

- [ ] Tidak ada secret hardcoded
- [ ] Tidak ada credential pada repository
- [ ] Code review selesai
- [ ] Branch protection aktif

### Bukti

- Screenshot Gitleaks
- Pull Request Review

---

## Dependency Security

- [ ] Tidak ada CVE Critical
- [ ] Tidak ada CVE High tanpa mitigasi
- [ ] Dependency Scan berhasil

### Bukti

- OWASP Dependency Check Report

---

## Testing

- [ ] Unit Test berhasil
- [ ] Integration Test berhasil
- [ ] Regression Test berhasil

### Bukti

- CI/CD Pipeline Result

---

## Build Integrity

- [ ] Build berhasil
- [ ] Artifact memiliki checksum
- [ ] Artifact berasal dari pipeline resmi

### Bukti

- Build Log
- SHA256 Checksum

---

## Deployment

- [ ] Deploy ke staging berhasil
- [ ] Smoke test berhasil
- [ ] DAST selesai

### Bukti

- Staging Report
- OWASP ZAP Report

---

## Approval

- [ ] Technical Lead approval
- [ ] Product Owner approval

### Bukti

- Approval pada GitHub Actions
- Approval pada Change Request

---

# WARNING

Release masih dapat dilakukan tetapi risiko harus dicatat.

## Monitoring

- [ ] Alert sudah dikonfigurasi
- [ ] Dashboard monitoring aktif

---

## Dokumentasi

- [ ] Release note dibuat
- [ ] Known issue didokumentasikan

---

## Dependency

- [ ] Ada CVE Medium yang belum diperbaiki
- [ ] Mitigasi sementara sudah tersedia

---

## Infrastruktur

- [ ] Backup terbaru tersedia
- [ ] Rollback plan tersedia

---

# Security Verification Summary

| Area | Status | Catatan |
|--------|--------|--------|
| Secret Scan | ☐ Pass | |
| SAST | ☐ Pass | |
| Unit Test | ☐ Pass | |
| Dependency Scan | ☐ Pass | |
| DAST | ☐ Pass | |
| Approval | ☐ Pass | |
| Production Deploy | ☐ Pass | |

---

# Artefak Pendukung

| Item | Bukti |
|--------|--------|
| Secret Scan | Screenshot / Report |
| SAST | Screenshot / Report |
| Dependency Scan | Screenshot / Report |
| Unit Test | Screenshot / Report |
| DAST | Screenshot / Report |
| Approval | Screenshot |
| Deployment | Screenshot / Log |

---

# Emergency Release Procedure

Jika terjadi insiden kritis:

1. Hotfix dibuat pada branch khusus.
2. Minimal dilakukan code review oleh satu engineer lain.
3. Secret scan tetap wajib dijalankan.
4. Unit test minimal harus lulus.
5. Deployment dilakukan ke staging terlebih dahulu jika memungkinkan.
6. Dokumentasikan seluruh keputusan dan risiko yang diterima.

---

# Final Sign-Off

| Role | Nama | Tanggal | Status |
|--------|--------|--------|--------|
| Developer | | | |
| Reviewer | | | |
| Technical Lead | | | |
| Product Owner | | | |

---

# Kesimpulan

Checklist ini digunakan untuk memastikan bahwa setiap release yang masuk ke production telah melalui proses validasi keamanan minimum. Dengan memisahkan kategori Blocker dan Warning, tim dapat mengambil keputusan yang lebih terukur tanpa mengorbankan keamanan maupun kebutuhan bisnis.
