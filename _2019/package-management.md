---
layout: lecture
title: "Manajemen Paket dan Ketergantungan"
presenter: Anish
video:
  aspect: 56.25
  id: tgvt473T8xA
---

Perangkat lunak biasanya dibangun di atas perangkat lunak lain, sehingga memerlukan manajemen ketergantungan.

Program manajemen paket/ketergantungan bersifat spesifik bahasa, tetapi banyak berbagi konsep umum.

# Repositori paket

Paket di-host di _repositori paket_. Ada repositori berbeda untuk berbagai bahasa (kadang lebih dari satu per bahasa), seperti [PyPI](https://pypi.org/) untuk Python, [RubyGems](https://rubygems.org/) untuk Ruby, dan [crates.io](https://crates.io/) untuk Rust. Repositori ini menyimpan perangkat lunak (kode sumber dan kadang biner pra-kompilasi untuk platform tertentu) untuk semua versi paket.

# Penomoran versi semantik

Perangkat lunak berkembang dari waktu ke waktu, dan kita perlu cara merujuk versinya. Cara sederhana adalah memakai nomor urut atau hash commit, tetapi kita bisa berkomunikasi lebih banyak informasi dengan nomor versi.

Ada banyak pendekatan; salah satu yang populer adalah [Semantic Versioning](https://semver.org/):

```
x.y.z
^ ^ ^
| | +- patch
| +--- minor
+----- major
```

Naikkan versi **major** saat Anda membuat perubahan API yang tidak kompatibel.

Naikkan versi **minor** saat Anda menambah fungsionalitas dengan tetap kompatibel mundur.

Naikkan versi **patch** saat memperbaiki bug yang kompatibel mundur.

Contoh, jika Anda bergantung pada fitur yang diperkenalkan di `v1.2.0`, Anda bisa memasang `v1.x.y` untuk minor `x >= 2` dan patch `y` berapa pun. Anda perlu memasang major versi `1` (karena `2` bisa membawa perubahan tidak kompatibel), dan perlu minor `>= 2` (karena Anda bergantung pada fitur yang diperkenalkan di minor tersebut). Anda bisa memakai minor atau patch yang lebih baru karena seharusnya tidak memperkenalkan perubahan tak kompatibel.

# Lock file

Selain menentukan versi, bagus juga menegakkan agar _isi_ ketergantungan tidak berubah untuk mencegah gangguan. Beberapa alat menggunakan _lock file_ yang berisi hash kriptografis (bersama versi) yang dicek saat pemasangan paket.

# Menentukan versi

Alat sering mengizinkan penentuan versi dengan beberapa cara, seperti:

- versi tepat, mis. `2.3.12`
- minimum major, mis. `>= 2`
- major tertentu dan minimum patch, mis. `>= 2.3, <3.0`

Menentukan versi tepat berguna untuk menghindari perilaku berbeda berdasarkan dependensi terpasang (seharusnya tidak terjadi jika semua mematuhi semver, tapi manusia bisa salah). Menentukan batas minimum memberi keuntungan perbaikan bug tetap bisa dipasang (mis. upgrade patch).

# Resolusi dependensi

Manajer paket memakai berbagai algoritma resolusi untuk memenuhi kebutuhan dependensi. Ini sering menantang ketika ketergantungan kompleks (mis. sebuah paket bisa dibutuhkan secara tidak langsung oleh banyak dependensi tingkat atas dengan versi berbeda). Setiap manajer paket memiliki kecanggihan berbeda dalam resolusi dependensi; Anda mungkin perlu memahaminya saat men-debug ketergantungan.

# Lingkungan virtual

Jika mengembangkan banyak proyek perangkat lunak, mungkin masing-masing bergantung pada versi berbeda dari komponen tertentu. Terkadang alat build Anda menangani ini secara alami (mis. dengan membangun biner statis).

Untuk alat build atau bahasa lain, salah satu pendekatan adalah memakai lingkungan virtual (mis. [virtualenv](https://docs.python-guide.org/dev/virtualenvs/) untuk Python). Alih-alih memasang dependensi secara sistem-wide, Anda memasang per proyek di lingkungan virtual, lalu _mengaktifkan_ lingkungan yang ingin dipakai saat bekerja pada proyek tertentu.

# Vendoring

Pendekatan lain dalam manajemen ketergantungan adalah _vendoring_. Alih-alih memakai manajer paket atau alat build untuk mengambil perangkat lunak, Anda menyalin seluruh kode sumber dependensi ke repositori perangkat lunak Anda. Keuntungannya: Anda selalu membangun terhadap versi dependensi yang sama dan tidak perlu mengandalkan repositori paket, tetapi butuh lebih banyak usaha untuk memutakhirkan dependensi.
