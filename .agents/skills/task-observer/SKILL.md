---
name: task-observer
description: Captures reusable workflow lessons, user corrections, recurring failure patterns, and skill improvement opportunities during substantive Lexora project work.
---

# Task Observer for Lexora

Adaptasi workflow observasi berkelanjutan untuk proyek Lexora. Skill ini bersifat internal dan tidak menggantikan task execution skill.

## Kapan aktif

Gunakan pada pekerjaan multi-langkah, penggunaan tool untuk menghasilkan deliverable, review hasil, koreksi pengguna, dan diskusi tentang metodologi kerja.

Jangan aktif untuk obrolan santai atau pertanyaan faktual sederhana.

## Yang diamati

Catat hanya hal yang berpotensi memperbaiki cara kerja secara berulang:

- Koreksi pengguna yang menunjukkan aturan belum jelas atau tidak dipatuhi.
- Langkah yang berulang kali menyebabkan kegagalan, rework, atau pemborosan.
- Teknik yang terbukti meningkatkan kualitas atau verifiability.
- Konflik antara instruksi, skill, dokumentasi, dan perilaku tool.
- Kebutuhan skill baru yang memiliki trigger, input, proses, dan output jelas.
- Aturan yang sudah tidak relevan, berulang, atau terlalu rumit.

Jangan catat:
- Preferensi satu kali yang hanya berlaku pada artefak tertentu.
- Dugaan tentang pengguna.
- Rahasia, kredensial, data pribadi, atau teks dataset mentah.
- Observasi yang tidak bisa ditindaklanjuti.

## Protokol

1. Pada awal task, baca `AGENTS.md`, roadmap aktif, dan observasi terbuka yang relevan.
2. Selama task, identifikasi sinyal pembelajaran tanpa menghentikan pekerjaan untuk komentar yang tidak perlu.
3. Ketika ada koreksi atau insight yang dapat digeneralisasi, catat pada `plan/OBSERVATION-LOG.md` pada turn yang sama atau checkpoint terdekat.
4. Beri status `OPEN`, `ACTIONED`, `DECLINED`, atau `DEFERRED`.
5. Jangan mengubah aturan permanen hanya dari satu kejadian. Cari pengulangan atau minta persetujuan pengguna jika perubahan berdampak luas.
6. Saat menerapkan observasi, ubah skill/instruksi yang tepat, lalu verifikasi diff.
7. Tinjau apakah aturan lama dapat disederhanakan. Jangan hanya menambah aturan.

## Format observasi

| ID | Tanggal | Area | Observasi dan bukti | Tindakan yang disarankan | Status |
|---|---|---|---|---|---|

Simpan metodologi generik, bukan detail sensitif proyek. Skill ini terinspirasi oleh pendekatan Task Observer karya Eoghan Henn (https://github.com/iamneilroberts/claude-skills/tree/main/skills/task-observer), dengan adaptasi untuk Lexora dan Antigravity. Sumber asli menyatakan lisensi CC BY 4.0; atribusi dipertahankan.
