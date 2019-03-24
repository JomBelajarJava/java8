---
date: 2019-03-24
description: Belajar Java 8, melihat penggunaan class Executor untuk customize bagaimana code concurrency dijalankan.
---

# Executor

Dalam tutorial ini, kita akan pergi lebih teknikal.

Dalam setiap tutorial sebelum ini di mana kita menggunakan method async, code
tersebut bukannya create thread yang baru, tetapi menggunakan thread yang sedia
ada daripada [ForkJoinPool.commonPool()][common-pool]. Thread pool ini mempunyai
bilangan thread tertentu yang ditetapkan oleh Java, katakanlah 10. Apabila semua
thread dalam pool sedang digunakan, program akan menunggu sehinggalah ada thread
yang selesai dan sedia untuk proses seterusnya.

Dengan maklumat ini, kita boleh menjangkakan jika semua thread dalam pool sedang
digunakan dan semua proses memakan masa yang lama, maka program akan tersangkut.
Bagi menyelesaikan masalah ini, kita perlu mengalihkan proses yang memakan masa
yang lama ke thread yang lain yang bukan daripada `ForkJoinPool.commonPool()`.
Caranya adalah dengan menggunakan `Executor`.

## Executors

Untuk create `Executor`, kita boleh menggunakan mana-mana method yang ada dalam
class [Executors][executors]. Contohnya jika kita mahu membuat thread pool untuk
database dan untuk log,

```java
ExecutorService dbThreadPool = Executors.newFixedThreadPool(100);
ExecutorService loggerThreadPool = Executors.newSingleThreadExecutor();
```

Bilangan thread yang ditetapkan bergantung kepada server. Mungkin server
mempunyai RAM kecil, jadi boleh kurangkan sedikit untuk mengelakkan
`OutOfMemoryError`. Mungkin jenis database terlalu canggih, jadi boleh tambah
bilangan thread. Terpulang.

`ExecutorService` akan tetap hidup walaupun program kita telah tertutup kerana
executor tersebut berada di thread yang lain. Untuk menutup `ExecutorService`,
kita perlu memanggil method `shutdown()` di penghujung program. Penggunaan
method `shutdown` sahaja tidak cukup kerana mungkin ada thread yang tersekat
atau menghadapi apa-apa masalah, jadi kita boleh paksa shutdown menggunakan
`shutdownNow` jika thread tidak ditutup dalam masa tertentu. Untuk itu, kita
boleh copy paste code daripada documentation [ExecutorService][executorservice],
seperti berikut,

```java
private static void shutdownExecutors(ExecutorService... pools) {
    for (ExecutorService pool : pools) {
        pool.shutdown();

        try {
            if (!pool.awaitTermination(60, TimeUnit.SECONDS)) {
                pool.shutdownNow();

                if (!pool.awaitTermination(60, TimeUnit.SECONDS)) {
                    System.err.println("Pool did not terminate");
                }
            }
        } catch (InterruptedException ie) {
            pool.shutdownNow();
            Thread.currentThread().interrupt();
        }
    }
}
```

Kita boleh memanggil method tersebut di penghujung code kita,

```java
shutdownExecutors(dbThreadPool, loggerThreadPool);
```

Satu lagi masalah ialah bukan semua program akan sampai ke penghujung code.
Contohnya program untuk server akan hidup sepanjang masa, dan hanya tertutup
apabila kita mahu tutup atau berlaku error. Apabila kes seperti ini berlaku,
method `shutdownExecutors` yang kita tulis di penghujung code kemungkinan tidak
akan dipanggil. Oleh itu, kita boleh menambah satu lagi panggilan ke
`shutdownExecutors` di dalam *shutdown hook*, contohnya,

```java
ExecutorService dbThreadPool = Executors.newFixedThreadPool(100);
ExecutorService loggerThreadPool = Executors.newSingleThreadExecutor();

Runtime.getRuntime().addShutdownHook(new Thread(() -> shutdownExecutors(dbThreadPool, loggerThreadPool)));
```

Setelah selesai, barulah kita boleh menggunakan executor tersebut pada proses future yang kita tulis sebelum ini. Caranya adalah dengan meletakkan executor tersebut sebagai argument kedua method async. Contohnya,

```java
CompletableFuture<String> firstNameFuture = supplyAsync(UserService::getFirstName, dbThreadPool);
CompletableFuture<String> lastNameFuture = supplyAsync(UserService::getLastName, dbThreadPool);
CompletableFuture<Integer> ageFuture = supplyAsync(UserService::getAge, dbThreadPool);

allOf(firstNameFuture, lastNameFuture, ageFuture)
        .thenApplyAsync(v -> String.format("%s %s, %s",
                firstNameFuture.join(),
                lastNameFuture.join(),
                ageFuture.join()))
        .thenAcceptAsync(System.out::println, loggerThreadPool);
```

Jadi dalam code di atas, kita menggunakan `dbThreadPool` untuk future
`firstName`, `lastName`, dan `age`, `ForkJoinPool.commonPool()` untuk format
string, dan `loggerThreadPool` untuk print.

Macam mana nak tahu perlu guna yang mana satu? Gunakan thread pool yang lain
untuk proses yang memakan masa yang melibatkan IO (seperti database, email, atau
connection ke server lain), dan gunakan yang default untuk code-code lain.



[common-pool]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html#commonPool--
[executors]: https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Executors.html
[executorservice]: https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ExecutorService.html
