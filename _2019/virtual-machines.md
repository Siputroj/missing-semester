---
layout: lecture
title: "Mesin Virtual dan Kontainer"
presenter: Anish, Jon
video:
  aspect: 56.25
  id: LJ9ki5zq6Ik
---

# Mesin Virtual

Mesin virtual adalah komputer yang disimulasikan. Kamu dapat mengonfigurasi
mesin virtual tamu dengan sistem operasi dan konfigurasi tertentu lalu
menggunakannya tanpa mempengaruhi lingkungan host.

Untuk kelas ini, kamu bisa memakai VM untuk bereksperimen dengan sistem
operasi, perangkat lunak, dan konfigurasi tanpa risiko: kamu tidak akan
mempengaruhi lingkungan pengembangan utama.

Secara umum, VM punya banyak kegunaan. VM biasa digunakan untuk menjalankan
perangkat lunak yang hanya berjalan di sistem operasi tertentu (mis. memakai VM
Windows di Linux untuk menjalankan perangkat lunak spesifik Windows). VM sering
dipakai untuk bereksperimen dengan perangkat lunak yang berpotensi berbahaya.

## Fitur berguna

- **Isolasi**: hypervisor cukup baik mengisolasi tamu dari host, jadi kamu bisa
memakai VM untuk menjalankan perangkat lunak buggy atau tak tepercaya dengan
cukup aman.

- **Snapshot**: kamu bisa mengambil "snapshot" mesin virtual, menangkap seluruh
keadaan mesin (disk, memori, dll.), melakukan perubahan pada mesin, lalu
memulihkan ke keadaan sebelumnya. Ini berguna untuk menguji tindakan yang
berpotensi destruktif, di antara hal lain.

## Kekurangan

Mesin virtual umumnya lebih lambat dibanding menjalankan langsung di hardware,
jadi mungkin tidak cocok untuk aplikasi tertentu.

## Penyiapan

- **Sumber daya**: dibagi dengan mesin host; perhatikan ini saat mengalokasikan
  sumber daya fisik.

- **Jaringan**: banyak opsi, NAT bawaan biasanya cukup untuk sebagian besar
  kasus.

- **Guest addons**: banyak hypervisor dapat memasang perangkat lunak di tamu
  untuk memungkinkan integrasi lebih baik dengan sistem host. Pakai ini jika
  bisa.

## Sumber

- Hypervisor
    - [VirtualBox](https://www.virtualbox.org/) (open-source)
    - [Virt-manager](https://virt-manager.org/) (open-source, mengelola VM KVM
      dan kontainer LXC)
    - [VMWare](https://www.vmware.com/) (komersial, tersedia dari IS&T
      [untuk mahasiswa MIT](https://ist.mit.edu/vmware-fusion))

Jika kamu sudah familier dengan hypervisor/VM populer, kamu mungkin ingin
belajar lebih lanjut tentang cara melakukannya dengan ramah baris perintah.
Salah satu opsi adalah toolkit [libvirt](https://wiki.libvirt.org/page/UbuntuKVMWalkthrough)
yang memungkinkanmu mengelola berbagai penyedia virtualisasi/hypervisor.

## Latihan

1. Unduh dan instal sebuah hypervisor.

1. Buat mesin virtual baru dan instal distribusi Linux (mis.
[Debian](https://www.debian.org/)).

1. Bereksperimenlah dengan snapshot. Cobalah hal-hal yang selalu ingin kamu
   coba, seperti menjalankan `sudo rm -rf --no-preserve-root /`, dan lihat
   apakah kamu bisa pulih dengan mudah.

1. Baca apa itu [fork-bomb](https://en.wikipedia.org/wiki/Fork_bomb)
   (`:(){ :|:& };:`) dan jalankan di VM untuk melihat bahwa isolasi sumber daya
   (CPU, Memori, dll.) bekerja.

1. Instal guest addons dan bereksperimen dengan mode windowing berbeda,
   berbagi file, dan fitur lainnya.

# Kontainer

Mesin Virtual relatif berat; bagaimana jika kamu ingin menyalakan mesin secara
terotomatisasi? Inilah kontainer!

 - Amazon Firecracker
 - Docker
 - rkt
 - lxc

Kontainer _kebanyakan_ hanya kumpulan berbagai fitur keamanan Linux, seperti
virtual file system, antarmuka jaringan virtual, chroot, trik memori virtual,
dan sejenisnya, yang bersama-sama memberi tampilan virtualisasi.

Tidak cukup aman atau terisolasi seperti VM, tapi cukup dekat dan semakin baik.
Biasanya performa lebih tinggi, dan jauh lebih cepat dijalankan, namun tidak
selalu.

Peningkatan performa datang dari fakta bahwa tidak seperti VM yang menjalankan
salinan penuh sistem operasi, kontainer berbagi kernel linux dengan host. Namun
perhatikan jika kamu menjalankan kontainer linux di Windows/macOS, sebuah VM
Linux perlu aktif sebagai lapisan tengah di antara keduanya.

![Docker vs VM](/2019/files/containers-vs-vms.png)
_Perbandingan antara kontainer Docker dan Mesin Virtual. Kredit: blog.docker.com_

Kontainer berguna ketika kamu ingin menjalankan tugas otomatis di setup
standar:

 - Sistem build
 - Lingkungan pengembangan
 - Server yang dikemas
 - Menjalankan program tak tepercaya
   - Menilai pengumpulan tugas mahasiswa
   - (Beberapa) komputasi cloud
 - Integrasi berkelanjutan
   - Travis CI
   - GitHub Actions

Selain itu, perangkat lunak kontainer seperti Docker juga banyak digunakan
sebagai solusi untuk [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell).
Jika sebuah mesin harus menjalankan banyak layanan dengan dependensi konflik,
mereka bisa diisolasi menggunakan kontainer.

Biasanya, kamu menulis sebuah file yang mendefinisikan cara membangun
kontainermu. Kamu mulai dengan _base image_ minimal (seperti Alpine Linux),
lalu daftar perintah untuk menyiapkan lingkungan yang diinginkan (instal
paket, salin file, build, tulis file konfigurasi, dll.). Biasanya, ada juga
cara menentukan port eksternal yang harus tersedia, dan sebuah _entrypoint_
yang menentukan perintah apa yang harus dijalankan saat kontainer dimulai
(mis. skrip penilaian).

Mirip dengan situs repository kode (seperti [GitHub](https://github.com/)) ada
situs repository kontainer (seperti [DockerHub](https://hub.docker.com/)) di
mana banyak layanan perangkat lunak memiliki image siap pakai yang bisa mudah
dideploy.

## Latihan

1. Pilih perangkat lunak kontainer (Docker, LXC, …) dan instal image Linux
   sederhana. Coba SSH ke dalamnya.

1. Cari dan unduh image kontainer siap pakai untuk server web populer (nginx,
   apache, …)
