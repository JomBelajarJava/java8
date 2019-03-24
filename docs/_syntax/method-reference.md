---
date: 2019-03-19
description: Lihat penggunaan method reference, alternatif kepada lambda untuk menggantikan anonymous class.
---

# Method Reference

Selain *lambda*, ada satu lagi cara untuk membuat *anonymous class* iaitu dengan
menggunakan *method reference*. Untuk menggunakan *method reference*, kita mesti
menulis method yang membuat benda yang sama terlebih dahulu. Contoh menggunakan
code dari [Tutorial Ulang Kaji](ulang-kaji), kita tambah method
`tambahSepuluh()` seperti berikut,

```java
    public static int tambahSepuluh(int num) {
        return num + 10;
    }
```

Sekarang kita tukar code *anonymous class* menggunakan *method reference*,
seperti berikut,

```java
    public static void main(String[] args) {
        int jawapan = Nilai.daripada(5 + 3)
                .kemudian(Application::tambahSepuluh);

        System.out.println(jawapan);
    }
```

Seperti code di atas, untuk membuat *method reference*, kita tulis nama class,
kemudian simbol `::`, kemudian nama method yang kita mahu gunakan.
