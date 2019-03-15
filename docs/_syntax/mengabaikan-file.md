---
date: 2018-12-03
description: Menggunakan file .gitignore untuk memilih file supaya tidak boleh dikesan oleh Git untuk keselamatan aplikasi.
---

# Mengabaikan File

Kita boleh menetapkan supaya sesetengah file tidak boleh dikesan oleh Git.
Contohnya apabila folder mempunyai file yang melibatkan security seperti
password database. Jika kita memasukkan file tersebut ke dalam Git, orang ramai
akan dapat melihat password, sekaligus boleh menggodam aplikasi kita.

Cara untuk mengabaikan file adalah dengan membina file `.gitignore`.

Kandungan file `.gitignore` ialah path ke file atau folder yang tidak mahu
dikesan. Contohnya,

```
password.txt
foldersecurity/
```

Taip `git status` untuk melihat sama ada Git boleh mengesan file-file tersebut.
