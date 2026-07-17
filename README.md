# Asisten Virtual HR - CARGILL

Chatbot internal untuk membantu karyawan menemukan informasi dan video panduan seputar BPJS Kesehatan, BPJS Ketenagakerjaan, dan layanan HR lainnya secara mandiri, kapan saja.

## Tentang Proyek Ini

Proyek ini adalah **prototipe/blueprint** chatbot layanan HR yang dibangun menggunakan **Dialogflow ES** (Google) sebagai mesin percakapan, dengan tampilan web yang di-hosting lewat **GitHub Pages**. 
Chatbot ini menjawab pertanyaan karyawan dengan mengarahkan mereka ke video tutorial yang relevan, tanpa perlu menghubungi tim HR secara langsung untuk pertanyaan-pertanyaan umum.

## Fitur

- **Menu interaktif** — pengguna memilih topik lewat tombol (chip), bukan harus mengetik pertanyaan
- **7 kategori utama** yang tersedia:
  - BPJS Kesehatan (JKN)
  - BPJS Ketenagakerjaan (JMO)
  - Jaminan Hari Tua (JHT)
  - Jaminan Kematian (JKM)
  - Jaminan Pensiun
  - Jaminan Kehilangan Pekerjaan (JKP)
  - Akun Siap Kerja & PHK
- **Video panduan langkah-demi-langkah** untuk setiap topik, dapat diputar langsung di halaman web
- **Jalur "Hubungi HR"** untuk pertanyaan yang tidak tercakup di menu, lengkap dengan tombol WhatsApp
- **Multi-platform** — dapat diakses lewat halaman web maupun Telegram

## Cara Mengakses

| Platform | Link |
|---|---|
| Website | `https://athalahluzel.github.io/TanyaHR-Cargilltrial/` |
| Telegram | `t.me/videotrial_bot` |

## Struktur File

```
├── index.html              # Halaman utama (daftar topik + widget chat)
├── video-jkn.html           # Halaman video BPJS Kesehatan
├── video-jmo.html           # Halaman video BPJS Ketenagakerjaan
├── video-jht.html           # Halaman video Jaminan Hari Tua
├── video-jkm.html           # Halaman video Jaminan Kematian
├── video-pensiun.html       # Halaman video Jaminan Pensiun
├── video-jkp.html           # Halaman video Jaminan Kehilangan Pekerjaan
├── video-siapkerja.html     # Halaman video Akun Siap Kerja & PHK
├── videos/                  # Folder berisi seluruh file video (.mp4)
└── images/                  # Folder berisi aset gambar (logo, background)
```

## Teknologi yang Digunakan

- **Dialogflow ES** — mesin NLU (Natural Language Understanding) yang memahami maksud pertanyaan pengguna
- **Dialogflow Messenger** — widget chat yang ditampilkan di halaman web
- **HTML/CSS** — struktur dan tampilan halaman web
- **GitHub Pages** — hosting gratis untuk halaman web dan video

## Alur Percakapan

```
Pengguna membuka chat
   → Sapaan awal + menu 7 kategori
   → Pilih kategori
   → Muncul sub-topik/video terkait
   → Klik video untuk menonton
   → Bisa kembali ke menu kapan saja
```

## Catatan Pengembangan

- Chatbot ini masih berstatus **percobaan/pengembangan** dan dapat terus disempurnakan
- Video disimpan langsung di repository ini (bukan Google Drive) untuk menghindari kendala akses akun
- Untuk pertanyaan di luar topik yang tersedia, pengguna diarahkan untuk menghubungi tim HR langsung

## Kontak

Untuk pertanyaan seputar proyek ini, hubungi tim HR melalui WhatsApp yang tertera di dalam chatbot.

---
*Dibangun sebagai bagian dari inisiatif digitalisasi layanan HR internal.*
