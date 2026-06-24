# Praktikum 3 - Retry Storm pada Laravel Queue

## Tujuan

Memahami bagaimana retry yang berlebihan dapat menyebabkan overload pada sistem.

---

## Source Code Awal

![Original Job](../../screenshots/p10/p3/original-job.png)

Konfigurasi:

```php
public $tries = 10;
```

---

## Analisis

Ketika payment gateway mengalami timeout:

* Job gagal
* Queue melakukan retry
* Retry dilakukan berulang

Jika banyak job mengalami kegagalan yang sama, sistem dapat mengalami Retry Storm.

---

## Risiko

* Queue penuh
* Resource server habis
* Payment gateway semakin terbebani
* Recovery menjadi lebih lambat

---

## Refactor

```php
public $tries = 3;

public function backoff(): array
{
    return [10, 30, 60];
}
```

---

## Keuntungan Backoff

Retry tidak dilakukan secara langsung.

Interval:

* Retry 1 → 10 detik
* Retry 2 → 30 detik
* Retry 3 → 60 detik

---

## Analisis Diskusi

### tries = 10

Kelebihan:

* Peluang sukses lebih besar

Kekurangan:

* Beban sistem meningkat

### tries = 3

Kelebihan:

* Lebih terkendali

Kekurangan:

* Peluang recovery sedikit berkurang

---

## Kapan Retry Diperbolehkan?

* Timeout sementara
* Gangguan jaringan sementara
* Service overload sementara

---

## Kapan Retry Berbahaya?

* Credential salah
* Resource tidak tersedia permanen
* Error konfigurasi

---

## Kesimpulan

Retry harus dibatasi dan dikombinasikan dengan backoff untuk mencegah Retry Storm.
