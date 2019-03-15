---
date: 2018-11-27
description: Melihat cara untuk push atau mengupload code ke remote, iaitu server yang host Git, atau website-website seperti GitHub, GitLab, dan Bitbucket.
---

# Remote

Remote bermaksud sesuatu yang jauh. Kalau ternampak perkataan remote dalam Git,
itu bermaksud tempat yang jauh untuk kita upload code iaitu server, tidak
kiralah server sendiri atau website seperti GitHub, GitLab, dan Bitbucket. Jadi,
jika anda masih belum daftar mana-mana website tersebut, anda boleh daftar
sekarang untuk mengikuti tutorial ini.

## Repository

Setelah mendaftar, anda boleh membina repository untuk projek. Repository ialah
tempat untuk meletakkan code. Kita boleh menggunakan nama yang sama dengan nama
folder yang telah kita buat untuk nama repository, iaitu `cikgu`.

Cara untuk create repository berbeza mengikut website. Jadi, anda boleh ambil
masa untuk meneroka website tersebut.

Setelah membina repository, biasanya website akan menunjukkan cara untuk upload
code. Ikut sahaja arahan tersebut.

## git remote

Antara arahan yang anda perlu ikut ialah command `git remote`. Kita boleh
menggunakan `git remote` untuk menetapkan url untuk upload code di website
tersebut.

Dalam arahan, biasanya ada command seperti ini,

```
git remote add origin https://github.com:namasaya/cikgu.git
```

misalannya.

`git remote add origin` bermaksud kita tambah url kemudian tetapkan nama
`origin` untuk url tersebut.

## git push

`git push` ialah command yang akan upload code kita ke website berkenaan. Jika
anda mengikuti arahan website, anda mungkin perlu menaip,

```
git push -u origin master
```

Jadi, Git akan *push*(upload) code ke url origin yang telah kita tetapkan
sebelum ini ke branch master.

> Kita akan lihat dengan lebih jelas mengenai branch pada tutorial akan datang.

Huruf `-u` hanya perlu ditulis jika kita upload buat pertama kali. Selepas ini
kita hanya perlu tulis

```
git push origin master
```

Anda mungkin perlu memberi password semasa push code.

Setelah selesai push, anda boleh refresh website tersebut dan anda boleh melihat
dan meneroka file-file yang telah diupload.

## git clone

Sebagai demo, kita akan padam folder `cikgu` yang telah kita bina di komputer
kita untuk cuba download kembali file-file tersebut.

`git clone` ialah command untuk download seluruh projek dari remote. Jadi, download kembali projek `cikgu` dengan menaip,

```
git clone https://github.com:namasaya/cikgu.git
```

Url tersebut bergantung kepada url repository anda.

## git pull

`git pull` pula ialah command untuk download hanya perubahan code, bukan seluruh
projek. Jika anda menggunakan website, anda boleh edit file di website.
Kemudian, anda boleh cuba download perubahan menggunakan

```
git pull origin master
```

dan anda boleh lihat perubahan berlaku pada file di komputer anda.
