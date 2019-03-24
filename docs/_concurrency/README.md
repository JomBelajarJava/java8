---
date: 2019-03-22
permalink: /:collection/
title: Concurrency
description: Pengenalan kepada concurrency. Melihat contoh racing condition. Juga melihat parallelStream dalam Java 8.
---

# Concurrency

Dalam Java 8, selain lambda dan stream, ada satu lagi penambahbaikan yang jarang
orang sebut, iaitu kemudahan membuat concurrency. Concurrency dalam Java 8
menggunakan konsep future dan promise, iaitu konsep yang berasal daripada
functional programming. Jadi, kita boleh memanfaatkan konsep lambda dan stream
yang kita belajar dalam bab-bab sebelum ini.

## Apa itu concurrency?

Concurrency ialah proses untuk menjalankan code tanpa mengikut urutan tetapi
memberi output yang sama. Zaman sekarang dengan wujudnya CPU yang mempunyai
lebih daripada satu core, program boleh menjalankan code secara serentak
sekaligus meningkatkan kelajuan program kita.

Masalah dalam concurrency bukannya bagaimana untuk menjalankan code secara
serentak, tetapi bagaimana untuk mencantumkan kembali semua output daripada code
tersebut.

Katakanlah kita tidak menggunakan konsep functional programming. Kita sentiasa
tukar-tukar nilai variable. Semasa kita mengambil data, adakah kita yakin data
tersebut telah diupdate atau data yang lama. Jika kita membuat update dengan
data yang lama, nilai terbaru akan menjadi tidak tepat. Kita akan melihat contoh
code yang bermasalah tersebut.

Katakanlah kita ada class User,

```java
import lombok.Builder;
import lombok.Data;

@Data
@Builder
public class User {
    private String name;
    private double money;
}
```

Kemudian, kita create object sebagai contoh,

```java
User user = User.builder()
        .name("Ali")
        .money(10000)
        .build();
```

Sekarang kita cuba buat dua proses untuk update duit Ali secara serentak. Ini
ibarat dua orang sedang *bank in* duit kepada Ali secara serentak. Codenya,

```java
Thread addMoney1 = new Thread(() -> {
    double money = user.getMoney();
    sleep(100);
    user.setMoney(money + 500);
});
addMoney1.start();

Thread addMoney2 = new Thread(() -> {
    double money = user.getMoney();
    sleep(200);
    user.setMoney(money + 300);
});
addMoney2.start();
```

`Thread` ialah class untuk meletakkan sebahagian code yang boleh berjalan secara
serentak. Method `sleep()` tersebut kita letakkan di method yang lain, seperti
berikut,

```java
private static void sleep(long millis) {
    try {
        Thread.sleep(millis);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

Method `sleep` ini hanya untuk demo sahaja. Method tersebut akan pause thread
yang berkenaan. Ini ibarat kita sedang membuat panggilan database yang mengambil
masa yang lama.

Kita menggunakan 100 ms untuk thread pertama dan 200 ms untuk thread kedua.
Maka, sepatutnya pada waktu 300 ms, kedua-dua thread tersebut sudah selesai.
Jadi, kita akan check nilai terkini pada 300 ms,

```java
sleep(300);
System.out.println(user);
```

Proses pertama menambah 500 dan proses kedua menambah 300 pada duit Ali. Secara
lojik akalnya sekarang Ali sepatutnya mempunyai 10800. Namun, selepas run code
tersebut, duit Ali ialah 10300. Ini berlaku kerana dalam thread kedua, variable
masih memegang nilai yang lama selepas update di thread pertama. Kes seperti ini
dinamakan racing condition.

Disebabkan code yang menggunakan nilai variable yang berubah-ubah mudah terkena
racing condition, satu cara untuk mengelakkannya adalah dengan tidak mengubah
nilai variable. Itu sebabnya kita belajar final dan immutable collection.

## Parallel stream

Cara paling mudah untuk menjalankan code secara serentak adalah dengan
menggunakan *parallel stream*. Caranya mudah sahaja. Tukar sahaja method
`stream()` ke `parallelStream()` dan siap. Contoh untuk code dari tutorial
sebelum ini,

```java
double taxAverage = taxes.stream()
        .mapToDouble(x -> x)
        .average()
        .orElse(0.0);
```

Tukar sahaja ke,

```java
double taxAverage = taxes.parallelStream()
        .mapToDouble(x -> x)
        .average()
        .orElse(0.0);
```

Namun, bukan semua code untuk concurrency menggunakan stream. Contoh, code
mungkin perlu mengambil data daripada database dan pada masa yang sama perlu
mengambil data daripada website lain. Untuk kes ini, kita akan lihat dalam
tutorial akan datang.

## Terminologi

Ada beberapa istilah yang digunakan dalam bab concurrency.

**Synchronous** bermaksud program berjalan satu per satu mengikut code. Jika ada
code yang lembab, program akan kelihatan seperti tersekat. Analoginya ibarat
membuat pesanan di KFC, kita membuat pesanan, pekerja mengambil makanan, kita
tunggu, siap, baru boleh duduk makan, kemudian pelanggan seterusnya pula membuat
pesanan.

**Asynchronous** pula bermaksud program berjalan tanpa mengikut urutan code,
sebaliknya mengikut CPU. Cara ini memberi ilusi seperti code kita sedang
berjalan dengan serentak. Analoginya ibarat membuat pesanan di Burger King, kita
membuat pesanan, pekerja bagi nombor, kita sudah boleh duduk, pelanggan
seterusnya sudah boleh membuat pesanan, nombor kita dipanggil, ambil burger,
makan.

**Future** dan **promise** ialah teknik untuk menulis code concurrency. Future
ialah data yang akan wujud pada masa akan datang, dan promise pula ialah tempat
yang dijanjikan untuk meletakkan data tersebut. Disebabkan lain programming
language lain cara penulisannya, maka kedua-dua istilah tersebut boleh bersilih
ganti.
