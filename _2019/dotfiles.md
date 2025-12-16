---
layout: lecture
title: "Dotfiles"
presenter: Anish
video:
  aspect: 62.5
  id: YSZBWWJw3mI
---

Banyak program dikonfigurasi menggunakan berkas teks biasa yang disebut "dotfiles" (karena nama berkasnya diawali `.` seperti `~/.gitconfig`, sehingga tersembunyi saat `ls` secara bawaan).

Banyak alat yang Anda gunakan punya banyak pengaturan yang bisa diutak-atik. Sering kali alat dikustomisasi dengan bahasa khusus, misalnya Vimscript untuk Vim atau bahasa shell itu sendiri.

Menyesuaikan alat dengan alur kerja pilihan Anda akan membuat Anda lebih produktif. Kami menyarankan menghabiskan waktu menyesuaikan alat sendiri ketimbang menyalin dotfiles orang lain dari GitHub.

Anda mungkin sudah punya beberapa dotfile. Cek di:

- `~/.bashrc`
- `~/.emacs`
- `~/.vim`
- `~/.gitconfig`

Beberapa program tidak menaruh berkas langsung di folder home dan malah menaruhnya di dalam `~/.config`.

Dotfile tidak eksklusif untuk aplikasi command-line; misalnya pemutar video [MPV](https://mpv.io/) dapat dikonfigurasi lewat berkas di `~/.config/mpv`.

# Belajar mengustomisasi alat

Anda bisa mempelajari pengaturan alat melalui dokumentasi daring atau [man page](https://en.wikipedia.org/wiki/Man_page). Cara lain yang bagus adalah mencari tulisan blog tentang program tertentu, di mana penulis menjelaskan kustomisasi favorit mereka. Cara lain lagi adalah melihat dotfiles orang lain: ada banyak [repositori dotfiles](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories) di GitHub — lihat yang paling populer [di sini](https://github.com/mathiasbynens/dotfiles) (meski kami menyarankan untuk tidak menyalin mentah-mentah).

# Organisasi

Bagaimana sebaiknya mengatur dotfile? Simpan di folder khusus, gunakan kontrol versi, dan **symlink** ke tempatnya dengan skrip. Keuntungannya:

- **Pemasangan mudah**: jika masuk ke mesin baru, menerapkan kustomisasi hanya semenit
- **Portabilitas**: alat bekerja sama di mana saja
- **Sinkronisasi**: Anda bisa memperbarui dotfile di mana saja dan tetap sinkron
- **Pelacakan perubahan**: Anda mungkin merawat dotfile sepanjang karier, dan riwayat versi berguna untuk proyek jangka panjang

```shell
cd ~/src
mkdir dotfiles
cd dotfiles
git init
touch bashrc
# buat bashrc dengan beberapa pengaturan, contoh:
#     PS1='\w > '
touch install
chmod +x install
# isi skrip install dengan:
#     #!/usr/bin/env bash
#     BASEDIR=$(dirname $0)
#     cd $BASEDIR
#
#     ln -s ${PWD}/bashrc ~/.bashrc
git add bashrc install
git commit -m 'Initial commit'
```

# Topik lanjutan

## Kustomisasi spesifik mesin

Sebagian besar waktu Anda ingin konfigurasi yang sama di semua mesin, tetapi kadang perlu perbedaan kecil di mesin tertentu. Ada beberapa cara:

### Branch per mesin

Gunakan kontrol versi untuk memelihara branch per mesin. Secara logis sederhana tetapi bisa berat.

### If statement

Jika berkas konfigurasi mendukung, gunakan if-statement untuk menerapkan kustomisasi spesifik mesin. Misalnya, shell Anda bisa memiliki:

```shell
if [[ "$(uname)" == "Linux" ]]; then {do_something else}; fi

# Darwin adalah nama arsitektur untuk macOS
if [[ "$(uname)" == "Darwin" ]]; then {do_something}; fi

# Bisa juga spesifik mesin
if [[ "$(hostname)" == "myServer" ]]; then {do_something}; fi
```

### Include

Jika berkas konfigurasi mendukung, gunakan include. Misalnya, `~/.gitconfig` bisa memiliki:

```
[include]
    path = ~/.gitconfig_local
```

Lalu di setiap mesin, `~/.gitconfig_local` berisi pengaturan khusus mesin. Anda bahkan bisa melacaknya di repo terpisah.

Ide ini juga berguna jika ingin beberapa program berbagi konfigurasi. Misalnya jika ingin `bash` dan `zsh` berbagi alias yang sama, tulis di `.aliases` dan tambahkan blok berikut di keduanya:

```bash
# Cek apakah ~/.aliases ada lalu source
if [ -f ~/.aliases ]; then
    source ~/.aliases
fi
```

# Sumber daya

- Dotfile pengajar: [Anish](https://github.com/anishathalye/dotfiles), [Jon](https://github.com/jonhoo/configs), [Jose](https://github.com/jjgo/dotfiles)
- [GitHub does dotfiles](http://dotfiles.github.io/): kerangka dotfile, utilitas, contoh, dan tutorial
- [Shell startup scripts](https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html): penjelasan berbagai berkas konfigurasi shell

# Latihan

1. Buat folder untuk dotfile Anda dan siapkan [kontrol versi](/2019/version-control/).
1. Tambahkan konfigurasi minimal satu program, misalnya shell, dengan sedikit kustomisasi (awal bisa sesederhana mengubah prompt shell dengan `$PS1`).
1. Siapkan metode memasang dotfile dengan cepat (tanpa kerja manual) di mesin baru. Bisa berupa skrip shell yang memanggil `ln -s` untuk tiap berkas, atau gunakan [utilitas khusus](http://dotfiles.github.io/utilities/).
1. Uji skrip pemasangan di mesin virtual baru.
1. Migrasikan semua konfigurasi alat Anda ke repositori dotfile.
1. Publikasikan dotfile Anda di GitHub.
