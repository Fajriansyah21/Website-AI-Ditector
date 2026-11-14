# Dokumentasi Proyek Lexora

Folder ini menyimpan rencana produk, kebutuhan sistem, keputusan, status elisitasi, dan rencana riset Lexora.

## Mulai dari sini

1. Baca [Product Overview](PRODUCT-OVERVIEW.md) untuk memahami produk dan cakupan MVP.
2. Baca [Requirements Elicitation Status](REQUIREMENTS-ELICITATION-STATUS.md) untuk mengetahui apa yang sudah dibahas dan pekerjaan yang masih terbuka.
3. Gunakan [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md) sebagai sumber utama kebutuhan sistem.
4. Gunakan [Decision Log](DECISION-LOG.md) untuk menelusuri keputusan, alasan, dan status historisnya.
5. Periksa [Active Roadmap](ACTIVE-ROADMAP.md) sebelum memulai task baru.

## Peta dokumen dan sumber kebenaran

| Dokumen | Fungsi | Status informasi |
|---|---|---|
| [Product Overview](PRODUCT-OVERVIEW.md) | Ringkasan tujuan, pengguna, modul, dan prinsip produk | Ringkasan tingkat produk |
| [Requirements Elicitation Status](REQUIREMENTS-ELICITATION-STATUS.md) | Rekap pembahasan, kemajuan, gap, dan urutan pekerjaan | Ringkasan navigasi, bukan requirement normatif |
| [System Requirements Analysis](SYSTEM-REQUIREMENTS-ANALYSIS.md) | Kebutuhan fungsional/nonfungsional dan aturan bisnis | **Sumber utama requirement** |
| [Decision Log](DECISION-LOG.md) | Keputusan yang disahkan, alasan, alternatif, dan dampak | **Sumber utama keputusan** |
| [AI Humanizer Plan](AI-HUMANIZER-PLAN.md) | Alur konseptual, prinsip kualitas, risiko, dan isu terbuka Humanizer | Rencana fitur, merujuk ke SRS |
| [AI Text Detector Plan](AI-TEXT-DETECTOR-PLAN.md) | Rencana produk dan riset model Detector | Rencana fitur/riset, merujuk ke SRS |
| [Dataset and Model Research Plan](DATASET-AND-MODEL-RESEARCH-PLAN.md) | Prosedur dataset, sumber data, split, dan evaluasi model | Rencana riset |
| [Active Roadmap](ACTIVE-ROADMAP.md) | Prioritas kerja dan gerbang tahap proyek | Status kerja |
| [Operating Workflow](OPERATING-WORKFLOW.md) | Siklus task, verifikasi, dan pelaporan | Aturan kerja |
| [Observation Log](OBSERVATION-LOG.md) | Pembelajaran workflow yang dapat digeneralisasi | Log observasi |
| [Tooling and Skills](../docs/TOOLING-AND-SKILLS.md) | Peran tool dan evaluasi skill | Panduan tooling |
| [Task plans](tasks/) | Brief dan rencana task multi-langkah | Catatan task |

## Aturan agar tidak duplikatif

- Requirement normatif ditulis satu kali di SRS. Dokumen fitur merangkum alur dan menunjuk ID requirement, bukan menyalin tabel requirement.
- Keputusan dan alasan dicatat satu kali di Decision Log. SRS menyatakan perilaku yang wajib dipenuhi dan dapat merujuk ID keputusan.
- Status elisitasi hanya merangkum cakupan, kemajuan, dan gap. Jangan memperlakukannya sebagai pengganti SRS.
- Roadmap menunjukkan urutan dan status pekerjaan, bukan tempat menyimpan ulang spesifikasi fitur.
- Hasil pengujian harus disimpan bersama artefak/fixture yang dapat diperiksa, bukan ditulis sebagai klaim tanpa bukti.

## Struktur repository terkait

```text
/
├── AGENTS.md
├── README.md
├── docs/
│   └── TOOLING-AND-SKILLS.md
├── .agents/
│   └── skills/
└── plan/
    ├── README.md
    ├── ACTIVE-ROADMAP.md
    ├── DECISION-LOG.md
    ├── PRODUCT-OVERVIEW.md
    ├── REQUIREMENTS-ELICITATION-STATUS.md
    ├── SYSTEM-REQUIREMENTS-ANALYSIS.md
    ├── AI-HUMANIZER-PLAN.md
    ├── AI-TEXT-DETECTOR-PLAN.md
    ├── DATASET-AND-MODEL-RESEARCH-PLAN.md
    ├── OPERATING-WORKFLOW.md
    ├── OBSERVATION-LOG.md
    └── tasks/
```
