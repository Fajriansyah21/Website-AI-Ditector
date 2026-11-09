# Prompt Bank v1.0

**Status:** Proposed for pilot. Prompts should be versioned; do not edit a prompt in place after generation has started.  
**Language:** Indonesian.  
**Safety:** Use ordinary, non-sensitive topics. No request for private disclosures, real-person imitation, fabricated citations, or deliberate errors.

## How to use

- Each row is a prompt template. Replace `{TOPIC}` with one topic-bank item.
- Give the same task intent and topic to human and AI participants wherever feasible.
- Record the exact rendered prompt and `prompt_id`, not only the template.
- Use one prompt version for a batch; if wording changes, create a new version.
- Ask AI systems to write naturally, but never to deliberately insert mistakes or evade detectors.
- Save raw outputs before any edits.

## Topic bank

| topic_id | Genre | Topic | Notes |
|---|---|---|---|
| TOP-001 | Education | Cara mahasiswa membagi waktu antara kuliah, tugas, dan istirahat | No personal disclosure required |
| TOP-002 | Education | Manfaat dan keterbatasan belajar kelompok | Balanced arguments |
| TOP-003 | Informative | Cara kerja pembayaran digital dalam transaksi sehari-hari | Avoid financial advice |
| TOP-004 | Informative | Mengapa sampah perlu dipilah sebelum didaur ulang | General explanatory |
| TOP-005 | Opinion | Apakah tugas kelompok sebaiknya memiliki penilaian individu? | Ask for counterargument |
| TOP-006 | Opinion | Perlukah sekolah dan kampus membatasi penggunaan ponsel saat belajar? | Avoid absolute framing |
| TOP-007 | Narrative | Cerita fiksi tentang seseorang yang terlambat naik kendaraan umum | Clearly fictional |
| TOP-008 | Narrative | Cerita fiksi tentang persiapan acara kelas sederhana | No real identifying details |
| TOP-009 | Instructional | Langkah menyusun daftar tugas mingguan yang realistis | Ordinary safe process |
| TOP-010 | Instructional | Cara merawat tanaman hias dalam pot untuk pemula | Non-specialist guidance |
| TOP-011 | Technical | Penjelasan sederhana tentang fungsi HTML, CSS, dan JavaScript | Introductory technical |
| TOP-012 | Technical | Perbedaan basis data relasional dan spreadsheet | Introductory technical |
| TOP-013 | Casual | Pesan singkat untuk mengajak teman belajar bersama | No names/phone numbers |
| TOP-014 | Casual | Komentar forum yang menjelaskan cara mengatasi kebingungan saat belajar konsep baru | General, not personal diagnosis |
| TOP-015 | Education | Mengapa sumber rujukan perlu diperiksa sebelum digunakan dalam tugas | Do not invent citations |
| TOP-016 | Informative | Dampak ruang terbuka hijau bagi lingkungan perkotaan | Explain benefits and trade-offs |
| TOP-017 | Opinion | Apakah transportasi umum perlu lebih diprioritaskan daripada kendaraan pribadi? | Include counterpoint |
| TOP-018 | Narrative | Cerita fiksi tentang kesalahpahaman kecil dalam kerja tim | Low stakes |
| TOP-019 | Instructional | Cara memeriksa konsistensi data dalam tabel sederhana | No real personal data |
| TOP-020 | Technical | Mengapa pencadangan data penting bagi pengguna komputer | No product promotion |

## Prompt templates

### PB-EDU-01 — explanatory
Jelaskan topik berikut kepada mahasiswa tahun pertama dengan Bahasa Indonesia yang jelas dan wajar: **{TOPIC}**. Tulis sekitar 120–180 kata. Jelaskan gagasan utama dan berikan satu contoh konkret. Jangan membuat klaim faktual yang tidak dapat dipertanggungjawabkan. Jangan menambahkan daftar sumber kecuali sumbernya benar-benar tersedia.

### PB-INFO-01 — informative
Tulis penjelasan informatif tentang **{TOPIC}** untuk pembaca umum. Panjang 150–220 kata. Susun penjelasan secara runtut, jelaskan istilah yang diperlukan, dan sebutkan satu keterbatasan atau hal yang perlu diperhatikan. Jangan mengarang statistik.

### PB-OPIN-01 — argumentative
Tulis pendapat yang beralasan mengenai **{TOPIC}** dalam 150–220 kata. Nyatakan posisi, berikan dua alasan, lalu jelaskan satu argumen tandingan secara adil. Hindari klaim statistik tanpa sumber dan hindari bahasa yang menyerang pihak tertentu.

### PB-NARR-01 — fictional narrative
Tulis cerita fiksi pendek berbahasa Indonesia berdasarkan topik **{TOPIC}**, sekitar 150–220 kata. Buat urutan peristiwa yang masuk akal dan akhir yang jelas. Jangan menggunakan nama atau detail yang mengidentifikasi orang nyata.

### PB-INST-01 — instructional
Jelaskan langkah-langkah melakukan **{TOPIC}** untuk pemula, sekitar 120–180 kata. Gunakan urutan yang mudah diikuti, sertakan satu kesalahan umum yang perlu dihindari, dan jangan memberikan instruksi berbahaya.

### PB-TECH-01 — technical explanation
Jelaskan konsep teknis berikut kepada pembaca pemula: **{TOPIC}**. Panjang 150–220 kata. Gunakan istilah yang tepat, berikan satu contoh, dan jelaskan batasan analogi jika menggunakan analogi. Jangan mengarang API, angka, atau sumber.

### PB-CAS-01 — casual message
Buat teks kasual berbahasa Indonesia untuk konteks berikut: **{TOPIC}**. Panjang 50–100 kata. Gunakan bahasa yang sesuai untuk komunikasi sehari-hari, tetapi jangan menambahkan nama, nomor telepon, alamat, atau detail pribadi.

## Length conditions

Untuk setiap genre, jika anggaran memungkinkan, buat kondisi panjang pendek dan sedang:
- pendek: 50–100 kata;
- sedang: 120–220 kata.

Catat target panjang dan panjang aktual. Jangan memotong output tanpa mencatat proses pemotongan. Teks di bawah 30 kata dapat disimpan untuk analisis khusus, tetapi jangan dicampur tanpa penandaan ke evaluasi utama karena deteksi teks sangat pendek lebih tidak pasti.

## Paired generation protocol

1. Pilih `topic_id` dan `prompt_id`.
2. Ambil satu jawaban manusia dan satu jawaban AI untuk tugas yang sepadan, jika tersedia.
3. Simpan keduanya sebagai record terpisah.
4. Beri `pair_id` yang sama dan `group_id` yang sama agar tidak melintasi split.
5. Jangan menyalin jawaban manusia ke prompt AI atau sebaliknya.
6. Jika AI ditanya tentang teks manusia atau diberi teks manusia sebagai konteks, catat hubungan tersebut dan jangan perlakukan output sebagai sampel independen.
7. Simpan model/provider dan konfigurasi generasi yang tepat.

## Prompt registry fields

`prompt_id`, `prompt_version`, `topic_id`, `genre`, `task_condition`, `template_text`, `language`, `target_min_words`, `target_max_words`, `created_at`, `approved_by`, `notes`.

## Pilot balance

Jangan mengejar distribusi kelas dengan mengubah label. Sebagai target awal, sebar topik lintas genre dan provider. Catat jumlah aktual per kelas, genre, panjang, prompt, penulis, dan generator. Bila suatu sel terlalu kecil, tandai sebagai keterbatasan pilot.
