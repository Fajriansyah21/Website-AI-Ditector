# Lexora Tooling and Skills Adoption

Status: **rencana adopsi**, bukan bukti bahwa skill sudah terpasang di komputer pengguna.

## 1. Toolchain produk

| Tool | Peran | Status |
|---|---|---|
| Google Stitch | Eksplorasi UI/UX, layout, design direction, dan handoff | Dipilih; belum menjadi prioritas aktif |
| Google Antigravity | Implementasi, debugging, test, dan review perubahan | Dipilih; skill workspace mulai disiapkan |
| GitHub | Source of truth untuk instruksi, rencana, keputusan, dan bukti | Aktif |

## 2. Standar skill Antigravity

Gunakan struktur native workspace:

```text
.agents/
  skills/
    <skill-name>/
      SKILL.md
      references/   # optional
      scripts/      # optional
```

Skill memiliki YAML frontmatter minimal `name` dan `description`. Gunakan skill yang fokus dan dipanggil sesuai kebutuhan, bukan satu prompt raksasa untuk semua pekerjaan.

Dokumentasi resmi:
- https://antigravity.google/docs/skills
- https://antigravity.google/docs/rules-workflows
- https://github.com/vercel-labs/skills

Catatan kompatibilitas: Antigravity saat ini menggunakan `.agents/skills/` sebagai lokasi standar workspace dan masih mendukung `.agent/skills/` sebagai kompatibilitas lama. Untuk skill baru Lexora, gunakan `.agents/skills/`.

## 3. Evaluasi referensi skill eksternal

### UI UX Pro Max

Sumber: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill  
Lisensi repo: MIT.

Nilai guna:
- Design-system reasoning.
- Pilihan style, layout, typography, palette, dan kategori produk.
- Referensi desain untuk membantu menilai hasil Stitch dan konsistensi implementasi.

Keputusan adopsi:
- **Direkomendasikan sebagai skill desain pendamping**, bukan pengganti Google Stitch.
- Jangan menjalankannya selama tahap uji prosedur dataset kecuali ada kebutuhan desain yang eksplisit.
- README upstream menyediakan instalasi Antigravity melalui `uipro init --ai antigravity`.
- Sebelum instalasi, periksa versi CLI, file yang akan dibuat/diubah, dan diff setelah pemasangan.

### Impeccable

Sumber: https://github.com/pbakaus/impeccable  
Lisensi skill: Apache-2.0.

Nilai guna:
- UX critique, audit aksesibilitas, responsive behavior, visual hierarchy, typography, dan polish.
- Memiliki alur untuk memeriksa hasil render, bukan hanya source code.

Keputusan adopsi:
- **Direkomendasikan untuk tahap implementasi frontend dan UI review.**
- Jangan menjalankan command UI selama fokus riset dataset masih aktif.
- Upstream mendukung Google Antigravity dan menyediakan `npx impeccable install`.
- Installer dapat menambahkan skill dan menawarkan hook. Periksa perubahan dan izin hook sebelum menyetujuinya.
- Jangan mengaktifkan hook otomatis tanpa memahami file dan perilaku yang dipantau.

### Stop Slop

Sumber: https://github.com/hardikpandya/stop-slop  
Lisensi repo: MIT.

Nilai guna:
- Mengurangi filler, pola kalimat generik, repetisi, dan prosa yang terlalu formulaik.

Keputusan adopsi:
- **Direkomendasikan untuk review copywriting, dokumentasi, dan teks UI.**
- Tidak boleh diterapkan pada teks dataset, fixture provenance, prompt eksperimen, atau data anotasi karena dapat mengubah karakteristik teks yang justru sedang diukur.
- Aturan upstream yang terlalu absolut, seperti menghapus semua adverbia atau semua passive voice, diperlakukan sebagai heuristik review, bukan aturan bahasa universal.

### Huashu Design

Sumber: https://github.com/pc-software-org/huashu-design-en  
Lisensi README/repo: MIT.

Nilai guna:
- High-fidelity HTML prototypes, interactive demos, motion design, presentation, dan eksplorasi variasi visual.

Batasan:
- README dan SKILL.md upstream menyatakan skill ini bukan untuk production-level web apps, SEO websites, atau sistem dinamis yang memerlukan backend.
- Lexora sudah memilih Google Stitch untuk UI/UX dan Antigravity untuk implementasi.

Keputusan:
- **Tidak dipasang pada tahap ini.**
- Pertimbangkan hanya untuk prototipe interaktif terpisah, motion/launch assets, atau demonstrasi konsep jika kebutuhan tersebut muncul.

### Task Observer

Sumber: https://github.com/iamneilroberts/claude-skills/tree/main/skills/task-observer  
Lisensi sumber: CC BY 4.0.

Nilai guna:
- Mengamati koreksi pengguna, friction, pola kegagalan, dan peluang perbaikan skill secara berkelanjutan.

Batasan:
- Sumber dibuat untuk ekosistem Claude dan mengasumsikan file/log/workspace tertentu. Jangan memasangnya mentah-mentah lalu menganggap seluruh instruksinya kompatibel dengan Antigravity.

Keputusan:
- Metodologinya diadaptasi menjadi skill internal `.agents/skills/task-observer/SKILL.md`.
- Atribusi sumber dipertahankan.
- Log lokal proyek berada di `plan/OBSERVATION-LOG.md`.

## 4. Skill internal Lexora yang sudah ditambahkan

| Skill | Lokasi | Fungsi |
|---|---|---|
| Task Observer | `.agents/skills/task-observer/SKILL.md` | Menangkap pembelajaran workflow yang dapat digeneralisasi |
| Synthetic Dataset Test | `.agents/skills/synthetic-dataset-test/SKILL.md` | Prosedur uji sintetis dataset |

Kedua skill ini sudah berada di GitHub branch `main`. Aktivasi aktual perlu diverifikasi di Antigravity Customizations setelah repository disinkronkan/dibuka sebagai workspace.

## 5. Urutan instalasi yang disarankan

Jangan memasang semua skill sekaligus. Urutan yang disarankan:

1. Sinkronkan repository Lexora dan buka sebagai workspace Antigravity.
2. Pastikan `AGENTS.md` dan dua skill internal muncul/terbaca.
3. Jalankan task sintetis dataset sesuai roadmap.
4. Ketika UI work dimulai, evaluasi pemasangan UI UX Pro Max.
5. Pasang Impeccable saat ada frontend yang dapat diaudit.
6. Pasang Stop Slop ketika mulai mengerjakan copywriting atau dokumentasi produk.

Perintah upstream yang tercantum untuk referensi:
- UI UX Pro Max: `npm install -g ui-ux-pro-max-cli`, lalu `uipro init --ai antigravity`.
- Impeccable: `npx impeccable install`.

Jalankan perintah tersebut dari root workspace Lexora hanya saat tahapnya relevan. Baca perubahan yang dibuat installer sebelum commit. Stop Slop dapat dipasang melalui Vercel Skills CLI setelah memeriksa opsi CLI saat itu.

## 6. Prinsip keamanan adopsi

- Baca `SKILL.md`, README, lisensi, dan script yang akan dieksekusi.
- Hindari pemasangan global jika kebutuhan hanya untuk Lexora.
- Review setiap script, hook, dependency, dan permission.
- Jangan izinkan skill membaca atau mengirim data sensitif ke layanan eksternal tanpa kebutuhan dan persetujuan.
- Pin versi/commit untuk dependency yang memengaruhi hasil kerja bila stabilitas reproduksibilitas dibutuhkan.
- Setelah instalasi, periksa file tree, diff, dan cara menonaktifkan/menghapus integrasi.
