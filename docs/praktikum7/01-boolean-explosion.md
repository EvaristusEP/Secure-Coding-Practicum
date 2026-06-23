# 01. Boolean Explosion

## Tujuan

Memahami masalah Boolean Explosion akibat penggunaan banyak variabel boolean untuk merepresentasikan state bisnis.

## Struktur Order

![Order Booleans](../../screenshots/p7/01/order-booleans.png)

## Validation Rules

![Validation Rules](../../screenshots/p7/01/validation-rules.png)

## Hasil Eksekusi

![State Count](../../screenshots/p7/01/state-count.png)

## Success dan Failure Case

![Success Failure](../../screenshots/p7/01/success-failure-case.png)

## Analisis

Program menghasilkan 64 kemungkinan state dari 6 variabel boolean. Namun hanya 11 state yang valid dan 53 state lainnya tidak valid. Kondisi ini menunjukkan bahwa semakin banyak boolean yang digunakan, semakin besar kemungkinan muncul kombinasi state yang tidak masuk akal.

## Kesimpulan

Boolean Explosion menyebabkan kompleksitas sistem meningkat dan berpotensi menimbulkan bug. Pendekatan yang lebih baik adalah menggunakan State Machine.
