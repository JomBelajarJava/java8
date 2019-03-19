---
date: 2019-03-19
description: Lihat interface dalam package java.util.function yang lain yang boleh digunakan untuk lambda.
---

# Functional Interfaces

Selain interface Function, ada beberapa lagi interface lain yang boleh digunakan
yang ada dalam package java.util.function.

[Klik sini untuk melihat documentation untuk package java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

## BiFunction

BiFunction adalah sama seperti Function tetapi boleh mengambil dua input. Contoh
menggunakan *lambda*,

```java
(x, y) -> x + y + 10
```

Jangan lupa kurungan untuk input.

## Consumer

Consumer ialah interface untuk function yang mengambil input dan tidak return
apa-apa nilai (void). BiConsumer ialah jenis yang mengambil dua input.

## Supplier

Supplier ialah interface untuk function yang tiada input, hanya return nilai.
Contoh menggunakan *lambda*,

```java
() -> 10
```

----

Kita tidak perlu hafal jenis-jenis interface di atas kerana kita hanya
menggunakan *lambda*. Cuma perlu tahu function yang kita akan tulis boleh
mempunyai ciri-ciri di atas.

Jika anda perasan, kita hanya boleh meletak sehingga dua input untuk function
sahaja. Jika kita mahu lebih, kita perlu menulis sendiri interface yang kita
mahu ataupun menggunakan library seperti [Vavr](https://www.vavr.io).
