# TextLab Decision Log

Dokumen ini mencatat keputusan yang telah dibuat, statusnya, dan hal yang masih terbuka. Perubahan keputusan harus ditambahkan sebagai entri baru; jangan menghapus keputusan lama tanpa catatan.

## Keputusan yang ditetapkan

| ID | Area | Keputusan | Status |
|---|---|---|---|
| D-001 | Produk | Nama kerja produk: TextLab | Ditetapkan |
| D-002 | Detector | Bahasa utama Bahasa Indonesia | Ditetapkan |
| D-003 | Detector | Mendukung teks pendek dan panjang | Ditetapkan |
| D-004 | Detector | Model sendiri melalui fine-tuning pretrained backbone | Ditetapkan sebagai pendekatan |
| D-005 | Detector | CPU-first dan model kecil menjadi prioritas | Ditetapkan sebagai prioritas |
| D-006 | Detector | Highlight tingkat kalimat | Ditetapkan |
| D-007 | Detector | Kandidat awal IndoBERT-Lite; belum final | Kandidat |
| D-008 | Humanizer | Gaya dari profil pengguna atau pustaka | Ditetapkan |
| D-009 | Humanizer | Profil personal membutuhkan akun | Ditetapkan |
| D-010 | Humanizer | Gunakan konsep Humanize Kit yang relevan, adaptasi ke backend TextLab | Ditetapkan |
| D-011 | Metadata Cleaner | Format awal DOCX | Ditetapkan |
| D-012 | Akses | Detector dapat digunakan tanpa akun | Ditetapkan |
| D-013 | Akses | Kuota detector gratis 3 penggunaan per hari | Ditetapkan sebagai rancangan |
| D-014 | Monetisasi | Penggunaan tambahan detector melalui iklan | Menunggu verifikasi kebijakan |
| D-015 | Monetisasi | Humanizer dan Metadata Cleaner menggunakan iklan | Menunggu verifikasi kebijakan |
| D-016 | Privasi | Pemrosesan sementara dan penyimpanan minimal | Prinsip ditetapkan |
| D-017 | Dataset | Sumber hybrid: publik terverifikasi dan terkontrol | Ditetapkan |
| D-018 | Dataset | Tiga kelas: HUMAN, AI, MIXED | Ditetapkan |
| D-019 | Dataset | UNCERTAIN di luar kelas training utama | Ditetapkan |
| D-020 | Dataset | Target pilot 5.000–10.000 dokumen | Ditetapkan sebagai target |
| D-021 | Dataset | Split 70/15/15 berbasis group_id | Ditetapkan |
| D-022 | Dataset | Test ketat lintas penulis dan sebagian generator AI | Ditetapkan |
| D-023 | Dataset | Anotasi span-level diturunkan menjadi sentence-level | Ditetapkan |
| D-024 | Dataset | Pilot awal 200–500 dokumen sebelum skala besar | Rekomendasi langkah berikutnya |

## Keputusan yang belum final

1. Model detector final.
2. Konfigurasi training final.
3. Panjang maksimum input dan strategi konteks.
4. Threshold dan aturan abstention.
5. Aturan agregasi skor dokumen.
6. Target latensi dan kapasitas server.
7. Daftar dataset publik yang lolos audit.
8. Daftar generator AI dan kebijakan holdout.
9. Bank topik dan prompt eksperimen.
10. Formulir partisipan dan proses pengumpulan tulisan manusia.
11. Detail retensi dan keamanan data.
12. Format dan ketentuan iklan yang diizinkan.
13. Detail metadata DOCX yang akan dibersihkan.

## Tahap berikutnya

**Data Collection Blueprint**, meliputi:
- Daftar kandidat dataset publik dan status lisensi.
- Daftar generator AI.
- Bank topik dan prompt.
- Formulir pengumpulan tulisan manusia.
- Template editing history.
- Pedoman anotasi lengkap dengan contoh.
- Skema database dan JSON final.
- Aturan deduplikasi dan pembagian kelompok.

Sebelum pengumpulan 5.000–10.000 dokumen, uji proses pada pilot 200–500 dokumen. Jangan menetapkan konfigurasi training final sebelum kualitas data dan benchmark awal diperiksa.
