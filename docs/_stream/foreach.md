---
date: 2019-03-20
description: Bukan semua stream perlu menghasilkan nilai yang baru, jadi kita boleh menggunakan method foreach.
---

# Foreach

Bukan semua proses dalam programming memerlukan output, contoh mungkin jika kita
ingin memaparkan setiap nilai pada console, atau output ke file. Untuk kes ini,
kita boleh menggunakan method `forEach()`.

Contoh jika kita ingin memaparkan nombor genap yang telah ditapis pada console,

```java
    List<Integer> numbers = Arrays.asList(1, 1, 2, 3, 5, 8, 13);

    numbers.stream()
            .filter(num -> num % 2 == 0)
            .forEach(System.out::println);
```

Kita boleh menggunakan *method reference* untuk print ke console. Perhatikan
kita tidak perlu meletakkan variable untuk stream tersebut.
