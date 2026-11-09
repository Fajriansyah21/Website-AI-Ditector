# Riset Model dan Rancangan AI Text Detector

**Status:** kandidat dan hipotesis teknis, belum ada model final.  
**Bahasa target:** Bahasa Indonesia.  
**Prioritas:** CPU-first, klasifikasi tingkat kalimat.

## 1. Kebutuhan model

Detector TextLab perlu:

- Mengolah teks pendek dan panjang.
- Mengklasifikasikan kalimat ke HUMAN, AI, atau MIXED.
- Mendukung status UNCERTAIN di luar kelas training utama.
- Mempertimbangkan konteks kalimat tetangga.
- Mempertahankan offset kalimat untuk highlight UI.
- Berjalan dengan biaya inferensi rendah dan mengutamakan CPU.

## 2. Kandidat pretrained backbone

| Model | Karakteristik dari riset awal | Peran |
|---|---|---|
| IndoBERT-Lite Base P2 | Bahasa Indonesia, ALBERT, sekitar 11,7M parameter | Kandidat utama untuk uji CPU |
| IndoBERT Base P2 | Bahasa Indonesia, sekitar 124,5M parameter | Pembanding kualitas |
| XLM-R Base | Multilingual | Baseline multilingual opsional |
| mDeBERTa-v3 Base | Multilingual; embedding perlu diperhitungkan dalam ukuran | Kandidat opsional |

Model card:
- https://huggingface.co/indobenchmark/indobert-lite-base-p2
- https://huggingface.co/indobenchmark/indobert-base-p2
- https://huggingface.co/FacebookAI/xlm-roberta-base
- https://huggingface.co/microsoft/mdeberta-v3-base

Semua kandidat di atas adalah pretrained backbone, bukan detector AI siap pakai. Masing-masing memerlukan fine-tuning dan evaluasi pada dataset TextLab.

## 3. Rencana eksperimen

| ID | Eksperimen |
|---|---|
| E0 | Majority-class baseline |
| E1 | TF-IDF + Logistic Regression |
| E2 | TF-IDF + SVM |
| E3 | IndoBERT-Lite fine-tuning |
| E4 | IndoBERT Base fine-tuning |
| E5 | Sentence-alone dibanding sentence + neighboring context |
| E6 | ONNX INT8 dibanding FP32 |

Gunakan split dan preprocessing yang sama untuk perbandingan model. Jalankan beberapa random seed untuk eksperimen utama.

## 4. Rentang konfigurasi eksploratif

Ini hanya rentang untuk eksperimen awal, bukan konfigurasi final:

- Input length: 128, 256, 512 token.
- Batch size: 8, 16, 32 jika memori memungkinkan.
- Learning rate: 1e-5, 2e-5, 3e-5.
- Epoch: 2–5 dengan early stopping.
- Beberapa random seed.

Konfigurasi final harus ditentukan dari hasil validation dan benchmark.

## 5. Metrik

Wajib dilaporkan:

- Macro F1.
- Precision dan recall per kelas.
- False Positive Rate untuk HUMAN.
- PR-AUC.
- Confusion matrix.
- Calibration error atau evaluasi kalibrasi yang sesuai.
- Latensi per dokumen dan per kalimat.
- Throughput.
- Peak RAM.

Lakukan breakdown untuk teks pendek/panjang, genre, jenis penyuntingan, penulis yang ditahan, dan generator yang ditahan.

## 6. Arsitektur inferensi konseptual

Frontend → API Gateway → Detection Service → CPU inference runtime → calibration/aggregation → structured result.

API Gateway:
- Validasi input.
- Rate limiting dan quota.
- Request ID.
- Kegagalan inferensi tidak mengurangi kuota.

Detection Service:
- Segmentasi kalimat.
- Menyimpan start_char dan end_char pada teks asli.
- Mengelola Unicode dan normalisasi.
- Membentuk input dengan konteks jika eksperimen mendukungnya.

Inference runtime:
- Memuat model dan tokenizer versi tertentu.
- Mendukung batch inference.
- Uji ONNX hanya setelah baseline akurasi stabil.

Aggregation:
- Menghasilkan skor kelas per kalimat.
- Menetapkan UNCERTAIN jika threshold/aturan abstention terpenuhi.
- Menghasilkan ringkasan dokumen yang tidak diklaim sebagai persentase pasti kepengarangan.

## 7. Kuantisasi dan infrastruktur

Estimasi bobot mentah:
- IndoBERT-Lite 11,7M: ~47 MB FP32; ~12 MB teoritis INT8.
- IndoBERT Base 124,5M: ~498 MB FP32; ~125 MB teoritis INT8.

Ini bukan estimasi total RAM proses. Tokenizer, aktivasi, runtime, batch, dan overhead harus dihitung melalui pengukuran.

ONNX Runtime quantization:
https://onnxruntime.ai/docs/performance/model-optimizations/quantization.html

Dynamic quantization layak diuji sebagai langkah awal untuk transformer, tetapi tidak menjamin inferensi lebih cepat pada semua CPU. Evaluasi akurasi, latensi, dan RAM pada mesin target.

## 8. Catatan metodologis

- Jangan menggunakan akurasi tunggal sebagai ukuran kualitas.
- Split berdasarkan group_id untuk mencegah kebocoran.
- Test lintas penulis dan sebagian generator.
- Dokumen berlabel tidak boleh otomatis mewariskan label yang sama ke semua kalimat.
- Teks pendek atau ambigu dapat menghasilkan UNCERTAIN.
- Model detector tidak membuktikan kepengarangan atau kecurangan.

## 9. Referensi awal

- IndoBERT-Lite: https://huggingface.co/indobenchmark/indobert-lite-base-p2
- IndoBERT Base: https://huggingface.co/indobenchmark/indobert-base-p2
- XLM-R: https://huggingface.co/FacebookAI/xlm-roberta-base
- mDeBERTa-v3: https://huggingface.co/microsoft/mdeberta-v3-base
- ONNX Runtime quantization: https://onnxruntime.ai/docs/performance/model-optimizations/quantization.html

Catatan studi tentang deteksi teks AI Bahasa Indonesia yang pernah dibahas masih perlu diverifikasi ke publikasi primer sebelum dipakai sebagai sitasi formal.
