# Lexora Project Operating Workflow

Status: **aturan kerja proyek**  
Berlaku untuk: riset dataset, eksperimen model, desain produk, dan implementasi aplikasi.

## A. Hirarki sumber kebenaran

1. Instruksi proyek: `AGENTS.md`.
2. Keputusan yang disahkan: dokumen decision log.
3. Spesifikasi aktif: dokumen spesifikasi pada tahap terkait.
4. Rencana kerja: roadmap dan task plan.
5. Hasil pengujian: fixture, log, laporan, dan artefak yang dapat diulang.
6. Percakapan: konteks kerja, bukan pengganti dokumentasi yang sudah disahkan.

[Requirements Elicitation Status](REQUIREMENTS-ELICITATION-STATUS.md) adalah ringkasan navigasi dan kemajuan, bukan sumber requirement atau keputusan. Gunakan SRS dan Decision Log untuk isi yang mengikat.

Jika dua dokumen bertentangan, jangan memilih diam-diam. Catat konflik, tentukan dokumen yang lebih baru/berwenang, lalu minta keputusan jika dampaknya substantif.

## B. Siklus setiap task

### 1. Orientasi
- Periksa branch dan working tree.
- Baca instruksi proyek dan dokumen terkait.
- Periksa status task terakhir dan perubahan yang belum diverifikasi.
- Pastikan tidak ada task aktif yang tertinggal.

### 2. Task brief
Sebelum eksekusi, tulis:
- Tujuan.
- Latar belakang dan alasan.
- Scope in / scope out.
- Input dan output yang diharapkan.
- Acceptance criteria.
- Risiko dan dependensi.

Untuk task kecil, brief boleh berupa checklist singkat. Untuk task multi-langkah, simpan di `plan/tasks/`.

### 3. Plan-first
- Pecah task menjadi langkah berurutan.
- Tentukan file yang akan dibaca/diubah.
- Tentukan tes atau bukti yang akan membuktikan hasil.
- Jangan mulai implementasi sebelum rancangan cukup jelas.
- Jika muncul temuan baru yang mengubah scope, hentikan dan revisi plan terlebih dahulu.

### 4. Execute
- Kerjakan satu unit perubahan dalam satu waktu.
- Hindari perubahan yang tidak terkait.
- Pertahankan keputusan lama sampai perubahan baru disetujui.
- Gunakan data sintetis untuk menguji prosedur sebelum memakai data nyata.

### 5. Verify
- Jalankan tes yang relevan, bukan sekadar membaca kode.
- Simpan output penting dan command yang dijalankan.
- Uji kasus normal, invalid, boundary, dan adversarial.
- Periksa ulang hasil pada lingkungan target bila memungkinkan.
- Jika tes tidak dapat dijalankan, tulis alasan dan statusnya sebagai **belum diverifikasi**.

### 6. Review
Periksa:
- Apakah acceptance criteria terpenuhi?
- Apakah ada data leakage atau asumsi yang belum dibuktikan?
- Apakah perubahan merusak kompatibilitas atau keputusan lain?
- Apakah dokumentasi dan status sesuai dengan kenyataan?
- Apakah ada dependency, script, atau perubahan permission yang tidak diperlukan?

### 7. Closeout
- Perbarui roadmap, decision log, dan dokumentasi yang terdampak.
- Simpan hasil eksperimen secara terpisah dari interpretasi.
- Baca ulang file yang diubah dari GitHub.
- Laporkan commit, file, tes yang benar-benar dijalankan, hasil, dan risiko tersisa.

## C. Status yang boleh digunakan

| Status | Makna |
|---|---|
| Proposed | Usulan, belum disetujui |
| Planned | Rencana disetujui, belum dijalankan |
| In progress | Sedang dikerjakan |
| Implemented | Perubahan sudah dibuat |
| Verified | Kriteria verifikasi telah dijalankan dan lolos |
| Blocked | Terhambat oleh dependensi/masalah |
| Deferred | Sengaja ditunda |
| Rejected | Tidak dipilih, alasan dicatat |

Jangan menyamakan `Implemented` dengan `Verified`.

## D. Alur tahap proyek

```text
Research & Specification
        |
        v
Synthetic Procedure Tests
        |
        v
Public Source / License Audit
        |
        v
Pilot Dataset (200-500 documents)
        |
        v
Annotation Quality Review
        |
        v
Dataset Freeze & Dataset Card
        |
        v
Training Preparation
        |
        v
Baseline and Model Experiments
        |
        v
Product Implementation
```

Urutan ini adalah gerbang kerja, bukan janji bahwa semua tahap otomatis lolos. Setiap tahap memerlukan bukti dan keputusan lanjut.

## E. Peran tools

| Tool | Tanggung jawab |
|---|---|
| GitHub | Source of truth, versioning, decision history, test artifacts |
| Google Stitch | Eksplorasi UI/UX dan handoff desain |
| Google Antigravity | Implementasi, debugging, testing, review diff |
| Skills | Prosedur spesifik yang dapat dipanggil sesuai konteks |

Jangan memakai tool desain untuk menggantikan riset dataset, atau tool coding untuk mengesahkan keputusan metodologis tanpa bukti.

## F. Format laporan akhir task

1. **Hasil**: apa yang berubah.
2. **Bukti**: file, commit, command, dan output tes.
3. **Status**: implemented / verified / blocked.
4. **Risiko**: hal yang belum dipastikan.
5. **Next step**: satu langkah paling logis.
