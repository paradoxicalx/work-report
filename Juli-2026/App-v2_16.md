# 📝 Daily Work Report - Dedy S.N Putra (2026-07-16)

---

## 📅 Laporan Harian - 16 Juli 2026

---

## 🌿 Branch: `master` (via `issue-140`) — Peningkatan Registrasi & Perbaikan Umum

### 📌 Informasi Issue

- **Nomor Issue**: #140
- **Judul Issue**: Peningkatan Controller Registrasi, Opsi Layanan Statis, dan Perbaikan Kode Umum
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`6edcc10`](backend/src/controllers/registration.controller.js) - resolve #140 - Kamis, 16 Juli 2026 19:52

- **Komponen yang Berubah**:
  - `.gitignore`
  - `backend/src/controllers/productDataAccess.controller.js`
  - `backend/src/controllers/registration.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/partner.service.js`
  - `backend/src/services/productDataAccess.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/warehouseItem.service.js`
  - `backend/src/services/warehouseRequest.service.js`
  - `backend/src/utils/generateTicketReport.js`
  - `frontend/src/app/contexts/auth/Provider.jsx`
  - `frontend/src/app/pages/profile/index.jsx`
  - `frontend/src/app/pages/public/PublicBAPDocument.jsx` _(rename dari `publicBAPDocument.jsx`)_
  - `frontend/src/app/pages/public/registration.jsx`
  - `frontend/src/app/pages/services/dataAccess/createBAA.jsx`
  - `frontend/src/app/pages/settings/sections/General.jsx`
  - `frontend/src/app/pages/users/business/editBatch.jsx` _(rename dari `editbatch.jsx`)_
  - `frontend/src/app/pages/users/partner/editBatch.jsx` _(rename dari `editbatch.jsx`)_
  - `frontend/src/app/pages/users/registration/detail.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/template/Notifications.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/axios.js`
  - `telegram-api/package-lock.json`
  - `telegram-api/package.json`
- **Jumlah Perubahan**: 28 file, +243 baris, -95 baris
- **Deskripsi Perubahan & Fungsi**:
  - **Controller Registrasi** (`registration.controller.js`): Implementasi dukungan opsi layanan statis pada formulir registrasi publik. Penambahan konstanta `STATIC_SERVICE_OPTIONS` untuk layanan yang tidak bergantung pada produk database (contoh: Dedicated Internet). Pembaruan logika `readRegistration`, `createRegistration`, dan `getMyRequests` untuk menangani package statis maupun produk database secara dinamis. Penambahan validasi `isObjectId` sebelum query database.
  - **Controller ProductDataAccess** (`productDataAccess.controller.js`): Penambahan field `created_at` dari data `baaDate` saat membuat BAA (Berita Acara Aktivasi), sehingga tanggal pembuatan dokumen dapat dikustomisasi oleh user.
  - **Controller Settings** (`settings.controller.js`): Perbaikan formatting dan penyesuaian minor pada endpoint settings.
  - **Auth Provider** (`Provider.jsx`): Peningkatan alur autentikasi dengan penanganan token refresh yang lebih robust, logging error yang lebih baik, dan optimasi flow redirect.
  - **FormInput** (`FormInput.jsx`): Penambahan properti dan perbaikan dukungan untuk berbagai jenis input form.
  - **Axios Utility** (`axios.js`): Peningkatan konfigurasi HTTP client — penambahan interceptor untuk token refresh otomatis, penanganan error 401/403 yang lebih baik, dan konfigurasi retry.
  - **Notifications** (`Notifications.jsx`): Perbaikan rendering komponen notifikasi.
  - **Service Code Formatting**: Pemformatan ulang (reformat) pada beberapa service untuk konsistensi style — `admin.service.js`, `customer.service.js`, `partner.service.js`, `productDataAccess.service.js`, `radiusAuthentication.service.js`, `warehouseItem.service.js`, `warehouseRequest.service.js`. Perubahan hanya pada formatting (whitespace, line breaks), tidak mengubah logika bisnis.
  - **File Renaming**: Rename file untuk konsistensi naming convention PascalCase:
    - `publicBAPDocument.jsx` → `PublicBAPDocument.jsx`
    - `editbatch.jsx` → `editBatch.jsx` (business & partner)
  - **Telegram API** (`package.json`, `package-lock.json`): Pembaruan dependency versi untuk keamanan dan kompatibilitas.

---

## 🌿 Branch: `master` (via `issue-137`) — Peningkatan Sistem Tiket & Laporan

### 📌 Informasi Issue

- **Nomor Issue**: #137
- **Judul Issue**: Peningkatan Sistem Tiket, Laporan Backbone/Partner, dan Komponen Pelaporan Insiden
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`2302e78`](backend/src/controllers/ticket.controller.js) - resolve #137 - Kamis, 16 Juli 2026 17:20

- **Komponen yang Berubah**:
  - `CREATE_REPORT.md`
  - `audit-report-issue-137.md` [NEW]
  - `audit-task-issue-137.md` [NEW]
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/ticket.model.js`
  - `backend/src/routes/ticket.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/partner.service.js`
  - `backend/src/services/productDataAccess.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/services/warehouseItem.service.js`
  - `backend/src/services/warehouseRequest.service.js`
  - `backend/src/utils/generateTicketReport.js` [NEW]
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx`
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx`
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`
  - `frontend/src/app/pages/tickets/backbone/BackboneReport.jsx`
  - `frontend/src/app/pages/tickets/backbone/close.jsx`
  - `frontend/src/app/pages/tickets/backbone/detail.jsx`
  - `frontend/src/app/pages/tickets/backbone/schema/closeSchema.js` [NEW]
  - `frontend/src/app/pages/tickets/backbone/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/components/PostIncidentReportModal.jsx` [NEW]
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx`
  - `frontend/src/app/pages/tickets/partner/close.jsx`
  - `frontend/src/app/pages/tickets/partner/create.jsx`
  - `frontend/src/app/pages/tickets/partner/detail.jsx`
  - `frontend/src/app/pages/tickets/partner/edit.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/closeSchema.js` [NEW]
  - `frontend/src/app/pages/tickets/partner/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/createSchema.js`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/constants/fiberColors.constant.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/styles/index.css`
  - `frontend/src/utils/formatUptime.js` [NEW]
  - `frontend/src/utils/ticketUtils.js` [NEW]
- **Jumlah Perubahan**: 55 file, +4061 baris, -1237 baris
- **Deskripsi Perubahan & Fungsi**:
  - **Controller Tiket** (`ticket.controller.js`): Refaktorisasi besar-besaran pada alur pembacaan data tiket, penambahan validasi dependensi data (stok gudang, pelanggan, partner) sebelum aksi administratif seperti pelepasan layanan. Implementasi aturan cascading pembatalan tagihan → penghapusan layanan → penghapusan identitas.
  - **Service Tiket** (`ticket.service.js`): Peningkatan query populate untuk menyertakan data yang diperlukan UI (image, admin_id), optimasi performa query.
  - **Laporan Tiket** (`generateTicketReport.js`): Utilitas baru untuk menghasilkan laporan tiket backbone secara terstruktur, termasuk perhitungan durasi pending dan statistik resolusi.
  - **Laporan Backbone** (`BackboneReport.jsx`, `close.jsx`): Peningkatan tampilan laporan backbone dengan integrasi data perangkat, durasi uptime, dan detail penutupan tiket yang lebih komprehensif.
  - **Laporan Partner** (`PartnerReport.jsx`, `close.jsx`, `create.jsx`, `edit.jsx`): Peningkatan seluruh alur partner ticket — pembuatan, pengeditan, pelaporan, dan penutupan tiket dengan schema validasi baru.
  - **Komponen Pelaporan Insiden** (`PostIncidentReportModal.jsx`): Komponen modal baru untuk membuat laporan pasca-insiden pada tiket backbone, mendukung pengisian data kerusakan, dampak, dan tindakan perbaikan.
  - **Utilitas Tiket** (`formatUptime.js`, `ticketUtils.js`): Fungsi bantu baru untuk format durasi uptime dan manipulasi data tiket (ekstraksi metadata, formatting angka, dll.).
  - **Update Kolom Tabel**: Pembaruan schema kolom untuk berbagai jenis tiket (backbone, partner, customer, dismantle, installation, payment, survey, other) agar konsisten dengan data baru.
  - **Template Rows**: Penambahan wrapper cell baru di `rows.jsx` untuk visualisasi badge kustom pada kolom tabel tiket.
  - **Service Pendukung**: Penambahan fungsi `findMultipleWithDeleted` pada beberapa service (`admin`, `customer`, `partner`, `productDataAccess`, `radiusAuthentication`, `warehouseItem`, `warehouseRequest`) untuk mendukung pencarian data termasuk yang sudah di-soft-delete.

---

## 🌿 Branch: `master` (via `issue-115`) — Sistem Pengaturan (Settings) & Profil

### 📌 Informasi Issue

- **Nomor Issue**: #115
- **Judul Issue**: Implementasi Sistem Pengaturan Aplikasi, General, dan System beserta Halaman Profil
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`df3feef`](frontend/src/app/pages/settings/sections/Application.jsx) - Merge branch 'master' into issue-115 - Kamis, 16 Juli 2026 17:22

- **Komponen yang Berubah**:
  - _(Merge commit — menggabungkan perubahan terbaru dari master ke dalam branch issue-115)_
- **Deskripsi Perubahan & Fungsi**:
  - Merge master ke branch issue-115 untuk menyinkronkan perubahan terbaru dari issue #137 sebelum resolve.

#### [`e613954`](frontend/src/app/pages/settings/sections/Application.jsx) - resolve #115 - Kamis, 16 Juli 2026 17:22

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/admin.controller.js` [NEW]
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/settings.controller.js` [NEW]
  - `backend/src/controllers/utils.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/admin.route.js` [NEW]
  - `backend/src/routes/settings.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `frontend/src/App.jsx`
  - `frontend/src/app/contexts/config/Provider.jsx` [NEW]
  - `frontend/src/app/contexts/config/context.js` [NEW]
  - `frontend/src/app/layouts/MainLayout/Profile.jsx`
  - `frontend/src/app/layouts/Sideblock/Profile.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/CollapsibleItem/index.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx`
  - `frontend/src/app/layouts/Sideblock/Sidebar/Menu/index.jsx`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/navigation/services.js`
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/navigation/tickets.js`
  - `frontend/src/app/navigation/users.js`
  - `frontend/src/app/navigation/warehouse.js`
  - `frontend/src/app/pages/Auth/index.jsx`
  - `frontend/src/app/pages/profile/index.jsx`
  - `frontend/src/app/pages/public/termsPrivacy.jsx`
  - `frontend/src/app/pages/settings/schema/applicationSchema.js` [NEW]
  - `frontend/src/app/pages/settings/schema/systemSchema.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Application.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/General.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/Page.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/form/TextEditor.jsx`
  - `frontend/src/components/template/Notifications.jsx`
  - `frontend/src/components/ui/Form/Input.jsx`
  - `frontend/src/configs/server.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Jumlah Perubahan**: 42 file, +3172 baris, -304 baris
- **Deskripsi Perubahan & Fungsi**:
  - **Controller Settings** (`settings.controller.js`): Endpoint baru untuk CRUD pengaturan sistem — General (informasi perusahaan, logo), Application (konfigurasi aplikasi seperti maintenance mode, registration), dan System (konfigurasi teknis seperti server, database). Mendukung operasi `readSettings`, `updateGeneralSettings`, `updateApplicationSettings`, `updateSystemSettings`.
  - **Controller Admin** (`admin.controller.js`): Endpoint baru untuk manajemen admin — `readAdminDashboard` untuk data dashboard, `readAdminProfile` untuk profil admin yang sedang login, dan `updateAdminProfile` untuk pembaruan profil.
  - **Rute Settings & Admin** (`settings.route.js`, `admin.route.js`): Definisi rute REST API baru untuk pengaturan dan admin, dilengkapi middleware otorisasi dan validasi privilege.
  - **Config Provider Context** (`config/Provider.jsx`, `context.js`): Context React baru untuk menyediakan data konfigurasi aplikasi (pengaturan) ke seluruh komponen. Data dimuat saat aplikasi diinisialisasi dan disediakan melalui `ConfigProvider`.
  - **Halaman Profil** (`profile/index.jsx`): Halaman profil pengguna yang ditingkatkan dengan tampilan informasi akun, avatar, dan kemampuan edit profil.
  - **Halaman Settings**: Tiga halaman pengaturan baru — `Application.jsx` (700+ baris, pengaturan aplikasi lengkap), `General.jsx` (diperbarui dengan layout baru), dan `System.jsx` (466 baris, pengaturan teknis sistem).
  - **Schema Validasi Settings**: `applicationSchema.js` dan `systemSchema.js` untuk validasi input form pengaturan menggunakan Yup.
  - **Navigasi Sidebar**: Pembaruan struktur menu navigasi — penambahan menu Pengaturan dengan submenu General, Application, System; pembaruan group menu pada Sidebar.
  - **Privilege Config** (`privilege.json`): Penambahan privilege baru untuk pengaturan: `settings.read`, `settings.general.update`, `settings.application.update`, `settings.system.update`.
  - **Template Notifications** (`Notifications.jsx`): Peningkatan komponen notifikasi dengan layout dan interaksi yang lebih baik.
  - **TextEditor** (`TextEditor.jsx`): Peningkatan komponen editor teks (Quill) dengan konfigurasi toolbar yang lebih lengkap.
  - **FormInput** (`FormInput.jsx`): Penambahan dukungan input mode baru pada komponen form.
  - **Server Config** (`server.config.js`): Pembaruan konfigurasi URL server dan environment.
  - **Router Protected** (`protected.jsx`): Penambahan rute proteksi baru untuk halaman pengaturan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                   | Dampak Utama                                                                                                      |
| ----- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| #137  | Peningkatan Sistem Tiket & Laporan      | Refaktorisasi besar tiket backbone & partner dengan pelaporan insiden, utilitas uptime, dan cascading deletion    |
| #115  | Sistem Pengaturan & Profil              | Tiga halaman settings baru (General, Application, System), admin controller, config provider, profil ditingkatkan |
| #140  | Peningkatan Registrasi & Perbaikan Umum | Dukungan layanan statis pada registrasi, autentikasi lebih robust, normalisasi naming file                        |

### Kemampuan Baru Pengguna/Admin

- **Manajemen Pengaturan Aplikasi**: Admin dapat mengelola pengaturan General (informasi perusahaan), Application (maintenance mode, registrasi), dan System (konfigurasi teknis) melalui halaman settings terpisah
- **Laporan Insiden Pasca-Tiket**: Admin dapat membuat laporan pasca-insiden (Post-Incident Report) pada tiket backbone, mencakup dokumentasi kerusakan, dampak, dan tindakan perbaikan
- **Pelaporan Tiket Backbone & Partner**: Laporan tiket backbone dan partner telah ditingkatkan dengan informasi durasi uptime, detail perangkat, dan statistik resolusi yang lebih lengkap
- **Registrasi Layanan Statis**: Formulir registrasi publik kini mendukung opsi layanan statis (Dedicated Internet) yang tidak bergantung pada data produk di database
- **Profil Admin**: Admin dapat melihat dan memperbarui profil mereka sendiri melalui halaman profil yang baru

### Bug Fix / Solusi Masalah

- **Pembatalan Cascading Tiket**: Memperbaiki alur penutupan tiket pelepasan dengan menerapkan aturan dependensi berjenjang — pembatalan tagihan otomatis mempengaruhi layanan dan pelanggan terkait
- **Validasi ObjectId pada Registrasi**: Memperbaiki potensi error pada registrasi publik dengan menambahkan validasi `isObjectId` sebelum query database untuk package
- **Tanggal BAA yang Dikustomisasi**: Memperbaiki pembuatan BAA agar dapat menggunakan tanggal yang dipilih user melalui field `baaDate`
- **Autentikasi Token Refresh**: Meningkatkan robustness alur refresh token pada Auth Provider untuk mencegah sesi expired secara tiba-tiba

### Menu/Fitur Baru

- **Menu Pengaturan (Settings)**: Tiga submenu baru — General, Application, System
- **Halaman Profil**: Halaman profil pengguna dengan informasi akun dan kemampuan edit
- **Post-Incident Report Modal**: Modal baru pada tiket backbone untuk laporan pasca-insiden

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Sistem Pengaturan (Settings) adalah modul baru yang memungkinkan administrator mengelola konfigurasi aplikasi secara terpusat. Terdiri dari tiga bagian: **General** (informasi perusahaan, kontak, logo), **Application** (mode maintenance, registrasi publik, fitur aktif), dan **System** (konfigurasi server, database, cache). Pengaturan disimpan di backend dan didistribusikan ke seluruh aplikasi melalui Config Provider Context.

- **Langkah Penggunaan (Tutorial)**:
  1. Login ke panel admin dengan akun yang memiliki privilege `settings.read`
  2. Akses menu **Pengaturan** di sidebar navigasi
  3. Pilih submenu **General** untuk mengatur informasi perusahaan (nama, alamat, kontak, logo)
  4. Pilih submenu **Application** untuk mengatur mode aplikasi (maintenance, registrasi publik, fitur yang aktif)
  5. Pilih submenu **System** untuk mengatur konfigurasi teknis (server URL, database, cache, dll.)
  6. Klik **Simpan** pada setiap bagian untuk menyimpan perubahan
  7. Perubahan akan langsung berlaku ke seluruh pengguna aplikasi setelah disimpan
