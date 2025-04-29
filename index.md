---
layout: page
title: Semester yang Hilang dalam Pendidikan Ilmu Komputer Anda
nositetitle: true
---
Kelas kuliah mengajarkan berbagai topik lanjutan dalam Ilmu Komputer, mulai 
dari sistem operasi hingga machine learning, tetapi ada satu topik penting 
yang jarang dibahas dan justru dibiarkan untuk dipelajari sendiri oleh mahasiswa: 
kemahiran dalam menggunakan alat-alat mereka. Kami akan mengajarkan cara menguasai command-line, 
menggunakan editor teks yang kuat, memanfaatkan fitur-fitur canggih dari sistem kontrol versi, 
dan masih banyak lagi!

Mahasiswa menghabiskan ratusan jam menggunakan alat-alat ini selama masa pendidikan 
mereka (dan ribuan jam sepanjang karier mereka), jadi masuk akal untuk membuat pengalaman 
tersebut semulus dan semudah mungkin. Menguasai alat-alat ini tidak hanya memungkinkan Anda
menghabiskan lebih sedikit waktu untuk mencari cara menyesuaikan alat dengan kebutuhan Anda, 
tetapi juga memungkinkan Anda memecahkan masalah yang sebelumnya tampak sangat rumit.

Baca tentang [motivasi di balik kelas ini](/about/).

{% comment %}
# Registration

Sign up for the IAP 2020 class by filling out this [registration form](https://forms.gle/TD1KnwCSV52qexVt9).
{% endcomment %}

# Jadwal

{% comment %}
**Lecture**: 35-225, 2pm--3pm<br>
**Office hours**: 32-G9 lounge, 3pm--4pm (every day, right after lecture)
{% endcomment %}

<ul>
{% assign lectures = site['2020'] | sort: 'date' %}
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

Rekaman video dari kuliah tersedia [di YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).

# Informasi Kelas

**Staff**: This class is co-taught by [Anish](https://www.anishathalye.com/), [Jon](https://thesquareplanet.com/), and [Jose](http://josejg.com/).<br>
**Pertanyaan**: Email kami di [missing-semester@mit.edu](mailto:missing-semester@mit.edu).

# Di Luar MIT

Kami juga telah membagikan kelas ini di luar MIT dengan harapan bahwa orang lain juga 
dapat memperoleh manfaat dari sumber daya ini. Anda dapat menemukan postingan dan diskusi di

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

- [Mandarin (Sederhana)](https://missing-semester-cn.github.io/)
- [Mandarin (Tradisional)](https://missing-semester-zh-hant.github.io/)
- [Bahasa Jepang](https://missing-semester-jp.github.io/)
- [Bahasa Korea](https://missing-semester-kr.github.io/)
- [Bahasa Portugis](https://missing-semester-pt.github.io/)
- [Bahasa Rusia](https://missing-semester-rus.github.io/)
- [Bahasa Serbia](https://netboxify.com/missing-semester/)
- [Bahansa Spanyol](https://missing-semester-esp.github.io/)
- [Bahasa Turki](https://missing-semester-tr.github.io/)
- [Bahasa Vietnam](https://missing-semester-vn.github.io/)
- [Bahasa Arab](https://missing-semester-ar.github.io/)
- [Bahasa Italia](https://missing-semester-it.github.io/)
- [Bahasa Persia](https://missing-semester-fa.github.io/)
- [Bahasa Jerman](https://missing-semester-de.github.io/)
- [Bahasa Bengali](https://missing-semester-bn.github.io/)

Catatan: ini adalah tautan eksternal ke terjemahan komunitas. 
Kami belum memverifikasi terjemahan tersebut.

Apakah Anda telah membuat terjemahan dari catatan kuliah dari kelas ini? 
Kirimkan [pull request](https://github.com/missing-semester/missing-semester/pulls) so
Kami dapat menambahkannya ke dalam daftar!

## Penghargaan

Kami mengucapkan terima kasih kepada Elaine Mello, Jim Cain, dan [MIT Open
Learning](https://openlearning.mit.edu/) untuk memungkinkan kami merekam video 
kuliah di atas; Anthony Zolnik dan [MIT
AeroAstro](https://aeroastro.mit.edu/) untuk peralatan A/V; dan Brandi Adams dan
[MIT EECS](https://www.eecs.mit.edu/) untuk dukungan terhadap kelas ini.

---

<div class="small center">
<p><a href="https://github.com/missing-semester/missing-semester">Source code</a>.</p>
<p>Lisensi di bawah CC BY-NC-SA.</p>
<p>Lihat <a href="/license/">di sini</a> untuk panduan kontribusi  &amp; terjemahan.</p>
</div>
