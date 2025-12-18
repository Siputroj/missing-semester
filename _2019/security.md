---
layout: lecture
title: "Keamanan dan Privasi"
presenter: Jon
video:
  aspect: 56.25
  id: OBx_c-i-M8s
---

Dunia ini menakutkan, dan semua orang seolah ingin menjatuhkanmu.

Baiklah, mungkin tidak, tapi bukan berarti kamu ingin memamerkan semua
rahasia. Keamanan (dan privasi) umumnya soal menaikkan ambang bagi penyerang.
Temukan model ancamanmu, lalu rancang mekanisme keamanan berdasar itu! Jika
model ancamannya NSA atau Mossad, kamu _mungkin_ akan kesulitan.

Ada _banyak_ cara membuat persona teknismu lebih aman. Di sini kita menyentuh
banyak hal tingkat tinggi, tapi ini proses, dan mendidik dirimu sendiri adalah
salah satu hal terbaik yang bisa dilakukan. Jadi:

## Ikuti Orang yang Tepat

Salah satu cara terbaik meningkatkan pengetahuan keamananmu adalah mengikuti
orang-orang yang vokal tentang keamanan. Beberapa saran:

 - [@TroyHunt](https://twitter.com/TroyHunt)
 - [@SwiftOnSecurity](https://twitter.com/SwiftOnSecurity)
 - [@taviso](https://twitter.com/taviso)
 - [@thegrugq](https://twitter.com/thegrugq)
 - [@tqbf](https://twitter.com/tqbf)
 - [@mattblaze](https://twitter.com/mattblaze)
 - [@moxie](https://twitter.com/moxie)

Lihat juga [daftar ini](https://heimdalsecurity.com/blog/best-twitter-cybersec-accounts/)
untuk lebih banyak saran.

## Saran Keamanan Umum

Tech Solidarity punya daftar [do dan don't untuk jurnalis](https://web.archive.org/web/20221123204419/https://techsolidarity.org/resources/basic_security.htm)
yang berisi banyak saran masuk akal dan cukup terbaru. [@thegrugq](https://medium.com/@thegrugq)
juga punya posting blog bagus tentang [saran keamanan perjalanan](https://medium.com/@thegrugq/stop-fabricating-travel-security-advice-35259bf0e869)
yang layak dibaca. Kami akan mengulangi banyak saran dari sana, plus beberapa
lainnya. Juga, dapatkan [USB data blocker](https://www.amazon.com/dp/B00QRRZ2QM/),
karena [USB itu menakutkan](https://www.bleepingcomputer.com/news/security/heres-a-list-of-29-different-types-of-usb-attacks/).

## Autentikasi

Hal pertama yang harus kamu lakukan, jika belum, adalah mengunduh pengelola
kata sandi. Beberapa yang bagus:

 - [1password](https://1password.com/)
 - [KeePass](https://keepass.info/)
 - [BitWarden](https://bitwarden.com/)
 - [`pass`](https://git.zx2c4.com/password-store/about/)

Jika kamu sangat paranoid, gunakan yang mengenkripsi kata sandi secara lokal di
komputermu, bukan menyimpannya dalam teks biasa di server. Gunakan untuk
menghasilkan kata sandi untuk semua situs web yang kamu pedulikan sekarang.
Lalu aktifkan autentikasi dua faktor, idealnya dengan dongle
[FIDO/U2F](https://fidoalliance.org/) (misalnya [YubiKey](https://www.yubico.com/quiz/)
yang memiliki [diskon 20% untuk siswa](https://www.yubico.com/why-yubico/for-education/)).
TOTP (seperti Google Authenticator atau Duo) juga bisa, tapi
[tidak melindungi dari phishing](https://twitter.com/taviso/status/1082015009348104192).
SMS hampir tidak berguna kecuali model ancamanmu hanya orang asing acak yang
menyadap kata sandimu saat transit.

Juga catatan tentang paper key. Sering kali layanan akan memberimu "backup key"
yang bisa dipakai sebagai faktor kedua jika kamu kehilangan faktor kedua
sebenarnya (selalu simpan dongle cadangan di tempat aman!). Kamu _bisa_
menaruhnya di pengelola kata sandi, tapi jika seseorang mendapatkan akses ke
pengelola kata sandi, kamu benar-benar celaka (tapi mungkin kamu okay dengan
model ancaman itu). Jika benar-benar paranoid, cetak paper key tersebut, jangan
pernah simpan digital, dan taruh di brankas di dunia nyata.

## Komunikasi Privat

Gunakan [Signal](https://www.signal.org/) ([instruksi
penyiapan](https://medium.com/@mshelton/signal-for-beginners-c6b44f76a1f0).
[Wire](https://wire.com/en/) juga [baik-baik saja](https://www.securemessagingapps.com/);
WhatsApp oke; [jangan gunakan Telegram](https://twitter.com/bascule/status/897187286554628096)).
Messenger desktop cukup bermasalah (sebagian karena biasanya bergantung pada
Electron, yang merupakan tumpukan kepercayaan besar).

Email sangat bermasalah, bahkan jika ditandatangani PGP. Ini umumnya tidak
forward-secure, dan masalah distribusi kuncinya cukup parah.
[keybase.io](https://keybase.io/) membantu, dan berguna untuk sejumlah alasan
lain. Juga, kunci PGP biasanya ditangani di komputer desktop, salah satu
lingkungan komputasi paling tidak aman. Terkait, pertimbangkan membeli
Chromebook, atau bekerja di tablet dengan keyboard.

## Keamanan File

Keamanan file itu sulit dan bekerja di banyak level. Apa yang coba kamu lindungi?

[![$5 wrench](https://imgs.xkcd.com/comics/security.png)](https://xkcd.com/538/)

 - Serangan offline (seseorang mencuri laptopmu saat mati): nyalakan enkripsi
   disk penuh. ([cryptsetup + LUKS](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_a_non-root_file_system)
   di Linux, [BitLocker](https://fossbytes.com/enable-full-disk-encryption-windows-10/)
   di Windows, [FileVault](https://support.apple.com/en-us/HT204837) di macOS.
   Perhatikan ini tidak membantu jika penyerang juga menahanmu dan benar-benar
   menginginkan rahasiamu.
 - Serangan online (seseorang memiliki laptopmu dan sedang menyala): gunakan
   enkripsi file. Ada dua mekanisme utama
    - Sistem berkas terenkripsi: perangkat lunak enkripsi sistem berkas bertingkat
      mengenkripsi file satu per satu alih-alih perangkat blok terenkripsi.
      Kamu bisa "mount" sistem berkas ini dengan memberi kunci dekripsi, lalu
      menjelajah file di dalamnya bebas. Saat kamu unmount, file tersebut tidak
      tersedia. Solusi modern termasuk [gocryptfs](https://github.com/rfjakob/gocryptfs)
      dan [eCryptFS](http://ecryptfs.org/). Perbandingan detail bisa ditemukan
      [di sini](https://nuetzlich.net/gocryptfs/comparison/) dan
      [di sini](https://wiki.archlinux.org/index.php/disk_encryption#Comparison_table)
    - File terenkripsi: enkripsi file individual dengan enkripsi simetris (lihat
      `gpg -c`) dan kunci rahasia. Atau, seperti `pass`, juga enkripsi kuncinya
      dengan kunci publikmu sehingga hanya kamu yang dapat membacanya lagi
      dengan kunci privatmu. Pengaturan enkripsi yang tepat sangat penting!
 - [Plausible deniability](https://en.wikipedia.org/wiki/Plausible_deniability)
   (apa masalahnya, Pak?): biasanya performa lebih rendah, dan lebih mudah
   kehilangan data. Sulit benar-benar membuktikan bahwa ia menyediakan
   [deniable encryption](https://en.wikipedia.org/wiki/Deniable_encryption)!
   Lihat [diskusi ini](https://security.stackexchange.com/questions/135846/is-plausible-deniability-actually-feasible-for-encrypted-volumes-disks),
   lalu pertimbangkan apakah kamu ingin mencoba [VeraCrypt](https://www.veracrypt.fr/en/Home.html)
   (fork terpelihara dari TrueCrypt).
 - Backup terenkripsi: gunakan [Tarsnap](https://www.tarsnap.com/) atau [Borgbase](https://www.borgbase.com/)
    - Pikirkan apakah penyerang dapat menghapus backup-mu jika mereka mendapatkan
      laptopmu!

## Keamanan & Privasi Internet

Internet tempat yang _sangat_ menakutkan. Jaringan WiFi terbuka
[itu](https://www.troyhunt.com/the-beginners-guide-to-breaking-website/)
[menakutkan](https://www.troyhunt.com/talking-with-scott-hanselman-on/).
Pastikan kamu menghapusnya setelahnya, jika tidak ponselmu akan dengan senang
hati mengumumkan dan menyambung lagi ke sesuatu dengan nama yang sama nanti!

Jika kamu berada di jaringan yang tidak kamu percayai, VPN _mungkin_ berguna,
tetapi ingat kamu mempercayai penyedia VPN _sangat_ besar. Apakah kamu benar-benar
lebih percaya mereka daripada ISP? Jika benar-benar ingin VPN, gunakan penyedia
yang benar-benar kamu percaya, dan sebaiknya kamu membayarnya. Atau pasang
[WireGuard](https://www.wireguard.com/) sendiri -- ini
[hebat](https://web.archive.org/web/20210526211307/https://latacora.micro.blog/there-will-be/)!

Ada juga pengaturan konfigurasi aman untuk banyak aplikasi berinternet di
[cipherlist.eu](https://cipherlist.eu/). Jika kamu sangat peduli privasi,
[privacytools.io](https://privacytools.io) juga sumber bagus.

Beberapa dari kalian mungkin bertanya soal [Tor](https://www.torproject.org/).
Ingat bahwa Tor _tidak_ terlalu tahan terhadap penyerang global kuat, dan lemah
terhadap serangan analisis lalu lintas. Ini mungkin berguna untuk menyembunyikan
lalu lintas dalam skala kecil, tapi tidak akan banyak memberi privasi. Lebih
baik gunakan layanan yang lebih aman sejak awal (Signal, TLS + certificate
pinning, dll.).

## Keamanan Web

Jadi, kamu juga ingin berselancar di Web?
Wah, kamu benar-benar menantang keberuntungan.

Pasang [HTTPS Everywhere](https://www.eff.org/https-everywhere).
SSL/TLS itu
[krusial](https://www.troyhunt.com/ssl-is-not-about-encryption/), dan bukan
hanya soal enkripsi, tapi juga memverifikasi bahwa kamu berbicara dengan layanan
yang tepat sejak awal! Jika kamu menjalankan server web sendiri,
[uji](https://www.ssllabs.com/ssltest/index.html). Konfigurasi TLS
[bisa rumit](https://wiki.mozilla.org/Security/Server_Side_TLS). HTTPS Everywhere
akan berusaha sebaik mungkin untuk tidak menavigasimu ke situs HTTP ketika ada
alternatif. Itu tidak menyelamatkanmu, tapi membantu. Jika benar-benar paranoid,
blacklist semua CA SSL/TLS yang tidak benar-benar kamu butuhkan.

Pasang [uBlock Origin](https://github.com/gorhill/uBlock). Ini adalah
[pemblokir spektrum luas](https://github.com/gorhill/uBlock/wiki/Blocking-mode)
yang tidak hanya menghentikan iklan, tetapi segala jenis komunikasi pihak ketiga
yang mungkin dicoba halaman. Juga skrip inline dan semacamnya. Jika kamu mau
meluangkan waktu mengonfigurasi agar semuanya bekerja, gunakan
[mode medium](https://github.com/gorhill/uBlock/wiki/Blocking-mode:-medium-mode)
atau bahkan [mode hard](https://github.com/gorhill/uBlock/wiki/Blocking-mode:-hard-mode).
Mode tersebut _akan_ membuat beberapa situs tidak bekerja sampai kamu cukup
mengotak-atik pengaturan, tetapi juga secara signifikan meningkatkan keamanan
onlinemu.

Jika menggunakan Firefox, aktifkan [Multi-Account Containers](https://support.mozilla.org/en-US/kb/containers).
Buat kontainer terpisah untuk jejaring sosial, perbankan, belanja, dll. Firefox
akan menjaga cookie dan state lainnya untuk setiap kontainer tetap benar-benar
terpisah, sehingga situs yang kamu kunjungi di satu kontainer tidak bisa
mengintip data sensitif dari kontainer lain. Di Google Chrome, kamu bisa memakai
[Chrome Profiles](https://support.google.com/chrome/answer/2364824) untuk
hasil serupa.

Latihan

TODO

1. Enkripsi file menggunakan PGP
1. Gunakan veracrypt untuk membuat volume terenkripsi sederhana
1. Aktifkan 2FA untuk akun yang paling sensitif datanya, mis. GMail, Dropbox,
   Github, dll
