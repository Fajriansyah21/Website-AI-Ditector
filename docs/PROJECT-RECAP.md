# Rekap Rancangan Proyek TextLab

Dokumen ini merekam rancangan yang telah dibahas sebelum penyusunan Data Collection Blueprint. Isinya merupakan catatan keputusan dan analisis terdahulu, bukan klaim bahwa seluruh komponen sudah dibangun atau divalidasi.

**Status:** dokumentasi analisis  
**Produk:** TextLab  
**Repository:** Website-AI-Ditector

## 1. Visi dan ruang lingkup produk

TextLab dirancang sebagai website yang menyediakan tiga layanan:

1. **AI Text Detector** untuk menganalisis teks dan memberikan indikasi apakah teks ditulis manusia, dihasilkan AI, atau merupakan gabungan keduanya.
2. **AI Humanizer** untuk membantu menyesuaikan gaya penulisan berdasarkan profil suara pengguna atau gaya dari pustaka.
3. **DOCX Metadata Cleaner** untuk membersihkan metadata pada dokumen DOCX. Format awal yang dipilih hanya DOCX.

Target pengguna awal adalah pengguna umum.

## 2. Monetisasi dan akses

Keputusan awal yang telah dibahas:

- Detector dapat digunakan tanpa membuat akun.
- Pengguna detector memperoleh 3 penggunaan gratis per hari.
- Penggunaan detector di luar kuota gratis direncanakan melalui mekanisme menonton iklan.
- Humanizer dan Metadata Cleaner direncanakan memerlukan penayangan iklan.
- Kelayakan dan kebijakan Google Ads, khususnya format rewarded ads, harus diverifikasi sebelum model monetisasi ini difinalisasi. Jangan menganggap setiap format iklan atau insentif menonton iklan otomatis memenuhi kebijakan Google.
- Profil personal dan fitur yang menyimpan preferensi pengguna memerlukan akun.

## 3. Privasi dan pengelolaan data

Prinsip yang dipilih:

- Teks yang dikirim untuk pemrosesan diproses sementara.
- Penyimpanan data pengguna diminimalkan.
- Data teks mentah tidak disimpan secara permanen tanpa kebutuhan dan dasar yang jelas.
- Sampel tulisan untuk pembuatan profil gaya dihapus setelah proses profiling, kecuali pengguna secara eksplisit memilih untuk menyimpannya.
- Informasi profil gaya disimpan sebagai data terpisah dan hanya untuk kebutuhan akun pengguna.
- Kebijakan retensi, penghapusan, keamanan, dan persetujuan perlu dirinci sebelum implementasi.

## 4. Rancangan AI Humanizer

### 4.1 Audit konsep Humanize Kit

Repository yang pernah ditinjau:
https://github.com/Branded-Mayhem-Collective-LLC/humanize-kit

Komponen/konsep yang dinilai relevan untuk diadaptasi:

- Voice Profiler
- Python Analyzer
- Template Renderer
- Humanize Skill
- Sample Profile sebagai referensi struktur profil
- Prinsip post-processing dan pemrosesan tanda baca secara selektif

Humanize Kit dibuat untuk alur kerja agent/Claude, bukan sebagai API web mandiri. Karena itu, TextLab tidak dirancang untuk menjalankan skill/plugin Claude secara langsung. Logika dan aset yang relevan perlu diadaptasi ke backend TextLab.

Komponen yang tidak perlu dibawa langsung ke produk:

- Manifest marketplace Claude
- Installer CLI
- Session hooks
- Slash commands
- Integrasi newsletter/Supabase yang tidak relevan dengan fungsi TextLab

### 4.2 Profil gaya penulisan

Rancangan onboarding profil:

1. Pengguna memberikan sekitar 3–5 sampel tulisan.
2. Sistem menganalisis kosakata, ritme kalimat, tingkat formalitas, struktur paragraf, tanda baca, dan nada.
3. Sistem membuat profil gaya awal.
4. Pengguna dapat meninjau dan mengoreksi profil.
5. Profil disimpan sebagai profil personal.
6. Profil umum dan profil kontekstual dapat dipisahkan jika kebutuhan produk mendukungnya.

Sampel tulisan mentah dihapus setelah profiling, kecuali pengguna memilih penyimpanan secara eksplisit.

### 4.3 Pipeline Humanizer

Alur konseptual:

Input → validasi dan pemeriksaan akses → pemilihan profil/gaya → analisis teks → prompt builder → model generatif → validasi keluaran → hasil dan tampilan perbandingan.

Validasi keluaran harus menjaga makna, fakta, kutipan, dan struktur penting. Sistem tidak boleh sengaja menambahkan kesalahan hanya agar tulisan terlihat lebih manusiawi.

Pilihan gaya: profil pengguna atau gaya pustaka.

## 5. Rancangan AI Text Detector

Keputusan awal:

- Bahasa utama: Bahasa Indonesia.
- Mendukung teks pendek dan panjang.
- Menggunakan model sendiri yang dilatih dari pretrained model.
- Fine-tuning menjadi pendekatan utama.
- Infrastruktur inferensi mengutamakan CPU dan model kecil.
- Granularitas hasil: tingkat kalimat.
- Kalimat diberi highlight; dokumen memiliki ringkasan hasil.
- Model belum diputuskan final.

Kandidat model yang pernah dipertimbangkan:

- IndoBERT-Lite Base P2 sebagai kandidat utama awal.
- IndoBERT Base P2 sebagai pembanding.
- XLM-R Base sebagai baseline multilingual opsional.
- mDeBERTa-v3 Base sebagai kandidat multilingual opsional.

Kandidat tersebut adalah backbone pretrained, bukan detector AI siap pakai. Semuanya masih memerlukan dataset dan fine-tuning khusus.

## 6. Riset model terdahulu

### IndoBERT-Lite Base P2

- Model berfokus pada Bahasa Indonesia.
- Arsitektur ALBERT.
- Sekitar 11,7 juta parameter menurut model card yang pernah ditinjau.
- Dipertimbangkan karena ukuran relatif kecil untuk target CPU-first.

Model card:
https://huggingface.co/indobenchmark/indobert-lite-base-p2

### IndoBERT Base P2

- Model Bahasa Indonesia dengan ukuran jauh lebih besar.
- Sekitar 124,5 juta parameter menurut model card yang pernah ditinjau.
- Digunakan sebagai pembanding kualitas terhadap IndoBERT-Lite.

Model card:
https://huggingface.co/indobenchmark/indobert-base-p2

### XLM-R Base

- Model multilingual yang pernah dipertimbangkan sebagai pembanding.
- Lebih besar dan tidak secara khusus hanya berfokus pada Bahasa Indonesia.

Model card:
https://huggingface.co/FacebookAI/xlm-roberta-base

### mDeBERTa-v3 Base

- Kandidat multilingual lain.
- Perlu memperhitungkan ukuran embedding dan kebutuhan runtime, bukan hanya jumlah parameter backbone.

Model card:
https://huggingface.co/microsoft/mdeberta-v3-base

### ONNX dan kuantisasi

ONNX Runtime menyediakan dynamic dan static quantization. Dynamic quantization pernah direkomendasikan sebagai titik awal untuk model transformer, tetapi peningkatan kecepatan CPU tidak dijamin pada setiap perangkat. Akurasi, latensi, dan RAM harus diukur ulang pada perangkat target.

Dokumentasi:
https://onnxruntime.ai/docs/performance/model-optimizations/quantization.html

### Catatan penelitian

Dalam riset terdahulu ditemukan sebuah studi tentang fine-tuning IndoBERT untuk deteksi teks AI berbahasa Indonesia. Catatan diskusi sebelumnya menyebut dataset sekitar 5.774 sampel, isu data leakage pada pembagian data yang tidak dikelompokkan berdasarkan sumber, serta hasil yang menunjukkan bahwa akurasi IndoBERT tidak otomatis mengungguli baseline SVM.

Temuan tersebut harus diperlakukan sebagai catatan riset awal dan diverifikasi kembali terhadap publikasi asli sebelum dikutip dalam laporan atau keputusan ilmiah. Pelajaran metodologis yang dipertahankan: split berdasarkan sumber/kelompok sangat penting dan akurasi tinggi tidak cukup untuk menyatakan generalisasi.

## 7. Dataset dan evaluasi

Keputusan yang kemudian ditetapkan dalam Dataset Specification & Benchmark Protocol v1.0:

- Sumber hybrid: dataset publik yang lolos audit dan dataset terkontrol.
- Tiga kelas: HUMAN, AI, MIXED.
- UNCERTAIN menjadi status di luar kelas training utama.
- Target pilot: 5.000–10.000 dokumen.
- Split utama: 70% training, 15% validation, 15% test.
- Split dilakukan berdasarkan group_id, bukan kalimat secara acak.
- Test ketat mencakup penulis yang tidak muncul di training dan sebagian generator AI yang ditahan.
- Anotasi rentang menjadi sumber informasi untuk menurunkan label kalimat.
- Evaluasi wajib mencakup Macro F1, precision/recall per kelas, FPR tulisan manusia, PR-AUC, calibration, latensi, dan RAM.
- Test challenge tidak boleh dipakai memilih model atau threshold.

Target awal komposisi dokumen: HUMAN 35%, AI 35%, MIXED 30%. Ini merupakan target perencanaan, bukan syarat untuk memanipulasi distribusi alami data.

## 8. Rancangan arsitektur inferensi detector

Alur konseptual:

Frontend → API Gateway (validasi, rate limit, kuota, request ID) → Detection Service (segmentasi kalimat dan offset) → CPU inference runtime (model dan tokenizer, opsi ONNX) → calibration dan aggregation → hasil terstruktur.

Catatan teknis:

- Simpan offset karakter yang merujuk ke teks asli.
- Perhatikan Unicode dan normalisasi agar highlight tidak bergeser.
- Pertimbangkan konteks kalimat tetangga dalam eksperimen.
- Gunakan batch inference bila relevan.
- Model tidak ditempatkan di browser pada tahap awal.
- Kegagalan inferensi tidak boleh menghabiskan kuota pengguna.
- Versi model, tokenizer, preprocessing, dan threshold harus dicatat.

Contoh struktur keluaran konseptual:

```json
{
  "model_version": "version_id",
  "language": "id",
  "document_status": "UNCERTAIN",
  "document_score": null,
  "sentences": [
    {
      "sentence_index": 0,
      "start_char": 0,
      "end_char": 42,
      "label": "AI",
      "score": 0.82,
      "status": "PREDICTED"
    }
  ]
}
```

Angka skor hanya contoh format, bukan hasil model nyata. Skor model tidak boleh disebut probabilitas terkalibrasi sebelum kalibrasi diuji. Ringkasan dokumen harus disebut skor indikasi model, bukan persentase pasti kepengarangan.

## 9. Perkiraan ukuran dan infrastruktur awal

Estimasi ukuran bobot mentah yang pernah dihitung:

- IndoBERT-Lite 11,7M parameter: sekitar 47 MB FP32 dan sekitar 12 MB secara teoritis untuk INT8.
- IndoBERT Base 124,5M parameter: sekitar 498 MB FP32 dan sekitar 125 MB secara teoritis untuk INT8.

Angka tersebut hanya estimasi ukuran bobot mentah, bukan ukuran paket model atau total RAM runtime.

Inferensi CPU awal dapat diuji pada 2–4 vCPU dan RAM 4–8 GB untuk IndoBERT-Lite, tetapi ini bukan jaminan kapasitas produksi. Training dapat menggunakan GPU cloud sementara; pembelian GPU tidak perlu dilakukan sebelum kebutuhan terbukti.

## 10. Urutan eksperimen yang pernah direncanakan

1. Majority baseline.
2. TF-IDF + Logistic Regression/SVM.
3. Fine-tuning IndoBERT-Lite.
4. Fine-tuning IndoBERT Base sebagai pembanding.
5. Uji kalimat mandiri versus kalimat dengan konteks tetangga.
6. Uji ONNX INT8 versus FP32.

Rentang konfigurasi eksploratif yang pernah dibahas, belum final:

- Input length: 128, 256, 512 token.
- Batch size: 8, 16, 32 jika memori memungkinkan.
- Learning rate: 1e-5, 2e-5, 3e-5.
- Epoch: 2–5 dengan early stopping.
- Beberapa random seed untuk mengukur variasi hasil.

Konfigurasi tersebut tidak boleh dianggap keputusan training final.

## 11. Prinsip produk dan batas klaim

Detector tidak dapat membuktikan identitas penulis hanya dari karakteristik bahasa. Hasil harus diposisikan sebagai estimasi model, bukan bukti pasti kecurangan atau kepengarangan.

Prioritas kualitas:

1. Mengurangi false positive terhadap tulisan manusia.
2. Mengukur generalisasi lintas penulis, genre, dan generator.
3. Menyediakan status tidak pasti saat bukti model tidak memadai.
4. Menjaga transparansi skor dan keterbatasan detector.

## 12. Hal yang belum dikerjakan

- Data Collection Blueprint.
- Audit dan seleksi dataset publik.
- Bank prompt dan topik eksperimen.
- Formulir partisipan dan pengumpulan tulisan manusia.
- Template editing history.
- Pedoman anotasi lengkap.
- Implementasi skema database dataset.
- Pilot dataset 200–500 dokumen.
- Training dan benchmark aktual.
- Penentuan model, threshold, panjang input, dan target latensi final.
- Implementasi aplikasi.

Dokumen ini menjadi rekaman rancangan terdahulu. Perubahan selanjutnya harus dicatat di Decision Log dan tidak menghapus konteks keputusan lama.
