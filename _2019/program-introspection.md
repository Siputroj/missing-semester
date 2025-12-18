---
layout: lecture
title: "Introspeksi Program"
presenter: Anish
video:
  aspect: 62.5
  id: 74MhV-7hYzg
---

# Debugging

Saat debugging dengan printf tidak cukup: gunakan debugger.

Debugger memungkinkanmu berinteraksi dengan eksekusi program, misalnya:

- menghentikan eksekusi program ketika mencapai baris tertentu
- melangkah satu per satu melalui program
- memeriksa nilai variabel
- masih banyak fitur lanjut lainnya

## GDB/LLDB

[GDB](https://www.gnu.org/software/gdb/) dan [LLDB](https://lldb.llvm.org/).
Mendukung banyak bahasa mirip C.

Mari lihat [example.c](/2019/files/example.c). Kompilasi dengan flag debug:
`gcc -g -o example example.c`.

Buka GDB:

`gdb example`

Beberapa perintah:

- `run`
- `b {nama fungsi}` - pasang breakpoint
- `b {file}:{baris}` - pasang breakpoint
- `c` - lanjutkan
- `step` / `next` / `finish` - masuk / lompat lewat / keluar
- `p {variabel}` - cetak nilai variabel
- `watch {ekspresi}` - pasang watchpoint yang terpicu ketika nilai ekspresi berubah
- `rwatch {ekspresi}` - pasang watchpoint yang terpicu ketika nilai dibaca
- `layout`

## PDB

[PDB](https://docs.python.org/3/library/pdb.html) adalah debugger Python.

Sisipkan `import pdb; pdb.set_trace()` di tempat kamu ingin masuk ke PDB,
pada dasarnya hibrida antara debugger (seperti GDB) dan shell Python.

## Developer Tools di peramban web

Contoh lain debugger, kali ini dengan antarmuka grafis.

# strace

Amati system call yang dibuat program: `strace {program}`.

# Profiling

Jenis profiling: CPU, memori, dll.

Profiler paling sederhana: `time`.

## Go

Jalankan kode uji dengan CPU profiler: `go test -cpuprofile=cpu.out`

Analisis profil: `go tool pprof -web cpu.out`

Jalankan kode uji dengan Memory profiler: `go test -memprofile=mem.out`

Analisis profil: `go tool pprof -web mem.out`

## Perf

Statistik performa dasar: `perf stat {command}`

Jalankan program dengan profiler: `perf record {command}`

Analisis profil: `perf report`
