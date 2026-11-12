# Lexora Active Roadmap

Terakhir diperbarui: 2026-11-12

## Cakupan produk MVP

Keputusan **DEC-001** menetapkan dua modul untuk MVP pertama:
1. AI Text Detector.
2. AI Humanizer.

DOCX Metadata Cleaner ditunda ke fase setelah MVP. Arsitektur produk tetap modular, tetapi infrastruktur untuk modul yang ditunda tidak perlu dibangun sekarang. Sistem referral dan fitur perbandingan dua teks mandiri juga tidak masuk MVP berdasarkan DEC-010.

## Status analisis kebutuhan sistem

Dokumen SYSTEM-REQUIREMENTS-ANALYSIS.md telah dibuat berdasarkan sesi elisitasi kebutuhan pemilik proyek tanggal 2026-11-12.

**Status: Draft terdokumentasi, menunggu review pemilik proyek dan validasi teknis.**

Dokumen tersebut mencatat kebutuhan untuk:
- Target utama mahasiswa dan pelajar, dengan penggunaan akademik dan umum.
- Detector dan Humanizer dengan prioritas setara, dapat digunakan mandiri atau terhubung dengan persetujuan pengguna.
- Bahasa Indonesia dan Inggris, serta dukungan terbatas untuk teks campuran.
- Input TXT, DOCX, PDF dan keluaran TXT, DOCX, PDF.
- Kategori Detector HUMAN, AI, MIXED, UNCERTAIN dan batas klaim hasil.
- Humanizer dengan gaya bawaan dan profil gaya personal.
- Guest/login, dashboard, riwayat opsional, dan kebijakan retensi.
- Sistem kredit, reward iklan, pembelian, dan refund.
- Admin Panel, RBAC, audit, dukungan tiket, notifikasi, antrean, dan pemulihan.

Dokumen ini belum mengesahkan pilihan framework, database, provider AI, payment gateway, produk iklan, deployment, target uptime, angka batas input, harga, atau rumus kredit final.

## Tahap perencanaan sistem MVP

| Urutan | Pekerjaan | Status | Kriteria keluaran |
|---:|---|---|---|
| 1 | Analisis kebutuhan sistem | Draft documented; review pending | SRS ditinjau, konflik/ketidakjelasan ditutup, requirement diberi acceptance criteria |
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

1. Minta pemilik proyek meninjau SYSTEM-REQUIREMENTS-ANALYSIS.md.
2. Catat koreksi atau keputusan tambahan tanpa menghapus keputusan sebelumnya.
3. Setelah review kebutuhan, susun user flow Detector dan Humanizer.
4. Pertahankan jalur uji prosedur sintetis dataset sesuai urutan dan gerbang yang sudah ditetapkan.
