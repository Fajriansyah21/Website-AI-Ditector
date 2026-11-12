# DOCX Metadata Cleaner Plan

Status: **Draft rancangan fitur**  
Modul: DOCX Metadata Cleaner  
Cakupan format awal: DOCX  
Terakhir ditinjau: 2026-11-12

## 1. Tujuan

Menyediakan alat untuk memeriksa dan membersihkan metadata tertentu dari dokumen Microsoft Word berformat DOCX. Fitur ini berfokus pada metadata dokumen, bukan mengubah isi tulisan agar tampak ditulis manusia.

## 2. Batas cakupan yang pernah dibahas

- DOCX menjadi format awal.
- Fokus pada metadata yang dapat mengungkap informasi pembuat, aplikasi, atau riwayat tertentu.
- Jangan mengklaim menghapus seluruh jejak AI atau menjamin dokumen tidak dapat dianalisis.

Dukungan PDF, ODT, DOC lama, dan format lain belum diputuskan.

## 3. Prinsip keamanan dan integritas

- File asli tidak boleh ditimpa tanpa persetujuan eksplisit.
- Hasil bersih harus menjadi file keluaran terpisah.
- Sistem harus memvalidasi file sebagai DOCX yang dapat dibaca.
- Penanganan file rusak, terenkripsi, terlalu besar, atau tidak valid harus jelas.
- Jangan mengubah isi dokumen secara tidak sengaja.
- Beri tahu pengguna kategori metadata yang diperiksa, dihapus, atau dipertahankan.
- Hapus file sementara sesuai kebijakan retensi yang nantinya ditetapkan.
- Perlakukan file sebagai input tidak tepercaya; pertimbangkan risiko arsip ZIP, XML, file berukuran ekstrem, dan konten berbahaya.

## 4. Alur pengguna konseptual

1. Pengguna mengunggah file DOCX.
2. Sistem memvalidasi format dan batas ukuran.
3. Sistem membaca struktur package DOCX dan memeriksa metadata yang didukung.
4. Sistem menampilkan ringkasan tindakan yang akan dilakukan atau kebijakan pembersihan yang dipilih.
5. Sistem menghasilkan file DOCX baru.
6. Sistem memvalidasi hasil dan menyediakan unduhan.
7. Sistem menghapus file sementara sesuai kebijakan retensi.

Alur ini masih konseptual dan belum menjadi spesifikasi teknis.

## 5. Area metadata yang perlu dianalisis

Daftar berikut adalah area investigasi, bukan janji bahwa semuanya akan dihapus:
- Core properties seperti creator, last modified by, title, subject, dan timestamps.
- Application properties.
- Custom properties.
- Komentar, revision tracking, atau data perubahan jika nantinya masuk cakupan.
- Metadata tambahan dalam bagian package DOCX.

Penghapusan komentar atau revision tracking dapat mengubah informasi substantif dokumen. Tindakan tersebut harus dipisahkan dari pembersihan metadata dasar dan memerlukan keputusan pengguna yang jelas.

## 6. Pengujian yang dibutuhkan

- DOCX valid dengan metadata lengkap.
- DOCX tanpa metadata.
- File yang rusak atau bukan DOCX.
- File besar dan struktur ZIP yang tidak wajar.
- Dokumen dengan komentar dan tracked changes.
- Validasi bahwa isi utama, gambar, tabel, hyperlink, dan format tidak rusak setelah pemrosesan.
- Pemeriksaan bahwa metadata yang ditargetkan benar-benar berubah.
- Pemeriksaan bahwa metadata yang tidak termasuk cakupan tetap dipertahankan.

## 7. Belum diputuskan

- Library pemrosesan DOCX.
- Proses server-side atau client-side.
- Batas ukuran file.
- Daftar metadata yang dihapus secara default.
- Penanganan komentar dan tracked changes.
- Kebijakan retensi dan penghapusan file.
- Apakah pemrosesan membutuhkan akun atau kuota.
- Target kompatibilitas Microsoft Word dan aplikasi office lain.
