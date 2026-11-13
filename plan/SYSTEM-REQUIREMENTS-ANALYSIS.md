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
| FR-HUM-16 | Pengguna dapat menambah, mengedit, atau menghapus setiap entri sampel profil gaya secara individual tanpa harus menghapus atau mengirim ulang entri sampel lainnya. | Disepakati |
| FR-HUM-17 | Setiap kali entri sampel pada profil gaya yang sudah terbentuk ditambahkan, diedit, atau dihapus, sistem mengevaluasi ulang seluruh kumpulan sampel terkini. Jika kumpulan tersebut tidak lagi memadai, sistem memberi peringatan dan meminta pengguna menambahkan sampel sebelum profil digunakan atau diperbarui kembali. | Disepakati; kriteria kecukupan perlu dirancang |
| FR-HUM-18 | Jika hasil evaluasi menunjukkan kumpulan sampel profil tidak lagi memadai, profil tetap ditampilkan dalam daftar pengelolaan, tetapi tidak dapat dipilih untuk pemrosesan Humanizer sampai pengguna menambahkan sampel dan profil kembali memenuhi kriteria. | Disepakati; status UI dan transisi pemulihan perlu dirancang |
| FR-HUM-19 | Jika evaluasi ulang profil gagal akibat gangguan teknis, sistem mencatat status evaluasi gagal dan meminta pengguna mencoba kembali. Kegagalan teknis tidak boleh dianggap sebagai bukti bahwa sampel tidak memadai atau otomatis mengubah profil menjadi tidak valid. | Disepakati; retry, batas percobaan, dan perilaku profil selama evaluasi gagal perlu dirancang |
| FR-HUM-20 | Selama evaluasi ulang belum menghasilkan keputusan bahwa sampel tidak memadai, profil versi sebelumnya tetap dapat digunakan untuk pemrosesan Humanizer. Aturan ini juga berlaku ketika evaluasi gagal secara teknis. Jika evaluasi berhasil dan menyatakan sampel tidak memadai, profil tidak lagi dapat dipilih. | Disepakati; pengelolaan versi profil dan transisi status perlu dirancang |
| FR-HUM-21 | Jika evaluasi berhasil dan menyatakan kumpulan sampel memadai, sistem membangun versi profil baru secara terpisah tanpa mengganti versi aktif terlebih dahulu. Versi baru hanya diaktifkan setelah proses pembangunan berhasil. Jika pembangunan gagal, versi profil aktif sebelumnya tetap digunakan dan sistem mencatat kegagalan pembaruan. | Disepakati; mekanisme teknis pembangunan dan aktivasi versi perlu dirancang |
| FR-HUM-22 | Setelah versi profil baru berhasil diaktifkan, versi sebelumnya disimpan selama 30 hari dan dapat dipulihkan oleh pengguna dalam periode tersebut. Pemulihan mengaktifkan kembali artefak profil versi lama tanpa mengembalikan atau mengubah kumpulan sampel sumber saat ini. Versi yang melewati masa retensi dapat dihapus otomatis. | Disepakati; mekanisme pemulihan, retensi, dan penghapusan perlu dirancang |
| FR-HUM-23 | Setiap pergantian versi profil yang berhasil, termasuk pemulihan versi historis, menjadikan versi yang sebelumnya aktif sebagai versi historis dengan masa retensi 30 hari sejak pergantian berhasil. Masa retensi dihitung per versi yang dinonaktifkan; masa retensi versi historis lain tidak ikut diperpanjang. | Disepakati; pencatatan waktu pergantian dan retensi per versi perlu dirancang |
| FR-HUM-24 | Setiap profil gaya dapat memiliki maksimal 5 versi historis yang belum kedaluwarsa. Versi yang masih dalam masa retensi tidak boleh dihapus otomatis untuk memberi ruang bagi versi baru. Jika kapasitas historis penuh, sistem menunda aktivasi versi baru atau pemulihan versi historis sampai tersedia slot, sementara versi aktif tetap digunakan. | Disepakati; mekanisme antrean/percobaan ulang aktivasi dan pemulihan perlu dirancang |
| FR-HUM-25 | Jika permintaan pemulihan versi historis dan pembaruan profil sama-sama tertunda karena kapasitas versi historis penuh, permintaan pemulihan mendapat prioritas lebih tinggi. Permintaan pembaruan menunggu sampai kapasitas tersedia setelah permintaan berprioritas lebih tinggi ditangani. Pengguna dapat membatalkan permintaan pergantian versi selama permintaan tersebut masih berstatus tertunda dan belum mulai dieksekusi. | Disepakati; aturan urutan, status permintaan, dan retry perlu dirancang |
| FR-HUM-26 | Setiap profil hanya boleh memiliki satu permintaan pemulihan versi yang aktif pada satu waktu, termasuk permintaan yang masih tertunda atau sedang dieksekusi. Permintaan pemulihan tambahan untuk profil yang sama ditolak sementara dan pengguna diminta menyelesaikan atau membatalkan permintaan sebelumnya. Jika permintaan sebelumnya masih tertunda, pengguna dapat membatalkannya sebelum mengajukan pemulihan ke versi lain. Jika pemulihan gagal, versi aktif tidak berubah dan pengguna dapat mengajukan percobaan baru setelah permintaan gagal diselesaikan. | Disepakati; definisi status final dan penanganan konkurensi perlu dirancang |
| FR-HUM-27 | Saat pemulihan versi profil mulai dieksekusi, proses pembaruan atau pembentukan ulang profil ditahan sampai pemulihan selesai. Jika pemulihan berhasil maupun gagal, pembaruan yang tertahan dilanjutkan menggunakan kumpulan sampel terbaru dan wajib menjalani evaluasi ulang; hasil evaluasi lama tidak boleh digunakan untuk membangun atau mengaktifkan versi baru. Kegagalan pemulihan tidak mengubah versi aktif dan tidak menghalangi pembaruan profil. | Disepakati |
| FR-HUM-28 | Pengguna tetap dapat menghapus sampel gaya ketika pembaruan profil tertahan oleh proses pemulihan. Setelah pemulihan selesai dan pembaruan dilanjutkan, sistem wajib menggunakan kumpulan sampel yang tersisa setelah penghapusan dan mengevaluasi ulang seluruh kumpulan tersebut. Sampel yang telah dihapus tidak boleh digunakan dalam pembentukan versi profil baru. | Disepakati |
| FR-HUM-29 | Pengguna tetap dapat menambah dan mengedit sampel gaya ketika pembaruan profil tertahan oleh proses pemulihan. Setelah pemulihan selesai, pembaruan dilanjutkan dengan mengambil kondisi terbaru seluruh sampel, termasuk penambahan, pengeditan, dan penghapusan yang terjadi selama penahanan. Sistem mengevaluasi ulang kumpulan terbaru tersebut satu kali untuk proses pembaruan yang dilanjutkan, bukan menjalankan evaluasi terpisah untuk setiap perubahan selama penahanan. | Disepakati |
| FR-HUM-30 | Evaluasi sampel gaya harus berjalan terhadap snapshot kumpulan sampel yang tidak berubah selama satu proses evaluasi. Jika pengguna menambah, mengedit, atau menghapus sampel ketika evaluasi berlangsung, hasil evaluasi dari snapshot lama tidak boleh digunakan untuk membangun atau mengaktifkan versi profil. Sistem harus mengambil snapshot terbaru dan mengevaluasinya kembali sebelum melanjutkan pembentukan versi profil. | Disepakati; mekanisme konsistensi snapshot dan pembatalan pekerjaan usang perlu dirancang |
| FR-HUM-31 | Sistem harus membatasi pengulangan evaluasi otomatis ketika perubahan sampel berulang membuat snapshot evaluasi menjadi usang. Jika batas pengulangan otomatis tercapai, sistem menghentikan percobaan otomatis, mempertahankan seluruh perubahan sampel pengguna, dan menandai pembaruan profil sebagai tertunda karena perubahan berulang. Pengguna harus memulai ulang evaluasi secara manual. Nilai batas pengulangan ditetapkan pada tahap desain teknis berdasarkan estimasi biaya dan durasi evaluasi, bukan ditentukan pada tahap kebutuhan ini. | Disepakati; nilai batas dan perilaku rinci restart manual perlu dirancang |
| FR-HUM-32 | Ketika pengguna memulai ulang evaluasi profil secara manual setelah batas pengulangan otomatis tercapai, sistem harus memulai seluruh proses evaluasi dari awal menggunakan snapshot terbaru kumpulan sampel. Hasil antara dari evaluasi sebelumnya tidak boleh digunakan kembali sebagai dasar untuk melanjutkan proses baru. | Disepakati |
| FR-HUM-33 | Untuk evaluasi yang dihentikan atau menjadi usang, sistem hanya menyimpan metadata riwayat evaluasi yang diperlukan untuk audit, seperti waktu, status, alasan penghentian, dan identitas versi snapshot. Metadata tersebut hanya tersedia secara internal untuk audit sistem dan tidak ditampilkan sebagai riwayat kepada pengguna. Metadata dihapus otomatis setelah 30 hari, kecuali masih diperlukan untuk investigasi insiden atau kewajiban keamanan. Hasil analisis antara yang tidak berlaku tidak disimpan sebagai hasil yang dapat digunakan kembali dan tidak boleh dipakai dalam evaluasi atau pembentukan profil berikutnya. | Disepakati; definisi kebutuhan retensi khusus untuk insiden dan keamanan perlu dirancang |
| FR-HUM-34 | Akses metadata audit evaluasi dibatasi berdasarkan peran: Super Admin dapat mengaksesnya sesuai kebutuhan audit; Admin hanya dapat mengaksesnya secara terbatas untuk operasional atau investigasi yang ditugaskan; Moderator tidak memiliki akses. Setiap akses wajib mencatat identitas petugas, alasan, waktu, dan tindakan. Izin untuk mengakses metadata audit harus terpisah dari izin untuk mengakses teks asli pengguna. | Disepakati; matriks izin rinci dan mekanisme penugasan investigasi perlu dirancang |
| FR-HUM-35 | Admin yang belum memiliki penugasan investigasi wajib mengajukan permintaan akses metadata audit dengan alasan dan nomor tiket investigasi. Super Admin menyetujui atau menolak permintaan tersebut. Masa berlaku maksimal 24 jam dihitung sejak waktu Super Admin menyetujui permintaan, bukan sejak akses pertama digunakan. Persetujuan berlaku maksimal 24 jam, tidak diperpanjang otomatis, dan dapat dicabut Super Admin kapan saja. Setelah kedaluwarsa atau dicabut, Admin harus mengajukan permintaan baru jika akses masih diperlukan. Akses hanya berlaku untuk cakupan investigasi yang disetujui. | Disepakati |
| FR-HUM-36 | Untuk persetujuan akses investigasi metadata audit, Admin menerima notifikasi saat akses disetujui yang mencakup status aktif, cakupan izin, dan waktu kedaluwarsa. Admin menerima pengingat satu jam sebelum kedaluwarsa. Saat akses kedaluwarsa, sistem mencabut akses otomatis dan memberi tahu Admin. Jika akses dicabut lebih awal, Admin segera diberi tahu beserta alasan pencabutan. Super Admin menerima catatan bahwa akses telah kedaluwarsa atau dicabut. Notifikasi dikirim melalui notifikasi dalam aplikasi dan email. Jika pengiriman melalui salah satu kanal gagal, sistem tetap mencoba kanal lainnya. Kegagalan pengiriman notifikasi tidak boleh menghambat atau menunda pencabutan akses otomatis. Jika kedua kanal notifikasi gagal, status kedaluwarsa atau pencabutan akses tetap berlaku. Kegagalan kedua kanal dicatat dalam audit log dan ditampilkan kepada Super Admin sebagai indikator kegagalan notifikasi. Sistem dapat melakukan retry terbatas, tetapi kegagalan pengiriman maupun retry tidak boleh mengaktifkan kembali izin atau menunda pencabutan. Untuk setiap kanal secara independen, sistem melakukan satu percobaan pengiriman awal dan maksimal tiga percobaan ulang otomatis jika kanal tersebut gagal, dengan jeda lima menit antarpercobaan ulang pada kanal yang sama. Dengan demikian, setiap kanal memiliki maksimal empat percobaan total. Jika satu kanal berhasil, kanal tersebut tidak dikirim ulang; retry hanya dilakukan pada kanal yang gagal. Jika seluruh percobaan pada suatu kanal gagal, status pengiriman kanal tersebut menjadi `Delivery Failed`. Super Admin dapat melihat detail kegagalan dan memicu pengiriman ulang secara manual. Pengiriman ulang manual hanya mengirim notifikasi dan tidak mengubah status akses. Notifikasi tidak memperpanjang masa akses. | Disepakati |
| FR-HUM-37 | Dalam satu proses Humanizer, pengguna dapat mengunggah maksimal satu dokumen TXT, DOCX, atau PDF, kemudian meninjau dan mengedit teks hasil ekstraksi serta menambahkan teks secara manual sebelum pemrosesan. Seluruh konten yang telah disusun dalam editor diperlakukan sebagai satu input Humanizer. Pengguna tidak dapat menggabungkan beberapa dokumen unggahan dalam satu proses. Dokumen sumber tidak boleh diubah atau ditimpa. | Disepakati; perilaku editor, penanganan struktur dokumen, dan format ekspor hasil gabungan perlu dirancang |
| FR-HUM-38 | Jika pengguna ingin mengganti dokumen yang sudah diunggah sebelum pemrosesan, sistem harus meminta konfirmasi terlebih dahulu. Jika pengguna membatalkan, teks hasil ekstraksi dan seluruh tambahan/editan manual yang ada di editor tetap dipertahankan. Jika pengguna mengonfirmasi, konten editor dari dokumen sebelumnya beserta tambahan/editan manual dihapus dan diganti dengan hasil ekstraksi dokumen baru. Dokumen sumber lama dan baru tidak boleh diubah atau ditimpa. | Disepakati; detail interaksi konfirmasi dan penanganan kegagalan ekstraksi dokumen pengganti perlu dirancang |
| FR-HUM-39 | Sebelum pemrosesan Humanizer, pengguna dapat mengedit teks dalam editor secara bebas, termasuk mengubah, menambah, dan menghapus bagian mana pun dari teks hasil ekstraksi maupun teks yang ditambahkan secara manual. Seluruh isi editor pada saat pengguna menjalankan Humanizer menjadi input yang diproses. | Disepakati; batas karakter dan validasi input mengikuti ketentuan umum Humanizer yang masih perlu ditetapkan |
| FR-HUM-40 | Setelah pemrosesan Humanizer selesai, pengguna dapat melihat hasil di website, menyalin teks, dan mengunduh hasil. Jika proses menggunakan dokumen unggahan, format file hasil unduhan mengikuti format dokumen yang diunggah (TXT, DOCX, atau PDF). Dokumen sumber tetap tidak diubah atau ditimpa. Kesamaan tata letak dan struktur hasil dengan dokumen sumber tidak dijamin secara identik. | Disepakati; format unduhan ketika input hanya berupa teks manual tanpa dokumen unggahan perlu ditentukan |
| FR-HUM-41 | Jika pengguna memulai proses Humanizer hanya dengan mengetik atau menempelkan teks tanpa mengunggah dokumen, pengguna dapat memilih format unduhan hasil berupa TXT, DOCX, atau PDF. Hasil tetap ditampilkan di website dan dapat disalin. | Disepakati; implementasi generator dan validasi format mengikuti desain teknis |
| FR-HUM-42 | Hasil pemrosesan Humanizer tidak disimpan otomatis ke riwayat akun. Pengguna dapat memilih secara eksplisit apakah hasil tertentu ingin disimpan. | Disepakati; mekanisme persetujuan, pengelolaan, dan retensi hasil tersimpan perlu dirancang |
| FR-HUM-43 | Jika pengguna secara eksplisit memilih menyimpan hasil Humanizer, sistem menyimpan teks asli yang diproses dan hasil revisinya secara lengkap sebagai satu entri riwayat. Jika pengguna tidak memilih menyimpan, kedua teks tersebut tidak dimasukkan ke riwayat akun. | Disepakati; retensi, penghapusan, keamanan penyimpanan, dan batas ukuran riwayat perlu dirancang |
| FR-HUM-44 | Pengguna dapat menghapus entri riwayat Humanizer yang telah disimpan. Setelah pengguna memilih hapus dan mengonfirmasi tindakan tersebut, entri dihapus permanen dari riwayat yang dapat diakses melalui aplikasi. | Disepakati; cakupan penghapusan pada backup, log, dan penyimpanan turunan perlu ditetapkan dalam kebijakan retensi dan desain teknis |
| FR-HUM-45 | Kapasitas penyimpanan riwayat Humanizer dibatasi berdasarkan total ukuran data yang disimpan per akun, bukan berdasarkan jumlah entri. Nilai kuota, satuan penghitungan, dan perilaku sistem ketika kuota tercapai ditentukan pada tahap desain teknis. | Disepakati; kuota dan penanganan kapasitas penuh perlu dirancang |
| FR-HUM-46 | Jika kapasitas penyimpanan riwayat Humanizer penuh, sistem tidak boleh menghapus entri lama secara otomatis. Pengguna harus dapat memilih sendiri entri riwayat lama yang ingin dihapus untuk menyediakan kapasitas sebelum menyimpan hasil baru. | Disepakati; alur pemilihan entri dan penanganan jika kapasitas tetap tidak mencukupi perlu dirancang |
| FR-HUM-47 | Sistem memberi peringatan kepada pengguna melalui notifikasi dalam aplikasi ketika penggunaan kapasitas penyimpanan riwayat Humanizer mencapai 90% dari kuota akun. Peringatan disampaikan sebelum kapasitas penuh. Peringatan berikutnya hanya dipicu kembali jika penggunaan sebelumnya telah turun di bawah 90% lalu mencapai atau melewati ambang 90% lagi. Peringatan kapasitas ini tidak dikirim melalui email. | Disepakati; detail implementasi pemantauan ambang perlu dirancang |
| FR-HUM-48 | Sistem menampilkan indikator penggunaan kapasitas penyimpanan riwayat Humanizer pada halaman riwayat Humanizer, bukan pada dashboard utama. Indikator menampilkan pemakaian terhadap kuota akun. | Disepakati; format visual dan satuan ukuran ditentukan pada desain UI dan teknis |
| FR-HUM-49 | Ketika penggunaan kapasitas riwayat Humanizer mencapai 90% dari kuota akun, sistem menampilkan peringatan dalam bentuk banner pada bagian atas halaman riwayat Humanizer. Banner tidak menghalangi interaksi pengguna dengan halaman. | Disepakati; teks, gaya visual, dan perilaku banner setelah ditampilkan ditentukan pada desain UI |
| FR-HUM-50 | Pengguna dapat menutup banner peringatan kapasitas riwayat Humanizer secara manual. Jika kapasitas akun masih mencapai atau melebihi 90%, banner ditampilkan kembali ketika pengguna membuka kembali halaman riwayat Humanizer. | Disepakati; cakupan status penutupan selama halaman tetap terbuka perlu dirancang pada desain UI/teknis |
| FR-HUM-51 | Jika pengguna memilih menyimpan hasil Humanizer ketika kapasitas riwayat akun penuh, sistem tetap menampilkan hasil pemrosesan dan menyediakan fungsi salin serta unduh. Sistem tidak menyimpan hasil tersebut ke riwayat selama kapasitas belum mencukupi. Setelah pengguna menghapus entri lama sehingga kapasitas mencukupi, pengguna dapat mencoba menyimpan kembali hasil yang sama dari tampilan hasil yang masih tersedia tanpa menjalankan pemrosesan Humanizer ulang. Kondisi kuota penuh tidak boleh menghalangi akses ke hasil pemrosesan yang sudah selesai. | Disepakati; masa ketersediaan tampilan hasil dan perilaku setelah sesi berakhir perlu dirancang |
| FR-HUM-52 | Hasil Humanizer yang belum berhasil disimpan karena kuota riwayat penuh hanya tersedia selama tampilan hasil pada sesi aktif masih terbuka. Jika pengguna menutup halaman atau browser, hasil yang belum tersimpan hilang dan tidak dapat dipulihkan melalui aplikasi. Pengguna harus menjalankan pemrosesan Humanizer kembali untuk memperoleh hasil tersebut. Sistem tidak menyimpan hasil tersebut secara sementara di server atau browser untuk pemulihan setelah sesi berakhir. | Disepakati; definisi teknis batas sesi aktif dan perilaku refresh halaman perlu ditetapkan pada desain teknis |
| FR-HUM-53 | Jika pengguna melakukan tindakan yang meninggalkan halaman hasil Humanizer ketika terdapat hasil yang belum disimpan, sistem meminta konfirmasi sebelum navigasi dilanjutkan. Cakupan meliputi navigasi internal, tombol Back, refresh, serta navigasi/penutupan halaman lain sejauh didukung dan dapat ditangani oleh browser. Jika pengguna membatalkan, halaman dan hasil tetap dipertahankan. Jika pengguna mengonfirmasi, navigasi dilanjutkan dan hasil yang belum tersimpan hilang sesuai FR-HUM-52. | Disepakati; cakupan teknis bergantung pada kemampuan browser dan mekanisme navigasi yang digunakan |
| FR-HUM-54 | Setelah pengguna mengonfirmasi tindakan navigasi yang akan meninggalkan halaman hasil Humanizer yang belum disimpan, sistem tidak menampilkan peringatan/konfirmasi kedua. Konfirmasi pertama dianggap cukup dan navigasi dilanjutkan. | Disepakati |
| FR-HUM-55 | Ketika pengguna mencoba berpindah melalui navigasi internal dari halaman hasil Humanizer sementara hasil belum disimpan, sistem menampilkan dialog konfirmasi modal di tengah layar dengan pilihan **Tetap di halaman** dan **Lanjutkan**. Memilih Tetap di halaman membatalkan navigasi dan mempertahankan hasil; memilih Lanjutkan meneruskan navigasi dan menghilangkan hasil yang belum disimpan. Dialog hanya ditampilkan ketika hasil benar-benar belum disimpan. Jika hasil sudah tersimpan, navigasi internal berlangsung tanpa konfirmasi. | Disepakati |
| FR-HUM-56 | Jika pengguna memilih **Tetap di halaman** pada dialog konfirmasi navigasi internal, sistem membatalkan navigasi dan mempertahankan seluruh kondisi halaman hasil Humanizer tanpa perubahan, termasuk posisi scroll, teks, hasil pemrosesan, dan kondisi editor yang sedang aktif. | Disepakati |
| FR-HUM-57 | Jika pengguna memilih **Lanjutkan** pada dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan, sistem langsung meneruskan navigasi tanpa jeda tambahan atau konfirmasi kedua. Hasil yang belum disimpan hilang sesuai ketentuan retensi hasil. | Disepakati |
| FR-HUM-58 | Jika pengguna menekan tombol **X** pada dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan, sistem menutup dialog dan membatalkan navigasi. Perilakunya sama dengan memilih **Tetap di halaman**: hasil dan seluruh kondisi halaman tetap dipertahankan. | Disepakati |
| FR-HUM-59 | Ketika dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan terbuka, menekan tombol **Escape** menutup dialog dan membatalkan navigasi. Perilakunya sama dengan tombol **X** dan pilihan **Tetap di halaman**: hasil dan seluruh kondisi halaman tetap dipertahankan. | Disepakati |
| FR-HUM-60 | Ketika dialog konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan terbuka, klik pada area backdrop di luar modal tidak menutup dialog dan tidak mengubah status navigasi. Pengguna harus memilih tombol **X**, menekan **Escape**, atau memilih **Tetap di halaman** untuk membatalkan navigasi. | Disepakati |
| FR-HUM-61 | Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan terbuka, fokus keyboard harus dikunci di dalam modal. Tombol Tab dan Shift+Tab hanya berpindah di antara elemen interaktif modal dan berputar dari elemen terakhir ke pertama atau sebaliknya. Elemen di belakang modal tidak menerima fokus keyboard selama modal aktif. | Disepakati |
| FR-HUM-62 | Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan dibuka, fokus keyboard awal diarahkan ke tombol **Tetap di halaman**. | Disepakati |
| FR-HUM-63 | Setelah modal konfirmasi navigasi internal ditutup karena pengguna membatalkan navigasi, sistem mengembalikan fokus keyboard ke elemen yang sebelumnya memicu modal, misalnya tombol Dashboard. Jika elemen pemicu tidak lagi tersedia, sistem menggunakan mekanisme fallback fokus yang aksesibel. | Disepakati; perilaku fallback dirinci pada desain teknis |
| FR-HUM-64 | Modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan harus mendukung screen reader. Saat modal dibuka, teknologi bantu dapat mengenali modal sebagai dialog serta mengumumkan judul dan isi pesan peringatannya. | Disepakati; detail semantik dan kompatibilitas ditentukan pada desain teknis dan pengujian aksesibilitas |
| FR-HUM-65 | Modal konfirmasi navigasi internal ketika hasil Humanizer belum disimpan harus menggunakan semantik alert dialog, bukan dialog biasa. Screen reader mengumumkan dialog sebagai peringatan karena melanjutkan navigasi dapat menghilangkan hasil yang belum disimpan. | Disepakati; kompatibilitas dan perilaku pengumuman diuji pada pengujian aksesibilitas |
| FR-HUM-66 | Ketika modal konfirmasi navigasi internal dari halaman hasil Humanizer yang belum disimpan dibuka, screen reader harus secara otomatis mengumumkan judul dan seluruh pesan peringatan tanpa mengharuskan pengguna berpindah fokus untuk membaca pesan tersebut. Pengumuman harus dikoordinasikan dengan fokus awal pada tombol Tetap di halaman agar tidak terjadi pengumuman ganda, terpotong, atau saling bertabrakan. | Disepakati; perilaku diuji dengan kombinasi screen reader dan browser pada pengujian aksesibilitas |
| FR-HUM-67 | Judul dan seluruh pesan peringatan pada modal konfirmasi navigasi Humanizer diumumkan secara otomatis satu kali ketika modal dibuka. Sistem tidak mengumumkan ulang pesan secara otomatis setiap kali fokus berpindah ke bagian isi modal. Pengguna screen reader tetap dapat membaca ulang isi pesan melalui navigasi screen reader secara manual. | Disepakati; pengujian memastikan pengumuman awal tidak terduplikasi dan isi tetap dapat dinavigasi secara manual |
| FR-HUM-68 | Satu pekerjaan AI Humanizer menerima maksimal 50.000 karakter untuk keseluruhan teks yang akan diproses, termasuk teks hasil ekstraksi dokumen dan tambahan/editan manual pada editor. Batas berlaku untuk keseluruhan pekerjaan, bukan per chunk. Backend memvalidasi batas sebelum pemrosesan dan menolak input yang melebihinya dengan meminta pengguna mengurangi teks. Batas token internal tetap diterapkan sesuai provider/model yang digunakan; chunking tidak boleh melewati batas karakter keseluruhan. | Disepakati; kelayakan teknis, tokenizer, dan strategi chunking divalidasi setelah provider/model dipilih |
| FR-HUM-69 | Satu pekerjaan AI Humanizer mensyaratkan minimal 100 karakter pada keseluruhan isi editor sebelum pemrosesan. Penghitungan mencakup spasi dan baris baru. Jika isi editor kurang dari 100 karakter, tombol pemrosesan dinonaktifkan dan antarmuka menampilkan jumlah karakter saat ini serta jumlah karakter yang masih dibutuhkan. Batas minimum ini berlaku untuk seluruh gaya penulisan pada MVP dan terpisah dari batas minimum sampel profil gaya. | Disepakati |
| FR-HUM-70 | Setiap entri sampel tulisan untuk membentuk atau memperbarui profil gaya personal harus berisi minimal 500 karakter, termasuk spasi dan baris baru. Sampel di bawah batas tetap dipertahankan sebagai entri, tetapi belum memenuhi syarat minimum; sistem menampilkan jumlah karakter saat ini dan jumlah karakter yang masih dibutuhkan. Batas berlaku per entri sampel dan terpisah dari penilaian adaptif kecukupan total kumpulan sampel. | Disepakati |
| FR-HUM-71 | Setiap akun dapat memiliki maksimal 6 profil gaya personal, terdiri dari 1 profil utama dan maksimal 5 profil tambahan. Jika batas tercapai, pengguna harus menghapus salah satu profil tambahan sebelum membuat profil baru. Profil utama tidak dapat dihapus sebelum pengguna menetapkan profil utama pengganti. | Disepakati |
| FR-HUM-72 | Profil gaya personal bersifat privat dan hanya dapat dilihat, dikelola, serta digunakan oleh pemilik akun. Pengguna lain tidak dapat menggunakan profil tersebut. Fitur berbagi profil antar akun tidak termasuk MVP. | Disepakati |
| FR-HUM-73 | Pengguna dapat menghapus profil gaya tambahan kapan saja. Profil utama hanya dapat dihapus setelah profil utama pengganti ditetapkan. Sistem wajib meminta konfirmasi sebelum penghapusan. Setelah dikonfirmasi, profil beserta seluruh versi historisnya dihapus secara permanen dari sistem yang dapat diakses pengguna. Riwayat pekerjaan Humanizer yang pernah menggunakan profil tersebut tidak ikut dihapus. Mekanisme dan jangka waktu penghapusan dari backup ditetapkan pada desain teknis dan kebijakan retensi. | Disepakati; penghapusan dari backup belum ditentukan |
| FR-HUM-74 | Pengguna dapat menetapkan profil gaya personal mana pun yang tersedia sebagai profil utama kapan saja. Profil utama sebelumnya otomatis menjadi profil tambahan. Pergantian berlaku untuk pekerjaan Humanizer berikutnya dan tidak mengubah pekerjaan yang sedang berjalan. Pergantian status profil utama tidak membuat versi historis baru karena isi profil tidak berubah. | Disepakati |
| FR-HUM-75 | Pengguna dapat menghapus seluruh entri sampel pada satu profil gaya dalam satu tindakan. Sistem menampilkan jumlah sampel yang akan dihapus dan meminta konfirmasi. Setelah dikonfirmasi, seluruh sampel dihapus. Profil tanpa sampel tetap terlihat dalam daftar pengelolaan tetapi tidak dapat digunakan untuk pemrosesan Humanizer; pengguna dapat membangunnya kembali dengan menambahkan sampel baru. Versi historis profil yang telah terbentuk tetap mengikuti kebijakan retensi dan pemulihan yang berlaku. | Disepakati |
| FR-HUM-76 | Pengguna dapat mengunduh setiap entri sampel tulisan secara individual dan mengunduh seluruh sampel dalam satu profil sekaligus sebagai satu file ZIP. Format unduhan individual mengikuti format sumber asli jika tersedia. Sampel yang berasal dari teks yang ditempel langsung dapat diunduh sebagai TXT. Pengunduhan tidak mengubah atau menghapus sampel yang tersimpan. | Disepakati |
| FR-HUM-77 | Ukuran maksimum setiap file sampel adalah 10 MB, berlaku per file dan bukan akumulasi seluruh sampel. Format yang diterima adalah TXT, DOCX, dan PDF. File yang melebihi batas ditolak sebelum ekstraksi dan sistem menampilkan pesan yang menyebutkan ukuran maksimum yang diizinkan. Batas ini berlaku untuk MVP dan dapat dievaluasi kembali setelah pengujian teknis. | Disepakati; validasi teknis diperlukan |
| FR-HUM-78 | Pengguna dapat mengunggah maksimal 5 file sampel dalam satu tindakan unggah. Batas 10 MB berlaku untuk setiap file. Pengguna dapat melakukan unggahan berikutnya untuk menambahkan file lain. Sistem memvalidasi format dan ukuran setiap file secara individual; file yang tidak valid ditolak, sedangkan file lain yang valid tetap dapat diproses. Batas batch ini tidak menetapkan jumlah total sampel yang dapat tersimpan pada profil. | Disepakati |
| FR-HUM-79 | Setiap file sampel diekstrak secara independen. File yang berhasil diekstrak dimasukkan ke daftar sampel profil. File yang gagal diekstrak tidak dimasukkan sebagai sampel; sistem menampilkan nama file dan alasan kegagalan. Pengguna dapat mengunggah ulang file yang gagal atau menggunakan file lain. Sampel yang berhasil diekstrak tetap tersimpan meskipun file lain dalam batch yang sama gagal. | Disepakati |
| FR-HUM-80 | Setelah ekstraksi file sampel berhasil, sistem menampilkan pratinjau teks hasil ekstraksi. Pengguna dapat mengedit teks sebelum menyimpannya atau membatalkan sampel jika hasil ekstraksi tidak sesuai. Sampel hanya disimpan setelah pengguna menekan tombol konfirmasi. Batas minimum 500 karakter diperiksa terhadap teks final setelah diedit. Jika hasil ekstraksi terdeteksi tidak lengkap, sistem memberikan peringatan sebelum penyimpanan. | Disepakati |
| FR-HUM-81 | Setiap profil gaya dapat menyimpan maksimal 20 entri sampel, baik dari teks tempel maupun unggahan file. Setiap entri tetap harus memenuhi minimum 500 karakter. Jika profil telah mencapai 20 entri, pengguna harus menghapus sampel lama sebelum menambahkan sampel baru. Pengguna tetap dapat mengedit sampel yang sudah ada; pengeditan dan penghapusan mengikuti mekanisme evaluasi ulang yang berlaku. | Disepakati; batas perlu divalidasi melalui pengujian profiling |
| FR-HUM-82 | Daftar sampel secara default diurutkan berdasarkan waktu penambahan terbaru, dengan sampel terbaru di posisi paling atas. Pengguna dapat mengurutkan berdasarkan tanggal ditambahkan atau nama file. Sampel teks tempel tanpa nama file menggunakan label yang dibuat sistem. Pengurutan hanya memengaruhi tampilan, tidak mengubah urutan evaluasi atau isi profil, dan tidak memicu evaluasi ulang. | Disepakati |
| FR-HUM-83 | Sampel dari teks tempel mendapat nama default otomatis, misalnya "Sampel 1", "Sampel 2", dan seterusnya. Sampel dari file menggunakan nama file tanpa ekstensi sebagai nama default. Pengguna dapat mengganti nama sampel. Nama harus unik dalam satu profil dan maksimal 60 karakter. Perubahan nama tidak mengubah isi teks, tidak memicu evaluasi ulang, dan tidak memengaruhi hasil profiling. | Disepakati |
| FR-HUM-84 | Nama sampel harus unik dalam satu profil dengan validasi yang tidak membedakan huruf besar dan kecil. Spasi di awal dan akhir nama dipangkas otomatis sebelum validasi. Jika nama duplikat, penyimpanan atau perubahan nama ditolak dan sistem menampilkan pesan bahwa nama sudah digunakan. Nama yang sama boleh digunakan pada profil berbeda. | Disepakati |
| FR-HUM-85 | Nama sampel boleh menggunakan huruf, angka, spasi, tanda baca, dan karakter Unicode, termasuk emoji. Nama tidak boleh kosong setelah spasi di awal dan akhir dipangkas serta tidak boleh mengandung karakter kontrol atau karakter yang tidak terlihat. Batas maksimal 60 karakter tetap berlaku. Nama disimpan sebagai teks biasa dan di-escape secara aman saat ditampilkan untuk mencegah XSS. | Disepakati |
| FR-HUM-86 | Sampel yang dihapus dipindahkan ke area Recently Deleted dan dapat dipulihkan selama 30 hari sejak dihapus. Setelah 30 hari, sampel dihapus permanen. Sampel yang dipulihkan kembali ke profil asal. Jika profil telah mencapai batas 20 sampel, pemulihan ditolak sampai tersedia slot. Pemulihan memicu evaluasi ulang profil. Penghapusan seluruh sampel juga mengikuti kebijakan pemulihan 30 hari ini. | Disepakati |
| FR-HUM-87 | Sebelum memulihkan sampel dari Recently Deleted, sistem memeriksa konflik nama dengan sampel aktif pada profil asal. Jika nama konflik, pemulihan ditolak, sampel tetap berada di Recently Deleted, dan sistem memberi tahu nama yang konflik. Pengguna harus mengganti nama sampel aktif yang konflik sebelum mencoba pemulihan kembali. Sistem tidak mengganti nama secara otomatis. Seluruh aturan validasi nama sampel yang berlaku tetap diterapkan. | Disepakati |
| FR-HUM-88 | Pengguna dapat memilih Pulihkan Semua pada Recently Deleted. Sistem memvalidasi seluruh sampel sekaligus, termasuk kapasitas maksimum 20 sampel per profil dan seluruh aturan validasi nama. Jika semua sampel memenuhi syarat, seluruhnya dipulihkan dalam satu tindakan. Jika satu atau lebih sampel gagal validasi, tidak ada sampel yang dipulihkan; sistem membatalkan seluruh tindakan dan menjelaskan alasan serta sampel yang bermasalah. Pengguna dapat menyelesaikan masalah lalu mencoba kembali. Setelah pemulihan massal berhasil, sistem menjalankan satu evaluasi ulang profil untuk keseluruhan perubahan. | Disepakati |
| FR-HUM-14 | Setiap entri sampel tulisan untuk membentuk atau memperbarui profil gaya personal harus memenuhi batas minimal 500 karakter, termasuk spasi dan baris baru. Sampel di bawah batas tetap dipertahankan sebagai entri, tetapi belum memenuhi syarat minimum. Sistem menampilkan jumlah karakter saat ini dan jumlah karakter yang masih dibutuhkan. | Disepakati |
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
| FR-ACC-10 | Pengguna dapat menghapus riwayat yang disimpan. Untuk entri riwayat Humanizer, penghapusan dilakukan setelah pengguna mengonfirmasi dan menghapus entri secara permanen dari riwayat yang dapat diakses melalui aplikasi. | Disepakati; cakupan penghapusan pada backup, log, dan penyimpanan turunan perlu ditetapkan dalam kebijakan retensi dan desain teknis |
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
| FR-HUM-89 | Ketika pengguna menghapus profil gaya, seluruh sampel yang berada di Recently Deleted dan berasal dari profil tersebut ikut dihapus permanen dari sistem yang dapat diakses pengguna. Sampel tersebut tidak dapat dipulihkan setelah profil asal dihapus. Ketentuan ini berlaku bersama penghapusan profil dan seluruh versi historisnya. | Disepakati |
| FR-HUM-90 | Pengguna dapat menghapus satu sampel secara permanen melalui tindakan Hapus Permanen atau menghapus seluruh sampel di Recently Deleted melalui tindakan Kosongkan Recently Deleted. Kedua tindakan wajib menampilkan dialog konfirmasi. Setelah dikonfirmasi, sampel tidak dapat dipulihkan melalui aplikasi dan tindakan tidak dapat dibatalkan. Penghapusan tidak memicu evaluasi ulang profil karena sampel tersebut sudah tidak termasuk dalam kumpulan sampel aktif. Penghapusan dari backup mengikuti kebijakan retensi teknis yang ditentukan pada tahap desain teknis. | Disepakati |
| FR-HUM-91 | Sistem mencatat metadata setiap tindakan penghapusan permanen sampel, baik penghapusan satu sampel maupun tindakan Kosongkan Recently Deleted. Catatan audit memuat ID sampel, ID profil asal, ID pengguna yang melakukan tindakan, waktu penghapusan, jenis tindakan, serta status keberhasilan atau kegagalan. Isi teks sampel tidak boleh disimpan di audit log. Retensi dan kontrol akses audit log mengikuti kebijakan audit dan keamanan yang dirinci pada desain teknis. | Disepakati |
| FR-HUM-92 | Pengguna dapat melihat riwayat aktivitas penghapusan sampelnya sendiri melalui halaman aktivitas akun. Riwayat menampilkan nama sampel, nama profil asal, waktu penghapusan, jenis tindakan (penghapusan biasa atau penghapusan permanen), serta status tindakan. Isi teks sampel, ID internal, dan informasi investigasi tidak ditampilkan. Pengguna tidak memperoleh akses langsung ke audit log internal. | Disepakati |
| FR-HUM-93 | Riwayat aktivitas penghapusan sampel ditampilkan kepada pengguna selama 90 hari sejak tindakan dilakukan, berlaku untuk penghapusan biasa maupun permanen. Setelah 90 hari, riwayat dihapus otomatis dari halaman aktivitas pengguna. Penghapusan riwayat aktivitas tidak mengubah status sampel dan tidak menghapus atau mengubah audit log internal. Retensi audit log internal mengikuti kebijakan tersendiri. | Disepakati |
| FR-HUM-94 | Pengguna dapat menghapus satu entri riwayat aktivitas penghapusan atau menghapus seluruh riwayat aktivitas penghapusan yang ditampilkan pada akunnya. Kedua tindakan wajib menampilkan dialog konfirmasi. Penghapusan hanya memengaruhi riwayat aktivitas yang terlihat oleh pengguna dan tidak menghapus atau mengubah sampel, status pemulihan, maupun audit log internal. | Disepakati |
| FR-HUM-95 | Penghapusan satu entri atau seluruh riwayat aktivitas penghapusan berlaku pada tingkat akun dan tersinkronisasi ke seluruh perangkat serta sesi akun. Jika halaman aktivitas masih terbuka di perangkat lain, sistem memperbarui tampilannya tanpa pengguna harus mengulangi penghapusan. Jika sinkronisasi real-time gagal sementara, sistem memperbarui tampilan ketika perangkat kembali terhubung atau saat data dimuat ulang. | Disepakati |
| FR-HUM-96 | Ketika penghapusan akun pengguna berhasil diselesaikan, seluruh riwayat aktivitas penghapusan milik akun tersebut ikut dihapus dari halaman aktivitas dan tidak dapat dipulihkan melalui aplikasi. Sampel, profil gaya, dan data pengguna lainnya mengikuti kebijakan penghapusan akun yang berlaku. Audit log internal tidak otomatis ikut terhapus dan mengikuti kebijakan retensi keamanan, audit, serta kewajiban yang berlaku. | Disepakati |
| FR-HUM-97 | Pengguna dapat membatalkan permintaan penghapusan akun selama status penghapusan masih tertunda dan proses penghapusan permanen belum dimulai. Jika pembatalan berhasil, akun kembali aktif seperti sebelumnya dan sampel, profil gaya, riwayat aktivitas, serta data terkait tetap dipertahankan. Setelah proses penghapusan permanen dimulai, pembatalan tidak lagi diperbolehkan. Sistem harus menampilkan status penghapusan akun dan batas waktu pembatalan secara jelas. Durasi masa penundaan belum ditentukan oleh kebutuhan ini. | Disepakati |
| FR-HUM-98 | Masa penundaan penghapusan akun adalah 30 hari sejak pengguna mengonfirmasi permintaan penghapusan. Selama periode tersebut akun berstatus Pending Deletion dan pengguna dapat membatalkan permintaan kapan saja. Setelah 30 hari, sistem memulai proses penghapusan permanen secara otomatis. Setelah proses penghapusan permanen dimulai, pembatalan tidak lagi tersedia. Lewatnya masa 30 hari tidak boleh dianggap sebagai bukti bahwa data telah terhapus jika proses penghapusan mengalami kegagalan teknis. | Disepakati |
| FR-HUM-99 | Selama masa penundaan penghapusan akun, akun berstatus Pending Deletion dengan akses terbatas. Pengguna tetap dapat login, melihat pemberitahuan penghapusan, membatalkan permintaan penghapusan, dan mengunduh data pribadinya. Pengguna tidak dapat menggunakan Humanizer atau AI Text Detector, membuat atau mengubah profil gaya, membeli atau menggunakan kredit, maupun mengubah pengaturan akun lainnya. Pengecualian pengaturan hanya berlaku untuk tindakan yang diperlukan guna membatalkan penghapusan akun. Mekanisme dan cakupan ekspor data pribadi dirinci dalam kebutuhan tersendiri. | Disepakati |
| FR-HUM-100 | Pengguna dapat mengunduh data pribadinya dalam satu paket ZIP, termasuk selama masa Pending Deletion. Paket memuat account-data.json (informasi akun dan pengaturan), humanizer-profiles.json (data profil gaya dan metadata sampel), direktori samples/ (teks sampel yang masih tersedia dan dapat diakses pengguna), humanizer-history.json (riwayat penggunaan Humanizer), detector-history.json (riwayat penggunaan AI Text Detector), dan credit-transactions.json (riwayat kredit dan transaksi). Ekspor tidak mencakup password, token autentikasi, audit log internal, atau data rahasia sistem. File sampel disertakan dalam format aslinya jika tersedia; jika tidak, teks diekspor sebagai TXT. Sistem memberi tahu pengguna jika ada data yang gagal dimasukkan ke paket ekspor. | Disepakati |
| FR-HUM-101 | Setiap akun dibatasi maksimal 3 permintaan pembuatan ekspor data pribadi dalam jendela bergulir 24 jam. Batas dihitung berdasarkan permintaan pembuatan ekspor, bukan jumlah unduhan. Pengguna dapat mengunduh ulang paket yang sama selama paket masih tersedia tanpa memakai kuota baru. Permintaan yang gagal akibat gangguan sistem tidak mengurangi kuota. Setelah batas tercapai, pengguna harus menunggu hingga jendela 24 jam bergulir. Sistem menampilkan jumlah permintaan yang tersisa. | Disepakati |
| FR-HUM-102 | Paket ekspor data pribadi tersedia selama 24 jam sejak berhasil dibuat dan dapat diunduh ulang berkali-kali selama masa berlaku. Setelah 24 jam, paket otomatis dihapus dari penyimpanan unduhan sementara. Paket kedaluwarsa harus dibuat ulang melalui permintaan ekspor baru dan menggunakan kuota ekspor. Sistem menampilkan waktu kedaluwarsa. Unduhan yang sudah berjalan saat paket mencapai waktu kedaluwarsa tidak langsung diputus dan dapat diselesaikan. | Disepakati |
| FR-HUM-103 | Pengguna dapat membatalkan proses pembuatan ekspor data pribadi selama status masih Processing melalui tombol Batalkan Ekspor dan konfirmasi. Sistem menghentikan proses jika pembatalan berhasil. Pembatalan yang berhasil tidak mengurangi kuota permintaan ekspor. Jika proses selesai sebelum pembatalan diproses, pembatalan ditolak dan paket tetap tersedia sesuai masa berlakunya. Sistem menampilkan status akhir Cancelled atau Completed sesuai hasil proses. | Disepakati |
| FR-HUM-104 | Setiap akun hanya boleh memiliki satu permintaan ekspor data pribadi berstatus Processing pada satu waktu. Pengguna harus menunggu proses selesai atau membatalkannya sebelum membuat permintaan baru. Paket yang telah selesai dibuat tidak dihitung sebagai proses aktif; pengguna dapat membuat ekspor baru selama kuota tersedia. Jika pengguna mengajukan permintaan baru ketika proses lain masih Processing, sistem menolak permintaan dan menampilkan status ekspor yang sedang berjalan. | Disepakati |
