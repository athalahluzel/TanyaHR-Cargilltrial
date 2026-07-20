# Asisten Virtual HR - CARGILL

Chatbot internal untuk membantu karyawan menemukan video panduan seputar BPJS Kesehatan, BPJS Ketenagakerjaan, dan layanan HR lainnya, tanpa harus menunggu balasan manual dari tim HR.

## Akses

| Platform | Link |
|---|---|
| Website | `https://athalahluzel.github.io/TanyaHR-Cargilltrial/` |
| Telegram | `t.me/videotrial_bot` |

## Arsitektur

```
User → Dialogflow Messenger widget (di website) / Telegram bot
         ↓
      Dialogflow ES (NLU engine, hosted di Google Cloud)
         ↓
      Response: teks / chip menu / link ke halaman video (GitHub Pages)
```

Website ini (HTML statis di GitHub Pages) hanya berfungsi sebagai **tampilan** — semua logika percakapan dan pencocokan intent ditangani oleh **Dialogflow ES**, bukan oleh kode di repo ini.

## Struktur Repo

```
├── index.html              # Landing page, berisi 7 tombol topik + widget Dialogflow Messenger
├── video-jkn.html           # Video player per kategori (7 file, pola nama: video-[kategori].html)
├── video-jmo.html
├── video-jht.html
├── video-jkm.html
├── video-pensiun.html
├── video-jkp.html
├── video-siapkerja.html
├── videos/                  # File .mp4, referenced langsung via <video src="videos/xxx.mp4">
└── images/                  # Logo & background
```

Tidak ada build step — semua file HTML/CSS ditulis manual (single-file, inline `<style>`), tidak pakai framework atau bundler.

## Tentang Dialogflow ES

Bagian "otak" chatbot ini **tidak ada di repo ini** — dikelola terpisah di [Dialogflow Console](https://dialogflow.cloud.google.com), project GCP: `trialbot-501602`.

### Struktur Intent

Tidak pakai context/follow-up intent. Semua intent flat dan independen, dicocokkan murni dari teks yang dikirim (baik hasil ketikan user maupun teks dari chip yang diklik).

```
Default Welcome Intent  → trigger: event WELCOME, tampilkan menu 7 kategori
menu_utama               → trigger: teks "menu" / klik "Kembali ke Menu", isinya sama seperti Welcome
[7 intent kategori]       → trigger: teks kategori persis dari chip menu_utama (mis. "BPJS kesehatan")
[12 intent video]         → trigger: teks judul video persis dari chip intent kategori
```

**Constraint penting:** teks chip di suatu intent harus **exact match** (termasuk kapitalisasi) dengan training phrase di intent tujuan, karena tidak ada fuzzy routing di antaranya — chip cuma ngirim teks biasa yang lalu di-NLU-match seperti input manual.

### Response Format

Tiap intent kategori/video punya response terpisah per platform (tab **Default** dan **Telegram** di UI Dialogflow), karena format custom payload beda:

**Default (Messenger widget di website):**
```json
{ "richContent": [[{ "type": "chips", "options": [...] }]] }
```

**Telegram:**
```json
{ "telegram": { "text": "...", "reply_markup": { "inline_keyboard": [...] } } }
```

Link video di kedua format mengarah ke file di `videos/` folder repo ini (bukan Google Drive — sempat ada masalah akses akun Cargill vs personal Google account, jadi dipindah ke GitHub Pages supaya publicly accessible tanpa auth).

### Menambah Intent Baru

1. Create Intent di Dialogflow Console
2. Isi training phrases
3. Isi response (Default + Telegram kalau perlu keduanya)
4. Kalau perlu muncul di menu, tambahkan chip baru di payload `menu_utama` dengan teks yang **sama persis** dengan training phrase intent baru

### Menghapus Intent

Delete langsung dari list Intents. Cek dulu apakah ada chip di intent lain yang masih reference ke situ (search manual, tidak ada dependency checker otomatis).

## Catatan Teknis

- **Case-sensitive**: nama file di `videos/` folder harus persis sama (termasuk kapitalisasi ekstensi `.mp4` vs `.MP4`) dengan yang direferensikan di kode HTML dan payload Dialogflow. Ini sempat jadi sumber bug (beberapa video "corrupt" ternyata cuma mismatch case).
- GCP project masih **Trial tier** — rawan kena rate limit (429) kalau traffic naik. Perlu enable billing kalau mau dipakai lebih dari sekadar testing (Dialogflow ES sendiri gratis di bawah 180 req/menit, tapi Trial tier GCP punya limit lebih ketat dari itu).
- **Interaction Logging** di Dialogflow aktif secara default (nyimpen isi percakapan user, bisa dilihat di tab History). Perlu dipertimbangkan untuk dimatikan mengingat topik pembicaraan menyangkut data ketenagakerjaan.
- Sempat dicoba integrasi WhatsApp via Twilio (Dialogflow tidak lagi punya native Twilio integration, perlu custom webhook/Cloud Function sebagai jembatan) — masih tahap testing pakai WhatsApp Sandbox, belum production-ready.
