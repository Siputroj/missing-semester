---
layout: lecture
title: "Kontrol Versi"
presenter: Jon
video:
  aspect: 56.25
  id: 3fig2Vz8QXs
---

Kapan pun kamu mengerjakan sesuatu yang berubah seiring waktu, berguna untuk
bisa _melacak_ perubahan itu. Alasan dan keuntungannya banyak: memberimu catatan
apa yang berubah, cara membatalkannya, siapa yang mengubahnya, dan mungkin
bahkan alasannya. Sistem kontrol versi (VCS) memberimu kemampuan itu. Mereka
memungkinkanmu _commit_ perubahan ke sekumpulan file, dengan pesan yang
mendeskripsikan perubahan, serta melihat dan membatalkan perubahan yang telah
kamu lakukan sebelumnya.

Sebagian besar VCS mendukung berbagi riwayat commit antar banyak pengguna.
Ini memungkinkan kolaborasi yang nyaman: kamu bisa melihat perubahan yang saya
buat, dan saya bisa melihat perubahan yang kamu buat. Dan karena VCS melacak
_perubahan_, ia sering kali (walau tidak selalu) bisa mencari cara
menggabungkan perubahan kita selama menyentuh bagian yang cukup terpisah.

Ada [_banyak_](https://en.wikipedia.org/wiki/Comparison_of_version-control_software)
VCS di luar sana yang sangat berbeda dalam dukungan, cara kerja, dan cara
berinteraksi. Di sini, kita fokus pada [git](https://git-scm.com/), salah satu
yang paling umum, tetapi saya sarankan juga melihat
[Mercurial](https://www.mercurial-scm.org/).

Dengan itu semua — mari ke ringkasan cepat!

## Apakah git sihir gelap?

Tidak juga.. kamu perlu memahami model datanya.
Kita akan melewati beberapa detail, tetapi secara garis besar, "inti" git adalah
sebuah commit.

 - setiap commit punya nama unik, "revision hash"
   hash panjang seperti `998622294a6c520db718867354bf98348ae3c7e2`
   sering dipendekkan ke prefiks (agak) unik: `9986222`
 - commit punya penulis + pesan commit
 - juga punya hash _commit leluhur_
   biasanya hanya hash commit sebelumnya
 - commit juga merepresentasikan sebuah _diff_, representasi cara berpindah dari
   leluhur commit ke commit itu (mis., hapus baris ini di file ini, tambah baris
   berikut ke file itu, ganti nama file tersebut, dll.)
   - sebenarnya, git menyimpan kondisi sebelum dan sesudah secara penuh
   - mungkin jangan simpan file besar yang sering berubah!

Awalnya, _repository_ (kurang lebih: folder yang dikelola git) tidak punya
konten dan tidak ada commit. Mari kita siapkan:

```console
$ git init hackers
$ cd hackers
$ git status
```

Keluaran ini sebenarnya memberi titik awal yang bagus. Mari telusuri dan pastikan
kita memahaminya.

Pertama, "On branch master".

 - tidak ingin selalu memakai hash.
 - branch adalah nama yang menunjuk ke hash.
 - master secara tradisional nama untuk commit "terbaru".
   setiap kali commit baru dibuat, nama master akan diarahkan ke hash commit
   baru itu.
 - nama khusus `HEAD` merujuk ke nama "saat ini"
 - kamu juga bisa membuat nama sendiri dengan `git branch` (atau `git tag`)
   kita akan kembali ke sana

Lewati "No commits yet" karena hanya itu saja.

Lalu, "nothing to commit".

 - setiap commit berisi diff dengan semua perubahan yang kamu buat.
   tapi bagaimana diff itu dikonstruksi?
 - _bisa saja_ selalu me-commit _semua_ perubahan sejak commit terakhir
   - kadang kamu hanya ingin me-commit sebagian (mis., bukan `TODO`)
   - kadang kamu ingin memecah sebuah perubahan menjadi beberapa commit untuk
     memberi pesan commit terpisah untuk masing-masing
 - git memungkinkanmu _stage_ perubahan untuk menyusun commit
   - tambahkan perubahan file ke staged changes dengan `git add`
     - tambahkan hanya sebagian perubahan di file dengan `git add -p`
     - tanpa argumen `git add` bekerja pada "semua file yang dikenal"
   - hapus file dan stage penghapusannya dengan `git rm`
   - kosongkan set staged changes `git reset`
     - perhatikan ini _tidak_ mengubah file apa pun!
       ini _hanya_ berarti tidak ada perubahan yang akan dimasukkan ke commit
     - untuk menghapus hanya sebagian staged changes:
       `git reset FILE` atau `git reset -p`
   - periksa staged changes dengan `git diff --staged`
   - lihat sisa perubahan dengan `git diff`
   - ketika senang dengan stage, buat commit dengan `git commit`
     - jika hanya ingin me-commit *semua* perubahan: `git commit -a`
     - `git help add` punya banyak info membantu

Saat bermain dengan hal di atas, coba jalankan `git status` untuk melihat apa
yang git pikir sedang kamu lakukan — itu sangat membantu!

## Sebuah commit, katamu...

Oke, kita punya commit, lalu apa?

 - kita bisa melihat perubahan terbaru: `git log` (atau `git log --oneline`)
 - kita bisa melihat perubahan lengkap: `git log -p`
 - kita bisa menampilkan commit tertentu: `git show master`
   - atau dengan `-p` untuk diff/patch penuh
 - kita bisa kembali ke keadaan pada suatu commit menggunakan `git checkout NAME`
   - jika `NAME` adalah hash commit, git bilang kita "detached". Ini hanya
     berarti tidak ada `NAME` yang merujuk ke commit ini, jadi jika kita membuat
     commit, tidak ada yang tahu tentang mereka.
 - kita bisa membalik sebuah perubahan dengan `git revert NAME`
   - menerapkan diff di commit pada `NAME` secara terbalik.
 - kita bisa membandingkan versi lama dengan versi sekarang menggunakan
   `git diff NAME..`
   - `a..b` adalah _rentang_ commit. jika salah satunya dihilangkan, artinya
     `HEAD`.
 - kita bisa menampilkan semua commit di antaranya menggunakan `git log NAME..`
   - `-p` juga berlaku di sini
 - kita bisa mengubah `master` menunjuk ke commit tertentu (efektif membatalkan
   semuanya sejak itu) dengan `git reset NAME`:
   - huh, kenapa? bukankah `reset` untuk mengubah staged changes?
     reset punya "bentuk" kedua (lihat `git help reset`) yang mengatur `HEAD`
     ke commit yang ditunjuk nama yang diberikan.
   - perhatikan bahwa ini tidak mengubah file apa pun — `git diff` sekarang
     efektif menampilkan `git diff NAME..`.

## Apa arti sebuah nama?

Jelas, nama penting di git. Dan kunci memahami *banyak* hal yang terjadi di git.
Sejauh ini, kita bicara tentang hash commit, master, dan `HEAD`. Tapi ada lagi!

 - kamu bisa membuat branch sendiri (seperti master) dengan `git branch b`
   - membuat nama baru, `b`, yang menunjuk ke commit di `HEAD`
   - kamu masih "di" master, jadi jika membuat commit baru, master akan menunjuk
     ke commit baru itu, `b` tidak.
   - beralih ke branch dengan `git checkout b`
     - commit yang kamu buat kini akan memperbarui nama `b`
     - beralih kembali ke master dengan `git checkout master`
       - semua perubahanmu di `b` tersembunyi
     - cara yang sangat praktis untuk mudah mencoba perubahan
 - tag adalah nama lain yang tidak pernah berubah, dan memiliki pesan sendiri.
   sering dipakai menandai rilis + changelog.
 - `NAME^` artinya "commit sebelum `NAME`"
   - bisa diterapkan berulang: `NAME^^^`
   - kamu _kemungkinan besar_ maksud `~` saat menggunakan `^`
     - `~` bersifat "temporal", sedangkan `^` berdasarkan leluhur
     - `~~` sama dengan `^^`
     - dengan `~` kamu juga bisa menulis `X~3` untuk "3 commit lebih tua dari `X`"
     - kamu tidak ingin `^3`
   - `git diff HEAD^`
 - `-` berarti "nama sebelumnya"
 - sebagian besar perintah beroperasi pada `HEAD` kecuali kamu memberi argumen

## Rapikan kekacauanmu

Riwayat commit-mu _sering sekali_ akan berakhir seperti:

 - `add feature x` — bahkan mungkin dengan pesan commit tentang `x`!
 - `forgot to add file`
 - `fix bug`
 - `typo`
 - `typo2`
 - `actually fix`
 - `actually actually fix`
 - `tests pass`
 - `fix example code`
 - `typo`
 - `x`
 - `x`
 - `x`
 - `x`

Itu _baik-baik saja_ bagi git, tapi tidak terlalu membantu dirimu di masa
depan, atau orang lain yang penasaran apa yang berubah. Git membiarkanmu
merapikannya:

 - `git commit --amend`: gabungkan staged changes ke commit sebelumnya
   - perhatikan ini _mengubah_ commit sebelumnya, memberi hash baru!
 - `git rebase -i HEAD~13` itu _ajaib_.
   untuk setiap commit dari 13 yang lalu, pilih apa yang dilakukan:
   - defaultnya `pick`; tidak melakukan apa-apa
   - `r`: ubah pesan commit
   - `e`: ubah commit (tambah atau hapus file)
   - `s`: gabungkan commit dengan sebelumnya dan sunting pesan commit
   - `f`: "fixup" — gabungkan commit dengan sebelumnya; buang pesan commit
   - di akhir, `HEAD` diarahkan ke commit yang kini terakhir
   - sering disebut _squashing_ commit
   - yang sebenarnya dilakukan: memundurkan `HEAD` ke titik mulai rebase, lalu
     menerapkan kembali commit berurutan sesuai instruksi.
 - `git reset --hard NAME`: atur ulang keadaan semua file ke keadaan `NAME`
   (atau `HEAD` jika tidak diberi nama). Praktis untuk membatalkan perubahan.

## Bermain dengan orang lain

Penggunaan umum kontrol versi adalah memungkinkan banyak orang membuat perubahan
ke sekumpulan file tanpa saling menginjak kaki. Atau lebih tepatnya, memastikan
bahwa _jika_ mereka saling menginjak, mereka tidak diam-diam menimpa perubahan
satu sama lain.

Git adalah VCS _terdistribusi_: semua orang punya salinan lokal seluruh
repository (yah, dari semua yang dipublikasikan orang lain). Beberapa VCS itu
_terpusat_ (mis., subversion): server memiliki semua commit, klien hanya punya
file yang mereka "checkout". Pada dasarnya, mereka hanya punya file _saat ini_,
dan perlu bertanya ke server jika ingin yang lain.

Setiap salinan repository git bisa didaftarkan sebagai "remote". Kamu bisa
menyalin repository git yang ada dengan `git clone ADDRESS` (alih-alih `git
init`). Ini membuat remote bernama _origin_ yang menunjuk ke `ADDRESS`. Kamu
bisa mengambil nama dan commit yang mereka tunjuk dari remote dengan `git fetch
REMOTE`. Semua nama di remote tersedia sebagai `REMOTE/NAME`, dan bisa kamu
gunakan layaknya nama lokal.

Jika kamu punya akses tulis ke remote, kamu bisa mengubah nama di remote untuk
menunjuk ke commit yang kamu buat menggunakan `git push`. Misalnya, mari buat
nama master (branch) di remote `origin` menunjuk ke commit yang saat ini
ditunjuk master kita:

   - `git push origin master:master`
   - demi kenyamanan, kamu bisa mengatur `origin/master` sebagai target bawaan
     saat kamu `git push` dari branch saat ini dengan `-u`
   - pertimbangkan: apa yang dilakukan perintah ini? `git push origin master:HEAD^`

Sering kali kamu akan memakai GitHub, GitLab, BitBucket, atau lainnya sebagai
remote. Tidak ada yang "spesial" tentang itu bagi git. Semuanya hanya nama dan
commit. Jika seseorang membuat perubahan ke master dan memperbarui
`github/master` menunjuk ke commit mereka (kita akan kembali ke itu sebentar),
maka ketika kamu `git fetch github`, kamu bisa melihat perubahan mereka dengan
`git log github/master`.

## Bekerja dengan orang lain

Sejauh ini, branch terlihat cukup sia-sia: kamu bisa membuatnya, bekerja di
dalamnya, lalu apa? Akhirnya, kamu hanya akan membuat master menunjuk ke situ,
bukan?

 - bagaimana jika kamu harus memperbaiki sesuatu saat mengerjakan fitur besar?
 - bagaimana jika orang lain membuat perubahan ke master sementara itu?

Tidak terelakkan, kamu harus _merge_ perubahan di satu branch dengan perubahan
di branch lain, baik perubahan itu dibuat kamu atau orang lain. Git memungkinkan
ini dengan, tidak mengejutkan, `git merge NAME`. `merge` akan:

 - mencari titik terakhir di mana `HEAD` dan `NAME` berbagi commit leluhur
   (yaitu, tempat mereka berpisah)
 - (mencoba) menerapkan semua perubahan itu ke `HEAD` saat ini
 - menghasilkan commit yang berisi semua perubahan itu, dan mencantumkan
   `HEAD` dan `NAME` sebagai leluhurnya
 - mengatur `HEAD` ke hash commit itu

Setelah fitur besar selesai, kamu bisa merge branch-nya ke master, dan git akan
memastikan kamu tidak kehilangan perubahan dari kedua branch!

Jika kamu pernah memakai git sebelumnya, kamu mungkin mengenali `merge` dengan
nama lain: `pull`. Saat kamu menjalankan `git pull REMOTE BRANCH`, itu adalah:

 - `git fetch REMOTE`
 - `git merge REMOTE/BRANCH`
 - di mana, seperti `push`, `REMOTE` dan `BRANCH` sering dihilangkan dan memakai
   remote branch "tracking" (ingat `-u`?)

Ini biasanya bekerja _sangat baik_. Selama perubahan di branch yang di-merge
itu terpisah. Jika tidak, kamu mendapat _merge conflict_. Kedengarannya
menakutkan...

 - merge conflict hanyalah git yang memberitahumu bahwa ia tidak tahu seperti
   apa diff akhir seharusnya
 - git berhenti dan meminta kamu menyelesaikan staging "merge commit"
 - buka file yang konflik di editor dan cari banyak tanda kurung sudut
   (`<<<<<<<`). Bagian di atas `=======` adalah perubahan yang dibuat di `HEAD`
   sejak commit leluhur bersama. Bagian di bawah adalah perubahan yang dibuat di
   `NAME` sejak commit bersama.
 - `git mergetool` cukup membantu — membuka editor diff
 - setelah kamu _menyelesaikan_ konflik dengan memutuskan seharusnya seperti apa
   file sekarang, stage perubahan itu dengan `git add`.
 - ketika semua konflik terselesaikan, akhiri dengan `git commit`
   - kamu bisa menyerah dengan `git merge --abort`

Kamu baru saja menyelesaikan konflik merge git pertamamu! \o/
Sekarang kamu bisa mempublikasikan perubahan selesai dengan `git push`

## Ketika dunia bertabrakan

Saat kamu `push`, git memeriksa bahwa pekerjaan orang lain tidak hilang jika
kamu memperbarui nama remote yang kamu dorong. Ia melakukan ini dengan memeriksa
bahwa commit saat ini dari nama remote adalah leluhur commit yang kamu dorong.
Jika ya, git bisa aman hanya memperbarui nama; ini disebut _fast-forward_. Jika
tidak, git akan menolak memperbarui nama remote, dan memberi tahu ada perubahan.

Jika push-mu ditolak, apa yang kamu lakukan?

 - merge perubahan remote dengan `git pull` (yakni, `fetch` + `merge`)
 - paksa push dengan `--force`: ini akan menghilangkan perubahan orang lain!
   - ada juga `--force-with-lease`, yang hanya memaksa perubahan jika nama
     remote belum berubah sejak terakhir kali kamu fetch dari remote itu. Lebih
     aman!
   - jika kamu merebase commit lokal yang sebelumnya pernah kamu push
     ("menulis ulang riwayat"; sebaiknya jangan lakukan), kamu harus force push.
     Pikirkan alasannya!
 - coba terapkan ulang perubahanmu "di atas" perubahan yang dibuat di remote
   - ini adalah `rebase`!
     - mundurkan semua commit lokal sejak leluhur bersama
     - fast-forward `HEAD` ke commit di nama remote
     - terapkan commit lokal berurutan
       - mungkin ada konflik yang harus kamu selesaikan manual
       - `git rebase --continue` atau `--abort`
     - lebih banyak lagi [di sini](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
   - `git pull --rebase` akan memulai proses ini untukmu
   - apakah sebaiknya merge atau rebase adalah topik panas! bacaan bagus:
     - [ini](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
     - [ini](http://web.archive.org/web/20210106220723/https://derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/)
     - [ini](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge)

# Bacaan lanjut

[![XKCD on git](https://imgs.xkcd.com/comics/git.png)](https://xkcd.com/1597/)

 - [Learn git branching](https://learngitbranching.js.org/)
 - [How to explain git in simple words](https://smusamashah.github.io/blog/2017/10/14/explain-git-in-simple-words)
 - [Git from the bottom up](https://jwiegley.github.io/git-from-the-bottom-up/)
 - [Git for computer scientists](http://eagain.net/articles/git-for-computer-scientists/)
 - [Oh shit, git!](https://ohshitgit.com/)
 - [The Pro Git book](https://git-scm.com/book/en/v2)

# Latihan

1. Di sebuah repo coba ubah file yang ada. Apa yang terjadi ketika kamu menjalankan `git stash`?
   Apa yang kamu lihat saat menjalankan `git log --all --oneline`? Jalankan
   `git stash pop` untuk membatalkan apa yang kamu lakukan dengan `git stash`.
   Dalam skenario apa ini berguna?

1. Kesalahan umum saat belajar git adalah me-commit file besar yang seharusnya
   tidak dikelola git atau menambahkan informasi sensitif. Coba tambahkan file
   ke repository, buat beberapa commit lalu hapus file itu dari riwayat (kamu
   mungkin ingin melihat [ini](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)).
   Juga jika kamu memang ingin git mengelola file besar, lihat
   [Git-LFS](https://git-lfs.github.com/)

1. Git sangat nyaman untuk membatalkan perubahan tetapi kamu harus akrab bahkan
   dengan perubahan paling tidak mungkin
   1. Jika sebuah file salah dimodifikasi di suatu commit, itu dapat dibalik
      dengan `git revert`. Namun jika sebuah commit melibatkan beberapa perubahan
      `revert` mungkin bukan opsi terbaik. Bagaimana kita bisa menggunakan
      `git checkout` untuk mengambil versi file dari commit tertentu?
   1. Buat sebuah branch, buat commit di branch itu lalu hapus. Bisakah kamu
      tetap memulihkan commit itu? Coba lihat `git reflog`. (Catatan: Pulihkan
      hal yang menggantung dengan cepat, git akan secara berkala otomatis
      membersihkan commit yang tidak dirujuk.)
   1. Jika terlalu gembira memakai `git reset --hard` alih-alih `git reset`
      perubahan bisa mudah hilang. Namun karena perubahan tersebut sudah
      di-stage, kita bisa memulihkannya. (lihat `git fsck --lost-found` dan
      `.git/lost-found`)

1. Di repo git mana pun lihat ke folder `.git/hooks` kamu akan menemukan banyak
   skrip yang berakhiran `.sample`. Jika kamu mengganti namanya tanpa
   `.sample` skrip itu akan berjalan sesuai namanya. Misalnya `pre-commit`
   akan dieksekusi sebelum commit. Bereksperimenlah dengan mereka

1. Seperti banyak alat baris perintah, `git` menyediakan file konfigurasi (atau
   dotfile) bernama `~/.gitconfig`. Buat alias menggunakan `~/.gitconfig`
   sehingga ketika kamu menjalankan `git graph` kamu mendapat keluaran
   `git log --oneline --decorate --all --graph` (ini perintah bagus untuk cepat
   memvisualisasikan grafik commit)

1. Git juga membiarkanmu mendefinisikan pola abaikan global di
   `~/.gitignore_global`, ini berguna untuk mencegah kesalahan umum seperti
   menambahkan kunci RSA. Buat file `~/.gitignore_global` dan tambahkan pola
   `*rsa`, lalu uji bahwa itu bekerja di repo.

1. Setelah semakin akrab dengan `git`, kamu akan sering menemui tugas umum,
   seperti mengedit `.gitignore`. [git extras](https://github.com/tj/git-extras/blob/master/Commands.md)
   menyediakan banyak utilitas kecil yang terintegrasi dengan `git`. Misalnya
   `git ignore PATTERN` akan menambahkan pola yang ditentukan ke file
   `.gitignore` di repo dan `git ignore-io LANGUAGE` akan mengambil pola abaikan
   umum untuk bahasa itu dari [gitignore.io](https://www.gitignore.io). Instal
   `git extras` dan coba beberapa alat seperti `git alias` atau `git ignore`.

1. Program GUI Git kadang sangat berguna. Coba jalankan
   [gitk](https://git-scm.com/docs/gitk) di repo git dan jelajahi berbagai bagian
   antarmuka. Lalu jalankan `gitk --all` apa perbedaannya?

1. Setelah terbiasa dengan aplikasi baris perintah, alat GUI bisa terasa
   berat/ribet. Kompromi bagus di antara keduanya adalah alat berbasis ncurses
   yang bisa dinavigasi dari baris perintah namun tetap menyediakan antarmuka
   interaktif. Git punya [tig](https://github.com/jonas/tig), coba instal dan
   jalankan di repo. Kamu bisa menemukan contoh penggunaan
   [di sini](https://www.atlassian.com/blog/git/git-tig).


{% comment %}

 - forced push + `--force-with-lease`
 - git merge/rebase --abort
 - git blame
 - exercise about why rebasing public commits is bad

{% endcomment %}
