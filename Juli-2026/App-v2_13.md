# 📝 Daily Work Report - Dedy Putra (2026-07-13)

---

## 📅 Laporan Harian - 13 Juli 2026

---

## 🌿 Branch: `issue-137` — Dokumen PIR (Post Incident Report)

### 📌 Informasi Issue
- **Nomor Issue**: #137
- **Judul Issue**: Audit & Refactoring Modul Tiket ()
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [63c6953] - resolve #137 - Sat Jul 11 19:50:51 2026

- **Komponen yang Berubah**:
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
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomEdge.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx`
  - `frontend/src/app/pages/network/fiberCable/index.jsx`
  - `frontend/src/app/pages/services/salesOrder/create.jsx`
  - `frontend/src/app/pages/services/salesOrder/edit.jsx`
  - `frontend/src/app/pages/services/activation/components/SOReviewDrawer.jsx`
  - `frontend/src/app/pages/vendorManagement/schema/ticketColumns.jsx`
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx`
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx`
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`
  - `frontend/src/app/pages/tickets/backbone/BackboneReport.jsx`
  - `frontend/src/app/pages/tickets/backbone/close.jsx`
  - `frontend/src/app/pages/tickets/backbone/detail.jsx`
  - `frontend/src/app/pages/tickets/backbone/schema/closeSchema.js` [NEW]
  - `frontend/src/app/pages/tickets/backbone/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/components/PostIncidentReportModal.jsx` [NEW]
  - `frontend/src/app/pages/tickets/customer/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/dismantle/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/installation/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/other/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx`
  - `frontend/src/app/pages/tickets/partner/close.jsx`
  - `frontend/src/app/pages/tickets/partner/create.jsx`
  - `frontend/src/app/pages/tickets/partner/detail.jsx`
  - `frontend/src/app/pages/tickets/partner/edit.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/closeSchema.js` [NEW]
  - `frontend/src/app/pages/tickets/partner/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/createSchema.js`
  - `frontend/src/app/pages/tickets/payment/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/survey/schema/columns.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/constants/fiberColors.constant.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/styles/index.css`
  - `frontend/src/utils/formatUptime.js` [NEW]
  - `frontend/src/utils/ticketUtils.js` [NEW]
  - `audit-report-issue-137.md` [NEW]
  - `audit-task-issue-137.md` [NEW]
  - `CREATE_REPORT.md`

- **Deskripsi Perubahan & Fungsi**:

  **Backend:**
  - **Refactoring Ticket Controller**: Optimasi besar-besaran pada `ticket.controller.js` (524 baris berubah). Menyelesaikan N+1 query issues dengan implementasi batch fetching untuk installation tickets dan batch counting untuk warehouse requests. Refactoring fungsi `listTicketForTable` untuk performa query database yang lebih optimal.
  - **Ticket Service Optimization**: Refactoring `getTicketStatusByType` dari 5 query terpisah menjadi satu aggregation pipeline MongoDB untuk mengurangi database round-trips.
  - **Service Layer Enhancement**: Penambahan fungsi batch pada `admin.service.js`, `customer.service.js`, `partner.service.js`, `productDataAccess.service.js`, `radiusAuthentication.service.js`, `warehouseItem.service.js`, dan `warehouseRequest.service.js` untuk mendukung batch data fetching.
  - **Ticket Model Update**: Pembaruan model tiket (`ticket.model.js`) untuk mendukung field-field baru terkait close schema dan incident report.
  - **Route Cleanup**: Pembersihan 47 baris route yang tidak terpakai di `ticket.route.js`.
  - **Generate Ticket Report Utility**: Pembuatan utility baru `generateTicketReport.js` untuk menghasilkan laporan tiket otomatis (Post Incident Report).
  - **i18n Backend**: Penambahan key terjemahan baru untuk tiket backbone & partner (EN dan ID).

  **Frontend:**
  - **Post Incident Report Modal**: Pembuatan komponen baru `PostIncidentReportModal.jsx` (651 baris) yang menyediakan modal untuk menampilkan dan mencetak laporan insiden pasca-penyelesaian tiket. Mencakup informasi perusahaan, detail tiket, kronologi kejadian, dan resolusi.
  - **Backbone Report Overhaul**: Refactoring besar pada `BackboneReport.jsx` (242+ baris perubahan) untuk menampilkan data laporan tiket yang lebih lengkap termasuk MTTR (Mean Time To Resolution).
  - **Partner Report Overhaul**: Refactoring pada `PartnerReport.jsx` (372+ baris perubahan) dengan pola yang sama seperti Backbone Report.
  - **Close Schema**: Penambahan skema validasi baru `closeSchema.js` untuk tiket backbone dan partner, memastikan form penutupan tiket tervalidasi dengan baik.
  - **Kolom MTTR**: Penambahan kolom MTTR pada tabel tiket backbone dan partner melalui perubahan di `columns.jsx`.
  - **Ticket Detail Drawer**: Simplifikasi `TicketDetailDrawer.jsx` dengan memindahkan 205 baris ke komponen-komponen yang lebih kecil.
  - **Message Update Enhancement**: Peningkatan komponen `MessageUpdate.jsx` (136+ baris perubahan) untuk menampilkan pesan update tiket yang lebih informatif.
  - **Shared Utilities**: Pembuatan `ticketUtils.js` sebagai shared utility untuk kalkulasi durasi pending tiket, menggantikan implementasi duplikat di 3 komponen. Pembuatan `formatUptime.js` untuk format durasi uptime.
  - **Table Rows Enhancement**: Penambahan komponen helper baru di `rows.jsx` untuk standardisasi cell rendering (StatusBadgeCell, DateCell, MTTRCell).
  - **i18n Frontend**: Pembaruan masif pada file terjemahan EN dan ID (438/447 baris perubahan) untuk mendukung semua string baru pada modul tiket.
  - **Fiber Cable Minor Fix**: Perbaikan minor pada komponen fiber cable (`CoreTopologyCustomEdge.jsx`, `DropCoreModal.jsx`, `index.jsx`).
  - **Sales Order Fix**: Perbaikan pada komponen sales order (`create.jsx`, `edit.jsx`, `SOReviewDrawer.jsx`).

---

## 🌿 Branch: `issue-115` — Halaman Settings (System & Application)

### 📌 Informasi Issue
- **Nomor Issue**: #115
- **Judul Issue**: Implementasi Halaman Settings System & Application
- **Status Branch**: `Dalam pengerjaan (Work In Progress — belum di-commit)`

### 📅 Rincian Perubahan (Belum Di-commit)

#### [Unstaged Changes] - Work In Progress - 13 Juli 2026

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/settings.controller.js` [NEW]
  - `backend/src/routes/settings.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/Application.jsx` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:

  **Backend:**
  - **Settings Controller (Baru)**: Pembuatan controller baru `settings.controller.js` dengan dua endpoint utama:
    - `readSettings`: Membaca pengaturan berdasarkan nama (`system_settings` atau `application_settings`) dari collection Option di MongoDB.
    - `updateSettings`: Memperbarui pengaturan dengan dukungan upload file (company logo dan Google Drive key) melalui `multipart/form-data`. Menggunakan pola merge untuk memastikan data lama tidak hilang saat update parsial.
  - **Settings Route (Baru)**: Pembuatan route baru `settings.route.js` dengan dua endpoint:
    - `POST /settings/read` — Mendapatkan data pengaturan (dilindungi privilege `settings.read`).
    - `POST /settings/update` — Memperbarui pengaturan (dilindungi privilege `settings.update`).
    - Dokumentasi Swagger JSDoc lengkap untuk kedua endpoint.
  - **Option Service Enhancement**: Penambahan dua fungsi baru di `option.service.js`:
    - `updateSystemSettings`: Menyimpan/memperbarui pengaturan sistem ke MongoDB dengan upsert.
    - `updateAppSettings`: Menyimpan/memperbarui pengaturan aplikasi ke MongoDB dengan upsert.
  - **Privilege Configuration**: Penambahan privilege baru `settings.read` dan `settings.update` pada `privilege.json` serta restrukturisasi posisi vendor privileges.
  - **App Registration**: Registrasi route Settings baru di `app.js`.
  - **i18n Backend**: Penambahan key terjemahan untuk option/settings (`option.name`, `option.notFound`, `option.editFailed`), perbaikan duplikat key pada translation, dan konsolidasi key `fiber.drawer` ke dalam namespace yang benar.

  **Frontend:**
  - **System Settings Page (Baru)**: Pembuatan halaman `System.jsx` (334 baris) dengan tab-based interface menggunakan Headless UI TabGroup:
    - **Tab Radius**: Konfigurasi authentication port, accounting port, drop/block profile MikroTik, max invoice, limit retry, wait accounting, auto-generate auth credentials.
    - **Tab Hotspot**: Konfigurasi hotspot authentication & accounting port.
    - **Tab Email (SMTP)**: Konfigurasi SMTP host, port, user, dan password.
    - **Tab Other**: Konfigurasi syslog port dan upload Google Drive service key (file JSON).
  - **Application Settings Page (Baru)**: Pembuatan halaman `Application.jsx` (495 baris) dengan tab-based interface:
    - **Tab General**: Konfigurasi nama aplikasi, informasi perusahaan (nama, alamat, email, telepon, WhatsApp, website), dan upload logo perusahaan dengan preview avatar.
    - **Tab Maps**: Konfigurasi timezone dan Mapbox token.
    - **Tab Invoice**: Konfigurasi catatan invoice, pengaturan auto-invoice (due day, pajak, format nama), pengingat WhatsApp.
    - **Tab iPaymu**: Konfigurasi payment gateway (API key, VA, sandbox mode, callback URL, payment link).
    - **Tab Telegram**: Konfigurasi bot token dan chat group IDs untuk berbagai kanal notifikasi (tiket, laporan, debug, approval, gudang, presensi).
  - **Navigation Update**: Penambahan dua menu baru pada navigasi settings:
    - "System" (`/settings/system`) dengan ikon `ComputerDesktopIcon`.
    - "Application" (`/settings/application`) dengan ikon `WindowIcon`.
    - Kedua menu dilindungi role `settings.read`.
  - **Router Update**: Penambahan lazy-loaded route baru untuk `/settings/system` dan `/settings/application` pada `protected.jsx`.
  - **i18n Frontend**: Penambahan dan perbaikan key terjemahan di `translations.json` (EN & ID) dengan perubahan 365/363 baris, termasuk konsolidasi duplikat key.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
|-------|-------|--------------|
| #137  | Audit & Refactoring Modul Tiket | Optimasi performa query database, Post Incident Report, kolom MTTR, refactoring laporan tiket backbone & partner, standardisasi shared utilities |
| #115  | Halaman Settings System & Application | Halaman konfigurasi sistem (Radius, Hotspot, SMTP, Syslog) dan aplikasi (Company Info, Maps, Invoice, Payment, Telegram) dengan full CRUD dan privilege-based access |

### Kemampuan Baru Pengguna/Admin
- Admin dapat melihat **MTTR (Mean Time To Resolution)** pada tabel daftar tiket backbone dan partner untuk analisis performa penyelesaian gangguan.
- Admin dapat membuat dan mencetak **Post Incident Report** langsung dari halaman detail tiket, termasuk kronologi kejadian dan resolusi.
- Admin dengan privilege `settings.read` dapat mengakses halaman **System Settings** untuk melihat konfigurasi Radius, Hotspot, Email SMTP, dan Syslog.
- Admin dengan privilege `settings.update` dapat mengubah konfigurasi **Application Settings** termasuk informasi perusahaan, logo, timezone, konfigurasi invoice, payment gateway iPaymu, dan bot Telegram.
- Admin dapat meng-upload logo perusahaan dan Google Drive service key langsung dari halaman settings.

### Bug Fix / Solusi Masalah
- Penyelesaian **N+1 query issues** pada ticket controller yang menyebabkan response time lambat saat memuat daftar tiket dengan banyak data.
- Optimasi query `getTicketStatusByType` dari 5 query menjadi 1 aggregation pipeline, mengurangi beban database secara signifikan.
- Perbaikan **duplikat key** pada file terjemahan backend (EN & ID), termasuk konsolidasi key `error.node_not_found` dan `fiber.drawer.equipment`.
- Eliminasi **code duplication** pada fungsi `getPendingDuration` yang sebelumnya terduplikasi di 3 komponen frontend.
- Perbaikan **error handling** pada komponen tiket frontend (`BackboneReport.jsx`, `PartnerReport.jsx`, `PostIncidentReportModal.jsx`) dengan standardisasi `console.error` dan user feedback.
- Simplifikasi `TicketDetailDrawer.jsx` dengan memecah komponen monolitik menjadi bagian-bagian yang lebih kecil dan maintainable.

### Menu/Fitur Baru
- **Menu Settings > System**: Halaman konfigurasi sistem dengan tab Radius, Hotspot, Email, dan Other.
- **Menu Settings > Application**: Halaman konfigurasi aplikasi dengan tab General, Maps, Invoice, iPaymu, dan Telegram.
- **Post Incident Report Modal**: Modal untuk menampilkan dan mencetak laporan insiden tiket.
- **Kolom MTTR**: Kolom baru pada tabel tiket backbone dan partner yang menampilkan durasi penyelesaian tiket.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur — Post Incident Report**:
  Post Incident Report adalah dokumen formal yang dihasilkan saat tiket (backbone/partner) ditutup. Laporan ini mencakup informasi perusahaan, detail tiket, kronologi kejadian (waktu lapor, waktu mulai perbaikan, waktu selesai), deskripsi masalah, dan solusi yang dilakukan. Laporan dapat dicetak langsung dari browser sebagai PDF.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka halaman detail tiket backbone atau partner yang sudah ditutup (closed).
  2. Klik tombol **"Post Incident Report"** pada halaman detail.
  3. Modal akan menampilkan laporan lengkap termasuk header perusahaan, informasi tiket, dan rincian kejadian.
  4. Klik tombol **"Cetak"** untuk mencetak atau menyimpan sebagai PDF.

- **Penjelasan Fitur — Halaman Settings**:
  Halaman Settings menyediakan antarmuka terpusat untuk admin mengkonfigurasi seluruh parameter sistem dan aplikasi. Pengaturan disimpan dalam collection `options` di MongoDB dengan pattern upsert, sehingga konfigurasi baru otomatis dibuat jika belum ada.

- **Langkah Penggunaan (Tutorial)**:
  1. Navigasi ke menu **Settings > System** atau **Settings > Application** di sidebar.
  2. Pilih tab yang sesuai dengan konfigurasi yang ingin diubah (misal: Radius, Invoice, Telegram).
  3. Ubah nilai konfigurasi sesuai kebutuhan.
  4. Klik tombol **"Simpan"** untuk menyimpan perubahan.
  5. Untuk upload logo perusahaan, klik ikon pensil pada avatar dan pilih file gambar.
