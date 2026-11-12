# Dataset and Model Research Plan

Status: **Rencana riset; beberapa keputusan metodologis telah disepakati**  
Modul terkait: AI Text Detector  
Terakhir ditinjau: 2026-11-12

## 1. Prinsip sumber data

Rencana dataset menggunakan pendekatan hybrid:
- Sumber publik yang telah melalui audit sumber, lisensi, dan kelayakan penggunaan.
- Data terkontrol yang dikumpulkan dengan provenance dan dasar penggunaan yang jelas.

Tidak ada sumber publik yang otomatis dianggap sebagai teks HUMAN atau otomatis disetujui untuk training. Label, hak penggunaan, dan provenance harus diverifikasi.

## 2. Kategori data

Kelas utama:
- `HUMAN`
- `AI`
- `MIXED`

`UNCERTAIN` dikelola di luar kelas training utama sampai ada keputusan metodologis lebih lanjut.

Setiap record perlu memiliki provenance yang cukup untuk menelusuri sumber, proses pengumpulan, transformasi, dan anotasinya. Detail schema final belum diarsipkan sebagai keputusan dalam dokumen ini.

## 3. Target dan urutan pengumpulan

Urutan yang disepakati:
1. Menyelesaikan blueprint pengumpulan data.
2. Menjalankan pilot 200–500 dokumen.
3. Mengevaluasi prosedur dan kualitas data.
4. Merevisi prosedur berdasarkan bukti.
5. Memperluas dataset menuju target 5.000–10.000 dokumen.
6. Menyiapkan training dan eksperimen model.

Target 5.000–10.000 adalah sasaran skala dataset jangka lanjut, bukan jumlah pilot awal dan bukan klaim bahwa data tersebut sudah tersedia.

## 4. Pengujian prosedur sintetis sebelum pilot

Prioritas aktif saat ini adalah menguji mekanisme pengolahan dataset menggunakan fixture sintetis, bukan mengumpulkan data nyata atau melatih model.

Urutan:
1. JSON Schema dan struktur record.
2. Sentence offsets terhadap teks sumber.
3. Editing history, versi, dan lineage.
4. Exact dan near-duplicate detection.
5. Group-based split.

Setiap pengujian harus mencakup kasus valid, invalid, boundary, dan adversarial. Hasil harus dapat diulang dan dicatat.

## 5. Pencegahan data leakage

- Pasangan prompt-output, parafrasa, revisi, dan turunan dari sumber yang sama perlu dikelompokkan.
- Group/pair/source lineage harus dipertimbangkan sebelum split.
- Tidak boleh ada kelompok terkait yang tersebar ke train, validation, dan test.
- Split berbasis grup lebih penting daripada sekadar random split record.
- Test set harus dijaga sebagai evaluasi akhir yang tidak digunakan untuk tuning.

Skema rasio split 70/15/15 pernah dibahas sebagai kandidat, tetapi perlu disahkan bersama definisi group dan kebutuhan evaluasi sebelum dianggap final.

## 6. Audit sumber dan kandidat data

Sumber yang pernah disebut sebagai kandidat eksplorasi:
- IndoNLU.
- Leipzig Corpora.
- OSCAR.
- CC-100.
- Wikipedia dumps.
- `sansaks/dataset_indonesia`.

Daftar ini bukan daftar sumber yang telah disetujui. Setiap kandidat perlu diperiksa:
- Lisensi dan ketentuan penggunaan.
- Bahasa, domain, dan periode.
- Provenance dan cara pengumpulan.
- Kesesuaian label.
- Duplikasi dan kontaminasi.
- Kualitas serta keterwakilan.
- Batas redistribusi dan penggunaan komersial jika relevan.

## 7. Data terkontrol

Data manusia memerlukan consent dan provenance yang jelas. Data AI perlu menyimpan informasi generator/provider, model jika tersedia, prompt, waktu, dan transformasi yang relevan. Data campuran memerlukan catatan kontribusi manusia dan AI.

Detail protokol pengumpulan, kompensasi, consent, anonimisasi, dan retensi belum final.

## 8. Rencana evaluasi model

Metrik kandidat yang pernah dibahas:
- Macro F1.
- Precision/recall per kelas.
- HUMAN false positive rate.
- PR-AUC jika sesuai.
- Confusion matrix.
- Calibration.
- Latency, throughput, dan peak RAM.

Eksperimen kandidat:
- Majority baseline.
- TF-IDF + Logistic Regression/SVM.
- IndoBERT-Lite.
- IndoBERT Base.
- Sentence-only versus context-aware.
- ONNX INT8 versus FP32.

Semua model dan eksperimen tersebut masih kandidat. Tidak ada model final yang disahkan oleh dokumen ini.

## 9. Gerbang keputusan

Tidak boleh berpindah ke pilot nyata sebelum uji prosedur sintetis dan blueprint memadai. Tidak boleh memulai training serius sebelum sumber data diaudit, label dapat dipertanggungjawabkan, group split tervalidasi, dan protokol evaluasi terdokumentasi.

## 10. Hal yang belum diputuskan

- Schema final dan data dictionary.
- Definisi operasional tiap label dan panduan anotasi.
- Proporsi kelas dan domain.
- Daftar sumber publik yang disetujui.
- Protokol consent dan pengumpulan data terkontrol.
- Rasio split final dan challenge set.
- Model final, hyperparameter, dan lingkungan training.
