# Rancangan Produk dan Sistem TextLab

**Status:** rancangan konseptual, belum diimplementasikan.

## 1. Fitur produk

### AI Text Detector
- Input teks Bahasa Indonesia, pendek maupun panjang.
- Dapat digunakan tanpa akun.
- Kuota awal 3 penggunaan gratis per hari.
- Penggunaan tambahan direncanakan melalui iklan, dengan syarat format iklan diverifikasi kebijakannya.
- Highlight hasil pada tingkat kalimat.
- Ringkasan tingkat dokumen.
- Status UNCERTAIN ketika hasil tidak cukup meyakinkan.

### AI Humanizer
- Mengubah gaya penulisan berdasarkan profil pengguna atau gaya pustaka.
- Profil personal memerlukan akun.
- Profil dibuat dari 3–5 sampel tulisan sebagai rancangan awal.
- Pengguna dapat meninjau dan memperbaiki profil.
- Sampel mentah dihapus setelah profiling kecuali pengguna memilih menyimpannya.
- Menjaga makna, fakta, kutipan, dan struktur penting.
- Tidak menambahkan kesalahan secara sengaja.

### DOCX Metadata Cleaner
- Format awal DOCX saja.
- Membersihkan metadata dokumen.
- Detail metadata yang dihapus, batasan format, dan validasi output perlu dirinci sebelum implementasi.
- Jangan menjanjikan penghapusan seluruh jejak atau menjamin anonimitas tanpa verifikasi teknis.

## 2. Tool pengembangan

### Google Stitch
Peran: desain UI/UX, eksplorasi layout, iterasi tampilan, dan dokumentasi sistem desain.

Hasil Stitch harus ditinjau sebelum implementasi, termasuk hierarchy, responsive states, accessibility, dan keadaan komponen. Mockup bukan bukti bahwa interaksi sudah berfungsi.

### Google Antigravity
Peran: implementasi website, pengembangan komponen, integrasi layanan, debugging, dan verifikasi melalui test serta browser.

Kode yang dihasilkan agen wajib ditinjau. Klaim agen bahwa fitur selesai tidak menggantikan bukti test atau pemeriksaan manual.

### Alur kerja
Spesifikasi fitur → desain di Stitch → tinjauan desain → implementasi di Antigravity → test dan pemeriksaan browser → review diff → catat status dan hasil di GitHub.

Rincian: [Development Workflow: Stitch + Antigravity](DEVELOPMENT-WORKFLOW.md).

## 3. Akun dan akses

- Detector dapat digunakan tanpa login.
- Personal profile membutuhkan akun.
- Humanizer dan Metadata Cleaner direncanakan menggunakan mekanisme akses berbasis iklan.
- Rancangan monetisasi masih menunggu verifikasi kebijakan Google Ads dan ketersediaan format rewarded ads yang sesuai.

## 4. Privasi

- Pemrosesan teks sementara.
- Penyimpanan minimal.
- Pemisahan profil pengguna dari teks mentah.
- Retensi dan penghapusan ditetapkan secara eksplisit.
- Data pribadi tidak dikumpulkan jika tidak dibutuhkan.
- Consent dan dasar penggunaan dataset harus terdokumentasi.

## 5. Arsitektur detector

Frontend → API Gateway → Detection Service → CPU Inference Runtime → Calibration/Aggregation → Response.

### API Gateway
- Input validation.
- Rate limiting.
- Kuota.
- Request ID.
- Pengelolaan kegagalan agar inferensi gagal tidak mengurangi kuota.

### Detection Service
- Segmentasi kalimat.
- Menjaga offset karakter terhadap teks asli.
- Penanganan Unicode dan normalisasi.
- Menyusun konteks kalimat bila model menggunakannya.

### Inference Runtime
- Model dan tokenizer dengan versi eksplisit.
- CPU-first.
- Batch inference.
- ONNX quantization dievaluasi setelah model dasar stabil.

### Response
- Label dan skor per kalimat.
- Status ketidakpastian.
- Ringkasan dokumen.
- Model version dan preprocessing version.

## 6. Arsitektur Humanizer

Input → validation/access → style resolver → text analysis → prompt builder → generative model → output validation → result/diff.

Output validation:
- Pemeriksaan makna.
- Pemeriksaan fakta yang dapat dipertahankan.
- Perlindungan kutipan.
- Perlindungan struktur penting.
- Perbandingan hasil dengan teks asli.

## 7. Batas klaim

- Detector menghasilkan estimasi, bukan bukti pasti kepengarangan.
- Skor bukan persentase pasti teks AI.
- Metadata cleaner tidak boleh mengklaim menghapus seluruh jejak digital.
- Humanizer tidak menjamin teks lolos detector atau menyamarkan asal-usul secara sempurna.

## 8. Hal yang menunggu keputusan

- Kebijakan dan implementasi iklan.
- Provider/model generatif untuk humanizer.
- Detail keamanan dan retensi.
- Spesifikasi DOCX metadata yang dihapus.
- Target performa server.
- Integrasi akun dan billing/kuota.
- Frontend framework, backend stack, dan deployment target.
- Kriteria acceptance dan test matrix setelah screen pertama dibuat.
