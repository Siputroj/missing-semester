---
layout: page
title: The Missing Semester of Your CS Education
subtitle: IAP 2026
nositetitle: true
---

Kelas-kelas mengajarkan berbagai topik lanjutan dalam ilmu komputer, mulai dari sistem operasi hingga pembelajaran mesin, tetapi ada satu topik penting yang jarang dibahas dan justru dibiarkan untuk dipelajari sendiri: kemahiran menggunakan alat. Kami akan mengajarkan cara menguasai command-line, menggunakan editor teks yang kuat, memanfaatkan fitur-fitur canggih dari sistem kontrol versi, dan banyak lagi.

Mahasiswa menghabiskan ratusan jam menggunakan alat-alat ini selama masa pendidikan mereka (dan ribuan jam sepanjang karier), jadi masuk akal jika pengalaman tersebut dibuat senyaman dan seefisien mungkin. Menguasai alat-alat ini bukan hanya mengurangi waktu untuk mencari cara menyesuaikan alat, tetapi juga memungkinkan Anda memecahkan masalah yang sebelumnya tampak mustahil.

Saat ini banyak aspek rekayasa perangkat lunak berubah karena hadirnya alat dan alur kerja yang didukung AI. Jika digunakan dengan tepat dan dengan kesadaran akan kekurangannya, alat-alat ini dapat memberikan manfaat besar bagi praktisi CS dan layak untuk dikuasai. Karena AI adalah teknologi lintas fungsi, tidak ada kuliah khusus AI; kami justru menyisipkan penggunaan alat dan teknik AI terbaru yang relevan di tiap kuliah.

Baca [alasan di balik kelas ini](/about/).

# Pendaftaran

Daftarkan diri untuk kelas IAP 2026 dengan mengisi [formulir pendaftaran](https://forms.gle/j2wMzi7qeiZmzEWy9).

# Jadwal

**Kuliah**: [35-225](https://whereis.mit.edu/?go=35), 1:30--2:30pm (_pengecualian_: 3--4pm pada Jumat 1/16)<br>
**Diskusi**: [OSSU Discord](https://ossu.dev/#community), di `#missing-semester`

<ul>
{% assign lectures = site['2026'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-d/%-m/%y' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[tidak ada kelas]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

Kami masih memfinalisasi silabus untuk perkuliahan IAP 2026, sehingga topik yang kami bahas mungkin sedikit berubah.

Jika Anda tidak sabar menunggu hingga Januari 2026, Anda dapat melihat kuliah dari [penyelenggaraan kursus sebelumnya](/2020/), yang membahas banyak topik serupa.

{% comment %}
Video recordings of the lectures are available [on
YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).
{% endcomment %}

# Informasi Kelas

**Staff**: Kelas ini diajarkan bersama oleh [Anish](https://anish.io/), [Jon](https://thesquareplanet.com/), dan [Jose](http://josejg.com/).<br>
**Pertanyaan**: Kirim email ke [missing-semester@mit.edu](mailto:missing-semester@mit.edu).

# Di Luar MIT

Kami juga telah membagikan kelas ini di luar MIT dengan harapan orang lain dapat memperoleh manfaat dari sumber daya ini. Anda dapat menemukan postingan dan diskusi di

 - [Hacker News](https://news.ycombinator.com/item?id=22226380)
 - [Lobsters](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit)
 - [/r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/)
 - [/r/programming](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/)
 - [Twitter](https://twitter.com/jonhoo/status/1224383452591509507)
 - [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J)

{% comment %}
Some more URLs:

- https://news.ycombinator.com/item?id=27154577
- https://news.ycombinator.com/item?id=34934216
- https://www.reddit.com/r/learnprogramming/comments/nca1v3/mit_the_missing_semester_of_your_cs_education/
- https://www.reddit.com/r/compsci/comments/eyywv8/the_missing_semester_of_your_cs_education_from_mit/
- https://www.reddit.com/r/programming/comments/io7nq3/the_missing_semester_of_your_cs_education_mit/
- https://twitter.com/MIT_CSAIL/status/1349766980413263873
- https://twitter.com/MIT_CSAIL/status/1481676163491659780
- https://twitter.com/MIT_CSAIL/status/1581313961093484545
{% endcomment %}

# Terjemahan Lain

- [Chinese (Simplified)](https://missing-semester-cn.github.io/)
- [Japanese](https://missing-semester-jp.github.io/)
- [Korean](https://missing-semester-kr.github.io/)
- [Portuguese](https://missing-semester-pt.github.io/)
- [Russian](https://missing-semester-rus.github.io/)
- [Serbian](https://netboxify.com/missing-semester/)
- [Spanish](https://missing-semester-esp.github.io/)
- [Turkish](https://missing-semester-tr.github.io/)
- [Vietnamese](https://missing-semester-vn.github.io/)
- [Arabic](https://missing-semester-ar.github.io/)
- [Italian](https://missing-semester-it.github.io/)
- [Persian](https://missing-semester-fa.github.io/)
- [German](https://missing-semester-de.github.io/)
- [Bengali](https://missing-semester-bn.github.io/)

Catatan: ini adalah tautan eksternal ke terjemahan komunitas. Kami belum memverifikasi terjemahan tersebut.

Apakah Anda telah membuat terjemahan catatan kuliah dari kelas ini? Kirimkan [pull request](https://github.com/missing-semester/missing-semester/pulls) agar kami dapat menambahkannya ke daftar!

## Ucapan Terima Kasih

{% comment %}
2026 acks; previous years' acks are on their respective pages
{% endcomment %}

Kami berterima kasih kepada Luis Turino / [SIPB](https://sipb.mit.edu/) atas dukungan mereka pada kelas ini sebagai bagian dari [SIPB IAP 2026](https://sipb.mit.edu/iap/).

---

<div class="small center">
<p><a href="https://github.com/missing-semester/missing-semester">Kode sumber</a>.</p>
<p>Dilisensikan di bawah CC BY-NC-SA.</p>
<p>Lihat <a href="/license/">di sini</a> untuk panduan kontribusi dan terjemahan.</p>
</div>
