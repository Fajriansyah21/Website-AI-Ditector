# Lexora

**Lexora** adalah nama produk untuk platform analisis dan pengolahan teks berbahasa Indonesia. Nama historis/internal proyek adalah TextLab.

Repositori ini menjadi sumber kebenaran untuk instruksi agent, keputusan, rencana kerja, spesifikasi, eksperimen, dan bukti implementasi.

> **Status saat audit 2026-11-12:** tahap persiapan workflow dan riset. Prioritas aktif adalah uji prosedur sintetis dataset. Belum ada bukti pilot dataset nyata, training model final, atau implementasi aplikasi pada snapshot branch ini.

## Fokus produk

Ruang lingkup yang direncanakan:
1. **AI Text Detector**: indikasi karakteristik asal-usul teks pada tingkat kalimat dan ringkasan dokumen.
2. **AI Humanizer**: bantuan penyesuaian gaya penulisan.
3. **DOCX Metadata Cleaner**: pembersihan metadata dokumen DOCX pada tahap awal.

Hasil detector adalah estimasi model, bukan bukti pasti kepengarangan.

## Prioritas aktif

**Uji prosedur sintetis dataset**, dengan urutan:
1. JSON Schema.
2. Sentence offset.
3. Editing history dan version lineage.
4. Deduplikasi exact dan near-duplicate.
5. Group-based split untuk mencegah data leakage.

Pilot dataset nyata dan training belum dimulai. Lihat [Active Roadmap](plan/ACTIVE-ROADMAP.md).

## Instruksi dan workflow

- [AGENTS.md](AGENTS.md): aturan kerja permanen untuk agent.
- [Operating Workflow](plan/OPERATING-WORKFLOW.md): siklus task dari orientasi sampai verifikasi.
- [Active Roadmap](plan/ACTIVE-ROADMAP.md): prioritas dan gerbang kelulusan.
- [Observation Log](plan/OBSERVATION-LOG.md): pembelajaran workflow yang dapat ditindaklanjuti.
- [Tooling and Skills](docs/TOOLING-AND-SKILLS.md): strategi adopsi skill dan tool.

## Antigravity workspace skills

- [Task Observer](.agents/skills/task-observer/SKILL.md)
- [Synthetic Dataset Test](.agents/skills/synthetic-dataset-test/SKILL.md)

Skill Antigravity menggunakan struktur `.agents/skills/<skill-name>/SKILL.md`. Pastikan skill terbaca dari menu Customizations setelah workspace disinkronkan.

## Toolchain

| Tool | Peran | Status |
|---|---|---|
| GitHub | Source of truth dan version history | Aktif |
| Google Stitch | UI/UX design dan handoff | Dipilih, belum menjadi prioritas aktif |
| Google Antigravity | Implementasi, debugging, dan verifikasi | Dipilih; workspace rules/skills disiapkan |

## Catatan audit repositori

Pada pemeriksaan branch `main` tanggal 2026-11-12, file yang tersedia sebelum pembaruan workflow hanya `README.md`, `.skill/contohSkill.md`, dan `plan/contohPlan.md`. Beberapa dokumen riset yang pernah dirujuk dalam percakapan sebelumnya tidak ditemukan pada tree branch tersebut. Karena itu, dokumen tersebut tidak dicantumkan sebagai sumber aktif dan harus dipulihkan atau dibuat ulang berdasarkan keputusan yang dapat diverifikasi sebelum pengujian yang bergantung padanya dimulai.

## Prinsip proyek

- Riset dan validasi mendahului implementasi.
- Setiap data training harus memiliki provenance dan dasar penggunaan yang jelas.
- Dataset publik tidak otomatis berlabel HUMAN atau otomatis boleh digunakan.
- Dokumen terkait, pasangan, versi, dan turunannya harus dikelompokkan sebelum split.
- False positive terhadap tulisan manusia merupakan risiko utama.
- Status rancangan, implementasi, dan hasil pengujian harus dibedakan.
- Perubahan keputusan dicatat beserta alasan, dampak, dan tanggalnya.
- Jangan commit secrets, credentials, atau data pribadi.
