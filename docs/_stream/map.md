---
date: 2019-03-20
description: Lihat penggunaan map dari stream dengan lebih mendalam.
---

# Map

Kita sudah lihat method `.map()` sebelum ini, iaitu,

```java
    List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
    List<Integer> selepasTambahSepuluh = numbers.stream()
            .map(num -> num + 10)
            .collect(toList());

    System.out.println(selepasTambahSepuluh);
```

Apa yang berlaku ialah method `.map()` akan menjalankan code *lambda* untuk
setiap data dalam list `numbers` untuk menghasilkan list yang baru. Dalam code
di atas, setiap nombor akan ditambah dengan sepuluh.

Disebabkan method-method dalam Stream berangkai, method `map()` akan return
Stream semula. Untuk menjadikan hasil stream tersebut ke list, kita boleh
menggunakan method `collect()` seperti code di atas.

## Map ke jenis data yang lebih khusus/spesifik

Method `map()` yang kita gunakan di atas akan menghasilkan data mengikut jenis
data yang return daripada *lambda*. Kadang kala kita mungkin perlu membuat
pengiraan yang lebih spesifik mengikut jenis data. Contohnya jika kita ingin
menjumlahkan semua data dalam list, kita boleh menggunakan method `sum()`.

Method `sum()` adalah daripada `IntStream`. Untuk mendapatkan `IntStream`, kita
boleh menggunakan method `mapToInt()`. Contoh code adalah seperti berikut,

```java
    List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
    int jumlah = numbers.stream()
            .mapToInt(num -> num + 10)
            .sum();

    System.out.println(jumlah);
```

Selain `mapToInt()`, ada juga `mapToDouble()`, `mapToLong()` untuk jenis data
yang lain. Anda boleh rujuk documentation.

## Kesilapan biasa

Kesilapan yang biasa dilakukan semasa menggunakan method `map()` ialah
penggunaan method tersebut secara berturutan. Contohnya,

```java
    List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
    List<Integer> updatedNumbers = numbers.stream()
            .map(num -> num + 10)
            .map(num -> num + 5)
            .collect(toList());

    System.out.println(updatedNumbers);
```

Sebaliknya, letakkan pengiraan dalam satu function, seperti ini,

```java
            .map(num -> num + 10 + 5)
```

atau menggunakan method `andThen()` atau `compose()` untuk menggabungkan
function, seperti ini,

```java
    Function<Integer, Integer> tambahSepuluh = num -> num + 10;
    Function<Integer, Integer> tambahLima = num -> num + 5;

    List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
    List<Integer> updatedNumbers = numbers.stream()
            .map(tambahSepuluh.andThen(tambahLima))
            .collect(toList());
```

Jika kita menggunakan `map()` secara berturutan, list yang terhasil selepas
`map()` yang pertama akan kekal dalam memory sehinggalah proses selesai. Kurang
efisyen.
