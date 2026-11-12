# Lexora Product Overview

Status: **Draft arsip rancangan**  
Nama produk: **Lexora**  
Nama historis: **TextLab**  
Terakhir ditinjau: 2026-11-12

## 1. Tujuan dokumen

Dokumen ini mengarsipkan arah produk yang pernah dibahas agar tidak hilang saat perencanaan sistem dan implementasi dimulai. Dokumen ini bukan spesifikasi final dan tidak mengesahkan detail teknis yang belum diputuskan.

## 2. Gambaran produk

Lexora direncanakan sebagai website untuk membantu pengguna menganalisis dan mengolah teks berbahasa Indonesia melalui tiga kapabilitas produk:

1. **AI Text Detector**: memperkirakan kemungkinan karakteristik teks yang dihasilkan AI, manusia, atau gabungan keduanya.
2. **AI Humanizer**: membantu pengguna menyunting teks agar lebih alami dengan tetap menjaga maksud dan informasi penting.
3. **DOCX Metadata Cleaner**: membersihkan metadata dokumen Microsoft Word berformat DOCX yang tidak diperlukan atau berpotensi mengungkap informasi pembuat/perangkat lunak.

Ketiga fitur memiliki tujuan berbeda. Jangan menganggap hasil detector sebagai bukti kepengarangan, humanizer sebagai jaminan lolos detector, atau metadata cleaner sebagai pengubah isi tulisan.

## 3. Cakupan MVP yang disahkan

Berdasarkan keputusan **DEC-001** di `plan/DECISION-LOG.md`:

- MVP pertama mencakup **AI Text Detector** dan **AI Humanizer**.
- **DOCX Metadata Cleaner** ditunda ke fase setelah MVP.
- Ketiga kapabilitas tetap dipertahankan dalam arah produk jangka panjang.
- Arsitektur harus modular, tetapi tidak perlu membangun infrastruktur untuk modul yang belum masuk MVP.

Keputusan ini menetapkan cakupan produk, bukan urutan implementasi antara Detector dan Humanizer.

## 4. Prinsip produk yang perlu dipertahankan

- Fokus awal bahasa: Bahasa Indonesia.
- Pengguna harus memahami batas akurasi dan keterbatasan AI Text Detector.
- Hasil analisis harus disampaikan sebagai estimasi, bukan vonis pasti.
- Humanizer harus berusaha mempertahankan makna, fakta, dan maksud asli.
- Metadata cleaner, ketika masuk fase berikutnya, harus menjelaskan apa yang dibersihkan dan apa yang tidak disentuh.
- Privasi teks pengguna perlu menjadi persyaratan desain sejak awal.
- Sistem direncanakan modular agar fitur dapat dikembangkan, diuji, dan dipelihara secara terpisah.
- UI/UX harus mengikuti kebutuhan dan spesifikasi produk, bukan sebaliknya.

## 5. Batas ruang lingkup awal

| Area | Rencana awal | Status |
|---|---|---|
| Bahasa | Bahasa Indonesia | Arah produk |
| AI Text Detector | Teks pendek dan panjang | Modul MVP; detail batas belum ditetapkan |
| AI Humanizer | Penyuntingan teks agar lebih alami | Modul MVP; alur dan kebijakan belum final |
| Metadata Cleaner | DOCX saja pada tahap awal | Ditunda setelah MVP |
| Arsitektur | Modular | Ditetapkan sebagai prinsip oleh pemilik proyek |
| Frontend | React dan Next.js pernah disebut sebagai stack prototype | Perlu konfirmasi kondisi repo dan keputusan final |
| Backend/API | Belum ditetapkan | Terbuka |
| Database | Belum ditetapkan | Terbuka |
| Deployment | Belum ditetapkan | Terbuka |

## 6. Tool dan tanggung jawab

- **GitHub**: sumber kebenaran untuk keputusan, rencana, spesifikasi, hasil tes, dan status implementasi.
- **Google Stitch**: eksplorasi dan rancangan UI/UX.
- **Google Antigravity**: implementasi, debugging, pengujian, dan review perubahan.
- **Folder `.skill/`**: tempat skill referensi yang dipilih dan dikelola pemilik proyek. Skill desain dapat memberi panduan, tetapi tidak menggantikan spesifikasi Lexora.

## 7. Hal yang belum diputuskan

- Segmen pengguna dan use case prioritas.
- Urutan implementasi Detector dan Humanizer.
- Batas panjang teks dan format input untuk detector.
- Bentuk output detector, tingkat detail, dan cara menjelaskan confidence.
- Kebijakan penggunaan dan retensi teks pengguna.
- Perilaku humanizer, kontrol gaya, serta mekanisme menjaga fakta.
- Model, backend, database, autentikasi, penyimpanan file, dan deployment.
- Model monetisasi dan batas penggunaan gratis/berbayar.

## 8. Aturan perubahan

Setiap perubahan arah produk harus dicatat sebagai keputusan baru beserta alasan, dampak pada modul lain, dan tanggal. Jangan menimpa riwayat keputusan lama tanpa catatan.
