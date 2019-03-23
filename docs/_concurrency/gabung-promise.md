---
date: 2019-03-24
description: Belajar Java 8, melihat bagaimana menggabungkan beberapa CompletableFuture untuk diproses.
---

# Gabung Promise

Sekarang kita lihat bagaimana untuk menggabungkan beberapa `CompletableFuture`.
Katakanlah kita pecahkan method `getName()` dari tutorial sebelum ini ke
`getFirstName()` dan `getLastName()`,

```java
public static String getFirstName() {
    return "John";
}

public static String getLastName() {
    return "Jackson";
}
```

Maka `CompletableFuture` untuk mendapatkan kedua-dua nilai tersebut ialah,

```java
CompletableFuture<String> firstNameFuture = supplyAsync(UserService::getFirstName);
CompletableFuture<String> lastNameFuture = supplyAsync(UserService::getLastName);
```

> Nota: Dalam situasi sebenar, biasanya `firstName` dan `lastName` diletakkan
> sekali dalam satu object, contohnya class `User`, jadi tidak perlu menggunakan
> dua thread. Untuk tutorial ini, saya menggunakan dua thread sebagai demo
> sahaja. Anggap `firstName` kita ambil daripada database dan `lastName`
> daripada Facebook API misalannya.

## thenCombine

Sekarang untuk menggabungkan kedua-dua future tersebut, kita boleh menggunakan
method `thenCombine`, seperti berikut,

```java
firstNameFuture
        .thenCombineAsync(lastNameFuture,
                (firstName, lastName) -> firstName + " " + lastName)
        .thenApplyAsync(UserService::makeId)
        .thenAcceptAsync(System.out:: println);
```

Method `thenCombine` mempunyai dua parameter, parameter pertama ialah future
yang kita ingin gabungkan, parameter kedua ialah BiFunction.

## thenAcceptBoth

Jika kita mahu terus print tanpa menulis secara berangkai, kita boleh
menggunakan `thenAcceptBoth`, contohnya,

```java
firstNameFuture.thenAcceptBothAsync(lastNameFuture, UserService::printLastName);
```

Method `printLastName` di atas adalah seperti berikut,

```java
public static void printLastName(String firstName, String lastName) {
    System.out.println(firstName + " " + lastName);
}
```

## allOf

Kedua-dua method di atas hanyalah untuk dua future sahaja. Jika kita mahu
menggabungkan lebih daripada dua future, kita perlu menggunakan method `allOf`.
Katakanlah ada satu lagi method `getAge`,

```java
public static int getAge() {
    return 25;
}
```

Future untuk method `getAge` ialah,

```java
CompletableFuture<Integer> ageFuture = supplyAsync(UserService::getAge);
```

Maka, penggunaan `allOf` adalah seperti berikut,

```java
allOf(firstNameFuture, lastNameFuture, ageFuture)
        .thenApplyAsync(v -> String.format("%s %s, %s",
                firstNameFuture.join(),
                lastNameFuture.join(),
                ageFuture.join()))
        .thenAcceptAsync(System.out::println);
```

Method `allOf` agak pelik sedikit kerana method tersebut akan return
`CompletableFuture<Void>` jadi kita tidak boleh menggunakan Function yang
mempunyai tiga parameter misalannya. Perhatikan parameter untuk function
tersebut saya letakkan `v`. Anda boleh guna apa sahaja keyword kerana kita tidak
akan menggunakan parameter tersebut. Kita boleh menggunakan method `join()`
untuk mengambil output daripada ketiga-tiga future tersebut.

Method `join()` jika digunakan di luar `allOf` akan memaksa program untuk
menunggu sehingga mendapat output dan boleh menyekat thread yang lain. Method
`allOf` pula hanya akan menjalankan method `then` setelah semua future tersebut
selesai, jadi tidak menjadi masalah untuk memanggil method `join`.
