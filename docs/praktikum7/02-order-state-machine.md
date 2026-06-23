# 02. Secure State Machine

## Tujuan

Mengatasi masalah Boolean Explosion menggunakan State Machine.

## Enum State

![Enum State](../../screenshots/p7/02/order-state-enum.png)

## Transition Validation

![Transition Validation](../../screenshots/p7/02/transition-method.png)

## Success Case

![Success Case](../../screenshots/p7/02/success-case.png)

## Failure Case

![Failure Case](../../screenshots/p7/02/failure-case.png)

## Analisis

State Machine hanya mengizinkan perpindahan state yang valid. Pada pengujian, order berhasil berpindah dari CREATED ke PAID, SHIPPED, dan DELIVERED. Ketika mencoba langsung melakukan DELIVERED dari CREATED, sistem menolak operasi tersebut.

## Kesimpulan

State Machine mampu membatasi state yang valid sehingga lebih aman dan mudah dipelihara dibandingkan banyak variabel boolean.
