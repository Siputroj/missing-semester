---
layout: lecture
title: "Pengolahan Data"
presenter: Jon
video:
  aspect: 56.25
  id: VW2jn9Okjhw
---

Pernah punya sekumpulan teks dan ingin melakukan sesuatu dengannya? Bagus. Itulah inti pengolahan data: mengadaptasi data dari satu format ke format lain sampai menjadi persis seperti yang Anda mau.

Kita sudah melihat pengolahan data dasar: `journalctl | grep -i intel`.
 - mencari semua entri log sistem yang menyebut Intel (tidak peka huruf besar/kecil)
 - sebagian besar pengolahan data adalah tahu alat apa yang Anda miliki dan cara menggabungkannya.

Mulai dari awal: kita butuh sumber data dan sesuatu untuk dilakukan. Log sering jadi kasus bagus karena kita sering ingin menyelidiki sesuatu tentang log, dan membaca seluruhnya tidak mungkin. Mari cari tahu siapa yang mencoba login ke server saya dengan melihat log server:

```bash
ssh myserver journalctl
```

Terlalu banyak. Batasi ke hal terkait ssh:

```bash
ssh myserver journalctl | grep sshd
```

Perhatikan kita memakai pipe untuk mengalirkan berkas _jarak jauh_ melalui `grep` di komputer lokal! `ssh` ajaib. Masih terlalu banyak dan susah dibaca. Mari lebih baik:

```bash
ssh myserver journalctl | grep sshd | grep "Disconnected from"
```

Masih banyak noise. Ada _banyak_ cara menyingkirkannya, tapi mari lihat salah satu alat terkuat: `sed`.

`sed` adalah "stream editor" yang dibangun di atas editor lama `ed`. Anda memberi perintah singkat untuk memodifikasi berkas, bukan memanipulasi isinya langsung (meski bisa juga). Ada banyak perintah, salah satu yang paling umum adalah `s`: substitusi. Contoh:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed 's/.*Disconnected from //'
```

Apa yang kita tulis adalah _regular expression_ sederhana; konstruksi kuat untuk mencocokkan teks terhadap pola. Perintah `s` ditulis dengan bentuk: `s/REGEX/PENGGANTI/`, di mana `REGEX` adalah regex yang ingin dicari, dan `PENGGANTI` adalah teks pengganti.

## Regular expression

Regex cukup umum dan berguna sehingga layak memahami cara kerjanya. Lihat yang kita pakai: `/.*Disconnected from /`. Regex biasanya (meski tidak selalu) diapit `/`. Kebanyakan karakter ASCII bermakna biasa, tetapi beberapa karakter punya perilaku pencocokan "khusus". Persis karakter apa yang melakukan apa bisa berbeda antar implementasi, yang bikin frustrasi. Pola yang sangat umum:

 - `.` berarti "sebarang satu karakter" kecuali newline
 - `*` nol atau lebih dari kecocokan sebelumnya
 - `+` satu atau lebih dari kecocokan sebelumnya
 - `[abc]` salah satu karakter `a`, `b`, atau `c`
 - `(RX1|RX2)` sesuatu yang cocok `RX1` atau `RX2`
 - `^` awal baris
 - `$` akhir baris

Regex di `sed` agak aneh, dan mengharuskan Anda menaruh `\` sebelum kebanyakan karakter ini agar bermakna khusus. Atau gunakan `-E`.

Jadi, melihat `/.*Disconnected from /`, pola ini mencocokkan teks yang dimulai dengan sejumlah karakter apa saja, diikuti string literal "Disconnected from ". Itu yang kita mau. Tapi hati-hati, regex itu rumit. Bagaimana jika seseorang mencoba login dengan username "Disconnected from"? Kita akan mendapat:

```
Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from invalid user Disconnected from 46.97.239.16 port 55920 [preauth]
```

Apa hasil kita? Secara default `*` dan `+` bersifat "rakus". Mereka mencocokkan sebanyak mungkin teks. Jadi di atas, hasilnya hanya:

```
46.97.239.16 port 55920 [preauth]
```

Mungkin bukan yang kita mau. Pada beberapa implementasi regex, Anda bisa menambahkan `?` setelah `*` atau `+` agar tidak rakus, tetapi sayangnya `sed` tidak mendukung. Kita _bisa_ beralih ke mode command-line perl yang mendukung:

```bash
perl -pe 's/.*?Disconnected from //'
```

Namun kita akan bertahan dengan `sed` karena jauh lebih umum untuk pekerjaan seperti ini. `sed` juga bisa melakukan hal berguna lain seperti mencetak baris setelah kecocokan, melakukan beberapa substitusi sekaligus, mencari sesuatu, dll. Tapi tidak akan kita bahas terlalu banyak di sini. `sed` sendiri bisa jadi satu topik penuh, meski sering ada alat yang lebih baik.

Oke, kita juga punya sufiks yang ingin dihapus. Bagaimana caranya? Cukup rumit mencocokkan teks setelah username, apalagi jika username bisa punya spasi, dsb! Kita perlu mencocokkan _seluruh_ baris:

```bash
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user .* [^ ]+ port [0-9]+( \[preauth\])?$//'
```

Mari lihat apa yang terjadi dengan [debugger regex](https://regex101.com/r/qqbZqh/2). Awalannya sama seperti sebelumnya. Lalu kita mencocokkan variasi "user" (ada dua prefiks di log). Lalu mencocokkan string apa pun tempat username. Lalu mencocokkan satu kata apa pun (`[^ ]+`; rangkaian non-spasi yang tidak kosong). Lalu kata "port" diikuti digit. Lalu mungkin sufiks ` [preauth]`, dan akhir baris.

Perhatikan dengan teknik ini, username "Disconnected from" tidak akan membingungkan lagi. Bisa lihat alasannya?

Ada satu masalah: seluruh log menjadi kosong. Kita ingin _menyimpan_ username. Untuk ini kita bisa memakai "capture group". Teks apa pun yang dicocokkan regex diapit tanda kurung disimpan di capture group bernomor. Ini tersedia di bagian pengganti (dan di beberapa mesin bahkan di pola!) sebagai `\1`, `\2`, `\3`, dll. Jadi:

```bash
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
```

Seperti yang bisa Anda bayangkan, Anda bisa membuat regex yang _sangat_ rumit. Misalnya, artikel ini tentang mencocokkan [alamat e-mail](https://www.regular-expressions.info/email.html). Itu [tidak mudah](https://web.archive.org/web/20221223174323/http://emailregex.com/). Dan ada [banyak diskusi](https://stackoverflow.com/questions/201323/how-to-validate-an-email-address-using-a-regular-expression/1917982). Dan orang-orang [menulis tes](https://fightingforalostcause.net/content/misc/2006/compare-email-regex.php). Dan [matriks tes](https://mathiasbynens.be/demo/url-regex). Anda bahkan bisa menulis regex untuk menentukan apakah sebuah angka [adalah bilangan prima](https://www.noulakaz.net/2007/03/18/a-regular-expression-to-check-for-prime-numbers/).

Regex terkenal sulit dibuat benar, tetapi sangat berguna untuk dimiliki!

## Kembali ke pengolahan data

Sekarang kita punya:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
```

Kita bisa melakukannya hanya dengan `sed`, tapi untuk bersenang-senang saja:

```bash
ssh myserver journalctl
 | sed -E
   -e '/Disconnected from/!d'
   -e 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
```

Ini menunjukkan beberapa kemampuan `sed`. `sed` juga bisa menyisipkan teks (dengan perintah `i`), secara eksplisit mencetak baris (dengan `p`), memilih baris berdasarkan indeks, dan banyak hal lain. Lihat `man sed`!

Bagaimanapun. Apa yang kita punya sekarang memberi daftar semua username yang mencoba login. Tapi ini kurang membantu. Mari cari yang umum:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
```

`sort` akan mengurutkan input. `uniq -c` akan menggabungkan baris berurutan yang sama menjadi satu baris, diawali jumlah kemunculan. Kita mungkin ingin mengurutkan itu juga dan hanya menyimpan login paling umum:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
```

`sort -n` akan mengurutkan numerik (bukan leksikografis). `-k1,1` berarti "urutkan hanya berdasarkan kolom pertama yang dipisah spasi". Bagian `,n` berarti "urutkan sampai kolom ke-n, defaultnya akhir baris". Dalam contoh _tertentu_ ini, mengurutkan seluruh baris tidak masalah, tapi kita belajar!

Jika ingin yang paling jarang, pakai `head` alih-alih `tail`. Ada juga `sort -r` untuk urutan terbalik.

Oke, cukup keren, tapi kita ingin hanya username dan mungkin tidak satu per baris?

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd,
```

Mulai dari `paste`: memungkinkan Anda menggabungkan baris (`-s`) dengan delimiter satu karakter (`-d`). Tapi apa itu `awk`?

## awk -- editor lain

`awk` adalah bahasa pemrograman yang kebetulan sangat bagus untuk memproses stream teks. Ada _banyak_ yang bisa dikatakan jika Anda mempelajarinya benar, tapi seperti hal lain di sini, kita bahas dasar-dasarnya.

Pertama, apa yang dilakukan `{print $2}`? Program `awk` berbentuk pola opsional plus blok yang mengatakan apa yang dilakukan jika pola cocok dengan baris. Pola default (yang kita pakai) cocok semua baris. Dalam blok, `$0` berisi seluruh baris, dan `$1` hingga `$n` berisi _field_ ke-n di baris itu, dipisah oleh pemisah field `awk` (bawaan whitespace, ubah dengan `-F`). Dalam kasus ini, kita bilang untuk setiap baris, cetak isi field kedua, yaitu username!

Mari coba yang lebih rumit. Hitung jumlah username sekali pakai yang diawali `c` dan diakhiri `e`:

```bash
 | awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
```

Banyak yang perlu diurai. Pertama, perhatikan kita sekarang punya pola (sebelum `{...}`). Pola mengatakan field pertama harus sama dengan 1 (itu hitungan dari `uniq -c`), dan field kedua harus cocok regex. Bloknya hanya mencetak username. Lalu kita hitung jumlah baris output dengan `wc -l`.

Namun, `awk` adalah bahasa pemrograman, ingat?

```awk
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```

`BEGIN` adalah pola yang cocok awal input (dan `END` cocok akhir). Kini blok per baris hanya menambah hitungan dari field pertama (meski di kasus ini selalu 1), lalu kita cetak di akhir. Faktanya, kita _bisa_ menyingkirkan `grep` dan `sed` sepenuhnya karena `awk` [bisa melakukan semuanya](https://backreference.org/2010/02/10/idiomatic-awk/), tapi kita serahkan sebagai latihan.

## Menganalisis data

Anda bisa berhitung!

```bash
 | paste -sd+ | bc -l
```

```bash
echo "2*($(data | paste -sd+))" | bc -l
```

Anda bisa mendapatkan statistik dengan berbagai cara. [`st`](https://github.com/nferraz/st) cukup keren, tapi jika Anda sudah punya R:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | awk '{print $1}' | R --slave -e 'x <- scan(file="stdin", quiet=TRUE); summary(x)'
```

R adalah bahasa pemrograman (unik) lain yang hebat untuk analisis data dan [plotting](https://ggplot2.tidyverse.org/). Kita tidak bahas detail, cukup katakan `summary` mencetak statistik ringkas tentang matriks, dan kita menghitung matriks dari stream input angka, jadi R memberi statistik yang kita mau!

Jika hanya ingin plotting sederhana, `gnuplot` teman Anda:

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | gnuplot -p -e 'set boxwidth 0.5; plot "-" using 1:xtic(2) with boxes'
```

## Pengolahan data untuk membuat argumen

Kadang Anda ingin melakukan pengolahan data untuk mencari hal yang perlu dipasang atau dihapus berdasarkan daftar panjang. Pengolahan data sejauh ini + `xargs` bisa jadi kombo kuat:

```bash
rustup toolchain list | grep nightly | grep -vE "nightly-x86|01-17" | sed 's/-x86.*//' | xargs rustup toolchain uninstall
```

# Latihan

1. Jika Anda belum familiar dengan Regex, [di sini](https://regexone.com/) ada tutorial interaktif singkat yang mencakup dasar-dasar.
1. Bagaimana `sed s/REGEX/SUBSTITUTION/g` berbeda dari sed biasa? Bagaimana dengan `/I` atau `/m`?
1. Untuk substitusi in-place menggoda untuk menulis `sed s/REGEX/SUBSTITUTION/ input.txt > input.txt`. Tapi ini ide buruk, kenapa? Apakah ini khusus untuk `sed`?
1. Implementasikan alat sederhana setara grep dalam bahasa yang Anda kuasai menggunakan regex. Jika ingin output berwarna seperti grep, cari urutan escape warna ANSI.
1. Kadang operasi seperti mengganti nama berkas bisa rumit dengan perintah mentah seperti `mv`. `rename` adalah alat praktis dengan sintaks mirip sed. Coba buat banyak berkas dengan spasi di namanya dan gunakan `rename` untuk mengganti spasi dengan underscore.
1. Cari pesan boot yang _tidak_ sama di tiga boot terakhir Anda (lihat flag `-b` `journalctl`). Anda mungkin ingin menggabungkan semua log boot ke satu berkas agar lebih mudah.
1. Hasilkan statistik waktu boot sistem Anda selama sepuluh boot terakhir menggunakan stempel waktu log pesan
   ```
   Logs begin at ...
   ```
   dan
   ```
   systemd[577]: Startup finished in ...
   ```
1. Temukan jumlah kata (di `/usr/share/dict/words`) yang mengandung setidaknya tiga `a` dan tidak berakhiran `'s`. Apa tiga dua huruf terakhir paling umum dari kata-kata itu? Perintah `y` di `sed`, atau program `tr`, bisa membantu untuk mengabaikan huruf besar/kecil. Ada berapa kombinasi dua huruf itu? Tantangan: kombinasi mana yang tidak muncul?
1. Temukan dataset online seperti [ini](https://commons.wikimedia.org/wiki/Data:Wikipedia_statistics/data.tab) atau [ini](https://ucr.fbi.gov/crime-in-the-u.s/2016/crime-in-the-u.s.-2016/topic-pages/tables/table-1). Mungkin yang lain [dari sini](https://www.springboard.com/blog/data-science/free-public-data-sets-data-science-project/). Ambil menggunakan `curl` dan ekstrak dua kolom data numerik. Jika mengambil data HTML, [`pup`](https://github.com/EricChiang/pup) mungkin membantu. Untuk JSON, coba [`jq`](https://stedolan.github.io/jq/). Temukan min dan maks salah satu kolom dalam satu perintah, dan jumlah selisih antara kedua kolom dalam perintah lain.
