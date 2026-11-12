# Lexora Decision Log

Dokumen ini mencatat keputusan produk dan teknis yang disahkan. Usulan dan hipotesis tidak boleh dicatat sebagai keputusan final.

## DEC-001: Cakupan modul MVP

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** MVP Lexora mencakup dua modul utama: **AI Text Detector** dan **AI Humanizer**.
- **Ditunda:** **DOCX Metadata Cleaner** tidak masuk cakupan MVP pertama dan dipertimbangkan untuk fase setelah MVP.
- **Alasan:** Pemilik proyek memilih memprioritaskan Detector dan Humanizer terlebih dahulu.
- **Dampak:**
  - Analisis sistem, user flow, flowchart, spesifikasi, dan implementasi MVP difokuskan pada Detector dan Humanizer.
  - DOCX Metadata Cleaner tetap menjadi rencana produk yang diarsipkan, tetapi tidak boleh masuk scope MVP tanpa keputusan baru.
  - Prinsip arsitektur modular tetap berlaku agar modul DOCX Cleaner dapat ditambahkan kemudian tanpa memaksa implementasi sekarang.
  - Shared services hanya dirancang jika memang dibutuhkan oleh dua modul MVP, bukan dibuat berlebihan untuk fitur masa depan.
- **Bukan bagian dari keputusan ini:** framework, backend, database, provider/model, autentikasi, monetisasi, detail UI, dan urutan rilis internal kedua modul.
- **Sumber:** Konfirmasi eksplisit pemilik proyek dalam percakapan 2026-11-12.

## Template keputusan baru

### DEC-XXX: Judul keputusan

- **Tanggal:**
- **Status:** Proposed / Accepted / Rejected / Superseded
- **Keputusan:**
- **Alasan:**
- **Alternatif yang dipertimbangkan:**
- **Dampak:**
- **Sumber/bukti:**
