# AI Humanizer Plan

Status: **Draft rancangan produk; kebutuhan produk awal telah dielisitasi**  
Modul: AI Humanizer  
Bahasa: Bahasa Indonesia dan Bahasa Inggris; mempertahankan campuran bahasa jika memungkinkan  
Terakhir ditinjau: 2026-11-12

Rincian kebutuhan produk yang disepakati berada di [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md), khususnya bagian AI Humanizer.

## 1. Tujuan

Membantu pengguna menyunting teks agar lebih alami, jelas, dan sesuai gaya yang diinginkan tanpa mengubah makna, fakta, atau maksud utama.

Humanizer adalah alat bantu penyuntingan. Fitur ini tidak boleh menjanjikan bahwa hasilnya pasti lolos AI detector atau tidak dapat dikenali sebagai teks AI.

## 2. Kebutuhan produk yang telah disepakati

- AI Humanizer hanya dapat digunakan oleh pengguna yang sudah login/terdaftar. Guest hanya memiliki akses ke AI Text Detector.

- Target utama pengguna adalah mahasiswa dan pelajar; cakupan tulisan akademik dan umum.
- Humanizer memiliki prioritas setara dengan Detector dan dapat digunakan mandiri.
- Teks dari Detector hanya dikirim ke Humanizer setelah tindakan pengguna.
- Bahasa yang didukung adalah Bahasa Indonesia dan Bahasa Inggris.
- Teks campuran dapat diproses dengan upaya mempertahankan kedua bahasa dan peringatan keterbatasan.
- Input dapat berupa teks atau file TXT, DOCX, PDF.
- Batas yang terlihat menggunakan karakter; batas token juga diterapkan internal. Angka belum ditentukan.
- Teks panjang dapat diproses secara bertahap dengan mekanisme penjagaan konteks.
- Pengguna memilih gaya bawaan atau profil gaya personal. Kontrol intensitas perubahan tidak termasuk kebutuhan MVP yang dipilih.
- Sampel gaya dapat ditempel atau diunggah sebagai TXT, DOCX, atau PDF.
- Sistem menilai kecukupan sampel secara adaptif dan meminta tambahan jika perlu.
- Jika sampel belum cukup beragam atau konsisten, sistem mempertahankan sampel sebelumnya dan meminta pengguna menambahkan tulisan sebagai entri sampel terpisah. Setiap sampel ditampilkan dan dikelola sebagai entri tersendiri, sedangkan sistem menilai seluruh sampel sebagai satu kumpulan untuk membentuk atau memperbarui profil gaya. Pengguna tidak perlu mengirim ulang seluruh sampel.
- Sampel profil gaya harus memenuhi batas minimal karakter. Jika belum cukup, sistem mempertahankan sampel yang sudah diberikan, meminta tambahan, dan menampilkan jumlah minimum yang diwajibkan serta jumlah karakter sampel saat ini.
- Penghitungan panjang sampel mencakup seluruh karakter yang dimasukkan, termasuk spasi dan baris baru. Angka minimum belum ditentukan.
- Profil gaya dapat disimpan secara opsional, digunakan kembali, diperbarui, dan dihapus.
- Pengguna dapat mengedit atau menghapus setiap entri sampel secara individual tanpa mengganggu entri lainnya.
- Jika evaluasi ulang menunjukkan kumpulan sampel tidak lagi memadai, profil tetap terlihat di daftar pengelolaan tetapi tidak dapat dipilih untuk pemrosesan Humanizer sampai sampel tambahan membuatnya memenuhi kriteria kembali.
- Jika evaluasi ulang gagal karena gangguan teknis, sistem menandai evaluasi sebagai gagal dan meminta pengguna mencoba kembali. Kegagalan teknis tidak boleh dianggap sebagai bukti bahwa sampel tidak memadai atau otomatis menonaktifkan profil.
- Selama evaluasi ulang belum menghasilkan keputusan bahwa sampel tidak memadai, versi profil sebelumnya tetap dapat digunakan untuk pemrosesan Humanizer, termasuk ketika evaluasi gagal secara teknis. Jika evaluasi berhasil dan menyatakan sampel tidak memadai, profil tidak lagi dapat dipilih.
- Setiap kali sampel pada profil yang sudah terbentuk diedit atau dihapus, sistem mengevaluasi ulang seluruh kumpulan sampel yang tersisa. Jika tidak lagi memadai, sistem memberi peringatan dan meminta sampel tambahan sebelum profil digunakan atau diperbarui kembali.
- Pengguna dapat memiliki satu profil utama dan beberapa profil tambahan.
- Hasil menampilkan teks asli, hasil revisi, dan bagian yang berubah.
- Pengguna dapat menyalin seluruh atau sebagian hasil dan mengunduh TXT, DOCX, atau PDF.

## 3. Prinsip kualitas

1. **Meaning preservation:** jangan mengubah klaim, angka, nama, kutipan, atau hubungan sebab-akibat tanpa izin.
2. **Factual integrity:** jangan menambahkan fakta atau referensi yang tidak ada.
3. **Traceability:** pengguna dapat membandingkan teks asli dan hasil revisi serta melihat perubahan.
4. **User control:** pengguna memutuskan menerima, mengubah, menyalin, atau menolak hasil.
5. **Transparency:** jelaskan bahwa hasil adalah saran penyuntingan, bukan jaminan hasil Detector tertentu.
6. **Privacy by design:** minimalkan pengumpulan dan penyimpanan teks.

## 4. Profil gaya personal

Profil gaya merupakan data terpisah dari dokumen sampel sumber. Pengguna dapat:
- Membuat profil dari sampel tulisan.
- Memilih menyimpan profil.
- Menetapkan satu profil utama.
- Membuat profil tambahan.
- Menggunakan kembali, memperbarui, dan menghapus profil.

Daftar gaya bawaan, jumlah profil maksimum, bentuk representasi profil, dan mekanisme profiling belum diputuskan.

Setelah profil terbentuk, setiap perubahan atau penghapusan entri sampel memicu evaluasi ulang terhadap seluruh kumpulan sampel yang tersisa. Jika kumpulan tidak lagi memadai, pengguna diberi peringatan dan diminta menambahkan sampel. Kriteria kecukupan, status profil selama evaluasi, serta strategi versi atau pemulihan profil sebelumnya masih perlu dirancang. Profil yang tidak memadai tetap ditampilkan pada daftar pengelolaan, tetapi tidak tersedia sebagai pilihan untuk pemrosesan Humanizer sampai sampel tambahan membuatnya memadai kembali. Jika evaluasi ulang gagal karena gangguan teknis, sistem harus membedakan kegagalan evaluasi dari hasil penilaian kecukupan, meminta pengguna mencoba kembali, dan tidak otomatis menonaktifkan profil. Versi profil sebelumnya tetap dapat digunakan sampai evaluasi yang berhasil menyatakan sampel tidak memadai. Jika evaluasi berhasil dan menyatakan kumpulan sampel memadai, sistem membangun versi profil baru secara terpisah. Versi aktif sebelumnya tidak diganti selama pembangunan berlangsung. Versi baru diaktifkan hanya setelah pembangunan berhasil; jika pembangunan gagal, versi aktif sebelumnya tetap digunakan dan kegagalan pembaruan dicatat. Dengan demikian, aktivasi versi baru dilakukan secara atomik dari sudut pandang penggunaan profil. Setelah versi baru berhasil diaktifkan, versi sebelumnya disimpan selama 30 hari dan dapat dipulihkan pengguna dalam periode tersebut. Pemulihan hanya mengaktifkan kembali artefak profil versi lama dan tidak mengembalikan kumpulan sampel sumber ke kondisi sebelumnya. Versi lama yang melewati masa retensi dapat dihapus otomatis. Setiap pemulihan yang berhasil juga menonaktifkan versi yang sebelumnya aktif dan menyimpannya sebagai versi historis selama 30 hari sejak waktu pemulihan. Aturan masa retensi berlaku per versi yang baru dinonaktifkan; pemulihan satu versi tidak memperpanjang masa retensi versi historis lainnya. Setiap profil dibatasi maksimal 5 versi historis yang belum kedaluwarsa. Versi historis yang masih berada dalam masa retensi tidak boleh dihapus otomatis hanya untuk menyediakan slot. Jika seluruh slot terisi, aktivasi versi baru atau pemulihan versi historis ditunda sampai tersedia slot; versi aktif tetap digunakan selama penundaan. Jika permintaan pemulihan versi historis dan pembaruan profil sama-sama tertunda, pemulihan mendapat prioritas lebih tinggi dan pembaruan menunggu. Pengguna dapat membatalkan permintaan pergantian versi selama masih tertunda dan belum mulai dieksekusi. Untuk setiap profil, hanya satu permintaan pemulihan versi yang boleh aktif pada satu waktu, baik saat tertunda maupun sedang dieksekusi. Permintaan tambahan ditolak sementara sampai permintaan sebelumnya diselesaikan atau dibatalkan. Jika permintaan masih tertunda, pengguna dapat membatalkannya sebelum mengajukan pemulihan ke versi lain. Jika pemulihan gagal, versi aktif tetap tidak berubah dan pengguna dapat mengajukan percobaan baru setelah permintaan gagal diselesaikan. Ketika pemulihan mulai dieksekusi, pembaruan atau pembentukan ulang profil ditahan sampai pemulihan selesai. Baik pemulihan berhasil maupun gagal, pembaruan yang tertahan dilanjutkan menggunakan kumpulan sampel terbaru dan menjalani evaluasi ulang. Sistem tidak boleh memakai hasil evaluasi lama untuk membangun atau mengaktifkan versi baru. Jika pemulihan gagal, versi aktif tidak berubah dan kegagalan tersebut tidak menghalangi pembaruan profil. Pengguna tetap dapat menghapus sampel selama pembaruan tertahan. Ketika pembaruan dilanjutkan, sistem menggunakan kumpulan sampel yang tersisa setelah penghapusan dan mengevaluasi ulang seluruh kumpulan tersebut. Sampel yang sudah dihapus tidak boleh digunakan untuk membangun versi profil baru.

Retensi sampel mentah setelah profiling juga belum diputuskan secara teknis. Jangan menganggap penyimpanan profil berarti penyimpanan dokumen sumber. Default minimisasi data perlu dipertahankan dan kebijakan retensi harus dinyatakan secara eksplisit.

## 5. Dokumen dan ekstraksi

TXT, DOCX, dan PDF diterima sebagai sumber teks maupun sampel gaya. Sistem mengekstrak teks dan mempertahankan struktur yang didukung. Tabel, header/footer, catatan kaki, kolom, dan teks di gambar tidak dijamin utuh. PDF hasil pemindaian memerlukan OCR. Sistem perlu memberi tahu pengguna jika ekstraksi tidak lengkap.

Dokumen sumber tidak boleh ditimpa. Hasil ekspor dapat berupa TXT, DOCX, atau PDF, tetapi tidak dijanjikan identik dengan tata letak dokumen sumber.

## 6. Alur pengguna konseptual

Prasyarat: pengguna harus login sebelum dapat memulai Humanizer atau mengelola profil gaya. Jika Guest mencoba membuka alur Humanizer, sistem meminta login/registrasi sebelum teks diproses.

1. Pengguna memasukkan teks atau mengunggah dokumen.
2. Sistem mengekstrak dan menampilkan/menjelaskan masalah ekstraksi jika ada.
3. Pengguna memilih gaya bawaan atau profil gaya personal.
4. Sistem memvalidasi kecukupan input dan kapasitas.
5. Sistem memproses teks, langsung atau melalui antrean.
6. Sistem menampilkan teks asli, hasil revisi, dan penyorotan perubahan.
7. Pengguna meninjau, menyalin, mengunduh, atau menyimpan hasil secara opsional.

## 7. Risiko dan pengujian

- Perubahan makna atau fakta.
- Hilangnya terminologi teknis.
- Perubahan kutipan atau data numerik.
- Hasil terlalu generik dan menghilangkan karakter penulis.
- Hasil tidak alami atau tidak sesuai konteks bahasa.
- Prompt injection yang tertanam di dalam teks input.
- Kebocoran teks pengguna ke layanan pihak ketiga.
- Profil gaya yang dibentuk dari sampel terlalu sedikit atau tidak konsisten.
- Inkonsistensi gaya akibat chunking.

Pengujian perlu mencakup teks akademik, formal, informal, teknis, teks dengan angka/nama, Bahasa Indonesia, Bahasa Inggris, dan code-switching. Dataset evaluasi Humanizer harus dipisahkan dari data training/evaluasi Detector agar transformasi tidak merusak validitas pengukuran Detector.

## 8. Hal yang belum diputuskan

- Provider/model dan biaya.
- Daftar final gaya bawaan.
- Algoritma profiling dan kriteria kecukupan sampel.
- Bentuk penyimpanan profil dan retensi sampel mentah.
- Batas karakter/token, ukuran file, dan strategi chunking.
- Library ekstraksi, OCR, serta generator dokumen.
- Target kualitas dan protokol evaluasi.
- Penyimpanan hasil, autentikasi, kuota, dan monetisasi pada tingkat implementasi.
