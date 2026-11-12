# Lexora Active Roadmap

Terakhir diperbarui: 2026-11-12

## Cakupan produk MVP

Keputusan **DEC-001** menetapkan dua modul untuk MVP pertama:
1. AI Text Detector.
2. AI Humanizer.

DOCX Metadata Cleaner ditunda ke fase setelah MVP. Ketiga modul tetap dipertimbangkan dalam arsitektur modular jangka panjang. Keputusan ini belum menetapkan urutan implementasi Detector dan Humanizer.

## Tahap perencanaan sistem MVP

Sebelum implementasi fitur, rencanakan dan dokumentasikan:
1. Analisis sistem: tujuan, aktor, kebutuhan fungsional/nonfungsional, batasan, dan risiko.
2. Modular architecture: batas modul, tanggung jawab, dependensi, dan kontrak komunikasi.
3. User flow untuk Detector dan Humanizer.
4. System flowchart termasuk validasi, jalur sukses, dan jalur gagal.
5. Data flow, kebutuhan penyimpanan, privasi, dan retensi.
6. Evaluasi opsi teknologi berdasarkan kebutuhan yang sudah disahkan.
7. Spesifikasi MVP dan acceptance criteria.

Semua butir di atas merupakan pekerjaan perencanaan yang belum ditandai selesai.

## Fokus riset aktif: uji prosedur sintetis dataset

Fokus riset dataset yang sudah direncanakan tetap berjalan sebagai jalur kerja tersendiri. Pemilihan cakupan MVP tidak menghapus kebutuhan untuk menguji prosedur dataset sebelum pilot nyata dan training detector.

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
- Membuat UI website di Stitch sebelum analisis dan spesifikasi cukup jelas.
- Implementasi frontend/backend di Antigravity sebelum acceptance criteria tersedia.
- Memilih framework, database, atau deployment tanpa analisis kebutuhan.
- Mengumpulkan pilot dataset nyata sebelum prosedur dan sumber data siap.
- Training model sebelum gerbang dataset dipenuhi.

Aktivitas tersebut dapat direncanakan kemudian, tetapi tidak boleh menggantikan task aktif tanpa keputusan eksplisit.

## Langkah berikutnya

Mulai analisis sistem MVP untuk Detector dan Humanizer. Secara paralel, pertahankan jalur uji prosedur sintetis dataset. Periksa spesifikasi JSON yang benar-benar tersedia; jika belum ada di branch aktif, susun atau pulihkan berdasarkan keputusan yang disahkan sebelum membuat test harness. Jangan menebak schema dari contoh parsial.
