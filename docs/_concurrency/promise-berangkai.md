---
date: 2019-03-24
description: Belajar Java 8, melihat penggunaan CompletableFuture yang berangkai, sama seperti stream, iaitu thenApply dan thenCompose.
---

# Promise Berangkai

Sama seperti method-method stream, method-method `CompletableFuture` juga boleh
berangkai. Sebelum ini, kita menggunakan method `thenAccept`. Method
`thenAccept` tidak akan return output. Untuk return output, kita boleh
menggunakan method `then` yang lain, contohnya `thenApply`.

## thenApply

Penggunaan `thenApply` boleh diibaratkan seperti setiap langkah yang perlu
dibuat selepas mendapat output.

Contoh, katakanlah kita ada service untuk mendapatkan nama user dan untuk
menjadikan nama user sebagai id, seperti berikut,

```java
public class UserService {
    public static String getName() {
        return "Muhammad Ali";
    }

    public static String makeId(String name) {
        return name
                .toLowerCase()
                .replace(' ', '-');
    }
}
```

Maka kita boleh menulis code berangkai seperti berikut,

```java
supplyAsync(UserService::getName)
        .thenApplyAsync(UserService::makeId)
        .thenAcceptAsync(System.out::println);
```

Jadi, apabila kita membaca code tersebut, kita boleh faham bagaimana urutan
proses code tersebut; `getName()`, kemudian `makeId()`, kemudian print. Dari
segi teknikal pula, urutan program adalah terpulang kepada CPU, mungkin CPU akan
menjalankan code yang lain dahulu selepas `getName()`, kemudian sambung
`makeId()` semula.

## thenCompose

Katakanlah dalam method `makeId()` pun kita mahu menulis code menggunakan
CompletableFuture, seperti berikut,

```java
public static CompletionStage<String> makeId(String name) {
    return completedFuture(name)
            .thenApplyAsync(String::toLowerCase)
            .thenApplyAsync(s -> s.replace(' ', '-'));
}
```

> Nota: `CompletionStage` ialah interface untuk `CompletableFuture`. Method
> `completedFuture` adalah untuk menggunakan `CompletableFuture` tanpa membuat
> Thread yang baru.

Selepas mengubah method `makeId`, apabila kita run semula code, program akan
print `CompletableFuture`, bukannya output dari future tersebut kerana jenis
return data untuk method `makeId` sekarang ialah `CompletionStage`. Untuk
menggunakan output dari future, kita boleh menggunakan method `thenCompose`,
seperti berikut,

```java
supplyAsync(UserService::getName)
        .thenComposeAsync(UserService::makeId)
        .thenAcceptAsync(System.out::println);
```

Run semula program dan kita dapat lihat program menghasilkan output yang sama
dengan code yang menggunakan `thenApply` sebelum ini. Cara ini disebut sebagai
`flatMap`.
