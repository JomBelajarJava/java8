---
date: 2019-03-23
title: CompletableFuture
description: Belajar Java 8, lihat penggunaan CompletableFuture untuk membuat code concurrency.
---

# CompletableFuture

`CompletableFuture` ialah class yang disediakan untuk menulis code concurrency
menggunakan cara penulisan yang sama dengan syntax Java 8. Cara ini sama seperti
future dan promise yang digunakan dalam JavaScript.

Untuk membuat `CompletableFuture`, kita boleh menggunakan method
`supplyAsync()`, seperti berikut,

```java
CompletableFuture<Integer> numberFuture = CompletableFuture.supplyAsync(() -> 100);
```

Untuk lebih ringkas, kita boleh menggunakan `import static`,

```java
import static java.util.concurrent.CompletableFuture.supplyAsync;
```

Kemudian tukar ke,

```java
CompletableFuture<Integer> numberFuture = supplyAsync(() -> 100);
```

Code di dalam *lambda* tersebut akan proses dalam thread yang tersendiri, sama
seperti menggunakan class `Thread`.

Jika anda masih ingat, apabila kita menggunakan syntax Java 8, kita tidak perlu
menukar mana-mana variable. Kita hanya perlu mengambil tahu output yang return
daripada sesebuah code. Code di atas akan return nombor 100. Untuk menggunakan
nombor tersebut, kita boleh menggunakan salah satu daripada method `then` dalam
`CompletableFuture`. Contoh jika kita mahu print nombor tersebut, kita boleh
menggunakan method `thenAcceptAsync`, seperti berikut,

```java
numberFuture
        .thenAcceptAsync(x -> System.out.println("Log nombor: " + x));
```

Kita juga boleh menggunakan beberapa method `then` untuk future yang sama.
Contohnya,

```java
CompletableFuture<Integer> numberFuture = supplyAsync(() -> 100);

numberFuture
        .thenAcceptAsync(x -> System.out.println("Log nombor: " + x));

numberFuture
        .thenAcceptAsync(x -> System.out.println("Nombor dari database: " + x));
```

Apa yang berlaku ialah program akan menjalankan kedua-dua code tersebut secara
serentak selepas mendapat output daripada future berkenaan. Jika kita run
program ini berulang kali, kita boleh lihat ada masa program akan print tidak
mengikut urutan.

Jika anda run code tersebut, kadang-kadang program tidak akan mengeluarkan
output kerana program kita sudah berakhir semasa code-code tersebut sedang
berjalan di thread yang lain. Kita boleh membuatkan code menunggu dengan
menambah code berikut di penghujung,

```java
try {
    Thread.sleep(300);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```
