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
