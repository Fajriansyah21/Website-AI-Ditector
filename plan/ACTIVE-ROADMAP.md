# Lexora Active Roadmap

Terakhir diperbarui: 2026-11-12

## Fokus aktif

**Uji prosedur sintetis dataset.** Pemilihan nama Lexora, Google Stitch, dan Google Antigravity tidak mengubah urutan riset ini.

### Task aktif

| Urutan | Pengujian | Bukti yang harus dihasilkan | Status |
|---:|---|---|---|
| 1 | JSON Schema | Validator dijalankan pada fixture valid dan invalid; hasil positif/negatif tercatat | Planned |
| 2 | Sentence offset | Pengujian offset untuk tanda baca, whitespace, Unicode, newline, dan teks kosong | Planned |
| 3 | Editing history & lineage | Pengujian versi, parent-child, urutan event, dan konsistensi perubahan | Planned |
| 4 | Deduplikasi | Pengujian exact duplicate, near duplicate, dan false merge | Planned |
| 5 | Group-based split | Pengujian tidak ada group/pair/derivative tersebar lintas split | Planned |

Belum ada bukti pengujian prosedur sintetis yang dijalankan pada repositori saat roadmap ini dibuat.

## Gerbang kelulusan

Tahap ini hanya dapat ditandai **Verified** jika:

- Fixture valid diterima dan fixture invalid ditolak sesuai alasan yang diharapkan.
- Offset setiap kalimat menunjuk tepat ke substring sumber, dengan konvensi indeks yang konsisten.
- Editing history membentuk lineage yang dapat ditelusuri dan menolak referensi versi yang rusak.
- Deduplikasi mendeteksi duplikat tanpa menggabungkan teks berbeda yang hanya mirip secara wajar.
- Group-based split menjaga semua pasangan, turunan, dan kelompok terkait berada pada split yang sama.
- Pengujian dapat diulang dengan command terdokumentasi dan menghasilkan output yang dapat diperiksa.
- Kegagalan dan keterbatasan dicatat, bukan disembunyikan.

## Bukan prioritas aktif

- Membuat UI website di Stitch.
- Implementasi frontend/backend di Antigravity.
- Memilih framework, database, atau deployment.
- Mengumpulkan pilot dataset nyata.
- Training model.

Aktivitas tersebut dapat direncanakan kemudian, tetapi tidak boleh menggantikan task aktif tanpa keputusan eksplisit.

## Langkah berikutnya

Mulai dari pemeriksaan struktur repo dan spesifikasi JSON yang benar-benar tersedia. Jika spesifikasi belum ada di branch aktif, pulihkan atau tulis ulang berdasarkan keputusan yang sudah disahkan sebelum membuat test harness. Jangan menebak schema dari contoh parsial.
