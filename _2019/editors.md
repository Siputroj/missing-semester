---
layout: lecture
title: "Editor"
presenter: Anish
video:
  aspect: 62.5
  id: 1vLcusYSrI4
---

# Pentingnya Editor

Sebagai pemrogram, kita menghabiskan sebagian besar waktu dengan mengedit file
teks biasa. Pantas untuk meluangkan waktu mempelajari editor yang sesuai dengan
kebutuhanmu.

Bagaimana belajar editor baru? Paksa dirimu memakai editor itu selama beberapa
waktu, meski sementara menurunkan produktivitasmu. Itu akan segera terbayar
(dua minggu cukup untuk mempelajari dasar-dasarnya).

Kami akan mengajarkan Vim, tetapi kami mendorongmu untuk bereksperimen dengan
editor lain. Ini pilihan yang sangat personal, dan orang-orang memiliki
[pendapat kuat](https://en.wikipedia.org/wiki/Editor_war).

Kami tak bisa mengajarkan penggunaan editor kuat dalam 50 menit, jadi kami
fokus pada dasar-dasarnya, menampilkan beberapa fungsi lanjut, dan memberi
referensi untuk menguasainya. Kami mengajar dengan konteks Vim, namun banyak
gagasannya berlaku untuk editor kuat lain (jika tidak, mungkin editor itu
takterlalu layak digunakan!).

![Editor Learning Curves](/2019/files/editor-learning-curves.jpg)

<!-- source: https://blogs.msdn.microsoft.com/steverowe/2004/11/17/code-editor-learning-curves/ -->

Grafik kurva belajar editor adalah mitos. Mempelajari dasar editor yang kuat
sangat mudah (meski butuh bertahun-tahun untuk benar-benar menguasainya).

Editor mana yang populer hari ini? Lihat [survei Stack
Overflow](https://insights.stackoverflow.com/survey/2018/#development-environments-and-tools)
(mungkin ada bias karena pengguna Stack Overflow belum tentu mewakili seluruh
komunitas pemrogram).

## Editor Baris Perintah

Meski pada akhirnya kamu memakai editor GUI, mempelajari editor baris perintah
berguna untuk mengedit file di mesin remote dengan mudah.

# Nano

Nano adalah editor baris perintah yang sederhana.

- Bergerak dengan tombol panah
- Pintasan lain (simpan, keluar) terlihat di bagian bawah

# Vim

Vi/Vim adalah editor teks yang kuat. Ia adalah program baris perintah yang
umumnya terinstal di mana-mana, sehingga praktis untuk mengedit file di mesin
remote.

Vim juga memiliki versi grafis seperti GVim dan
[MacVim](https://macvim-dev.github.io/macvim/). Versi ini menyediakan fitur
tambahan seperti warna 24-bit, menu, dan popup.

## Filosofi Vim

- Saat pemrograman, kamu lebih banyak membaca/mengedit daripada menulis
    - Vim adalah editor **modal**: mode berbeda untuk menyisipkan teks dan
    memanipulasi teks
- Vim dapat diprogram (dengan Vimscript dan bahasa lain seperti Python)
- Antarmuka Vim sendiri seperti bahasa pemrograman
    - Tombol (dengan nama mnemonik) adalah perintah
    - Perintah bersifat komposabel
- Jangan pakai mouse: terlalu lambat
- Editor sebaiknya bekerja secepat kamu berpikir

## Pengantar Vim

### Mode

Vim menampilkan mode saat ini di kiri bawah.

- Normal mode: untuk bergerak di file dan membuat edit
    - Habiskan sebagian besar waktumu di sini
- Insert mode: untuk menyisipkan teks
- Visual (visual, baris, atau blok) mode: untuk memilih blok teks

Kamu berpindah mode dengan menekan `<ESC>` untuk kembali ke normal mode. Dari
normal mode, masuk insert mode dengan `i`, visual mode dengan `v`, visual line
mode dengan `V`, dan visual block mode dengan `<C-v>`.

Kamu sering memakai tombol `<ESC>` saat menggunakan Vim: pertimbangkan
memetakan ulang Caps Lock menjadi Escape.

### Dasar

Perintah Vim ex dijalankan melalui `:{command}` di normal mode.

- `:q` keluar (tutup jendela)
- `:w` simpan
- `:wq` simpan dan keluar
- `:e {nama file}` buka file untuk diedit
- `:ls` tampilkan buffer yang terbuka
- `:help {topik}` buka bantuan
    - `:help :w` membuka bantuan untuk perintah ex `:w`
    - `:help w` membuka bantuan untuk gerakan `w`

### Gerakan

Vim berfokus pada pergerakan yang efisien. Navigasi file di Normal mode.

- Nonaktifkan tombol panah untuk menghindari kebiasaan buruk
```vim
nnoremap <Left> :echoe "Use h"<CR>
nnoremap <Right> :echoe "Use l"<CR>
nnoremap <Up> :echoe "Use k"<CR>
nnoremap <Down> :echoe "Use j"<CR>
```
- Gerakan dasar: `hjkl` (kiri, bawah, atas, kanan)
- Kata: `w` (kata berikutnya), `b` (awal kata), `e` (akhir kata)
- Baris: `0` (awal baris), `^` (karakter non-kosong pertama), `$` (akhir baris)
- Layar: `H` (atas layar), `M` (tengah layar), `L` (bawah layar)
- File: `gg` (awal file), `G` (akhir file)
- Nomor baris: `:{angka}<CR>` atau `{angka}G` (baris {angka})
- Lain-lain: `%` (pasangan yang berkorespondensi)
- Temukan: `f{karakter}`, `t{karakter}`, `F{karakter}`, `T{karakter}`
    - temukan/menuju {karakter} ke depan/belakang pada baris saat ini
- Mengulang N kali: `{angka}{gerakan}`, mis. `10j` turun 10 baris
- Pencarian: `/{regex}`, `n` / `N` untuk menavigasi hasil

### Seleksi

Mode visual:

- Visual
- Visual Line
- Visual Block

Bisa menggunakan tombol gerak untuk membuat seleksi.

### Memanipulasi teks

Hal-hal yang dulu kamu lakukan dengan mouse, sekarang dilakukan dengan
keyboard (dan perintah yang kuat serta komposabel).

- `i` masuk insert mode
    - tetapi untuk memanipulasi/menghapus teks, gunakan lebih dari sekadar
    backspace
- `o` / `O` sisipkan baris di bawah / di atas
- `d{gerakan}` hapus {gerakan}
    - mis. `dw` menghapus kata, `d$` menghapus hingga akhir baris, `d0`
    menghapus hingga awal baris
- `c{gerakan}` ubah {gerakan}
    - mis. `cw` mengubah kata
    - seperti `d{gerakan}` diikuti `i`
- `x` hapus karakter (sama dengan `dl`)
- `s` ganti karakter (sama dengan `xi`)
- mode visual + manipulasi
    - pilih teks, `d` untuk menghapusnya atau `c` untuk mengubahnya
- `u` untuk undo, `<C-r>` untuk redo
- Masih banyak hal untuk dipelajari: mis. `~` membalik huruf besar/kecil sebuah karakter

### Sumber

- Program baris perintah `vimtutor` untuk mengajarkan Vim
- Gim [Vim Adventures](https://vim-adventures.com/) untuk belajar Vim

## Menyesuaikan Vim

Vim disesuaikan melalui file konfigurasi teks biasa `~/.vimrc` (berisi perintah
Vimscript). Kemungkinan ada banyak pengaturan dasar yang ingin kamu aktifkan.

Lihat dotfiles orang lain di GitHub untuk inspirasi, tetapi cobalah untuk tidak
menyalin penuh konfigurasi orang lain. Baca, pahami, dan ambil yang kamu
butuhkan.

Beberapa penyesuaian yang patut dipertimbangkan:

- Penyorotan sintaks: `syntax on`
- Skema warna
- Nomor baris: `set nu` / `set rnu`
- Backspace untuk segala sesuatu: `set backspace=indent,eol,start`

## Vim Lanjutan

Berikut beberapa contoh untuk menunjukkan kekuatan editor ini. Kami tidak bisa
mengajarkan semua hal seperti ini, tetapi kamu akan mempelajarinya seiring
waktu. Heuristik yang baik: kapan pun kamu memakai editor dan berpikir "pasti
ada cara yang lebih baik", biasanya memang ada: cari saja secara daring.

### Cari dan ganti

Perintah `:s` (substitute)
([dokumentasi](http://vim.wikia.com/wiki/Search_and_replace)).

- `%s/foo/bar/g`
    - ganti foo dengan bar secara global di file
- `%s/\[.*\](\(.*\))/\1/g`
    - ganti tautan Markdown bernama dengan URL polos

### Banyak jendela

- `sp` / `vsp` untuk membagi jendela
- Bisa memiliki banyak tampilan untuk buffer yang sama.

### Dukungan mouse

- `set mouse+=a`
    - bisa klik, gulir, dan memilih

### Makro

- `q{karakter}` untuk mulai merekam makro di register `{karakter}`
- `q` untuk menghentikan rekaman
- `@{karakter}` memutar ulang makro
- Eksekusi makro berhenti saat ada error
- `{angka}@{karakter}` menjalankan makro {angka} kali
- Makro bisa rekursif
    - pertama bersihkan makro dengan `q{karakter}q`
    - rekam makro, dengan `@{karakter}` untuk memanggil makro secara rekursif
    (tidak akan melakukan apa-apa sampai rekaman selesai)
- Contoh: ubah xml ke json ([file](/2019/files/example-data.xml))
    - Larik objek dengan kunci "name" / "email"
    - Pakai program Python?
    - Pakai sed / regex
        - `g/people/d`
        - `%s/<person>/{/g`
        - `%s/<name>\(.*\)<\/name>/"name": "\1",/g`
        - ...
    - Perintah / makro Vim
        - `Gdd`, `ggdd` menghapus baris pertama dan terakhir
        - Makro untuk memformat satu elemen (register `e`)
            - Pergi ke baris dengan `<name>`
            - `qe^r"f>s": "<ESC>f<C"<ESC>q`
        - Makro untuk memformat satu person
            - Pergi ke baris dengan `<person>`
            - `qpS{<ESC>j@eA,<ESC>j@ejS},<ESC>q`
        - Makro untuk memformat satu person dan lanjut ke person berikutnya
            - Pergi ke baris dengan `<person>`
            - `qq@pjq`
        - Eksekusi makro hingga akhir file
            - `999@q`
        - Hapus `,` terakhir secara manual dan tambahkan pembatas `[` dan `]`

## Memperluas Vim

Ada banyak plugin untuk memperluas vim.

Pertama, pasang pengelola plugin seperti
[vim-plug](https://github.com/junegunn/vim-plug),
[Vundle](https://github.com/VundleVim/Vundle.vim), atau
[pathogen.vim](https://github.com/tpope/vim-pathogen).

Beberapa plugin yang patut dipertimbangkan:

- [ctrlp.vim](https://github.com/kien/ctrlp.vim): pencari file fuzzy
- [vim-fugitive](https://github.com/tpope/vim-fugitive): integrasi git
- [vim-surround](https://github.com/tpope/vim-surround): memanipulasi "surroundings"
- [gundo.vim](https://github.com/sjl/gundo.vim): menavigasi pohon undo
- [nerdtree](https://github.com/scrooloose/nerdtree): penjelajah file
- [syntastic](https://github.com/vim-syntastic/syntastic): pemeriksaan sintaks
- [vim-easymotion](https://github.com/easymotion/vim-easymotion): motion ajaib
- [vim-over](https://github.com/osyo-manga/vim-over): pratinjau substitusi

Daftar plugin:

- [Vim Awesome](https://vimawesome.com/)

## Mode Vim di Program Lain

Untuk banyak editor populer (mis. vim dan emacs), banyak alat lain mendukung
emulasi editor tersebut.

- Shell
    - bash: `set -o vi`
    - zsh: `bindkey -v`
    - `export EDITOR=vim` (variabel lingkungan yang digunakan program seperti `git`)
- `~/.inputrc`
    - `set editing-mode vi`

Bahkan ada ekstensi keybinding vim untuk [peramban web](http://vim.wikia.com/wiki/Vim_key_bindings_for_web_browsers),
beberapa yang populer adalah [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en)
untuk Google Chrome dan [Tridactyl](https://github.com/tridactyl/tridactyl)
untuk Firefox.


## Sumber

- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2018/): berbagai tips Vim
- [Neovim](https://neovim.io/) adalah reimplementasi vim modern dengan
  pengembangan yang lebih aktif.
- [Vim Golf](http://www.vimgolf.com/): Berbagai tantangan Vim

{% comment %}
# Resources

TODO resources for other editors?
{% endcomment %}

# Latihan

1. Bereksperimenlah dengan beberapa editor. Coba setidaknya satu editor baris
   perintah (mis. Vim) dan setidaknya satu editor GUI (mis. Atom). Belajar
   melalui tutorial seperti `vimtutor` (atau padanan untuk editor lain). Untuk
   benar-benar merasakan editor baru, komit menggunakan editor itu secara
   eksklusif selama beberapa hari sambil bekerja.

1. Sesuaikan editor-mu. Lihat tips dan trik secara daring, dan lihat
   konfigurasi orang lain (sering kali, dokumentasinya bagus).

1. Bereksperimenlah dengan plugin untuk editor-mu.

1. Komit memakai editor yang kuat setidaknya selama beberapa minggu: kamu
   seharusnya mulai merasakan manfaatnya. Pada titik tertentu, kamu seharusnya
   bisa membuat editormu bekerja secepat kamu berpikir.

1. Instal linter (mis. pyflakes untuk python), hubungkan ke editor, dan uji
   apakah berfungsi.
