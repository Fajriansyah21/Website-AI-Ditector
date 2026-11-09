# TextLab Decision Log

Dokumen ini mencatat keputusan yang telah dibuat, statusnya, dan hal yang masih terbuka. Perubahan keputusan harus ditambahkan sebagai entri baru; jangan menghapus keputusan lama tanpa catatan.

## Keputusan yang ditetapkan

| ID | Area | Keputusan | Status |
|---|---|---|---|
| D-001 | Produk | Nama kerja produk: TextLab | Ditetapkan |
| D-002 | Detector | Bahasa utama Bahasa Indonesia | Ditetapkan |
| D-003 | Detector | Mendukung teks pendek dan panjang | Ditetapkan |
| D-004 | Detector | Model sendiri melalui fine-tuning pretrained backbone | Ditetapkan sebagai pendekatan |
| D-005 | Detector | CPU-first dan model kecil menjadi prioritas | Ditetapkan sebagai prioritas |
| D-006 | Detector | Highlight tingkat kalimat | Ditetapkan |
| D-007 | Detector | Kandidat awal IndoBERT-Lite; belum final | Kandidat |
| D-008 | Humanizer | Gaya dari profil pengguna atau pustaka | Ditetapkan |
| D-009 | Humanizer | Profil personal membutuhkan akun | Ditetapkan |
| D-010 | Humanizer | Gunakan konsep Humanize Kit yang relevan, adaptasi ke backend TextLab | Ditetapkan |
| D-011 | Metadata Cleaner | Format awal DOCX | Ditetapkan |
| D-012 | Akses | Detector dapat digunakan tanpa login | Ditetapkan |
| D-013 | Akses | Kuota detector gratis 3 penggunaan per hari | Ditetapkan sebagai rancangan |
| D-014 | Monetisasi | Penggunaan tambahan detector melalui iklan | Menunggu verifikasi kebijakan |
| D-015 | Monetisasi | Humanizer dan Metadata Cleaner menggunakan iklan | Menunggu verifikasi kebijakan |
| D-016 | Privasi | Pemrosesan sementara dan penyimpanan minimal | Prinsip ditetapkan |
| D-017 | Dataset | Sumber hybrid: publik terverifikasi dan terkontrol | Ditetapkan |
| D-018 | Dataset | Tiga kelas: HUMAN, AI, MIXED | Ditetapkan |
| D-019 | Dataset | UNCERTAIN di luar kelas training utama | Ditetapkan |
| D-020 | Dataset | Target pilot 5.000–10.000 dokumen | Ditetapkan sebagai target |
| D-021 | Dataset | Split utama 70/15/15 berbasis group_id | Ditetapkan |
| D-022 | Dataset | Test ketat lintas penulis dan sebagian generator AI | Ditetapkan |
| D-023 | Dataset | Anotasi span-level diturunkan menjadi sentence-level | Ditetapkan |
| D-024 | Dataset | Pilot awal 200–500 dokumen sebelum skala besar | Langkah yang disepakati |

## Entri lanjutan

| ID | Tanggal | Area | Keputusan / perubahan | Alasan dan konsekuensi | Status |
|---|---|---|---|---|---|
| D-025 | 2026-11-09 | Dokumentasi | Membuat Data Collection Blueprint v1.0 dan artefak pendukung tanpa memulai pengumpulan data aktual | Menurunkan spesifikasi dataset menjadi proses operasional yang dapat diuji sebelum skala besar | Draf tersedia; menunggu tinjauan |
| D-026 | 2026-11-09 | Sumber publik | Dataset publik tidak diberi label HUMAN hanya karena teksnya tersedia publik atau berada dalam korpus bahasa | Provenance, riwayat AI, dan hak penggunaan bisa tidak diketahui; mencegah label palsu dan risiko penggunaan data | Prinsip operasional ditetapkan |
| D-027 | 2026-11-09 | Audit lisensi | Kandidat sumber publik dicatat sebagai CONDITIONAL/PENDING sampai audit versi dan sumbernya selesai | Lisensi repositori tidak selalu menyelesaikan hak atas materi sumber di dalam korpus | Ditetapkan |
| D-028 | 2026-11-09 | Generator | Provider OpenAI, Google Gemini, dan DeepSeek menjadi shortlist awal; model ID persis belum dibekukan | Mengurangi ketergantungan pada satu provider, sambil mengakui model dan alias dapat berubah | Kandidat, belum final |
| D-029 | 2026-11-09 | Pilot | Pilot harus memvalidasi provenance, consent, anotasi, deduplikasi, offset, dan split sebelum skala 5.000–10.000 | Jumlah data tidak dapat menggantikan kualitas label dan integritas evaluasi | Ditetapkan sebagai gate |
| D-030 | 2026-11-09 | Skema | Menambahkan JSON Schema v1.0 untuk record dokumen dan kalimat | Membuat kontrak data yang dapat divalidasi otomatis; skema masih perlu diuji dengan validator dan data contoh | Draf, belum tervalidasi |
| D-031 | 2026-11-09 | Database | Menambahkan rancangan logis tabel dokumen, anotasi kalimat, provenance span, versi edit, audit sumber, generator run, prompt, dan event anotasi | Memisahkan data teks, metadata, riwayat, dan informasi identitas; belum memilih engine atau membuat migration | Draf |
| D-032 | 2026-11-09 | JSON | Menambahkan contoh record sintetis untuk memperjelas format ekspor | Membantu menguji kontrak data tanpa memakai teks partisipan; perlu diuji dengan validator JSON Schema | Draf |
| D-033 | 2026-11-09 | Development | Google Stitch digunakan untuk desain UI/UX dan Google Antigravity untuk implementasi, debugging, dan verifikasi website | Memisahkan sumber desain dari lingkungan implementasi; mockup tidak dianggap fitur selesai sebelum diuji | Toolchain dipilih; workflow perlu diuji pada fitur pertama |

## Keputusan yang belum final

1. Model detector final.
2. Konfigurasi training final.
3. Panjang maksimum input dan strategi konteks.
4. Threshold dan aturan abstention.
5. Aturan agregasi skor dokumen.
6. Target latensi dan kapasitas server.
7. Dataset publik yang benar-benar lolos audit sumber, lisensi, privasi, dan provenance.
8. Model ID generator final, versi, parameter, dan generator yang akan di-holdout.
9. Bank prompt final setelah ditinjau dan diuji.
10. Formulir persetujuan final, jalur penarikan, retensi, dan tinjauan yang diperlukan.
11. Skema JSON yang lolos pengujian validator dan contoh data.
12. Prosedur anotasi yang lolos uji kesepakatan pada pilot.
13. Aturan deduplikasi dan grouping yang lolos audit lintas split.
14. Detail retensi dan keamanan data.
15. Format dan ketentuan iklan yang diizinkan.
16. Detail metadata DOCX yang akan dibersihkan.
17. Frontend framework, backend stack, database engine, dan deployment target.
18. Acceptance criteria serta test matrix UI setelah screen pertama dirancang.

## Urutan kerja

1. Tinjau Data Collection Blueprint dan artefak terkait.
2. Audit sumber dataset dan ketentuan penggunaannya sebelum mengunduh atau memasukkannya ke training.
3. Siapkan serta uji validator skema, offset, dan duplicate/group checks menggunakan contoh sintetis.
4. Jalankan pilot 200–500 dokumen dengan persetujuan dan provenance terdokumentasi.
5. Audit label, kesepakatan anotator, dan kebocoran antar-split.
6. Revisi blueprint dan catat keputusan baru.
7. Baru pertimbangkan pengumpulan skala besar.
8. Tetapkan konfigurasi training setelah kualitas data dan benchmark awal diperiksa.

## Dokumen pendukung

- [Data Collection Blueprint v1.0](DATA-COLLECTION-BLUEPRINT-v1.md)
- [Annotation Guidelines v1.0](ANNOTATION-GUIDELINES-v1.md)
- [Prompt Bank v1.0](PROMPT-BANK-v1.md)
- [Logical Database Schema v1.0](DATABASE-SCHEMA-v1.md)
- [Development Workflow: Stitch + Antigravity](DEVELOPMENT-WORKFLOW.md)
- [Human Writing Consent Form](../templates/human-writing-consent-form.md)
- [Editing History Template](../templates/editing-history.template.json)
- [Dataset JSON Schema](../schemas/dataset-records-v1.schema.json)
- [Synthetic Example JSON Record](../schemas/example-document-record-v1.json)

## Catatan status

Blueprint, prompt bank, consent form, editing history, database schema, JSON Schema, dan development workflow adalah artefak rancangan. Pembuatan file tidak berarti sumber publik telah disetujui, partisipan telah direkrut, data telah dikumpulkan, skema telah lolos validator, fitur website telah diimplementasikan, atau model telah dilatih.

| D-034 | 2026-11-09 | Project workflow | Menambahkan workflow induk dari fondasi proyek hingga maintenance, dengan jalur riset, produk, dan release yang saling terhubung serta exit criteria per gate | Menjaga urutan riset tetap jelas, memungkinkan pekerjaan paralel yang aman, dan membedakan rancangan dari hasil yang benar-benar diverifikasi | Workflow terdokumentasi; prioritas aktif tetap uji prosedur sintetis dataset |
