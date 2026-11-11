# 📝 Daily Work Report - Dedy S.N Putra (2 Oktober 2026)

---

## 📅 Laporan Harian - 2 Oktober 2026

---

## 🌿 Branch: `master` — Issue #346: Menu Sidebar Dinamis & Kustomisasi Logo Utama Aplikasi

### 📌 Informasi Issue

- **Nomor Issue**: #346
- **Judul Issue**: Menu Sidebar Dinamis (Dynamic Tree Reordering) & Kustomisasi Logo Utama Aplikasi / Favicon
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 2 Oktober 2026, 11:33:59 WIB via commit `a16a10cc`)

### 📅 Rincian Commit

#### [a16a10cc] - resolve #346 - Jumat, 2 Oktober 2026, 11:33:59 WIB

_(Merge commit menggabungkan branch `issue-346` / commit `e9850a9a` ke branch `master`)_

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `docs/superpowers/specs/2026-10-01-dynamic-sidebar-menu-design.md` [NEW]
  - `docs/superpowers/specs/2026-10-02-app-logo-design.md` [NEW]
  - `backend/src/app.js`
  - `backend/src/controllers/appLogo.controller.js` [NEW]
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/menuLayout.controller.js` [NEW]
  - `backend/src/controllers/utils.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/appLogo.route.js` [NEW]
  - `backend/src/routes/menuLayout.route.js` [NEW]
  - `backend/src/routes/public.route.js` [NEW]
  - `backend/src/services/appLogo.service.js` [NEW]
  - `backend/src/services/menuLayout.service.js` [NEW]
  - `backend/src/services/scheduler.service.js`
  - `backend/src/utils/menuLayoutValidator.js` [NEW]
  - `backend/test/integration/appLogo.controller.test.js` [NEW]
  - `backend/test/integration/appLogo.service.test.js` [NEW]
  - `backend/test/integration/menuLayout.controller.test.js` [NEW]
  - `backend/test/integration/menuLayout.service.test.js` [NEW]
  - `backend/test/unit/menuLayoutValidator.test.js` [NEW]
  - `frontend/package.json`
  - `frontend/package-lock.json`
  - `frontend/src/App.jsx`
  - `frontend/src/store.js`
  - `frontend/src/vitest.config.js`
  - `frontend/src/app/contexts/config/Provider.jsx`
  - `frontend/src/app/layouts/Root.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/Item.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/CollapsibleItem/index.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Header.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/index.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/CollapsibleItem/index.jsx`
  - `frontend/src/app/navigation/index.js`
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/navigation/menuIcons.js` [NEW]
  - `frontend/src/app/navigation/menuIcons.test.js` [NEW]
  - `frontend/src/app/pages/activities/scheduler/create.jsx`
  - `frontend/src/app/pages/activities/scheduler/detail.jsx`
  - `frontend/src/app/pages/activities/scheduler/edit.jsx`
  - `frontend/src/app/pages/settings/sections/MenuLayout.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx`
  - `frontend/src/app/pages/settings/menuEditor/AppLogoSettings.jsx` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/GroupDialog.jsx` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/IconPicker.jsx` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/MenuTree.jsx` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/MoveDialog.jsx` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/layoutOps.js` [NEW]
  - `frontend/src/app/pages/settings/menuEditor/layoutOps.test.js` [NEW]
  - `frontend/src/app/router/settings/settingsRoute.jsx`
  - `frontend/src/components/shared/AppLogo.jsx` [NEW]
  - `frontend/src/components/shared/AppLogo.test.jsx` [NEW]
  - `frontend/src/components/shared/form/CoverImageUpload.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/template/AppFavicon.jsx` [NEW]
  - `frontend/src/components/template/Search/index.jsx`
  - `frontend/src/components/template/SplashScreen.jsx`
  - `frontend/src/components/ui/Avatar/Avatar.jsx`
  - `frontend/src/features/menuLayoutSlice.js` [NEW]
  - `frontend/src/features/menuLayoutSlice.test.js` [NEW]
  - `frontend/src/hooks/useMenuTree.js` [NEW]
  - `frontend/src/hooks/useTicketBadge.js`
  - `frontend/src/hooks/useTicketBadge.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/appLogo.js` [NEW]
  - `frontend/src/utils/appLogo.test.js` [NEW]
  - `frontend/src/utils/menuTree.js` [NEW]
  - `frontend/src/utils/menuTree.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Menu Sidebar Dinamis Global (`backend/src/services/menuLayout.service.js`, `menuLayout.controller.js`, `menuLayout.route.js`)**:
    - Membangun backend service dan REST API untuk menyimpan tata letak menu sidebar terpusat dalam koleksi `Option` (`menu_layout`), yang berlaku secara global bagi seluruh administrator pada kedua mode sidebar (`main-layout` dan `sideblock`).
    - Mengimplementasikan skema pohon menu hingga kedalaman maksimal 3 level (`root` → `sub-grup` → `item`), mempertahankan integritas katalog bawaan dan memisahkan menu `Settings` agar tidak dapat terkunci atau hilang.
    - Menerapkan **Optimistic Locking** menggunakan field versi `__v` pada update payload; server menolak perubahan dengan HTTP `409 Conflict` jika terjadi benturan penyimpanan bersamaan oleh admin lain.
  - **Validasi Integritas Struktur Pohon Menu (`backend/src/utils/menuLayoutValidator.js`)**:
    - Membangun modul validasi mendalam yang memeriksa keabsahan payload susunan menu: pencegahan ID duplikat, verifikasi ID katalog statis yang terdaftar, pemblokiran siklus rekursif pada struktur bersarang, serta penolakan node yang melanggar batas kedalaman.
  - **Kustomisasi Logo Aplikasi & Favicon Dinamis (`backend/src/services/appLogo.service.js`, `appLogo.controller.js`, `appLogo.route.js`, `public.route.js`)**:
    - Membangun endpoint publik dan terproteksi untuk konfigurasi logo utama aplikasi (`GET /api/v1/app-logo`, `PUT /api/v1/app-logo`, `POST /api/v1/app-logo/reset`, dan `GET /public/app-logo`).
    - Mendukung dua mode logo: pemilihan simbol ikon Lucide dari registri internal atau pengunggahan file gambar kustom (SVG, PNG, JPG, WebP, ICO) ke penyimpanan MinIO.
    - Mengintegrasikan rute publik agar logo dan favicon dapat diakses oleh browser/klien sebelum proses autentikasi (misal pada splash screen awal dan favicon tab browser).
  - **Editor Visual Susunan Menu di Pengaturan (`frontend/src/app/pages/settings/sections/MenuLayout.jsx`, `MenuTree.jsx`)**:
    - Mengembangkan antarmuka editor susunan menu di panel **Pengaturan → Susunan Menu**.
    - Mendukung manipulasi struktur pohon melalui **Drag-and-Drop** interaktif menggunakan `@atlaskit/pragmatic-drag-and-drop` serta modal aksi alternatif non-drag (**MoveDialog**) demi kepatuhan aksesibilitas keyboard dan kenyamanan navigasi.
    - Menyediakan dialog pembuatan dan penyuntingan grup menu (**GroupDialog**) lengkap dengan dukungan judul dwibahasa (ID/EN), seleksi ikon grup (**IconPicker**) dari registri terkurasi (`menuIcons.js`), serta pengaturan divider garis pemisah.
    - Fitur **Reset ke Bawaan** untuk memulihkan susunan standar sistem kapan saja.
  - **Komponen Kustomisasi Logo & Favicon Frontend (`AppLogoSettings.jsx`, `AppLogo.jsx`, `AppFavicon.jsx`, `appLogo.js`)**:
    - Membangun komponen UI kartu pengaturan logo di menu Settings untuk memilih ikon atau mengunggah gambar logo.
    - Membangun komponen `AppLogo` serbaguna yang adaptif terhadap mode ikon maupun gambar, digunakan pada sidebar rail `main-layout`, header `sideblock`, dan splash screen pemuatan sistem.
    - Mengembangkan komponen reaktif `AppFavicon` yang secara dinamis memperbarui tag `<link rel="icon">` pada dokumen HTML sesuai logo aktif (termasuk rendering SVG on-the-fly dari Lucide Icon).
  - **Sinkronisasi Navigasi, Privilege & Badging (`useMenuTree.js`, `menuTree.js`, `useTicketBadge.js`, `menuLayoutSlice.js`)**:
    - Membuat Redux slice `menuLayoutSlice` untuk menampung tata letak menu aktif dan status sinkronisasi.
    - Memusatkan filtering privilege navigasi (sintaks tunggal maupun kombinasi `a|b`) ke dalam utilitas murni `menuTree.js`, menggantikan duplikasi pengecekan hak akses di 4 berkas komponen layout yang sebelumnya rawan inkonsistensi.
    - Menghubungkan kalkulasi badge notifikasi secara reaktif ke `useTicketBadge.js` sehingga penanda badge tiket tetap muncul tepat pada item menu meskipun item tersebut telah dipindahkan ke sub-grup baru.
  - **Rangkaian Pengujian Otomatis Menyeluruh**:
    - Menambahkan pengujian integrasi dan unit backend untuk `appLogo` dan `menuLayout` (`menuLayoutValidator.test.js`, `menuLayout.controller.test.js`, `appLogo.service.test.js`, dll.).
    - Menambahkan pengujian unit frontend untuk seluruh operasi pohon menu (`layoutOps.test.js`, `menuTree.test.js`), slice Redux, registri ikon, serta verifikasi rendering komponen `AppLogo`.

---

## 🌿 Branch: `production` — Mitigasi Rate Limit HTTP 429 & Penyempurnaan Rekonsiliasi Gateway iPaymu

### 📌 Informasi Issue

- **Nomor Issue**: Hotfix / Task Production
- **Judul Issue**: Perbaikan Bug Rate Limit HTTP 429, Circuit Breaker Defensif & Pacing Rekonsiliasi Payment Gateway iPaymu
- **Status Branch**: `Sudah di-merge` (Di-commit langsung pada branch `production` dan diselaraskan ke `master` via commit `084031b9`)

### 📅 Rincian Commit

#### [084031b9] - fix rekonsiliasi ipaymu - Jumat, 2 Oktober 2026, 10:22:26 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/utils/callWithRetry.js`
  - `backend/test/unit/paymentGatewayRetry.test.js`
  - `hasil-analisa-ai.md` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Analisis Akar Masalah SRE (`hasil-analisa-ai.md`)**:
    - Melakukan investigasi menyeluruh terhadap kegagalan massal rekonsiliasi pembayaran iPaymu dengan korelasi ID unik `97050ac2-66b7-440d-a373-9a5ec218dfb7`.
    - Mengidentifikasi bahwa eksekusi `Promise.all` paralel berukuran chunk dan interval request agresif (< 250ms) memicu penolakan HTTP 429 (Too Many Requests) dari API iPaymu secara beruntun (50x dalam ~7 detik), disusul kegagalan total batch rekonsiliasi akibat ketiadaan isolasi kegagalan per-baris.
  - **Pacing Rileks Antar-Request & Serial Execution (`backend/src/utils/callWithRetry.js`, `financeGateway.service.js`)**:
    - Mengganti eksekusi paralel chunking (`Promise.all`) menjadi **serial loop murni** (`concurrency = 1`) pada alur sapuan pending (`sweepPendingTransactions`), retry transaksi gagal, dan penarikan riwayat mutasi iPaymu (`runGatewayHistoryReconciliation`).
    - Menaikkan jeda aman antar-panggilan HTTP (`DEFAULT_INTER_REQUEST_DELAY_MS`) dari 250 ms menjadi **minimal 2000 ms (2 detik)** guna memastikan batas request rate yang ditetapkan gateway iPaymu tidak terlampaui.
    - Menyesuaikan parameter retry `callWithRetry`: mengurangi `maxRetries` dari 3 menjadi 2 kali, menaikkan `baseDelayMs` ke 2000 ms, dan batas atas backoff `maxDelayMs` ke 15000 ms.
  - **Circuit Breaker Defensif untuk Perlindungan IP Backend**:
    - Menambahkan mekanisme **Circuit Breaker** pada seluruh loop rekonsiliasi: begitu error status HTTP 429 terdeteksi (`isRateLimitError`), pemrosesan batch transaksi saat itu langsung dihentikan secara anggun (*graceful exit*).
    - Mekanisme ini mencegah backend membombardir gateway ketika kuota habis, memberikan jendela pendinginan (*cooldown*) bagi IP server backend agar tidak terkena sanksi blokir permanen dari pihak penyedia payment gateway.
  - **Diferensiasi Penanganan Error: Rate Limit vs Kesalahan Bisnis**:
    - **Kasus Terblokir iPaymu / HTTP 429**: Transaksi ditandai dengan `recon_status: 'RATE_LIMITED'`. Status ini **TIDAK** menaikkan counter percobaan (`recon_retry_count`) dan **TIDAK** terkena cooldown 12 jam, sehingga dapat langsung dicoba kembali pada jadwal cron berikutnya saat rate limit gateway sudah pulih.
    - **Kasus Kegagalan Bisnis Rekonsiliasi (non-429)**: Transaksi ditandai dengan status `recon_status: 'FAILED_NEED_MANUAL_REVIEW'`, nilai `recon_retry_count` dinaikkan secara bertahap, dan cron memberlakukan **cooldown 12 jam** (`RECON_FAILED_COOLDOWN_MS = 12 jam`) sebelum mencoba baris tersebut kembali.
    - **Batas Maksimal Percobaan Otomatis**: Memberlakukan batas maksimal **4 kali percobaan otomatis** (`RECON_MAX_AUTO_RETRIES = 4`) atau usia transaksi maksimal **2 hari / 48 jam** (`RECON_FAILED_MAX_AGE_MS = 2 hari`). Transaksi yang melampaui batas ini ditinggalkan oleh cron otomatis dan dialihkan untuk peninjauan manual administrator guna mencegah perulangan sia-sia.
  - **Pembaruan Skema Basis Data Transaksi (`models/financeGatewayTransaction.model.js`)**:
    - Menambahkan field `recon_retry_count` berjenis `Number` (default: 0) pada skema `FinanceGatewayTransaction`.
    - Memastikan field `recon_retry_count` di-reset kembali ke 0 ketika transaksi berhasil dibukukan (`SUCCESS`).
    - Memperbarui query monitoring status rekonsiliasi (`getGatewayReconciliationStatus`) agar mengekspos field `recon_retry_count` untuk transparansi pantauan di dashboard finance.
  - **Pengujian Unit Otomatis (`test/unit/paymentGatewayRetry.test.js`)**:
    - Memperbarui rangkaian uji unit berbasis `node:test` dan `node:assert` untuk memvalidasi:
      1. Kepatuhan jeda pacing minimal 2 detik antar-request.
      2. Penghentian seketika batch (circuit breaker) saat gateway merespons HTTP 429.
      3. Verifikasi alur retry count dan kriteria cooldown 12 jam untuk kegagalan bisnis.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #346  | Menu Sidebar Dinamis & Kustomisasi Logo Utama Aplikasi / Favicon | Admin dapat menata ulang struktur menu navigasi sidebar secara visual (drag-and-drop / modal), membuat grup & sub-grup hingga 3 level, memilih ikon Lucide, serta mengunggah logo kustom dan memperbarui favicon tab aplikasi secara dinamis. |
| Hotfix / Prod | Mitigasi Rate Limit HTTP 429 & Pacing Rekonsiliasi iPaymu | Mengeliminasi insiden kegagalan batch rekonsiliasi payment gateway dengan menerapkan serial execution, jeda request minimal 2 detik, circuit breaker saat 429, serta isolasi retry 12 jam untuk kegagalan bisnis. |

### Kemampuan Baru Pengguna/Admin

- **Kustomisasi Tata Letak Menu Sidebar**: Administrator dengan hak akses pengaturan kini dapat menyusun tata letak menu sidebar sesuai kebutuhan operasional perusahaan, termasuk memindahkan modul ke grup tertentu, membuat kategori kustom, dan menyematkan ikon representatif.
- **Kustomisasi Identitas Visual (Branding Logo & Favicon)**: Perusahaan dapat menggunakan logo merek sendiri pada sudut aplikasi (sidebar rail, header, dan splash screen) serta favicon browser, baik dengan memilih dari koleksi ratusan ikon vektor Lucide maupun mengunggah gambar logo perusahaan.
- **Pelacakan Transaksi Gateway yang Lebih Transparan**: Tim finance dapat melihat riwayat percobaan rekonsiliasi (`recon_retry_count`) dan membedakan transaksi yang tertunda akibat batasan rate limit gateway (`RATE_LIMITED`) versus transaksi yang memerlukan peninjauan manual pembukuan (`FAILED_NEED_MANUAL_REVIEW`).

### Bug Fix / Solusi Masalah

- **Eliminasi Error HTTP 429 (Too Many Requests)**: Menyelesaikan masalah spam request ke payment gateway iPaymu saat cron rekonsiliasi berjalan, menggantikan flood request paralel dengan antrean serial berjeda 2000 ms.
- **Pencegahan Kegagalan Total Batch Rekonsiliasi**: Satu transaksi bermasalah kini tidak lagi menggugurkan seluruh proses rekonsiliasi harian berkat isolasi *try-catch* per-baris dan *circuit breaker* saat deteksi penolakan gateway.
- **Pencegahan Perulangan Cron Sia-Sia**: Transaksi gagal bisnis kini diberi jeda penundaan 12 jam dan dibatasi maksimal 4 kali percobaan / 2 hari sebelum diserahkan ke penanganan manual, menghemat beban CPU server dan kuota request gateway.
- **Inkonsistensi Validasi Privilege Sidebar**: Penyeragaman logika verifikasi hak akses modul sidebar dari 4 implementasi terpisah menjadi modul terpadu `menuTree.js`, menyelesaikan *bug* evaluasi role gabungan (`a|b`) pada mode sideblock.

### Menu/Fitur Baru

- **Menu Pengaturan Susunan Menu (`/settings/menu-layout`)**: Halaman khusus di dalam modul Pengaturan untuk mengelola struktur pohon menu sidebar, dialog pembuatan grup, pemilih ikon, dan reset ke susunan bawaan.
- **Kartu Pengaturan Logo Aplikasi di Menu Settings**: Bagian antarmuka untuk mengganti logo aplikasi dengan pemilih ikon Lucide interaktif atau unggah berkas gambar (SVG/PNG/JPG/WebP/ICO).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penataan Ulang Susunan Menu Sidebar Dinamis

- **Penjelasan Fitur**: Fitur ini memungkinkan Superadmin/Admin mengatur posisi dan hirarki menu sidebar (hingga 3 level: Root Menu → Sub-grup → Item Menu). Perubahan disimpan secara global di basis data dan langsung diselaraskan ke seluruh akun pengguna pada mode sidebar `main-layout` maupun `sideblock`.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi sebagai Admin, buka menu **Pengaturan** (Settings), lalu pilih submenu **Susunan Menu** (`/settings/menu-layout`).
  2. Untuk memindahkan posisi menu:
     - **Metode Drag-and-Drop**: Klik dan tahan ikon pegangan (drag handle) di sebelah kiri item menu, lalu geser ke posisi baru atau masukkan ke dalam grup target.
     - **Metode Tombol Pindah (Aksesibel)**: Klik tombol opsi/titik tiga pada baris menu → pilih **Pindahkan** → tentukan grup target dan posisi urutannya pada dialog pemindahan.
  3. Untuk membuat grup menu baru, klik tombol **Tambah Grup Baru** di bagian atas, masukkan nama grup (Bahasa Indonesia & Bahasa Inggris), pilih ikon representatif melalui pemilih ikon, lalu klik **Simpan**.
  4. Klik tombol **Simpan Perubahan** di pojok kanan atas untuk menerapkan tata letak baru secara permanen ke seluruh sistem. Jika ingin kembali ke struktur default pabrik, klik tombol **Reset ke Bawaan**.

### 2. Kustomisasi Logo Utama & Favicon Aplikasi

- **Penjelasan Fitur**: Modul ini memungkinkan pergantian identitas visual logo aplikasi yang tampil di bilah navigasi, splash screen awal, dan ikon tab browser (favicon) tanpa perlu melakukan kompilasi ulang kode frontend.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengaturan** → **Susunan Menu** (atau tab Logo Aplikasi).
  2. Pada kartu **Logo Aplikasi**:
     - **Opsi Ikon**: Pilih tab *Pilih Ikon*, cari simbol yang sesuai melalui kotak pencarian ikon Lucide, dan pilih warna atau bentuk yang diinginkan.
     - **Opsi Unggah Gambar**: Pilih tab *Unggah Gambar*, klik area unggah berkas (atau seret gambar logo format SVG/PNG/JPG/ICO maksimal 2MB), lalu tunggu proses upload selesai.
  3. Klik **Simpan Logo**. Logo pada sidebar rail, header atas, serta favicon tab browser akan langsung terbarui secara reaktif tanpa perlu *reload* halaman secara paksa.
