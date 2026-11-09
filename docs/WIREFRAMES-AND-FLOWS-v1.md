# 🖼️ TextLab — Wireframes & Flowcharts

**Dokumen terkait:** [Project Workflow](PROJECT-WORKFLOW.md), [Development Workflow](DEVELOPMENT-WORKFLOW.md), dan [Product & System Design](PRODUCT-AND-SYSTEM-DESIGN.md).  
**Tanggal:** 2026-11-09  
**Status:** rancangan wireframe dan alur interaksi; belum menjadi spesifikasi implementasi final.

---

> **Catatan status dan batasan**
> - Seluruh wireframe, flowchart, state diagram, dan breakpoint di dokumen ini adalah rancangan konseptual, bukan implementasi atau hasil pengujian.
> - Angka seperti batas minimum 50 karakter, kuota 3 penggunaan per hari, batas upload 10 MB, cooldown 60 detik, serta jumlah sampel adalah nilai rancangan/placeholder yang harus dikonfirmasi terhadap spesifikasi dan keputusan proyek sebelum implementasi.
> - Alur iklan hanya bersifat opsional dan tidak boleh diaktifkan sebelum kebijakan, kelayakan teknis, dan pengalaman pengguna diverifikasi. Jika belum terverifikasi, gunakan jalur tanpa iklan.
> - Klaim privasi pada mockup harus disesuaikan dengan perilaku sistem yang benar-benar diterapkan dan diuji. Jangan menjanjikan penghapusan seluruh jejak digital atau penyimpanan nol tanpa bukti teknis.
> - Data nama, email, dan metadata pada contoh layar bersifat placeholder, bukan data pengguna nyata.
> - Prioritas proyek saat ini tetap **uji prosedur sintetis dataset**. Dokumen desain ini tidak mengubah urutan prioritas riset.

## 1. Master Navigation Flow

```mermaid
flowchart TD
    ROOT["🧪 TextLab App"] --> NAV{"Navigation"}

    NAV --> HOME["🏠 Home / Detector"]
    NAV --> HUM["✍️ Humanizer"]
    NAV --> CLN["📄 Cleaner"]
    NAV --> ACC["👤 Account"]

    HOME --> HOME_INPUT["Input Teks"]
    HOME_INPUT --> HOME_LOADING["⏳ Analyzing..."]
    HOME_LOADING --> HOME_RESULT["📊 Results"]
    HOME_RESULT -->|"Analisis Baru"| HOME_INPUT
    HOME_RESULT -->|"Coba Humanize"| HUM

    HUM --> HUM_GATE{"Login?"}
    HUM_GATE -->|"Belum"| AUTH_LOGIN["🔐 Login"]
    HUM_GATE -->|"Sudah"| HUM_SELECT["Pilih Gaya"]
    AUTH_LOGIN -->|"Berhasil"| HUM_SELECT
    HUM_SELECT --> HUM_INPUT["Input Teks"]
    HUM_INPUT --> HUM_LOADING["⏳ Humanizing..."]
    HUM_LOADING --> HUM_RESULT["🔄 Before/After"]
    HUM_RESULT -->|"Ulang"| HUM_INPUT
    HUM_RESULT -->|"Salin"| HUM_DONE["📋 Copied!"]

    CLN --> CLN_UPLOAD["📤 Upload DOCX"]
    CLN_UPLOAD --> CLN_PREVIEW["🔍 Preview Metadata"]
    CLN_PREVIEW --> CLN_CONFIRM{"Konfirmasi?"}
    CLN_CONFIRM -->|"Ya"| CLN_PROCESS["⚙️ Processing"]
    CLN_CONFIRM -->|"Batal"| CLN_UPLOAD
    CLN_PROCESS --> CLN_DOWNLOAD["📥 Download"]

    ACC --> ACC_GATE{"Login?"}
    ACC_GATE -->|"Belum"| AUTH_LOGIN2["🔐 Login / Register"]
    ACC_GATE -->|"Sudah"| DASH["📋 Dashboard"]
    AUTH_LOGIN2 -->|"Berhasil"| DASH
    DASH --> DASH_PROFILE["🎨 Voice Profiles"]
    DASH --> DASH_HISTORY["📊 History"]
    DASH --> DASH_SETTINGS["⚙️ Settings"]
    DASH_PROFILE --> PROFILE_CREATE["+ Buat Profil"]
    DASH_SETTINGS --> SETTINGS_THEME["🌙 Theme"]
    DASH_SETTINGS --> SETTINGS_PRIVACY["🛡️ Privacy"]
    DASH_SETTINGS --> SETTINGS_LOGOUT["🚪 Logout"]
```

---

## 2. Desktop — Global Layout

```
┌══════════════════════════════════════════════════════════════════════════┐
│ NAVBAR (sticky, backdrop-blur)                                         │
│ ┌──────────────────────────────────────────────────────────────────────┐│
│ │ 🧪 TextLab     │ Detector │ Humanizer │ Cleaner │    [Login]  🌙   ││
│ └──────────────────────────────────────────────────────────────────────┘│
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│                        ┌──────────────────────┐                        │
│                        │                      │                        │
│                        │    PAGE CONTENT       │                        │
│                        │    max-w: 1280px      │                        │
│                        │    mx: auto           │                        │
│                        │    px: 24px           │                        │
│                        │                      │                        │
│                        └──────────────────────┘                        │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│ FOOTER                                                                 │
│ ┌──────────────────────────────────────────────────────────────────────┐│
│ │  🧪 TextLab          Produk          Legal          Sosial          ││
│ │  © 2026              Detector        Privasi        Media sosial         ││
│ │                      Humanizer       Ketentuan      GitHub          ││
│ │                      Cleaner         Kontak                         ││
│ └──────────────────────────────────────────────────────────────────────┘│
└══════════════════════════════════════════════════════════════════════════┘
```

## 3. Mobile — Global Layout

```
┌════════════════════════════┐
│ TOP BAR (auto-hide scroll) │
│ ┌────────────────────────┐ │
│ │ 🧪 TextLab    [≡] [🌙]│ │
│ └────────────────────────┘ │
├════════════════════════════┤
│                            │
│     PAGE CONTENT           │
│     px: 16px               │
│     pb: 80px (tab space)   │
│                            │
├════════════════════════════┤
│ BOTTOM TAB BAR (fixed)     │
│ ┌────────────────────────┐ │
│ │  🔍      ✍️     📄   👤│ │
│ │Detect  Human  Clean Akun│ │
│ └────────────────────────┘ │
└════════════════════════════┘
```

### Hamburger Drawer (Mobile)

```
┌════════════════════════════┐
│ 🧪 TextLab         [✕]    │
├────────────────────────────┤
│                            │
│  👤  Nama Pengguna        │
│      nama@example.com     │
│                            │
│  ─────────────────────     │
│                            │
│  🏠  Home                  │
│  🔍  Detector              │
│  ✍️  Humanizer             │
│  📄  Cleaner               │
│                            │
│  ─────────────────────     │
│                            │
│  📋  Dashboard             │
│  🎨  Voice Profiles        │
│  📊  Riwayat               │
│  ⚙️  Settings              │
│                            │
│  ─────────────────────     │
│                            │
│  🛡️  Kebijakan Privasi     │
│  📜  Ketentuan Layanan     │
│  ℹ️  Tentang TextLab       │
│                            │
│  ─────────────────────     │
│                            │
│  🚪  Logout                │
│                            │
└════════════════════════════┘
```

---

## 4. Screen 1 — Landing / Detector Input

### Desktop

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │        [Login]  🌙  │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│                          🧪                                            │
│                       TextLab                                          │
│                 "Kenali Asal Tulisanmu"                                 │
│            Analisis teks Bahasa Indonesia untuk                         │
│           mendeteksi konten yang dihasilkan AI.                         │
│                                                                        │
│     ┌──────────────────────────────────────────────────────────┐        │
│     │                                                          │        │
│     │  Tempelkan atau ketik teks Bahasa Indonesia di sini...   │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     ├──────────────────────────────────────────────────────────┤        │
│     │  0 karakter (min. 50)                    ●●● 3/3 gratis  │        │
│     └──────────────────────────────────────────────────────────┘        │
│                                                                        │
│                    ┌─────────────────────────┐                         │
│                    │   🔍  Analisis Teks      │                         │
│                    └─────────────────────────┘                         │
│                                                                        │
│     ─────────────────  Fitur Lainnya  ──────────────────               │
│                                                                        │
│     ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐   │
│     │  ✍️               │  │  📄               │  │  🛡️               │   │
│     │                  │  │                  │  │                  │   │
│     │  AI Humanizer    │  │  Metadata        │  │  Privasi         │   │
│     │                  │  │  Cleaner         │  │  Terjamin        │   │
│     │  Sesuaikan gaya  │  │                  │  │                  │   │
│     │  tulisanmu agar  │  │  Bersihkan       │  │  Teksmu diproses │   │
│     │  lebih personal  │  │  metadata DOCX   │  │  sementara &     │   │
│     │  dan autentik.   │  │  dengan mudah.   │  │  tidak disimpan. │   │
│     │                  │  │                  │  │                  │   │
│     │  [Coba →]        │  │  [Coba →]        │  │  [Pelajari →]    │   │
│     └──────────────────┘  └──────────────────┘  └──────────────────┘   │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  🧪 TextLab          Produk          Legal             Sosial          │
│  © 2026              Detector        Privasi           GitHub          │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile

```
┌══════════════════════════════┐
│  🧪 TextLab         [≡] [🌙]│
├══════════════════════════════┤
│                              │
│           🧪                 │
│        TextLab               │
│   "Kenali Asal Tulisanmu"   │
│                              │
│  Analisis teks Bahasa        │
│  Indonesia untuk mendeteksi  │
│  konten AI.                  │
│                              │
│  ┌────────────────────────┐  │
│  │                        │  │
│  │  Tempelkan teks        │  │
│  │  di sini...            │  │
│  │                        │  │
│  │                        │  │
│  │                        │  │
│  ├────────────────────────┤  │
│  │  0 char    ●●● 3/3    │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │   🔍  Analisis Teks    │  │
│  └────────────────────────┘  │
│                              │
│  ── Fitur Lainnya ────────   │
│                              │
│  ┌────────────────────────┐  │
│  │ ✍️ AI Humanizer        │  │
│  │ Sesuaikan gaya         │  │
│  │ tulisanmu.    [Coba →] │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │ 📄 Metadata Cleaner    │  │
│  │ Bersihkan metadata     │  │
│  │ DOCX.        [Coba →]  │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │ 🛡️ Privasi Terjamin    │  │
│  │ Teksmu tidak           │  │
│  │ disimpan.  [Info →]    │  │
│  └────────────────────────┘  │
│                              │
├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │
└══════════════════════════════┘
```

### Detector Input — State Flow

```mermaid
stateDiagram-v2
    [*] --> Empty : Page load

    Empty --> Typing : User mulai ketik
    Typing --> TooShort : char < 50
    Typing --> Valid : char >= 50
    TooShort --> Typing : Lanjut ketik
    Valid --> Typing : Hapus teks

    state quota_check <<choice>>
    Valid --> quota_check : Klik Analisis
    quota_check --> Loading : kuota > 0
    quota_check --> QuotaEmpty : kuota == 0

    Loading --> Results : API sukses
    Loading --> Error : API gagal
    Error --> Valid : Coba lagi
    QuotaEmpty --> WatchAd : Tonton iklan
    QuotaEmpty --> ComeBack : Kembali besok
    WatchAd --> Loading : Iklan selesai
    Results --> Empty : Analisis Baru

    note right of QuotaEmpty
        Iklan hanya jika kebijakan
        Google Ads sudah diverifikasi.
        Default: "Kembali besok"
    end note
```

---

## 5. Screen 2 — Detector Results

### Desktop (Two-Column)

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │        [👤 Pengguna] 🌙│
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│  ← Kembali                    Hasil Analisis                           │
│                                                                        │
│  ┌─────────────────────────┐  ┌────────────────────────────────────┐   │
│  │  📊 RINGKASAN           │  │  📝 TEKS DENGAN HIGHLIGHT          │   │
│  │                         │  │                                    │   │
│  │  Status Dokumen:        │  │  ┌────────────────────────────┐    │   │
│  │  ┌───────────────────┐  │  │  │ 🟢 "Pendidikan di         │    │   │
│  │  │      MIXED        │  │  │  │    Indonesia mengalami     │    │   │
│  │  └───────────────────┘  │  │  │    perkembangan pesat."    │    │   │
│  │                         │  │  │    Skor: 0.15 (HUMAN)      │    │   │
│  │  Distribusi:            │  │  └────────────────────────────┘    │   │
│  │  ████████████░░░░░░░░   │  │                                    │   │
│  │  HUMAN ████ 60%         │  │  ┌────────────────────────────┐    │   │
│  │  AI    ████ 30%         │  │  │ 🔴 "Dengan demikian,       │    │   │
│  │  UNCERTAIN █ 10%        │  │  │    dapat disimpulkan bahwa │    │   │
│  │                         │  │  │    transformasi digital    │    │   │
│  │  Detail:                │  │  │    telah memberikan..."    │    │   │
│  │  ● HUMAN: 6 kalimat    │  │  │    Skor: 0.87 (AI)        │    │   │
│  │  ● AI: 3 kalimat       │  │  └────────────────────────────┘    │   │
│  │  ● UNCERTAIN: 1        │  │                                    │   │
│  │                         │  │  ┌────────────────────────────┐    │   │
│  │  ─────────────────      │  │  │ 🟢 "Saya pribadi melihat  │    │   │
│  │                         │  │  │    hal ini dari sudut      │    │   │
│  │  Model: v0.1-lite      │  │  │    pandang berbeda."       │    │   │
│  │  Preprocessing: v1     │  │  │    Skor: 0.08 (HUMAN)      │    │   │
│  │                         │  │  └────────────────────────────┘    │   │
│  │  ─────────────────      │  │                                    │   │
│  │                         │  │  ┌────────────────────────────┐    │   │
│  │  ⚠️ Disclaimer          │  │  │ ⚪ "Hal tersebut perlu     │    │   │
│  │  Hasil ini adalah       │  │  │    dikaji lebih lanjut."   │    │   │
│  │  estimasi model, bukan  │  │  │    Skor: 0.48 (UNCERTAIN) │    │   │
│  │  bukti pasti asal-usul  │  │  └────────────────────────────┘    │   │
│  │  atau kepengarangan     │  │                                    │   │
│  │  teks.                  │  │  ┌────────────────────────────┐    │   │
│  │                         │  │  │ 🔴 "Implementasi kebijakan│    │   │
│  │  ─────────────────      │  │  │    ini memerlukan sinergi │    │   │
│  │                         │  │  │    antar pemangku..."     │    │   │
│  │  ┌───────────────────┐  │  │  │    Skor: 0.91 (AI)        │    │   │
│  │  │  🔍 Analisis Baru  │  │  │  └────────────────────────────┘    │   │
│  │  └───────────────────┘  │  │                                    │   │
│  │                         │  │  ──────────────────────────────    │   │
│  │  ┌───────────────────┐  │  │                                    │   │
│  │  │  ✍️ Coba Humanize  │  │  │  FILTER:                          │   │
│  │  └───────────────────┘  │  │  [🟢 HUMAN] [🔴 AI] [⚪ UNCERTAIN]│   │
│  │                         │  │                                    │   │
│  └─────────────────────────┘  └────────────────────────────────────┘   │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile (Tabbed View)

```
┌══════════════════════════════┐
│  ← Hasil           🧪   🌙  │
├══════════════════════════════┤
│                              │
│  Status Dokumen:             │
│  ┌────────────────────────┐  │
│  │        MIXED           │  │
│  └────────────────────────┘  │
│                              │
│  ████████████░░░░░░░░        │
│  🟢 60%  🔴 30%  ⚪ 10%     │
│                              │
│  ┌────────────┬───────────┐  │
│  │  📊 Ringkas │ 📝 Kalimat│  │
│  ├────────────┴───────────┤  │
│  │                        │  │
│  │  ┌──────────────────┐  │  │
│  │  │ 🟢 "Pendidikan   │  │  │
│  │  │ di Indonesia     │  │  │
│  │  │ mengalami..."    │  │  │
│  │  │ Skor: 0.15       │  │  │
│  │  └──────────────────┘  │  │
│  │                        │  │
│  │  ┌──────────────────┐  │  │
│  │  │ 🔴 "Dengan       │  │  │
│  │  │ demikian, dapat  │  │  │
│  │  │ disimpulkan..."  │  │  │
│  │  │ Skor: 0.87       │  │  │
│  │  └──────────────────┘  │  │
│  │                        │  │
│  │  ┌──────────────────┐  │  │
│  │  │ ⚪ "Hal tersebut │  │  │
│  │  │ perlu dikaji..." │  │  │
│  │  │ Skor: 0.48       │  │  │
│  │  └──────────────────┘  │  │
│  │                        │  │
│  └────────────────────────┘  │
│                              │
│  FILTER:                     │
│  [🟢ALL] [🔴AI] [⚪UNSURE]  │
│                              │
│  ⚠️ Estimasi model, bukan   │
│  bukti pasti.                │
│                              │
│  ┌────────────────────────┐  │
│  │   🔍 Analisis Baru     │  │
│  └────────────────────────┘  │
│  ┌────────────────────────┐  │
│  │   ✍️ Coba Humanize     │  │
│  └────────────────────────┘  │
│                              │
├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │
└══════════════════════════════┘
```

### Detector — Full User Journey

```mermaid
flowchart TD
    START["👤 User membuka TextLab"] --> LAND["🏠 Landing Page"]
    LAND --> INPUT["Paste/ketik teks"]
    INPUT --> VALIDATE{"Validasi Input"}

    VALIDATE -->|"< 50 karakter"| ERR_SHORT["⚠️ Teks terlalu pendek"]
    ERR_SHORT --> INPUT
    VALIDATE -->|"Kosong"| ERR_EMPTY["⚠️ Masukkan teks"]
    ERR_EMPTY --> INPUT
    VALIDATE -->|">= 50 karakter"| CHECK_QUOTA{"Cek Kuota"}

    CHECK_QUOTA -->|"kuota > 0"| SUBMIT["🔍 Klik Analisis"]
    CHECK_QUOTA -->|"kuota == 0"| QUOTA_WALL["🚫 Kuota Habis"]
    QUOTA_WALL --> COMEBACK["Kembali besok"]

    SUBMIT --> LOADING["⏳ Loading State"]
    LOADING --> API{"API Response?"}

    API -->|"200 OK"| RESULT["📊 Results Page"]
    API -->|"429"| ERR_RATE["⚠️ Rate limited"]
    API -->|"500"| ERR_SERVER["⚠️ Server error"]
    API -->|"Timeout"| ERR_TIMEOUT["⚠️ Request timeout"]

    ERR_RATE --> RETRY["Tunggu & coba lagi"]
    ERR_SERVER --> RETRY
    ERR_TIMEOUT --> RETRY
    RETRY --> SUBMIT

    RESULT --> VIEW_SUMMARY["Lihat ringkasan"]
    RESULT --> VIEW_HIGHLIGHT["Lihat highlight kalimat"]
    RESULT --> FILTER["Filter by label"]
    RESULT --> CLICK_SENTENCE["Klik kalimat → detail"]

    VIEW_SUMMARY --> ACTION{"Aksi Selanjutnya?"}
    VIEW_HIGHLIGHT --> ACTION
    FILTER --> ACTION
    CLICK_SENTENCE --> ACTION

    ACTION -->|"Analisis Baru"| INPUT
    ACTION -->|"Coba Humanize"| HUMANIZER["✍️ Humanizer"]
    ACTION -->|"Tutup"| LAND
```

---

## 6. Screen 3 — Humanizer Input

### Desktop

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │      [👤 Pengguna]  🌙 │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│                       ✍️ AI Humanizer                                   │
│                 "Sesuaikan gaya penulisanmu"                            │
│                                                                        │
│     ┌──────────────────────────────────────────────────────────┐        │
│     │  📌 PILIH GAYA PENULISAN                                 │        │
│     │                                                          │        │
│     │  ┌─────────────────────────────────────────────────────┐ │        │
│     │  │ ◉ Profil Personal                                   │ │        │
│     │  │   🎨 "Gaya Skripsi"  ▾  (dropdown profil)           │ │        │
│     │  │   [+ Buat Profil Baru]  [Kelola Profil →]           │ │        │
│     │  └─────────────────────────────────────────────────────┘ │        │
│     │                                                          │        │
│     │  ── atau pilih gaya pustaka ──                           │        │
│     │                                                          │        │
│     │  ┌────────────┐ ┌────────────┐ ┌────────────┐           │        │
│     │  │ ○ 🎓       │ │ ○ 💬       │ │ ○ 📰       │           │        │
│     │  │ Akademik   │ │ Kasual     │ │ Jurnalistik│           │        │
│     │  └────────────┘ └────────────┘ └────────────┘           │        │
│     │  ┌────────────┐ ┌────────────┐                           │        │
│     │  │ ○ 💼       │ │ ○ 📖       │                           │        │
│     │  │ Bisnis     │ │ Naratif    │                           │        │
│     │  └────────────┘ └────────────┘                           │        │
│     └──────────────────────────────────────────────────────────┘        │
│                                                                        │
│     ┌──────────────────────────────────────────────────────────┐        │
│     │                                                          │        │
│     │  Tempelkan teks yang ingin di-humanize...                │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     │                                                          │        │
│     ├──────────────────────────────────────────────────────────┤        │
│     │  0 karakter                                              │        │
│     └──────────────────────────────────────────────────────────┘        │
│                                                                        │
│                    ┌─────────────────────────┐                         │
│                    │   ✍️  Humanize Teks      │                         │
│                    └─────────────────────────┘                         │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile

```
┌══════════════════════════════┐
│  🧪 TextLab         [≡] [🌙]│
├══════════════════════════════┤
│                              │
│  ✍️ AI Humanizer             │
│  "Sesuaikan gaya             │
│   penulisanmu"               │
│                              │
│  ┌────────────────────────┐  │
│  │ 📌 PILIH GAYA          │  │
│  │                        │  │
│  │ ◉ Profil Personal      │  │
│  │   "Gaya Skripsi"  ▾    │  │
│  │   [+ Buat] [Kelola →]  │  │
│  │                        │  │
│  │ ── atau pustaka ──     │  │
│  │                        │  │
│  │ ┌──────┐ ┌──────┐      │  │
│  │ │🎓    │ │💬    │      │  │
│  │ │Akade.│ │Kasual│      │  │
│  │ └──────┘ └──────┘      │  │
│  │ ┌──────┐ ┌──────┐      │  │
│  │ │📰    │ │💼    │      │  │
│  │ │Jurna.│ │Bisnis│      │  │
│  │ └──────┘ └──────┘      │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │                        │  │
│  │  Tempelkan teks...     │  │
│  │                        │  │
│  │                        │  │
│  ├────────────────────────┤  │
│  │  0 karakter            │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │  ✍️  Humanize Teks     │  │
│  └────────────────────────┘  │
│                              │
├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │
└══════════════════════════════┘
```

### Humanizer — Full User Journey

```mermaid
flowchart TD
    START["User buka Humanizer"] --> AUTH{"Sudah login?"}

    AUTH -->|"Belum"| SHOW_GUEST["Tampilkan halaman<br/>dengan prompt login"]
    SHOW_GUEST --> GUEST_ACTION{"Aksi user?"}
    GUEST_ACTION -->|"Pilih Profil Personal"| LOGIN_PROMPT["🔐 Login dulu"]
    GUEST_ACTION -->|"Pilih Gaya Pustaka"| SELECT_LIB["Pilih gaya pustaka"]
    LOGIN_PROMPT --> LOGIN["Login / Register"]
    LOGIN -->|"Berhasil"| AUTH_OK["Redirect ke Humanizer"]
    AUTH_OK --> SELECT

    AUTH -->|"Sudah"| SELECT{"Pilih Gaya"}

    SELECT --> PERSONAL["◉ Profil Personal"]
    SELECT --> LIBRARY["◉ Gaya Pustaka"]
    SELECT_LIB --> LIBRARY

    PERSONAL --> HAS_PROFILE{"Punya profil?"}
    HAS_PROFILE -->|"Ya"| PICK_PROFILE["Pilih dari dropdown"]
    HAS_PROFILE -->|"Tidak"| CREATE_PROFILE["+ Buat Profil Baru"]
    CREATE_PROFILE --> PROFILE_FLOW["Profile Creation Flow"]
    PROFILE_FLOW --> PICK_PROFILE

    LIBRARY --> PICK_STYLE["Pilih: Akademik / Kasual / dll"]

    PICK_PROFILE --> INPUT_TEXT["Input teks"]
    PICK_STYLE --> INPUT_TEXT

    INPUT_TEXT --> VALIDATE{"Validasi"}
    VALIDATE -->|"OK"| SUBMIT["✍️ Humanize"]
    VALIDATE -->|"Terlalu pendek"| INPUT_TEXT

    SUBMIT --> LOADING["⏳ Processing..."]
    LOADING --> API{"API Response?"}

    API -->|"Sukses"| RESULT["🔄 Before/After"]
    API -->|"Error"| ERROR["⚠️ Error"]
    ERROR --> SUBMIT

    RESULT --> COPY["📋 Salin hasil"]
    RESULT --> REDO["✍️ Humanize ulang"]
    REDO --> SUBMIT
    RESULT --> NEW["Input teks baru"]
    NEW --> INPUT_TEXT
```

---

## 7. Screen 4 — Humanizer Results

### Desktop (Side-by-Side Diff)

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │      [👤 Pengguna]  🌙 │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│  ← Kembali                 Hasil Humanize                              │
│                                                                        │
│  Gaya: 🎓 Akademik  ·  Makna ✅  ·  Kutipan ✅  ·  Struktur ✅         │
│                                                                        │
│  ┌─────────────────────────────┐  ┌─────────────────────────────┐      │
│  │  📄 ORIGINAL                │  │  ✍️ HUMANIZED                │      │
│  │                             │  │                             │      │
│  │  Dengan demikian, dapat     │  │  Oleh karena itu, bisa      │      │
│  │  disimpulkan bahwa          │  │  kita simpulkan bahwa       │      │
│  │  ~~transformasi digital~~   │  │  **perubahan teknologi**    │      │
│  │  telah memberikan           │  │  sudah memberikan           │      │
│  │  ~~dampak signifikan~~      │  │  **pengaruh yang cukup      │      │
│  │  terhadap berbagai aspek    │  │  besar** terhadap banyak    │      │
│  │  kehidupan masyarakat       │  │  sisi kehidupan masyarakat  │      │
│  │  ~~Indonesia secara         │  │  **di Indonesia, terutama   │      │
│  │  komprehensif~~.            │  │  dalam hal menyeluruh**.    │      │
│  │                             │  │                             │      │
│  │  ~~Perlu dicatat bahwa~~    │  │  **Penting untuk diingat    │      │
│  │  implementasi kebijakan     │  │  bahwa** penerapan aturan   │      │
│  │  ini memerlukan sinergi     │  │  ini butuh kerja sama       │      │
│  │  antar pemangku             │  │  antar pihak yang           │      │
│  │  kepentingan.               │  │  berkepentingan.            │      │
│  │                             │  │                             │      │
│  │  ~~strikethrough~~ =        │  │  **bold** = bagian baru     │      │
│  │  bagian yang berubah        │  │                             │      │
│  └─────────────────────────────┘  └─────────────────────────────┘      │
│                                                                        │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐      │
│  │  📋 Salin Hasil   │  │  ✍️ Humanize Ulang│  │  🔍 Cek Detector │      │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘      │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile (Tab Toggle)

```
┌══════════════════════════════┐
│  ← Hasil           🧪   🌙  │
├══════════════════════════════┤
│                              │
│  Gaya: 🎓 Akademik          │
│  Makna ✅ Kutipan ✅          │
│                              │
│  ┌───────────┬────────────┐  │
│  │ 📄 Original│✍️ Humanized│  │
│  ├───────────┴────────────┤  │
│  │                        │  │
│  │  Oleh karena itu,      │  │
│  │  bisa kita simpulkan   │  │
│  │  bahwa **perubahan     │  │
│  │  teknologi** sudah     │  │
│  │  memberikan            │  │
│  │  **pengaruh yang       │  │
│  │  cukup besar**         │  │
│  │  terhadap banyak sisi  │  │
│  │  kehidupan masyarakat  │  │
│  │  **di Indonesia**.     │  │
│  │                        │  │
│  │  **Penting untuk       │  │
│  │  diingat bahwa**       │  │
│  │  penerapan aturan      │  │
│  │  ini butuh kerja       │  │
│  │  sama antar pihak      │  │
│  │  yang berkepentingan.  │  │
│  │                        │  │
│  │  **bold** = baru       │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │   📋 Salin Hasil       │  │
│  └────────────────────────┘  │
│  ┌────────────────────────┐  │
│  │   ✍️ Humanize Ulang    │  │
│  └────────────────────────┘  │
│  ┌────────────────────────┐  │
│  │   🔍 Cek Detector      │  │
│  └────────────────────────┘  │
│                              │
├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │
└══════════════════════════════┘
```

---

## 8. Screen 5 — DOCX Metadata Cleaner

### Desktop — Step 1: Upload

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │        [Login]  🌙  │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│                     📄 Metadata Cleaner                                 │
│              "Bersihkan jejak metadata dokumenmu"                       │
│                                                                        │
│     ┌──────────────────────────────────────────────────────────┐        │
│     │                                                          │        │
│     │                         📁                               │        │
│     │                                                          │        │
│     │             Seret file DOCX ke sini                      │        │
│     │                  atau                                    │        │
│     │            ┌──────────────────┐                          │        │
│     │            │   Pilih File...   │                          │        │
│     │            └──────────────────┘                          │        │
│     │                                                          │        │
│     │         Maks. 10MB  ·  Format: .docx saja                │        │
│     │                                                          │        │
│     └──────────────────────────────────────────────────────────┘        │
│                                                                        │
│     ⚠️ Metadata cleaner tidak menjamin penghapusan seluruh             │
│        jejak digital. Lihat ketentuan penggunaan.                      │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Desktop — Step 2: Preview & Clean

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │        [Login]  🌙  │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│  ← Kembali               Preview Metadata                              │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────┐       │
│  │  📄 laporan-skripsi-final.docx                    3.2 MB    │       │
│  └──────────────────────────────────────────────────────────────┘       │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────┐       │
│  │  METADATA DITEMUKAN                                          │       │
│  │                                                              │       │
│  │  ┌─────────────────────────────────────────────────────────┐ │       │
│  │  │  ☑️  Author              │  Nama Penulis               │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Last Modified By    │  Dosen Pembimbing           │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Created             │  2026-03-15T10:30:00        │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Last Modified       │  2026-10-20T14:22:00        │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Company             │  Universitas XYZ            │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Manager             │  -                          │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Revision Number     │  47                         │ │       │
│  │  ├─────────────────────────────────────────────────────────┤ │       │
│  │  │  ☑️  Comments            │  "Draft v3 - revisi bab 4"  │ │       │
│  │  └─────────────────────────────────────────────────────────┘ │       │
│  │                                                              │       │
│  │  ☑️ Pilih Semua    □ Batal Semua                              │       │
│  │                                                              │       │
│  │  ┌───────────────────────────────┐                           │       │
│  │  │  🧹 Bersihkan & Download      │                           │       │
│  │  └───────────────────────────────┘                           │       │
│  └──────────────────────────────────────────────────────────────┘       │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile — Upload + Preview

```
┌══════════════════════════════┐     ┌══════════════════════════════┐
│  🧪 TextLab         [≡] [🌙]│     │  ← Preview          🧪  🌙  │
├══════════════════════════════┤     ├══════════════════════════════┤
│                              │     │                              │
│  📄 Metadata Cleaner         │     │  📄 laporan-final.docx       │
│  "Bersihkan metadata"        │     │     3.2 MB                   │
│                              │     │                              │
│  ┌────────────────────────┐  │     │  METADATA:                   │
│  │                        │  │     │  ┌────────────────────────┐  │
│  │       📁               │  │     │  │ ☑️ Author              │  │
│  │                        │  │     │  │   Nama Penulis         │  │
│  │  Seret file DOCX       │  │     │  ├────────────────────────┤  │
│  │  atau [Pilih File]     │  │     │  │ ☑️ Last Modified By    │  │
│  │                        │  │     │  │   Dosen Pembimbing     │  │
│  │  Maks. 10MB            │  │     │  ├────────────────────────┤  │
│  │                        │  │     │  │ ☑️ Created             │  │
│  └────────────────────────┘  │     │  │   2026-03-15           │  │
│                              │     │  ├────────────────────────┤  │
│  ⚠️ Tidak menjamin          │     │  │ ☑️ Company             │  │
│  penghapusan seluruh         │     │  │   Universitas XYZ      │  │
│  jejak digital.              │     │  ├────────────────────────┤  │
│                              │     │  │ ☑️ Comments            │  │
│                              │     │  │   "Draft v3"           │  │
│                              │     │  └────────────────────────┘  │
│                              │     │                              │
│                              │     │  ☑️ Pilih Semua             │
│                              │     │                              │
│                              │     │  ┌────────────────────────┐  │
│                              │     │  │ 🧹 Bersihkan & Download│  │
│                              │     │  └────────────────────────┘  │
│                              │     │                              │
├══════════════════════════════┤     ├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │     │  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │     │ Detect  Human  Clean  Akun   │
└══════════════════════════════┘     └══════════════════════════════┘
       (Step 1: Upload)                    (Step 2: Preview)
```

### Cleaner — Full Flow

```mermaid
flowchart TD
    START["User buka Cleaner"] --> UPLOAD_PAGE["📤 Upload Page"]

    UPLOAD_PAGE --> DRAG["Drag & Drop file"]
    UPLOAD_PAGE --> CLICK["Klik Pilih File"]
    DRAG --> VALIDATE
    CLICK --> VALIDATE

    VALIDATE{"Validasi File"}
    VALIDATE -->|"Bukan .docx"| ERR_FORMAT["⚠️ Format tidak didukung"]
    VALIDATE -->|"> 10MB"| ERR_SIZE["⚠️ File terlalu besar"]
    VALIDATE -->|"OK"| UPLOADING["⬆️ Uploading..."]

    ERR_FORMAT --> UPLOAD_PAGE
    ERR_SIZE --> UPLOAD_PAGE

    UPLOADING --> PARSE["Parse metadata"]
    PARSE --> PREVIEW["🔍 Preview Metadata"]

    PREVIEW --> SELECT["User pilih metadata"]
    SELECT --> SELECT_ALL["☑️ Pilih Semua"]
    SELECT --> SELECT_SOME["Pilih sebagian"]
    SELECT --> DESELECT["Batal pilih"]

    SELECT_ALL --> CONFIRM["🧹 Klik Bersihkan"]
    SELECT_SOME --> CONFIRM
    DESELECT --> CANCEL["← Kembali"]
    CANCEL --> UPLOAD_PAGE

    CONFIRM --> PROCESSING["⚙️ Processing..."]
    PROCESSING --> SUCCESS{"Berhasil?"}

    SUCCESS -->|"Ya"| DOWNLOAD["📥 Auto-download file bersih"]
    SUCCESS -->|"Gagal"| ERR_PROCESS["⚠️ Error processing"]

    DOWNLOAD --> DONE["✅ Selesai"]
    DONE --> ANOTHER["Upload file lain"]
    ANOTHER --> UPLOAD_PAGE
    ERR_PROCESS --> PREVIEW
```

---

## 9. Screen 6 — Auth (Login & Register)

### Desktop — Modal Overlay

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │        [Login]  🌙  │
├══════════════════════════════════════════════════════════════════════════┤
│  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│  ░░░░░░░░  ┌─────────────────────────────────────────┐  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                   [✕]   │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │          🧪 Masuk ke TextLab             │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  ┌───────────────────────────────────┐  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  │ 📧 Email                          │  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  └───────────────────────────────────┘  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  ┌───────────────────────────────────┐  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  │ 🔒 Password                  [👁] │  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  └───────────────────────────────────┘  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  [Lupa password?]                       │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  ┌───────────────────────────────────┐  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  │           🔐 Masuk                │  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  └───────────────────────────────────┘  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  ──────── atau ────────                 │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  ┌───────────────────────────────────┐  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  │  G  Lanjutkan dengan Google       │  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  └───────────────────────────────────┘  │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │  Belum punya akun? [Daftar]             │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  │                                         │  ░░░░░░░░░░░░  │
│  ░░░░░░░░  └─────────────────────────────────────────┘  ░░░░░░░░░░░░  │
│  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile — Full Page

```
┌══════════════════════════════┐     ┌══════════════════════════════┐
│  ← Masuk           🧪       │     │  ← Daftar          🧪       │
├══════════════════════════════┤     ├══════════════════════════════┤
│                              │     │                              │
│         🧪                   │     │         🧪                   │
│      TextLab                 │     │      TextLab                 │
│                              │     │                              │
│  Masuk ke akunmu             │     │  Buat akun baru              │
│                              │     │                              │
│  ┌────────────────────────┐  │     │  ┌────────────────────────┐  │
│  │ 📧 Email               │  │     │  │ 👤 Nama Lengkap        │  │
│  └────────────────────────┘  │     │  └────────────────────────┘  │
│                              │     │                              │
│  ┌────────────────────────┐  │     │  ┌────────────────────────┐  │
│  │ 🔒 Password       [👁] │  │     │  │ 📧 Email               │  │
│  └────────────────────────┘  │     │  └────────────────────────┘  │
│                              │     │                              │
│  [Lupa password?]            │     │  ┌────────────────────────┐  │
│                              │     │  │ 🔒 Password       [👁] │  │
│  ┌────────────────────────┐  │     │  └────────────────────────┘  │
│  │      🔐 Masuk          │  │     │                              │
│  └────────────────────────┘  │     │  ┌────────────────────────┐  │
│                              │     │  │ 🔒 Konfirmasi     [👁] │  │
│  ──────── atau ────────      │     │  └────────────────────────┘  │
│                              │     │                              │
│  ┌────────────────────────┐  │     │  ☑️ Saya setuju dengan      │
│  │ G Lanjutkan dgn Google │  │     │  ketentuan layanan          │
│  └────────────────────────┘  │     │                              │
│                              │     │  ┌────────────────────────┐  │
│  Belum punya akun? [Daftar]  │     │  │     📝 Daftar          │  │
│                              │     │  └────────────────────────┘  │
│                              │     │                              │
│                              │     │  ──────── atau ────────      │
│                              │     │                              │
│                              │     │  ┌────────────────────────┐  │
│                              │     │  │ G Lanjutkan dgn Google │  │
│                              │     │  └────────────────────────┘  │
│                              │     │                              │
│                              │     │  Sudah punya akun? [Masuk]  │
│                              │     │                              │
├══════════════════════════════┤     ├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │     │  🔍      ✍️      📄     👤  │
└══════════════════════════════┘     └══════════════════════════════┘
         (Login)                            (Register)
```

### Auth — State Machine

```mermaid
stateDiagram-v2
    [*] --> Idle : Page load

    Idle --> Filling : User mulai isi form
    Filling --> Validating : Submit
    Validating --> FormError : Validasi gagal
    FormError --> Filling : Perbaiki input
    Validating --> Submitting : Validasi OK

    Submitting --> Success : 200 OK
    Submitting --> AuthError : 401/403
    Submitting --> ServerError : 500
    Submitting --> RateLimit : 429

    AuthError --> Filling : Email/password salah
    ServerError --> Filling : Coba lagi
    RateLimit --> Cooldown : Tunggu 60 detik
    Cooldown --> Filling : Timer selesai

    Success --> Redirect : Redirect ke halaman asal
    Redirect --> [*]

    note right of FormError
        - Email format invalid
        - Password < 8 karakter
        - Password tidak cocok (register)
        - Terms belum dicentang
    end note
```

---

## 10. Screen 7 — Dashboard & Voice Profiles

### Desktop — Dashboard Overview

```
┌══════════════════════════════════════════════════════════════════════════┐
│  🧪 TextLab     │ Detector │ Humanizer │ Cleaner │      [👤 Pengguna]  🌙 │
├══════════════════════════════════════════════════════════════════════════┤
│                                                                        │
│  ┌────────────┐                                                        │
│  │ 👤         │  Nama Penulis                                           │
│  │            │  nama@example.com                                       │
│  │ [Avatar]   │  Bergabung: Nov 2026                                   │
│  └────────────┘                                                        │
│                                                                        │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐        │
│  │  🎨 Voice        │  │  📊 Riwayat      │  │  ⚙️ Settings     │        │
│  │  Profiles        │  │  Penggunaan     │  │                 │        │
│  │                  │  │                 │  │  Tema, privasi, │        │
│  │  2 profil aktif  │  │  14 deteksi     │  │  akun           │        │
│  │                  │  │  8 humanize     │  │                 │        │
│  │  [Kelola →]      │  │  [Lihat →]      │  │  [Buka →]       │        │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘        │
│                                                                        │
│  ── Voice Profiles ────────────────────────────────────────────        │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────┐       │
│  │  🎨  "Gaya Skripsi"                                         │       │
│  │                                                              │       │
│  │  Formal · Akademik · Bahasa baku                             │       │
│  │  5 sampel · Dibuat: 01 Okt 2026                              │       │
│  │                                                    [Edit] [🗑]│       │
│  ├──────────────────────────────────────────────────────────────┤       │
│  │  🎨  "Gaya Blog"                                             │       │
│  │                                                              │       │
│  │  Semi-formal · Conversational · Santai                       │       │
│  │  3 sampel · Dibuat: 15 Okt 2026                              │       │
│  │                                                    [Edit] [🗑]│       │
│  ├──────────────────────────────────────────────────────────────┤       │
│  │                                                              │       │
│  │            ┌──────────────────────────┐                      │       │
│  │            │  + Buat Profil Baru       │                      │       │
│  │            └──────────────────────────┘                      │       │
│  │                                                              │       │
│  └──────────────────────────────────────────────────────────────┘       │
│                                                                        │
│  ── Riwayat Terbaru ──────────────────────────────────────────         │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────┐       │
│  │  🔍 Deteksi  ·  09 Nov 2026 12:30  ·  MIXED  ·  347 char    │       │
│  │  ✍️ Humanize ·  08 Nov 2026 09:15  ·  Akademik ·  512 char  │       │
│  │  📄 Cleaner  ·  07 Nov 2026 16:45  ·  laporan.docx          │       │
│  │                                            [Lihat Semua →]   │       │
│  └──────────────────────────────────────────────────────────────┘       │
│                                                                        │
├══════════════════════════════════════════════════════════════════════════┤
│  FOOTER                                                                │
└══════════════════════════════════════════════════════════════════════════┘
```

### Mobile — Dashboard

```
┌══════════════════════════════┐
│  🧪 TextLab         [≡] [🌙]│
├══════════════════════════════┤
│                              │
│  ┌──────┐                    │
│  │ 👤   │  Nama Penulis       │
│  └──────┘  nama@example.com   │
│                              │
│  ┌──────┐ ┌──────┐ ┌──────┐ │
│  │🎨    │ │📊    │ │⚙️    │ │
│  │Profil│ │Histor│ │Sett. │ │
│  │  2   │ │  14  │ │      │ │
│  └──────┘ └──────┘ └──────┘ │
│                              │
│  ── Voice Profiles ──        │
│                              │
│  ┌────────────────────────┐  │
│  │ 🎨 "Gaya Skripsi"      │  │
│  │ Formal · Akademik       │  │
│  │ 5 sampel · 01 Okt 2026 │  │
│  │            [Edit] [🗑]  │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │ 🎨 "Gaya Blog"         │  │
│  │ Semi-formal · Santai    │  │
│  │ 3 sampel · 15 Okt 2026 │  │
│  │            [Edit] [🗑]  │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │  + Buat Profil Baru    │  │
│  └────────────────────────┘  │
│                              │
│  ── Riwayat Terbaru ──      │
│                              │
│  🔍 09 Nov · MIXED · 347ch  │
│  ✍️ 08 Nov · Akademik       │
│  📄 07 Nov · laporan.docx   │
│             [Lihat Semua →]  │
│                              │
├══════════════════════════════┤
│  🔍      ✍️      📄     👤  │
│ Detect  Human  Clean  Akun   │
└══════════════════════════════┘
```

### Voice Profile Creation — Step-by-Step

```mermaid
flowchart TD
    START["Klik + Buat Profil Baru"] --> STEP1

    subgraph STEP1["Step 1: Identitas"]
        NAME["Input nama profil<br/>contoh: Gaya Skripsi"]
        DESC["Deskripsi singkat opsional"]
        NAME --> DESC
    end

    STEP1 --> STEP2

    subgraph STEP2["Step 2: Upload Sampel"]
        UPLOAD["Upload 3-5 sampel tulisan"]
        FORMAT["Format: .txt / .docx / paste"]
        COUNTER["Sampel: 0/3 min, 5 maks"]
        UPLOAD --> FORMAT --> COUNTER
    end

    STEP2 --> STEP3

    subgraph STEP3["Step 3: Analisis"]
        ANALYZING["⏳ Menganalisis gaya penulisan..."]
        EXTRACT["Ekstrak: kosakata, ritme,<br/>formalitas, struktur, nada"]
        ANALYZING --> EXTRACT
    end

    STEP3 --> STEP4

    subgraph STEP4["Step 4: Review Profil"]
        PREVIEW["Preview profil yang dihasilkan"]
        TRAITS["Traits: formal, akademik,<br/>kalimat panjang, baku"]
        EDIT["User bisa edit/koreksi"]
        PREVIEW --> TRAITS --> EDIT
    end

    STEP4 --> CONFIRM{"Setuju?"}
    CONFIRM -->|"Ya"| SAVE["💾 Simpan profil"]
    CONFIRM -->|"Edit lagi"| STEP4

    SAVE --> CLEANUP["🗑️ Hapus sampel mentah<br/>default, kecuali user opt-in"]
    CLEANUP --> DONE["✅ Profil siap digunakan!"]
    DONE --> REDIRECT["Redirect ke Humanizer<br/>dengan profil terpilih"]
```

---

## 11. Kuota & Access Control — Flow

```mermaid
flowchart TD
    USER["👤 User Request"] --> FEATURE{"Fitur apa?"}

    FEATURE -->|"Detector"| DET_AUTH{"Login?"}
    DET_AUTH -->|"Tidak perlu"| DET_QUOTA{"Kuota tersisa?"}
    DET_AUTH -->|"Login opsional"| DET_QUOTA

    DET_QUOTA -->|"3/3"| DET_OK["✅ Proses - kuota jadi 2/3"]
    DET_QUOTA -->|"2/3"| DET_OK2["✅ Proses - kuota jadi 1/3"]
    DET_QUOTA -->|"1/3"| DET_WARN["⚠️ Proses - kuota jadi 0/3"]
    DET_QUOTA -->|"0/3"| DET_BLOCK["🚫 Kuota habis"]

    DET_BLOCK --> AD_CHECK{"Iklan tersedia?"}
    AD_CHECK -->|"Ya (terverifikasi)"| WATCH_AD["Tonton iklan"]
    AD_CHECK -->|"Tidak"| COME_BACK["Kembali besok"]
    WATCH_AD --> DET_OK3["✅ Kuota +1"]

    FEATURE -->|"Humanizer"| HUM_AUTH{"Login?"}
    HUM_AUTH -->|"Belum"| HUM_LOGIN["🔐 Wajib login"]
    HUM_AUTH -->|"Sudah"| HUM_ACCESS{"Profil Personal?"}

    HUM_ACCESS -->|"Ya"| HUM_OK["✅ Proses"]
    HUM_ACCESS -->|"Gaya Pustaka"| HUM_AD{"Perlu iklan?"}
    HUM_AD -->|"Ya"| HUM_WATCH["Tonton iklan"]
    HUM_AD -->|"Belum ditentukan"| HUM_OK
    HUM_WATCH --> HUM_OK

    FEATURE -->|"Cleaner"| CLN_AD{"Perlu iklan?"}
    CLN_AD -->|"Ya"| CLN_WATCH["Tonton iklan"]
    CLN_AD -->|"Belum ditentukan"| CLN_OK["✅ Proses"]
    CLN_WATCH --> CLN_OK

    FEATURE -->|"Dashboard"| DASH_AUTH{"Login?"}
    DASH_AUTH -->|"Belum"| DASH_LOGIN["🔐 Wajib login"]
    DASH_AUTH -->|"Sudah"| DASH_OK["✅ Dashboard"]
```

---

## 12. Responsive Breakpoint Behavior

```mermaid
flowchart LR
    subgraph MOBILE["📱 Mobile - under 640px"]
        M1["Bottom Tab Bar"]
        M2["Single column"]
        M3["Hamburger drawer"]
        M4["Tabbed views"]
        M5["Full-page auth"]
        M6["Stacked cards"]
        M7["Top bar auto-hide"]
    end

    subgraph TABLET["📱 Tablet - 640px to 1024px"]
        T1["Bottom Tab Bar hidden"]
        T2["Top navbar visible"]
        T3["2-column grid"]
        T4["Side-by-side diff"]
        T5["Modal auth"]
        T6["Cards in 2-col grid"]
        T7["Sticky navbar"]
    end

    subgraph DESKTOP["💻 Desktop - over 1024px"]
        D1["Full top navbar"]
        D2["12-column grid"]
        D3["Two-panel results"]
        D4["Side-by-side diff"]
        D5["Modal auth overlay"]
        D6["Cards in 3-col grid"]
        D7["Sticky navbar + blur"]
    end

    MOBILE --> TABLET --> DESKTOP
```

### Component Behavior per Breakpoint

| Komponen | Mobile (< 640px) | Tablet (640–1024px) | Desktop (> 1024px) |
|---|---|---|---|
| **Navigation** | Bottom Tab Bar + Hamburger | Top Navbar | Top Navbar |
| **Detector Results** | Tabbed (Ringkasan / Kalimat) | Two-column | Two-column (sticky sidebar) |
| **Humanizer Results** | Tab Toggle (Original / Humanized) | Side-by-side | Side-by-side |
| **Feature Cards** | Stacked (1 column) | 2-column grid | 3-column grid |
| **Auth Form** | Full-page | Modal overlay | Modal overlay |
| **Dashboard Cards** | 3 mini cards in row | 3 cards in row | 3 cards in row |
| **Voice Profile List** | Full-width cards | Full-width cards | Full-width cards (max 800px) |
| **Textarea** | Full width, shorter | Full width | Max 720px, centered |
| **Footer** | Stacked, simplified | 4-column | 4-column |
| **Font sizes** | -1px to -2px smaller | Base size | Base size |

---

*Wireframes dan flowcharts ini merupakan rancangan visual untuk panduan diskusi dan implementasi. Dokumen ini belum membuktikan bahwa layar atau interaksi sudah dibuat. Detail arsitektur dan urutan proyek mengikuti [Product & System Design](PRODUCT-AND-SYSTEM-DESIGN.md), [Development Workflow](DEVELOPMENT-WORKFLOW.md), dan [Project Workflow](PROJECT-WORKFLOW.md).*