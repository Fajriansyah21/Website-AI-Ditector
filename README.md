# Website AI Detector (TextLab)

Repository ini menyimpan dokumentasi keputusan produk, riset teknis, spesifikasi dataset, dan rancangan sistem TextLab. Dokumentasi dibuat sebelum implementasi agar keputusan dan alasan teknisnya tidak hilang ketika proyek mulai dikembangkan.

> **Status proyek:** tahap analisis dan perancangan. Belum ada implementasi kode aplikasi, pengumpulan pilot aktual, atau training model final.

## Ruang lingkup produk

TextLab dirancang sebagai website dengan tiga kemampuan utama:

1. **AI Text Detector** — menganalisis teks berbahasa Indonesia dan memberikan indikasi asal-usul teks pada tingkat kalimat serta ringkasan dokumen.
2. **AI Humanizer** — membantu mengubah gaya penulisan berdasarkan profil suara pengguna atau gaya dari pustaka.
3. **DOCX Metadata Cleaner** — membersihkan metadata dokumen DOCX pada tahap awal.

## Dokumen utama

- [Rekap rancangan proyek](docs/PROJECT-RECAP.md)
- [Riset dan rancangan model detector](docs/DETECTOR-MODEL-RESEARCH.md)
- [Spesifikasi dataset dan protokol benchmark v1.0](docs/DATASET-SPECIFICATION-BENCHMARK-v1.md)
- [Data Collection Blueprint v1.0](docs/DATA-COLLECTION-BLUEPRINT-v1.md)
- [Pedoman anotasi v1.0](docs/ANNOTATION-GUIDELINES-v1.md)
- [Bank topik dan prompt v1.0](docs/PROMPT-BANK-v1.md)
- [Rancangan produk dan arsitektur sistem](docs/PRODUCT-AND-SYSTEM-DESIGN.md)
- [Log keputusan dan hal yang belum diputuskan](docs/DECISION-LOG.md)
- [Skema JSON dataset v1.0](schemas/dataset-records-v1.schema.json)
- [Draf formulir persetujuan penulis manusia](templates/human-writing-consent-form.md)
- [Template editing history](templates/editing-history.template.json)

## Prinsip proyek

- Analisis dan validasi dilakukan sebelum implementasi.
- Setiap data training harus memiliki asal-usul dan dasar penggunaan yang jelas.
- Dataset publik tidak otomatis menjadi data berlabel HUMAN; provenance dan izin penggunaan harus diperiksa.
- Hasil detector adalah estimasi model, bukan bukti pasti kepengarangan atau kecurangan.
- False positive terhadap tulisan manusia harus diperlakukan sebagai risiko utama.
- Pemrosesan data pengguna dilakukan secara sementara dan dengan penyimpanan seminimal mungkin.
- Perubahan keputusan harus dicatat, bukan diam-diam mengganti rancangan sebelumnya.
- Status rancangan, hasil eksperimen, dan implementasi harus dibedakan.

## Status tahapan

| Tahap | Status |
|---|---|
| Definisi produk awal | Dirancang |
| Audit konsep Humanize Kit | Ditinjau |
| Pemilihan kandidat model detector | Kandidat, belum final |
| Dataset Specification & Benchmark Protocol | v1.0 dirancang |
| Data Collection Blueprint | v1.0 ditulis; menunggu tinjauan dan validasi pilot |
| Kandidat dataset publik | Skrining awal; belum otomatis disetujui |
| Generator AI | Provider shortlist; model ID final belum dibekukan |
| Formulir persetujuan dan editing history | Draf |
| Skema JSON dataset | v1.0 draf, perlu diuji dengan validator dan data contoh |
| Pilot dataset 200–500 dokumen | Belum dimulai |
| Konfigurasi training final | Belum ditentukan |
| Implementasi aplikasi | Belum dimulai |

Lihat [Decision Log](docs/DECISION-LOG.md) untuk keputusan yang telah ditetapkan dan pertanyaan terbuka.
