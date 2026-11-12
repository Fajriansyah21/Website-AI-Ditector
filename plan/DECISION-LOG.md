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

## DEC-002: Target pengguna, cakupan penggunaan, dan hubungan modul

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Target utama Lexora adalah mahasiswa dan pelajar.
  - Cakupan penggunaan meliputi kebutuhan akademik dan umum.
  - AI Text Detector dan AI Humanizer memiliki prioritas setara.
  - Kedua modul dapat digunakan mandiri dan saling terhubung; pengiriman teks dari Detector ke Humanizer selalu memerlukan tindakan pengguna.
  - Bahasa produk adalah Bahasa Indonesia dan Bahasa Inggris. Teks campuran Indonesia-Inggris diterima dengan peringatan keterbatasan.
- **Alasan:** Pilihan eksplisit pemilik proyek selama elisitasi kebutuhan.
- **Dampak:** Use case, pengujian, UX, dan evaluasi model perlu mencakup dua bahasa dan domain akademik/nonakademik. Kemampuan code-switching belum boleh diklaim akurat sebelum diuji.
- **Bukan bagian dari keputusan ini:** target pasar sekunder yang lebih rinci, model final, kualitas deteksi yang dijanjikan, dan desain UI.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-003: Output Detector dan batas klaim

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Detector menampilkan ringkasan keseluruhan, analisis per kalimat, dan penjelasan. Kategori produk adalah HUMAN, AI, MIXED, dan UNCERTAIN. Hasil harus disampaikan sebagai estimasi, bukan bukti pasti penggunaan AI.
- **Alasan:** Pemilik proyek memilih hasil yang informatif sekaligus menyediakan kategori ketidakpastian.
- **Dampak:** Model, threshold, kalibrasi, penjelasan, dan UI harus mendukung ketidakpastian. Skor tidak boleh disebut probabilitas terkalibrasi tanpa bukti kalibrasi.
- **Bukan bagian dari keputusan ini:** definisi label operasional, confidence threshold, metode model, target akurasi, dan tampilan skor numerik.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-004: Personalisasi Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Humanizer menyediakan pilihan gaya bawaan dan kemampuan membentuk gaya personal dari sampel tulisan pengguna. Sampel dapat ditempel atau diunggah. Kecukupan sampel dinilai adaptif. Profil gaya dapat disimpan secara opsional, digunakan kembali, diperbarui, dan dihapus. Pengguna dapat memiliki satu profil utama dan beberapa profil tambahan.
- **Alasan:** Pemilik proyek memilih kontrol berbasis gaya dan personalisasi dari contoh tulisan sendiri.
- **Dampak:** Sistem memerlukan proses profiling, consent, penyimpanan profil privat, pengelolaan profil, dan pengujian konsistensi gaya. Profil harus dipisahkan dari dokumen sampel sumber.
- **Bukan bagian dari keputusan ini:** daftar final gaya bawaan, jumlah profil maksimum, algoritma profiling, bentuk representasi profil, dan retensi sampel mentah.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-005: Input dokumen, batas teks, dan keluaran

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - TXT, DOCX, dan PDF didukung untuk input Detector, Humanizer, dan sampel profil gaya.
  - Ekstraksi dokumen bersifat adaptif dan memberi peringatan jika sebagian konten tidak terbaca.
  - Batas yang ditampilkan kepada pengguna menggunakan karakter; sistem juga membatasi pemrosesan berdasarkan token.
  - Batas input Detector dan Humanizer dapat berbeda.
  - Kedua modul menyediakan keluaran website serta unduhan TXT, DOCX, dan PDF.
  - Pengguna dapat menyalin seluruh hasil atau bagian tertentu.
- **Alasan:** Pilihan eksplisit pemilik proyek.
- **Dampak:** Dibutuhkan ekstraksi dokumen, validasi file, kemungkinan OCR untuk PDF hasil pemindaian, dan generator dokumen keluaran.
- **Bukan bagian dari keputusan ini:** angka karakter/token, batas ukuran file, library ekstraksi/OCR, dan fidelity tata letak.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-006: Akun, dashboard, dan penyimpanan riwayat

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Akses menggunakan model hybrid Guest dan pengguna terdaftar.
  - Autentikasi mendukung email/password dan Google Sign-In.
  - Dashboard mencakup profil, kredit, riwayat, profil gaya, dan pengaturan akun.
  - Riwayat pengguna login bersifat opsional; Guest tidak memiliki riwayat permanen.
  - Teks yang tidak dipilih untuk disimpan dihapus setelah pemrosesan selesai.
- **Alasan:** Pemilik proyek memilih akses mudah untuk Guest sekaligus pengelolaan akun bagi pengguna terdaftar.
- **Dampak:** Sistem memerlukan autentikasi, manajemen sesi, pemisahan data Guest/akun, kontrol penyimpanan, dan kebijakan retensi.
- **Bukan bagian dari keputusan ini:** provider autentikasi, desain account linking, durasi sesi, retensi cache/backup/provider, dan detail legal.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-007: Model kredit, reward, dan pembayaran

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Kredit internal diperoleh dari pembelian, reward iklan, dan bonus harian.
  - Pengguna login menerima bonus 20 kredit pada pukul 03.00; Guest tidak menerima bonus harian.
  - Guest dapat memperoleh reward iklan dengan batas lebih kecil; pengguna login juga dapat memperoleh reward.
  - Biaya fitur menggabungkan biaya dasar dan biaya variabel yang mempertimbangkan konsumsi token.
  - Pemrosesan ditolak jika saldo tidak mencukupi.
  - Kredit pembelian tidak kedaluwarsa; bonus dan reward iklan berlaku 7 hari.
  - Kredit promosi dengan kedaluwarsa terdekat digunakan terlebih dahulu, kemudian kredit pembelian.
  - Refund bergantung pada penyebab dan status kegagalan/pembatalan yang diverifikasi.
  - Pembelian mendukung paket tetap dan jumlah khusus, dengan metode QRIS, e-wallet, transfer bank, dan Virtual Account melalui payment gateway.
  - Guest dapat memulai pembelian, tetapi harus memiliki akun sebelum kredit diberikan.
  - Transaksi gagal/kedaluwarsa dibatalkan dan pembelian berikutnya menggunakan transaksi baru.
- **Alasan:** Pemilik proyek memilih sistem kredit dengan pembelian, reward iklan, dan bonus harian.
- **Dampak:** Diperlukan ledger kredit, expiry per lot, idempotensi, verifikasi pembayaran server-side, rekonsiliasi transaksi, kontrol abuse, dan analisis unit economics.
- **Bukan bagian dari keputusan ini:** harga, jumlah kredit paket, formula biaya final, batas reward, zona waktu bonus, payment gateway, dan produk iklan Google yang sesuai.
- **Catatan validasi:** penggunaan iklan berhadiah harus diperiksa terhadap dokumentasi dan kebijakan produk Google yang tepat. Jangan menganggap Google Ads, AdSense, dan rewarded ads sebagai produk yang dapat dipertukarkan.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-008: Admin Panel, hak akses, dan privasi dukungan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - MVP menyediakan Admin Panel lengkap.
  - Struktur peran dirancang untuk Super Admin, Admin, dan Moderator melalui RBAC; tidak harus banyak akun aktif sejak MVP.
  - Admin menggunakan email/password, 2FA, serta pengelolaan sesi/perangkat.
  - Admin hanya dapat mengakses isi teks yang relevan dengan laporan jika pengguna memberi persetujuan.
  - Akses harus dibatasi, beralasan, dan tercatat dalam audit log.
- **Alasan:** Pemilik proyek memilih kontrol operasional lengkap tanpa mengabaikan privasi teks.
- **Dampak:** Dibutuhkan RBAC, audit log, persetujuan yang dapat ditelusuri, dan pembatasan akses berdasarkan kasus.
- **Bukan bagian dari keputusan ini:** matriks izin final, provider 2FA, jumlah akun admin, dan prosedur investigasi insiden.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-009: Dukungan, notifikasi, dan pemrosesan operasional

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Dukungan pengguna menggunakan formulir dan sistem tiket, bukan live chat.
  - Notifikasi tersedia di website dan email; promosi dipisahkan dan memerlukan persetujuan.
  - Sistem menampilkan gangguan layanan dan menyediakan mekanisme pemulihan.
  - Pemrosesan menggunakan kombinasi langsung dan antrean.
  - Antrean menampilkan status, estimasi waktu berbasis kondisi aktual, dan notifikasi saat selesai.
  - Pengguna dapat membatalkan pekerjaan saat menunggu atau diproses; refund mengikuti hasil pembatalan yang diverifikasi.
  - Prioritas performa adalah keseimbangan kualitas, waktu respons, dan biaya.
- **Alasan:** Pemilik proyek memilih pengalaman operasional yang transparan dan dapat dipulihkan.
- **Dampak:** Dibutuhkan job lifecycle, status service, queue, notifikasi, retry idempotent, dan prosedur cancellation.
- **Bukan bagian dari keputusan ini:** teknologi antrean, target uptime, SLA, response time, dan kapasitas concurrency.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## DEC-010: Fitur yang tidak masuk MVP

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sistem referral dan fitur perbandingan dua teks yang berdiri sendiri tidak masuk MVP. Fitur perbandingan teks asli dan hasil revisi di Humanizer tetap termasuk.
- **Alasan:** Pemilik proyek memilih menjaga fokus pada alur inti Detector dan Humanizer.
- **Dampak:** Tidak perlu membangun kode referral/reward referral atau modul pembanding teks terpisah pada MVP.
- **Sumber:** Sesi analisis kebutuhan 2026-11-12.

## Template keputusan baru

### DEC-XXX: Judul keputusan

- **Tanggal:**
- **Status:** Proposed / Accepted / Rejected / Superseded
- **Keputusan:**
- **Alasan:**
- **Alternatif yang dipertimbangkan:**
- **Dampak:**
- **Sumber/bukti:**


## DEC-011: Hak akses Guest dan AI Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Guest hanya boleh menggunakan AI Text Detector.
  - AI Humanizer hanya tersedia bagi pengguna yang sudah login/terdaftar.
  - Pengguna terdaftar dapat menggunakan AI Text Detector dan AI Humanizer.
  - Reward/kredit Guest hanya relevan untuk penggunaan AI Text Detector; Guest tidak memperoleh akses Humanizer hanya karena memiliki saldo kredit.
- **Alasan:** Konfirmasi eksplisit pemilik proyek saat menyusun User Flow UF-01.
- **Dampak:**
  - Hak akses fitur harus diperiksa di backend, bukan hanya menyembunyikan tombol Humanizer di antarmuka Guest.
  - Alur Guest ke Humanizer harus mengarahkan pengguna untuk login/registrasi sebelum dapat memproses teks.
  - Profil gaya personal dan pengelolaannya hanya tersedia untuk pengguna terautentikasi.
  - Perhitungan kuota/kredit perlu mempertimbangkan entitlement fitur, bukan saldo saja.
- **Bukan bagian dari keputusan ini:** apakah Guest boleh melihat landing page/preview Humanizer, cara membawa teks Detector ke Humanizer setelah login, serta apakah kredit Guest dapat dikonversi atau dipindahkan saat registrasi.
- **Sumber:** Konfirmasi eksplisit pemilik proyek dalam sesi User Flow 2026-11-12.
