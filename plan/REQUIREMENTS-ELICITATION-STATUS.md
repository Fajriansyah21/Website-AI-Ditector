# Requirements Elicitation Status

- **Produk:** Lexora
- **Tanggal rekap:** 2026-11-14
- **Status keseluruhan:** Elisitasi kebutuhan masih berlangsung
- **Cakupan MVP:** AI Text Detector dan AI Humanizer
- **Sumber utama:** [Decision Log](DECISION-LOG.md) dan [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md)

## 1. Ringkasan kuantitatif dokumentasi

Berdasarkan pemeriksaan dokumen GitHub pada 2026-11-14:

- **137** catatan keputusan, DEC-001 sampai DEC-137.
- **135** keputusan berstatus `Accepted`.
- **2** keputusan berstatus `Superseded`, yaitu DEC-018 dan DEC-019. Keduanya dipertahankan sebagai jejak historis, bukan aturan aktif.
- **188** baris kebutuhan fungsional pada SRS.
- **117** requirement Humanizer, FR-HUM-01 sampai FR-HUM-117.
- **11** requirement Detector, FR-DET-01 sampai FR-DET-11.

Rincian jumlah requirement lain dalam SRS:

| Kelompok | Jumlah |
|---|---:|
| Umum (FR-GEN) | 9 |
| File dan ekstraksi (FR-FILE) | 9 |
| AI Text Detector (FR-DET) | 11 |
| AI Humanizer (FR-HUM) | 117 |
| Akun (FR-ACC) | 15 |
| Kredit (FR-CRED) | 16 |
| Dukungan (FR-SUP) | 3 |
| Notifikasi (FR-NOT) | 3 |
| Operasional (FR-OPS) | 5 |
| **Total** | **188** |

Jumlah baris requirement bukan ukuran kelengkapan produk. Requirement masih perlu ditinjau, diuji konsistensinya, dan diberi acceptance criteria.

## 2. Materi yang sudah dibahas

### 2.1 Fondasi produk dan cakupan MVP

**Keputusan terkait:** DEC-001 sampai DEC-014, dengan DEC-003 juga menjadi dasar kebutuhan Detector.

Pokok pembahasan:
- MVP terdiri dari AI Text Detector dan AI Humanizer.
- DOCX Metadata Cleaner ditunda ke fase setelah MVP.
- Target utama mahasiswa dan pelajar, dengan penggunaan akademik maupun umum.
- Detector dan Humanizer memiliki prioritas setara dan dapat digunakan mandiri.
- Hasil Detector hanya dikirim ke Humanizer setelah tindakan pengguna.
- Bahasa utama Bahasa Indonesia dan Bahasa Inggris; teks campuran diterima dengan peringatan keterbatasan.
- Hak akses Guest dan pengguna terdaftar.
- Draft Guest dipertahankan ketika autentikasi dibatalkan dan dibawa setelah autentikasi berhasil.
- Fondasi akun, dashboard, kredit, reward, pembayaran, Admin Panel, dukungan, notifikasi, dan fitur di luar MVP.

### 2.2 AI Text Detector

**Keputusan terkait:** DEC-003 dan DEC-015 sampai DEC-019.

Pokok pembahasan:
- Ringkasan analisis keseluruhan, analisis per kalimat, dan penjelasan.
- Kategori produk HUMAN, AI, MIXED, dan UNCERTAIN.
- Hasil merupakan estimasi, bukan bukti pasti kepengarangan.
- Input teks langsung dan unggahan TXT, DOCX, atau PDF.
- Batas maksimum input berlaku sama bagi Guest dan pengguna terdaftar, tetapi angka final belum ditetapkan.
- Tidak ada batas minimum karakter input Detector yang disepakati.

**Klarifikasi penting:** DEC-018 dan DEC-019 berstatus Superseded. DEC-020 mengklarifikasi bahwa batas minimum karakter berlaku untuk setiap sampel profil gaya Humanizer, bukan untuk input Detector.

**Status:** Fondasi kebutuhan sudah ada, tetapi elisitasi Detector belum menyeluruh. Kebutuhan tentang penjelasan, tingkat keyakinan, batasan bahasa/domain, hasil laporan, perilaku kasus sulit, serta kondisi gagal masih perlu diaudit dan dilengkapi.

### 2.3 Profil gaya personal Humanizer

**Keputusan terkait:** DEC-020 sampai DEC-051.

Pokok pembahasan:
- Sampel dapat ditempel atau diunggah dan dikelola sebagai entri terpisah.
- Kecukupan dinilai terhadap kumpulan sampel secara adaptif.
- Perubahan sampel memicu evaluasi ulang.
- Profil yang tidak memadai tetap terlihat tetapi tidak dapat dipilih.
- Kegagalan teknis evaluasi dibedakan dari hasil penilaian sampel.
- Versi aktif sebelumnya dipertahankan selama evaluasi/pembangunan versi baru.
- Versioning, masa retensi historis, pemulihan versi, batas versi historis, prioritas antrean, dan penanganan konkurensi.
- Snapshot evaluasi yang konsisten dan batas pengulangan evaluasi otomatis.
- Metadata audit evaluasi dan kontrol akses investigasi Admin.
- Masa persetujuan, pencabutan, notifikasi, serta retry notifikasi akses investigasi.

### 2.4 Input, pemrosesan, hasil, dan riwayat Humanizer

**Keputusan terkait:** DEC-052 sampai DEC-070.

Pokok pembahasan:
- Penggabungan teks hasil ekstraksi dan teks manual dalam satu editor.
- Maksimal satu dokumen unggahan per proses.
- Konfirmasi sebelum mengganti dokumen.
- Pengeditan bebas sebelum pemrosesan.
- Format keluaran mengikuti sumber dokumen atau dipilih pengguna untuk input manual.
- Hasil tidak disimpan otomatis.
- Riwayat menyimpan teks asli dan hasil revisi jika pengguna memilih menyimpan.
- Penghapusan entri riwayat.
- Kapasitas riwayat berdasarkan ukuran data, bukan jumlah entri.
- Ambang peringatan 90%, indikator kapasitas, banner, dan perilaku ketika kapasitas penuh.
- Hasil tetap dapat disalin/diunduh dan dapat disimpan kembali tanpa pemrosesan ulang setelah kapasitas tersedia.

### 2.5 Hasil belum tersimpan dan aksesibilitas

**Keputusan terkait:** DEC-071 sampai DEC-086.

Pokok pembahasan:
- Hasil yang belum disimpan hilang setelah halaman/sesi ditutup.
- Konfirmasi sebelum navigasi yang meninggalkan hasil.
- Perilaku tombol Tetap di halaman, Lanjutkan, X, Escape, dan backdrop.
- Pemeliharaan kondisi halaman saat navigasi dibatalkan.
- Focus trap, fokus awal, pengembalian fokus, dan dukungan screen reader.
- Modal menggunakan semantik alert dialog dan pengumuman yang terkoordinasi.

### 2.6 Batasan Humanizer, sampel, dan file

**Keputusan terkait:** DEC-087 sampai DEC-105.

Pokok pembahasan:
- Input Humanizer maksimum 50.000 karakter dan minimum 100 karakter per pekerjaan.
- Setiap sampel profil gaya minimum 500 karakter.
- Maksimal 6 profil gaya per akun.
- Profil privat dan dikelola pemilik akun.
- Penghapusan profil dan pergantian profil utama.
- Maksimal 20 sampel per profil.
- Ukuran maksimal file sampel 10 MB dan maksimal 5 file per batch.
- TXT, DOCX, PDF; ekstraksi independen per file.
- Pratinjau dan konfirmasi teks hasil ekstraksi.
- Nama sampel, aturan duplikasi, karakter Unicode, dan batas panjang nama.
- Pengunduhan sampel individual dan ZIP.

### 2.7 Recently Deleted, aktivitas akun, dan penghapusan akun

**Keputusan terkait:** DEC-106 sampai DEC-119.

Pokok pembahasan:
- Sampel terhapus dapat dipulihkan selama 30 hari.
- Konflik nama saat pemulihan.
- Restore All bersifat atomik.
- Penghapusan permanen sampel dan audit log.
- Riwayat aktivitas penghapusan untuk pengguna dengan retensi 90 hari.
- Penghapusan riwayat aktivitas dan sinkronisasi lintas perangkat.
- Perilaku riwayat saat akun dihapus.
- Pembatalan permintaan penghapusan akun.
- Masa penundaan penghapusan akun 30 hari.
- Akses terbatas selama status Pending Deletion.

### 2.8 Ekspor data pribadi dan riwayat ekspor

**Keputusan terkait:** DEC-120 sampai DEC-137.

Pokok pembahasan:
- Ekspor data pribadi dalam satu paket ZIP.
- Isi paket dan pengecualian data sensitif.
- Maksimal 3 permintaan ekspor dalam jendela bergulir 24 jam.
- Paket berlaku 24 jam dan dapat diunduh ulang selama tersedia.
- Pembatalan proses ekspor dan satu proses aktif per akun.
- Notifikasi hasil ekspor melalui aplikasi dan email.
- Halaman riwayat ekspor dan retensi 90 hari.
- Penghapusan satu entri atau seluruh riwayat.
- Sinkronisasi lintas perangkat dan audit log internal.
- Notifikasi penghapusan hanya dalam aplikasi.
- Autentikasi ulang untuk penghapusan seluruh riwayat dan masa berlaku 5 menit.
- Tidak ada pembatalan setelah konfirmasi akhir.
- Penghapusan seluruh riwayat bersifat atomik.
- Percobaan ulang memerlukan autentikasi ulang dan konfirmasi baru.
- Rate limiting jeda minimal 30 detik setelah kegagalan, dihitung per akun lintas perangkat.

## 3. Status per area

| Area | Status saat ini | Catatan |
|---|---|---|
| Fondasi produk | Sudah dibahas | Cakupan MVP dan aturan dasar sudah tercatat |
| Humanizer | Elisitasi mendalam, belum ditutup | 117 FR; masih ada isu produk/teknis dan acceptance criteria |
| Detector | Baru sebagian | 11 FR; perlu sesi elisitasi lanjutan |
| Akun dan dashboard | Fondasi tercatat | 15 FR; detail alur dan kondisi tepi perlu dilengkapi |
| Kredit dan pembayaran | Fondasi tercatat | 16 FR; aturan ekonomi dan operasional belum final |
| Dukungan dan notifikasi | Fondasi tercatat | Detail SLA, alur tiket, dan kondisi gagal perlu dilengkapi |
| Admin dan operasional | Fondasi tercatat | Matriks RBAC dan alur operasional rinci belum lengkap |
| Audit konsistensi | Belum diselesaikan | Perlu pemeriksaan lintas requirement dan keputusan |
| Acceptance criteria | Belum diselesaikan | Belum semua requirement memiliki kriteria uji terukur |
| Wireframe/System Design/implementasi | Belum dimulai | Tidak boleh dimulai sebelum gerbang kebutuhan dipenuhi |

## 4. Hal yang masih terbuka

### 4.1 Humanizer

- Provider/model AI dan estimasi biaya.
- Daftar final gaya bawaan.
- Algoritma profiling serta kriteria adaptif untuk menilai kecukupan keseluruhan kumpulan sampel.
- Bentuk representasi profil dan retensi sampel mentah.
- Batas token internal, strategi chunking, penjagaan konteks, dan batas provider.
- Library ekstraksi dokumen, OCR, serta generator TXT/DOCX/PDF.
- Target kualitas, metrik, protokol evaluasi, dan dataset benchmark Humanizer.
- Nilai kuota penyimpanan riwayat, satuan penghitungan, dan detail teknis penghitungan kapasitas.
- Kebijakan teknis retensi cache, file sementara, backup, serta data yang diproses penyedia AI.
- Acceptance criteria terukur.

Rincian berada di bagian 8 [AI Humanizer Plan](AI-HUMANIZER-PLAN.md) dan bagian 8 SRS.

### 4.2 AI Text Detector

- Angka batas maksimum input dan batas token internal.
- Definisi operasional label serta panduan anotasi.
- Confidence, threshold, dan kalibrasi.
- Penjelasan hasil yang dapat ditelusuri.
- Perilaku untuk teks pendek, campuran bahasa, domain berbeda, dan hasil tidak pasti.
- Strategi chunking dan konteks.
- Format laporan final.
- Target kualitas, monitoring drift, dan evaluasi ulang.
- Model dan skema inference final.

Rincian berada di [AI Text Detector Plan](AI-TEXT-DETECTOR-PLAN.md) dan bagian 8 SRS.

### 4.3 Akun, kredit, pembayaran, Admin, dan operasional

- Detail halaman dan pengaturan dashboard.
- Zona waktu serta periode bonus harian.
- Batas reward iklan Guest/login dan kredit per iklan.
- Formula biaya per fitur, tarif, pembulatan, dan unit economics.
- Paket, harga, pembelian, refund, dan payment gateway.
- Produk iklan yang sesuai kebijakan.
- Detail guest checkout dan pemulihan transaksi.
- Matriks RBAC rinci.
- SLA dukungan tiket dan retensi tiket.
- Detail notifikasi, observability, dan pemulihan operasional.

Daftar lengkap dan statusnya mengacu pada bagian 8 SRS.

## 5. Pertanyaan terakhir yang belum diputuskan

**Pertanyaan 138 belum dijawab dan belum menjadi keputusan.**

Topik: titik awal penghitungan jeda minimal 30 detik untuk rate limiting percobaan ulang penghapusan seluruh riwayat ekspor.

Jangan menambahkan aturan tentang sumber waktu atau titik awal hitung mundur ke SRS/Decision Log sebelum pengguna menyetujuinya.

## 6. Urutan pekerjaan berikutnya

1. Audit kelengkapan dan konsistensi kebutuhan Humanizer; jangan memperpanjang cabang pertanyaan yang sudah cukup rinci sebelum memeriksa area yang belum dibahas.
2. Lanjutkan elisitasi Humanizer pada gap yang benar-benar terbuka.
3. Lakukan elisitasi AI Text Detector secara menyeluruh.
4. Lengkapi akun, dashboard, kredit, transaksi, pembayaran, Admin, dukungan, dan operasional.
5. Audit konflik, dependensi, duplikasi, serta keputusan superseded.
6. Susun acceptance criteria yang dapat diuji untuk setiap requirement.
7. Konsolidasikan SRS dan Feature Plans.
8. Minta review pemilik proyek sebelum user flow, wireframe, Google Stitch, atau System Design.

## 7. Batas tahap

Rekap ini tidak mengesahkan:
- Pemilihan framework, database, provider AI, payment gateway, atau deployment.
- Wireframe atau desain visual.
- System Design final.
- Implementasi aplikasi.
- Klaim bahwa fitur sudah dibangun atau diuji.

Roadmap dan prioritas riset dataset tetap mengikuti [Active Roadmap](ACTIVE-ROADMAP.md) serta instruksi proyek di `AGENTS.md`.
