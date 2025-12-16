---
layout: lecture
title: "Shell dan Scripting"
presenter: Jon
video:
  aspect: 56.25
  id: dbDRfmH5uSI
---

Shell adalah antarmuka tekstual yang efisien ke komputer Anda.

Prompt shell adalah yang menyambut Anda saat membuka terminal.
Di situ Anda menjalankan program dan perintah; yang umum:

 - `cd` untuk mengganti direktori
 - `ls` untuk menampilkan berkas dan direktori
 - `mv` dan `cp` untuk memindah serta menyalin berkas

Namun shell memungkinkan _jauh_ lebih banyak; Anda bisa memanggil program apa pun di komputer, dan alat command-line tersedia untuk hampir semua hal yang ingin Anda lakukan—sering kali lebih efisien daripada antarmuka grafis. Kita akan membahas banyak di kelas ini.

Shell juga menyediakan bahasa pemrograman interaktif ("scripting"). Ada banyak shell:

 - Anda mungkin pernah memakai `sh` atau `bash`.
 - Ada shell yang menyamai bahasa tertentu: `csh`.
 - Atau shell yang dianggap "lebih baik": `fish`, `zsh`, `ksh`.

Di kelas ini kita fokus pada `sh` dan `bash` yang ubiquitous, tapi silakan bereksperimen dengan yang lain. Saya suka `fish`.

Pemrograman shell adalah alat yang *sangat* berguna. Anda bisa menulis program langsung di prompt atau di berkas. Gunakan `#!/bin/sh` + `chmod +x` untuk membuat skrip shell dapat dieksekusi.

## Bekerja dengan shell

Menjalankan perintah berulang:

```bash
for i in $(seq 1 5); do echo hello; done
```

Banyak hal yang perlu diurai:

 - `for x in list; do BODY; done`
   - `;` mengakhiri perintah—setara dengan baris baru
   - membagi `list`, menugaskan tiap entri ke `x`, lalu menjalankan BODY
   - pembagiannya berdasarkan whitespace (akan dibahas lagi)
   - tidak ada kurung kurawal di shell, jadi pakai `do` + `done`
 - `$(seq 1 5)`
   - jalankan program `seq` dengan argumen `1` dan `5`
   - ganti seluruh `$()` dengan keluaran program
   - setara dengan
     ```bash
     for i in 1 2 3 4 5
     ```
 - `echo hello`
   - semuanya dalam skrip shell adalah perintah
   - di sini menjalankan perintah `echo` dengan argumen `hello`
   - semua perintah dicari di `$PATH` (dipisah kolon)

Kita punya variabel:
```bash
for f in $(ls); do echo $f; done
```

Ini mencetak setiap nama berkas di direktori saat ini.
Variabel diset dengan `=` (tanpa spasi!):

```bash
foo=bar
echo $foo
```

Ada juga banyak variabel khusus:

 - `$1` sampai `$9`: argumen skrip
 - `$0` nama skrip itu sendiri
 - `$#` jumlah argumen
 - `$$` ID proses shell saat ini

Untuk hanya mencetak direktori:

```bash
for f in $(ls); do if test -d $f; then echo dir $f; fi; done
```

Penjelasan:

 - `if CONDITION; then BODY; fi`
   - `CONDITION` adalah perintah; jika keluarannya status 0 (sukses), maka `BODY` dijalankan.
   - bisa ada `else` atau `elif`
   - lagi-lagi tidak ada kurung kurawal, jadi `then` + `fi`
 - `test` adalah program lain yang menyediakan berbagai pengecekan dan perbandingan, keluar dengan 0 jika benar (`$?`)
   - `man COMMAND` adalah teman Anda: `man test`
   - juga bisa dipanggil dengan `[` + `]`: `[ -d $f ]`
     - lihat `man test` dan `which "["`

Tapi tunggu! Ini salah! Bagaimana jika ada berkas bernama "My Documents"?

 - `for f in $(ls)` mengembang menjadi `for f in My Documents`
 - pertama mengetes `My`, lalu `Documents`
 - bukan yang kita mau!
 - sumber bug terbesar di skrip shell

## Pemisahan argumen

Bash memisah argumen berdasarkan whitespace; tidak selalu diinginkan!

 - perlu memakai kutip untuk menangani spasi dalam argumen  
   `for f in "My Documents"` akan bekerja benar
 - masalah yang sama di tempat lain—lihat di mana?  
   `test -d $f`: jika `$f` mengandung spasi, `test` akan error!
 - `echo` kebetulan aman karena split + join spasi, tapi bagaimana jika nama berkas mengandung newline?! menjadi spasi!
 - kutip semua penggunaan variabel yang tidak ingin di-split
 - tapi bagaimana memperbaiki skrip tadi?  
   apa yang dilakukan `for f in "$(ls)"` menurut Anda?

Globbing adalah jawabannya!

 - bash tahu cara mencari berkas dengan pola:
   - `*` sebarang rangkaian karakter
   - `?` sebarang satu karakter
   - `{a,b,c}` salah satu dari karakter ini
 - `for f in *`: semua berkas di direktori ini
 - saat globbing, setiap berkas yang cocok jadi argumen sendiri
   - tetap pastikan mengutip saat *memakai*: `test -d "$f"`
 - bisa membuat pola lebih lanjut:
   - `for f in a*`: semua berkas berawalan `a` di direktori saat ini
   - `for f in foo/*.txt`: semua berkas `.txt` di `foo`
   - `for f in foo/*/p??.txt`  
     semua berkas teks tiga huruf berawalan p di subdir `foo`

Masalah whitespace tidak berhenti di sana:

 - `if [ $foo = "bar" ]; then` — lihat masalahnya?
 - bagaimana jika `$foo` kosong? argumen untuk `[` adalah `=` dan `bar`...
 - _bisa_ diakali dengan `[ x$foo = "xbar" ]`, tapi meh
 - gunakan `[[`: komparator bawaan bash dengan parsing khusus  
   - juga mendukung `&&` alih-alih `-a`, `||` alih-alih `-o`, dsb.

<!-- TODO: arrays? $@. ${array[@]} vs "${array[@]}". -->

## Komposabilitas

Shell kuat karena komposabilitas. Anda bisa merangkai beberapa program ketimbang satu program melakukan semuanya.

Karakter kuncinya `|` (pipe).

 - `a | b` berarti jalankan `a` dan `b`, kirim seluruh keluaran `a` sebagai masukan `b`, lalu cetak keluaran `b`

Setiap program yang Anda jalankan ("proses") punya tiga "stream":

 - `STDIN`: input program dibaca dari sini
 - `STDOUT`: keluaran program ke sini
 - `STDERR`: keluaran kedua yang dapat dipakai program
 - secara bawaan, `STDIN` adalah keyboard, `STDOUT` dan `STDERR` adalah terminal Anda. tapi Anda bisa mengubahnya!
   - `a | b` membuat `STDOUT` `a` menjadi `STDIN` `b`.
   - ada juga:
     - `a > foo` (`STDOUT` `a` ke berkas `foo`)
     - `a 2> foo` (`STDERR` `a` ke berkas `foo`)
     - `a < foo` (`STDIN` `a` dibaca dari berkas `foo`)
     - tips: `tail -f` akan mencetak berkas saat sedang ditulis
 - kenapa ini berguna? memungkinkan Anda memanipulasi keluaran program!
   - `ls | grep foo`: semua berkas yang mengandung kata `foo`
   - `ps | grep foo`: semua proses yang mengandung kata `foo`
   - `journalctl | grep -i intel | tail -n5`:
     5 pesan log sistem terakhir yang mengandung intel (case-insensitive)
   - `who | sendmail -t me@example.com`
     kirim daftar pengguna yang login ke `me@example.com`
   - ini dasar dari banyak pengolahan data, yang akan kita bahas nanti

Bash juga menyediakan sejumlah cara lain untuk mengomposisi program.

Anda dapat mengelompokkan perintah dengan `(a; b) | tac`: jalankan `a`, lalu `b`, dan kirim semua keluarannya ke `tac` yang mencetak input secara terbalik.

Fitur kurang dikenal tetapi sangat berguna adalah *process substitution*. `b <(a)` akan menjalankan `a`, membuat nama berkas sementara untuk stream keluarannya, dan memberikan nama berkas itu ke `b`. Contoh:

```bash
diff <(journalctl -b -1 | head -n20) <(journalctl -b -2 | head -n20)
```
menunjukkan perbedaan 20 baris pertama log boot terakhir dan sebelumnya.

<!-- TODO: exit codes? -->

## Kontrol job dan proses

Bagaimana jika ingin menjalankan proses jangka panjang di latar?

 - akhiran `&` menjalankan program "di latar belakang"
   - segera mengembalikan prompt
   - berguna jika ingin menjalankan dua program sekaligus, misalnya server dan klien: `server & client`
   - ingat program yang berjalan masih memakai terminal Anda sebagai `STDOUT`! coba: `server > server.log & client`
 - lihat semua proses semacam itu dengan `jobs`
   - perhatikan status "Running"
 - bawa ke depan dengan `fg %JOB` (tanpa argumen berarti yang terakhir)
 - jika ingin memindahkan program aktif ke latar: `^Z` + `bg` (di sini `^Z` berarti Ctrl+Z)
   - `^Z` menghentikan proses saat ini dan menjadikannya "job"
   - `bg` menjalankan job terakhir di latar (seperti menambah `&`)
 - job di latar tetap terikat sesi sekarang, dan keluar jika Anda logout. `disown` memutuskan koneksi itu. atau pakai `nohup`.
 - `$!` adalah pid proses latar terakhir

<!-- TODO: process output control (^S and ^Q)? -->

Bagaimana dengan hal lain yang berjalan di komputer?

 - `ps` adalah teman Anda: menampilkan proses berjalan
   - `ps -A`: cetak proses dari semua pengguna (juga `ps ax`)
   - `ps` punya *banyak* argumen: lihat `man ps`
 - `pgrep`: cari proses lewat pencarian (seperti `ps -A | grep`)
   - `pgrep -af`: cari dan tampilkan beserta argumen
 - `kill`: kirim _signal_ ke proses berdasarkan ID (`pkill` berdasar pencarian + `-f`)
   - sinyal memberi tahu proses untuk "melakukan sesuatu"
   - paling umum: `SIGKILL` (`-9` atau `-KILL`): suruh keluar *sekarang* (setara `^\`)
   - juga `SIGTERM` (`-15` atau `-TERM`): minta keluar dengan baik (setara `^C`)

## Flag

Sebagian besar utilitas command-line menerima parameter menggunakan **flag**. Flag biasanya ada dalam bentuk pendek (`-h`) dan panjang (`--help`). Biasanya menjalankan `CMD -h` atau `man CMD` memberi daftar flag yang didukung.
Flag pendek biasanya dapat digabung; menjalankan `rm -r -f` setara dengan `rm -rf` atau `rm -fr`.
Beberapa flag umum menjadi standar de facto dan sering Anda lihat:

* `-a` biasanya berarti semua berkas (termasuk yang diawali titik)
* `-f` biasanya memaksa sesuatu, seperti `rm -f`
* `-h` menampilkan bantuan untuk sebagian besar perintah
* `-v` biasanya mengaktifkan keluaran verbose
* `-V` biasanya mencetak versi perintah

Tanda double dash `--` digunakan di perintah bawaan dan banyak perintah lain untuk menandakan akhir opsi; setelah itu hanya argumen posisi diterima. Jadi jika Anda punya berkas bernama `-v` (bisa saja) dan ingin meng-grep-nya, `grep pattern -- -v` akan bekerja sedangkan `grep pattern -v` tidak. Cara membuat berkas demikian: `touch -- -v`.

## Latihan

1. Jika Anda benar-benar baru di shell, Anda bisa membaca panduan yang lebih komprehensif seperti [BashGuide](http://mywiki.wooledge.org/BashGuide). Untuk pengantar lebih mendalam, [The Linux Command Line](http://linuxcommand.org/tlcl.php) adalah sumber yang baik.

1. **PATH, which, type**

    Kita singgung bahwa variabel lingkungan `PATH` dipakai untuk mencari program yang Anda jalankan dari command-line. Mari mengeksplorasi lebih jauh:
    - Jalankan `echo $PATH` (atau `echo $PATH | tr -s ':' '\n'` agar rapi) dan periksa isinya, lokasi apa saja yang tercantum?
    - Perintah `which` mencari program di PATH pengguna. Coba jalankan `which` untuk perintah umum seperti `echo`, `ls`, atau `mv`. Perhatikan `which` agak terbatas karena tidak memahami alias shell. Coba jalankan `type` dan `command -v` untuk perintah yang sama. Bagaimana perbedaannya?
    - Jalankan `PATH=` lalu coba ulang perintah sebelumnya; beberapa jalan dan beberapa tidak, bisakah Anda menebak alasannya?

1. **Variabel khusus**
    - `~` berkembang menjadi apa? Bagaimana dengan `.`? Dan `..`?
    - Apa fungsi variabel `$?`?
    - Apa fungsi variabel `$_`?
    - `!!` berkembang menjadi apa? Bagaimana dengan `!!*`? Dan `!l`?
    - Cari dokumentasi opsi ini dan kenali fungsinya.

1. **xargs**

    Kadang piping tidak bekerja karena perintah yang dituju tidak mengharapkan format dipisah baris. Misalnya perintah `file` memberi tahu properti berkas.

    Coba jalankan `ls | file` dan `ls | xargs file`. Apa yang dilakukan `xargs`?

1. **Shebang**

    Saat menulis skrip, Anda bisa menentukan interpreter yang dipakai dengan baris [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)). Tulis skrip bernama `hello` dengan isi berikut, buat dapat dieksekusi dengan `chmod +x hello`, lalu jalankan `./hello`. Kemudian hapus baris pertama dan jalankan lagi. Bagaimana shell menggunakan baris pertama itu?

    ```bash
      #! /usr/bin/python

      print("Hello World!")
    ```

    Anda sering melihat shebang seperti `#! usr/bin/env bash`. Ini solusi lebih portabel dengan [kelebihan dan kekurangan](https://unix.stackexchange.com/questions/29608/why-is-it-better-to-use-usr-bin-env-name-instead-of-path-to-name-as-my) tersendiri. Bagaimana `env` berbeda dari `which`? Variabel lingkungan apa yang dipakai `env` untuk memutuskan program apa yang dijalankan?

1. **Pipes, process substitution, subshell**

    Buat skrip `slow_seq.sh` dengan isi berikut lalu `chmod +x slow_seq.sh`:

    ```bash
      #! /usr/bin/env bash

      for i in $(seq 1 10); do
              echo $i;
              sleep 1;
      done
    ```

    Ada perbedaan antara pipes (dan process substitution) dengan menjalankan di subshell, yaitu `$()`. Jalankan perintah berikut dan amati perbedaannya:

    - `./slow_seq.sh | grep -P "[3-6]"`
    - `grep -P "[3-6]" <(./slow_seq.sh)`
    - `echo $(./slow_seq.sh) | grep -P "[3-6]"`

1. **Lain-lain**
    - Coba jalankan `touch {a,b}{a,b}` lalu `ls`. Apa yang muncul?
    - Kadang Anda ingin mempertahankan STDIN dan tetap menyalurkan ke berkas. Coba `echo HELLO | tee hello.txt`
    - Coba jalankan `cat hello.txt > hello.txt` apa yang Anda perkirakan? Apa yang terjadi?
    - Jalankan `echo HELLO > hello.txt` lalu `echo WORLD >> hello.txt`. Apa isi `hello.txt`? Bagaimana `>` berbeda dari `>>`?
    - Jalankan `printf "\e[38;5;81mfoo\e[0m\n"`. Mengapa keluaran berbeda? Jika ingin tahu lebih, cari urutan escape warna ANSI.
    - Jalankan `touch a.txt` lalu `^txt^log` apa yang dilakukan bash untuk Anda? Dengan semangat sama, jalankan `fc`. Apa yang dilakukannya?

{% comment %}

TODO

1. **parallel**
- set -e, set -x
- traps

{% endcomment %}

1. **Keyboard shortcut**

    Seperti aplikasi yang sering Anda pakai, ada baiknya mengenal pintasan keyboard-nya. Ketik yang berikut dan coba pahami apa yang mereka lakukan dan kapan berguna. Untuk beberapa mungkin lebih mudah mencari secara daring. (ingat `^X` berarti menekan Ctrl+X)

    - `^A`, `^E`
    - `^R`
    - `^L`
    - `^C`, `^\` dan `^D`
    - `^U` dan `^Y`
