# Praktikum 1 - Dependency Risk Analysis

## Tujuan

Melakukan audit dependency pada aplikasi Spring Boot untuk mengidentifikasi kerentanan yang dapat berdampak terhadap keamanan sistem fintech.

## Dependency Awal

![Dependencies](../../screenshots/p9/p1/pom-dependencies.png)

Dependency yang digunakan berasal dari project lama dan belum pernah diaudit sebelumnya.

## Menjalankan OWASP Dependency Check

![Dependency Check Command](../../screenshots/p9/p1/dependency-check-command.png)

Tool OWASP Dependency Check digunakan untuk memetakan library yang digunakan terhadap database CVE yang diketahui.

## Ringkasan Hasil Pemindaian

![Summary](../../screenshots/p9/p1/dependency-check-report-summary.png)

## Detail Kerentanan

![CVE Detail](../../screenshots/p9/p1/dependency-cve-detail.png)

## Analisis

Beberapa dependency menggunakan versi lama yang telah diketahui memiliki kerentanan keamanan. Risiko terbesar berasal dari library yang dapat menyebabkan Remote Code Execution, deserialisasi tidak aman, ataupun kebocoran data sensitif.

## Kesimpulan

Dependency audit penting dilakukan secara berkala karena kerentanan baru dapat ditemukan pada library yang sudah lama digunakan meskipun kode aplikasi tidak berubah.
