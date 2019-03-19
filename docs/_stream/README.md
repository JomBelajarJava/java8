---
date: 2019-03-20
permalink: /:collection/
title: Stream
description: Memperkenalkan Stream, interface Java 8 menggantikan penggunaan loop untuk collection.
---

# Stream

Sekarang kita sudah melihat bagaimana *lambda* dan *method reference* boleh
meringkaskan *anonymous class* tetapi bagaimana syntax tersebut boleh membantu
mengurangkan bilangan code kita sedangkan kita jarang menggunakan *anonymous
class*? Jawapannya ialah Stream.

Dalam aplikasi bisnes yang tipikal, proses yang dijalankan kebanyakannya sama
sahaja. Cari senarai data, jumlahkan data. Cari senarai data, update setiap
data. Cari senarai data, tapis data. Disebabkan proses-proses tersebut selalu
digunakan, takkanlah kita perlu menulis loop selalu? Jadi, Java memperkenalkan
Stream.

Dalam Java 8, setiap collection telah implement interface Stream. Untuk
menggunakan Stream, kita boleh memanggil method `.stream()` daripada collection
tersebut. Contohnya,

```java
List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
Stream<Integer> stream = numbers.stream();
```

Tetapi kita jarang menggunakan seperti di atas kerana method-method dalam
interface Stream boleh berangkai. Biasanya kita akan menulis proses-proses
tersebut secara berangkai.

Ini contoh untuk menambah 10 untuk setiap nombor dalam `numbers`,

```java
import java.util.Arrays;
import java.util.List;

import static java.util.stream.Collectors.toList;

public class Application {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(25, 100, 50, 10, 8);
        List<Integer> selepasTambahSepuluh = numbers.stream()
                .map(x -> x + 10)
                .collect(toList());

        System.out.println(selepasTambahSepuluh);
    }
}
```

Saya menggunakan import static di atas untuk mengurangkan code di dalam main.

Perhatikan code di atas, kita ada menggunakan *lambda* iaitu,

```java
x -> x + 10
```

dalam method `.map()`. Stream mengandungi method-method yang akan menjalankan
code *lambda* untuk setiap data dalam collection. Jadi, kita tidak perlu lagi
menulis loop.

Dalam bab ini, kita akan melihat method-method yang selalu digunakan seperti
`map()` kemudian kita akan melihat bagaimana method-method tersebut boleh
digunakan bersama untuk menyelesaikan sesuatu masalah.
