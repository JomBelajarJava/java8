---
date: 2019-03-20
description: Gunakan method reduce untuk agregat setiap data dalam collection.
---

# Reduce

Method `reduce()` boleh digunakan untuk membuat pengiraan yang melibatkan semua
nombor dalam collection.

Contoh jika kita ingin mendarab setiap nombor dalam list antara satu sama lain,
seperti ini,

```java
    List<Integer> numbers = Arrays.asList(1, 1, 2, 3, 5, 8, 13);

    int hasilDarabSetiapNombor = numbers.stream()
            .reduce(1, (x, y) -> x * y);

    System.out.println(hasilDarabSetiapNombor);
```

Method `reduce()` yang asas mempunyai dua parameter. Parameter yang pertama
ialah data pertama untuk pengiraan. Parameter yang kedua ialah BiFunction untuk
membuat pengiraan antara data yang terkumpul dengan data yang seterusnya.

Untuk code di atas, data pertama ialah nombor 1 kerana kita menggunakan operasi
darab. Jika 0, maka hasil terakhir akan tetap 0. BiFunction yang kita gunakan
akan darab nombor dengan nombor yang lain. Maka, code tersebut menjadi,

```java
1 * 1 * 2 * 3 * 5 * 8 * 13
```

Method `reduce()` sepatutnya menjadi pilihan terakhir untuk menjumlahkan
collection. Utamakan dahulu method-method yang sedia ada seperti `sum()` yang
kita lihat sebelum ini.
