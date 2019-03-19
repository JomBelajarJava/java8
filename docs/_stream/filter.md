---
date: 2019-03-20
description: Menggunakan method filter untuk menapis data ke collection yang baru.
---

# Filter

Method `filter()` boleh digunakan untuk menapis data yang kita mahu daripada
collection untuk membentuk collection yang baru.

Contoh untuk mengambil nombor genap,

```java
    List<Integer> numbers = Arrays.asList(1, 1, 2, 3, 5, 8, 13);

    List<Integer> nomborGenap = numbers.stream()
            .filter(num -> num % 2 == 0)
            .collect(toList());

    System.out.println(nomborGenap);
```

Method `filter()` mengambil function sebagai argument. Jika function tersebut
memberi boolean `true` untuk satu data, data tersebut akan diambil ke collection
baru.

Dalam code di atas, jika baki nombor selepas dibahagi dengan 2 ialah 0, bermakna
nombor itu ialah nombor genap, maka nombor tersebut akan termasuk dalam list
yang baru.
