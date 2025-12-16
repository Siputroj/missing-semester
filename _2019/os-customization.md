---
layout: lecture
title: "Kustomisasi OS"
presenter: Anish
video:
  aspect: 62.5
  id: epSRVqQzeDo
---

Banyak hal yang bisa Anda lakukan untuk menyesuaikan sistem operasi di luar menu pengaturan bawaan.

# Pemetaulang tombol

Papan ketik Anda mungkin punya tombol yang jarang dipakai. Alih-alih membiarkannya, Anda bisa memetakan ulang menjadi fungsi berguna.

## Memetakan ke tombol lain

Hal termudah adalah memetakan tombol ke tombol lain. Misalnya, jika Anda jarang memakai caps lock, Anda bisa memetakannya ke sesuatu yang lebih berguna. Jika Anda pengguna Vim, Anda mungkin ingin memetakan caps lock menjadi Escape.

Di macOS, beberapa pemetaan dapat dilakukan melalui pengaturan Keyboard di System Preferences; untuk pemetaan lebih rumit, Anda perlu perangkat lunak khusus.

## Memetakan ke perintah arbitrer

Anda tidak harus memetakan tombol ke tombol lain: ada alat yang memungkinkan Anda memetakan tombol (atau kombinasi tombol) ke perintah apa pun. Misalnya, Anda bisa membuat command-shift-t membuka jendela terminal baru.

# Menyesuaikan pengaturan OS tersembunyi

## macOS

macOS mengekspose banyak pengaturan berguna melalui perintah `defaults`. Misalnya, Anda dapat membuat ikon Dock dari aplikasi tersembunyi menjadi tembus pandang:

```shell
defaults write com.apple.dock showhidden -bool true
```

Tidak ada daftar tunggal semua pengaturan, tetapi Anda bisa menemukan daftar kustomisasi tertentu secara daring, seperti [.macos](https://github.com/mathiasbynens/dotfiles/blob/master/.macos) milik Mathias Bynens.

# Manajemen jendela

## Manajemen jendela tiling

[Tiling window management](https://en.wikipedia.org/wiki/Tiling_window_manager) adalah pendekatan mengelola jendela dengan menata jendela ke bingkai yang tidak tumpang tindih. Jika memakai sistem operasi berbasis Unix, Anda bisa memasang tiling window manager; jika menggunakan Windows atau macOS, Anda bisa memasang aplikasi yang menirukan perilaku ini.

## Manajemen layar

Anda bisa menyiapkan pintasan keyboard untuk membantu memanipulasi jendela antar layar.

## Tata letak

Jika ada cara khusus Anda menata jendela di layar, daripada menata secara manual setiap kali, Anda bisa menskriptnya sehingga memunculkan tata letak menjadi sepele.

# Sumber daya

- [Hammerspoon](https://www.hammerspoon.org/) - otomasi desktop macOS
- [Rectangle](https://rectangleapp.com/) - pengelola jendela macOS
- [Karabiner](https://karabiner-elements.pqrs.org/) - pemetaulang keyboard macOS canggih
- [r/unixporn](https://www.reddit.com/r/unixporn/) - tangkapan layar dan dokumentasi konfigurasi keren orang lain

# Latihan

1. Cari cara memetakan ulang tombol Caps Lock ke sesuatu yang lebih sering Anda pakai (misalnya Escape, Ctrl, atau Backspace).
1. Buat pintasan keyboard global khusus untuk membuka jendela terminal baru atau jendela browser baru.

{% comment %}

TODO

- Bitbar / Polybar
- Clipboard Manager (riwayat tumpukan/yang bisa dicari)

{% endcomment %}
