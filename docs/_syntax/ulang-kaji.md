---
date: 2019-03-19
description: Ulang kaji konsep-konsep yang berkaitan functional programming untuk menggunakan syntax Java 8.
---

# Ulang Kaji

Dalam [Tutorial Final](/java/quality/final), kita didedahkan sedikit dengan
konsep *functional programming*. Jika anda sudah biasakan diri dengan penggunaan
`final`, tahniah! Cara pemikiran anda lebih mudah untuk memahami konsep
*functional programming*.

## Imperative vs Declarative

*Imperative programming* ialah cara penulisan code yang biasa kita lihat. Cara
ini akan mengubah nilai variable dalam beberapa baris code. Baris-baris code
tersebut adalah seperti resepi selangkah demi selangkah. Contohnya untuk membuat
roti tempek,

```
1. Ambil tepung.
2. Tambah air, tambah telur.
3. Uli tepung.
4. Tempek tepung di atas kuali.
```

Jika menggunakan code, mungkin akan kelihatan begini,

```java
Tepung tepung = new Tepung();
tepung.add(new Air());
tepung.add(new Telur());
tepung.uli();

Kuali kuali = new Kuali();
kuali.add(tepung);
kuali.masak();

RotiTempek rotiTempek = kuali.cedok();
```

*Declarative programming* pula ialah cara penulisan code yang mengutamakan apa
yang akan code tersebut hasilkan. Cara ini terus memberitahu hubungan antara
input dan output. Contoh yang sama untuk membuat roti tempek,

```
Roti tempek ialah tepung yang diuli dengan air dan telur ditempek di atas kuali.
```

Jika menggunakan code, mungkin akan kelihatan begini,

```java
RotiTempek rotiTempek = Kuali.masak(Tepung.uli(new Air(), new Telur()));
```

*Functional programming* menggunakan cara *declarative*.

## Function

Setiap programming language mempunyai kerenah masing-masing. Dalam Java, apa-apa
code terpaksa diletakkan dalam class. Kekangan ini tidak selari dengan
*functional language* yang lain. Dalam *functional language* yang lain, code
boleh terus diletakkan di dalam file. Apabila method/function/procedure berada
di hierarki yang teratas, code tersebut dinamakan *higher-order function*.

Disebabkan Java tiada *higher-order function*, kita boleh menjadikan class
sebagai *higher-order function* dengan membina class yang mengandungi satu
method.

Sekarang kita akan cuba membuat *higher-order function* menggunakan class.

Dalam Java kita boleh menggunakan *anonymous class* untuk implement sesuatu
interface. Jadi kita bina interface yang mengandungi satu method sebagai
function seperti berikut,

```java
public interface Function {
    int apply(int num);
}
```

Kemudian kita akan membina class yang boleh memanggil method `apply()` tersebut.

```java
public class Nilai {
    private int num;

    public Nilai(int num) {
        this.num = num;
    }

    public int kemudian(Function func) {
        return func.apply(num);
    }

    public static Nilai daripada(int num) {
        return new Nilai(num);
    }
}
```

Untuk menggunakan function tersebut, kita boleh menggunakan *anonymous class*
seperti berikut,

```java
public class Application {
    public static void main(String[] args) {
        int jawapan = Nilai.daripada(5 + 3)
                .kemudian(new Function() {
                    @Override
                    public int apply(int num) {
                        return num + 10;
                    }
                });

        System.out.println(jawapan);
    }
}
```

Kita boleh faham code dalam method `main()` di atas seperti membaca,

```
Jawapan ialah nilai daripada 5 + 3, kemudian ditambah dengan 10.
```

Cuma kelemahannya sekarang ialah syntax untuk membuat *anonymous class* tersebut
sangat bercelaru. Jadi, Java 8 memperkenalkan syntax untuk menggantikan syntax
untuk membuat *anonymous class*. Kita akan melihat syntax tersebut di tutorial
selepas ini.
