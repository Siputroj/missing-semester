---
layout: lecture
title: "Cadangan"
presenter: Jose
video:
  aspect: 56.25
  id: lrpqYF8tcYQ
---

Ada dua tipe orang:

- Mereka yang membuat cadangan
- Mereka yang akan membuat cadangan

Data apa pun yang Anda miliki tetapi tidak dicadangkan bisa hilang kapan saja, selamanya. Di sini kita bahas dasar-dasar pencadangan yang baik dan jebakan beberapa pendekatan.

## Aturan 3-2-1

[Aturan 3-2-1](https://www.us-cert.gov/sites/default/files/publications/data_backup_options.pdf) adalah strategi umum yang direkomendasikan untuk mencadangkan data. Artinya Anda sebaiknya memiliki:

- setidaknya **3 salinan** data
- **2** salinan di **media berbeda**
- **1** salinan disimpan **di luar lokasi**

Intinya agar tidak menaruh semua telur di satu keranjang. Memiliki 2 perangkat/disk berbeda memastikan kegagalan perangkat keras tunggal tidak menghapus semua data. Demikian pula, jika satu-satunya cadangan disimpan di rumah lalu rumah terbakar atau dirampok, semuanya lenyap! Salinan offsite melindungi skenario itu. Cadangan lokal memberi ketersediaan dan kecepatan, cadangan offsite memberi ketahanan ketika bencana terjadi.

## Menguji cadangan

Kesalahan umum saat mencadangkan adalah mempercayai begitu saja pesan sistem tanpa memverifikasi bahwa data bisa dipulihkan. Toy Story 2 hampir hilang dan cadangan mereka tidak bekerja; [keberuntungan](https://www.youtube.com/watch?v=8dhp_20j0Ys) yang menyelamatkan mereka.

## Versi

Pahami bahwa [RAID](https://en.wikipedia.org/wiki/RAID) bukan cadangan, dan secara umum **mirroring bukan solusi cadangan**. Sekadar menyinkronkan berkas tidak membantu dalam skenario seperti:

- Kerusakan data
- Perangkat lunak jahat
- Menghapus berkas tanpa sengaja

Jika perubahan data ikut tersalin ke cadangan, Anda tidak bisa pulih dalam skenario itu. Ini berlaku untuk banyak penyimpanan cloud seperti Dropbox, Google Drive, One Drive, dan sejenisnya. Sebagian memang menyimpan berkas terhapus untuk waktu singkat, tetapi antarmuka pemulihannya biasanya menyebalkan untuk memulihkan banyak berkas.

Sistem cadangan yang baik seharusnya berversi untuk menghindari kegagalan ini. Dengan menyediakan snapshot pada waktu berbeda, kita bisa menelusuri dan memulihkan apa pun yang hilang. Perangkat lunak paling terkenal jenis ini adalah Time Machine di macOS.

## Deduplikasi

Namun, membuat banyak salinan data sangat mahal dari sisi ruang. Padahal dari satu versi ke versi berikutnya, sebagian besar data identik dan tidak perlu dikirim lagi. Di sinilah [deduplikasi data](https://en.wikipedia.org/wiki/Data_deduplication) berperan: dengan melacak apa yang sudah disimpan kita dapat melakukan **pencadangan inkremental** di mana hanya perubahan yang disimpan. Ini sangat mengurangi ruang yang dibutuhkan setelah salinan pertama.

## Enkripsi

Karena kita mungkin mencadangkan ke pihak ketiga yang tidak selalu dipercaya (penyedia cloud), jika data dikirim *apa adanya* bisa saja dibaca pihak tak diinginkan. Dokumen sensitif seperti pajak sebaiknya tidak dicadangkan dalam bentuk terbuka. Untuk mencegah ini, banyak solusi cadangan menawarkan **enkripsi sisi klien** di mana data dienkripsi sebelum dikirim ke server. Server tidak bisa membaca data yang disimpan, tetapi Anda bisa mendekripsinya dengan kunci rahasia.

Catatan lain: jika disk (atau partisi home) tidak dienkripsi, siapa pun yang memegang komputer bisa mengakali kontrol akses pengguna dan membaca data Anda. Perangkat keras modern mendukung baca/tulis terenkripsi dengan cepat, jadi pertimbangkan mengaktifkan **enkripsi seluruh disk**.

## Hanya-tambah

Sifat yang dibahas sejauh ini fokus pada kegagalan hardware atau kesalahan pengguna, belum menjawab jika ada pihak jahat ingin menghapus data. Jika seseorang meretas sistem Anda, bisakah mereka menghapus semua salinan data penting? Jika khawatir akan hal itu, Anda perlu solusi cadangan hanya-tambah (*append only*). Artinya ada server yang mengizinkan Anda mengirim data baru tetapi menolak menghapus data yang sudah ada. Biasanya pengguna memiliki dua kunci: kunci hanya-tambah untuk membuat cadangan baru, dan kunci akses penuh untuk menghapus cadangan lama yang sudah tidak dibutuhkan—kunci kedua disimpan offline.

Ini skenario menantang karena Anda perlu kemampuan mengubah namun tetap mencegah penghapusan data oleh penyerang. Solusi komersial yang ada antara lain [Tarsnap](https://www.tarsnap.com/) dan [Borgbase](https://www.borgbase.com/).

## Pertimbangan tambahan

Hal lain yang mungkin ingin Anda perhatikan:

- **Cadangan berkala**: cadangan usang bisa jadi tak berguna. Jadwalkan pencadangan rutin.
- **Cadangan dapat di-boot**: beberapa program memungkinkan Anda mengkloning seluruh disk. Anda punya citra lengkap sistem yang bisa langsung di-boot.
- **Strategi cadangan diferensial**: prioritas data berbeda-beda; tetapkan kebijakan cadangan berbeda untuk tiap kategori.
- **Cadangan hanya-tambah**: menerapkan kebijakan hanya-tambah pada repositori cadangan untuk mencegah penyerang menghapusnya jika mereka menguasai mesin.

## Layanan web

Tidak semua data yang Anda gunakan ada di hard disk. Jika Anda memakai **layanan web**, sebagian data penting seperti presentasi Google Docs atau playlist Spotify tersimpan daring. Contoh mudah lain yang sering terlupakan adalah email berbasis web, misalnya Gmail. Menemukan solusi cadangan di kasus ini agak rumit. Namun banyak layanan memungkinkan Anda mengunduh data, langsung atau melalui API. Ada alat seperti [gmvault](https://github.com/gaubert/gmvault) untuk Gmail guna mengunduh email ke komputer Anda.

## Halaman web

Demikian pula, banyak konten berkualitas tersedia secara daring sebagai halaman web. Jika kontennya statis, Anda bisa mencadangkannya dengan menyimpan situs beserta lampiran. Alternatif lain adalah [Wayback Machine](https://archive.org/web/), arsip digital besar web yang dikelola [Internet Archive](https://archive.org/), organisasi nirlaba yang fokus melestarikan berbagai media. Wayback Machine memungkinkan Anda menangkap dan mengarsipkan halaman, lalu mengambil snapshot yang pernah disimpan. Jika berguna, pertimbangkan untuk [berdonasi](https://archive.org/donate/) ke proyek tersebut.

## Sumber daya

Beberapa program dan layanan cadangan yang kami pakai dan bisa direkomendasikan:

- [Tarsnap](https://www.tarsnap.com/) — layanan cadangan online terenkripsi dan deduplikasi untuk yang sangat paranoid.
- [Borg Backup](https://borgbackup.readthedocs.io) — program cadangan deduplikasi yang mendukung kompresi dan enkripsi terautentikasi. Jika perlu penyedia cloud, [rsync.net](https://www.rsync.net/products/borg.html) punya penawaran khusus untuk pengguna Borg.
- [rsync](https://rsync.samba.org/) adalah utilitas transfer berkas inkremental cepat. Ini bukan solusi cadangan penuh.
- [rclone](https://rclone.org/) seperti rsync tetapi untuk penyimpanan cloud seperti Amazon S3, Dropbox, Google Drive, rsync.net, dll. Mendukung enkripsi sisi klien untuk folder jarak jauh.

## Latihan

1. Pikirkan bagaimana Anda (tidak) mencadangkan data dan perbaiki/tingkatkan.
1. Cari cara mencadangkan akun email Anda.
1. Pilih layanan web yang sering Anda pakai (Spotify, Google Music, dll.) dan cari opsi mencadangkan data Anda. Sering kali sudah ada alat buatan orang (seperti [youtube-dl](https://ytdl-org.github.io/youtube-dl/)) yang memanfaatkan API.
1. Pikirkan situs yang sudah lama Anda kunjungi berulang kali dan lihat di [archive.org](https://archive.org/web/); ada berapa versi?
1. Salah satu cara efisien menerapkan deduplikasi adalah memakai hardlink. *Symbolic link* (symlink) adalah berkas yang menunjuk ke berkas/folder lain, sedangkan hardlink adalah salinan tepat penunjuknya (menggunakan inode yang sama dan menunjuk lokasi disk yang sama). Jika berkas asli dihapus, symlink rusak tetapi hardlink tetap bekerja. Hardlink hanya untuk berkas. Cobalah perintah `ln` untuk membuat hardlink dan bandingkan dengan symlink (`ln -s`). (Di macOS Anda perlu memasang gnu coreutils atau paket hln).
