---
layout: lecture
title: "Lingkungan Command-line"
presenter: Jose
video:
  aspect: 62.5
  id: i0rf1gpKL1E
---

## Alias & Fungsi

Seperti yang bisa Anda bayangkan, mengetik perintah panjang dengan banyak flag atau opsi verbose bisa melelahkan. Untungnya, sebagian besar shell mendukung **alias**. Misalnya, alias di bash memiliki struktur berikut (perhatikan tidak ada spasi di sekitar tanda `=`):

```bash
alias alias_name="command_to_alias"
```

Alias punya banyak kegunaan:

```bash
# Alias bisa merangkum flag default yang baik
alias ll="ls -lh"

# Hemat banyak ketikan untuk perintah umum
alias gc="git commit"

# Alias dapat menimpa perintah yang ada
alias mv="mv -i"
alias mkdir="mkdir -p"

# Alias bisa dikomposisi
alias la="ls -A"
alias lla="la -l"

# Untuk mengabaikan alias, jalankan dengan awalan \
\ls
# Atau bisa dinonaktifkan memakai unalias
unalias la
```

Namun dalam banyak skenario alias bisa membatasi, terutama saat ingin merangkai perintah yang membutuhkan argumen sama. Alternatifnya adalah **fungsi**, titik tengah antara alias dan skrip shell khusus.

Contoh fungsi yang membuat direktori lalu masuk ke sana:

```bash
mcd () {
    mkdir -p $1
    cd $1
}
```

Alias dan fungsi tidak bertahan antar sesi shell secara bawaan. Untuk membuatnya persisten, masukkan ke salah satu berkas startup shell seperti `.bashrc` atau `.zshrc`. Saran saya, tulis terpisah di `.alias` dan `source` berkas itu dari berbagai konfigurasi shell Anda.

## Shell & Framework

Saat membahas shell dan scripting kita memakai `bash` karena paling umum dan jadi default di banyak sistem. Namun itu bukan satu-satunya opsi.

Misalnya `zsh` adalah superset `bash` dan menyediakan banyak fitur praktis langsung seperti:

- Globbing lebih pintar, `**`
- Globbing/ekspansi wildcard inline
- Koreksi ejaan
- Tab completion/pemilihan lebih baik
- Ekspansi path (`cd /u/lo/b` akan menjadi `/usr/local/bin`)

Banyak shell juga bisa ditingkatkan dengan **framework**, seperti [prezto](https://github.com/sorin-ionescu/prezto) atau [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh), dan yang lebih kecil untuk fitur spesifik seperti [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) atau [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search). Shell lain seperti [fish](https://fishshell.com/) sudah menyertakan banyak fitur ramah pengguna secara bawaan, seperti:

- Right prompt
- Highlight sintaks perintah
- Pencarian substring history
- Completion flag berbasis manpage
- Autocompletion lebih cerdas
- Tema prompt

Catatan: jika kode yang dijalankan framework ini tidak dioptimalkan atau terlalu banyak, shell Anda bisa melambat. Anda selalu bisa memprofil dan menonaktifkan fitur yang jarang dipakai atau yang tidak sebanding dengan biayanya.

## Terminal Emulator & Multiplexer

Selain menyesuaikan shell, luangkan waktu memilih **terminal emulator** dan pengaturannya. Ada banyak sekali terminal emulator (berikut [perbandingan](https://anarc.at/blog/2018-04-12-terminal-emulators-1/)).

Karena Anda mungkin menghabiskan ratusan hingga ribuan jam di terminal, ada baiknya menyesuaikan pengaturannya. Beberapa aspek yang bisa Anda ubah:

- Pilihan font
- Skema warna
- Pintasan keyboard
- Dukungan tab/pane
- Konfigurasi scrollback
- Performa (terminal baru seperti [Alacritty](https://github.com/jwilm/alacritty) menawarkan akselerasi GPU)

Perlu juga menyebut **terminal multiplexer** seperti [tmux](https://github.com/tmux/tmux). `tmux` memungkinkan Anda membuat pane dan tab untuk banyak sesi shell. Ia juga mendukung attach/detach—berguna saat bekerja di server jarak jauh dan ingin menjaga shell tetap berjalan tanpa repot `disown` proses (secara bawaan proses berhenti saat logout). Dengan `tmux` Anda bisa keluar-masuk tata letak terminal kompleks. Seperti terminal emulator, `tmux` sangat bisa dikustomisasi lewat berkas `~/.tmux.conf`.

## Utilitas command-line

Utilitas command line yang disertakan di sebagian besar sistem berbasis UNIX sudah cukup untuk 99% kebutuhan.

Pada beberapa subseksi berikut saya bahas alat alternatif untuk operasi shell yang sangat umum yang lebih nyaman digunakan. Sebagian menambah fungsionalitas, sebagian lain fokus pada antarmuka yang lebih sederhana dengan default lebih baik.

### `fasd` vs `cd`

Meski ekspansi path dan autocomplete sudah membantu, ganti direktori bisa tetap repetitif. [Fasd](https://github.com/clvv/fasd) (atau [autojump](https://github.com/wting/autojump)) menyelesaikan ini dengan melacak folder terbaru/sering dikunjungi dan melakukan pencocokan fuzzy.

Jadi jika saya pernah mengunjungi `/home/user/awesome_project/code`, menjalankan `z code` akan `cd` ke sana. Jika ada banyak folder bernama code saya bisa bedakan dengan `z awe code` yang lebih cocok. Berbeda dengan autojump, fasd juga menyediakan perintah yang tidak melakukan `cd` tetapi hanya mengekspansi berkas/folder yang sering/baru.

### `bat` vs `cat`

`cat` bekerja dengan baik, tetapi [bat](https://github.com/sharkdp/bat) meningkatkannya dengan highlight sintaks, paging, nomor baris, dan integrasi git.

### `exa`/`ranger` vs `ls`

`ls` bagus tetapi beberapa default bisa mengganggu seperti ukuran dalam byte mentah. [exa](https://github.com/ogham/exa) memberi default lebih baik.

Jika Anda perlu menelusuri banyak folder dan/atau mempratinjau banyak berkas, [ranger](https://github.com/ranger/ranger) bisa jauh lebih efisien daripada `cd` dan `cat` berkat antarmukanya. Ranger cukup bisa dikustomisasi dan dengan pengaturan yang tepat Anda bahkan bisa [pratinjau gambar](https://github.com/ranger/ranger/wiki/Image-Previews) di terminal.

### `fd` vs `find`

[fd](https://github.com/sharkdp/fd) adalah alternatif `find` yang sederhana, cepat, dan ramah pengguna. Default `find` seperti harus memakai flag `--name` (yang 99% kasus Anda inginkan) diganti dengan perilaku yang lebih mudah dipakai sehari-hari. Ia juga sadar `git` dan akan melewati berkas di `.gitignore` dan folder `.git` secara bawaan, plus pewarnaan yang enak.

### `rg/fzf` vs `grep`

`grep` hebat, tetapi jika ingin mencari di banyak berkas sekaligus ada alat yang lebih cocok. [ack](https://github.com/beyondgrep/ack3), [ag](https://github.com/ggreer/the_silver_searcher), dan [rg](https://github.com/BurntSushi/ripgrep) mencari rekursif direktori saat ini untuk pola regex sambil menghormati aturan gitignore. Semuanya mirip, tetapi saya pilih `rg` karena seberapa cepat ia bisa menelusuri seluruh home saya.

Demikian pula, mudah saja berulang kali melakukan `CMD | grep PATTERN`. [fzf](https://github.com/junegunn/fzf) adalah fuzzy finder command line yang memungkinkan Anda memfilter output hampir perintah apa pun secara interaktif.

### `rsync` vs `cp/scp`

Walau `mv` dan `scp` cocok untuk banyak skenario, saat menyalin/memindah banyak berkas, berkas besar, atau ketika sebagian data sudah ada di tujuan, `rsync` merupakan peningkatan besar. `rsync` akan melewati berkas yang sudah ditransfer dan dengan flag `--partial` bisa melanjutkan salinan yang terputus.

### `trash` vs `rm`

`rm` berbahaya karena sekali hapus berkas, tidak ada jalan kembali. Namun OS modern tidak bertindak seperti itu di file explorer; mereka memindahkan ke folder Trash yang dibersihkan berkala.

Karena pengelolaan trash berbeda per OS, tidak ada satu utilitas CLI tunggal. Di macOS ada [trash](https://hasseg.org/trash/) dan di Linux ada [trash-cli](https://github.com/andreafrancia/trash-cli/) dan lainnya.

### `mosh` vs `ssh`

`ssh` sangat berguna tetapi jika koneksi lambat, lag terasa mengganggu dan jika terputus Anda harus menyambung lagi. [mosh](https://mosh.org/) memungkinkan roaming, mendukung konektivitas terputus-putus, dan menyediakan local echo yang cerdas.

### `tldr` vs `man`

Anda bisa mencari tahu apa yang dilakukan perintah dan opsi-opsinya memakai `man` dan flag `-h`/`--help` kebanyakan waktu. Namun kadang agak menakutkan menelusuri dokumentasi detail.

Perintah [tldr](https://github.com/tldr-pages/tldr) adalah sistem dokumentasi berbasis komunitas yang tersedia dari command line dan memberi beberapa contoh sederhana apa yang dilakukan perintah beserta opsi argumen paling umum.

### `aunpack` vs `tar/unzip/unrar`

Seperti referensi [xkcd ini](https://xkcd.com/1168/), mengingat opsi `tar` bisa rumit dan kadang Anda butuh alat lain seperti `unrar` untuk berkas .rar. Paket [atool](https://www.nongnu.org/atool/) menyediakan perintah `aunpack` yang akan mencari opsi yang tepat dan selalu menaruh arsip yang diekstrak di folder baru.

## Latihan

1. Jalankan `cat .bash_history | sort | uniq -c | sort -rn | head -n 10` (atau `cat .zhistory | sort | uniq -c | sort -rn | head -n 10` untuk zsh) untuk melihat 10 perintah paling sering dan pertimbangkan membuat alias lebih pendek untuk mereka.
1. Pilih terminal emulator dan cari cara mengubah properti berikut:
    - Pilihan font
    - Skema warna. Berapa banyak warna skema standar? mengapa?
    - Ukuran riwayat scrollback
1. Pasang `fasd` atau perangkat lunak serupa dan tulis fungsi bash/zsh bernama `v` yang melakukan fuzzy matching pada argumen yang diberikan dan membuka hasil teratas di editor pilihan Anda. Lalu modifikasi agar jika ada banyak kecocokan Anda bisa memilihnya dengan `fzf`.
1. Karena `fzf` nyaman untuk pencarian fuzzy dan riwayat shell cocok untuk itu, cari cara mengikat `fzf` ke `^R`. Anda bisa menemukan info [di sini](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings).
1. Apa yang dilakukan opsi `--bar` di `ack`?
