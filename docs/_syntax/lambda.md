---
date: 2019-03-19
description: Lihat syntax baru Java 8 untuk membuat anonymous class iaitu lambda.
---

# Lambda

Java 8 memperkenalkan syntax baru untuk membuat *anonymous class* iaitu dengan
menggunakan simbol *lambda*. Simbol *lambda* dalam Java ialah `->`. Kita akan
menggunakan code daripada [Tutorial Ulang Kaji](ulang-kaji) untuk melihat cara
penggunaan *lambda*.

Dengan menggunakan IDE, kita boleh tukar code tersebut dengan lebih mudah.
Contohnya dengan menggunakan IntelliJ, letakkan cursor di *anonymous class*
tersebut, kemudian tekan `Alt+Enter`. Popup akan memaparkan pilihan untuk
menukarkan code ke *lambda*.

![Gambar popup tukar ke lambda]({{ site.baseurl }}/assets/img/replace_with_lambda.png)

Selepas kita klik pilihan tersebut, code akan bertukar ke bentuk yang
menggunakan *lambda*, seperti berikut,

![Gambar code lambda]({{ site.baseurl }}/assets/img/lambda.png)

Sekarang kita dapat melihat code menjadi semakin pendek. *Anonymous class*
tersebut hanya dalam satu baris sahaja.

## Penjelasan

Seperti yang kita dapat lihat dalam code di atas, code *lambda* adalah seperti
berikut,

```java
num -> num + 10
```

Maksudnya apa-apa di sebelah kiri *lambda* ialah input. Apa-apa di sebelah kanan
ialah cara untuk mendapatkan output. Cara ini sama seperti function dalam
matematik yang kita belajar di sekolah menggunakan arrow notation, seperti ini,

```
f:x -> x + 10
f(x) = x + 10
```

Penggunaan *lambda* di atas adalah untuk code yang mempunyai satu baris. Jika code kita memerlukan beberapa baris, kita boleh menulis begini,

```
num -> {
    int nomborDariDatabase = 10;
    return num + nomborDariDatabase;
}
```
