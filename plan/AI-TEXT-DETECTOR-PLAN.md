# AI Text Detector Plan

Status: **Draft rancangan produk dan riset**  
Modul: AI Text Detector  
Bahasa fokus: Bahasa Indonesia  
Terakhir ditinjau: 2026-11-12

## 1. Tujuan

Membantu pengguna memperoleh indikasi apakah suatu teks memiliki karakteristik tulisan manusia, teks yang dihasilkan AI, atau campuran keduanya.

Detector tidak boleh diposisikan sebagai alat pembuktian mutlak. Hasil model adalah estimasi berdasarkan data dan distribusi yang dipelajari. Tulisan manusia dapat salah ditandai sebagai AI dan teks AI dapat tidak terdeteksi.

## 2. Cakupan yang pernah direncanakan

- Mendukung analisis teks Bahasa Indonesia.
- Mempertimbangkan teks pendek dan panjang.
- Menampilkan ringkasan hasil dokumen.
- Menyediakan indikasi atau highlight pada tingkat kalimat.
- Memperhatikan false positive terhadap tulisan manusia.
- Mempertimbangkan penggunaan model yang dapat dijalankan dengan kebutuhan komputasi relatif ringan/CPU-first.

Catatan: detail panjang minimum/maksimum, format input, UI hasil, dan target performa belum disahkan.

## 3. Kategori label riset

| Label | Makna rancangan |
|---|---|
| HUMAN | Teks yang ditulis manusia dengan provenance yang memadai |
| AI | Teks yang dihasilkan AI dengan provenance yang memadai |
| MIXED | Teks yang menggabungkan kontribusi manusia dan AI |
| UNCERTAIN | Kasus yang tidak cukup jelas untuk label utama; dikelola terpisah dan tidak menjadi kelas training utama |

Label dokumen tidak otomatis boleh diwariskan ke setiap kalimat. Anotasi tingkat kalimat memerlukan bukti dan aturan tersendiri.

## 4. Rencana pendekatan eksperimen

Urutan eksperimen yang pernah dibahas sebagai kandidat:

1. Majority-class baseline.
2. TF-IDF dengan Logistic Regression dan/atau SVM.
3. IndoBERT-Lite sebagai kandidat backbone.
4. IndoBERT Base sebagai kandidat backbone.
5. Perbandingan klasifikasi berbasis kalimat dan pendekatan yang menggunakan konteks.
6. Evaluasi optimasi inference, termasuk ONNX INT8 dibanding FP32 jika relevan.

Nama model di atas adalah kandidat pretrained backbone, bukan detector siap pakai dan bukan keputusan final pemilihan model.

Kandidat yang pernah disebut:
- `indobenchmark/indobert-lite-base-p2`
- `indobenchmark/indobert-base-p2`
- XLM-RoBERTa-base
- mDeBERTa-v3-base

Pemilihan akhir harus didasarkan pada evaluasi data, lisensi, biaya komputasi, latensi, dan performa pada domain target.

## 5. Metrik evaluasi yang pernah direncanakan

- Macro F1.
- Precision dan recall per kelas.
- False Positive Rate untuk kelas HUMAN.
- PR-AUC jika sesuai dengan setup evaluasi.
- Confusion matrix.
- Calibration dan kualitas confidence.
- Latency, throughput, serta peak RAM.

Threshold harus ditentukan menggunakan validation set. Test set tidak boleh digunakan untuk memilih threshold atau tuning model.

## 6. Risiko metodologis

- Dataset publik tidak otomatis berlabel HUMAN.
- Gaya penulisan, topik, panjang teks, dan sumber dapat menjadi shortcut bagi model.
- Teks turunan, parafrasa, pasangan prompt-output, dan versi suntingan dapat menyebabkan data leakage.
- Generator AI yang hanya berasal dari sedikit provider dapat membuat evaluasi tidak mewakili penggunaan nyata.
- False positive terhadap tulisan manusia dapat merugikan pengguna.
- Skor probabilitas tidak boleh dipresentasikan sebagai persentase kepastian kepengarangan tanpa kalibrasi dan penjelasan yang benar.

## 7. Output produk yang perlu dirancang

Masih perlu diputuskan melalui analisis sistem dan UI/UX:
- Ringkasan dokumen.
- Hasil per kategori dan penjelasan confidence.
- Highlight tingkat kalimat.
- Penanganan teks terlalu pendek atau input tidak valid.
- Pesan untuk kasus di luar distribusi atau confidence rendah.
- Batasan dan penjelasan interpretasi hasil.

## 8. Belum diputuskan

- Model final dan konfigurasi training.
- Skema inference dan API.
- Panjang input dan strategi chunking.
- Format output final.
- Target akurasi/performa yang realistis.
- Strategi monitoring drift dan evaluasi ulang.
- Apakah model berjalan pada server sendiri atau layanan eksternal.

## 9. Gerbang sebelum implementasi model

Jangan mulai training serius sebelum:
1. Struktur data dan schema ditetapkan.
2. Uji prosedur sintetis lulus.
3. Sumber data, provenance, dan lisensi diaudit.
4. Pilot 200–500 dokumen dikumpulkan dan dievaluasi kualitasnya.
5. Aturan anotasi dan split berbasis grup ditetapkan.
6. Baseline dan protokol evaluasi dapat diulang.
