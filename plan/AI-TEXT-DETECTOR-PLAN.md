# AI Text Detector Plan

Status: **Draft rancangan produk dan riset; kebutuhan produk awal telah dielisitasi**  
Modul: AI Text Detector  
Bahasa: Bahasa Indonesia dan Bahasa Inggris; code-switching dengan peringatan keterbatasan  
Terakhir ditinjau: 2026-11-14

Rincian kebutuhan produk yang disepakati berada di [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md), khususnya bagian 4.3 AI Text Detector. Dokumen ini mempertahankan fokus rencana produk tingkat tinggi dan riset metodologi model; ringkasan kemajuan elisitasi berada di [Requirements Elicitation Status](REQUIREMENTS-ELICITATION-STATUS.md).

## 1. Tujuan

Membantu pengguna memperoleh indikasi apakah suatu teks memiliki karakteristik tulisan manusia, teks yang dihasilkan AI, atau campuran keduanya.

Detector tidak boleh diposisikan sebagai alat pembuktian mutlak. Hasil model adalah estimasi berdasarkan data dan distribusi yang dipelajari. Tulisan manusia dapat salah ditandai sebagai AI dan teks AI dapat tidak terdeteksi.

## 2. Kebutuhan produk yang telah disepakati

- Target utama pengguna adalah mahasiswa dan pelajar, untuk kebutuhan akademik maupun umum.
- Detector dan Humanizer memiliki prioritas setara dan dapat digunakan mandiri.
- Hasil Detector dapat dikirim ke Humanizer hanya setelah tindakan pengguna.
- Bahasa yang didukung adalah Bahasa Indonesia dan Bahasa Inggris.
- Teks campuran Indonesia-Inggris dapat dianalisis dengan peringatan keterbatasan.
- Input Detector menyediakan dua cara: mengetik/menempelkan teks langsung dan mengunggah file TXT, DOCX, atau PDF.
- AI Text Detector memiliki batas maksimal jumlah karakter input. Angka batas maksimal belum ditentukan. Tidak ada batas minimum karakter yang disepakati untuk input Detector.
- Batas yang ditampilkan kepada pengguna menggunakan karakter, sedangkan sistem juga membatasi token internal.
- Batas input Detector dapat berbeda dari Humanizer.
- Pemrosesan pendek/panjang bersifat adaptif; teks panjang dapat dipecah dengan chunking.
- Output mencakup ringkasan keseluruhan, analisis per kalimat, dan penjelasan.
- Kategori produk: HUMAN, AI, MIXED, UNCERTAIN.
- Hasil dapat ditampilkan di website dan diunduh sebagai TXT, DOCX, atau PDF.
- Pengguna dapat menyalin seluruh atau sebagian hasil.

## 3. Kategori label produk dan metodologi

| Label produk | Makna umum |
|---|---|
| HUMAN | Karakteristik teks lebih konsisten dengan tulisan manusia |
| AI | Karakteristik teks lebih konsisten dengan teks yang dihasilkan AI |
| MIXED | Teks menunjukkan indikasi kontribusi manusia dan AI |
| UNCERTAIN | Bukti tidak cukup untuk kesimpulan yang dapat diandalkan |

Kategori produk tersebut belum menggantikan definisi operasional label dataset. Label dokumen tidak otomatis boleh diwariskan ke setiap kalimat. Anotasi tingkat kalimat memerlukan bukti dan aturan tersendiri.

UNCERTAIN adalah kategori hasil produk. Penanganannya dalam training/evaluasi tetap mengikuti keputusan metodologis terpisah dan belum menjadi kelas training utama.

## 4. Rencana pendekatan eksperimen

Urutan eksperimen kandidat:
1. Majority-class baseline.
2. TF-IDF dengan Logistic Regression dan/atau SVM.
3. IndoBERT-Lite sebagai kandidat backbone.
4. IndoBERT Base sebagai kandidat backbone.
5. Perbandingan klasifikasi berbasis kalimat dan pendekatan yang menggunakan konteks.
6. Evaluasi optimasi inference, termasuk ONNX INT8 dibanding FP32 jika relevan.

Nama model di atas adalah kandidat pretrained backbone, bukan detector siap pakai dan bukan keputusan final pemilihan model.

Kandidat yang pernah disebut:
- indobenchmark/indobert-lite-base-p2
- indobenchmark/indobert-base-p2
- XLM-RoBERTa-base
- mDeBERTa-v3-base

Pemilihan akhir harus didasarkan pada evaluasi data, lisensi, biaya komputasi, latensi, dan performa pada domain target.

## 5. Metrik evaluasi yang direncanakan

- Macro F1.
- Precision dan recall per kelas.
- False Positive Rate untuk kelas HUMAN.
- PR-AUC jika sesuai dengan setup evaluasi.
- Confusion matrix.
- Calibration dan kualitas confidence.
- Latency, throughput, serta peak RAM.

Threshold harus ditentukan menggunakan validation set. Test set tidak boleh digunakan untuk memilih threshold atau tuning model.

## 6. Risiko metodologis dan batas klaim

- Dataset publik tidak otomatis berlabel HUMAN.
- Gaya penulisan, topik, panjang teks, bahasa, dan sumber dapat menjadi shortcut bagi model.
- Teks turunan, parafrasa, pasangan prompt-output, dan versi suntingan dapat menyebabkan data leakage.
- Generator AI dari sedikit provider dapat membuat evaluasi tidak mewakili penggunaan nyata.
- False positive terhadap tulisan manusia dapat merugikan pengguna.
- Skor probabilitas tidak boleh dipresentasikan sebagai persentase kepastian kepengarangan tanpa kalibrasi dan penjelasan yang benar.
- Penjelasan hasil tidak boleh mengarang alasan yang tidak didukung analisis.

## 7. Hal yang belum diputuskan

- Model final dan konfigurasi training.
- Definisi operasional kelas dan panduan anotasi.
- Mekanisme confidence, threshold, dan kalibrasi.
- Skema inference dan API.
- Batas input karakter/token dan strategi chunking rinci.
- Format laporan final dan desain UI.
- Target akurasi/performa yang realistis.
- Strategi monitoring drift dan evaluasi ulang.
- Apakah model berjalan pada server sendiri atau layanan eksternal.

## 8. Gerbang sebelum implementasi model

Jangan mulai training serius sebelum:
1. Struktur data dan schema ditetapkan.
2. Uji prosedur sintetis lulus.
3. Sumber data, provenance, dan lisensi diaudit.
4. Pilot 200-500 dokumen dikumpulkan dan dievaluasi kualitasnya.
5. Aturan anotasi dan split berbasis grup ditetapkan.
6. Baseline dan protokol evaluasi dapat diulang.
