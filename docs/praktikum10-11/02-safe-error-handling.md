# Praktikum 2 - Safe Error Handling

## Tujuan

Memahami risiko Information Disclosure akibat error handling yang tidak aman.

---

## Source Code Awal

![Source Code](../../screenshots/p10/p2/source-code.png)

Endpoint menampilkan:

* Pesan error lengkap
* Stack trace
* Konfigurasi database

---

## Informasi Sensitif yang Bocor

Dari kode awal, attacker dapat memperoleh:

* Nama database
* Username database
* Struktur direktori aplikasi
* Framework yang digunakan
* Stack trace lengkap
* Konfigurasi internal

---

## Risiko Keamanan

Informasi tersebut dapat digunakan untuk:

* Reconnaissance
* Menyusun serangan yang lebih spesifik
* Mengetahui struktur sistem internal
* Membantu eksploitasi vulnerability lain

---

## Refactor Aman

```php
return response()->json([
    'error' => 'Internal server error',
    'reference' => 'ERR-001'
], 500);
```

---

## Praktik yang Direkomendasikan

User hanya menerima pesan generik.

Detail error disimpan pada:

* Laravel Log
* Sentry
* SIEM
* Monitoring Platform

---

## Kesimpulan

Error detail tidak boleh dikirim ke client production karena dapat menyebabkan information disclosure.
