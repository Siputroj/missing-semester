---
layout: lecture
title: "Mengapa kami mengajar kelas ini"
---

Selama pendidikan Ilmu Komputer tradisional, kemungkinan Anda akan mengambil banyak kelas yang mengajarkan topik-topik lanjutan dalam CS, mulai dari Sistem Operasi hingga Bahasa Pemrograman hingga Machine Learning. Namun di banyak institusi ada satu topik esensial yang jarang dibahas dan justru dibiarkan untuk dipelajari sendiri oleh mahasiswa: literasi ekosistem komputasi.

Selama bertahun-tahun kami membantu mengajar beberapa kelas di MIT, dan berulang kali kami melihat banyak mahasiswa memiliki pengetahuan terbatas tentang alat yang tersedia bagi mereka. Komputer dibangun untuk mengotomatisasi tugas manual, tetapi mahasiswa sering kali melakukan pekerjaan berulang secara manual atau gagal memanfaatkan sepenuhnya alat hebat seperti kontrol versi dan editor teks. Dalam kasus terbaik hal ini hanya menyebabkan inefisiensi dan waktu terbuang; dalam kasus terburuk, dapat menyebabkan kehilangan data atau ketidakmampuan menyelesaikan tugas tertentu.

Topik-topik ini tidak diajarkan sebagai bagian dari kurikulum universitas: mahasiswa tidak pernah ditunjukkan cara menggunakan alat-alat ini, atau setidaknya tidak diajarkan cara menggunakannya secara efisien, sehingga mereka membuang waktu dan tenaga pada tugas yang _seharusnya_ sederhana. Kurikulum CS standar kehilangan topik-topik penting tentang ekosistem komputasi yang dapat membuat hidup mahasiswa jauh lebih mudah.

# Semester yang hilang dari pendidikan CS Anda

Untuk membantu mengatasinya, kami membuat kelas yang mencakup semua topik yang kami anggap krusial untuk menjadi ilmuwan komputer dan programmer yang efektif. Kelas ini pragmatis dan praktis, serta memberikan pengenalan langsung terhadap alat dan teknik yang dapat segera Anda terapkan di berbagai situasi yang akan Anda temui. Iterasi terbaru kelas ini, dengan materi yang direvisi secara substansial, diselenggarakan selama "Independent Activities Period" MIT pada Januari 2026 — semester satu bulan yang berisi kelas singkat yang dijalankan mahasiswa. Meski kuliahnya sendiri hanya tersedia untuk komunitas MIT, kami akan menyediakan seluruh materi kuliah beserta rekaman videonya untuk publik.

Jika terdengar cocok untuk Anda, berikut beberapa contoh konkret tentang apa yang akan diajarkan kelas ini:

## Command shell

Cara mengotomatisasi tugas-tugas umum dan berulang dengan alias, skrip, dan sistem build. Tidak ada lagi copy-paste perintah dari dokumen teks. Tidak ada lagi "jalankan 15 perintah ini secara berurutan". Tidak ada lagi "kamu lupa menjalankan ini" atau "kamu lupa memberi argumen ini".

Sebagai contoh, menelusuri riwayat perintah dengan cepat bisa menghemat banyak waktu. Pada contoh di bawah, kami menunjukkan beberapa trik terkait menavigasi riwayat shell untuk perintah `convert`.

<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/history.mp4" type="video/mp4">
</video>

## Kontrol versi

Cara menggunakan kontrol versi _dengan benar_, dan memanfaatkannya untuk menyelamatkan diri dari bencana, berkolaborasi dengan orang lain, serta cepat menemukan dan mengisolasi perubahan bermasalah. Tidak ada lagi `rm -rf; git clone`. Tidak ada lagi konflik merge (yah, setidaknya lebih sedikit). Tidak ada lagi blok kode besar yang dikomentari. Tidak ada lagi panik mencari apa yang merusak kode Anda. Kami bahkan akan mengajarkan cara berkontribusi ke proyek orang lain dengan pull request!

Pada contoh di bawah kami menggunakan `git bisect` untuk menemukan commit mana yang merusak sebuah unit test lalu memperbaikinya dengan `git revert`.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/git.mp4" type="video/mp4">
</video>

## Penyuntingan teks

Cara mengedit berkas dari command-line secara efisien, baik lokal maupun jarak jauh, serta memanfaatkan fitur editor tingkat lanjut. Tidak ada lagi bolak-balik menyalin berkas. Tidak ada lagi pengeditan berkas yang berulang.

Makro Vim adalah salah satu fitur terbaiknya; pada contoh di bawah kami dengan cepat mengonversi tabel HTML menjadi format CSV menggunakan makro Vim bersarang.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/vim.mp4" type="video/mp4">
</video>

## Mesin jarak jauh

Cara tetap waras saat bekerja dengan mesin jarak jauh menggunakan kunci SSH dan terminal multiplexing. Tidak perlu lagi membuka banyak terminal hanya untuk menjalankan dua perintah sekaligus. Tidak perlu lagi mengetik kata sandi setiap kali terhubung. Tidak perlu lagi kehilangan semuanya hanya karena koneksi Internet terputus atau Anda harus me-reboot laptop.

Pada contoh di bawah kami menggunakan `tmux` untuk menjaga sesi tetap hidup di server jarak jauh dan `mosh` untuk mendukung roaming jaringan dan pemutusan sambungan.

<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/ssh.mp4" type="video/mp4">
</video>

## Mencari berkas

Cara cepat menemukan berkas yang Anda cari. Tidak perlu lagi mengklik-klik berkas dalam proyek hingga menemukan kode yang Anda inginkan.

Pada contoh di bawah kami dengan cepat mencari berkas menggunakan `fd` dan potongan kode dengan `rg`. Kami juga cepat `cd` dan `vim` berkas/folder terbaru/sering dipakai menggunakan `fasd`.

<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/find.mp4" type="video/mp4">
</video>

## Pengolahan data

Cara dengan cepat dan mudah memodifikasi, melihat, mengurai, memplot, dan menghitung data serta berkas langsung dari command-line. Tidak perlu lagi copy-paste dari log. Tidak perlu lagi menghitung statistik data secara manual. Tidak perlu lagi membuat plot di spreadsheet.

## Kualitas kode dan integrasi berkelanjutan

Cara menggunakan autoformatting, linting, testing, dan alat cakupan kode untuk meningkatkan kualitas kode. Tidak ada lagi kode berantakan. Tidak ada lagi regresi. Tidak ada lagi kode yang berjalan di komputer Anda tapi crash di komputer orang lain.

## Di luar kode

Cara menulis dokumentasi yang hebat, berkomunikasi jelas dengan maintainer open-source, mengirim issue yang dapat ditindaklanjuti, dan berkontribusi pull request yang diterima. Tidak ada lagi pengguna bingung yang tidak bisa memulai menggunakan perangkat lunak Anda. Tidak ada lagi diabaikan oleh maintainer.

# Kesimpulan

Semua ini, dan lebih banyak lagi, akan dibahas dalam 9 kuliah kelas, masing-masing disertai latihan untuk membuat Anda lebih akrab dengan alat-alat tersebut. Jika Anda tidak sabar menunggu hingga Januari 2026, Anda juga dapat melihat kuliah dari [penyelenggaraan sebelumnya](/2020/), yang membahas banyak topik serupa.

Kami harap bertemu Anda Januari nanti, baik secara virtual maupun langsung!

Selamat hacking,<br>
Anish, Jose, dan Jon
