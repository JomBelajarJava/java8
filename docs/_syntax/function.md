---
date: 2019-03-19
description: Gunakan class generic Function yang sedia ada dalam Java 8 untuk menggunakan lambda dan method reference.
---

# Function

Dalam [Tutorial Ulang Kaji](/ulang-kaji), kita membina class Function kita
sendiri. Dalam Java 8, kita tidak perlu membuat sedemikian kerana class Function
sudah sedia ada.

[Klik sini untuk melihat documentation class Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)

Seperti yang tertera di documentation tersebut, class Function menggunakan
generics, jadi kita hanya perlu beritahu jenis data yang kita mahu gunakan untuk
input dan output.

Oleh itu, kita boleh delete class Function yang kita bina. Kemudian update
method `kemudian()` seperti berikut,

```java
    public int kemudian(Function<Integer, Integer> func) {
        return func.apply(num);
    }
```

Anda mungkin perlu import class tersebut ataupun dengan menggunakan IDE,

```java
import java.util.function.Function;
```

Code yang lain kekal sama, dan anda boleh test untuk melihat program masih
berfungsi sama seperti sebelum ini.
