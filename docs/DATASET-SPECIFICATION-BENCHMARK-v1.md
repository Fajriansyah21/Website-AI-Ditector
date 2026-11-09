# Dataset Specification & Benchmark Protocol v1.0

**Proyek:** TextLab  
**Status:** rancangan ditetapkan untuk tahap pilot  
**Bahasa:** Bahasa Indonesia  
**Target awal:** 5.000–10.000 dokumen

## 1. Tujuan

Membangun dataset untuk melatih dan mengevaluasi detector teks AI Bahasa Indonesia pada tingkat kalimat, dengan ringkasan dokumen.

Model harus mempelajari indikasi asal-usul teks, bukan sekadar membedakan teks formal dari teks informal.

## 2. Keputusan final

| Aspek | Keputusan |
|---|---|
| Strategi sumber | Hybrid: dataset publik terverifikasi + dataset terkontrol |
| Unit data | Dokumen dan kalimat |
| Kelas model | HUMAN, AI, MIXED |
| Status di luar kelas | UNCERTAIN |
| Target pilot | 5.000–10.000 dokumen |
| Split utama | 70% train, 15% validation, 15% test |
| Pencegahan leakage | Group-based split |
| Test ketat | Penulis dan sebagian generator AI ditahan |
| Model kandidat | IndoBERT-Lite sebagai kandidat awal, belum final |

## 3. Target komposisi dokumen

Target perencanaan awal:
- HUMAN: 35%.
- AI: 35%.
- MIXED: 30%.

| Kelas | 5.000 dokumen | 10.000 dokumen |
|---|---:|---:|
| HUMAN | 1.750 | 3.500 |
| AI | 1.750 | 3.500 |
| MIXED | 1.500 | 3.000 |

Ini target komposisi, bukan alasan untuk mengubah label atau menyembunyikan distribusi aktual. Distribusi kelas pada tingkat kalimat harus dilaporkan secara terpisah.

## 4. Genre

Target komposisi genre awal:

| Genre | Target |
|---|---:|
| Akademik dan pendidikan | 30% |
| Artikel dan informatif | 20% |
| Opini dan argumentasi | 15% |
| Naratif dan pengalaman pribadi | 15% |
| Instruksional dan teknis | 10% |
| Kasual dan percakapan | 10% |

Genre digunakan untuk analisis subgroup. Jangan sampai satu genre hanya berisi satu kelas.

## 5. Sumber data

### 5.1 Dataset publik

Terima hanya setelah audit:
- Sumber asli dapat diidentifikasi.
- Bahasa dan genre dapat diverifikasi.
- Label asal-usul memiliki dasar yang dapat diperiksa.
- Lisensi mengizinkan penggunaan yang direncanakan.
- Tidak ada data pribadi yang tidak diperlukan.
- Tidak ada indikasi label dibuat dari prediksi detector lain.
- Hubungan dokumen asli dan turunannya dapat dikenali.

Data yang belum lolos audit berstatus PENDING_REVIEW dan tidak masuk training.

### 5.2 Dataset terkontrol

Kumpulkan tulisan manusia dengan persetujuan dan prosedur yang jelas. Buat teks AI dengan beberapa generator, prompt, topik, panjang, dan tingkat formalitas.

Untuk setiap eksperimen, simpan:
- Prompt dan prompt_id.
- Generator dan versi model.
- Tanggal generasi.
- Konfigurasi generasi jika tersedia.
- Teks asli.
- Versi hasil penyuntingan.
- Riwayat perubahan.
- Identitas kelompok/pasangan.

Teks manusia dan AI yang dibuat dari topik/instruksi eksperimen sama diberi pair_id. Kelompok yang saling berkaitan tidak boleh tersebar antar-split.

## 6. Definisi label

| Label | Definisi |
|---|---|
| HUMAN | Kalimat ditulis manusia tanpa kontribusi AI substantif pada kalimat tersebut |
| AI | Kalimat dihasilkan AI tanpa penyuntingan manusia yang substantif |
| MIXED | Kalimat memuat kontribusi substantif manusia dan AI |
| UNCERTAIN | Asal-usul tidak cukup diketahui atau anotator tidak dapat memutuskan berdasarkan bukti |

UNCERTAIN adalah status kualitas data dan status hasil prediksi, bukan kelas training utama.

Koreksi ejaan/tanda baca mekanis tidak mengubah label AI jika tidak mengubah substansi. Perubahan struktur atau substansi yang melibatkan kontribusi manusia dapat dikategorikan MIXED. Penulisan ulang manusia secara menyeluruh tidak otomatis MIXED; kebijakan harus membedakan inspirasi dari kontribusi aktual pada teks akhir.

## 7. Anotasi dua tingkat

### Tingkat 1: span-level

Catat rentang teks yang diketahui berasal dari manusia atau AI jika provenance tersedia.

### Tingkat 2: sentence-level

Turunkan label:
- Semua kontribusi substantif manusia → HUMAN.
- Semua kontribusi substantif AI → AI.
- Kontribusi substantif keduanya → MIXED.
- Bukti tidak cukup → UNCERTAIN.

Simpan offset karakter yang merujuk ke teks asli dan gunakan konvensi indeks yang konsisten.

## 8. Kualitas anotasi

- Minimal dua anotator pada subset pemeriksaan kualitas.
- Seluruh perselisihan diperiksa.
- Gunakan adjudicator.
- Catat alasan perubahan label.
- Hitung Cohen's Kappa atau ukuran kesepakatan kategoris yang sesuai.
- Target awal kesepakatan: 0,80 pada subset audit. Ini target operasional, bukan bukti kebenaran otomatis.

Jangan memaksa kasus ambigu menjadi salah satu kelas.

## 9. Metadata wajib

Dokumen:
- document_id
- group_id
- pair_id bila ada
- text
- language
- genre
- document_label
- source_type
- source_reference
- license_status
- generator_name/version bila ada
- prompt_id bila ada
- editing_history
- review_status

Kalimat:
- sentence_id
- document_id
- sentence_index
- start_char
- end_char
- text
- label
- annotation_status
- annotator_count

Data identitas partisipan disimpan terpisah dan diminimalkan.

## 10. Split

Split utama:
- 70% training.
- 15% validation.
- 15% test.

Semua split dilakukan berdasarkan group_id, bukan kalimat secara acak.

Kelompok yang harus tetap satu split:
- Dokumen asli dan versi editannya.
- Pasangan manusia-AI dari satu eksperimen.
- Salinan, parafrasa, atau turunan sumber yang sama.
- Dokumen dari satu rangkaian eksperimen yang dapat membocorkan template/prompt.

### Test ketat

- Penulis pada test ketat tidak boleh muncul pada training/validation.
- Sebagian generator AI ditahan sepenuhnya dari training/validation.
- Test in-distribution menguji data independen dari distribusi yang dikenal.
- Test challenge menguji sumber, penulis, dan generator yang ditahan.

Contoh komposisi test untuk dataset 10.000:
- 7.000 train.
- 1.500 validation.
- 1.000 test in-distribution.
- 500 test challenge.

Untuk dataset 5.000:
- 3.500 train.
- 750 validation.
- 500 test in-distribution.
- 250 test challenge.

Persentase tersebut dihitung dari dokumen independen/kelompok. Pastikan pembagian generator dan penulis benar-benar memenuhi tujuan holdout.

## 11. Deduplikasi dan leakage control

Sebelum split:
1. Simpan raw text dan hash.
2. Normalisasi salinan untuk keperluan perbandingan tanpa mengganti teks asli.
3. Cari exact duplicates.
4. Cari near-duplicates/parafrasa yang terkait.
5. Identifikasi versi turunan dan pasangan eksperimen.
6. Gabungkan hubungan tersebut ke group_id.
7. Baru lakukan split berdasarkan grup.

Jangan mengandalkan random sentence split. Jangan membiarkan teks asli berada di train dan hasil editannya berada di test.

## 12. Benchmark

Urutan:
- E0: majority-class baseline.
- E1: TF-IDF + Logistic Regression.
- E2: TF-IDF + SVM.
- E3: IndoBERT-Lite fine-tuning.
- E4: IndoBERT Base fine-tuning.
- E5: sentence-alone versus sentence + neighboring context.
- E6: ONNX INT8 versus FP32.

Gunakan split dan preprocessing konsisten. Jalankan beberapa random seed untuk eksperimen utama.

### Metrik wajib

- Macro F1.
- Precision/recall per kelas.
- FPR untuk HUMAN.
- PR-AUC.
- Confusion matrix.
- Calibration.
- Latensi.
- Throughput.
- Peak RAM.

Laporkan per teks pendek/panjang, genre, tipe penyuntingan, penulis holdout, dan generator holdout.

## 13. Ketidakpastian dan agregasi

Model menghasilkan skor untuk HUMAN, AI, dan MIXED. Aturan abstention menghasilkan UNCERTAIN ketika skor atau bukti tidak cukup.

Threshold dipilih pada validation set, bukan test set.

Skor dokumen disebut skor indikasi model. Jangan menyebutnya persentase pasti kepengarangan.

## 14. Risiko dan mitigasi

| Risiko | Mitigasi |
|---|---|
| Data leakage | Group split dan deduplikasi sebelum split |
| Label salah | Provenance dan anotasi dua tingkat |
| Bias genre | Variasi genre dan evaluasi per genre |
| Bias generator | Holdout generator |
| Bias penulis | Holdout penulis |
| Data publik tidak sah | Audit lisensi dan sumber |
| Teks pendek/ambigu | UNCERTAIN dan evaluasi subgroup |
| Overconfidence | Kalibrasi dan abstention |

## 15. Kriteria kelulusan pilot

- Semua dokumen memiliki provenance.
- Semua sumber training lolos audit lisensi.
- Label mengikuti guideline.
- Kualitas anotasi diperiksa.
- Duplikasi dan dokumen turunan telah diperiksa.
- Split bebas leakage berdasarkan group_id.
- Penulis dan generator holdout benar-benar terpisah.
- Test challenge terkunci dan tidak dipakai memilih model.
- Dataset card dan manifest tersedia.

## 16. Catatan

Target 5.000–10.000 dokumen adalah sasaran pengumpulan, bukan jaminan kecukupan statistik atau kesiapan produksi. Sebelum memperbesar pengumpulan, uji prosedur pada pilot 200–500 dokumen.

Model detector hanya memberikan estimasi berdasarkan pola yang dipelajari. Model tidak membuktikan identitas penulis atau kecurangan.
