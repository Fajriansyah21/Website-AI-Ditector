# Lexora Product Overview

Status: **Draft berdasarkan elisitasi kebutuhan 2026-11-12**  
Nama produk: **Lexora**  
Nama historis: **TextLab**  
Terakhir ditinjau: 2026-11-12

## 1. Tujuan dokumen

Dokumen ini memberi ringkasan arah produk. Rincian kebutuhan yang lebih lengkap berada di [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md). Keputusan yang disahkan dicatat di [Decision Log](DECISION-LOG.md).

## 2. Gambaran produk

Lexora adalah website untuk membantu pengguna menganalisis dan mengolah tulisan dalam konteks akademik maupun umum.

Target utama:
- Mahasiswa.
- Pelajar.

Dua modul MVP memiliki prioritas setara:

1. **AI Text Detector**: memberikan estimasi karakteristik teks dengan kategori HUMAN, AI, MIXED, atau UNCERTAIN. Hasil bukan bukti pasti kepengarangan.
2. **AI Humanizer**: menyunting teks agar lebih alami dan sesuai gaya yang dipilih dengan menjaga makna, fakta, dan maksud asli.

Kedua modul dapat digunakan secara mandiri atau dihubungkan melalui tindakan pengguna. Hasil Detector tidak otomatis dikirim ke Humanizer.

## 3. Cakupan dan bahasa

- MVP: AI Text Detector dan AI Humanizer.
- DOCX Metadata Cleaner: ditunda setelah MVP.
- Bahasa antarmuka dan fitur: Bahasa Indonesia dan Bahasa Inggris.
- Teks campuran Indonesia-Inggris dapat diproses dengan peringatan keterbatasan.
- Input dokumen: TXT, DOCX, PDF untuk Detector, Humanizer, dan sampel profil gaya.
- Output kedua modul: tampilan website dan unduhan TXT, DOCX, PDF.
- Arsitektur: modular.

## 4. Arah pengalaman pengguna

- Guest dan pengguna login dapat mengakses layanan dengan batasan berbeda.
- Pengguna login menggunakan email/password atau Google Sign-In.
- Dashboard mencakup profil, kredit, riwayat opsional, profil gaya, dan pengaturan.
- Detector menyediakan ringkasan, analisis per kalimat, penjelasan, dan kategori ketidakpastian.
- Humanizer menyediakan gaya bawaan dan profil gaya personal, perbandingan teks asli/revisi, serta penyorotan perubahan.
- Riwayat tidak disimpan otomatis. Teks yang tidak dipilih untuk disimpan dihapus setelah pemrosesan sesuai batas retensi yang kelak diverifikasi.
- Sistem kredit mencakup pembelian, reward iklan, dan bonus harian.

## 5. Prinsip produk

- Hasil Detector adalah estimasi, bukan vonis atau bukti pasti penggunaan AI.
- Humanizer tidak menjanjikan hasil pasti lolos AI Detector.
- Pengguna selalu mengendalikan pengiriman teks dari Detector ke Humanizer.
- Makna, fakta, dan maksud teks harus dipertahankan.
- Privasi dan minimisasi data menjadi persyaratan desain.
- Fitur dan modul dikembangkan secara modular.
- Tidak ada klaim akurasi, keamanan, performa, atau retensi yang belum dibuktikan.

## 6. Tool dan tanggung jawab

- **GitHub**: sumber kebenaran untuk keputusan, rencana, spesifikasi, hasil tes, dan status implementasi.
- **Google Stitch**: eksplorasi dan rancangan UI/UX.
- **Google Antigravity**: implementasi, debugging, pengujian, dan review perubahan.
- **Folder .skill/**: tempat skill referensi yang dipilih dan dikelola pemilik proyek. Skill tidak menggantikan spesifikasi Lexora.

## 7. Hal yang belum diputuskan

- Framework, backend, database, storage, queue, hosting, dan deployment.
- Provider/model AI untuk masing-masing modul.
- Payment gateway dan produk iklan Google yang sesuai untuk reward.
- Angka batas karakter/token, ukuran file, dan kapasitas sistem.
- Harga kredit, paket, formula biaya, batas reward, dan zona waktu bonus.
- Daftar final gaya bawaan dan mekanisme teknis profil gaya.
- Target uptime, response time, concurrency, dan SLA dukungan.
- Kebijakan teknis retensi cache, backup, file sementara, dan penyedia AI.

Jangan mengisi hal-hal tersebut dengan asumsi. Gunakan dokumen analisis kebutuhan sebagai dasar tahap System Design dan evaluasi teknologi.
