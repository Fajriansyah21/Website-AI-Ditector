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


## DEC-012: Membawa teks input Guest setelah autentikasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:**
  - Jika Guest memulai pendaftaran/login dari alur AI Text Detector, teks input yang sedang dikerjakan dibawa ke sesi setelah autentikasi berhasil.
  - Hasil analisis yang diperoleh saat masih Guest tidak dibawa sebagai hasil final.
  - Setelah autentikasi, pengguna menjalankan analisis ulang sebagai permintaan baru dengan akun terdaftar dan aturan kredit akun tersebut.
  - Pemindahan teks input sementara tidak berarti teks otomatis masuk ke riwayat permanen.
- **Alasan:** Pemilik proyek memilih opsi 2 agar pengguna tidak perlu memasukkan ulang teks setelah membuat akun, tanpa menganggap hasil analisis Guest sebagai hasil akun terdaftar.
- **Alternatif yang dipertimbangkan:** Tidak membawa teks input; membawa teks input sekaligus hasil analisis.
- **Dampak:**
  - Alur autentikasi perlu mempertahankan draft input Detector selama perpindahan halaman/proses login.
  - Sistem harus menjalankan analisis baru setelah autentikasi, bukan memakai ulang hasil Guest.
  - Biaya/kredit analisis ulang mengikuti akun terdaftar dan harus ditampilkan sebelum pemrosesan.
  - Draft sementara harus dipisahkan dari fitur riwayat dan tidak boleh otomatis disimpan sebagai riwayat.
  - Jika Guest membatalkan autentikasi dan kembali ke Detector, teks tetap tersedia selama sesi browser yang sama masih aktif.
  - Draft tidak dijanjikan dapat dipulihkan setelah sesi browser berakhir.
- **Belum diputuskan:** mekanisme teknis penyimpanan sementara, definisi batas sesi browser, serta perilaku saat halaman di-refresh atau tab ditutup tetapi sesi browser lain masih aktif.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat menyusun User Flow UF-01 pada 2026-11-12.


## DEC-013: Mempertahankan draft Guest ketika autentikasi dibatalkan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika Guest memulai autentikasi dari AI Text Detector lalu membatalkannya, teks input tetap tersedia ketika pengguna kembali ke Detector selama sesi browser yang sama masih aktif. Draft tidak dijanjikan dapat dipulihkan setelah sesi browser berakhir.
- **Alasan:** Pemilik proyek memilih opsi 2 agar perpindahan ke halaman autentikasi tidak menyebabkan teks hilang, tanpa memperluas MVP menjadi penyimpanan draft lintas sesi.
- **Alternatif yang dipertimbangkan:** Menghapus teks saat autentikasi dibatalkan; menyimpan draft sampai waktu kedaluwarsa dan memulihkannya lintas pemuatan halaman/sesi.
- **Dampak:** Alur autentikasi perlu mempertahankan draft dalam konteks sesi browser yang sama. Draft tetap bersifat sementara dan bukan riwayat pengguna.
- **Belum diputuskan:** mekanisme teknis penyimpanan, definisi batas sesi browser, perilaku saat halaman di-refresh, dan kondisi ketika tab ditutup tetapi browser tetap berjalan.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat menyusun User Flow UF-01 pada 2026-11-12.


## DEC-014: Klarifikasi makna kuota Guest dan pengguna terdaftar

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Perbedaan kuota yang dimaksud antara Guest dan pengguna terdaftar adalah jumlah kredit/reward yang tersedia, bukan perbedaan batas maksimal karakter input.
- **Klarifikasi ruang lingkup:** Batas karakter input Guest dan pengguna terdaftar belum diputuskan apakah sama atau berbeda. Tidak boleh menyimpulkan adanya batas karakter yang lebih rendah untuk Guest dari keputusan kuota kredit ini.
- **Dampak:** Persyaratan akun dan kredit harus menjelaskan perbedaan saldo/insentif kredit secara terpisah dari validasi panjang input Detector. Batas karakter perlu ditentukan berdasarkan kebutuhan produk, kemampuan model, biaya, dan pengujian, bukan status akun secara otomatis.
- **Sumber:** Koreksi eksplisit pemilik proyek saat membahas UF-02 pada 2026-11-12.


## DEC-015: Batas maksimal karakter input AI Text Detector

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** AI Text Detector harus menerapkan batas maksimal jumlah karakter untuk input teks.
- **Belum diputuskan:** angka maksimal karakter, apakah batasnya berlaku sama untuk Guest dan pengguna terdaftar, serta perilaku antarmuka ketika input melewati batas.
- **Klarifikasi:** Batas karakter merupakan batas input/pemrosesan dan tidak sama dengan kuota kredit. Perbedaan kuota Guest dan pengguna terdaftar tetap mengacu pada jumlah kredit/reward, bukan otomatis pada batas karakter.
- **Dampak:** Validasi panjang input harus diterapkan sebelum pemrosesan dan batasnya perlu konsisten dengan batas token internal, kemampuan model, strategi chunking, serta biaya layanan.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat membahas UF-02 pada 2026-11-12.


## DEC-016: Batas karakter Detector sama untuk Guest dan pengguna terdaftar

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Batas maksimal karakter input AI Text Detector berlaku sama untuk Guest dan pengguna terdaftar.
- **Klarifikasi:** Perbedaan Guest dan pengguna terdaftar terkait jumlah kredit/reward dan akses fitur, bukan batas maksimal karakter input Detector.
- **Dampak:** Validasi panjang input Detector tidak perlu bercabang berdasarkan status autentikasi. Sistem kredit tetap menghitung hak penggunaan dan saldo sesuai jenis akun.
- **Belum diputuskan:** angka maksimal karakter serta perilaku antarmuka ketika batas terlampaui.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat membahas UF-02 pada 2026-11-12.


## DEC-017: Metode input AI Text Detector

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** AI Text Detector menyediakan dua metode input:
  1. Pengguna mengetik atau menempelkan teks langsung ke area input.
  2. Pengguna mengunggah dokumen berformat TXT, DOCX, atau PDF untuk diekstrak sebelum dianalisis.
- **Dampak:** Antarmuka Detector perlu menyediakan area input teks dan opsi unggah file. Kedua jalur harus melewati validasi karakter, ekstraksi/validasi file bila relevan, dan pemeriksaan sebelum pemrosesan.
- **Batasan:** Persetujuan metode input ini tidak menetapkan batas ukuran file, batas karakter numerik, dukungan OCR final, atau jaminan pelestarian seluruh tata letak dokumen.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat membahas UF-02 pada 2026-11-12.


## DEC-018: Batas minimal karakter input AI Text Detector

- **Tanggal:** 2026-11-12
- **Status:** Superseded
- **Keputusan:** AI Text Detector harus menerapkan batas minimal jumlah karakter. Input yang berada di bawah batas minimal tidak diproses dan pengguna diminta menambahkan teks.
- **Belum diputuskan:** angka minimal karakter, cara menghitung karakter (termasuk spasi, baris baru, dan karakter khusus), serta perilaku UX final.
- **Klarifikasi:** Batas minimal ini merupakan validasi kualitas input dan berlaku terpisah dari kuota kredit. Belum ada keputusan untuk membedakan batas minimal berdasarkan status Guest atau pengguna terdaftar.
- **Dampak:** Validasi panjang teks harus memeriksa batas minimum dan maksimum sebelum mengurangi kredit atau mengirim permintaan ke layanan AI. Ambang minimum perlu ditentukan melalui evaluasi model dan pengujian kualitas deteksi, bukan angka arbitrer.
- **Sumber:** Persetujuan eksplisit pemilik proyek saat membahas UF-02 pada 2026-11-12.


## DEC-019: Informasi validasi untuk input di bawah batas minimal Detector

- **Tanggal:** 2026-11-12
- **Status:** Superseded
- **Keputusan:** Jika input AI Text Detector berada di bawah batas minimal karakter, sistem menolak pemrosesan dan menampilkan dua informasi: jumlah karakter minimum yang harus dipenuhi serta jumlah karakter yang saat ini dimasukkan.
- **Alasan:** Pengguna perlu mengetahui secara konkret berapa banyak teks yang perlu ditambahkan, bukan hanya menerima pesan bahwa input terlalu pendek.
- **Alternatif yang dipertimbangkan:** Menampilkan pesan penolakan tanpa menyebutkan jumlah minimum dan jumlah karakter saat ini.
- **Dampak:** Pesan validasi harus mengambil ambang minimum yang dikonfigurasi dan jumlah karakter hasil penghitungan input. Validasi tetap dilakukan sebelum pemrosesan dan pemotongan kredit.
- **Belum diputuskan:** angka batas minimal dan aturan penghitungan karakter, termasuk perlakuan spasi, baris baru, dan karakter khusus.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat menyusun User Flow UF-02 pada 2026-11-12.


## DEC-020: Batas minimum untuk sampel profil gaya Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Batas minimum karakter tidak berlaku untuk input AI Text Detector. Batas minimum ini hanya berlaku untuk contoh/sampel tulisan yang diberikan pengguna untuk membentuk profil gaya personal pada AI Humanizer. Sampel yang belum memenuhi batas tidak digunakan untuk membentuk profil; sistem meminta tambahan dan menampilkan jumlah karakter minimum yang diwajibkan serta jumlah karakter sampel saat ini.
- **Aturan penghitungan:** Seluruh karakter yang dimasukkan dihitung, termasuk spasi dan baris baru.
- **Alasan:** Kecukupan panjang dibutuhkan untuk menilai dan membentuk representasi gaya penulisan personal. Detector tidak memerlukan batas minimum karakter yang ditetapkan sebagai aturan produk pada tahap ini.
- **Dampak:** Persyaratan batas minimum dan pesan validasi dipindahkan dari AI Text Detector ke sampel profil gaya AI Humanizer. Penghitungan karakter sampel harus konsisten antara antarmuka dan backend.
- **Belum diputuskan:** angka minimum sampel, perlakuan karakter khusus/Unicode, dan apakah kecukupan profil juga mempertimbangkan faktor selain jumlah karakter seperti keragaman dan konsistensi tulisan.
- **Sumber:** Klarifikasi eksplisit pemilik proyek saat membahas validasi panjang input dan sampel gaya pada 2026-11-12.


## DEC-021: Mempertahankan sampel gaya saat meminta tambahan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap sampel tulisan untuk profil gaya personal disimpan dan ditampilkan sebagai entri terpisah. Jika kumpulan sampel belum cukup beragam atau belum konsisten, sistem mempertahankan seluruh entri yang sudah diberikan dan meminta pengguna menambahkan sampel baru sebagai entri terpisah. Sistem menilai seluruh entri sebagai satu kumpulan untuk membentuk atau memperbarui profil gaya; pengguna tidak perlu mengirim ulang sampel sebelumnya.
- **Alasan:** Pengguna tidak kehilangan materi yang sudah diberikan dan dapat meningkatkan kualitas sampel secara bertahap.
- **Alternatif yang dipertimbangkan:** Menolak sampel dan meminta seluruh sampel dikirim ulang; membentuk profil sementara meskipun kualitas sampel belum memadai.
- **Dampak:** Alur pembuatan profil perlu mendukung daftar entri sampel, penambahan sampel secara bertahap, pengelolaan tiap entri, dan penilaian kumpulan sampel secara keseluruhan. Data tiap sampel perlu tetap dapat ditelusuri sebagai entri terpisah.
- **Belum diputuskan:** indikator teknis keragaman dan konsistensi, batas jumlah entri sampel, metadata tiap entri, apakah pengguna dapat mengedit atau menghapus satu entri, serta aturan penghapusan sampel sumber setelah profil terbentuk.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat membahas kecukupan sampel profil gaya Humanizer pada 2026-11-12.


## DEC-022: Sampel profil gaya dikelola sebagai entri terpisah

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna memberikan sampel tulisan secara terpisah sebagai entri-entri individual. Sistem menampilkan dan mengelola setiap sampel sebagai entri tersendiri, tetapi mengevaluasi seluruh entri sebagai satu kumpulan ketika menilai kecukupan dan membentuk atau memperbarui profil gaya personal.
- **Alasan:** Entri terpisah memungkinkan sistem dan pengguna menelusuri perbedaan antarcontoh tulisan serta menilai konsistensi gaya tanpa kehilangan asal masing-masing sampel.
- **Alternatif yang dipertimbangkan:** Menggunakan satu kolom input gabungan yang menambahkan seluruh tulisan ke satu teks panjang.
- **Dampak:** Antarmuka dan model data perlu mendukung kumpulan sampel dengan identitas per entri. Proses profiling perlu menganalisis kumpulan, bukan memperlakukan setiap sampel sebagai profil mandiri.
- **Belum diputuskan:** batas jumlah sampel, metadata per sampel, format tampilan, dan aturan retensi sampel sumber.
- **Sumber:** Konfirmasi eksplisit pemilik proyek saat membahas pengumpulan sampel profil gaya Humanizer pada 2026-11-12.


## DEC-023: Pengelolaan sampel profil gaya secara individual

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna dapat mengedit atau menghapus setiap entri sampel profil gaya secara individual. Perubahan pada satu entri tidak mengharuskan pengguna menghapus atau mengirim ulang entri sampel lainnya.
- **Alasan:** Pengguna perlu dapat memperbaiki atau menghapus sampel yang tidak relevan, tidak konsisten, atau bukan tulisannya sendiri tanpa kehilangan kumpulan sampel lainnya.
- **Alternatif yang dipertimbangkan:** Mengunci sampel setelah ditambahkan atau hanya menyediakan penghapusan/penggantian seluruh kumpulan sampel.
- **Dampak:** Antarmuka dan backend perlu menyediakan operasi edit dan hapus per entri serta menjaga identitas dan keterkaitan setiap sampel dengan profil gaya pemiliknya.
- **Belum diputuskan:** apakah perubahan atau penghapusan sampel otomatis memicu evaluasi ulang profil yang sudah terbentuk, bagaimana versi profil dikelola, dan apakah penghapusan sampel sumber mengikuti kebijakan retensi tertentu.
- **Sumber:** Persetujuan eksplisit pemilik proyek atas pengeditan dan penghapusan sampel secara individual pada 2026-11-12.

## DEC-024: Evaluasi ulang profil gaya setelah perubahan sampel

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap kali pengguna mengedit atau menghapus satu entri sampel pada profil gaya yang sudah terbentuk, sistem mengevaluasi ulang seluruh kumpulan sampel yang tersisa. Jika kumpulan sampel tidak lagi memadai, sistem memberi peringatan dan meminta pengguna menambahkan sampel sebelum profil digunakan atau diperbarui kembali.
- **Alasan:** Profil gaya harus tetap merepresentasikan sampel yang benar-benar tersedia. Profil tidak boleh terus dianggap valid tanpa pemeriksaan setelah sumber pembentuknya berubah.
- **Alternatif yang dipertimbangkan:** Mempertahankan profil lama tanpa evaluasi ulang; menghapus profil secara otomatis ketika sampel berubah.
- **Dampak:** Alur edit/hapus sampel perlu memicu evaluasi kumpulan sampel. Sistem perlu dapat menyatakan apakah profil masih memadai dan memberi arahan penambahan sampel ketika tidak memadai.
- **Belum diputuskan:** kriteria teknis kecukupan, apakah profil lama tetap dapat digunakan selama evaluasi atau ketika sampel tidak lagi memadai, serta strategi versi/pemulihan profil sebelumnya. Penanganan evaluasi gagal karena gangguan teknis ditetapkan terpisah pada DEC-026.
- **Sumber:** Persetujuan eksplisit pemilik proyek saat membahas pengelolaan sampel profil gaya Humanizer pada 2026-11-12.

## DEC-025: Profil tidak memadai tetap terlihat tetapi tidak dapat dipilih

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika evaluasi ulang menunjukkan kumpulan sampel profil gaya tidak lagi memadai, profil tetap ditampilkan dalam daftar pengelolaan agar pengguna dapat melihat dan memperbaikinya. Namun, profil tidak dapat dipilih untuk pemrosesan AI Humanizer sampai pengguna menambahkan sampel dan profil kembali memenuhi kriteria kecukupan.
- **Alasan:** Pengguna tetap memiliki visibilitas dan kendali atas profilnya, sementara sistem mencegah penggunaan profil yang sudah diketahui tidak cukup merepresentasikan gaya penulis.
- **Alternatif yang dipertimbangkan:** Membolehkan pemilihan profil dengan peringatan; menyembunyikan profil sepenuhnya sampai pulih.
- **Dampak:** Status profil perlu dibedakan antara terlihat/dapat dikelola dan tersedia/tidak tersedia untuk pemrosesan. Antarmuka pemilihan Humanizer harus mengecualikan profil yang tidak memadai, sedangkan pengelola profil tetap menampilkannya beserta arahan perbaikan.
- **Belum diputuskan:** kriteria teknis kecukupan, label/status UI final, dan strategi versi/pemulihan profil sebelumnya.
- **Sumber:** Persetujuan eksplisit pemilik proyek saat membahas status profil gaya yang tidak lagi memadai pada 2026-11-12.

## DEC-026: Memisahkan kegagalan evaluasi teknis dari ketidakcukupan sampel

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika evaluasi ulang profil gaya gagal akibat gangguan teknis, sistem menandai evaluasi sebagai gagal dan meminta pengguna mencoba kembali. Kegagalan teknis tidak boleh dianggap sebagai bukti bahwa kumpulan sampel tidak memadai dan tidak otomatis mengubah profil menjadi tidak valid.
- **Alasan:** Kegagalan infrastruktur atau proses evaluasi tidak memberikan bukti tentang kualitas maupun kecukupan sampel. Menganggapnya sebagai ketidakcukupan akan menghasilkan status profil yang keliru dan merugikan pengguna.
- **Alternatif yang dipertimbangkan:** Memperlakukan setiap kegagalan evaluasi sebagai profil tidak memadai; membiarkan status profil berubah tanpa membedakan hasil evaluasi dan kegagalan teknis.
- **Dampak:** Sistem perlu membedakan hasil evaluasi kecukupan dari status kegagalan teknis, menyediakan percobaan ulang, dan tidak menerapkan status tidak memadai hanya karena evaluasi gagal.
- **Belum diputuskan:** jumlah/batas percobaan ulang, jeda retry, dan notifikasi kegagalan. Penggunaan profil sebelumnya selama evaluasi ditetapkan terpisah pada DEC-027.
- **Sumber:** Persetujuan eksplisit pemilik proyek saat membahas penanganan kegagalan evaluasi profil gaya Humanizer pada 2026-11-12.

## DEC-027: Mempertahankan penggunaan versi profil sebelumnya selama evaluasi ulang

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Selama evaluasi ulang belum menghasilkan keputusan yang menyatakan kumpulan sampel tidak memadai, versi profil gaya yang sebelumnya berhasil dibentuk tetap dapat digunakan untuk pemrosesan AI Humanizer. Ketentuan ini juga berlaku ketika evaluasi ulang gagal secara teknis. Jika evaluasi berhasil dan menyatakan sampel tidak memadai, profil tidak lagi dapat dipilih untuk pemrosesan sampai sampel tambahan membuatnya memenuhi kriteria kembali.
- **Alasan:** Perubahan sampel tidak serta-merta membuktikan bahwa profil lama tidak layak, sedangkan kegagalan teknis sama sekali bukan bukti ketidakcukupan. Penggunaan versi sebelumnya menjaga kontinuitas layanan tanpa mengabaikan hasil evaluasi yang valid.
- **Alternatif yang dipertimbangkan:** Menonaktifkan profil setiap kali sampel berubah; mengizinkan penggunaan profil tanpa batas meskipun evaluasi valid menyatakan sampel tidak memadai.
- **Dampak:** Sistem perlu mempertahankan versi profil terakhir yang berhasil dibentuk, memisahkan versi tersebut dari kumpulan sampel yang sedang dievaluasi, dan menonaktifkan pemilihan hanya setelah hasil evaluasi yang berhasil menyatakan sampel tidak memadai.
- **Belum diputuskan:** strategi penyimpanan/versi profil, jumlah/batas percobaan ulang, jeda retry, dan notifikasi kegagalan.
- **Sumber:** Persetujuan eksplisit pemilik proyek saat membahas penggunaan profil gaya selama evaluasi ulang pada 2026-11-12.


## DEC-028: Aktivasi versi profil gaya baru setelah pembangunan berhasil

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika evaluasi ulang berhasil dan menyatakan kumpulan sampel memadai, sistem membangun versi profil gaya baru secara terpisah dari versi aktif. Versi profil baru hanya diaktifkan setelah proses pembangunan berhasil. Selama pembangunan berlangsung, versi aktif sebelumnya tetap digunakan. Jika pembangunan versi baru gagal, versi aktif sebelumnya tetap digunakan dan sistem mencatat kegagalan pembaruan.
- **Alasan:** Pengguna tidak boleh kehilangan profil yang masih berfungsi hanya karena proses pembentukan ulang mengalami gangguan. Pemisahan proses pembangunan dan aktivasi mencegah profil setengah jadi atau gagal menggantikan versi yang sedang digunakan.
- **Alternatif yang dipertimbangkan:** Mengganti versi aktif sebelum pembangunan selesai; menonaktifkan profil selama pembangunan versi baru.
- **Dampak:** Sistem perlu memisahkan artefak versi profil yang sedang dibangun dari versi aktif, memvalidasi keberhasilan pembangunan sebelum aktivasi, dan melakukan pergantian versi secara atomik. Kegagalan pembangunan harus dibedakan dari hasil evaluasi sampel yang tidak memadai.
- **Klarifikasi hubungan dengan DEC-027:** Jika evaluasi berhasil menyatakan sampel tidak memadai, profil tetap tidak dapat dipilih sesuai DEC-025 dan DEC-027. Jika evaluasi menyatakan sampel memadai tetapi pembangunan versi baru gagal, versi aktif sebelumnya tetap dapat digunakan.
- **Belum diputuskan:** format artefak versi, mekanisme teknis aktivasi atomik, retensi versi lama, dan kebijakan pembersihan versi yang gagal dibangun.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-029: Retensi dan pemulihan versi profil gaya sebelumnya

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setelah versi profil gaya baru berhasil diaktifkan, versi sebelumnya disimpan selama 30 hari. Pengguna dapat memulihkan versi sebelumnya selama masa retensi tersebut. Pemulihan mengaktifkan kembali artefak profil versi lama tanpa mengembalikan atau mengubah kumpulan sampel sumber saat ini. Versi lama yang telah melewati masa retensi dapat dihapus secara otomatis.
- **Alasan:** Masa retensi memberi pengguna kesempatan untuk menilai hasil pembaruan dan memulihkan profil yang sebelumnya lebih sesuai, sekaligus membatasi penyimpanan versi historis tanpa batas.
- **Alternatif yang dipertimbangkan:** Menghapus versi lama segera setelah aktivasi; menyimpan seluruh versi tanpa batas waktu; menyimpan versi lama tanpa menyediakan pemulihan mandiri.
- **Dampak:** Sistem perlu mencatat waktu aktivasi dan batas akhir retensi setiap versi, menyediakan tindakan pemulihan untuk versi yang masih berada dalam masa retensi, serta menghapus versi historis setelah masa retensi berakhir. Pemulihan versi tidak boleh memundurkan kumpulan sampel sumber.
- **Klarifikasi:** Retensi 30 hari dihitung setelah versi baru berhasil diaktifkan. Versi aktif tidak kedaluwarsa hanya karena batas retensi versi sebelumnya. Kebijakan ini mengatur artefak versi profil, bukan retensi sampel tulisan mentah.
- **Belum diputuskan:** apakah versi yang dipulihkan mendapat masa retensi baru atau apakah versi yang sebelumnya aktif menjadi versi historis dengan masa retensi baru; detail teknis penjadwalan penghapusan dan audit pemulihan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-030: Retensi versi sebelumnya setelah pemulihan profil

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap pergantian versi profil yang berhasil, termasuk pemulihan versi historis, menjadikan versi yang sebelumnya aktif sebagai versi historis dan menyimpannya selama 30 hari sejak pergantian berhasil. Masa retensi dihitung secara individual untuk setiap versi yang dinonaktifkan. Pergantian atau pemulihan satu versi tidak memperpanjang masa retensi versi historis lainnya.
- **Alasan:** Pemulihan versi harus menjadi pergantian versi yang dapat dibalik, bukan tindakan yang menghilangkan versi yang sebelumnya aktif. Pengguna tetap memiliki kesempatan untuk kembali ke versi sebelum pemulihan tanpa menyimpan seluruh versi tanpa batas.
- **Alternatif yang dipertimbangkan:** Menghapus versi yang sebelumnya aktif ketika pemulihan dilakukan; memperpanjang masa retensi semua versi historis setiap kali terjadi pergantian.
- **Dampak:** Sistem perlu mencatat waktu aktivasi dan waktu penonaktifan setiap versi, menghitung masa retensi 30 hari per versi yang dinonaktifkan, serta memastikan proses pemulihan tidak mengubah tenggat retensi versi historis lainnya.
- **Klarifikasi:** Versi yang sedang aktif tidak memiliki tenggat retensi historis. Ketika versi tersebut dinonaktifkan oleh pergantian yang berhasil, masa retensi 30 hari dimulai untuk versi itu. Pemulihan tidak mengubah kumpulan sampel sumber.
- **Belum diputuskan:** batas jumlah versi historis yang dapat dipulihkan, detail audit pergantian/pemulihan, dan kebijakan pembersihan fisik artefak yang kedaluwarsa.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-031: Batas lima versi historis dan penundaan saat kapasitas penuh

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap profil gaya dapat memiliki maksimal 5 versi historis yang belum kedaluwarsa. Versi historis yang masih berada dalam masa retensi 30 hari tidak boleh dihapus otomatis hanya untuk menyediakan slot bagi versi baru. Jika seluruh slot historis terisi, aktivasi versi profil baru atau pemulihan versi historis ditunda sampai tersedia slot. Selama penundaan, versi profil yang sedang aktif tetap digunakan.
- **Alasan:** Batas jumlah versi mengendalikan pertumbuhan penyimpanan, sedangkan perlindungan masa retensi menjaga janji pemulihan kepada pengguna. Menunda pergantian lebih aman daripada menghapus versi yang masih dijanjikan dapat dipulihkan.
- **Alternatif yang dipertimbangkan:** Menghapus versi historis tertua sebelum masa retensi berakhir; membolehkan jumlah versi historis tumbuh tanpa batas; menolak pembaruan/pemulihan secara permanen ketika kapasitas penuh.
- **Dampak:** Sistem perlu menghitung slot historis yang masih terpakai, memeriksa kapasitas sebelum pergantian versi, menunda aktivasi/pemulihan ketika tidak tersedia slot, dan mencoba kembali setelah slot tersedia. Versi aktif tidak boleh berubah selama menunggu.
- **Klarifikasi:** Batas 5 berlaku untuk versi historis yang belum kedaluwarsa per profil. Versi aktif tidak dihitung sebagai versi historis. Versi yang telah melewati masa retensi dapat dibersihkan dan setelah slot benar-benar tersedia, pergantian yang tertunda dapat dilanjutkan.
- **Belum diputuskan:** apakah pengguna dapat membatalkan permintaan pergantian yang tertunda, bagaimana notifikasi dan retry dilakukan, serta prioritas jika pembaruan dan pemulihan tertunda bersamaan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-032: Prioritas dan pembatalan permintaan pergantian versi tertunda

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika permintaan pemulihan versi historis dan pembaruan profil sama-sama tertunda karena kapasitas maksimal lima versi historis belum tersedia, permintaan pemulihan mendapat prioritas lebih tinggi. Permintaan pembaruan profil menunggu sampai permintaan pemulihan berprioritas lebih tinggi ditangani dan kapasitas tersedia. Pengguna dapat membatalkan permintaan pergantian versi selama permintaan tersebut masih berstatus tertunda dan belum mulai dieksekusi.
- **Alasan:** Pemulihan merupakan tindakan eksplisit pengguna untuk mengembalikan profil ke versi yang diinginkan, sehingga perlu didahulukan daripada pembaruan otomatis/lanjutan. Hak pembatalan mencegah sistem menjalankan perubahan yang sudah tidak diinginkan pengguna.
- **Alternatif yang dipertimbangkan:** Mendahulukan pembaruan; menggunakan urutan kedatangan tanpa prioritas; tidak menyediakan pembatalan permintaan tertunda.
- **Dampak:** Sistem perlu membedakan jenis permintaan pergantian versi, status tertunda dan mulai dieksekusi, menerapkan prioritas pemulihan atas pembaruan, serta menyediakan tindakan pembatalan sebelum eksekusi dimulai.
- **Klarifikasi:** Prioritas ini berlaku ketika kedua jenis permintaan tertunda karena batas kapasitas historis. Pembatalan hanya dijamin selama permintaan belum mulai dieksekusi; perilaku pembatalan setelah eksekusi dimulai tidak ditetapkan oleh keputusan ini.
- **Belum diputuskan:** apakah beberapa permintaan pemulihan diurutkan berdasarkan waktu kedatangan atau aturan lain, apakah satu profil boleh memiliki lebih dari satu permintaan tertunda untuk jenis yang sama, dan detail notifikasi/status antrean.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-033: Satu permintaan pemulihan aktif per profil gaya

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap profil gaya hanya boleh memiliki satu permintaan pemulihan versi yang aktif pada satu waktu. Permintaan aktif mencakup permintaan yang masih tertunda maupun yang sedang dieksekusi. Permintaan pemulihan tambahan untuk profil yang sama ditolak sementara dan pengguna diminta menyelesaikan atau membatalkan permintaan sebelumnya. Jika permintaan sebelumnya masih tertunda, pengguna dapat membatalkannya sebelum mengajukan pemulihan ke versi lain. Jika pemulihan gagal, versi aktif tidak berubah dan pengguna dapat mengajukan percobaan baru setelah permintaan gagal diselesaikan.
- **Alasan:** Beberapa permintaan pemulihan yang tumpang tindih dapat saling bertentangan mengenai versi mana yang harus menjadi versi aktif. Membatasi satu permintaan aktif per profil menghindari kompetisi perubahan dan membuat hasil operasi dapat diprediksi.
- **Alternatif yang dipertimbangkan:** Mengizinkan beberapa permintaan pemulihan dalam antrean; mengganti permintaan sebelumnya secara otomatis dengan permintaan terbaru.
- **Dampak:** Sistem perlu menjamin keunikan permintaan pemulihan aktif per profil, menolak permintaan tambahan saat ada operasi tertunda/berjalan, menyediakan pembatalan untuk permintaan tertunda, serta menjaga versi aktif jika pemulihan gagal.
- **Klarifikasi:** Pembatasan ini berlaku per profil, bukan per akun. Permintaan pemulihan yang telah selesai atau gagal tidak lagi dianggap aktif. Aturan ini tidak menetapkan bahwa permintaan pembaruan profil juga dibatasi dengan mekanisme yang sama.
- **Belum diputuskan:** detail status permintaan, kebijakan retry teknis, dan bagaimana permintaan pembaruan berinteraksi dengan pemulihan yang sedang dieksekusi.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-034: Pemulihan profil mendahului pembaruan profil yang tertahan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika permintaan pemulihan versi profil mulai dieksekusi, proses pembaruan atau pembentukan ulang profil ditahan sampai pemulihan selesai. Jika pemulihan berhasil, pembaruan yang tertahan wajib menggunakan kumpulan sampel terbaru dan menjalani evaluasi ulang. Hasil evaluasi lama tidak boleh digunakan untuk membangun atau mengaktifkan versi baru setelah pemulihan.
- **Alasan:** Hasil evaluasi yang dibuat sebelum atau selama perubahan versi dapat menjadi tidak relevan setelah pemulihan. Menggunakan kumpulan sampel terbaru dan evaluasi baru mencegah versi profil yang dibangun dari kondisi usang menimpa hasil pemulihan.
- **Alternatif yang dipertimbangkan:** Membiarkan pembaruan berjalan bersamaan dengan pemulihan; melanjutkan pembaruan menggunakan hasil evaluasi yang sudah tersedia.
- **Dampak:** Sistem perlu menahan proses pembaruan selama pemulihan berlangsung, lalu mengevaluasi ulang kumpulan sampel terbaru jika pemulihan berhasil sebelum membangun atau mengaktifkan versi baru.
- **Belum diputuskan:** Penanganan pembaruan yang tertahan apabila pemulihan gagal, serta aturan antrean dan notifikasi untuk proses yang ditahan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-035: Pembaruan profil dilanjutkan setelah pemulihan berhasil maupun gagal

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika terdapat pembaruan profil yang tertahan saat pemulihan versi sedang dieksekusi, pembaruan dilanjutkan setelah pemulihan selesai, baik pemulihan berhasil maupun gagal. Pembaruan wajib memakai kumpulan sampel terbaru dan menjalani evaluasi ulang. Hasil evaluasi lama tidak boleh digunakan untuk membangun atau mengaktifkan versi baru. Jika pemulihan gagal, versi aktif tidak berubah dan kegagalan pemulihan tidak menghalangi pembaruan profil.
- **Alasan:** Kegagalan pemulihan tidak seharusnya memblokir proses pembaruan profil yang terpisah. Evaluasi ulang memastikan pembaruan didasarkan pada kondisi sampel terbaru, bukan hasil yang mungkin sudah kedaluwarsa.
- **Alternatif yang dipertimbangkan:** Membatalkan pembaruan tertahan ketika pemulihan gagal; melanjutkan pembaruan menggunakan hasil evaluasi yang sudah tersedia.
- **Dampak:** Setelah operasi pemulihan berakhir, sistem perlu melanjutkan pembaruan tertahan dan mengevaluasi ulang seluruh kumpulan sampel terbaru. Status versi aktif tetap mengikuti hasil operasi pemulihan, bukan hasil pembaruan yang belum selesai.
- **Belum diputuskan:** Detail antrean, notifikasi, dan kebijakan pembatalan pembaruan tertahan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-036: Penghapusan sampel tetap diizinkan selama pembaruan profil tertahan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna tetap dapat menghapus sampel gaya ketika pembaruan profil tertahan oleh proses pemulihan versi. Setelah pemulihan selesai dan pembaruan dilanjutkan, sistem wajib menggunakan kumpulan sampel yang tersisa setelah penghapusan dan mengevaluasi ulang seluruh kumpulan tersebut. Sampel yang telah dihapus tidak boleh digunakan untuk membangun versi profil baru.
- **Alasan:** Pengguna harus tetap dapat mengelola sumber data profilnya. Menggunakan kumpulan sampel terkini memastikan penghapusan dihormati dan sampel yang sudah tidak diinginkan tidak masuk ke versi profil berikutnya.
- **Alternatif yang dipertimbangkan:** Membekukan seluruh perubahan sampel sampai pemulihan selesai; membatalkan pembaruan tertahan jika ada sampel dihapus.
- **Dampak:** Sistem perlu memperlakukan kumpulan sampel sebagai sumber data terbaru ketika pembaruan dilanjutkan. Evaluasi dan pembentukan versi baru harus menggunakan snapshot kumpulan sampel setelah perubahan, bukan snapshot lama.
- **Belum diputuskan:** Perilaku jika pengguna menambah atau mengedit sampel selama pembaruan tertahan, serta bagaimana perubahan sampel yang berulang dikelompokkan untuk evaluasi.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-037: Perubahan sampel diperbolehkan selama pembaruan profil tertahan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna tetap dapat menambah dan mengedit sampel gaya ketika pembaruan profil tertahan oleh proses pemulihan. Setelah pemulihan selesai, pembaruan dilanjutkan dengan mengambil kondisi terbaru seluruh sampel, termasuk penambahan, pengeditan, dan penghapusan yang terjadi selama penahanan. Sistem menjalankan evaluasi ulang atas kumpulan terbaru tersebut untuk proses pembaruan yang dilanjutkan, bukan evaluasi terpisah untuk setiap perubahan selama penahanan.
- **Alasan:** Pengguna perlu tetap memiliki kendali atas sampel gaya tanpa membuat sistem menjalankan evaluasi berulang yang tidak perlu. Mengambil kondisi terbaru setelah pemulihan menjaga konsistensi sumber data pembaruan.
- **Alternatif yang dipertimbangkan:** Membekukan perubahan sampel selama pemulihan; menjalankan evaluasi setiap kali sampel berubah.
- **Dampak:** Sistem perlu mengambil snapshot kumpulan sampel terbaru setelah pemulihan selesai dan sebelum evaluasi ulang dimulai. Semua perubahan sampel selama penahanan harus tercermin dalam snapshot tersebut.
- **Belum diputuskan:** Perilaku jika sampel kembali berubah ketika evaluasi ulang atau pembentukan versi baru sedang berlangsung; mekanisme snapshot/versioning kumpulan sampel dan penanganan konkurensinya perlu dirancang.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-038: Evaluasi profil menggunakan snapshot sampel yang konsisten

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Evaluasi sampel gaya berjalan terhadap snapshot kumpulan sampel yang tidak berubah selama satu proses evaluasi. Jika pengguna menambah, mengedit, atau menghapus sampel ketika evaluasi berlangsung, hasil evaluasi dari snapshot lama tidak boleh digunakan untuk membangun atau mengaktifkan versi profil. Sistem harus mengambil snapshot terbaru dan mengevaluasinya kembali sebelum melanjutkan pembentukan versi profil.
- **Alasan:** Hasil evaluasi harus merepresentasikan satu keadaan kumpulan sampel yang konsisten. Perubahan sampel di tengah evaluasi membuat hasil snapshot lama tidak lagi mewakili sumber data terkini.
- **Alternatif yang dipertimbangkan:** Mengizinkan hasil evaluasi lama tetap dipakai; mengunci seluruh perubahan sampel selama evaluasi.
- **Dampak:** Sistem memerlukan identitas versi atau snapshot kumpulan sampel, pemeriksaan apakah snapshot masih terbaru sebelum hasil evaluasi dipakai, dan mekanisme untuk mengabaikan hasil evaluasi yang sudah usang.
- **Belum diputuskan:** Detail implementasi snapshot, pembatalan atau penghentian evaluasi usang, retry, serta penanganan perubahan sampel yang terjadi berulang kali selama evaluasi ulang.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-039: Batasi pengulangan evaluasi otomatis akibat perubahan sampel berulang

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sistem membatasi pengulangan evaluasi otomatis ketika perubahan sampel berulang membuat snapshot evaluasi menjadi usang. Jika batas pengulangan otomatis tercapai, sistem menghentikan percobaan otomatis, mempertahankan seluruh perubahan sampel pengguna, dan menandai pembaruan profil sebagai tertunda karena perubahan berulang. Pengguna harus memulai ulang evaluasi secara manual. Nilai batas ditetapkan pada tahap desain teknis berdasarkan estimasi biaya dan durasi evaluasi, bukan ditebak pada tahap kebutuhan.
- **Alasan:** Pengulangan tanpa batas dapat memboroskan biaya dan sumber daya, terutama jika pengguna terus mengubah sampel. Namun, menghentikan evaluasi tidak boleh menghilangkan perubahan yang telah dibuat pengguna.
- **Alternatif yang dipertimbangkan:** Mengulang evaluasi tanpa batas; membekukan perubahan sampel; menetapkan angka batas tanpa data biaya dan durasi.
- **Dampak:** Sistem perlu menghitung pengulangan evaluasi otomatis, menghentikan retry saat batas tercapai, mempertahankan sampel terbaru, dan menyediakan tindakan manual untuk memulai ulang evaluasi.
- **Belum diputuskan:** Nilai batas pengulangan, detail status tertunda, notifikasi kepada pengguna, dan apakah restart manual memulai ulang seluruh siklus evaluasi atau dapat melanjutkan dari tahap tertentu.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-040: Restart manual mengulang evaluasi profil dari awal

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika pengguna memulai ulang evaluasi profil secara manual setelah batas pengulangan otomatis tercapai, sistem memulai seluruh proses evaluasi dari awal menggunakan snapshot terbaru kumpulan sampel. Hasil antara dari evaluasi sebelumnya tidak boleh digunakan kembali sebagai dasar untuk melanjutkan proses baru.
- **Alasan:** Memulai dari awal menjaga konsistensi evaluasi terhadap satu snapshot terbaru dan menghindari ketergantungan pada hasil antara yang mungkin sudah tidak relevan. Keputusan ini mengutamakan integritas data meskipun berpotensi menambah biaya dan durasi pemrosesan.
- **Alternatif yang dipertimbangkan:** Melanjutkan dari tahap terakhir yang berhasil dengan menggunakan kembali hasil antara.
- **Dampak:** Restart manual harus membuat proses evaluasi baru yang lengkap dan terikat pada snapshot terbaru. Hasil antara dari percobaan sebelumnya tidak boleh dipakai sebagai hasil validasi untuk proses baru.
- **Belum diputuskan:** Detail teknis pembersihan atau retensi artefak evaluasi lama, serta apakah pengguna perlu melihat riwayat percobaan evaluasi.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-041: Simpan metadata audit, bukan hasil antara evaluasi yang tidak berlaku

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Untuk evaluasi yang dihentikan atau menjadi usang, sistem hanya menyimpan metadata riwayat evaluasi yang diperlukan untuk audit, seperti waktu, status, alasan penghentian, dan identitas versi snapshot. Hasil analisis antara yang tidak berlaku tidak disimpan sebagai hasil yang dapat digunakan kembali dan tidak boleh dipakai dalam evaluasi atau pembentukan profil berikutnya.
- **Alasan:** Metadata audit memberi jejak proses tanpa mempertahankan hasil antara yang sudah tidak relevan dan berisiko digunakan kembali secara keliru.
- **Alternatif yang dipertimbangkan:** Menyimpan seluruh hasil evaluasi antara; menghapus seluruh jejak evaluasi tanpa menyisakan metadata.
- **Dampak:** Sistem perlu memisahkan metadata riwayat evaluasi dari artefak hasil analisis. Artefak hasil antara yang tidak berlaku tidak boleh menjadi input proses berikutnya.
- **Belum diputuskan:** Masa retensi metadata, detail metadata wajib, akses pengguna terhadap riwayat percobaan, serta kebijakan penghapusan dan audit.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-042: Retensi dan akses metadata evaluasi yang dihentikan atau usang

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Metadata evaluasi yang dihentikan atau menjadi usang hanya disimpan untuk kebutuhan audit internal dan tidak ditampilkan sebagai riwayat kepada pengguna. Metadata dihapus otomatis setelah 30 hari, kecuali masih diperlukan untuk investigasi insiden atau kewajiban keamanan. Ketentuan ini tidak mengizinkan penyimpanan hasil analisis antara yang sudah tidak berlaku.
- **Alasan:** Riwayat teknis yang tidak membantu tindakan pengguna tidak perlu memenuhi halaman pengelolaan profil. Retensi terbatas mendukung audit secukupnya sekaligus membatasi penyimpanan data.
- **Alternatif yang dipertimbangkan:** Menampilkan riwayat evaluasi usang kepada pengguna; menyimpan metadata tanpa batas waktu; menghapus metadata segera setelah evaluasi berhenti.
- **Dampak:** Sistem perlu membatasi akses metadata untuk kebutuhan internal, menerapkan penghapusan otomatis 30 hari, dan menyediakan pengecualian retensi untuk investigasi insiden atau kewajiban keamanan yang sah.
- **Belum diputuskan:** Otorisasi internal yang dapat mengakses metadata, mekanisme legal/security hold, proses pelepasan hold, serta pencatatan akses audit.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-043: RBAC dan audit akses metadata evaluasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Akses metadata audit evaluasi dibatasi berdasarkan peran. Super Admin dapat mengakses metadata sesuai kebutuhan audit. Admin hanya dapat mengaksesnya secara terbatas untuk kebutuhan operasional atau investigasi yang ditugaskan. Moderator tidak memiliki akses. Setiap akses wajib mencatat identitas petugas, alasan, waktu, dan tindakan. Izin mengakses metadata audit harus terpisah dari izin mengakses teks asli pengguna.
- **Alasan:** Metadata audit tetap merupakan informasi internal yang perlu dilindungi. Pemisahan izin mencegah akses metadata otomatis memberikan hak untuk membaca konten pengguna.
- **Alternatif yang dipertimbangkan:** Memberikan akses kepada semua peran admin; menggunakan satu izin gabungan untuk metadata dan teks pengguna.
- **Dampak:** Sistem memerlukan kontrol akses berbasis peran dan izin terpisah, pencatatan setiap akses, serta pembatasan akses Admin berdasarkan penugasan operasional atau investigasi.
- **Belum diputuskan:** Matriks izin rinci, mekanisme penugasan dan persetujuan investigasi, cakupan metadata yang dapat dilihat tiap peran, serta retensi log akses.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-044: Persetujuan sementara untuk akses investigasi metadata oleh Admin

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Admin yang belum memiliki penugasan investigasi wajib mengajukan permintaan akses metadata audit dengan alasan dan nomor tiket investigasi. Super Admin menyetujui atau menolak permintaan tersebut. Persetujuan bersifat sementara dan hanya berlaku untuk cakupan investigasi yang disetujui.
- **Alasan:** Kebutuhan operasional yang diklaim sendiri tidak cukup untuk memperluas akses. Persetujuan eksplisit, alasan, tiket, dan cakupan terbatas menyediakan kontrol serta jejak akuntabilitas.
- **Alternatif yang dipertimbangkan:** Memberikan akses berdasarkan permintaan Admin tanpa persetujuan; memberikan akses permanen kepada Admin.
- **Dampak:** Sistem memerlukan alur permintaan dan persetujuan, pengaitan ke tiket investigasi, pembatasan cakupan akses, dan mekanisme izin sementara.
- **Belum diputuskan:** Masa berlaku persetujuan, mekanisme pencabutan lebih awal, definisi cakupan metadata, dan apakah akses dapat diperpanjang.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-045: Masa berlaku persetujuan akses investigasi maksimal 24 jam

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Persetujuan akses metadata audit untuk Admin berlaku maksimal 24 jam, tidak diperpanjang otomatis, dan dapat dicabut oleh Super Admin kapan saja. Setelah kedaluwarsa atau dicabut, Admin harus mengajukan permintaan baru jika akses masih diperlukan. Akses tetap terbatas pada cakupan investigasi yang disetujui.
- **Alasan:** Masa berlaku singkat mengurangi risiko izin sensitif terlupakan dan tetap terbuka setelah kebutuhan investigasi berakhir.
- **Alternatif yang dipertimbangkan:** Persetujuan tanpa batas waktu; perpanjangan otomatis; masa berlaku lebih panjang.
- **Dampak:** Sistem harus menerapkan kedaluwarsa otomatis paling lambat 24 jam sejak persetujuan, tidak memperpanjang izin secara otomatis, menyediakan pencabutan oleh Super Admin, dan mengharuskan permintaan baru setelah akses berakhir.
- **Belum diputuskan:** Apakah masa berlaku dihitung sejak waktu persetujuan atau sejak akses pertama digunakan, serta detail notifikasi sebelum kedaluwarsa.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-046: Awal masa berlaku akses investigasi Admin

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Masa berlaku maksimal 24 jam untuk persetujuan akses metadata audit dihitung sejak waktu Super Admin menyetujui permintaan, bukan sejak Admin pertama kali menggunakan akses.
- **Alasan:** Perhitungan sejak persetujuan mencegah izin disimpan tanpa digunakan lalu dimanfaatkan pada waktu yang tidak terduga.
- **Alternatif yang dipertimbangkan:** Memulai hitungan sejak akses pertama digunakan.
- **Dampak:** Sistem harus mencatat waktu persetujuan dan menetapkan waktu kedaluwarsa berdasarkan waktu tersebut. Waktu penggunaan pertama tidak mengubah atau mengatur ulang masa berlaku.
- **Belum diputuskan:** Detail zona waktu untuk tampilan, presisi timestamp, dan penanganan perbedaan jam antar layanan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-047: Notifikasi siklus hidup persetujuan akses investigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Saat akses investigasi metadata audit disetujui, Admin menerima notifikasi status aktif, cakupan izin, dan waktu kedaluwarsa. Admin menerima pengingat satu jam sebelum kedaluwarsa. Saat akses kedaluwarsa, sistem mencabut akses otomatis dan memberi tahu Admin. Jika akses dicabut lebih awal, Admin segera diberi tahu beserta alasan pencabutan. Super Admin menerima catatan bahwa akses telah kedaluwarsa atau dicabut. Notifikasi tidak memperpanjang masa akses.
- **Alasan:** Pihak yang diberi akses harus mengetahui batas dan cakupan izinnya, sementara pencabutan dan kedaluwarsa perlu transparan serta dapat diaudit. Notifikasi tidak boleh mengubah batas keamanan yang sudah ditetapkan.
- **Alternatif yang dipertimbangkan:** Hanya memberi notifikasi saat persetujuan; mengandalkan Admin memeriksa waktu kedaluwarsa sendiri; memperpanjang akses setelah pengingat.
- **Dampak:** Sistem memerlukan notifikasi persetujuan, pengingat satu jam sebelum kedaluwarsa, notifikasi kedaluwarsa dan pencabutan, serta catatan untuk Super Admin. Pengiriman notifikasi tidak mengubah status atau masa berlaku izin.
- **Belum diputuskan:** Kanal notifikasi, perilaku jika pengingat satu jam tidak dapat dikirim, serta detail isi catatan Super Admin.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-048: Kanal notifikasi dan ketahanan pencabutan akses investigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Notifikasi siklus hidup persetujuan akses investigasi dikirim melalui notifikasi dalam aplikasi dan email. Jika pengiriman melalui salah satu kanal gagal, sistem tetap mencoba kanal lainnya. Kegagalan pengiriman notifikasi tidak boleh menghambat atau menunda pencabutan akses otomatis. Notifikasi tidak memperpanjang masa akses.
- **Alasan:** Dua kanal meningkatkan kemungkinan Admin menerima informasi penting. Namun, pengiriman pesan merupakan fungsi pemberitahuan dan tidak boleh menjadi prasyarat bagi kontrol keamanan pencabutan akses.
- **Alternatif yang dipertimbangkan:** Mengandalkan satu kanal; menunda pencabutan sampai notifikasi berhasil dikirim.
- **Dampak:** Sistem memerlukan dua kanal notifikasi, penanganan kegagalan kanal secara independen, dan pemisahan proses notifikasi dari proses kedaluwarsa/pencabutan izin.
- **Belum diputuskan:** Kebijakan retry tiap kanal, batas percobaan, penanganan email tidak valid, dan status delivery yang ditampilkan kepada Super Admin.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-049: Penanganan kegagalan seluruh kanal notifikasi akses investigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika notifikasi dalam aplikasi dan email sama-sama gagal, status kedaluwarsa atau pencabutan akses tetap berlaku. Kegagalan kedua kanal dicatat dalam audit log dan ditampilkan kepada Super Admin sebagai indikator kegagalan notifikasi. Sistem dapat melakukan retry terbatas, tetapi kegagalan pengiriman maupun retry tidak boleh mengaktifkan kembali izin atau menunda pencabutan.
- **Alasan:** Pencabutan akses adalah kontrol keamanan yang harus independen dari keberhasilan pengiriman pesan. Kegagalan notifikasi perlu terlihat dan dapat diaudit tanpa mengubah status izin.
- **Alternatif yang dipertimbangkan:** Menunda pencabutan sampai notifikasi berhasil; mengaktifkan kembali akses sementara; mengabaikan kegagalan pengiriman.
- **Dampak:** Sistem perlu mencatat kegagalan dua kanal, menampilkan indikator kepada Super Admin, dan memisahkan retry notifikasi dari proses kedaluwarsa/pencabutan izin.
- **Belum diputuskan:** Jumlah dan interval retry, kriteria kegagalan permanen, serta format indikator dan audit event.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-050: Batas retry dan pengiriman ulang notifikasi akses investigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sistem melakukan maksimal tiga percobaan pengiriman ulang otomatis dengan jeda lima menit antarpercobaan. Jika seluruh percobaan gagal, status pengiriman menjadi `Delivery Failed`. Super Admin dapat melihat detail kegagalan dan memicu pengiriman ulang secara manual. Pengiriman ulang manual hanya mengirim notifikasi dan tidak mengubah status akses.
- **Alasan:** Retry terbatas mencegah pengiriman tanpa akhir, sedangkan pengiriman ulang manual memberi jalur pemulihan yang terkendali. Pemisahan status notifikasi dan status akses menjaga agar tindakan pengiriman pesan tidak mengubah kontrol keamanan.
- **Alternatif yang dipertimbangkan:** Retry tanpa batas; tidak menyediakan pengiriman ulang manual; menggabungkan status notifikasi dengan status akses.
- **Dampak:** Sistem perlu menghitung maksimal tiga retry otomatis dengan interval lima menit, menetapkan status `Delivery Failed` setelah kegagalan final, menyediakan detail kegagalan bagi Super Admin, dan memastikan pengiriman ulang manual tidak mengubah status izin.
- **Belum diputuskan:** Apakah tiga percobaan dihitung sebagai percobaan ulang setelah pengiriman awal atau total percobaan termasuk pengiriman awal, serta apakah retry berjalan per kanal atau per notifikasi gabungan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-051: Jumlah percobaan dan retry independen per kanal notifikasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setiap kanal notifikasi (in-app dan email) menjalankan satu percobaan pengiriman awal dan maksimal tiga percobaan ulang otomatis jika kanal tersebut gagal, sehingga maksimal empat percobaan total per kanal. Jeda antarpercobaan ulang adalah lima menit pada kanal yang sama. Retry berjalan secara independen per kanal: kanal yang berhasil tidak dikirim ulang, sedangkan hanya kanal yang gagal yang melanjutkan retry. Jika seluruh percobaan pada suatu kanal gagal, status pengiriman kanal tersebut menjadi `Delivery Failed`. Pengiriman ulang manual oleh Super Admin tetap hanya mengirim notifikasi dan tidak mengubah status akses.
- **Alasan:** Menghitung retry setelah percobaan awal menghilangkan ambiguitas jumlah percobaan. Pemisahan retry per kanal mencegah pengiriman duplikat melalui kanal yang sudah berhasil dan membuat kegagalan dapat ditangani secara terpisah.
- **Alternatif yang dipertimbangkan:** Tiga percobaan total termasuk pengiriman awal; retry gabungan yang mengirim ulang kedua kanal meskipun salah satunya sudah berhasil.
- **Dampak:** Sistem perlu melacak status, jumlah percobaan, dan jadwal retry secara terpisah untuk setiap kanal. Status keberhasilan satu kanal tidak boleh memicu pengiriman ulang pada kanal tersebut hanya karena kanal lain gagal. Proses retry tetap terpisah dari kedaluwarsa atau pencabutan akses.
- **Belum diputuskan:** Detail teknis backoff selain interval tetap lima menit, klasifikasi error sementara/permanen, dan perilaku manual resend jika hanya satu kanal yang gagal.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-052: Penggabungan dokumen unggahan dan teks manual dalam satu input Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Dalam satu proses Humanizer, pengguna dapat mengunggah dokumen TXT, DOCX, atau PDF, kemudian meninjau dan mengedit teks hasil ekstraksi serta menambahkan teks secara manual melalui editor sebelum pemrosesan. Seluruh konten yang telah disusun di editor diperlakukan sebagai satu input Humanizer. Dokumen sumber tidak boleh diubah atau ditimpa.
- **Alasan:** Pengguna dapat memperbaiki hasil ekstraksi, menambahkan bagian yang tertinggal, atau menggabungkan beberapa bagian tulisan tanpa harus memisahkan pekerjaan menjadi beberapa proses.
- **Alternatif yang dipertimbangkan:** Membatasi satu proses pada satu sumber input, sehingga pengguna harus memilih antara teks manual atau dokumen unggahan.
- **Dampak:** Humanizer memerlukan editor yang dapat memuat teks hasil ekstraksi, menerima perubahan dan tambahan manual, serta mengirimkan keseluruhan konten sebagai satu input. Sistem harus menjaga dokumen sumber tetap utuh.
- **Belum diputuskan:** Perilaku editor secara rinci, penanganan struktur paragraf/daftar/tabel setelah pengeditan, penanda asal teks, dan format ekspor untuk konten gabungan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-053: Batas satu dokumen unggahan per proses Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Dalam satu proses Humanizer, pengguna hanya dapat mengunggah maksimal satu dokumen TXT, DOCX, atau PDF. Setelah ekstraksi, pengguna tetap dapat meninjau dan mengedit teks di editor serta menambahkan teks secara manual. Beberapa dokumen unggahan tidak dapat digabungkan dalam satu proses. Keputusan ini membatasi jumlah dokumen unggahan, bukan jumlah bagian teks yang dapat diedit atau ditambahkan secara manual di editor.
- **Alasan:** Membatasi satu dokumen per proses menjaga alur input lebih sederhana dan mengurangi kompleksitas penggabungan, urutan konten, serta penanganan struktur dari beberapa dokumen.
- **Alternatif yang dipertimbangkan:** Mengizinkan beberapa dokumen diunggah dan digabungkan ke editor dalam satu proses.
- **Dampak:** UI unggah Humanizer harus membatasi satu dokumen per proses. Pengguna tetap dapat menambahkan teks manual ke editor setelah ekstraksi. Jika ingin memproses dokumen lain, pengguna harus memulai proses terpisah.
- **Belum diputuskan:** Apakah pengguna dapat mengganti dokumen yang sudah diunggah sebelum pemrosesan, serta bagaimana teks manual dipertahankan atau dibuang ketika dokumen diganti.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-054: Konfirmasi sebelum mengganti dokumen Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna ingin mengganti dokumen yang sudah diunggah sebelum pemrosesan Humanizer, sistem wajib meminta konfirmasi terlebih dahulu. Jika pengguna membatalkan, teks hasil ekstraksi dan seluruh tambahan atau editan manual yang ada di editor tetap dipertahankan. Jika pengguna mengonfirmasi, seluruh konten editor dari dokumen sebelumnya, termasuk tambahan dan editan manual, dihapus dan diganti dengan hasil ekstraksi dokumen baru. Dokumen sumber lama maupun baru tidak boleh diubah atau ditimpa.
- **Alasan:** Konfirmasi mencegah kehilangan konten yang sudah diedit atau ditambahkan pengguna secara tidak sengaja, sekaligus memberi perilaku penggantian yang tegas.
- **Alternatif yang dipertimbangkan:** Mengganti dokumen langsung tanpa konfirmasi.
- **Dampak:** UI perlu menyediakan langkah konfirmasi sebelum mengganti dokumen. Sistem harus mempertahankan state editor jika pengguna membatalkan dan mengganti seluruh konten editor jika pengguna mengonfirmasi.
- **Belum diputuskan:** Detail tampilan dialog konfirmasi, apakah nama dokumen lama dan baru ditampilkan, serta penanganan jika ekstraksi dokumen pengganti gagal setelah konfirmasi.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-055: Pengeditan bebas pada editor Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sebelum menjalankan pemrosesan Humanizer, pengguna dapat mengedit teks di editor secara bebas, termasuk mengubah, menambah, dan menghapus bagian mana pun dari teks hasil ekstraksi dokumen maupun teks yang ditambahkan secara manual. Seluruh isi editor pada saat pemrosesan dimulai menjadi satu input yang diproses oleh Humanizer.
- **Alasan:** Pengguna perlu memiliki kendali penuh atas materi yang akan diproses, tanpa pembatasan yang membedakan teks hasil ekstraksi dan teks manual.
- **Alternatif yang dipertimbangkan:** Membatasi perubahan teks hasil ekstraksi dan hanya mengizinkan penambahan teks manual atau perubahan melalui tindakan khusus.
- **Dampak:** Editor harus mendukung penyuntingan teks secara penuh. Sistem memproses isi editor terkini, bukan mengambil ulang isi dokumen sumber. Dokumen sumber tetap tidak diubah.
- **Belum diputuskan:** Batas karakter, validasi konten, dan perilaku editor ketika format dokumen memiliki struktur seperti tabel atau daftar.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-056: Format keluaran Humanizer mengikuti dokumen unggahan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setelah pemrosesan Humanizer selesai, pengguna dapat melihat hasil di website, menyalin teks, dan mengunduh hasil. Jika proses menggunakan dokumen unggahan, format file hasil unduhan mengikuti format dokumen yang diunggah, yaitu TXT, DOCX, atau PDF. Dokumen sumber tidak boleh diubah atau ditimpa. Sistem tidak menjanjikan tata letak atau struktur hasil ekspor identik dengan dokumen sumber.
- **Alasan:** Pengguna dapat melanjutkan pekerjaan dalam format yang sama dengan dokumen yang dimasukkan tanpa mengorbankan dokumen sumber.
- **Alternatif yang dipertimbangkan:** Hasil hanya tersedia sebagai teks untuk disalin, atau pengguna harus memilih format unduhan secara terpisah dari format dokumen sumber.
- **Dampak:** Fitur ekspor Humanizer perlu menghasilkan TXT, DOCX, atau PDF sesuai format dokumen unggahan. Hasil tetap ditampilkan di website dan dapat disalin. Struktur kompleks dan kesamaan tata letak tidak boleh dijanjikan identik.
- **Belum diputuskan:** Format unduhan default ketika pengguna hanya mengetik atau menempelkan teks tanpa mengunggah dokumen.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-057: Pilihan format unduhan untuk input teks manual Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna memulai proses Humanizer hanya dengan mengetik atau menempelkan teks tanpa mengunggah dokumen, pengguna dapat memilih format unduhan hasil berupa TXT, DOCX, atau PDF. Hasil tetap ditampilkan di website dan dapat disalin.
- **Alasan:** Input teks manual tidak memiliki format dokumen sumber yang dapat dijadikan acuan. Memberikan pilihan format unduhan membuat keluaran lebih fleksibel tanpa memaksakan satu format default.
- **Alternatif yang dipertimbangkan:** Hanya menyediakan TXT atau menetapkan DOCX sebagai format default dengan pilihan TXT dan PDF.
- **Dampak:** Antarmuka hasil Humanizer untuk input manual perlu menyediakan pemilih format unduhan TXT, DOCX, dan PDF. Generator keluaran dan validasi teknis ditentukan pada tahap desain teknis.
- **Belum diputuskan:** Apakah pilihan format diingat sebagai preferensi pengguna untuk proses berikutnya.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-058: Penyimpanan hasil Humanizer berdasarkan pilihan pengguna

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Hasil pemrosesan Humanizer tidak disimpan otomatis ke riwayat akun. Pengguna memilih secara eksplisit apakah hasil tertentu ingin disimpan.
- **Alasan:** Teks yang diproses dapat berisi materi akademik atau informasi pribadi. Penyimpanan berdasarkan pilihan pengguna mengurangi penyimpanan konten yang tidak diperlukan dan memberi kendali kepada pengguna.
- **Alternatif yang dipertimbangkan:** Menyimpan semua hasil secara otomatis atau menyimpan otomatis dengan opsi menonaktifkan fitur melalui pengaturan.
- **Dampak:** Alur hasil Humanizer perlu menyediakan tindakan eksplisit untuk menyimpan hasil. Sistem tidak boleh memasukkan konten hasil ke riwayat akun tanpa tindakan tersebut.
- **Belum diputuskan:** Detail tindakan simpan, apakah pilihan menyimpan berlaku per hasil atau dapat dijadikan preferensi, serta retensi dan penghapusan hasil tersimpan. Aturan riwayat umum akun tetap perlu diselaraskan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-059: Isi riwayat Humanizer menyimpan teks asli dan hasil revisi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Hasil Humanizer tidak disimpan otomatis. Jika pengguna secara eksplisit memilih menyimpan hasil tertentu, sistem menyimpan teks asli yang diproses dan hasil revisinya secara lengkap sebagai satu entri riwayat. Jika pengguna tidak memilih menyimpan, kedua teks tersebut tidak dimasukkan ke riwayat akun.
- **Alasan:** Menyimpan kedua versi memungkinkan pengguna membandingkan dan menelusuri perubahan yang dibuat Humanizer pada pekerjaan yang dipilih untuk disimpan.
- **Alternatif yang dipertimbangkan:** Menyimpan hanya hasil revisi atau hanya metadata proses.
- **Dampak:** Skema riwayat Humanizer perlu menyimpan dua konten teks yang saling terkait dalam satu entri. Persetujuan eksplisit tetap menjadi prasyarat penyimpanan.
- **Belum diputuskan:** Retensi, penghapusan, keamanan penyimpanan, batas ukuran riwayat, serta apakah metadata proses tambahan ikut disimpan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-060: Penghapusan permanen entri riwayat Humanizer setelah konfirmasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menghapus entri riwayat Humanizer yang telah disimpan. Setelah pengguna memilih tindakan hapus dan mengonfirmasi, entri dihapus permanen dari riwayat yang dapat diakses melalui aplikasi. Tidak ada tempat sampah atau masa pemulihan 30 hari untuk entri tersebut.
- **Alasan:** Pengguna harus memiliki kendali langsung untuk menghapus teks asli dan hasil revisi yang sebelumnya dipilih untuk disimpan.
- **Alternatif yang dipertimbangkan:** Memindahkan entri ke tempat sampah selama 30 hari atau memasukkannya ke antrean penghapusan dengan masa tunggu.
- **Dampak:** UI harus meminta konfirmasi sebelum penghapusan. Setelah dikonfirmasi, entri tidak lagi tersedia dalam riwayat pengguna. Penghapusan dari riwayat aplikasi harus dibedakan dari penghapusan salinan pada backup, log, atau penyimpanan turunan.
- **Belum diputuskan:** Cakupan dan jadwal penghapusan pada backup, log, cache, serta penyimpanan turunan; mekanisme teknis penghapusan menyeluruh.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-061: Batas riwayat Humanizer berdasarkan kapasitas penyimpanan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Riwayat Humanizer tidak dibatasi berdasarkan jumlah entri. Batas diberlakukan berdasarkan total ukuran data yang disimpan per akun.
- **Alasan:** Ukuran setiap entri dapat berbeda jauh. Batas jumlah entri saja tidak cukup untuk mengendalikan konsumsi penyimpanan.
- **Alternatif yang dipertimbangkan:** Tidak membatasi jumlah entri, batas tetap 50 entri, atau batas tetap 100 entri per akun.
- **Dampak:** Sistem perlu menghitung total ukuran data riwayat Humanizer per akun dan menerapkan kuota penyimpanan. Nilai kuota dan perilaku saat kuota tercapai belum ditentukan dan harus dirancang sebelum implementasi.
- **Klarifikasi melalui DEC-069:** Saat kuota penuh, hasil pemrosesan tetap dapat dilihat, disalin, dan diunduh, tetapi tidak dapat disimpan ke riwayat sampai kapasitas dibebaskan.
- **Belum diputuskan:** Besaran kuota, satuan dan metode penghitungan ukuran, apakah metadata ikut dihitung, serta detail alur penyimpanan ulang setelah kapasitas dibebaskan.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-062: Pengguna memilih entri yang dihapus saat kapasitas riwayat penuh

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika kapasitas penyimpanan riwayat Humanizer penuh, sistem tidak menghapus entri lama secara otomatis. Pengguna dapat memilih sendiri entri riwayat lama yang ingin dihapus untuk menyediakan kapasitas sebelum menyimpan hasil baru.
- **Alasan:** Penghapusan otomatis dapat menghilangkan hasil yang masih dibutuhkan pengguna. Pemilihan manual mempertahankan kendali pengguna atas konten yang disimpan.
- **Alternatif yang dipertimbangkan:** Menolak penyimpanan hasil baru sampai pengguna menghapus entri lama tanpa alur pemilihan yang diarahkan, atau menghapus otomatis entri paling lama.
- **Dampak:** Saat kapasitas tidak mencukupi, sistem perlu menampilkan informasi kapasitas dan menyediakan alur pemilihan entri lama untuk dihapus. Penghapusan tetap mengikuti keputusan DEC-060, yaitu setelah konfirmasi dan permanen dari riwayat aplikasi.
- **Belum diputuskan:** Apakah pengguna dapat memilih beberapa entri sekaligus, cara menampilkan ukuran setiap entri, dan perilaku jika kapasitas masih tidak cukup setelah penghapusan yang dipilih.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-063: Peringatan saat kapasitas riwayat Humanizer mencapai 90%

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sistem memberi peringatan kepada pengguna ketika penggunaan kapasitas penyimpanan riwayat Humanizer mencapai 90% dari kuota akun.
- **Alasan:** Peringatan sebelum kapasitas penuh memberi kesempatan kepada pengguna untuk meninjau dan mengelola entri riwayat tanpa menunggu sampai penyimpanan hasil baru terhenti.
- **Alternatif yang dipertimbangkan:** Memberi peringatan pada 80% atau 95%, atau hanya memberi tahu ketika kapasitas benar-benar penuh.
- **Dampak:** Sistem perlu memantau penggunaan kapasitas per akun dan memicu peringatan pada ambang 90%.
- **Klarifikasi melalui DEC-065:** Peringatan dipicu kembali hanya setelah penggunaan sempat turun di bawah 90% dan kemudian mencapai atau melewati ambang 90% lagi.
- **Klarifikasi melalui DEC-066:** Peringatan kapasitas dikirim melalui notifikasi dalam aplikasi saja, bukan email.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-064: Indikator kapasitas pada halaman riwayat Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Sistem menampilkan indikator penggunaan kapasitas penyimpanan pada halaman riwayat Humanizer. Indikator tidak ditampilkan pada dashboard utama dan memperlihatkan pemakaian terhadap kuota akun.
- **Alasan:** Informasi kapasitas paling relevan saat pengguna mengelola entri riwayat, sehingga tidak perlu menambah kepadatan dashboard utama.
- **Alternatif yang dipertimbangkan:** Hanya menampilkan peringatan saat ambang tercapai atau menempatkan indikator pada dashboard utama.
- **Dampak:** Halaman riwayat Humanizer perlu menampilkan pemakaian dan kuota. Peringatan ambang 90% dari DEC-063 tetap berlaku.
- **Belum diputuskan:** Format visual, satuan ukuran, dan detail tampilan indikator ditentukan pada desain UI dan teknis.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-065: Peringatan kapasitas berulang berdasarkan siklus ambang 90%

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Peringatan kapasitas riwayat Humanizer tidak dikirim berulang selama penggunaan tetap berada pada atau di atas 90%. Setelah penggunaan turun di bawah 90%, sistem mengaktifkan kembali pemicu peringatan. Jika penggunaan kemudian mencapai atau melewati 90% lagi, sistem mengirim peringatan baru.
- **Alasan:** Pengguna tidak menerima peringatan berulang tanpa perubahan kondisi, tetapi tetap mendapat peringatan pada siklus kapasitas penuh berikutnya.
- **Alternatif yang dipertimbangkan:** Peringatan hanya sekali per akun atau peringatan setiap kali halaman riwayat dibuka selama kapasitas minimal 90%.
- **Dampak:** Sistem perlu melacak status ambang per akun dan mendeteksi transisi dari penggunaan di bawah 90% ke penggunaan mencapai atau melewati 90%.
- **Belum diputuskan:** Detail teknis penyimpanan status ambang.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-066: Kanal notifikasi peringatan kapasitas riwayat Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Peringatan saat penggunaan kapasitas riwayat Humanizer mencapai 90% disampaikan melalui notifikasi dalam aplikasi saja. Sistem tidak mengirim peringatan kapasitas tersebut melalui email.
- **Alasan:** Peringatan ini berkaitan langsung dengan pengelolaan riwayat dan dapat ditampilkan saat pengguna menggunakan halaman riwayat Humanizer.
- **Alternatif yang dipertimbangkan:** Mengirim notifikasi dalam aplikasi dan email, atau email saja.
- **Dampak:** Sistem perlu menyediakan pemicu dan tampilan notifikasi dalam aplikasi untuk peringatan kapasitas. Keputusan ini merupakan ketentuan khusus untuk peringatan kapasitas riwayat Humanizer dan tidak mengubah kanal notifikasi untuk peristiwa lain.
- **Belum diputuskan:** Detail teknis penyimpanan status ambang dan tampilan notifikasi.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-067: Banner peringatan kapasitas riwayat Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika penggunaan kapasitas riwayat Humanizer mencapai 90% dari kuota akun, sistem menampilkan peringatan dalam bentuk banner pada bagian atas halaman riwayat Humanizer. Banner tidak menghalangi interaksi pengguna dengan halaman.
- **Alasan:** Banner memberi penekanan yang cukup tanpa memaksa pengguna menutup dialog atau menghentikan aktivitasnya.
- **Alternatif yang dipertimbangkan:** Toast sementara, modal/pop-up yang harus ditutup, atau kombinasi banner dan toast.
- **Dampak:** Halaman riwayat Humanizer perlu menyediakan area banner untuk peringatan ambang kapasitas. Kanal tetap notifikasi dalam aplikasi sesuai DEC-066.
- **Belum diputuskan:** Teks, gaya visual, dan perilaku banner setelah ditampilkan ditentukan pada desain UI.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-068: Banner peringatan dapat ditutup dan muncul kembali saat halaman dibuka

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menutup banner peringatan kapasitas riwayat Humanizer secara manual. Jika penggunaan kapasitas akun masih mencapai atau melebihi 90%, banner ditampilkan kembali ketika pengguna membuka kembali halaman riwayat Humanizer.
- **Alasan:** Pengguna dapat menghilangkan banner dari tampilan saat ini tanpa membuat peringatan hilang secara permanen ketika kondisi kapasitas masih kritis.
- **Alternatif yang dipertimbangkan:** Banner hanya dapat ditutup sekali secara permanen, banner hilang otomatis setelah beberapa detik, atau banner tidak dapat ditutup sampai kapasitas turun di bawah 90%.
- **Dampak:** Halaman riwayat Humanizer perlu mendukung tindakan menutup banner dan memeriksa kembali kondisi kapasitas saat halaman dibuka.
- **Belum diputuskan:** Apakah penutupan hanya berlaku selama halaman terbuka atau sesi aktif, serta detail perilaku jika kapasitas turun di bawah 90% ketika halaman masih terbuka.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-069: Hasil Humanizer tetap dapat diakses ketika kuota riwayat penuh

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna memilih menyimpan hasil Humanizer ketika kapasitas riwayat akun penuh, sistem tetap menampilkan hasil pemrosesan dan menyediakan fungsi salin serta unduh. Hasil tersebut tidak disimpan ke riwayat sampai pengguna menghapus entri lama dan kapasitas kembali mencukupi. Kuota penuh tidak boleh menghalangi akses terhadap hasil pemrosesan yang telah selesai.
- **Alasan:** Kapasitas penyimpanan riwayat merupakan batas fitur penyimpanan, bukan alasan untuk menahan atau menghilangkan hasil pemrosesan yang sudah selesai.
- **Alternatif yang dipertimbangkan:** Mengarahkan pengguna menghapus riwayat sebelum hasil ditampilkan atau menyimpan hasil sementara sampai kapasitas dibebaskan.
- **Dampak:** Proses penyimpanan riwayat harus dipisahkan dari penyajian hasil. Sistem perlu memberi tahu bahwa penyimpanan gagal karena kuota penuh, sementara fungsi lihat, salin, dan unduh tetap tersedia.
- **Klarifikasi melalui DEC-070:** Setelah kapasitas dibebaskan, pengguna dapat mencoba menyimpan kembali hasil yang sama dari tampilan hasil yang masih tersedia tanpa menjalankan pemrosesan Humanizer ulang.
- **Belum diputuskan:** Masa ketersediaan tampilan hasil dan perilaku setelah sesi berakhir.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-070: Percobaan ulang penyimpanan hasil Humanizer tanpa pemrosesan ulang

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika hasil Humanizer gagal disimpan karena kuota riwayat penuh, hasil tetap tersedia pada tampilan hasil. Setelah pengguna menghapus entri lama sehingga kapasitas mencukupi, pengguna dapat mencoba menyimpan kembali hasil yang sama tanpa menjalankan pemrosesan Humanizer ulang.
- **Alasan:** Kegagalan penyimpanan akibat kuota adalah kegagalan pada tahap persistensi riwayat, bukan kegagalan pemrosesan. Memproses ulang teks akan membuang waktu dan berpotensi menghasilkan revisi berbeda untuk input yang sama.
- **Alternatif yang dipertimbangkan:** Mewajibkan pengguna menjalankan Humanizer kembali setelah kapasitas dibebaskan.
- **Dampak:** Tampilan hasil perlu mempertahankan akses ke hasil yang belum tersimpan agar pengguna dapat mengulangi tindakan simpan setelah kapasitas tersedia. Mekanisme ini tidak berarti hasil disimpan otomatis ke riwayat.
- **Klarifikasi melalui DEC-071:** Hasil yang belum tersimpan hanya tersedia selama tampilan hasil pada sesi aktif masih terbuka. Jika halaman atau browser ditutup, hasil hilang dan pengguna harus memproses ulang. Hasil tersebut tidak disimpan sementara di server atau browser untuk pemulihan setelah sesi berakhir.
- **Klarifikasi melalui DEC-072:** Jika pengguna mencoba me-refresh halaman saat hasil belum tersimpan, sistem meminta konfirmasi. Jika dibatalkan, halaman dan hasil dipertahankan; jika dikonfirmasi, refresh dilanjutkan dan hasil hilang.
- **Belum diputuskan:** Definisi teknis batas sesi aktif dan detail penanganan navigasi browser.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-071: Hasil Humanizer yang belum tersimpan hilang setelah halaman ditutup

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Hasil Humanizer yang belum berhasil disimpan karena kuota riwayat penuh hanya tersedia selama tampilan hasil pada sesi aktif masih terbuka. Jika pengguna menutup halaman atau browser, hasil yang belum tersimpan hilang dan tidak dapat dipulihkan melalui aplikasi. Pengguna harus menjalankan pemrosesan Humanizer kembali untuk memperoleh hasil tersebut. Sistem tidak menyimpan hasil tersebut secara sementara di server atau browser untuk pemulihan setelah sesi berakhir.
- **Alasan:** Menghindari penyimpanan sementara konten pengguna di server atau browser tanpa kebutuhan retensi yang telah ditetapkan. Pengguna tetap dapat menyalin atau mengunduh hasil sebelum menutup halaman.
- **Alternatif yang dipertimbangkan:** Menyimpan hasil sementara di browser, menyimpan sementara di server selama 24 jam, atau menyimpan sementara di server selama tujuh hari.
- **Dampak:** Antarmuka perlu menjelaskan bahwa hasil yang belum tersimpan tidak akan tersedia setelah halaman/browser ditutup. Pengguna perlu diberi kesempatan untuk menyalin, mengunduh, atau membebaskan kapasitas dan menyimpan hasil sebelum meninggalkan halaman.
- **Belum diputuskan:** Definisi teknis batas sesi aktif dan perilaku refresh halaman.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-072: Konfirmasi sebelum refresh saat hasil Humanizer belum tersimpan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna mencoba me-refresh halaman ketika terdapat hasil Humanizer yang belum tersimpan, sistem menampilkan dialog konfirmasi sebelum refresh dilanjutkan. Jika pengguna membatalkan, halaman dan hasil tetap dipertahankan. Jika pengguna mengonfirmasi, refresh dilanjutkan dan hasil yang belum tersimpan hilang sesuai DEC-071.
- **Alasan:** Refresh dapat terjadi tanpa sengaja dan menghilangkan hasil yang belum disimpan. Konfirmasi memberi kesempatan kepada pengguna untuk membatalkan tindakan tersebut.
- **Alternatif yang dipertimbangkan:** Memperlakukan refresh seperti penutupan halaman tanpa konfirmasi atau mempertahankan hasil setelah refresh.
- **Dampak:** Antarmuka perlu mendeteksi kondisi hasil yang belum tersimpan dan meminta konfirmasi sebelum refresh. Mekanisme konfirmasi tidak boleh menyimpan hasil sementara.
- **Belum diputuskan:** Teks dialog dan detail penanganan navigasi browser ditentukan pada desain UI/teknis.
- **Sumber:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-073: Konfirmasi untuk seluruh navigasi yang meninggalkan hasil Humanizer belum tersimpan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Seluruh tindakan navigasi yang meninggalkan halaman hasil Humanizer ketika terdapat hasil yang belum disimpan harus meminta konfirmasi sebelum navigasi dilanjutkan. Cakupannya meliputi navigasi internal, tombol Back, refresh, dan navigasi/penutupan halaman lain sejauh dapat ditangani oleh mekanisme browser. Jika pengguna membatalkan, halaman dan hasil tetap dipertahankan. Jika pengguna mengonfirmasi, navigasi dilanjutkan dan hasil yang belum disimpan hilang sesuai DEC-071. Tidak ada peringatan kedua setelah konfirmasi pertama; konfirmasi pertama dianggap cukup.
- **Alasan:** Hasil yang belum disimpan berisiko hilang ketika pengguna meninggalkan halaman secara tidak sengaja. Satu konfirmasi memberikan perlindungan tanpa menambah langkah konfirmasi berulang.
- **Alternatif yang dipertimbangkan:** Hanya meminta konfirmasi saat refresh; meminta konfirmasi kedua sebelum hasil benar-benar dihapus.
- **Dampak:** UI perlu menjaga status hasil belum tersimpan dan meminta konfirmasi pada navigasi yang dapat dikendalikan aplikasi. Untuk navigasi browser eksternal, penutupan tab, atau refresh, implementasi harus mengikuti batasan API/perilaku browser; situs tidak selalu dapat menampilkan dialog khusus atau mengendalikan seluruh jalur navigasi. Tidak boleh menjanjikan dialog khusus pada tindakan yang tidak didukung browser. Tidak ada penyimpanan sementara atau pemulihan hasil yang ditambahkan.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-074: Dialog konfirmasi saat navigasi internal meninggalkan hasil Humanizer belum tersimpan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika pengguna mencoba berpindah melalui navigasi internal dari halaman hasil Humanizer sementara hasil belum disimpan, sistem menampilkan dialog konfirmasi modal di tengah layar. Dialog menyediakan dua tindakan: **Tetap di halaman** dan **Lanjutkan**. Memilih Tetap di halaman membatalkan navigasi dan mempertahankan hasil. Memilih Lanjutkan meneruskan navigasi dan menghilangkan hasil yang belum disimpan sesuai DEC-071. Dialog hanya ditampilkan ketika hasil benar-benar belum disimpan; hasil yang sudah tersimpan tidak memerlukan konfirmasi.
- **Alasan:** Kehilangan hasil yang belum disimpan merupakan konsekuensi penting. Dialog modal lebih mudah terlihat dan memberikan pilihan tindakan yang jelas dibanding notifikasi kecil atau panel bawah.
- **Alternatif yang dipertimbangkan:** Notifikasi kecil di bagian atas halaman; panel konfirmasi di bagian bawah layar.
- **Dampak:** Komponen navigasi internal perlu memeriksa status penyimpanan hasil sebelum berpindah halaman. Dialog harus menjelaskan konsekuensi kehilangan hasil dan menyediakan tindakan untuk tetap berada di halaman atau melanjutkan. Keputusan ini menetapkan pola untuk navigasi internal; keterbatasan browser untuk navigasi eksternal, refresh, dan penutupan tab tetap mengikuti DEC-073.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-075: Mempertahankan seluruh kondisi halaman saat navigasi dibatalkan

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna memilih **Tetap di halaman** pada dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan, sistem membatalkan navigasi dan mempertahankan kondisi halaman tanpa perubahan. Ini mencakup posisi scroll, teks, hasil pemrosesan, serta seluruh kondisi editor yang sedang aktif.
- **Alasan:** Membatalkan navigasi harus benar-benar mengembalikan pengguna ke keadaan sebelum tindakan navigasi dimulai. Mengubah posisi atau isi halaman setelah pembatalan dapat mengganggu pekerjaan pengguna.
- **Alternatif yang dipertimbangkan:** Mengembalikan halaman ke posisi paling atas; menampilkan notifikasi pembatalan navigasi.
- **Dampak:** Pembatalan navigasi internal tidak boleh mereset state halaman, memuat ulang hasil, mengubah teks, atau menggeser posisi scroll. State yang relevan harus tetap utuh.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-076: Navigasi langsung setelah konfirmasi meninggalkan hasil Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna memilih **Lanjutkan** pada dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan, sistem langsung meneruskan navigasi. Tidak ada jeda tambahan atau konfirmasi kedua.
- **Alasan:** Pengguna telah memberikan persetujuan secara eksplisit melalui dialog pertama. Jeda tambahan tidak memberi perlindungan berarti dan hanya memperlambat navigasi.
- **Alternatif yang dipertimbangkan:** Memberikan jeda dua detik; menampilkan indikator proses sebelum navigasi.
- **Dampak:** Setelah pilihan Lanjutkan diterima, aplikasi meneruskan navigasi sesuai tindakan yang diminta dan hasil yang belum disimpan hilang sesuai DEC-071. Implementasi tetap harus menangani transisi navigasi secara normal tanpa menambahkan waktu tunggu buatan.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-077: Tombol tutup modal membatalkan navigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Jika pengguna menekan tombol **X** pada dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan, sistem menutup dialog dan memperlakukan tindakan tersebut sama seperti memilih **Tetap di halaman**. Navigasi dibatalkan dan seluruh kondisi halaman tetap dipertahankan.
- **Alasan:** Tombol tutup dialog secara konvensional membatalkan dialog, bukan menyetujui tindakan yang berisiko menghilangkan hasil.
- **Alternatif yang dipertimbangkan:** Memperlakukan tombol X sebagai persetujuan navigasi; tidak menyediakan tombol X.
- **Dampak:** Tombol X tidak boleh meneruskan navigasi atau menghapus hasil yang belum disimpan. Perilakunya harus konsisten dengan tombol **Tetap di halaman** dan DEC-075.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-078: Tombol Escape membatalkan dialog konfirmasi navigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan sedang terbuka, menekan tombol **Escape** menutup dialog dan membatalkan navigasi. Perilakunya sama dengan tombol **X** dan pilihan **Tetap di halaman**. Hasil serta seluruh kondisi halaman tetap dipertahankan.
- **Alasan:** Mendukung pola interaksi keyboard yang umum dan aksesibilitas tanpa menjadikan Escape sebagai persetujuan atas tindakan yang berisiko menghilangkan hasil.
- **Alternatif yang dipertimbangkan:** Menonaktifkan Escape sehingga pengguna harus memilih tombol pada dialog.
- **Dampak:** Dialog perlu menangani input Escape secara konsisten. Escape tidak boleh meneruskan navigasi atau menghapus hasil yang belum disimpan.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-079: Klik backdrop tidak menutup dialog konfirmasi navigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan sedang terbuka, klik pada area backdrop di luar modal tidak menutup dialog dan tidak mengubah status navigasi. Pengguna harus menggunakan tombol **X**, tombol **Escape**, atau pilihan **Tetap di halaman** untuk membatalkan; pilihan **Lanjutkan** digunakan untuk meneruskan navigasi.
- **Alasan:** Mencegah dialog tertutup akibat salah klik ketika tindakan berikutnya dapat menghilangkan hasil yang belum disimpan.
- **Alternatif yang dipertimbangkan:** Mengizinkan klik backdrop menutup dialog dan membatalkan navigasi.
- **Dampak:** Modal harus mengabaikan klik backdrop sebagai tindakan penutupan. Aturan ini tidak mengubah perilaku tombol X, Escape, Tetap di halaman, atau Lanjutkan.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-080: Focus trap pada modal konfirmasi navigasi Humanizer

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan terbuka, fokus keyboard harus tetap berada di dalam modal. Tombol Tab dan Shift+Tab hanya berpindah di antara elemen interaktif modal; ketika mencapai elemen terakhir atau pertama, fokus berpindah melingkar ke elemen pertama atau terakhir. Elemen di belakang modal tidak boleh menerima fokus melalui navigasi keyboard selama modal aktif.
- **Alasan:** Mencegah interaksi keyboard yang tidak sengaja dengan halaman di belakang modal dan menjaga alur aksesibilitas yang konsisten.
- **Alternatif yang dipertimbangkan:** Mengizinkan fokus keyboard berpindah ke elemen di belakang modal.
- **Dampak:** Implementasi modal perlu menerapkan focus trap, mengelola urutan fokus, serta memastikan fokus awal dan pemulihan fokus setelah modal ditutup ditangani secara konsisten pada desain/implementasi aksesibilitas.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-081: Fokus awal pada tombol Tetap di halaman

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan dibuka, fokus keyboard awal diarahkan ke tombol **Tetap di halaman**, bukan tombol **Lanjutkan** atau area teks modal.
- **Alasan:** Memprioritaskan tindakan yang mempertahankan hasil dan mengurangi risiko pengguna mengonfirmasi kehilangan hasil secara tidak sengaja melalui aktivasi keyboard.
- **Alternatif yang dipertimbangkan:** Memberikan fokus awal ke tombol Lanjutkan; memberikan fokus awal ke judul atau area teks modal.
- **Dampak:** Modal harus mengatur fokus awal ke tombol Tetap di halaman ketika dibuka, tetap menerapkan focus trap sesuai DEC-080, dan mengembalikan fokus ke elemen pemicu setelah modal ditutup jika elemen tersebut masih tersedia.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-082: Mengembalikan fokus ke elemen pemicu setelah modal ditutup

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Setelah modal konfirmasi navigasi internal dari halaman hasil Humanizer ditutup tanpa melanjutkan navigasi, fokus keyboard dikembalikan ke elemen yang sebelumnya memicu modal, misalnya tombol Dashboard. Jika elemen pemicu tidak lagi tersedia, mekanisme fallback fokus yang aksesibel ditentukan pada desain teknis.
- **Alasan:** Mengembalikan fokus ke elemen pemicu mempertahankan konteks navigasi keyboard dan membuat pengguna memahami posisi interaksi setelah dialog ditutup.
- **Alternatif yang dipertimbangkan:** Selalu mengembalikan fokus ke area editor; mengembalikan fokus ke elemen yang sebelumnya aktif sebelum modal dibuka.
- **Dampak:** Komponen modal perlu menyimpan referensi elemen pemicu dan mengembalikan fokus ke elemen tersebut ketika dialog ditutup karena pembatalan. Perilaku saat elemen pemicu tidak tersedia perlu ditangani secara aksesibel.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-083: Dukungan screen reader pada modal konfirmasi navigasi

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan harus mendukung screen reader. Ketika modal dibuka, teknologi bantu harus dapat mengenali modal sebagai dialog serta mengumumkan judul dan isi pesan peringatannya agar pengguna memahami konteks dan konsekuensi navigasi.
- **Alasan:** Pengguna screen reader perlu memperoleh informasi yang sama mengenai dialog dan risiko kehilangan hasil sebelum mengambil keputusan.
- **Alternatif yang dipertimbangkan:** Tidak menyediakan pengumuman khusus untuk screen reader.
- **Dampak:** Implementasi perlu menggunakan semantik dialog yang sesuai, menghubungkan judul dan deskripsi modal secara programatis, serta memastikan pengumuman tidak terduplikasi atau terpotong oleh pengelolaan fokus. Detail atribut dan pengujian kompatibilitas ditentukan pada desain teknis dan pengujian aksesibilitas.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-084: Modal konfirmasi navigasi menggunakan alert dialog

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Modal konfirmasi navigasi internal ketika hasil Humanizer belum disimpan harus menggunakan semantik *alert dialog*, bukan dialog biasa. Screen reader harus mengumumkan dialog sebagai peringatan karena tindakan melanjutkan navigasi dapat menyebabkan hasil yang belum disimpan hilang.
- **Alasan:** Konsekuensi tindakan perlu disampaikan secara jelas sebelum pengguna memilih untuk tetap berada di halaman atau melanjutkan navigasi.
- **Alternatif yang dipertimbangkan:** Menggunakan dialog biasa tanpa tingkat urgensi khusus.
- **Dampak:** Implementasi harus menerapkan semantik alert dialog dan tetap mengikuti keputusan aksesibilitas sebelumnya, termasuk pengelolaan fokus. Kompatibilitas dan perilaku pengumuman perlu diuji dengan screen reader; hindari pengumuman berulang atau berlebihan.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-085: Pengumuman otomatis judul dan pesan alert dialog

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan dibuka, screen reader harus secara otomatis mengumumkan judul dan seluruh pesan peringatan tanpa mengharuskan pengguna berpindah fokus untuk membaca pesan tersebut.
- **Alasan:** Pengguna screen reader perlu langsung mengetahui alasan dialog muncul dan konsekuensi melanjutkan navigasi sebelum mengambil keputusan.
- **Alternatif yang dipertimbangkan:** Screen reader hanya mengumumkan judul, sementara isi pesan harus dibaca melalui navigasi screen reader.
- **Dampak:** Implementasi perlu mengatur semantik dan hubungan aksesibel judul/deskripsi alert dialog sehingga judul dan pesan diumumkan saat dialog dibuka. Pengumuman harus dikoordinasikan dengan fokus awal pada tombol **Tetap di halaman** sesuai DEC-081 agar tidak terjadi pengumuman ganda, terpotong, atau saling bertabrakan. Perilaku perlu diuji dengan kombinasi screen reader dan browser yang ditetapkan pada desain teknis.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.


## DEC-086: Pengumuman ulang isi alert dialog hanya atas navigasi manual pengguna

- **Tanggal:** 2026-11-12
- **Status:** Accepted
- **Keputusan:** Judul dan seluruh pesan peringatan pada modal konfirmasi navigasi Humanizer diumumkan secara otomatis satu kali ketika modal dibuka. Sistem tidak mengumumkan ulang pesan secara otomatis setiap kali fokus berpindah ke bagian isi modal. Pengguna screen reader tetap dapat membaca ulang isi pesan melalui navigasi screen reader secara manual.
- **Alasan:** Menghindari pengumuman berulang yang mengganggu, sekaligus mempertahankan kendali pengguna untuk meninjau ulang pesan sesuai kebutuhan.
- **Alternatif yang dipertimbangkan:** Mengumumkan ulang pesan secara otomatis setiap kali fokus berpindah ke bagian isi modal.
- **Dampak:** Implementasi harus membatasi pengumuman otomatis pada saat pembukaan modal dan tidak memasang perilaku pengumuman ulang yang terpicu oleh perpindahan fokus. Isi pesan tetap tersedia bagi navigasi screen reader. Pengujian aksesibilitas perlu memastikan pengumuman awal tidak terjadi dua kali.
- **Sumber/bukti:** Konfirmasi eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-12.

 
## DEC-087: Batas maksimal karakter input AI Humanizer

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Satu pekerjaan AI Humanizer menerima maksimal **50.000 karakter** untuk keseluruhan teks yang akan diproses. Batas ini berlaku pada satu input gabungan setelah ekstraksi dokumen dan pengeditan manual, bukan sebagai batas terpisah untuk setiap bagian atau chunk. Sistem tetap menerapkan batas token internal sesuai provider/model yang digunakan. Chunking tidak boleh digunakan untuk melewati batas karakter keseluruhan.
- **Alasan:** Pemilik proyek memilih 50.000 karakter dengan pertimbangan keseimbangan kualitas hasil, biaya, dan waktu pemrosesan.
- **Alternatif yang dipertimbangkan:** Batas 10.000, 20.000, 30.000, dan 100.000 karakter, serta angka khusus.
- **Dampak:**
  - Validasi batas karakter harus dilakukan terhadap keseluruhan input pekerjaan Humanizer, termasuk teks hasil ekstraksi dan tambahan/editan manual di editor.
  - Backend wajib memvalidasi batas sebelum pemrosesan.
  - Jika input melebihi 50.000 karakter, sistem menolak pemrosesan dan meminta pengguna mengurangi teks.
  - Strategi chunking, estimasi token, dan pengendalian biaya harus tetap bekerja di dalam batas keseluruhan tersebut.
  - Kelayakan teknis angka ini perlu divalidasi setelah provider/model dan tokenizer dipilih. Persetujuan batas produk ini bukan klaim bahwa kapasitas provider telah dibuktikan.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-088: Batas minimal karakter input AI Humanizer

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Satu pekerjaan AI Humanizer mensyaratkan minimal **100 karakter** pada keseluruhan teks di editor sebelum dapat diproses. Penghitungan mencakup spasi dan baris baru. Ketentuan minimum ini berlaku untuk seluruh gaya penulisan pada MVP.
- **Alasan:** Batas 100 karakter memberi konteks awal yang cukup untuk penyuntingan tanpa terlalu membatasi teks pendek, seperti deskripsi, kutipan, atau paragraf pembuka.
- **Alternatif yang dipertimbangkan:** Tidak menerapkan batas minimum atau menggunakan ambang minimum yang lebih tinggi.
- **Dampak:**
  - Validasi minimum diterapkan pada keseluruhan isi editor, termasuk teks hasil ekstraksi dokumen serta tambahan/editan manual.
  - Jika isi editor kurang dari 100 karakter, tombol pemrosesan dinonaktifkan dan antarmuka menampilkan jumlah karakter saat ini serta jumlah karakter yang masih dibutuhkan.
  - Penghitungan karakter memasukkan spasi dan baris baru.
  - Batas minimum 100 karakter ini berbeda dari batas minimum sampel tulisan untuk membentuk profil gaya personal; batas sampel gaya tetap belum ditentukan.
  - Validasi minimum dan maksimum input Humanizer harus berjalan bersama sebelum pemrosesan.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-089: Batas minimal karakter setiap sampel profil gaya personal

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setiap entri sampel tulisan yang digunakan untuk membentuk atau memperbarui profil gaya personal AI Humanizer harus berisi minimal **500 karakter**. Penghitungan mencakup spasi dan baris baru.
- **Alasan:** Batas ini memberi bahan awal yang lebih memadai untuk mengenali pola kalimat, pilihan kata, dan karakteristik gaya tulisan, tanpa membuat pengumpulan setiap sampel terlalu membebani pengguna.
- **Alternatif yang dipertimbangkan:** Tidak menetapkan batas minimum per sampel atau menetapkan ambang yang lebih tinggi.
- **Dampak:**
  - Sampel di bawah 500 karakter tetap dipertahankan sebagai entri, tetapi belum dapat digunakan sebagai sampel yang memenuhi batas minimum.
  - Sistem menampilkan jumlah karakter sampel saat ini dan jumlah karakter yang masih dibutuhkan.
  - Batas ini berlaku per entri sampel, bukan sebagai ketentuan kecukupan total kumpulan sampel. Kecukupan keseluruhan kumpulan tetap dinilai secara adaptif.
  - Batas minimum ini berbeda dari batas minimum 100 karakter untuk input pekerjaan Humanizer.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-090: Batas jumlah profil gaya personal per akun

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setiap akun dapat memiliki maksimal **6 profil gaya personal**, terdiri dari **1 profil utama** dan maksimal **5 profil tambahan**.
- **Alasan:** Batas ini memberi ruang bagi pengguna untuk memisahkan gaya tulisan berdasarkan kebutuhan, sekaligus menjaga kompleksitas pengelolaan dan penyimpanan profil tetap terkendali.
- **Alternatif yang dipertimbangkan:** Satu profil saja, jumlah profil tambahan tanpa batas, atau batas jumlah lain.
- **Dampak:**
  - Pengguna tidak dapat membuat profil tambahan jika total profil telah mencapai enam.
  - Untuk membuat profil baru ketika batas tercapai, pengguna harus menghapus salah satu profil tambahan terlebih dahulu.
  - Profil utama tidak dapat dihapus sebelum pengguna menetapkan profil utama pengganti.
  - Aturan jumlah profil berlaku terpisah dari batas maksimal versi historis pada setiap profil.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-091: Privasi dan penggunaan profil gaya personal

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Profil gaya personal bersifat privat dan hanya dapat dilihat, dikelola, serta digunakan oleh pemilik akun. Pengguna lain tidak dapat menggunakan profil tersebut. Fitur berbagi profil antar akun tidak termasuk MVP.
- **Alasan:** Profil gaya merepresentasikan karakteristik tulisan pribadi. Membatasi akses pada pemilik akun menjaga privasi serta menghindari kompleksitas kepemilikan, izin akses, dan pembagian profil pada MVP.
- **Alternatif yang dipertimbangkan:** Mengizinkan profil dibagikan atau digunakan oleh akun lain.
- **Dampak:**
  - Kontrol akses profil harus memastikan hanya pemilik akun yang dapat melihat, memilih, menggunakan, memperbarui, atau menghapus profilnya.
  - Tidak tersedia fitur berbagi, publikasi, atau penggunaan lintas akun pada MVP.
  - Pengguna yang menginginkan gaya serupa harus membentuk profil sendiri menggunakan sampel tulisan mereka.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-092: Penghapusan profil gaya personal

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menghapus profil gaya tambahan kapan saja. Profil utama hanya dapat dihapus setelah pengguna menetapkan profil utama pengganti. Sebelum penghapusan, sistem wajib menampilkan dialog konfirmasi. Setelah dikonfirmasi, profil beserta seluruh versi historisnya dihapus secara permanen dari sistem yang dapat diakses pengguna. Penghapusan profil tidak menghapus riwayat pekerjaan Humanizer yang pernah menggunakan profil tersebut.
- **Alasan:** Pengguna perlu memiliki kendali atas data profil personal, sementara dialog konfirmasi mengurangi risiko penghapusan yang tidak disengaja.
- **Alternatif yang dipertimbangkan:** Menonaktifkan profil tanpa menghapusnya atau menghapus profil tanpa konfirmasi.
- **Dampak:**
  - Alur penghapusan harus membedakan profil utama dan profil tambahan.
  - Penghapusan profil utama mensyaratkan penetapan profil utama pengganti terlebih dahulu.
  - Konfirmasi harus dilakukan sebelum penghapusan dijalankan.
  - Riwayat pekerjaan Humanizer yang telah menggunakan profil tetap dipertahankan.
  - Cakupan penghapusan permanen pada sistem aktif harus dibedakan dari retensi backup. Mekanisme dan jangka waktu penghapusan dari backup perlu ditetapkan pada desain teknis dan kebijakan retensi; sistem tidak boleh menjanjikan penghapusan langsung dari seluruh backup sebelum mekanismenya ditentukan.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-093: Pergantian profil gaya utama

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menetapkan profil gaya personal mana pun yang tersedia sebagai profil utama kapan saja. Ketika profil tambahan ditetapkan sebagai profil utama, profil utama sebelumnya otomatis berubah menjadi profil tambahan. Pergantian berlaku untuk pekerjaan Humanizer berikutnya dan tidak mengubah pekerjaan yang sedang berjalan. Pergantian status profil utama tidak membuat versi historis baru karena isi profil tidak berubah.
- **Alasan:** Status profil utama menentukan pilihan default, bukan perubahan isi profil. Memisahkan pergantian status dari versioning menghindari pembuatan versi yang tidak merepresentasikan perubahan profil.
- **Alternatif yang dipertimbangkan:** Mengharuskan pengguna menghapus profil utama lama atau membuat versi historis setiap kali status utama berubah.
- **Dampak:**
  - Pengguna dapat mengganti profil utama tanpa menghapus profil yang sebelumnya menjadi utama.
  - Jumlah profil tetap mengikuti batas satu profil utama dan maksimal lima profil tambahan.
  - Pekerjaan Humanizer yang sudah berjalan mempertahankan profil yang telah dipilih saat pekerjaan dimulai.
  - Pergantian status utama tidak memicu pembentukan atau pemulihan versi profil.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
## DEC-094: Evaluasi ulang setelah setiap perubahan sampel profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setiap penambahan, pengeditan, atau penghapusan entri sampel pada profil gaya yang sudah terbentuk memicu evaluasi ulang terhadap seluruh kumpulan sampel terkini, bukan hanya sampel yang berubah. Setiap evaluasi menggunakan snapshot kumpulan sampel yang konsisten. Jika sampel berubah selama evaluasi, hasil dari snapshot yang sudah usang tidak boleh digunakan untuk membangun atau mengaktifkan versi profil; sistem mengambil snapshot terbaru dan mengevaluasinya kembali.
- **Alasan:** Profil harus merepresentasikan keseluruhan kumpulan sampel terbaru. Mengevaluasi hanya sampel yang berubah dapat mengabaikan interaksi, keragaman, atau konsistensi antarentri.
- **Alternatif yang dipertimbangkan:** Mengevaluasi hanya entri yang berubah; mempertahankan hasil evaluasi lama setelah sampel berubah.
- **Dampak:**
  - Perubahan apa pun pada kumpulan sampel profil yang sudah terbentuk memicu evaluasi seluruh kumpulan.
  - Evaluasi dan pembentukan versi profil harus mengikuti aturan snapshot konsisten pada DEC-038 dan batas pengulangan otomatis pada DEC-039.
  - Versi profil aktif sebelumnya tetap dapat digunakan selama evaluasi belum menghasilkan keputusan valid bahwa sampel tidak memadai, termasuk ketika evaluasi gagal secara teknis, sesuai DEC-026 dan DEC-027.
  - Jika evaluasi menyatakan sampel memadai, pembentukan dan aktivasi versi baru mengikuti DEC-028 serta aturan retensi dan pemulihan versi yang telah disepakati.
  - Jika evaluasi menyatakan sampel tidak memadai, profil tetap terlihat dalam pengelolaan tetapi tidak dapat dipilih sampai kumpulan sampel kembali memenuhi kriteria, sesuai DEC-025.
  - Jika beberapa perubahan terjadi ketika proses pembaruan tertahan oleh pemulihan, sistem menggunakan kumpulan sampel terbaru dan mengevaluasinya satu kali setelah pemulihan selesai, sesuai DEC-034 sampai DEC-037.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13; konsisten dengan DEC-024 sampai DEC-039.
## DEC-095: Penghapusan seluruh sampel profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menghapus seluruh entri sampel pada satu profil gaya dalam satu tindakan. Sebelum penghapusan, sistem menampilkan jumlah sampel yang akan dihapus dan meminta konfirmasi. Setelah dikonfirmasi, seluruh sampel dihapus. Profil yang tidak lagi memiliki sampel tidak dapat digunakan untuk pemrosesan AI Humanizer, tetapi tetap terlihat dalam daftar pengelolaan agar pengguna dapat membangunnya kembali dengan sampel baru. Versi historis profil yang telah terbentuk tetap mengikuti kebijakan retensi dan pemulihan versi yang berlaku.
- **Alasan:** Pengguna perlu memiliki kendali untuk menghapus seluruh tulisan sumbernya. Namun, profil tanpa sampel tidak boleh terus dianggap valid untuk pemrosesan.
- **Alternatif yang dipertimbangkan:** Melarang penghapusan seluruh sampel; menghapus profil secara otomatis; membiarkan profil tetap dapat digunakan tanpa sampel.
- **Dampak:**
  - Tindakan hapus seluruh sampel harus menampilkan jumlah entri yang akan dihapus dan memerlukan konfirmasi eksplisit.
  - Setelah penghapusan, profil tetap tercatat dan terlihat dalam pengelolaan, tetapi tidak tersedia untuk pekerjaan Humanizer.
  - Pengguna dapat menambahkan sampel baru untuk membangun kembali profil; kumpulan baru harus memenuhi batas minimum per sampel dan kriteria kecukupan keseluruhan.
  - Versi historis yang telah terbentuk tidak otomatis dihapus oleh tindakan penghapusan sampel dan tetap tunduk pada kebijakan retensi/pemulihan versi.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-096: Unduh sampel tulisan profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat mengunduh setiap entri sampel tulisan secara individual dan mengunduh seluruh sampel dalam satu profil sekaligus sebagai satu file ZIP. Untuk sampel yang berasal dari dokumen, format unduhan individual mengikuti format sumber asli jika format tersebut tersedia. Untuk sampel yang berasal dari teks yang ditempel langsung, sistem menyediakan unduhan TXT. Pengunduhan tidak mengubah, menghapus, atau memicu evaluasi ulang sampel yang tersimpan.
- **Alasan:** Sampel merupakan data tulisan milik pengguna. Fitur ekspor mendukung portabilitas data tanpa mengubah kondisi profil atau kumpulan sampel.
- **Alternatif yang dipertimbangkan:** Tidak menyediakan ekspor; hanya menyediakan ekspor seluruh sampel; hanya menyediakan unduhan individual.
- **Dampak:**
  - Pengguna dapat mengunduh satu entri sampel tanpa harus mengekspor seluruh profil.
  - Pengguna dapat mengekspor semua sampel dalam satu profil sebagai satu file ZIP.
  - Format unduhan individual mengikuti format sumber asli jika tersedia; teks yang ditempel langsung dapat diunduh sebagai TXT.
  - Proses unduh bersifat baca-saja dan tidak mengubah sampel, profil, versi aktif, atau versi historis.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-097: Batas ukuran file sampel profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Ukuran maksimum setiap file sampel profil gaya adalah 10 MB. Batas berlaku per file, bukan akumulasi seluruh sampel. Format file yang diterima tetap TXT, DOCX, dan PDF. File yang melebihi batas ditolak sebelum proses ekstraksi dan sistem menampilkan pesan yang menyebutkan ukuran maksimum yang diizinkan. Ketentuan ini berlaku untuk MVP dan dapat dievaluasi kembali setelah pengujian teknis.
- **Alasan:** Sampel digunakan untuk mengekstrak teks sebagai bahan pembentukan profil gaya. Batas 10 MB menjadi batas awal yang perlu divalidasi terhadap kebutuhan ekstraksi dokumen, terutama PDF dan DOCX.
- **Alternatif yang dipertimbangkan:** Tidak membatasi ukuran file; menerapkan batas total untuk seluruh sampel; menetapkan batas lebih rendah atau lebih tinggi.
- **Dampak:**
  - Validasi ukuran dilakukan pada setiap file sebelum ekstraksi.
  - File di atas 10 MB ditolak dan pengguna diberi tahu batas ukuran yang berlaku.
  - Pengujian teknis perlu memvalidasi kelayakan batas ini terhadap file TXT, DOCX, dan PDF.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-098: Jumlah file sampel dalam satu tindakan unggah

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat mengunggah maksimal 5 file sampel dalam satu tindakan unggah. Batas ukuran 10 MB tetap berlaku untuk setiap file. Pengguna dapat melakukan tindakan unggah berikutnya untuk menambahkan file lain. Sistem memvalidasi format dan ukuran setiap file secara individual. File yang tidak valid ditolak, sedangkan file lain yang valid tetap dapat diproses.
- **Alasan:** Batas lima file per tindakan unggah menjaga proses penambahan beberapa sampel tetap praktis tanpa menjadikan satu batch terlalu besar. Batas batch ini berbeda dari jumlah total sampel yang dapat tersimpan pada profil.
- **Alternatif yang dipertimbangkan:** Hanya satu file per tindakan unggah; tanpa batas jumlah file per batch; membatasi jumlah total sampel pada profil dengan angka yang sama.
- **Dampak:**
  - Maksimal lima file dapat dipilih dalam satu tindakan unggah.
  - Setiap file tetap harus memenuhi format yang didukung dan batas 10 MB per file.
  - Validasi dilakukan per file; kegagalan satu file tidak otomatis menggagalkan file lain yang valid dalam batch yang sama.
  - Pengguna dapat mengulangi tindakan unggah untuk menambahkan file lain; jumlah total sampel tersimpan merupakan batasan yang terpisah dan belum ditentukan oleh keputusan ini.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-099: Penanganan kegagalan ekstraksi file sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setiap file sampel diproses dan diekstrak secara independen. Jika ekstraksi berhasil, sampel dimasukkan ke daftar sampel profil. Jika ekstraksi gagal, file tersebut tidak dimasukkan sebagai sampel. Sistem menampilkan nama file dan alasan kegagalan. Pengguna dapat mencoba mengunggah ulang file yang gagal atau menggunakan file lain. Sampel yang berhasil diekstrak tetap tersimpan meskipun file lain dalam batch yang sama gagal.
- **Alasan:** Kegagalan satu file tidak seharusnya menggagalkan file lain yang valid dalam batch. Penanganan independen membuat hasil unggahan lebih jelas dan menghindari hilangnya sampel yang berhasil diproses.
- **Alternatif yang dipertimbangkan:** Menggagalkan seluruh batch jika satu file gagal; memasukkan file yang gagal diekstrak sebagai sampel kosong atau tidak lengkap.
- **Dampak:**
  - Status ekstraksi ditentukan per file.
  - File yang gagal diekstrak tidak menjadi entri sampel.
  - Antarmuka menampilkan nama file dan alasan kegagalan untuk setiap file yang gagal.
  - Pengguna dapat mengunggah ulang file yang gagal atau memilih file pengganti.
  - Sampel yang berhasil diekstrak tetap tersedia dan tidak dibatalkan oleh kegagalan file lain.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-100: Pratinjau dan konfirmasi teks hasil ekstraksi sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setelah ekstraksi file sampel berhasil, sistem menampilkan pratinjau teks hasil ekstraksi. Pengguna dapat mengedit teks tersebut sebelum menyimpannya atau membatalkan sampel jika hasil ekstraksi tidak sesuai. Sampel baru disimpan hanya setelah pengguna menekan tombol konfirmasi. Batas minimum 500 karakter diperiksa terhadap teks final setelah diedit. Jika hasil ekstraksi terdeteksi tidak lengkap, sistem memberikan peringatan sebelum pengguna menyimpan sampel.
- **Alasan:** Hasil ekstraksi PDF dan DOCX dapat kehilangan atau mengubah struktur teks, sehingga pengguna perlu memeriksa dan memperbaiki hasil sebelum menjadikannya sumber profil gaya.
- **Alternatif yang dipertimbangkan:** Menyimpan hasil ekstraksi otomatis; hanya menampilkan pratinjau tanpa kemampuan mengedit; mewajibkan pengguna mengunggah ulang jika hasil ekstraksi kurang tepat.
- **Dampak:**
  - Hasil ekstraksi yang berhasil tidak langsung menjadi sampel tersimpan.
  - Pengguna harus mengonfirmasi penyimpanan setelah meninjau pratinjau.
  - Pengguna dapat mengedit hasil ekstraksi atau membatalkan sampel.
  - Validasi minimum 500 karakter dilakukan pada teks final yang akan disimpan.
  - Peringatan ketidaklengkapan ekstraksi ditampilkan sebelum konfirmasi penyimpanan jika kondisi tersebut terdeteksi.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-101: Batas jumlah total sampel per profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Setiap profil gaya dapat menyimpan maksimal 20 entri sampel. Batas berlaku untuk seluruh sampel, baik yang dimasukkan melalui teks tempel maupun diunggah sebagai file. Setiap entri tetap harus memenuhi batas minimum 500 karakter. Jika profil telah mencapai 20 entri, pengguna harus menghapus sampel yang sudah ada sebelum menambahkan sampel baru. Pengguna tetap dapat mengedit sampel yang sudah tersimpan. Penghapusan dan pengeditan mengikuti mekanisme evaluasi ulang yang telah disepakati.
- **Alasan:** Batas total sampel membantu mengendalikan biaya dan durasi evaluasi profil, sekaligus menyediakan ruang bagi variasi tulisan. Angka 20 merupakan batas awal dan perlu divalidasi melalui pengujian algoritma profiling.
- **Alternatif yang dipertimbangkan:** Tidak membatasi jumlah sampel; menetapkan batas lebih rendah; menetapkan batas lebih tinggi.
- **Dampak:**
  - Maksimal 20 entri sampel tersimpan pada setiap profil gaya.
  - Batas berlaku lintas metode input, bukan hanya jumlah file dalam satu batch.
  - Pengguna yang mencapai batas harus menghapus sampel lama sebelum menambahkan entri baru.
  - Pengeditan sampel yang sudah ada tetap diperbolehkan dan mengikuti evaluasi ulang.
  - Pengujian teknis perlu memvalidasi dampak batas ini terhadap kualitas profiling, biaya, dan waktu evaluasi.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-102: Urutan dan pengurutan daftar sampel profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Urutan default daftar sampel adalah berdasarkan waktu penambahan terbaru, sehingga sampel terbaru tampil paling atas. Pengguna dapat mengubah pengurutan berdasarkan tanggal ditambahkan atau nama file. Pengurutan hanya memengaruhi tampilan daftar, bukan urutan evaluasi atau isi profil, dan tidak memicu evaluasi ulang. Sampel dari teks tempel yang tidak memiliki nama file menggunakan label yang dibuat sistem. Urutan sampel tidak memengaruhi hasil profiling.
- **Alasan:** Pengurutan membantu pengguna menemukan sampel terbaru atau mencari sampel berdasarkan nama, sementara pemisahan urutan tampilan dari proses profiling mencegah perubahan presentasi memengaruhi hasil evaluasi.
- **Alternatif yang dipertimbangkan:** Hanya menyediakan satu urutan tetap; menjadikan urutan tampilan sebagai urutan evaluasi; mengurutkan berdasarkan nama file saja.
- **Dampak:**
  - Daftar sampel secara default menampilkan sampel terbaru di posisi paling atas.
  - Pengguna dapat memilih pengurutan berdasarkan tanggal ditambahkan atau nama file.
  - Sampel teks tempel tanpa nama file diberi label oleh sistem.
  - Perubahan pengurutan tidak mengubah data sampel, urutan evaluasi, isi profil, atau memicu evaluasi ulang.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-103: Penamaan entri sampel profil gaya

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Sampel yang dibuat dari teks tempel mendapat nama default otomatis, misalnya "Sampel 1", "Sampel 2", dan seterusnya. Pengguna dapat mengganti nama sampel. Nama harus unik dalam satu profil dan maksimal 60 karakter. Nama sampel tidak memengaruhi isi teks, evaluasi, atau hasil profiling. Untuk sampel dari file, nama default mengikuti nama file tanpa ekstensi dan pengguna tetap dapat mengubahnya.
- **Alasan:** Nama otomatis memberi identitas pada sampel tanpa nama file, sementara penamaan ulang membantu pengguna mengenali sumber atau jenis tulisan.
- **Alternatif yang dipertimbangkan:** Menggunakan nama otomatis yang tidak dapat diubah; mewajibkan pengguna memberi nama setiap sampel; menggunakan nama file atau teks awal sebagai nama tetap.
- **Dampak:**
  - Sampel dari teks tempel memperoleh nama default berurutan yang dibuat sistem.
  - Sampel dari file menggunakan nama file tanpa ekstensi sebagai nama default.
  - Pengguna dapat mengganti nama sampel.
  - Nama harus unik dalam lingkup satu profil dan dibatasi maksimal 60 karakter.
  - Perubahan nama tidak mengubah teks sampel, tidak memicu evaluasi ulang, dan tidak memengaruhi hasil profiling.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-104: Validasi nama sampel duplikat

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Nama sampel harus unik dalam satu profil dengan validasi yang tidak membedakan huruf besar dan kecil. Spasi di awal dan akhir nama dihapus otomatis sebelum validasi. Jika nama duplikat, penyimpanan sampel atau perubahan nama ditolak dan sistem menampilkan pesan bahwa nama sudah digunakan. Nama yang sama tetap boleh digunakan pada profil yang berbeda.
- **Alasan:** Validasi ini mencegah kebingungan saat pengguna mengelola banyak sampel dan menghindari duplikasi semu akibat perbedaan kapitalisasi atau spasi di tepi nama.
- **Alternatif yang dipertimbangkan:** Mengizinkan nama duplikat; membedakan nama berdasarkan kapitalisasi; menambahkan angka secara otomatis pada nama duplikat.
- **Dampak:**
  - Keunikan nama diperiksa dalam lingkup satu profil.
  - Pemeriksaan tidak peka terhadap kapitalisasi, sehingga "Esai Kampus" dan "esai kampus" dianggap sama.
  - Spasi di awal dan akhir nama dipangkas sebelum validasi.
  - Jika terjadi duplikasi, penyimpanan atau perubahan nama ditolak dan pengguna diminta memilih nama lain.
  - Nama yang sama dapat digunakan pada profil lain.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-105: Karakter yang diperbolehkan dalam nama sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Nama sampel boleh menggunakan huruf, angka, spasi, tanda baca, dan karakter Unicode, termasuk karakter dari Bahasa Indonesia maupun bahasa lain. Nama tidak boleh kosong setelah spasi di awal dan akhir dipangkas. Nama tidak boleh mengandung karakter kontrol atau karakter yang tidak terlihat. Batas maksimal 60 karakter tetap berlaku. Emoji diperbolehkan selama memenuhi batas karakter dan bukan karakter kontrol. Nama disimpan sebagai teks biasa dan harus di-escape secara aman saat ditampilkan untuk mencegah XSS.
- **Alasan:** Aturan karakter yang luas mendukung nama yang deskriptif dan multibahasa tanpa mengorbankan keamanan tampilan maupun validasi.
- **Alternatif yang dipertimbangkan:** Membatasi nama hanya pada huruf dan angka; melarang tanda baca atau emoji; mengizinkan seluruh karakter tanpa validasi keamanan.
- **Dampak:**
  - Nama sampel mendukung karakter Unicode, tanda baca, spasi, dan emoji.
  - Nama kosong setelah pemangkasan spasi serta nama yang mengandung karakter kontrol atau karakter tak terlihat ditolak.
  - Batas panjang 60 karakter tetap berlaku.
  - Nama harus diperlakukan sebagai teks biasa dan di-escape sesuai konteks saat ditampilkan untuk mencegah XSS.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-106: Pemulihan sampel terhapus selama 30 hari

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Sampel yang dihapus dipindahkan ke area "Recently Deleted" dan dapat dipulihkan selama 30 hari sejak waktu penghapusan. Setelah masa 30 hari berakhir, sampel dihapus permanen. Sampel yang dipulihkan dikembalikan ke profil asal. Jika profil telah mencapai batas 20 sampel, pemulihan ditolak sampai pengguna menyediakan slot. Pemulihan sampel memicu evaluasi ulang profil sesuai mekanisme yang telah disepakati. Penghapusan seluruh sampel juga mengikuti kebijakan pemulihan 30 hari ini.
- **Alasan:** Masa pemulihan melindungi pengguna dari penghapusan yang tidak disengaja, sekaligus menetapkan batas retensi yang jelas bagi sampel yang dihapus.
- **Alternatif yang dipertimbangkan:** Penghapusan langsung permanen; masa pemulihan lebih singkat atau lebih panjang; pemulihan tanpa batas waktu.
- **Dampak:**
  - Sampel yang dihapus masuk ke area "Recently Deleted".
  - Masa pemulihan dihitung selama 30 hari sejak sampel dihapus.
  - Sampel yang melewati masa pemulihan dihapus permanen.
  - Pemulihan mengembalikan sampel ke profil asal dan hanya dapat dilakukan jika tersedia slot dalam batas maksimal 20 sampel.
  - Pemulihan memicu evaluasi ulang profil sesuai aturan evaluasi sampel.
  - Penghapusan seluruh sampel juga memindahkan semua sampel terkait ke area "Recently Deleted" dan menerapkan masa pemulihan yang sama.
  - Sistem harus memenuhi masa retensi 30 hari dan mengelola kebutuhan penyimpanan terkait.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-107: Penanganan konflik nama saat memulihkan sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Sebelum memulihkan sampel dari Recently Deleted, sistem memeriksa apakah nama sampel tersebut bertabrakan dengan nama sampel aktif dalam profil asal. Jika terjadi konflik, pemulihan ditolak dan sampel tetap berada di Recently Deleted. Sistem memberi tahu pengguna nama yang mengalami konflik. Pengguna harus mengganti nama sampel aktif yang konflik, kemudian mencoba pemulihan kembali. Sistem tidak mengubah nama secara otomatis. Validasi nama yang telah disepakati, termasuk keunikan tanpa membedakan kapitalisasi, pemangkasan spasi tepi, batas panjang, serta aturan karakter, tetap berlaku.
- **Alasan:** Nama sampel merupakan identitas yang ditentukan pengguna. Penggantian nama otomatis dapat mengganggu konsistensi dan keterlacakan sampel.
- **Alternatif yang dipertimbangkan:** Mengganti nama sampel yang dipulihkan secara otomatis; mengganti nama sampel aktif secara otomatis; mengizinkan nama duplikat.
- **Dampak:**
  - Pemeriksaan konflik nama dilakukan sebelum pemulihan.
  - Jika konflik terjadi, sampel tidak dipulihkan dan tetap berada di Recently Deleted.
  - Sistem menampilkan nama yang konflik kepada pengguna.
  - Pengguna harus mengganti nama sampel aktif yang konflik sebelum mencoba pemulihan lagi.
  - Sistem tidak melakukan perubahan nama otomatis.
  - Seluruh aturan validasi nama sampel yang berlaku tetap diterapkan.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-108: Pemulihan massal seluruh sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat memilih tindakan **Pulihkan Semua** pada Recently Deleted untuk memulihkan seluruh sampel yang masih berada dalam masa pemulihan. Sistem memvalidasi seluruh sampel yang dipilih sebagai satu kesatuan sebelum melakukan perubahan. Validasi mencakup kapasitas maksimum 20 sampel per profil dan seluruh aturan validasi nama, termasuk konflik nama dengan sampel aktif. Jika seluruh sampel memenuhi syarat, semuanya dipulihkan dalam satu tindakan. Jika satu atau lebih sampel gagal validasi, tidak ada sampel yang dipulihkan; seluruh tindakan dibatalkan dan sistem menjelaskan alasan serta sampel yang bermasalah. Pengguna dapat menyelesaikan masalah lalu mencoba kembali. Setelah pemulihan massal berhasil, sistem menjalankan satu evaluasi ulang profil, bukan evaluasi terpisah untuk setiap sampel.
- **Alasan:** Pemulihan massal harus memberikan hasil yang konsisten dan tidak membuat pengguna salah memahami status ketika hanya sebagian sampel berhasil dipulihkan.
- **Alternatif yang dipertimbangkan:** Memulihkan hanya sampel yang lolos validasi sementara sampel lain gagal; meminta pengguna memulihkan setiap sampel satu per satu.
- **Dampak:**
  - Tersedia tindakan Pulihkan Semua pada Recently Deleted.
  - Sistem memvalidasi kapasitas dan konflik nama untuk seluruh sampel sebelum pemulihan.
  - Pemulihan bersifat atomik: semua berhasil atau tidak ada yang dipulihkan.
  - Kegagalan validasi menampilkan alasan dan sampel yang bermasalah.
  - Setelah berhasil, hanya satu evaluasi ulang profil dijalankan.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-109: Penghapusan sampel di Recently Deleted saat profil asal dihapus

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Ketika pengguna menghapus sebuah profil gaya, seluruh sampel yang berada di Recently Deleted dan berasal dari profil tersebut ikut dihapus permanen dari sistem yang dapat diakses pengguna. Sampel tersebut tidak dapat dipulihkan setelah profil asal dihapus. Ketentuan ini berlaku bersama penghapusan profil dan seluruh versi historisnya sebagaimana DEC-092.
- **Alasan:** Sampel yang dipulihkan harus kembali ke profil asal. Setelah profil asal dihapus, tujuan pemulihan tidak lagi tersedia. Penghapusan ini juga menjaga konsistensi antara siklus hidup profil dan sampelnya.
- **Alternatif yang dipertimbangkan:** Mempertahankan sampel di Recently Deleted selama sisa masa 30 hari; memindahkan sampel ke profil lain atau area pemulihan global.
- **Dampak:**
  - Penghapusan profil juga menghapus permanen sampel profil tersebut yang masih berada di Recently Deleted.
  - Sampel tersebut tidak lagi dapat dipulihkan.
  - Sampel aktif dan versi historis profil tetap mengikuti ketentuan penghapusan profil pada DEC-092.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-110: Penghapusan permanen sampel dari Recently Deleted

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Pengguna dapat menghapus satu sampel secara permanen melalui tindakan **Hapus Permanen**, atau menghapus seluruh sampel di Recently Deleted melalui tindakan **Kosongkan Recently Deleted**. Kedua tindakan wajib menampilkan dialog konfirmasi. Setelah dikonfirmasi, sampel tidak dapat dipulihkan melalui aplikasi dan tindakan tidak dapat dibatalkan. Penghapusan ini tidak memicu evaluasi ulang profil karena sampel yang berada di Recently Deleted sudah tidak termasuk dalam kumpulan sampel aktif. Penghapusan dari backup mengikuti kebijakan retensi teknis yang akan ditentukan pada tahap desain teknis.
- **Alasan:** Pengguna perlu memiliki kendali untuk menghapus data yang tidak lagi ingin disimpan tanpa menunggu masa pemulihan 30 hari berakhir.
- **Alternatif yang dipertimbangkan:** Mewajibkan pengguna menunggu 30 hari hingga penghapusan otomatis; hanya menyediakan penghapusan permanen satu per satu; hanya menyediakan pengosongan seluruh area.
- **Dampak:**
  - Tersedia tindakan Hapus Permanen untuk satu sampel.
  - Tersedia tindakan Kosongkan Recently Deleted untuk seluruh sampel.
  - Kedua tindakan memerlukan konfirmasi eksplisit.
  - Sampel yang dihapus permanen tidak dapat dipulihkan melalui aplikasi dan tindakan tidak dapat dibatalkan.
  - Tidak ada evaluasi ulang profil karena sampel tersebut sudah tidak aktif.
  - Retensi/penghapusan salinan backup ditentukan pada desain teknis.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.

## DEC-111: Audit log untuk penghapusan permanen sampel

- **Tanggal:** 2026-11-13
- **Status:** Accepted
- **Keputusan:** Sistem mencatat metadata setiap tindakan penghapusan permanen sampel, baik penghapusan satu sampel maupun tindakan Kosongkan Recently Deleted. Catatan audit memuat ID sampel, ID profil asal, ID pengguna yang melakukan tindakan, waktu penghapusan, jenis tindakan, serta status keberhasilan atau kegagalan. Isi teks sampel tidak boleh disimpan di audit log.
- **Alasan:** Metadata audit mendukung investigasi kesalahan dan sengketa tanpa menggandakan konten pribadi pengguna ke dalam sistem audit.
- **Alternatif yang dipertimbangkan:** Tidak mencatat penghapusan permanen; mencatat isi sampel bersama metadata; mencatat hanya pengosongan massal.
- **Dampak:**
  - Penghapusan permanen satu sampel dan pengosongan massal menghasilkan catatan audit.
  - Audit log hanya menyimpan metadata yang ditetapkan, bukan isi teks sampel.
  - Retensi dan kontrol akses audit log mengikuti kebijakan audit dan keamanan yang akan dirinci pada desain teknis.
- **Sumber/bukti:** Persetujuan eksplisit pemilik proyek dalam sesi elisitasi kebutuhan 2026-11-13.
