---
date: 2019-03-24
description: Belajar Java 8, lihat penggunaan CompletableFuture dalam web framework Spring Boot.
---

# Web Framework

Sesetengah *web framework* boleh memanfaatkan `CompletableFuture`, antaranya
Spring Boot dan Play Framework. Dalam tutorial ini, kita akan melihat contoh
menggunakan `CompletableFuture` dalam Spring Boot.

Katakanlah kita ada servis `UserService` seperti berikut,

```java
@Service
public class UserService {
    public CompletionStage<String> getName() {
        return "Muhammad Ali";
    }

    public CompletionStage<String> getFacebookImageUrl() {
        return "https://www.example.com/ali.jpg";
    }
}
```

> Anggap kedua-dua method tersebut mengambil data dari database.

Kemudian controller untuk maklumat user, `UserApiController`, seperti berikut,

```java
@RestController
public class UserApiController {
    @Autowired
    private UserService userService;

    @GetMapping("/user")
    public String userInfo() {
        return userService.getName() + ", " + userService.getFacebookImageUrl();
    }
}
```

## CompletionStage

Sekarang kita akan menggantikan code di atas dengan `CompletableFuture`. Caranya
adalah dengan menjadikan method supaya return `CompletionStage`.

> `CompletionStage` ialah interface kepada `CompletableFuture`.

Contoh untuk `UserService`,

```java
public CompletionStage<String> getName() {
    return supplyAsync(() -> "Muhammad Ali");
}

public CompletionStage<String> getFacebookImageUrl() {
    return supplyAsync(() -> "https://www.example.com/ali.jpg");
}
```

Kemudian kita gabungkan di controller, seperti berikut,

```java
@GetMapping("/user")
public CompletionStage<String> userInfo() {
    return userService.getName().thenCombineAsync(
            userService.getFacebookImageUrl(),
            (name, fbImgUrl) -> name + ", " + fbImgUrl
    );
}
```

Spring Boot secara automatik akan tahu yang kita sedang menulis code
asynchronous dan akan handle dengan betul. Jadi, kita tidak perlu menggunakan
`thenAccept` atau `join` untuk mendapatkan output untuk return dari controller.

## Executor

Dalam Tutorial Executor yang lepas, kita faham ada kerenah dengan program
apabila kita menggunakan lebih daripada satu thread, iaitu apabila program perlu
ditutup. Jika kita ingin menggunakan `ExecutorService` dalam sesuatu framework,
kita perlu mengambil tahu bagaimana *lifecycle*(kitar hidup) framework tersebut,
kemudian research bagaimana untuk menjalankan sesuatu proses pada fasa tertentu.

Selepas membuat sedikit research (cari di google), kita dapat tahu yang kita
boleh tetapkan method yang perlu dipanggil apabila sesuatu Spring Bean ditutup
menggunakan parameter `destroyMethod`.

Sekarang kita cuba create `ExecutorService` untuk database dan logger dalam
config,

```java
@Configuration
public class GlobalConfig {
    @Bean(destroyMethod = "shutdown")
    @Qualifier("db")
    public ExecutorService dbThreadPool() {
        return Executors.newFixedThreadPool(100);
    }

    @Bean(destroyMethod = "shutdown")
    @Qualifier("logger")
    public ExecutorService loggerThreadPool() {
        return Executors.newSingleThreadExecutor();
    }
}
```

Seperti code di atas, kita letakkan nama method yang perlu dipanggil, iaitu
`shutdown` untuk parameter `destroyMethod`. Kita juga menggunakan annotation
`Qualifier` untuk membezakan `ExecutorService` mana yang kita mahu semasa
membuat *autowire*.

Setelah selesai, maka kita boleh menggunakan thread pool tersebut pada code
async yang bersesuaian. Katakanlah `dbThreadPool` untuk proses database, dan
`loggerThreadPool` untuk melihat output yang kita beri kepada pengguna website.

Code untuk `UserService`,

```java
@Service
public class UserService {
    @Autowired
    @Qualifier("db")
    private ExecutorService dbThreadPool;

    public CompletionStage<String> getName() {
        return supplyAsync(() -> "Muhammad Ali", dbThreadPool);
    }

    public CompletionStage<String> getFacebookImageUrl() {
        return supplyAsync(() -> "https://www.example.com/ali.jpg", dbThreadPool);
    }
}
```

Code untuk `UserApiController`,

```java
@RestController
public class UserApiController {
    @Autowired
    @Qualifier("logger")
    private ExecutorService loggerThreadPool;

    @Autowired
    private UserService userService;

    @GetMapping("/user")
    public CompletionStage<String> userInfo() {
        CompletionStage<String> userInfo = userService.getName()
                .thenCombineAsync(
                        userService.getFacebookImageUrl(),
                        (name, fbImgUrl) -> name + ", " + fbImgUrl
                );

        userInfo.thenAcceptAsync(System.out::println, loggerThreadPool);

        return userInfo;
    }
}
```

Jika kita pergi ke page website tersebut, program akan output ke console pada
masa yang sama.
