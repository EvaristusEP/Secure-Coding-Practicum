# Modul 2 - Order & Refund System

## Tujuan

Menerapkan State Machine dan Immutability pada sistem order dan refund.

---

## Permasalahan Awal

Masalah yang ditemukan:

- Status order menggunakan string bebas.
- Nilai amount dapat diubah kapan saja.
- Refund tidak memiliki validasi urutan status.

## Kode Awal

![Insecure Order](../../screenshots/sbd/modul2/insecure-order.png)

---

## Risiko Keamanan

Contoh kondisi tidak valid:

- Refund sebelum pembayaran.
- Order langsung menjadi COMPLETED.
- Nilai transaksi berubah setelah pembayaran.

---

## State Machine

![State Machine](../../screenshots/sbd/modul2/state-machine.png)

Status dibatasi menjadi:

```text
CREATED
→ PAID
→ SHIPPED
→ COMPLETED
```

Perubahan status hanya dapat dilakukan melalui transisi yang valid.

---

## Refund Validation

![Refund Validation](../../screenshots/sbd/modul2/refund-validation.png)

Refund hanya dapat dilakukan apabila syarat bisnis telah terpenuhi dan status sebelumnya valid.

---

## Kesimpulan

State Machine membantu menjaga integritas proses bisnis dan mencegah manipulasi status order.
