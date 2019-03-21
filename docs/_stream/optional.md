---
date: 2019-03-21
description: Belajar Java 8, melihat class Optional untuk check sama ada variable ialah null ataupun tidak.
---

# Optional

Jika anda sudah biasa dengan Java, anda akan faham satu benda yang agak
menyusahkan ialah `null`. Kadang-kadang kita terlupa untuk check `null` dan
program akan terpadam dan mengeluarkan exception. Lalu, kita pun tambah code
untuk check `null`,

```java
Integer num = null;  // bayangkan dari database

if (num != null) {
    System.out.println(num);
}
```

Pattern di atas seperti tidak selari dengan penggunaan method stream seperti
`map`, `filter`, dan `reduce`. Jadi, Java 8 menyediakan class `Optional`. Dengan
menggunakan class `Optional`, kita boleh menukar code di atas ke,

```java
Integer num = null;  // bayangkan dari database

Optional.ofNullable(num)
        .ifPresent(System.out::println);
```

Class `Optional` juga mempunyai method `map` dan `filter` yang mempunyai fungsi
yang sama dengan method stream. Kita boleh menganggap `Optional` seperti data
structure untuk satu nilai. Dalam functional programming, konsep ini dinamakan
Monad.

Sebelum ini pun kita ada menggunakan optional, iaitu pada code,

```java
double taxAverage = taxes.stream()
        .mapToDouble(x -> x)
        .average()
        .orElse(0.0);
```

Code ini bermaksud jika tiada average, maka letakkan 0.0. Seperti code tersebut,
kita boleh menggunakan method `orElse` untuk mendapatkan data jika bukan `null`.
Jika `null`, gunakan nombor default yang diletakkan sebagai argument method
`orElse`.

Satu lagi method untuk mengambil data ialah `get()`, tetapi sebaik-baiknya elak
dari menggunakan method tersebut. Guna sahaja method `orElse` supaya code lebih
selamat.
