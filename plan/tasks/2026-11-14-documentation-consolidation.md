# Task Plan: Konsolidasi Dokumentasi Requirements Lexora

- **Tanggal:** 2026-11-14
- **Status:** In progress
- **Tujuan:** Memasukkan rekap hasil elisitasi ke GitHub dan menata dokumentasi agar setiap informasi memiliki satu sumber kebenaran, tidak berulang, serta mudah dinavigasi.
- **Sumber yang diperiksa:** `AGENTS.md`, `plan/DECISION-LOG.md`, `plan/SYSTEM-REQUIREMENTS-ANALYSIS.md`, `plan/AI-HUMANIZER-PLAN.md`, `plan/AI-TEXT-DETECTOR-PLAN.md`, `plan/ACTIVE-ROADMAP.md`, `plan/PRODUCT-OVERVIEW.md`, `plan/OPERATING-WORKFLOW.md`, dan `docs/TOOLING-AND-SKILLS.md`.

## Ruang lingkup

- Mempertahankan seluruh keputusan historis dan statusnya.
- Menyusun rekap status elisitasi yang merangkum cakupan pembahasan, keputusan, pekerjaan tersisa, dan posisi pertanyaan terakhir.
- Memindahkan seluruh requirement Humanizer ke bagian SRS yang benar dan mengurutkannya berdasarkan ID.
- Menyelaraskan requirement yang bertentangan akibat keputusan yang lebih baru.
- Memisahkan kebutuhan yang telah disepakati dari hal yang belum diputuskan dalam AI Humanizer Plan.
- Memperbarui indeks dokumentasi dan tautan antarfile.
- Menghapus placeholder yang tidak berisi dokumentasi substantif setelah memastikan tidak menjadi sumber informasi aktif.

## Di luar ruang lingkup

- Mengubah keputusan produk yang telah disetujui.
- Menjawab Pertanyaan 138 atau membuat keputusan baru.
- Menyelesaikan elisitasi Detector, akun, kredit, pembayaran, atau Admin.
- Memulai wireframe, System Design, atau implementasi.
- Mengubah prioritas riset dataset yang ditetapkan dalam `AGENTS.md`.

## Langkah kerja

1. Audit struktur file dan identifikasi sumber kebenaran tiap jenis informasi.
2. Buat indeks dokumentasi dan ringkasan status elisitasi.
3. Rapikan urutan requirement Humanizer dalam SRS dan selaraskan konflik yang sudah dapat diselesaikan dari keputusan eksplisit.
4. Rapikan AI Humanizer Plan agar bagian hal yang belum diputuskan hanya berisi isu yang benar-benar terbuka.
5. Perbarui Product Overview dan Active Roadmap agar tidak memuat status atau rujukan yang kedaluwarsa.
6. Perbarui README root dan Decision Log navigation.
7. Hapus file placeholder yang sudah digantikan oleh struktur dokumentasi resmi.
8. Baca ulang seluruh file yang diubah dan validasi tautan, ID requirement, duplikasi, serta konsistensi status.

## Kriteria selesai

- [ ] Rekap status elisitasi tersedia dan akurat.
- [ ] Decision Log tetap memuat DEC-001 sampai DEC-137 tanpa kehilangan keputusan; status superseded tetap ditandai.
- [ ] Semua FR-HUM-01 sampai FR-HUM-117 berada di bagian 4.4 SRS, terurut, dan tanpa ID ganda.
- [ ] Tidak ada FR-HUM yang tertinggal di luar tabel kebutuhan fungsional.
- [ ] Keputusan yang sudah disepakati tidak lagi tercantum sebagai hal belum diputuskan.
- [ ] Dokumen indeks menjelaskan sumber kebenaran dan lokasi setiap jenis informasi.
- [ ] README dan tautan internal mengarah ke lokasi yang benar.
- [ ] File placeholder yang tidak digunakan tidak tersisa.
- [ ] Semua perubahan dibaca ulang dari GitHub dan diverifikasi.
