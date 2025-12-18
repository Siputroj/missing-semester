---
layout: lecture
title: "Introspeksi Mesin"
presenter: Jon
video:
  aspect: 56.25
  id: eNYT2Oq3PF8
---

Terkadang, komputer berperilaku aneh. Dan sangat sering, kamu ingin tahu
alasannya. Mari lihat beberapa alat yang membantumu melakukan itu!

Tapi pertama-tama, pastikan kamu bisa melakukan introspeksi. Sering kali,
introspeksi sistem memerlukan hak tertentu, seperti menjadi anggota grup
(misalnya `power` untuk mematikan). Pengguna `root` punya hak istimewa penuh;
ia bisa melakukan hampir apa saja. Kamu dapat menjalankan perintah sebagai
`root` (hati-hati!) menggunakan `sudo`.

## Apa yang terjadi?

Jika ada yang salah, langkah pertama adalah melihat apa yang terjadi di sekitar
Waktu masalah muncul. Untuk itu, kita perlu melihat log.

Secara tradisional, log disimpan di `/var/log`, dan banyak yang masih di sana.
Biasanya ada satu file atau folder per program. Gunakan `grep` atau `less`
untuk menelusurinya.

Ada juga log kernel yang bisa dilihat dengan perintah `dmesg`. Dahulu ini
tersedia sebagai file teks biasa, tetapi kini kamu sering harus memakai
`dmesg` untuk mengaksesnya.

Akhirnya, ada "log sistem" yang semakin menjadi tempat semua pesan log
berada. Pada _kebanyakan_, meski tidak semua, sistem Linux, log itu dikelola
oleh `systemd`, "daemon sistem" yang mengontrol semua layanan yang berjalan di
latar belakang (dan lebih banyak lagi sekarang). Log itu dapat diakses melalui
`journalctl` (agak canggung) jika kamu adalah root, atau bagian dari grup
`admin` atau `wheel`.

Untuk `journalctl`, perhatikan flag berikut:

 - `-u UNIT`: hanya tampilkan pesan terkait layanan systemd tersebut
 - `--full`: jangan memotong baris panjang (fitur paling menjengkelkan)
 - `-b`: hanya tampilkan pesan dari boot terakhir (lihat juga `-b -2`)
 - `-n100`: hanya tampilkan 100 entri terakhir

## Apa yang sedang terjadi?

Jika memang _ada_ yang salah, atau kamu hanya ingin memahami apa yang sedang
terjadi di sistemmu, ada sejumlah alat untuk memeriksa sistem yang sedang
berjalan:

Pertama, ada `top`, dan versi yang lebih baik `htop`, yang menampilkan berbagai
statistik untuk proses yang sedang berjalan di sistem. Penggunaan CPU,
penggunaan memori, pohon proses, dll. Ada banyak pintasan, tetapi `t` sangat
berguna untuk mengaktifkan tampilan pohon. Kamu juga dapat melihat pohon
proses dengan `pstree` (+ `-p` untuk menyertakan PID). Jika ingin tahu apa
yang dilakukan program-program itu, sering kali kamu ingin mengamati log
file-nya. `journalctl -f`, `dmesg -w`, dan `tail -f` adalah temanmu di sini.

Kadang, kamu ingin tahu lebih banyak tentang sumber daya yang digunakan secara
keseluruhan di sistem. [`dstat`](http://dag.wiee.rs/home-made/dstat/) sangat
bagus untuk itu. Ia memberikan metrik sumber daya real-time untuk banyak
subsistem berbeda seperti I/O, jaringan, pemakaian CPU, context switch, dan
sebagainya. `man dstat` adalah tempat memulai.

Jika kehabisan ruang disk, ada dua utilitas utama yang perlu kamu ketahui: `df`
dan `du`. Yang pertama menampilkan status semua partisi di sistemmu (coba
dengan `-h`), sedangkan yang kedua mengukur ukuran semua folder yang kamu
berikan, termasuk isinya (lihat juga `-h` dan `-s`).

Untuk mengetahui koneksi jaringan apa saja yang terbuka, gunakan `ss`.
`ss -t` akan menampilkan semua koneksi TCP yang terbuka. `ss -tl` akan
menampilkan semua port yang mendengarkan (mis. server) di sistemmu. `-p` juga
akan menyertakan proses mana yang memakai koneksi tersebut, dan `-n` akan
menunjukkan nomor port mentah.


## Konfigurasi sistem

Ada _banyak_ cara mengonfigurasi sistem, tetapi kita akan membahas dua yang
sangat umum: jaringan dan layanan. Kebanyakan aplikasi di sistemmu memberi
tahu cara mengonfigurasinya di halaman manualnya, dan biasanya melibatkan
pengeditan file di `/etc`; direktori konfigurasi sistem.

Jika ingin mengonfigurasi jaringan, perintah `ip` memungkinkannya. Argumennya
berbentuk agak aneh, tetapi `ip help command` sudah cukup membantu. `ip addr`
menunjukkan informasi tentang antarmuka jaringan dan konfigurasinya (alamat IP
dll), dan `ip route` menunjukkan cara lalu lintas jaringan dirutekan ke host
jaringan yang berbeda. Masalah jaringan sering kali bisa diselesaikan hanya
dengan alat `ip`. Ada juga `iw` untuk mengelola antarmuka jaringan nirkabel.
`ping` adalah alat berguna untuk memeriksa seberapa parah masalahnya. Coba
ping nama host (google.com), alamat IP eksternal (1.1.1.1), dan alamat IP
internal (192.168.1.1 atau gateway bawaan). Kamu mungkin juga ingin mengutak-
atik `/etc/resolv.conf` untuk memeriksa pengaturan DNS-mu (bagaimana nama host
diubah menjadi alamat IP).

Untuk mengonfigurasi layanan, saat ini kamu hampir selalu harus berinteraksi
dengan `systemd`, suka atau tidak. Sebagian besar layanan di sistemmu akan
memiliki file layanan systemd yang mendefinisikan _unit_ systemd. File ini
mendefinisikan perintah apa yang dijalankan saat layanan dimulai, cara
menghentikannya, ke mana mencatat log, dll. File-file ini biasanya cukup mudah
dibaca, dan bisa ditemukan di `/usr/lib/systemd/system/`. Kamu juga dapat
mendefinisikan milikmu sendiri di `/etc/systemd/system` .

Setelah unit systemd yang dimaksud, gunakan perintah `systemctl` untuk
berinteraksi dengannya. `systemctl enable UNIT` akan membuat layanan mulai saat
boot (`disable` menghapusnya lagi), dan `start`, `stop`, serta `restart` akan
melakukan sesuai namanya. Jika ada yang salah, systemd akan memberi tahu, dan
kamu bisa memakai `journalctl -u UNIT` untuk melihat log aplikasinya. Kamu juga
bisa memakai `systemctl status` untuk melihat kondisi semua layanan sistem.
Jika proses boot terasa lambat, mungkin karena beberapa layanan lambat, dan
kamu bisa menggunakan `systemd-analyze` (coba dengan `blame`) untuk mencari
tahu yang mana.

# Latihan

`locate`?
`dmidecode`?
`tcpdump`?
`/boot`?
`iptables`?
`/proc`?
