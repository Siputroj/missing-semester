---
layout: lecture
title: "Otomasi"
presenter: Jose
video:
  aspect: 56.25
  id: BaLlAaHz-1k
---

Kadang Anda menulis skrip yang melakukan sesuatu tetapi ingin skrip itu berjalan berkala, misalnya tugas pencadangan. Anda bisa saja menulis solusi *ad hoc* yang berjalan di latar belakang dan aktif secara periodik. Namun, sebagian besar sistem UNIX sudah memiliki daemon cron yang dapat menjalankan tugas dengan frekuensi hingga setiap menit berdasarkan aturan sederhana.

Pada kebanyakan sistem UNIX daemon cron, `crond`, berjalan secara bawaan, tetapi Anda bisa memeriksanya dengan `ps aux | grep crond`.

## Crontab

Berkas konfigurasi untuk cron bisa dilihat dengan `crontab -l` dan diedit dengan `crontab -e`. Format waktu yang digunakan cron terdiri atas lima kolom dipisah spasi diikuti pengguna dan perintah:

- **minute** — menit pada jam saat perintah dijalankan, antara 0–59
- **hour** — jam pada hari (format 24 jam, 0–23; 0 berarti tengah malam)
- **dom** — *day of month*, tanggal dalam bulan, misalnya 19 agar jalan setiap tanggal 19
- **month** — bulan ketika perintah dijalankan, bisa angka (0–12) atau nama bulan (mis. May)
- **dow** — *day of week*, hari dalam minggu, bisa angka (0–7) atau nama hari (mis. sun)
- **user** — pengguna yang menjalankan perintah
- **command** — perintah yang ingin dijalankan; boleh berisi banyak kata/spasi

Tanda bintang `*` berarti semua nilai, dan pola `*/n` berarti tiap nilai ke-n. Jadi `*/5` berarti setiap lima satuan. Contoh:

```shell
*/5   *    *   *   *       # Setiap lima menit
  0   *    *   *   *       # Setiap jam tepat
  0   9    *   *   *       # Setiap hari pukul 9:00 pagi
  0   9-17 *   *   *       # Setiap jam antara 9:00–17:00
  0   0    *   *   5       # Setiap Jumat pukul 00:00
  0   0    1   */2 *       # Setiap dua bulan sekali, hari pertama, pukul 00:00
```

Lebih banyak contoh jadwal crontab bisa dilihat di [crontab.guru](https://crontab.guru/examples.html).

## Lingkungan shell dan pencatatan

Kesalahan umum saat memakai cron adalah cron tidak memuat skrip lingkungan yang biasa dibaca shell seperti `.bashrc`, `.zshrc`, dan tidak mencatat keluaran ke mana pun secara bawaan. Dikombinasikan dengan frekuensi minimum satu menit, debugging skrip cron bisa menyakitkan.

Untuk urusan environment, pastikan Anda memakai path absolut di semua skrip dan mengubah variabel lingkungan seperti `PATH` agar skrip dapat berjalan. Untuk mempermudah pencatatan, tulis crontab dengan format seperti ini:

```shell
* * * * *   user  /path/to/cronscripts/every_minute.sh >> /tmp/cron_every_minute.log 2>&1
```

Tuliskan skripnya di berkas terpisah. Ingat `>>` menambahkan ke berkas dan `2>&1` mengalihkan `stderr` ke `stdout` (meski Anda mungkin ingin memisahkannya).

## Anacron

Catatan saat memakai cron: jika komputer dimatikan atau tidur ketika jadwal cron seharusnya berjalan, tugas tidak dieksekusi. Untuk tugas yang sering mungkin tidak masalah, tetapi untuk tugas yang jarang Anda mungkin ingin menjamin tetap dijalankan. [anacron](https://linux.die.net/man/8/anacron) bekerja mirip `cron` tetapi frekuensinya dalam hari. Tidak seperti cron, anacron tidak mengasumsikan mesin menyala terus-menerus, sehingga cocok untuk mesin yang tidak berjalan 24 jam namun tetap ingin menjalankan tugas harian, mingguan, atau bulanan.

## Latihan

1. Buat skrip yang setiap menit memeriksa folder Downloads untuk berkas gambar (bisa cek [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) atau pakai regex ekstensi umum) dan memindahkannya ke folder Pictures.
1. Tulis skrip cron mingguan yang memeriksa paket usang di sistem lalu meminta Anda memperbarui atau memperbaruinya otomatis.

{% comment %}

- [fswatch](https://github.com/emcrisostomo/fswatch)
- GUI automation (pyautogui) [Automating the boring stuff Chapter 18](https://automatetheboringstuff.com/chapter18/)
- Ansible/puppet/chef

- https://xkcd.com/1205/
- https://xkcd.com/1319/

{% endcomment %}
