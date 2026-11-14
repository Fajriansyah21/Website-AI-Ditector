# Lexora Active Roadmap

Terakhir diperbarui: 2026-11-14

## Cakupan produk MVP

Keputusan **DEC-001** menetapkan dua modul untuk MVP pertama:
1. AI Text Detector.
2. AI Humanizer.

DOCX Metadata Cleaner ditunda ke fase setelah MVP. Arsitektur produk tetap modular, tetapi infrastruktur untuk modul yang ditunda tidak perlu dibangun sekarang. Sistem referral dan fitur perbandingan dua teks mandiri juga tidak masuk MVP berdasarkan DEC-010.

## Status analisis kebutuhan sistem

SRS tetap berstatus **Draft** dan elisitasi kebutuhan masih berlangsung. Rekap terperinci berada di [Requirements Elicitation Status](REQUIREMENTS-ELICITATION-STATUS.md).

Posisi dokumentasi pada 2026-11-14:
- 137 catatan keputusan: 135 Accepted dan 2 Superseded (DEC-018 dan DEC-019).
- 188 baris kebutuhan fungsional dalam SRS, termasuk 117 requirement Humanizer dan 11 requirement Detector.
- Humanizer telah dibahas mendalam, tetapi masih memiliki isu terbuka dan belum memiliki acceptance criteria lengkap.
- Detector baru memiliki fondasi kebutuhan; elisitasi belum menyeluruh.
- Akun, dashboard, kredit, pembayaran, dukungan, Admin, dan operasional sudah memiliki fondasi, tetapi detail kebutuhan belum lengkap.

SRS belum siap menjadi dasar System Design. Elisitasi seluruh modul, audit konflik/dependensi, dan acceptance criteria harus diselesaikan terlebih dahulu. Ringkasan status bukan pengganti SRS atau Decision Log.

## Tahap perencanaan sistem MVP

| Urutan | Pekerjaan | Status | Kriteria keluaran |
|---:|---|---|---|
| 1 | Elisitasi kebutuhan, audit konsistensi, dan acceptance criteria | In progress; Humanizer mendalam, Detector dan modul pendukung belum lengkap | Seluruh area kebutuhan ditinjau, konflik/ketidakjelasan ditutup, setiap requirement memiliki acceptance criteria |
| 2 | User flow Detector dan Humanizer | Planned | Alur normal, validasi, persetujuan handoff, pembatalan, dan kegagalan |
| 3 | Modular architecture | Planned | Batas modul, tanggung jawab, dependensi, dan kontrak komunikasi |
| 4 | System flowchart | Planned | Diagram jalur sukses, gagal, retry, refund, dan antrean |
| 5 | Data flow dan data lifecycle | Planned | Klasifikasi data, alur penyimpanan, consent, retensi, penghapusan |
| 6 | Evaluasi opsi teknologi | Planned | Perbandingan trade-off framework, backend, database, AI, payment, storage, queue, dan deployment |
| 7 | Spesifikasi MVP dan acceptance criteria | Planned | Spesifikasi yang dapat diuji dan menjadi dasar implementasi |

Jangan mulai implementasi aplikasi hanya karena SRS telah dibuat. Kebutuhan perlu ditinjau dan diterjemahkan menjadi alur, desain, serta kriteria penerimaan yang dapat diverifikasi.

## Fokus riset aktif: uji prosedur sintetis dataset

Jalur riset dataset tetap berjalan terpisah dari analisis sistem produk. Pengembangan requirement produk tidak membatalkan gerbang metodologi dataset sebelum pilot nyata dan training detector.

### Task pengujian

| Urutan | Pengujian | Bukti yang harus dihasilkan | Status |
|---:|---|---|---|
| 1 | JSON Schema | Validator dijalankan pada fixture valid dan invalid; hasil positif/negatif tercatat | Planned |
| 2 | Sentence offset | Pengujian offset untuk tanda baca, whitespace, Unicode, newline, dan teks kosong | Planned |
| 3 | Editing history & lineage | Pengujian versi, parent-child, urutan event, dan konsistensi perubahan | Planned |
| 4 | Deduplikasi | Pengujian exact duplicate, near duplicate, dan false merge | Planned |
| 5 | Group-based split | Pengujian tidak ada group/pair/derivative tersebar lintas split | Planned |

Belum ada bukti pengujian prosedur sintetis yang dijalankan pada repositori saat roadmap ini dibuat.

## Gerbang kelulusan pengujian sintetis

Tahap ini hanya dapat ditandai **Verified** jika:
- Fixture valid diterima dan fixture invalid ditolak sesuai alasan yang diharapkan.
- Offset setiap kalimat menunjuk tepat ke substring sumber, dengan konvensi indeks yang konsisten.
- Editing history membentuk lineage yang dapat ditelusuri dan menolak referensi versi yang rusak.
- Deduplikasi mendeteksi duplikat tanpa menggabungkan teks berbeda yang hanya mirip secara wajar.
- Group-based split menjaga semua pasangan, turunan, dan kelompok terkait berada pada split yang sama.
- Pengujian dapat diulang dengan command terdokumentasi dan menghasilkan output yang dapat diperiksa.
- Kegagalan dan keterbatasan dicatat, bukan disembunyikan.

## Bukan prioritas implementasi saat ini

- Implementasi DOCX Metadata Cleaner dalam MVP.
- Implementasi referral atau fitur pembanding teks mandiri.
- Membuat UI website di Stitch sebelum user flow dan spesifikasi cukup jelas.
- Implementasi frontend/backend di Antigravity sebelum acceptance criteria tersedia.
- Memilih framework, database, provider AI, payment gateway, produk iklan, atau deployment tanpa analisis kebutuhan dan validasi sumber.
- Mengumpulkan pilot dataset nyata sebelum prosedur dan sumber data siap.
- Training model sebelum gerbang dataset dipenuhi.

## Langkah berikutnya

1. Lanjutkan elisitasi pada gap Humanizer yang benar-benar belum dibahas; lihat Requirements Elicitation Status.
2. Lengkapi elisitasi AI Text Detector.
3. Lengkapi akun, dashboard, kredit, transaksi, pembayaran, Admin, dukungan, dan operasional.
4. Audit konflik/dependensi dan susun acceptance criteria sebelum review akhir SRS.
5. Setelah gerbang kebutuhan terpenuhi dan pemilik proyek meninjau SRS, susun user flow Detector dan Humanizer.
6. Pertahankan jalur uji prosedur sintetis dataset sesuai urutan dan gerbang yang sudah ditetapkan.
