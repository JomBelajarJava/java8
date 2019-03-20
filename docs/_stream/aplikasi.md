---
date: 2019-03-21
description: Melihat contoh-contoh penggunaan method map, reduce, dan filter.
---

# Aplikasi

Boleh dikatakan semua proses dalam aplikasi bisnes boleh diselesaikan
menggunakan method `map()`, `reduce()`, dan `filter()`. Dalam tutorial ini, kita
akan melihat beberapa contoh yang melibatkan semua method tersebut.

## Kes 1

Kerajaan ingin merancang bajet negara, maka institusi kita diminta untuk mengira
cukai yang dibayar oleh ahli. Kadar cukai ialah 2% jika melebihi RM 10000. Kita
sebagai programmer diminta untuk menjumlahkan cukai ahli.

Jadi, dengan menggunakan library Lombok dan Google Guava code, kita mempunyai
class untuk pembayar cukai, TaxPayer,

```java
import lombok.Builder;
import lombok.Value;

@Value
@Builder
public class TaxPayer {
    private String name;
    private double money;
}
```

Servis untuk mendapatkan senarai pembayar cukai berserta jumlah duit,

```java
import com.google.common.collect.ImmutableList;

import java.util.List;

public class TaxPayerService {
    public static List<TaxPayer> getAllTaxPayers() {
        return ImmutableList.of(
                TaxPayer.builder().name("Ali").money(20000).build(),
                TaxPayer.builder().name("Abu").money(5000).build(),
                TaxPayer.builder().name("Suzana").money(15000).build(),
                TaxPayer.builder().name("John").money(9000).build()
        );
    }
}
```

Maka code yang kita tulis untuk kes ini akan kelihatan begini,

```java
double totalTaxPaid = TaxPayerService
        .getAllTaxPayers()
        .stream()
        .mapToDouble(TaxPayer::getMoney)
        .filter(money -> money > 10000)
        .map(money -> money * 0.02)
        .sum();

System.out.println("Jumlah cukai yang akan dibayar: RM " + totalTaxPaid);
```

## Kes 2

Bersambung dari Kes 1, pihak atasan mahu senarai maklumat ahli berserta cukai
yang dibayar. Maka, kita pun google bagaimana format file csv. Format file csv
dibahagi menggunakan tanda koma, baris demi baris.

Menggunakan format tersebut, kita tulis code seperti ini,

```java
String csv = TaxPayerService
        .getAllTaxPayers()
        .stream()
        .map(taxPayer -> String.format(
                "%s,%.2f,%.2f",
                taxPayer.getName(),
                taxPayer.getMoney(),
                taxPayer.getMoney() > 10000 ? taxPayer.getMoney() * 0.02 : 0.0
        ))
        .collect(joining("\n"));

System.out.println(csv);
```

Untuk menghasilkan,

```
Ali,20000.00,400.00
Abu,5000.00,0.00
Suzana,15000.00,300.00
John,9000.00,0.00
```

Method `joining()` di atas adalah untuk menggabungkan string. Anda boleh melihat
documentation di
[Collectors#joining()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining-java.lang.CharSequence-).

## Kes 3

Sekarang mereka mahu bilangan pembayar cukai dan purata cukai yang dibayar.

Kita pun tulis code seperti ini,

```java
List<Double> taxes = TaxPayerService
        .getAllTaxPayers()
        .stream()
        .map(TaxPayer::getMoney)
        .filter(money -> money > 10000)
        .map(money -> money * 0.02)
        .collect(toList());

int taxPayersCount = taxes.size();

double taxAverage = taxes.stream()
        .mapToDouble(x -> x)
        .average()
        .orElse(0.0);

System.out.println("Bilangan pembayar cukai: " + taxPayersCount);
System.out.println("Purata cukai yang dibayar: " + taxAverage);
```

----

Seperti yang kita lihat dari contoh-contoh di atas, kita tidak perlu lagi
menulis loop atau membuat perubahan pada mana-mana variable. Kita juga tidak
perlu menggunakan method `reduce()` kerana telah banyak method yang telah
disediakan untuk menggantikan `reduce()` seperti `sum()`, `average()`, dan
`collect(joining())`.
