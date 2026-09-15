# 📝 Daily Work Report - Dedy S.N Putra (2026-09-15)

---

## 📅 Laporan Harian - 15 September 2026

---

## 🌿 Branch: `issue-297` — Implementasi Radius Authentication Non-Pelanggan (Internal & Pihak Luar)

### 📌 Informasi Issue

- **Nomor Issue**: #297
- **Judul Issue**: Implementasi Radius Authentication non Pelanggan (Internal & Pihak Luar)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan lokal dan remote `origin/issue-297`)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - Finalisasi Modul Radius Non-Pelanggan: Endpoint List-Status, Counter Cards, Pengkayaan Kolom Tabel & Standarisasi Form UI - 15 September 2026

- **Komponen yang Berubah**:
  - **Backend Core — Controller, Route, dan Integration Test**:
    - [`backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthenticationNonCustomer.controller.js) — Penambahan controller `getListStatusNonCustomerAuthentication`:
      - Mengomputasi agregasi status akun RADIUS non-pelanggan (`total`, `active`, `inactive`, `online`) menggunakan query filter terisolasi `{ customer: { $exists: false }, partner: { $exists: false }, ref_partner: { $exists: false } }`.
      - Menjamin ringkasan status akurat dan bebas dari campur aduk akun pelanggan maupun mitra.
    - [`backend/src/routes/radiusAuthenticationNonCustomer.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/radiusAuthenticationNonCustomer.route.js) — Penambahan rute `GET /api/v1/radius-non-customer/list-status`:
      - Dilindungi oleh middleware autentikasi `protectedAdmin` dan verifikasi hak akses granular `checkPrivilege('radiusNonCustomer.list')`.
      - Dilengkapi dokumentasi OpenAPI/Swagger lengkap dengan ringkasan status dan skema respons HTTP 200.
    - [`backend/test/integration/radiusAuthenticationNonCustomer.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthenticationNonCustomer.service.test.js) — Penambahan suite pengujian integrasi baru untuk memverifikasi fungsionalitas `findAuthenticationListStatus`:
      - Menguji komputasi metrik status akun (`total`, `active`, `inactive`, `online`) dengan isolasi penuh sehingga akun pelanggan bertransaksi tidak ikut terhitung ke dalam metrik non-pelanggan.
  - **Frontend — Tampilan List, Kartu Status, Kolom Tabel & Form Create/Edit**:
    - [`frontend/src/app/pages/network/radiusNonCustomer/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/index.jsx) — Peningkatan halaman daftar akun RADIUS non-pelanggan:
      - Penambahan 4 kartu widget ringkasan status akun secara responsif (`sm:grid-cols-4`):
        - **Terkoneksi (Online)**: Ikon `FaLink` berwarna hijau `text-success`.
        - **Total Akun**: Ikon `LuRadius` berwarna ungu `text-secondary`.
        - **Akun Aktif**: Ikon `CheckBadgeIcon` berwarna biru `text-primary-500`.
        - **Akun Tidak Aktif**: Ikon `NoSymbolIcon` berwarna oranye `text-warning`.
      - Penataan tata letak grid konten dengan transisi halus dan integrasi endpoint `/radius-non-customer/list-status`.
    - [`frontend/src/app/pages/network/radiusNonCustomer/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/schema/columns.jsx) — Pengkayaan definisi kolom TanStack Table:
      - Kolom Tanggal Dibuat (`created_at`) menggunakan helper `DateCell` dengan filter `dateRange`.
      - Kolom Status Konektivitas (`isOnline`) menggunakan `ConnectivityCell` dengan opsi filter dropdown `connectivityTypeOptions`.
      - Kolom Profil Paket Kecepatan (`profile`) terintegrasi interaktif dengan komponen drawer `BroadbandProfileDetailDrawer` untuk inspeksi cepat limit kecepatan profil langsung dari tabel.
      - Kolom Pemilik Akun (`owner`) dengan penanganan fallback cerdas: menampilkan komponen `AdminLink` jika ditautkan ke akun staf internal (`owner_admin`), atau teks tebal semantik jika diisi catatan teks bebas (`notes` pihak luar/vendor), serta fallback `-`.
      - Kolom Pembaruan Akuntansi Terakhir (`lastAccounting`) dan Terakhir Terhubung (`last_login`) dengan helper `DateCell` dan filter rentang tanggal.
    - [`frontend/src/app/pages/network/radiusNonCustomer/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/create.jsx) & [`frontend/src/app/pages/network/radiusNonCustomer/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/edit.jsx) — Perombakan tata letak formulir pembuatan dan pengubahan akun:
      - Mengadopsi arsitektur 2-kolom responsif modern (`col-span-12 xl:col-span-7` dan `col-span-12 xl:col-span-5`).
      - Pengelompokan field ke dalam kartu terstruktur (`Card`): Informasi Kredensial & Pengaturan Akun, Pengaturan Jaringan/IP Statis, dan Kartu Konfigurasi Kecepatan & Mikrotik Burst Limit.
      - Integrasi interaktif kalkulator Mikrotik Burst (`MdAutoGraph`, `InputCheckboxCustom`, `SpeedBadge`, `InputAppend`).
      - Penyematan indikator pemuatan `Spinner` pada tombol simpan untuk mencegah *double submit*.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Penambahan key translasi `radiusNonCustomerList` ("Daftar Akun Radius Non-Pelanggan") dan `editAccount` ("Ubah Akun Non-Pelanggan").

---

### 📅 Rincian Commit

#### [`a5b317a`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #297 - 15 September 2026, 17:35:05 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Arsitektur Sistem**:
    - [`docs/superpowers/specs/2026-09-15-radius-internal-auth-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-15-radius-internal-auth-design.md) [NEW] — Dokumen spesifikasi teknis lengkap mencakup latar belakang kebutuhan akses internet PPPoE non-pelanggan (karyawan, teknisi kontraktor, pihak luar), riset integrasi daemon Go `radiusd`, isolasi skema data, keputusan arsitektur data tanpa mencemari CRM/billing, serta mekanisme keamanan.
  - **Backend Core — Data Model, Helper Service, Controller & Privileges**:
    - [`backend/src/models/radiusAuthentication.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/radiusAuthentication.model.js) — Penambahan field opsional `owner_admin` (ref ke model `Admin`) untuk menandai kepemilikan akun oleh staf/karyawan internal.
    - [`backend/src/models/radiusProfile.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/radiusProfile.js) — Penyesuaian skema profil RADIUS agar mendukung penautan profil kecepatan non-pelanggan.
    - [`backend/src/services/radiusAuthentication.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusAuthentication.service.js):
      - `ensureNonCustomerProduct`: Fungsi utilitas penjamin keberadaan dokumen Product placeholder non-billing (`_id` statis/idempotent) agar akun internal memenuhi batasan integritas skema `bind_product` tanpa membuat tagihan berkala.
      - `createNewNonCustomerAuthentication`: Logika pembuatan akun RADIUS khusus non-pelanggan dengan validasi ketat bahwa salah satu dari `owner_admin` atau `notes` wajib diisi.
      - `findListNonCustomerAuthenticationForTable`: Query datatable dengan filter ketat `{ customer: { $exists: false }, partner: { $exists: false }, ref_partner: { $exists: false } }`.
    - [`backend/src/services/radiusIdentity.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusIdentity.service.js) & [`backend/src/services/radiusProfile.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusProfile.service.js) — Penanganan identitas aman saat `customer` bernilai `null` / tidak ada dan helper pencarian profil.
    - [`backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthenticationNonCustomer.controller.js) [NEW] — Controller penanganan CRUD akun non-pelanggan:
      - `listNonCustomerAuthentication`: Handler daftar akun dengan filter dan pagination datatable.
      - `createNonCustomerAuthentication`: Pembuatan akun baru dengan proteksi sanitasi whitelist field dan penanda `created_by`.
      - `editNonCustomerAuthentication`: Pengambilan data tunggal akun untuk form edit.
      - `updateNonCustomerAuthentication`: Pembaruan konfigurasi kredensial, profil, alamat IP statis, dan koordinat akun.
      - `deleteNonCustomerAuthentication`: Penghapusan akun dengan verifikasi dependensi dan audit log histori.
      - `changeStatusNonCustomerAuthentication`: Pengaktifan/penonaktifan akun dengan pemicuan Disconnect/CoA ke daemon RADIUS bila sesi sedang aktif.
    - [`backend/src/routes/radiusAuthenticationNonCustomer.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/radiusAuthenticationNonCustomer.route.js) [NEW] — Definisi rute RESTful API `/api/v1/radius-non-customer/*` dan dokumentasi OpenAPI/Swagger komprehensif.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) — Registrasi privilege RBAC granular baru `radiusNonCustomer` (`list`, `create`, `read`, `update`, `delete`, `changeStatus`).
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) — Translasi i18n pesan sukses dan error penanganan akun non-pelanggan.
    - [`backend/test/integration/radiusAuthenticationNonCustomer.model.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthenticationNonCustomer.model.test.js) [NEW], [`radiusAuthenticationNonCustomer.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthenticationNonCustomer.service.test.js) [NEW], [`radiusIdentity.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusIdentity.service.test.js), & [`radiusProfile.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusProfile.service.test.js) — Rangkaian unit & integration test untuk validasi model, service, identitas non-pelanggan, dan resolusi profil.
  - **Frontend — Router, Navigasi, Form & Schema**:
    - [`frontend/src/app/router/network/radiusNonCustomerRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/network/radiusNonCustomerRoute.jsx) [NEW] — Pendaftaran rute `/networks/radius-non-customer`, `/networks/radius-non-customer/create`, dan `/networks/radius-non-customer/edit/:id` yang dilindungi privilege RBAC.
    - [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx) — Penggabungan rute non-pelanggan ke pohon rute terproteksi.
    - [`frontend/src/app/navigation/networks.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/networks.js) — Entri menu sidebar `radiusNonCustomer` di bawah grup Jaringan -> Radius Autentikasi dengan proteksi `useHasPrivilege('radiusNonCustomer.list')`.
    - [`frontend/src/app/pages/network/radiusNonCustomer/schema/createSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/schema/createSchema.js) [NEW] — Skema validasi Yup untuk username, password, koordinat, profil kecepatan, status, dan validasi kondisional antara `owner_admin` atau `notes`.
    - [`frontend/src/constants/privilegeDescriptions.id.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/constants/privilegeDescriptions.id.json) & [`privilegeDescriptions.en.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/constants/privilegeDescriptions.en.json) — Deskripsi peran hak akses untuk manajemen otorisasi administrator.

---

## 🌿 Branch: `issue-305` — Persistensi Data Antrean/Bot & Mekanisme Otomatis Re-inisialisasi Bot Telegram saat Service Restart / Unique ID Basi

### 📌 Informasi Issue

- **Nomor Issue**: #305
- **Judul Issue**: Telegram API & Bot Integration: Fix Persistensi Volume Docker Antrean/Bot dan Mekanisme Otomatis Re-inisialisasi Bot Telegram saat Service Restart / Unique ID Basi
- **Status Branch**: `Belum di-merge` (Branch aktif di remote `origin/issue-305`)

---

### 📅 Rincian Commit

#### [`4fd8a2b`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #305 - 15 September 2026, 18:15:39 WIB

- **Komponen yang Berubah**:
  - **Microservice Telegram API (`/telegram-api`)**:
    - [`telegram-api/src/botManager.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-api/src/botManager.js) & [`telegram-api/src/messageQueue.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-api/src/messageQueue.js) — Restrukturisasi direktori penyimpanan data lokal:
      - Memindahkan target berkas `bots.json`, `messageQueue.json`, dan `failedQueue.json` ke subfolder `./src/data/` (`DATA_DIR`).
      - Penambahan logika `fs.mkdirSync(DATA_DIR, { recursive: true })` otomatis saat inisialisasi modul.
      - **Solusi Masalah**: Docker dan Coolify persistent volume hanya mendukung mounting direktori (bukan file tunggal secara langsung). Perubahan ini memungkinkan seluruh berkas antrean dan sesi bot tersimpan awet pada satu mount volume `./src/data:/usr/src/app/src/data` tanpa risiko file ter-overwrite menjadi direktori kosong saat deployment ulang.
    - [`telegram-api/docker-compose.yml`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-api/docker-compose.yml) — Penyesuaian definisi volume service menjadi satu direktori `./src/data:/usr/src/app/src/data`.
    - [`telegram-api/.gitignore`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-api/.gitignore) — Mengabaikan direktori runtime `src/data/` dari commit Git.
  - **Backend Core — Utilitas Bot Telegram**:
    - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) — Mekanisme *self-healing* otomatis saat registrasi bot terputus:
      - Penambahan regex deteksi kegagalan `BOT_NOT_REGISTERED_PATTERN = /bot.*tidak ditemukan/i`.
      - Implementasi fungsi `ensureBotReinitialized`:
        - Mencegah *race condition* dengan mengunci pemanggilan ulang menggunakan single in-flight promise (`reinitPromise`).
        - Menerapkan *cooldown debounce* (`REINIT_COOLDOWN_MS = 5000`) agar backend tidak membanjiri `telegram-api` dengan request re-init saat service memang sedang down.
      - Deteksi otomatis pada `attemptSendTelegramMessage`: Jika `telegram-api` mengembalikan error "bot tidak ditemukan" atau `uniqueId` hilang/belum siap (akibat `telegram-api` direstart terpisah), backend secara mandiri mengeksekusi registrasi ulang bot dan mencoba kembali pengiriman pesan tanpa perlu me-restart server Backend secara manual.

---

## 🌿 Branch: `master` & `production` — Perbaikan dan Refaktor Pipeline CI/CD Deploy Production Multi-Server Coolify

### 📌 Informasi Issue / Task

- **Task**: Perbaikan Pipeline GitHub Actions CI/CD Deploy Otomatis Multi-Server Coolify (`deploy-production.yml`)
- **Status Branch**: `Sudah di-merge` (Commit telah aktif di branch `master` dan `production`)

---

### 📅 Rincian Commit

#### [`1546343`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 20:15:05 WIB
#### [`6d9a020`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 19:57:36 WIB
#### [`3841388`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 19:26:37 WIB
#### [`630e8a1`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 19:21:36 WIB
#### [`6ffa265`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 19:11:32 WIB
#### [`8cd8111`](file:///home/dhedhy/Project/Dekasimal-V2) - fix ci/cd production - 15 September 2026, 19:00:58 WIB

- **Komponen yang Berubah**:
  - [`.github/workflows/deploy-production.yml`](file:///home/dhedhy/Project/Dekasimal-V2/.github/workflows/deploy-production.yml) — Rekonfigurasi total workflow deployment produksi ke server Coolify:
    - **Deteksi Perubahan Modul Presisi (`detect-changes`)**:
      - Menggunakan `git diff` antara `github.event.before` (atau `HEAD~1`) terhadap `HEAD` dengan parameter `git diff --name-only` untuk mendeteksi modul mana yang benar-benar mengalami perubahan kode.
      - Mendukung pemantauan terpisah untuk 10 modul monorepo: `frontend`, `telegram-apps`, `backend`, `cron-worker`, `telegram-api`, `whatsapp-api`, `radius-server`, `network-monitor`, `reverse-proxy`, dan `baileys-api`.
    - **Deployment Cerdas ke Coolify (`deploy-cool`)**:
      - Memetakan UUID aplikasi Coolify resmi produksi (`cool.deka.net.id`) untuk masing-masing modul (mis. `zewqgrdaom42t9uepwvzzv3r` & `icxlhflwblsg9zqolsjwyhp7` untuk backend, `claeknzyutnmhou8pgepjxjl` untuk radius-server, dll.).
      - Mengumpulkan UUID aplikasi yang berubah ke dalam satu query string `uuid=${UUIDS}` dan menembak webhook API Coolify (`https://cool.deka.net.id/api/v1/deploy?uuid=${UUIDS}&force=false`) menggunakan secret `COOLIFY_API_PROD`.
      - Melewati deployment secara bersih (*skip*) jika tidak ada berkas terkait yang berubah pada modul tersebut.
    - **Penyiapan Skalabilitas Multi-Server (`deploy-server-2` & `deploy-server-3`)**:
      - Menyediakan struktur job template deployment untuk server produksi sekunder/tersier dengan pengondisian token rahasia (`COOLIFY_TOKEN`) yang elegan (otomatis skip jika secret belum dikonfigurasi).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue / Scope | Judul | Dampak Utama |
| ------------- | ----- | ------------ |
| #297 | Radius Authentication Non-Pelanggan | Mengizinkan pembuatan akun PPPoE untuk staf internal (karyawan) maupun teknisi kontraktor luar tanpa membuat data pelanggan palsu di CRM/billing, lengkap dengan manajemen hak akses RBAC, counter status, dan drawer profil kecepatan. |
| #305 | Fix Persistensi & Auto Re-init Bot Telegram | Menjamin antrean pesan dan data sesi bot Telegram tersimpan permanen di Docker volume, serta mengeliminasi kegagalan pengiriman notifikasi saat service `telegram-api` di-restart terpisah dari backend. |
| CI/CD Prod | Pipeline Deployment Otomatis Multi-Server | Proses deployment produksi di Coolify kini sepenuhnya otomatis, mendeteksi modul monorepo yang berubah secara cerdas, dan siap untuk ekspansi deployment multi-server. |

### Kemampuan Baru Pengguna/Admin

- **Manajemen Akun PPPoE Non-Pelanggan**: Administrator jaringan kini dapat membuat akun PPPoE khusus internal untuk karyawan (menautkan ke profil staf) atau teknisi vendor/pihak luar (dengan mengisi catatan bebas pada kolom keterangan), tanpa mengotori database pelanggan (`customers`) dan tanpa membuat tagihan/invoice periodik.
- **Monitoring Status Akun Cepat**: Administrator dapat langsung memantau jumlah total akun internal, akun aktif, akun nonaktif, serta akun yang sedang terkoneksi *online* secara *real-time* melalui kartu ringkasan status di atas tabel.
- **Inspeksi Profil Tanpa Pindah Halaman**: Klik nama profil kecepatan pada tabel akun non-pelanggan langsung membuka drawer detail profil broadband (`BroadbandProfileDetailDrawer`) untuk melihat rincian batas bandwidth dan burst limit.

### Bug Fix / Solusi Masalah

- **Penanganan Restart Service Telegram API**: Memperbaiki masalah hilangnya pendaftaran bot dan error "bot tidak ditemukan" saat container `telegram-api` di-restart/redeploy; Backend kini memiliki mekanisme *self-healing* otomatis yang mendaftarkan ulang bot tanpa perlu restart manual.
- **Persistensi Mount Docker Volume Coolify**: Memindahkan berkas antrean dan data bot Telegram ke subdirektori `./src/data/` menyelesaikan kegagalan mount volume Docker pada sistem Coolify.
- **Isolasi Data Akun Non-Pelanggan**: Query tabel dan penghitungan status diisolasi penuh (`customer: { $exists: false }`), mencegah tumpang tindih dengan data pelanggan reguler maupun mitra.

### Menu/Fitur Baru

- **Menu Navigasi Baru**: Menu **Radius Non-Pelanggan** di sidebar pada grup menu `Jaringan` -> `Radius Autentikasi` (`/networks/radius-non-customer`).
- **Form Akun Non-Pelanggan Terpadu**: Formulir pembuatan (`/create`) dan pengubahan (`/edit/:id`) modern dengan 2 kolom responsif, kalkulator Mikrotik Burst Limit, integrasi pencarian karyawan pemilik, dan pencegahan submit ganda.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  Modul **Radius Non-Pelanggan** memungkinkan perusahaan ISP memberikan akses internet PPPoE kepada karyawan internal (mis. untuk operasional kantor cabang, rumah staf, NOC) atau pihak ketiga (kontraktor fiber, vendor pemeliharaan) menggunakan server RADIUS bawaan (`radiusd`), tanpa harus mendaftarkan mereka sebagai pelanggan berbayar di modul CRM/Keuangan. Akun ini tetap memiliki kontrol kecepatan bandwidth, opsi IP statis, burst rate Mikrotik, dan pelaporan akuntansi konektivitas secara penuh.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi web Dekasimal V2 dengan akun admin yang memiliki izin `radiusNonCustomer.list` dan `radiusNonCustomer.create`.
  2. Buka menu sidebar **Jaringan** -> **Radius Autentikasi** -> pilih **Radius Non-Pelanggan** (`/networks/radius-non-customer`).
  3. Pada halaman utama, tinjau ringkasan metrik akun (*Terkoneksi*, *Total*, *Aktif*, *Tidak Aktif*).
  4. Untuk membuat akun baru, klik tombol **Buat Akun Non-Pelanggan** di pojok kanan atas.
  5. Isi data kredensial:
     - Masukkan **Username** dan **Password** PPPoE.
     - Pilih **Karyawan Pemilik** jika akun diperuntukkan bagi staf internal perusahaan, ATAU kosongkan dan isi **Keterangan Pemilik** jika akun diperuntukkan bagi teknisi vendor/kontraktor luar (mis. "PT Mitra Fiber - Teknisi Rollout").
     - Tentukan **Profil Kecepatan** broadband.
     - *(Opsional)* Tentukan IP Statis, koordinat lokasi, dan aktifkan **Mikrotik Burst** jika memerlukan lonjakan bandwidth temporer.
  6. Klik **Simpan**. Akun akan langsung terdaftar di database RADIUS dan siap digunakan untuk dial-up koneksi PPPoE pada router/CPE terkait.
