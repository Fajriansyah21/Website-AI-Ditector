# Lexora System Requirements Analysis

- **Status:** Draft hasil elisitasi kebutuhan
- **Tanggal:** 2026-11-12
- **Cakupan:** MVP AI Text Detector dan AI Humanizer
- **Sumber:** Keputusan eksplisit pemilik proyek dalam sesi analisis kebutuhan 2026-11-12
- **Dokumen terkait:** [Product Overview](PRODUCT-OVERVIEW.md), [Decision Log](DECISION-LOG.md), [Active Roadmap](ACTIVE-ROADMAP.md), [AI Text Detector Plan](AI-TEXT-DETECTOR-PLAN.md), [AI Humanizer Plan](AI-HUMANIZER-PLAN.md)

## 1. Tujuan dan status dokumen

Dokumen ini merangkum kebutuhan produk dan kebutuhan sistem yang telah dipilih selama sesi elisitasi. Dokumen menjadi dasar untuk tahap user flow, system design, data flow, evaluasi teknologi, dan spesifikasi MVP.

Dokumen ini **belum merupakan desain teknis final atau bukti kelayakan implementasi**. Pilihan yang telah disetujui pemilik proyek dicatat sebagai keputusan produk. Angka, provider, teknologi, kebijakan operasional, dan detail yang belum dibahas ditandai sebagai belum diputuskan.

### 1.1 Konvensi status

- **Disepakati:** pilihan produk telah dikonfirmasi pemilik proyek.
- **Rancangan:** perilaku yang diturunkan untuk menjelaskan keputusan, tetapi masih perlu desain teknis.
- **Belum diputuskan:** detail belum memiliki keputusan.
- **Perlu validasi:** keputusan produk memerlukan pemeriksaan kebijakan, keamanan, biaya, atau kelayakan teknis sebelum implementasi.

## 2. Ringkasan produk dan ruang lingkup

Lexora adalah website untuk menganalisis dan mengolah tulisan dalam konteks akademik maupun umum.

### 2.1 Target pengguna

- **Target utama:** mahasiswa dan pelajar.
- **Cakupan penggunaan:** akademik dan umum.
- **Contoh akademik:** tugas sekolah, esai, makalah, laporan, dan tulisan ilmiah.
- **Contoh umum:** artikel, konten, tulisan blog, opini, dan deskripsi produk.

Mahasiswa dan pelajar merupakan prioritas produk, tetapi sistem tidak dibatasi hanya untuk tulisan akademik.

### 2.2 Cakupan MVP

MVP mencakup dua modul dengan prioritas setara:

1. **AI Text Detector:** menganalisis karakteristik teks dan memberikan estimasi kategori.
2. **AI Humanizer:** menyunting teks agar lebih alami, jelas, dan sesuai gaya yang dipilih tanpa sengaja mengubah makna, fakta, atau maksud.

Kedua modul:
- Kedua modul dapat digunakan secara mandiri, tetapi hak aksesnya berbeda berdasarkan status pengguna.
- Guest hanya dapat menggunakan AI Text Detector.
- AI Humanizer hanya dapat digunakan oleh pengguna yang sudah login/terdaftar.
- Pengguna terdaftar dapat menggunakan kedua modul dan menghubungkan hasil Detector ke Humanizer.
- Tidak menjalankan Humanizer secara otomatis setelah Detector.
- Memerlukan tindakan/konfirmasi pengguna sebelum teks hasil Detector dikirim ke Humanizer.

**DOCX Metadata Cleaner tidak termasuk MVP** dan tetap ditunda ke fase setelah MVP. Arsitektur produk direncanakan modular agar modul tambahan dapat dipertimbangkan kemudian.

### 2.3 Hal yang tidak termasuk MVP berdasarkan sesi ini

- Sistem referral.
- Fitur perbandingan dua teks yang berdiri sendiri. Perbandingan teks asli dan hasil revisi di dalam Humanizer tetap termasuk.
- Live chat. Dukungan pengguna menggunakan formulir dan sistem tiket.
- Kontrol intensitas perubahan Humanizer. Kendali gaya yang dipilih adalah gaya bawaan atau profil gaya personal.

## 3. Aktor dan sistem eksternal

| Aktor | Kebutuhan/peran |
|---|---|
| Guest | Menggunakan AI Text Detector saja dengan batasan; dapat memperoleh reward iklan terbatas untuk akses Detector |
| Pengguna terdaftar | Menggunakan AI Text Detector dan AI Humanizer, mengelola kredit, riwayat opsional, dan profil gaya |
| Super Admin | Mengelola seluruh konfigurasi dan operasional dengan hak tertinggi |
| Admin | Menangani operasional sesuai izin yang diberikan |
| Moderator | Menangani laporan/moderasi sesuai izin yang diberikan |
| Penyedia AI | Memproses analisis dan/atau penyuntingan teks |
| Payment gateway | Memproses pembayaran dan mengirim status transaksi |
| Penyedia iklan | Menyediakan iklan/reward sesuai produk dan kebijakan yang berlaku |
| Layanan email | Mengirim notifikasi dan komunikasi transaksional |

Detail provider dan kontrak integrasi belum dipilih.

## 4. Kebutuhan fungsional

### 4.1 Bahasa, input, dan pemrosesan umum

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-GEN-01 | Antarmuka dan kedua modul mendukung Bahasa Indonesia dan Bahasa Inggris. | Disepakati |
| FR-GEN-02 | Teks campuran Indonesia-Inggris dapat dikirim untuk diproses. Sistem memberi peringatan bahwa hasil mungkin kurang akurat. | Disepakati; kemampuan aktual perlu diuji |
| FR-GEN-03 | Batas yang terlihat oleh pengguna menggunakan jumlah karakter; batas internal sistem memperhitungkan token. | Disepakati; angka batas belum ditentukan |
| FR-GEN-04 | Detector dan Humanizer dapat memiliki batas input yang berbeda. | Disepakati; angka per modul belum ditentukan |
| FR-GEN-05 | Pemrosesan teks pendek dilakukan sekaligus; teks panjang dapat diproses dengan chunking secara adaptif. | Disepakati sebagai arah; strategi konteks perlu desain |
| FR-GEN-06 | Permintaan ringan dapat diproses langsung, sedangkan permintaan berat atau saat sistem sibuk dapat masuk antrean. | Disepakati sebagai arah |
| FR-GEN-07 | Input divalidasi secara adaptif. Sistem dapat menolak input, memberi saran, atau memproses dengan peringatan sesuai kondisi dan modul. | Disepakati |
| FR-GEN-08 | Validasi wajib dilakukan di backend, bukan hanya di browser. | Persyaratan keamanan rancangan |
| FR-GEN-09 | Pengguna dapat menyalin seluruh hasil atau bagian tertentu. | Disepakati |

### 4.2 Unggah dan ekstraksi dokumen

TXT, DOCX, dan PDF berlaku untuk seluruh kebutuhan unggah:
- Teks yang akan dianalisis Detector.
- Teks yang akan disunting Humanizer.
- Sampel tulisan untuk membentuk profil gaya.

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-FILE-01 | Menerima TXT, DOCX, dan PDF untuk ketiga kebutuhan unggah di atas. | Disepakati |
| FR-FILE-02 | Mengekstrak teks sebelum pemrosesan. | Disepakati |
| FR-FILE-03 | Ekstraksi mempertahankan struktur yang didukung, seperti paragraf, daftar, dan tabel jika memungkinkan. | Disepakati sebagai ekstraksi adaptif |
| FR-FILE-04 | Sistem memberi peringatan jika sebagian konten tidak dapat diekstrak. | Disepakati |
| FR-FILE-05 | PDF berbasis gambar membutuhkan OCR agar teks pada gambar dapat dibaca. | Kebutuhan konseptual; kemampuan OCR/provider belum dipilih |
| FR-FILE-06 | File kosong, rusak, tidak terbaca, atau tidak didukung ditolak dengan pesan yang jelas. | Disepakati |
| FR-FILE-07 | Ukuran file dan batas pemrosesan divalidasi di backend. | Disepakati; angka belum ditentukan |
| FR-FILE-08 | Dokumen sumber tidak boleh diubah atau ditimpa oleh proses ekstraksi/pemrosesan. | Disepakati |
| FR-FILE-09 | PDF yang dilindungi password dapat ditolak jika sistem tidak dapat mengekstraknya dengan aman. | Rancangan; aturan final perlu ditentukan |

Dukungan format file tidak berarti tata letak, gambar, header/footer, catatan kaki, kolom, atau seluruh elemen visual akan dipertahankan. Batas dukungan setiap elemen harus dijelaskan kepada pengguna.

### 4.3 AI Text Detector

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-DET-01 | Menampilkan ringkasan hasil analisis seluruh teks. | Disepakati |
| FR-DET-02 | Menyediakan analisis/indikasi pada tingkat kalimat. | Disepakati; metode anotasi/klasifikasi belum ditentukan |
| FR-DET-03 | Menyediakan penjelasan karakteristik yang mendukung hasil sejauh dapat ditelusuri ke analisis. | Disepakati |
| FR-DET-04 | Mendukung kategori HUMAN, AI, MIXED, dan UNCERTAIN. | Disepakati sebagai kategori produk; mekanisme klasifikasi perlu divalidasi |
| FR-DET-05 | Menampilkan keterbatasan dan peringatan saat teks terlalu pendek, campuran bahasa, ekstraksi tidak lengkap, atau hasil tidak meyakinkan. | Disepakati |
| FR-DET-06 | Tidak menyatakan hasil sebagai bukti pasti bahwa seseorang menggunakan AI. | Prinsip wajib |
| FR-DET-07 | Skor atau persentase tidak boleh dipresentasikan sebagai probabilitas terkalibrasi kecuali kalibrasi benar-benar diuji dan dibuktikan. | Prinsip wajib |
| FR-DET-08 | Setelah analisis, pengguna boleh memilih untuk mengirim teks ke Humanizer. Tidak ada pemrosesan otomatis. | Disepakati |
| FR-DET-09 | AI Text Detector menerapkan batas maksimal jumlah karakter pada input teks yang berlaku sama bagi Guest dan pengguna terdaftar. Sistem menolak atau meminta pengguna menyesuaikan input yang melewati batas. | Disepakati; angka batas dan perilaku UX final belum ditentukan |
| FR-DET-10 | Pengguna dapat memasukkan teks Detector secara langsung melalui area teks dengan mengetik atau menempelkan teks. | Disepakati |
| FR-DET-11 | Pengguna dapat mengunggah file TXT, DOCX, atau PDF sebagai sumber teks Detector. | Disepakati; ketentuan ekstraksi dan batas file mengikuti FR-FILE |

Label HUMAN/AI/MIXED/UNCERTAIN pada antarmuka adalah kebutuhan produk. Definisi operasional label, ambang batas, confidence, kalibrasi, dan pelabelan per kalimat tetap memerlukan rancangan metodologis dan bukti eksperimen.

### 4.4 AI Humanizer

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-HUM-01 | Pengguna dapat memilih gaya penulisan bawaan. | Disepakati; daftar final gaya belum ditentukan |
| FR-HUM-02 | Pengguna dapat memberikan sampel tulisan sendiri untuk membentuk profil gaya personal. | Disepakati |
| FR-HUM-03 | Sampel gaya dapat ditempel langsung atau diunggah sebagai TXT, DOCX, atau PDF. | Disepakati |
| FR-HUM-04 | Sistem menilai kecukupan sampel secara adaptif dan meminta sampel tambahan jika belum memadai. | Disepakati; kriteria penilaian perlu dirancang |
| FR-HUM-15 | Jika sampel belum cukup beragam atau belum konsisten untuk merepresentasikan gaya pengguna, sistem mempertahankan sampel yang telah diberikan dan meminta pengguna menambahkan tulisan sebagai entri sampel terpisah. Setiap sampel ditampilkan dan dikelola sebagai entri tersendiri, sedangkan sistem menilai seluruh sampel sebagai satu kumpulan untuk membentuk atau memperbarui profil gaya. Pengguna tidak perlu mengirim ulang sampel sebelumnya. | Disepakati; indikator keragaman, konsistensi, dan struktur penyimpanan perlu dirancang |
| FR-HUM-16 | Pengguna dapat mengedit atau menghapus setiap entri sampel profil gaya secara individual tanpa harus menghapus atau mengirim ulang entri sampel lainnya. | Disepakati; dampak perubahan sampel terhadap profil gaya yang sudah terbentuk perlu dirancang |
| FR-HUM-14 | Sampel tulisan untuk membentuk profil gaya personal harus memenuhi batas minimal karakter. Jika belum memenuhi, sistem mempertahankan sampel yang sudah diberikan dan meminta pengguna menambahkan tulisan. Sistem menampilkan jumlah minimum yang diwajibkan serta jumlah karakter sampel saat ini. Penghitungan mencakup seluruh karakter, termasuk spasi dan baris baru. | Disepakati; angka minimum dan perlakuan karakter khusus belum ditentukan |
| FR-HUM-05 | Pengguna dapat memilih untuk menyimpan profil gaya, menggunakan kembali, memperbarui, atau menghapusnya. | Disepakati |
| FR-HUM-06 | Akun dapat memiliki satu profil utama dan beberapa profil tambahan. | Disepakati; batas jumlah belum ditentukan |
| FR-HUM-07 | Profil gaya bersifat privat dan hanya dapat dikelola pemilik akun. | Disepakati |
| FR-HUM-08 | Humanizer menampilkan teks asli, hasil revisi, dan perubahan yang disorot. | Disepakati |
| FR-HUM-09 | Pengguna tetap menentukan apakah hasil revisi akan digunakan. | Disepakati |
| FR-HUM-10 | Makna, fakta, angka, nama, istilah penting, dan maksud teks harus dipertahankan sejauh memungkinkan. | Prinsip kualitas |
| FR-HUM-11 | Humanizer tidak menjanjikan hasil pasti lolos atau mengelabui AI Detector. | Prinsip produk |
| FR-HUM-12 | Humanizer berusaha mempertahankan bahasa campuran jika input menggunakan code-switching. | Disepakati sebagai arah; perlu diuji |
| FR-HUM-13 | Pengguna dapat menyalin hasil dan mengunduh keluaran. | Disepakati |

Gaya bawaan seperti akademik, formal, profesional, santai, informatif, dan kreatif merupakan contoh kandidat, bukan daftar final yang telah disahkan. Pengguna memilih gaya, tetapi tidak memilih intensitas perubahan pada tahap MVP.

### 4.5 Akun, autentikasi, dashboard, dan riwayat

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-ACC-01 | Guest dapat menggunakan fitur dengan batasan. | Disepakati |
| FR-ACC-02 | Guest hanya dapat menggunakan AI Text Detector; AI Humanizer mensyaratkan pengguna login/terdaftar. | Disepakati |
| FR-ACC-03 | Perbedaan kuota Guest dan pengguna terdaftar mengacu pada jumlah kredit/reward yang tersedia, bukan batas maksimal karakter input. | Disepakati; besaran kredit/reward belum ditentukan, batas karakter per status pengguna belum diputuskan |
| FR-ACC-04 | Registrasi dan login mendukung email/password serta Google Sign-In. | Disepakati |
| FR-ACC-05 | Mendukung verifikasi email dan reset password. | Disepakati |
| FR-ACC-06 | Menangani akun email/password dan Google dengan alamat sama secara aman. | Disepakati sebagai kebutuhan |
| FR-ACC-07 | Menyediakan dashboard pengguna untuk profil, kredit, riwayat, profil gaya, dan pengaturan akun. | Disepakati |
| FR-ACC-08 | Riwayat tidak disimpan otomatis. Pengguna login memilih apakah akan menyimpan riwayat. | Disepakati |
| FR-ACC-09 | Guest tidak memiliki riwayat permanen. Teks input sementara dipertahankan selama perpindahan ke proses autentikasi. Jika autentikasi dibatalkan, teks tetap tersedia ketika pengguna kembali ke Detector selama sesi browser yang sama masih aktif. Hal ini bukan penyimpanan riwayat dan tidak menjanjikan pemulihan setelah sesi browser berakhir. | Disepakati |
| FR-ACC-10 | Pengguna dapat menghapus riwayat yang disimpan. | Disepakati |
| FR-ACC-11 | Profil gaya disimpan secara terpisah dari sampel dokumen sumber. | Disepakati sebagai prinsip pemisahan data |
| FR-ACC-12 | Akses ke AI Humanizer dan pengelolaan profil gaya memerlukan autentikasi pengguna. | Disepakati |
| FR-ACC-13 | Jika Guest memilih mendaftar/login dari alur AI Text Detector, teks input yang sedang dikerjakan dibawa ke sesi pengguna setelah autentikasi berhasil. | Disepakati; mekanisme teknis dan masa berlaku draft belum ditentukan |
| FR-ACC-14 | Hasil analisis Guest tidak dibawa sebagai hasil final setelah registrasi/login. Pengguna menjalankan analisis ulang sebagai permintaan baru menggunakan akun terdaftar. | Disepakati |
| FR-ACC-15 | Jika Guest membatalkan autentikasi dan kembali ke Detector, teks input tetap tersedia selama sesi browser yang sama masih aktif. Draft tidak dijanjikan dapat dipulihkan setelah sesi browser berakhir. | Disepakati; perilaku saat reload/refresh dan batas teknis sesi belum ditentukan |

### 4.6 Kredit, iklan, dan pembayaran

Lexora menggunakan kredit internal sebagai unit konsumsi layanan. Kredit bukan token model AI.

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-CRED-01 | Kredit dapat diperoleh melalui pembelian, reward iklan, dan bonus harian. | Disepakati |
| FR-CRED-02 | Bonus harian sebesar 20 kredit hanya diberikan kepada pengguna login pada pukul 03.00. | Disepakati; zona waktu dan definisi periode belum dikonfirmasi |
| FR-CRED-03 | Guest dapat memperoleh reward iklan dengan batas lebih rendah daripada pengguna login, dan reward Guest hanya dapat digunakan untuk AI Text Detector. | Disepakati; angka batas belum ditentukan |
| FR-CRED-04 | Pengguna login juga dapat memperoleh reward iklan untuk penggunaan fitur yang tersedia pada akun. | Disepakati |
| FR-CRED-05 | Guest tidak memperoleh bonus harian. Saldo guest bersifat sementara dan tidak dapat ditransfer. | Disepakati |
| FR-CRED-06 | Biaya penggunaan terdiri dari biaya dasar per fitur dan biaya variabel yang mempertimbangkan konsumsi token/estimasi biaya. | Disepakati sebagai model; rumus belum ditentukan |
| FR-CRED-07 | Detector dan Humanizer memiliki tarif masing-masing. | Disepakati |
| FR-CRED-08 | Estimasi biaya ditampilkan sebelum pemrosesan. | Disepakati |
| FR-CRED-09 | Pemrosesan ditolak jika kredit tidak mencukupi. Saldo tidak boleh negatif. | Disepakati |
| FR-CRED-10 | Kredit pembelian tidak kedaluwarsa. Kredit bonus dan reward iklan kedaluwarsa setelah 7 hari. | Disepakati |
| FR-CRED-11 | Kredit dengan waktu kedaluwarsa terdekat digunakan lebih dahulu; kredit pembelian digunakan setelah kredit promosi yang relevan. | Disepakati |
| FR-CRED-12 | Setiap penambahan, pemotongan, kedaluwarsa, dan refund dicatat dalam ledger. | Kebutuhan rancangan |
| FR-CRED-13 | Kegagalan server Lexora atau penyedia AI mengakibatkan refund sesuai status transaksi yang diverifikasi. | Disepakati |
| FR-CRED-14 | Pembatalan saat antrean mengembalikan kredit; pembatalan saat pemrosesan memerlukan verifikasi apakah pekerjaan benar-benar berhenti. | Disepakati |
| FR-CRED-15 | Input tidak valid yang terdeteksi sebelum pemrosesan tidak dipotong kreditnya. | Disepakati |
| FR-CRED-16 | Refund tidak boleh terjadi lebih dari satu kali untuk satu transaksi pemrosesan. | Kebutuhan integritas transaksi |

Pembelian kredit:
- Menyediakan paket kredit tetap dan jumlah khusus.
- Metode pembayaran yang dituju: QRIS, e-wallet, transfer bank, dan Virtual Account.
- Pembayaran menggunakan payment gateway; provider belum dipilih.
- Guest dapat memulai pembelian, tetapi harus membuat akun/login sebelum kredit diberikan dan transaksi harus dapat dipulihkan jika halaman ditutup.
- Pembayaran gagal atau kedaluwarsa membatalkan transaksi. Pengguna membuat transaksi baru.
- Kredit hanya diberikan setelah status pembayaran diverifikasi server.
- Status transaksi perlu mencakup sekurang-kurangnya pending, paid, failed, expired, dan cancelled.
- Penambahan kredit harus idempotent agar webhook berulang tidak menggandakan saldo.

Reward iklan:
- Pengguna ingin memperoleh pendapatan iklan dan memberi kredit sebagai reward.
- Produk iklan Google yang tepat, kelayakan rewarded ads, dan kepatuhan kebijakannya **belum divalidasi**.
- Google AdSense, Google Ads, dan produk rewarded inventory tidak boleh dianggap saling dapat dipertukarkan tanpa pemeriksaan dokumentasi resmi.
- Jangan mengasumsikan setiap tayangan menghasilkan pendapatan atau reward otomatis memenuhi kebijakan penyedia.

### 4.7 Dukungan pengguna, notifikasi, dan gangguan layanan

| ID | Kebutuhan | Status/catatan |
|---|---|---|
| FR-SUP-01 | Menyediakan formulir laporan/kontak yang membuat tiket bantuan. | Disepakati |
| FR-SUP-02 | Setiap tiket memiliki ID, status, pesan, dan riwayat penanganan. | Disepakati |
| FR-SUP-03 | Pengguna dapat memantau status tiket. | Disepakati |
| FR-NOT-01 | Notifikasi tersedia di website dan email. | Disepakati |
| FR-NOT-02 | Notifikasi mencakup peristiwa penting seperti pembayaran, kredit, tiket, dan keamanan akun. | Disepakati |
| FR-NOT-03 | Notifikasi promosi dipisahkan dan memerlukan persetujuan pengguna. | Disepakati |
| FR-OPS-01 | Menampilkan status gangguan layanan dan menyediakan mekanisme pemulihan/retry yang aman. | Disepakati |
| FR-OPS-02 | Permintaan antrean memiliki status Waiting, Processing, Completed, Failed, atau Cancelled. | Disepakati |
| FR-OPS-03 | Pengguna melihat estimasi waktu tunggu berdasarkan kondisi antrean aktual. | Disepakati |
| FR-OPS-04 | Pengguna menerima notifikasi ketika pemrosesan selesai. | Disepakati |
| FR-OPS-05 | Pengguna dapat membatalkan pekerjaan saat menunggu atau diproses, dengan refund sesuai hasil pembatalan yang diverifikasi. | Disepakati |

### 4.8 Admin Panel dan tata kelola

Admin Panel termasuk dalam kebutuhan MVP. Modul yang direncanakan:
- Dashboard statistik pengguna, penggunaan, dan pendapatan.
- Manajemen pengguna.
- Pemeriksaan transaksi dan kredit.
- Manajemen paket/harga.
- Konfigurasi reward iklan.
- Konfigurasi layanan/model AI.
- Analitik penggunaan.
- Audit log.
- Pengelolaan tiket bantuan.

Struktur peran:
- **Super Admin:** hak tertinggi dan konfigurasi penuh.
- **Admin:** operasional sesuai izin.
- **Moderator:** moderasi/laporan sesuai izin.

Struktur RBAC dirancang sejak MVP; tidak wajib mengaktifkan banyak akun/peran sejak hari pertama.

Persyaratan keamanan:
- Login Admin menggunakan email/password, 2FA, dan kontrol sesi/perangkat.
- Admin dapat melihat isi teks pengguna hanya jika ada laporan relevan dan pengguna memberikan persetujuan.
- Akses hanya mencakup data yang relevan dengan laporan, memiliki alasan, dan tercatat dalam audit log.
- Persetujuan tidak menjadi izin permanen.
- Moderator tidak otomatis memperoleh akses ke isi teks.

### 4.9 Keluaran dan ekspor

Kedua modul menyediakan hasil di website dan opsi unduh:
- TXT
- DOCX
- PDF

Detector menghasilkan laporan yang memuat ringkasan, kategori, analisis kalimat, penjelasan, dan batasan hasil. Humanizer menyediakan teks asli, revisi, penyorotan perubahan, serta hasil yang dapat disalin/diunduh.

Ekspor tidak menjamin tata letak file sumber dipertahankan secara identik. Struktur dan format kompleks harus mengikuti kemampuan ekstraksi dan generator dokumen yang kelak dipilih.

## 5. Kebutuhan nonfungsional dan prinsip kualitas

### 5.1 Privasi dan retensi

- Teks yang tidak dipilih untuk disimpan dihapus setelah pemrosesan selesai.
- Guest tidak memiliki riwayat permanen.
- Pengguna login memilih penyimpanan riwayat.
- Isi teks tidak boleh dimasukkan ke log teknis secara default; log menggunakan metadata operasional seperlunya.
- Profil gaya yang disimpan dipisahkan dari dokumen sampel sumber.
- Akses Admin ke isi teks hanya berdasarkan laporan dan persetujuan pengguna.
- Penghapusan dari database utama belum membuktikan penghapusan dari cache, backup, log, atau penyedia AI eksternal. Retensi setiap lapisan dan provider harus diaudit sebelum klaim penghapusan menyeluruh dibuat.

### 5.2 Keamanan dan integritas

- Validasi input, file, saldo, dan hak akses dilakukan di backend.
- Pembayaran, pemotongan kredit, refund, bonus harian, dan reward harus idempotent.
- Perubahan saldo dan tindakan Admin tercatat dalam ledger/audit log.
- Sesi Admin dapat dipantau dan dicabut.
- Data dan profil personal tidak dapat diakses akun lain.
- File unggahan diperlakukan sebagai input tidak tepercaya dan dibatasi tipe/ukurannya.

### 5.3 Performa dan skalabilitas

- Prioritas performa adalah keseimbangan kualitas hasil, waktu respons, dan biaya.
- Pemrosesan adaptif: teks pendek sekaligus; teks panjang dapat dipecah dengan strategi konteks.
- Beban ringan dapat diproses langsung; beban berat atau kondisi sibuk dapat menggunakan antrean.
- Retry tidak boleh menghasilkan pekerjaan ganda atau pemotongan kredit ganda.
- Estimasi antrean harus berdasarkan data aktual, bukan angka statis tanpa dasar.
- Target waktu respons, kapasitas concurrency, dan batas antrean belum ditentukan.

### 5.4 Ketersediaan dan pemulihan

Target uptime/SLA belum ditentukan. MVP harus memiliki deteksi gangguan, pencatatan kegagalan, status layanan, dan prosedur pemulihan. Target formal baru ditentukan setelah infrastruktur dan biaya diketahui.

### 5.5 Kualitas hasil

- Detector harus meminimalkan false positive terhadap tulisan manusia dan mengkomunikasikan ketidakpastian.
- Humanizer harus menjaga fakta, makna, angka, nama, dan istilah penting.
- Penjelasan Detector tidak boleh mengarang alasan yang tidak didukung analisis.
- Hasil kedua modul harus diuji terpisah pada Bahasa Indonesia dan Inggris.
- Code-switching perlu skenario uji tersendiri.

## 6. Aturan bisnis utama

1. Detector dan Humanizer memiliki prioritas setara dalam cakupan MVP.
2. Guest hanya dapat menggunakan Detector; Humanizer mensyaratkan login.
3. Pengguna terdaftar dapat menggunakan kedua modul secara mandiri.
4. Humanizer hanya berjalan setelah tindakan pengguna.
5. Skor Detector bukan bukti pasti kepengarangan.
6. Kredit internal berbeda dari token model.
7. Tidak ada pemrosesan jika saldo tidak mencukupi.
8. Kredit pembelian tidak kedaluwarsa; bonus dan reward berlaku 7 hari.
9. Kredit promosi dengan kedaluwarsa terdekat digunakan terlebih dahulu.
10. Kredit dikembalikan hanya berdasarkan kondisi kegagalan/pembatalan yang diverifikasi.
11. Riwayat dan profil gaya disimpan berdasarkan pilihan pengguna.
12. Isi teks hanya dapat diakses Admin melalui laporan yang relevan dan persetujuan pengguna.
13. DOCX Metadata Cleaner dan referral tidak termasuk MVP.

## 7. Risiko utama

| Risiko | Dampak | Arah mitigasi |
|---|---|---|
| False positive Detector | Tulisan manusia salah ditandai AI | Kalibrasi, kategori Uncertain, evaluasi per domain/bahasa, penjelasan batas |
| Klaim deteksi berlebihan | Hasil dianggap bukti pelanggaran | Bahasa UI dan laporan yang tidak absolut |
| Humanizer mengubah fakta/makna | Pengguna menerima revisi yang salah | Diff, evaluasi preservation, peninjauan pengguna |
| Biaya AI melebihi pendapatan kredit/iklan | Kerugian operasional | Unit economics, tarif adaptif, limit, monitoring |
| Reward/bonus disalahgunakan | Kebocoran kredit dan biaya | Identitas akun/sesi, rate limit, idempotensi, deteksi abuse |
| Webhook pembayaran ganda/palsu | Saldo salah | Verifikasi server, signature, idempotency, rekonsiliasi |
| Retensi provider tidak sesuai janji | Risiko privasi | Audit provider, kontrak, konfigurasi retensi |
| Ekstraksi file tidak lengkap | Hasil Detector/Humanizer tidak akurat | Preview ekstraksi dan peringatan |
| Chunking kehilangan konteks | Hasil tidak konsisten | Overlap/konteks, penggabungan, evaluasi dokumen panjang |
| Profil gaya tidak representatif | Hasil Humanizer tidak sesuai | Penilaian kecukupan, persetujuan dan pembaruan profil |

## 8. Hal yang belum diputuskan

Daftar berikut sengaja dibiarkan terbuka. Jangan mengisinya dengan asumsi saat implementasi.

### Produk dan UX
- Daftar final gaya bawaan.
- Copywriting, layout, dan desain UI.
- Angka minimum/maksimum karakter per fitur.
- Batas minimum/maksimum ukuran file.
- Elemen DOCX/PDF yang dipertahankan dan dukungan OCR final.
- Batas jumlah profil gaya.
- Detail halaman dan pengaturan dashboard.

### Kredit, iklan, dan pembayaran
- Zona waktu bonus harian pukul 03.00.
- Batas reward iklan guest/login dan jumlah kredit per iklan.
- Masa/periode perhitungan bonus harian secara teknis.
- Rumus kredit per fitur, biaya dasar, biaya variabel, dan pembulatan.
- Paket, harga, jumlah minimum/maksimum pembelian khusus.
- Payment gateway dan detail refund pembayaran.
- Produk iklan Google yang tepat dan kelayakan reward berdasarkan kebijakan resmi.
- Apakah guest checkout meminta pembuatan akun sebelum pembayaran atau setelah pembayaran, dengan pemulihan transaksi yang aman.

### Teknologi dan operasional
- Framework frontend/backend, database, storage, queue, cache, hosting, dan deployment.
- Provider/model AI untuk Detector dan Humanizer.
- OCR dan library ekstraksi/generasi dokumen.
- Provider email dan mekanisme notifikasi real-time.
- Target uptime, response time, throughput, concurrency, dan kapasitas antrean.
- Implementasi 2FA dan aturan perangkat/sesi.
- Matriks izin RBAC yang rinci.
- SLA dukungan tiket dan retensi tiket.
- Detail consent, retensi sampel gaya, cache, backup, dan kebijakan provider AI.
- Kebutuhan aksesibilitas, observability, backup/restore, dan disaster recovery.

## 9. Kriteria kesiapan menuju System Design

Tahap berikutnya dapat dimulai setelah dokumen ini ditinjau pemilik proyek. System Design harus menghasilkan:
1. Batas modul dan tanggung jawab masing-masing.
2. User flow Detector, Humanizer, akun, kredit, pembayaran, dan dukungan.
3. Diagram sistem serta jalur sukses/gagal.
4. Data flow dan klasifikasi data sensitif.
5. Model data konseptual untuk akun, profil gaya, riwayat, kredit ledger, pembayaran, antrean, tiket, dan audit.
6. Kontrak API dan event antarmodul tingkat konseptual.
7. Evaluasi teknologi berdasarkan kebutuhan dan trade-off.
8. Acceptance criteria yang dapat diuji.

**Tidak ada implementasi aplikasi yang disahkan oleh dokumen ini.** Status implementasi seluruh fitur tetap belum dimulai dan belum diverifikasi.
