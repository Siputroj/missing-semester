---
layout: lecture
title: "Mesin Jarak Jauh"
presenter: Jose
video:
  aspect: 62.5
  id: X5c2Y8BCowM
---

Semakin umum bagi pemrogram menggunakan server jarak jauh dalam pekerjaan
sehari-hari. Jika kamu perlu server jarak jauh untuk men-deploy perangkat lunak
backend atau butuh server dengan kemampuan komputasi lebih tinggi, kamu akan
berakhir memakai Secure Shell (SSH). Seperti banyak alat lain, SSH sangat
dapat dikonfigurasi jadi layak untuk dipelajari.


## Menjalankan perintah

Fitur `ssh` yang sering diabaikan adalah kemampuan menjalankan perintah secara
langsung.

- `ssh foobar@server ls` akan mengeksekusi ls di folder home foobar
- Bekerja dengan pipe, sehingga `ssh foobar@server ls | grep PATTERN` akan
  melakukan grep secara lokal terhadap keluaran `ls` di remote dan `ls | ssh
  foobar@server grep PATTERN` akan melakukan grep secara remote terhadap
  keluaran `ls` di lokal.

## Kunci SSH

Autentikasi berbasis kunci memanfaatkan kriptografi kunci publik untuk membuktikan
ke server bahwa klien memiliki kunci privat rahasia tanpa mengungkapkan kunci
tersebut. Dengan begitu kamu tidak perlu memasukkan kata sandi setiap saat.
Meski begitu kunci privat (mis. `~/.ssh/id_rsa`) pada dasarnya adalah kata
sandimu, jadi perlakukan seperti itu.

- Pembuatan kunci. Untuk membuat sepasang kunci cukup jalankan `ssh-keygen -t
  rsa -b 4096`. Jika tidak memilih passphrase, siapa pun yang mendapatkan kunci
  privatmu bisa mengakses server yang diizinkan, jadi disarankan memilih
  passphrase dan memakai `ssh-agent` untuk mengelola sesi shell.

Jika sudah mengonfigurasi push ke Github menggunakan kunci SSH kamu mungkin
telah melakukan langkah-langkah yang diuraikan
[di sini](https://help.github.com/articles/connecting-to-github-with-ssh/) dan
sudah memiliki sepasang kunci. Untuk memeriksa apakah kamu memiliki passphrase
dan memvalidasinya jalankan `ssh-keygen -y -f /path/to/key`.

- Autentikasi berbasis kunci. `ssh` akan melihat ke `.ssh/authorized_keys` untuk
  menentukan klien mana yang boleh masuk. Untuk menyalin kunci publik kita bisa
  gunakan

```bash
cat .ssh/id_dsa.pub | ssh foobar@remote 'cat >> ~/.ssh/authorized_keys'
```

Solusi lebih sederhana bisa dicapai dengan `ssh-copy-id` jika tersedia.

```bash
ssh-copy-id -i .ssh/id_dsa.pub foobar@remote
```

## Menyalin file melalui ssh

Ada banyak cara menyalin file melalui ssh

- `ssh+tee`, cara paling sederhana adalah memakai eksekusi perintah `ssh` dan
  input stdin dengan `cat localfile | ssh remote_server tee serverfile`
- `scp` ketika menyalin banyak file/direktori, secure copy `scp` lebih nyaman
  karena bisa merekursi jalur dengan mudah. Sintaksnya `scp path/to/local_file
  remote_host:path/to/remote_file`
- `rsync` lebih baik dari `scp` dengan mendeteksi file identik di lokal dan
  remote sehingga tidak disalin lagi. Ia juga memberi kontrol lebih detail atas
  symlink, izin, dan memiliki fitur tambahan seperti flag `--partial` yang bisa
  melanjutkan dari salinan yang sebelumnya terputus. `rsync` memiliki sintaks
  mirip `scp`.


## Mem-background proses

Secara bawaan ketika koneksi ssh terputus, proses anak dari shell induk ikut
mati. Ada beberapa alternatif

- `nohup` - alat `nohup` membuat proses tetap hidup ketika terminal mati.
  Walaupun kadang bisa dicapai dengan `&` dan `disown`, nohup adalah default
  yang lebih baik. Detail lebih lanjut
  [di sini](https://unix.stackexchange.com/questions/3886/difference-between-nohup-disown-and).

- `tmux`, `screen` - sementara `nohup` benar-benar mem-background proses, ini
  tidak nyaman untuk sesi shell interaktif. Dalam kasus itu memakai multiplexer
  terminal seperti `screen` atau `tmux` menjadi pilihan yang nyaman karena kita
  bisa mudah melepas dan memasang kembali shell terkait.

Terakhir, jika kamu mendisown suatu program dan ingin memasangnya kembali ke
terminal saat ini, coba [reptyr](https://github.com/nelhage/reptyr). `reptyr
PID` akan mengambil proses dengan id PID dan menempelkannya ke terminalmu saat
ini.

## Penerusan Port

Dalam banyak skenario kamu akan berurusan dengan perangkat lunak yang bekerja
dengan mendengarkan port di mesin. Jika ini terjadi di mesin lokal kamu bisa
saja `localhost:PORT` atau `127.0.0.1:PORT`, tetapi bagaimana dengan server
remote yang port-nya tidak tersedia langsung melalui jaringan/internet? Ini
disebut port forwarding dan ada dua jenis: Local Port Forwarding dan Remote
Port Forwarding (lihat gambar untuk detail, kredit gambar dari [post SO
ini](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)).


**Local Port Forwarding**
![Local Port Forwarding](https://i.stack.imgur.com/a28N8.png)

**Remote Port Forwarding**
![Remote Port Forwarding](https://i.stack.imgur.com/4iK3b.png)


Skenario paling umum adalah local port forwarding di mana sebuah layanan di
mesin remote mendengarkan sebuah port dan kamu ingin menghubungkan port di
mesin lokal ke port remote. Misalnya jika kita menjalankan `jupyter notebook`
di server remote yang mendengarkan port `8888`. Untuk meneruskannya ke port
lokal `9999` kita akan menjalankan `ssh -L 9999:localhost:8888
foobar@remote_server` lalu membuka `localhost:9999` di mesin lokal.

## Penerusan Grafis

Kadang meneruskan port saja tidak cukup karena kita ingin menjalankan program
berbasis GUI di server. Kamu selalu bisa memakai perangkat lunak Remote Desktop
yang mengirim seluruh Desktop Environment (mis. RealVNC, Teamviewer, dst).
Namun untuk satu alat GUI, SSH menyediakan alternatif bagus: Graphics
Forwarding.

Menggunakan flag `-X` memberi tahu SSH untuk meneruskan

Untuk penerusan X11 tepercaya bisa gunakan flag `-Y`.

Catatan akhir: agar ini berfungsi, `sshd_config` di server harus memiliki
opsi berikut

```bash
X11Forwarding yes
X11DisplayOffset 10
```

## Roaming

Masalah umum saat terhubung ke server remote adalah terputus akibat
mematikan/menidurkan komputer atau berganti jaringan. Selain itu jika memiliki
koneksi dengan latensi tinggi, memakai ssh bisa membuat frustrasi.
[Mosh](https://mosh.org/), mobile shell, memperbaiki ssh dengan memungkinkan
koneksi roaming, konektivitas terputus-putus, dan menyediakan local echo yang
cerdas.

Mosh tersedia di semua distro dan manajer paket umum. Mosh membutuhkan server
ssh yang berfungsi di server. Kamu tidak perlu menjadi superuser untuk
menginstal mosh tetapi perlu port 60000 hingga 60010 terbuka di server (biasanya
terbuka karena tidak berada di rentang privilese).

Kekurangan `mosh` adalah tidak mendukung penerusan port/grafis, jadi jika kamu
sering memakainya `mosh` kurang membantu.

## Konfigurasi SSH

#### Klien

Kita telah membahas banyak sekali argumen yang bisa kita berikan. Alternatif
yang menggoda adalah membuat alias shell seperti `alias my_serer="ssh -X -i
~/.id_rsa -L 9999:localhost:8888 foobar@remote_server"`, namun ada alternatif
yang lebih baik, menggunakan `~/.ssh/config`.

```bash
Host vm
    User foobar
    HostName 172.16.174.141
    Port 22
    IdentityFile ~/.ssh/id_rsa
    RemoteForward 9999 localhost:8888

# Konfigurasi juga bisa memakai wildcard
Host *.mit.edu
    User foobaz
```


Keuntungan tambahan memakai file `~/.ssh/config` dibanding alias adalah program
lain seperti `scp`, `rsync`, `mosh`, dst juga bisa membacanya dan mengubah
pengaturan menjadi flag yang sesuai.

Perhatikan bahwa file `~/.ssh/config` bisa dianggap dotfile, dan secara umum
tidak masalah dimasukkan bersama dotfile lainnya. Namun jika membuatnya
publik, pikirkan informasi apa yang berpotensi kamu berikan ke orang asing di
internet: alamat servermu, pengguna yang dipakai, port terbuka, dsb. Ini bisa
mempermudah beberapa jenis serangan jadi berhati-hatilah membagikan konfigurasi
SSH-mu.

Peringatan: Jangan pernah memasukkan kunci RSA-mu (`~/.ssh/id_rsa*`) ke repositori publik!

#### Sisi server

Konfigurasi sisi server biasanya ditentukan di `/etc/ssh/sshd_config`. Di sini
kamu bisa membuat perubahan seperti menonaktifkan autentikasi kata sandi,
mengubah port ssh, mengaktifkan penerusan X11, dsb. Kamu bisa menentukan
pengaturan konfigurasi per pengguna.

## Sistem Berkas Remote

Kadang praktis memasang (mount) folder remote. [sshfs](https://github.com/libfuse/sshfs)
bisa memasang folder di server remote secara lokal, lalu kamu bisa memakai
editor lokal.

## Latihan

1. Agar SSH berfungsi host harus menjalankan server SSH. Instal server SSH
   (seperti OpenSSH) di mesin virtual sehingga kamu bisa melakukan sisa latihan.
   Untuk mengetahui IP mesin jalankan perintah `ip addr` dan cari bidang inet
   (abaikan entri `127.0.0.1`, itu adalah loopback).

1. Pergi ke `~/.ssh/` dan periksa apakah kamu punya sepasang kunci SSH. Jika
   tidak, buat dengan `ssh-keygen -t rsa -b 4096`. Disarankan menggunakan
   kata sandi dan `ssh-agent`, info lebih lanjut
   [di sini](https://www.ssh.com/ssh/agent).

1. Gunakan `ssh-copy-id` untuk menyalin kunci ke mesin virtualmu. Uji bahwa
   kamu bisa ssh tanpa kata sandi. Lalu, sunting `sshd_config` di server untuk
   menonaktifkan autentikasi kata sandi dengan mengubah nilai
   `PasswordAuthentication`. Nonaktifkan login root dengan mengubah nilai
   `PermitRootLogin`.

1. Sunting `sshd_config` di server untuk mengubah port ssh dan periksa bahwa
   kamu masih bisa ssh. Jika kamu pernah memiliki server publik, port tidak
   baku dan login berbasis kunci akan mengurangi cukup banyak serangan
   berbahaya.

1. Instal mosh di server/VM-mu, buat koneksi lalu putuskan adaptor jaringan
   server/VM. Bisakah mosh memulihkannya dengan baik?

1. Penggunaan lain local port forwarding adalah menyalurkan host tertentu ke
   server. Jika jaringanmu memblokir situs seperti `reddit.com` kamu bisa
   menyalurkannya melalui server sebagai berikut:

    - Jalankan `ssh remote_server -L 80:reddit.com:80`
    - Atur `reddit.com` dan `www.reddit.com` ke `127.0.0.1` di `/etc/hosts`
    - Periksa bahwa kamu mengakses situs itu melalui server
    - Jika tidak jelas gunakan situs seperti [ipinfo.io](https://ipinfo.io/)
      yang akan berubah tergantung IP publik host-mu.


1. Penerusan port di background bisa dicapai dengan beberapa flag tambahan.
   Cari tahu apa yang dilakukan flag `-N` dan `-f` di `ssh` dan pahami perintah
   seperti `ssh -N -f -L 9999:localhost:8888 foobar@remote_server`.


## Referensi

- [SSH Hacks](http://matt.might.net/articles/ssh-hacks/)
- [Secure Secure Shell](https://stribika.github.io/2015/01/04/secure-secure-shell.html)

{% comment %}
Lecture notes will be available by the start of lecture.
{% endcomment %}
