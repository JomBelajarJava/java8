---
date: 2019-03-19
description: Gunakan interface generic Function yang sedia ada dalam Java 8 untuk menggunakan lambda dan method reference.
---

# Function

Dalam [Tutorial Ulang Kaji](/ulang-kaji), kita membina interface Function kita
sendiri. Dalam Java 8, kita tidak perlu membuat sedemikian kerana interface
Function sudah sedia ada.

[Klik sini untuk melihat documentation interface Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)

Seperti yang tertera di documentation tersebut, interface Function menggunakan
generics, jadi kita hanya perlu beritahu jenis data yang kita mahu gunakan untuk
input dan output.

Oleh itu, kita boleh delete interface Function yang kita bina. Kemudian update
method `kemudian()` seperti berikut,

```java
    public int kemudian(Function<Integer, Integer> func) {
        return func.apply(num);
    }
```

Anda mungkin perlu import interface tersebut ataupun dengan menggunakan IDE,

```java
import java.util.function.Function;
```

Code yang lain kekal sama, dan anda boleh test untuk melihat program masih
berfungsi sama seperti sebelum ini.
