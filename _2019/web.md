---
layout: lecture
title: "Web dan Peramban"
presenter: Jose
video:
  aspect: 62.5
  id: XpZO3S8odec
---

Selain terminal, peramban web adalah alat tempat kamu menghabiskan banyak waktu.
Karenanya layak belajar cara memakainya dengan efisien.

## Pintasan

Klik sana-sini di peramban sering bukan opsi tercepat; mengenal pintasan umum
bisa sangat berguna dalam jangka panjang.

- `Middle Button Click` pada tautan membukanya di tab baru
- `Ctrl+T` Membuka tab baru
- `Ctrl+Shift+T` Membuka kembali tab yang baru ditutup
- `Ctrl+L` memilih isi bilah pencarian
- `Ctrl+F` untuk mencari di dalam halaman web. Jika sering melakukannya, kamu
  mungkin butuh ekstensi yang mendukung regular expression dalam pencarian.


## Operator pencarian

Mesin pencari web seperti Google atau DuckDuckGo menyediakan operator pencarian
untuk memungkinkan pencarian web lebih rumit:

- `"bar foo"` memaksa kecocokan persis bar foo
- `foo site:bar.com` mencari foo di dalam bar.com
- `foo -bar ` mengecualikan istilah yang mengandung bar dari pencarian
- `foobar filetype:pdf` Mencari file dengan ekstensi itu
- `(foo|bar)` mencari kecocokan yang memiliki foo ATAU bar

Daftar lebih lengkap tersedia untuk mesin populer seperti
[Google](https://ahrefs.com/blog/google-advanced-search-operators/) dan
[DuckDuckGo](https://duck.co/help/results/syntax)


## Bilah pencarian

Bilah pencarian juga alat yang kuat. Kebanyakan peramban bisa menebak mesin
pencari dari situs web dan menyimpannya. Dengan mengedit argumen kata kunci

- Di Google Chrome berada di [chrome://settings/searchEngines](chrome://settings/searchEngines)
- Di Firefox berada di [about:preferences#search](about:preferences#search)

Sebagai contoh kamu bisa membuat `y BEBERAPA ISTILAH PENCARIAN` langsung mencari
di youtube.

Selain itu, jika kamu punya domain, kamu bisa mengatur forward subdomain lewat
registrarmu. Misalnya saya memetakan `https://ht.josejg.com` ke situs kursus ini.
Dengan begitu saya cukup mengetik `ht.` dan bilah pencarian akan melengkapi.
Keunggulan lain setup ini adalah, tidak seperti bookmark, ia akan berfungsi di
setiap peramban.

## Ekstensi privasi

Kini berselancar web bisa cukup mengganggu karena iklan dan invasif karena
tracker. Selain itu adblocker yang bagus tidak hanya memblokir konten iklan tapi
akan memblokir situs mencurigakan dan berbahaya karena mereka termasuk dalam
daftar hitam umum. Mereka juga mengurangi waktu muat halaman dengan mengurangi
jumlah permintaan. Beberapa rekomendasi:

- **uBlock origin** ([Chrome](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)):
  memblokir iklan dan tracker berdasarkan aturan bawaan. Kamu juga sebaiknya
  melihat daftar hitam yang diaktifkan di pengaturan karena kamu bisa
  mengaktifkan lebih banyak sesuai wilayah atau kebiasaan browsing. Kamu bahkan
  bisa memasang filter dari [berbagai sumber](https://github.com/gorhill/uBlock/wiki/Filter-lists-from-around-the-web)

- **[Privacy Badger](https://privacybadger.org/)**: mendeteksi dan memblokir
  tracker secara otomatis. Misalnya ketika kamu berpindah situs, perusahaan iklan
  melacak situs yang kamu kunjungi dan membuat profil tentangmu

- **[HTTPS everywhere](https://www.eff.org/https-everywhere)** adalah ekstensi
  bagus yang otomatis mengalihkan ke versi HTTPS sebuah situs, jika ada.

Kamu bisa menemukan lebih banyak addon sejenis [di sini](https://www.privacytools.io/privacy-browser-addons/)

## Kustomisasi gaya

Peramban web adalah perangkat lunak lain yang berjalan di _mesinmu_ jadi kamu
biasanya punya keputusan akhir tentang apa yang mereka tampilkan atau bagaimana
berperilaku. Salah satu contohnya adalah gaya kustom. Peramban menentukan cara
merender gaya halaman web menggunakan Cascading Style Sheets (CSS).

Kamu bisa mengakses kode sumber situs web dengan menginspeksinya dan mengubah
konten serta gayanya secara sementara (ini juga alasan kenapa kamu tak boleh
percaya tangkapan layar halaman web).

Jika ingin memberi tahu peramban untuk menimpa pengaturan gaya sebuah situs
secara permanen, kamu perlu memakai ekstensi. Rekomendasi kami adalah
**[Stylus](https://github.com/openstyles/stylus)** ([Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), [Chrome](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=en)).


Misalnya, kita bisa menulis gaya berikut untuk situs kelas


```css

body {
    background-color: #2d2d2d;
    color: #eee;
    font-family: Fira Code;
    font-size: 16pt;
}

a:link {
    text-decoration: none;
    color: #0a0;
}
```

Selain itu, Stylus bisa menemukan gaya yang ditulis pengguna lain dan diterbitkan
di [userstyles.org](https://userstyles.org/). Sebagian besar situs populer punya
satu atau beberapa stylesheet tema gelap misalnya. FYI, jangan gunakan Stylish
karena terbukti membocorkan data pengguna, lebih lanjut
[di sini](https://arstechnica.com/information-technology/2018/07/stylish-extension-with-2m-downloads-banished-for-tracking-every-site-visit/)


## Kustomisasi fungsi

Seperti kamu bisa mengubah gaya, kamu juga bisa mengubah perilaku situs web
dengan menulis javascript kustom dan memuatnya menggunakan ekstensi peramban
seperti [Tampermonkey](https://tampermonkey.net/)

Misalnya skrip berikut memungkinkan navigasi ala vim memakai tombol J dan K.

```js
// ==UserScript==
// @name         VIM HT
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Vim JK for our website
// @author       You
// @match        https://hacker-tools.github.io/*
// @grant        none
// ==/UserScript==


(function() {
    'use strict';

    window.onkeyup = function(e) {
        var key = e.keyCode ? e.keyCode : e.which;

        if (key == 74) { // J is key 74
            window.scrollBy(0,500);;
        }else if (key == 75) { // K is key 75
            window.scrollBy(0,-500);;
        }
    }
})();
```

Ada juga repository skrip seperti [OpenUserJS](https://openuserjs.org/) dan
[Greasy Fork](https://greasyfork.org/en). Namun, hati-hati, memasang user script
dari orang lain bisa sangat berbahaya karena mereka bisa melakukan apa saja
seperti mencuri nomor kartu kreditmu. Jangan pernah memasang skrip kecuali kamu
membaca seluruhnya sendiri, memahami apa yang dilakukannya, dan benar-benar
yakin tidak ada hal mencurigakan. Jangan pernah memasang skrip yang mengandung
kode minimifikasi atau obfuscated yang tidak bisa kamu baca!

## Web API

Semakin umum layanan web menawarkan antarmuka aplikasi alias web API sehingga
kamu bisa berinteraksi dengan layanan melalui permintaan web. Pengantar lebih
mendalam bisa ditemukan [di sini](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction).
Ada [banyak API publik](https://github.com/toddmotto/public-apis). Web API bisa
berguna untuk banyak alasan:

- **Retrieval**. Web API bisa dengan mudah memberi info seperti peta, cuaca,
  atau alamat IP publikmu. Misalnya `curl ipinfo.io` akan mengembalikan objek
  JSON dengan beberapa detail tentang IP publik, wilayah, lokasi, dll. Dengan
  parsing yang tepat alat ini bisa diintegrasikan bahkan dengan alat baris
  perintah. Fungsi bash berikut berbicara ke API autocompletion Google dan
  mengembalikan sepuluh kecocokan pertama.

```bash
function c() {
    url='https://www.google.com/complete/search?client=hp&hl=en&xhr=t'
    # NB: user-agent must be specified to get back UTF-8 data!
    curl -H 'user-agent: Mozilla/5.0' -sSG --data-urlencode "q=$*" "$url" |
        jq -r ".[1][][0]" |
        sed 's,</\?b>,,g'
}
```

- **Interaksi**. Endpoint Web API juga bisa digunakan untuk memicu aksi. Ini
  biasanya memerlukan semacam token autentikasi yang bisa kamu dapatkan melalui
  layanan. Misalnya menjalankan
  `curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}' "https://hooks.slack.com/services/$SLACK_TOKEN"`
  akan mengirim pesan `Hello, World!` di sebuah kanal.

- **Piping**. Karena beberapa layanan dengan web API cukup populer, "perekat"
  web API umum sudah diimplementasikan dan disediakan dengan server terlampir.
  Ini terjadi pada layanan seperti [If This Then That](https://ifttt.com/) dan
  [Zapier](https://zapier.com/)


## Otomasi Web

Kadang web API tidak cukup. Jika hanya perlu membaca, kamu bisa memakai parser
html seperti `pup` atau memakai library, misalnya python punya BeautifulSoup.
Namun jika interaktivitas atau eksekusi javascript diperlukan, solusi itu kurang.
WebDriver


Misalnya, skrip berikut akan menyimpan URL tertentu menggunakan wayback machine
dengan mensimulasikan interaksi mengetik situs.

```python
from selenium.webdriver import Firefox
from selenium.webdriver.common.keys import Keys


def snapshot_wayback(driver, url):

    driver.get("https://web.archive.org/")
    elem = driver.find_element_by_class_name('web-save-url-input')
    elem.clear()
    elem.send_keys(url)
    elem.send_keys(Keys.RETURN)
    driver.close()


driver = Firefox()
url = 'https://hacker-tools.github.io'
snapshot_wayback(driver, url)
```


## Latihan

1. Edit mesin pencari kata kunci yang sering kamu gunakan di peramban web
1. Pasang ekstensi yang disebutkan. Cari tahu bagaimana menonaktifkan uBlock
   Origin/Privacy Badger untuk sebuah situs. Perbedaan apa yang kamu lihat? Coba
   lakukan di situs dengan banyak iklan seperti YouTube.
1. Pasang Stylus dan tulis gaya kustom untuk situs kelas menggunakan CSS yang
   diberikan. Berikut beberapa karakter pemrograman `=   ==   ===   >=   =>   ++   /=   ~=`.
   Apa yang terjadi pada mereka ketika mengganti font ke Fira Code? Jika ingin
   tahu lebih lanjut cari ligature font pemrograman.
1. Temukan web API untuk mendapatkan cuaca di kota/daerahmu.
1. Gunakan perangkat lunak WebDriver seperti [Selenium](https://docs.seleniumhq.org/)
   untuk mengotomatisasi tugas manual berulang yang sering kamu lakukan dengan
   perambanmu.
