# AI Humanizer Plan

Status: **Draft rancangan fitur**  
Modul: AI Humanizer  
Terakhir ditinjau: 2026-11-12

## 1. Tujuan

Membantu pengguna menyunting teks agar lebih alami, jelas, dan sesuai gaya yang diinginkan tanpa mengubah makna, fakta, atau maksud utama.

Humanizer adalah alat bantu penyuntingan. Fitur ini tidak boleh menjanjikan bahwa hasilnya pasti lolos AI detector atau tidak dapat dikenali sebagai teks AI.

## 2. Arah fitur yang pernah dibahas

- Pengguna memberikan teks untuk disunting.
- Sistem menghasilkan versi revisi.
- Makna, fakta, istilah penting, dan maksud penulis harus dipertahankan.
- Profil gaya penulisan berdasarkan sampel tulisan pengguna pernah dibahas sebagai kemungkinan pendekatan personalisasi.

Profil gaya dari sampel tulisan masih merupakan rancangan yang perlu ditinjau dari sisi manfaat, consent, keamanan, dan privasi. Belum menjadi keputusan implementasi.

## 3. Prinsip kualitas

1. **Meaning preservation**: jangan mengubah klaim, angka, nama, kutipan, atau hubungan sebab-akibat tanpa izin.
2. **Factual integrity**: jangan menambahkan fakta atau referensi yang tidak ada.
3. **Traceability**: pengguna sebaiknya dapat membandingkan teks asli dan hasil revisi.
4. **User control**: pengguna tetap memutuskan menerima, mengubah, atau menolak hasil.
5. **Transparency**: jelaskan bahwa hasil adalah saran penyuntingan, bukan jaminan hasil detector tertentu.
6. **Privacy by design**: minimalkan pengumpulan dan penyimpanan teks.

## 4. Privasi sampel gaya

Pernah dibahas bahwa sampel tulisan untuk membangun profil gaya sebaiknya tidak disimpan sebagai teks mentah setelah proses profiling, kecuali pengguna secara eksplisit memilih penyimpanan.

Ini adalah prinsip rancangan awal, bukan kebijakan final. Sebelum implementasi perlu ditentukan:
- Apakah profiling benar-benar diperlukan untuk MVP.
- Bentuk representasi profil yang disimpan.
- Consent dan mekanisme pencabutan.
- Retensi dan penghapusan.
- Apakah pemrosesan dilakukan lokal atau melalui layanan eksternal.
- Risiko sampel mengandung data pribadi atau informasi sensitif.

## 5. Alur pengguna konseptual

1. Pengguna memasukkan atau menempelkan teks.
2. Pengguna memilih tujuan/gaya revisi jika opsi tersebut disediakan.
3. Sistem memvalidasi input.
4. Sistem memproses teks dan menghasilkan saran revisi.
5. Sistem menampilkan teks asli dan hasil revisi.
6. Pengguna meninjau, menyalin, atau mengedit hasil.

Alur ini masih konseptual. Detail pilihan gaya, batas input, biaya, dan provider belum ditentukan.

## 6. Risiko dan pengujian

- Perubahan makna atau fakta.
- Hilangnya terminologi teknis.
- Perubahan kutipan atau data numerik.
- Hasil terlalu generik dan menghilangkan karakter penulis.
- Bahasa tidak alami atau tidak sesuai konteks Bahasa Indonesia.
- Prompt injection yang tertanam di dalam teks input.
- Kebocoran teks pengguna ke layanan pihak ketiga.

Pengujian perlu mencakup teks akademik, formal, informal, teknis, teks dengan angka/nama, dan teks campuran. Dataset evaluasi humanizer harus dipisahkan dari data training/evaluasi detector agar transformasi tidak merusak validitas pengukuran detector.

## 7. Belum diputuskan

- Apakah humanizer menggunakan LLM eksternal atau model yang dikelola sendiri.
- Pilihan provider dan biaya.
- Mode gaya penulisan.
- Profil gaya personal.
- Batas karakter/token dan penanganan dokumen panjang.
- Penyimpanan riwayat.
- Autentikasi, kuota, dan monetisasi.
- Target kualitas dan protokol evaluasi.
