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
