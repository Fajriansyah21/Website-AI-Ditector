# TextLab Project Workflow

**Status:** workflow induk / rancangan operasional  
**Tanggal:** 2026-11-09  
**Prioritas saat ini:** uji prosedur sintetis dataset  
**Catatan:** Dokumen ini mengatur alur kerja keseluruhan proyek. Dokumen ini bukan bukti bahwa fase yang tercantum telah selesai atau tervalidasi.

## 1. Tujuan dan prinsip

Workflow ini menjadi peta kerja utama TextLab dari perencanaan, riset dataset dan model, pengembangan website, pengujian, deployment, hingga pemeliharaan.

Prinsip pelaksanaan:

1. Riset dan validasi dilakukan sebelum keputusan implementasi yang bergantung pada hasil riset.
2. Setiap fase memiliki keluaran dan exit criteria yang dapat diperiksa.
3. Pekerjaan boleh berjalan paralel jika ketergantungannya tidak dilanggar.
4. Status **dirancang**, **dikerjakan**, **diimplementasikan**, dan **diverifikasi** harus dibedakan.
5. Kegagalan pengujian menjadi alasan untuk memperbaiki atau mengulang tahap terkait, bukan alasan untuk melewati gate.
6. Keputusan penting, alasan, perubahan, dan bukti pengujian dicatat di repository.
7. Privasi, provenance, lisensi, keamanan, dan risiko false positive dipertimbangkan sejak awal, bukan ditambahkan setelah produk selesai.

## 2. Peta fase proyek

| Fase | Fokus | Keluaran utama | Status saat workflow dicatat |
|---|---|---|---|
| 1. Fondasi dan perencanaan | Visi produk, kebutuhan, ruang lingkup, risiko, dan kriteria keberhasilan | Project specification dan daftar keputusan | Sebagian besar sudah dirancang; beberapa keputusan teknis masih terbuka |
| 2. Riset dan validasi dataset | Prosedur data, audit sumber, pilot, anotasi, deduplikasi, dan split | Dataset yang provenance dan kualitas labelnya dapat dipertanggungjawabkan | **Aktif; fokus langsung adalah uji prosedur sintetis** |
| 3. Eksperimen dan pengembangan model | Baseline, training, fine-tuning, evaluasi, kalibrasi, dan seleksi | Model candidate, hasil eksperimen, dan laporan evaluasi | Belum dimulai |
| 4. Perancangan produk dan sistem | User flow, arsitektur, API, database, UI/UX, keamanan, dan privasi | System design, kontrak API, dan desain layar | Sebagian rancangan konseptual tersedia; detail implementasi belum final |
| 5. Implementasi website | Frontend, backend, integrasi model, dan fitur produk | Aplikasi yang dapat dijalankan dan diuji | Belum dimulai |
| 6. Integrasi dan pengujian | Pengujian fungsi, model, integrasi, keamanan, performa, dan usability | Test report dan daftar isu | Belum dimulai |
| 7. Deployment dan peluncuran | Infrastruktur produksi, konfigurasi, domain, monitoring, dan peluncuran | Rilis aplikasi yang dapat diakses pengguna | Belum dimulai |
| 8. Pemeliharaan dan pengembangan | Monitoring, perbaikan, feedback, evaluasi model, dan iterasi fitur | Rilis perbaikan dan catatan perubahan | Belum dimulai |

Status di atas adalah snapshot dokumentasi, bukan estimasi persentase penyelesaian.

## 3. Research workflow

Jalur riset menentukan apakah data dan model memiliki dasar yang layak untuk digunakan.

1. **Literature review dan research framing**
   - Tinjau penelitian terkait AI-generated text detection, khususnya Bahasa Indonesia.
   - Rumuskan gap, pertanyaan riset, risiko, dan benchmark.
   - Verifikasi publikasi asli sebelum memakai temuan sebagai dasar klaim.

2. **Dataset specification dan collection blueprint**
   - Tetapkan kelas, unit anotasi, metadata, provenance, consent, dan target komposisi.
   - Tetapkan aturan sumber, lisensi, editing history, deduplikasi, grouping, serta split.
   - Sinkronkan blueprint, annotation guidelines, prompt bank, schema, dan database design.

3. **Synthetic procedure test**
   - Buat record sintetis untuk menguji struktur JSON dan validasi schema.
   - Uji sentence offsets terhadap teks sumber.
   - Uji editing/version lineage dan hubungan provenance.
   - Uji exact/near-duplicate detection dan grouping.
   - Uji group-based split untuk mencegah kebocoran pasangan, versi, penulis, atau sumber.
   - Catat hasil, kegagalan, dan perbaikan. Data sintetis hanya menguji prosedur, bukan membuktikan kualitas dataset nyata.

4. **Public source and rights audit**
   - Audit sumber, versi, lisensi, syarat penggunaan, provenance, privasi, dan batas redistribusi.
   - Beri status APPROVED, CONDITIONAL, atau REJECTED/PENDING sesuai bukti.
   - Jangan menganggap teks publik otomatis HUMAN atau otomatis boleh dipakai untuk training.

5. **Pilot dataset**
   - Setelah prosedur dan sumber yang digunakan siap, kumpulkan pilot 200–500 dokumen.
   - Jalankan consent dan provenance tracking untuk data terkontrol.
   - Lakukan anotasi span-level, turunkan label sentence-level, dan ukur konsistensi anotator.
   - Audit kualitas, distribusi kelas/genre, offset, deduplikasi, dan group split.

6. **Blueprint revision and scale-up decision**
   - Revisi prosedur berdasarkan temuan pilot.
   - Bekukan versi dataset dan dokumentasi hanya setelah kriteria kualitas dipenuhi.
   - Putuskan apakah layak melanjutkan pengumpulan skala besar. Jangan scale-up otomatis hanya karena jumlah pilot tercapai.

7. **Model experiments**
   - Bangun majority baseline dan baseline TF-IDF + Logistic Regression/SVM.
   - Bandingkan kandidat pretrained backbone melalui fine-tuning.
   - Uji konteks kalimat, kalibrasi, threshold, dan opsi optimasi inference.
   - Simpan konfigurasi, versi data, seed, metrik, dan artefak eksperimen.

8. **Model evaluation and selection**
   - Evaluasi Macro F1, precision/recall per kelas, HUMAN false positive rate, PR-AUC, confusion matrix, calibration, latency, throughput, dan peak RAM.
   - Gunakan validation set untuk pemilihan threshold; jangan mengoptimalkan berdasarkan test set.
   - Laporkan in-distribution dan challenge/held-out evaluation secara terpisah.
   - Pilih model berdasarkan trade-off kualitas, generalisasi, dan biaya runtime, bukan akurasi saja.

## 4. Product and system workflow

Jalur produk dapat berjalan paralel dengan riset pada bagian yang tidak bergantung pada hasil final model.

1. **Product requirements**
   - Definisikan persona, masalah pengguna, user journey, ruang lingkup MVP, dan acceptance criteria.
   - Tetapkan kebutuhan fungsional dan nonfungsional, termasuk privasi, aksesibilitas, dan keamanan.

2. **System architecture**
   - Rancang frontend, backend/API, database, inference service, autentikasi, quota, dan observability.
   - Definisikan API contract dan bentuk respons sebelum integrasi lintas komponen.
   - Tandai keputusan stack dan deployment yang masih terbuka.

3. **UI/UX design in Google Stitch**
   - Eksplorasi struktur halaman, layout, komponen, visual tokens, dan responsive behavior.
   - Dokumentasikan state normal, loading, empty, validation, error, success, dan disabled.
   - Review aksesibilitas, keterbacaan, dan konsistensi sebelum handoff.

4. **Design review and handoff**
   - Pilih desain yang disetujui.
   - Catat referensi proyek/ekspor Stitch, design rules, viewport, komponen, dan perilaku interaksi.
   - Mockup bukan bukti implementasi atau perilaku sistem.

5. **Implementation in Google Antigravity**
   - Periksa repository dan arsitektur yang ada sebelum mengubah kode.
   - Implementasikan dalam perubahan kecil dan dapat direview.
   - Bangun komponen, routes, forms, validation, states, API integration, dan responsive behavior.
   - Hindari penambahan dependency atau perubahan arsitektur tanpa alasan.

6. **Feature verification**
   - Jalankan aplikasi, test, lint, dan browser checks yang relevan.
   - Uji acceptance criteria, state, viewport, keyboard/accessibility, dan error handling.
   - Review diff, dependency, console/runtime errors, dan kemungkinan secret atau data sensitif.

7. **Record and commit**
   - Catat apa yang benar-benar diimplementasikan dan diverifikasi.
   - Pisahkan hasil yang belum diuji dari hasil yang lulus pengujian.
   - Commit dengan pesan yang jelas dan perubahan terfokus.

Peran alat:
- **Google Stitch:** desain UI/UX dan handoff desain.
- **Google Antigravity:** implementasi, debugging, menjalankan aplikasi, dan verifikasi.
- **GitHub:** source of truth untuk kode, dokumentasi, keputusan, dan bukti perubahan.

Rincian penggunaan Stitch dan Antigravity berada di [Development Workflow](DEVELOPMENT-WORKFLOW.md).

## 5. Release, deployment, and maintenance workflow

1. **System acceptance testing**
   - Jalankan functional, integration, regression, usability, dan end-to-end checks.
   - Pastikan hasil detector dan integrasi model sesuai kontrak yang disetujui.

2. **Security, privacy, and performance review**
   - Periksa autentikasi/otorisasi, input validation, rate limiting, data retention, deletion, secrets, dan logging.
   - Uji performa dan batas kapasitas pada lingkungan yang mewakili target.
   - Tinjau kembali risiko false positive dan cara hasil detector dijelaskan kepada pengguna.

3. **Production readiness**
   - Siapkan environment, konfigurasi, backup/restore, monitoring, alerting, dan prosedur rollback.
   - Pastikan kebijakan privasi, syarat layanan, dan mekanisme consent sesuai kebutuhan.
   - Verifikasi kebijakan dan kelayakan iklan sebelum fitur monetisasi berbasis iklan diaktifkan.

4. **Deployment and launch**
   - Deploy ke staging, lakukan smoke test, lalu rilis produksi setelah gate terpenuhi.
   - Catat versi aplikasi, model, tokenizer, preprocessing, threshold, dan konfigurasi rilis.
   - Siapkan mekanisme rollback.

5. **Maintenance and iteration**
   - Pantau error, latency, penggunaan resource, feedback, dan perubahan distribusi input.
   - Triage bug dan risiko; prioritaskan isu keamanan, privasi, dan kualitas hasil.
   - Evaluasi kebutuhan retraining berdasarkan data yang sah dan prosedur governance.
   - Ulangi pengujian sebelum model atau fitur baru dirilis.

## 6. Dependency and parallel-work rules

- Uji prosedur sintetis merupakan pekerjaan riset aktif dan tidak boleh digeser oleh keputusan tool desain atau implementasi.
- Audit sumber dan prosedur data harus mendahului pengumpulan pilot.
- Pilot dan evaluasi anotasi harus mendahului keputusan scale-up.
- Dataset dan split yang tervalidasi harus tersedia sebelum training final dan klaim evaluasi.
- UI exploration, design system, dan komponen frontend non-model dapat dikerjakan paralel dengan riset.
- Integrasi detector memerlukan API contract yang stabil dan model service yang siap diuji.
- Fitur Humanizer dan Metadata Cleaner dapat dirancang terpisah, tetapi prioritas dan urutan rilisnya belum ditetapkan dalam dokumen ini.
- Deployment baru dilakukan setelah acceptance, security, privacy, and production-readiness gates terpenuhi.

## 7. Exit criteria antar-fase

| Gate | Kriteria minimum untuk melanjutkan |
|---|---|
| Fondasi → dataset procedure | Scope, label, metadata, provenance, dan prosedur telah terdokumentasi |
| Synthetic test → pilot | Schema, offsets, lineage, deduplication, dan group split lulus test yang direncanakan; kegagalan kritis ditangani |
| Pilot → scale-up | Provenance, consent, anotasi, distribusi, dan leakage audit memenuhi kriteria yang ditetapkan |
| Dataset → model experiments | Dataset version, split, label quality, dan benchmark tercatat serta dapat direproduksi |
| Model experiments → candidate selection | Metrik per kelas, HUMAN false positive, calibration, generalization, dan runtime tersedia |
| Design → implementation | Desain ditinjau, user flow dan acceptance criteria jelas |
| Implementation → feature ready | Fitur berjalan, diff direview, dan test relevan benar-benar dijalankan |
| Testing → deployment | Tidak ada isu kritis terbuka; privacy/security/performance checks dan rollback plan tersedia |
| Deployment → maintenance | Monitoring, ownership, incident handling, dan version tracking tersedia |

Gate dapat menghasilkan keputusan **PASS**, **REVISE**, atau **BLOCK**. Alasan keputusan dan bukti pendukung harus dicatat.

## 8. Status langsung dan langkah berikutnya

Prioritas langsung proyek adalah **synthetic procedure test untuk dataset**. Urutan kerja terdekat:

1. Periksa dan uji JSON Schema dengan validator yang sesuai.
2. Buat fixtures sintetis yang mencakup record valid dan invalid.
3. Uji sentence offsets, termasuk teks dengan Unicode, tanda baca, dan whitespace.
4. Uji editing/version lineage dan provenance span.
5. Uji exact/near-duplicate grouping.
6. Uji group-based split dan pastikan pasangan/versi/kelompok terkait tidak bocor antar-split.
7. Jalankan test, simpan hasil aktual, perbaiki artefak, lalu ulangi test.
8. Setelah prosedur lolos, lanjutkan audit sumber publik sebelum pilot nyata.

Tidak ada pengumpulan pilot aktual atau training model yang dinyatakan selesai oleh dokumen ini.

## 9. Hal yang masih terbuka

Workflow ini tidak menetapkan secara sepihak:
- frontend framework, backend stack, database engine, dan deployment provider;
- urutan prioritas rilis Humanizer dan Metadata Cleaner;
- strategi peluncuran MVP versus peluncuran seluruh fitur;
- acceptance thresholds numerik untuk seluruh test produk;
- model detector, konfigurasi training, threshold, dan target runtime final;
- kebijakan retensi, consent, serta detail monetisasi yang masih memerlukan verifikasi.

Keputusan tersebut harus ditambahkan ke Decision Log setelah dibahas dan disetujui.

## 10. Dokumen terkait

- [Project Recap](PROJECT-RECAP.md)
- [Decision Log](DECISION-LOG.md)
- [Data Collection Blueprint](DATA-COLLECTION-BLUEPRINT-v1.md)
- [Dataset Specification and Benchmark](DATASET-SPECIFICATION-BENCHMARK-v1.md)
- [Annotation Guidelines](ANNOTATION-GUIDELINES-v1.md)
- [Development Workflow: Stitch + Antigravity](DEVELOPMENT-WORKFLOW.md)
- [Product and System Design](PRODUCT-AND-SYSTEM-DESIGN.md)

---

**Aturan pemeliharaan:** setiap perubahan besar pada urutan, ketergantungan, atau exit criteria harus disertai alasan dan dicatat dalam Decision Log. Perbarui status fase berdasarkan bukti, bukan perkiraan.
