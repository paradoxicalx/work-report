# 📝 Daily Work Report - Dedy S.N Putra (2026-07-28)

---

## 📅 Laporan Harian - 28 Juli 2026

---

## 🌿 Branch: `issue-162` — WhatsApp Broadcast (Pengiriman Massal WhatsApp)

### 📌 Informasi Issue

- **Nomor Issue**: #162
- **Judul Issue**: WhatsApp Broadcast (Pengiriman Massal WhatsApp)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`43890b4`](43890b4) - save #162 - 28 Juli 2026, 23:23

- **Komponen yang Berubah**:
  - `backend/package.json` — [NEW] Penambahan dependency BullMQ untuk queue management
  - `backend/package-lock.json` — Update lockfile dependency
  - `backend/src/app.js` — Registrasi route baru untuk WhatsApp Broadcast
  - `backend/src/controllers/internal.controller.js` — [NEW] Controller untuk endpoint internal antar-service
  - `backend/src/controllers/settings.controller.js` — Penyesuaian pengambilan data setting broadcast
  - `backend/src/controllers/waBroadcast.controller.js` — [NEW] Controller utama untuk manajemen broadcast WhatsApp
  - `backend/src/controllers/waChat.controller.js` — Penyesuaian terkait integrasi broadcast
  - `backend/src/controllers/waInternal.controller.js` — [NEW] Controller untuk webhook internal WhatsApp
  - `backend/src/lib/queueConnection.js` — [NEW] Konfigurasi koneksi BullMQ/Redis untuk message queue
  - `backend/src/locales/en/translation.json` — Penambahan 9 key i18n (EN) untuk modul broadcast
  - `backend/src/locales/id/translation.json` — Penambahan 9 key i18n (ID) untuk modul broadcast
  - `backend/src/models/waBroadcast.model.js` — [NEW] Model database untuk data broadcast (jadwal, status, template, target)
  - `backend/src/models/waBroadcastRecipient.model.js` — [NEW] Model database untuk penerima broadcast (status pengiriman per kontak)
  - `backend/src/models/waConversation.model.js` — Penyesuaian field terkait broadcast
  - `backend/src/routes/internal.route.js` — [NEW] Route untuk endpoint internal service-to-service
  - `backend/src/routes/waBroadcast.route.js` — [NEW] Route API untuk operasi CRUD broadcast dan pengiriman massal
  - `backend/src/services/option.service.js` — Penyesuaian pengambilan opsi template WhatsApp
  - `backend/src/services/waBroadcast.service.js` — [NEW] Service layer utama untuk logika bisnis broadcast (1578 baris)
  - `backend/src/services/waBroadcastQueue.service.js` — [NEW] Service untuk mengelola antrian pengiriman broadcast
  - `backend/src/services/waConversation.service.js` — Penyesuaian integrasi broadcast pada percakapan
  - `backend/src/services/waSender.service.js` — Penambahan method pengiriman broadcast
  - `backend/src/services/waTemplateVariable.service.js` — Penyesuaian variable template untuk broadcast
  - `backend/src/utils/serviceStatusLabels.js` — Penambahan label status broadcast
  - `backend/src/utils/validation-data.js` — Penambahan validasi data untuk broadcast
  - `cron-worker/src/app.js` — Registrasi worker baru untuk pemrosesan broadcast
  - `cron-worker/src/jobs/processors/waBroadcastDispatch.js` — [NEW] Processor untuk dispatch broadcast ke queue
  - `cron-worker/src/jobs/processors/waBroadcastReminderCheck.js` — [NEW] Processor untuk pengecekan reminder broadcast
  - `cron-worker/src/jobs/processors/waBroadcastSend.js` — [NEW] Processor untuk pengiriman broadcast via WhatsApp API
  - `cron-worker/src/jobs/scheduler.js` — Penambahan jadwal cron untuk broadcast
  - `cron-worker/src/jobs/waBroadcastWorkers.js` — [NEW] Daftar worker untuk pemrosesan broadcast
  - `cron-worker/src/jobs/worker.js` — Penyesuaian worker registration
  - `cron-worker/src/services/api.service.js` — [NEW] Service untuk komunikasi API antar-service pada cron-worker
  - `frontend/src/app/navigation/customerService.js` — Penambahan menu WhatsApp Broadcast di navigasi
  - `frontend/src/app/pages/customerService/whatsappBroadcast/index.jsx` — [NEW] Halaman utama daftar broadcast
  - `frontend/src/app/pages/customerService/whatsappBroadcast/schema/columns.jsx` — [NEW] Konfigurasi kolom tabel broadcast
  - `frontend/src/app/pages/customerService/whatsappBroadcast/schema/recipientColumns.jsx` — [NEW] Konfigurasi kolom tabel penerima broadcast
  - `frontend/src/app/pages/customerService/whatsappBroadcast/schema/rows.jsx` — [NEW] Konfigurasi baris (wrapper cell) untuk tabel broadcast
  - `frontend/src/app/pages/customerService/whatsappBroadcast/utils/exampleVariableContext.js` — [NEW] Contoh konteks variable template
  - `frontend/src/app/pages/customerService/whatsappBroadcast/utils/extractPlaceholders.js` — [NEW] Utilitas ekstraksi placeholder dari template
  - `frontend/src/app/pages/customerService/whatsappBroadcast/utils/getVariableContext.js` — [NEW] Utilitas pengambilan konteks variable template
  - `frontend/src/app/pages/customerService/whatsappBroadcast/utils/resolveMessagePreview.js` — [NEW] Utilitas render preview pesan broadcast
  - `frontend/src/app/pages/customerService/components/AccumulatedRecipientList.jsx` — [NEW] Komponen daftar akumulasi penerima broadcast
  - `frontend/src/app/pages/customerService/components/BroadcastDetailDrawer.jsx` — [NEW] Drawer detail broadcast (log pengiriman, status penerima)
  - `frontend/src/app/pages/customerService/components/CreateBroadcastDrawer.jsx` — [NEW] Drawer pembuatan broadcast baru (pilih template, tentukan penerima)
  - `frontend/src/app/pages/customerService/components/RecipientPicker.jsx` — [NEW] Komponen pemilih penerima broadcast dengan filter dan pencarian
  - `frontend/src/app/pages/customerService/components/TemplateVariableDocsModal.jsx` — [NEW] Modal dokumentasi variable template WhatsApp
  - `frontend/src/app/pages/customerService/components/TemplateVariableMapper.jsx` — [NEW] Komponen pemetaan variable template ke data sistem
  - `frontend/src/app/pages/settings/schema/applicationSchema.js` — Penghapusan field setting yang dipindah ke System
  - `frontend/src/app/pages/settings/schema/systemSchema.js` — Penambahan konfigurasi WhatsApp template di halaman System
  - `frontend/src/app/pages/settings/sections/Application.jsx` — Penghapusan bagian WhatsApp template dari Application
  - `frontend/src/app/pages/settings/sections/System.jsx` — Penambahan pengaturan WhatsApp Broadcast template di halaman System
  - `frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx` — Penyesuaian komponen preview template
  - `frontend/src/app/pages/tickets/TicketIconStatus.jsx` — Perbaikan minor icon status tiket
  - `frontend/src/app/router/customerService/whatsappBroadcastRoute.jsx` — [NEW] Route untuk halaman WhatsApp Broadcast
  - `frontend/src/app/router/protected.jsx` — Registrasi route baru ke dalam protected routes
  - `frontend/src/components/shared/form/TextEditor.jsx` — Penyesuaian komponen editor untuk dukungan broadcast
  - `frontend/src/i18n/locales/en/translations.json` — Penambahan 177 key i18n (EN) untuk modul broadcast
  - `frontend/src/i18n/locales/id/translations.json` — Penambahan 223 key i18n (ID) untuk modul broadcast
  - `whatsapp-api/.env.example` — Penghapusan variabel environment yang tidak lagi diperlukan
  - `whatsapp-api/src/config.js` — Penyesuaian konfigurasi WhatsApp API
  - `whatsapp-api/src/controllers/template.controller.js` — Penyesuaian controller template untuk dukungan broadcast
  - `whatsapp-api/src/routes/sendTemplate.route.js` — Penyesuaian route pengiriman template

- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan fitur **WhatsApp Broadcast** (pengiriman pesan massal WhatsApp) secara end-to-end, mencakup seluruh modul: Backend, Cron-Worker, Frontend, dan WhatsApp API.
  - **Backend**: Membuat model data `waBroadcast` dan `waBroadcastRecipient` untuk menyimpan data broadcast dan status penerima. Membuat service layer `waBroadcast.service.js` (1578 baris) yang menangani logika bisnis pembuatan, penjadwalan, dan pengiriman broadcast. Membuat controller dan route API untuk operasi CRUD serta pengiriman massal. Menambahkan endpoint internal untuk komunikasi antar-service.
  - **Cron-Worker**: Mengimplementasikan worker dan processor BullMQ untuk pemrosesan broadcast secara asinkron — dispatch ke queue, pengecekan reminder, dan pengiriman aktual via WhatsApp API.
  - **Frontend**: Membuat halaman manajemen broadcast lengkap dengan komponen `CreateBroadcastDrawer` (pembuatan broadcast), `RecipientPicker` (pemilihan penerima dengan filter), `BroadcastDetailDrawer` (detail dan log pengiriman), `TemplateVariableMapper` (pemetaan variable template), dan `AccumulatedRecipientList` (daftar akumulasi penerima). Menambahkan utilitas untuk ekstraksi placeholder, render preview pesan, dan dokumentasi variable template. Memindahkan pengaturan WhatsApp template dari halaman Application ke halaman System.
  - **WhatsApp API**: Menyesuaikan controller dan route template untuk mendukung pengiriman broadcast.
  - **i18n**: Menambahkan 232+ key terjemahan (ID & EN) untuk seluruh modul broadcast.

---

## 🌿 Branch: `master` — Attendance Management (Pengelolaan Kehadiran) Enhancement

### 📌 Informasi Issue

- **Nomor Issue**: #165
- **Judul Issue**: Attendance Management Enhancement
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`7ba5fdc`](7ba5fdc) - resolve #165 - 28 Juli 2026, 22:02

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json` — Penambahan privilege baru untuk akses fitur attendance
  - `backend/src/controllers/attendance.controller.js` — Refaktor besar-besaran controller attendance (+190 baris) — penambahan endpoint baru untuk detail kehadiran dan riwayat permintaan
  - `backend/src/controllers/files.controller.js` — Penyesuaian controller file untuk dukungan upload file permintaan attendance
  - `backend/src/models/attendanceAbsence.model.js` — Penyesuaian model attendance absence
  - `backend/src/models/attendancePermit.model.js` — Penyesuaian model attendance permit
  - `backend/src/models/attendancePresence.model.js` — Penyesuaian model attendance presence
  - `backend/src/routes/attendance.route.js` — [NEW] Pemisahan route attendance ke file terpisah (+136 baris)
  - `backend/src/routes/files.route.js` — Penyesuaian route file upload
  - `backend/src/services/attendance.service.js` — Penambahan method service baru untuk detail dan riwayat kehadiran (+78 baris)
  - `backend/src/utils/data-table.js` — Penyesuaian utilitas data-table untuk query attendance
  - `backend/src/utils/resolveAdminFilter.js` — [NEW] Utilitas untuk resolve filter admin pada query attendance
  - `frontend/src/app/navigation/activities.js` — Penambahan submenu Paid Leave dan Permission di navigasi Activities
  - `frontend/src/app/pages/activities/attendance/components/ManualCheckInDrawer.jsx` — Penyesuaian komponen manual check-in
  - `frontend/src/app/pages/activities/attendance/components/ManualCheckOutDrawer.jsx` — Penyesuaian komponen manual check-out
  - `frontend/src/app/pages/activities/attendance/index.jsx` — Refaktor halaman utama attendance (+229 baris) — penambahan tab dan filter baru
  - `frontend/src/app/pages/activities/attendance/components/AttendanceDetailModal.jsx` — [NEW] Modal detail kehadiran (277 baris) — menampilkan info lengkap kehadiran harian
  - `frontend/src/app/pages/activities/attendance/components/AttendanceRequestDetailModal.jsx` — [NEW] Modal detail permintaan attendance (323 baris) — menampilkan detail izin, sakit, dan cuti
  - `frontend/src/app/pages/activities/paidLeave/index.jsx` — [NEW] Halaman manajemen cuti berbayar (paid leave)
  - `frontend/src/app/pages/activities/paidLeave/schema/columns.jsx` — [NEW] Konfigurasi kolom tabel paid leave
  - `frontend/src/app/pages/activities/permission/index.jsx` — [NEW] Halaman manajemen izin (permission)
  - `frontend/src/app/pages/activities/permission/schema/columns.jsx` — [NEW] Konfigurasi kolom tabel permission
  - `frontend/src/app/pages/fiberCable/components/NodeInfoDrawer.jsx` — Perbaikan minor pada drawer info node
  - `frontend/src/app/pages/profile/index.jsx` — Penyesuaian halaman profil
  - `frontend/src/app/pages/users/admin/profile.jsx` — Penyesuaian profil admin — integrasi tab attendance
  - `frontend/src/app/pages/users/components/UserAttendanceTabs.jsx` — [NEW] Komponen tab kehadiran pada profil user (325 baris) — menampilkan riwayat kehadiran user
  - `frontend/src/app/pages/users/employee/profile.jsx` — Penyesuaian profil employee — integrasi tab attendance
  - `frontend/src/app/router/activities/paidLeave.jsx` — [NEW] Route untuk halaman paid leave
  - `frontend/src/app/router/activities/permission.jsx` — [NEW] Route untuk halaman permission
  - `frontend/src/app/router/protected.jsx` — Penambahan route baru ke protected routes
  - `frontend/src/components/shared/table/RowActions.jsx` — Penyesuaian komponen RowActions
  - `frontend/src/components/shared/table/rows.jsx` — Penambahan wrapper cell baru untuk tabel attendance
  - `frontend/src/components/shared/table/status.js` — Penambahan status badge untuk attendance
  - `frontend/src/i18n/locales/en/translations.json` — Penambahan 21 key i18n (EN) untuk modul attendance
  - `frontend/src/i18n/locales/id/translations.json` — Penambahan 21 key i18n (ID) untuk modul attendance
  - `frontend/src/utils/timeFormatter.js` — [NEW] Utilitas format waktu untuk tampilan kehadiran

- **Deskripsi Perubahan & Fungsi**:
  - Melakukan peningkatan signifikan pada modul **Attendance Management** (Pengelolaan Kehadiran).
  - **Backend**: Memisahkan route attendance ke file terpisah (`attendance.route.js`), menambahkan endpoint untuk detail kehadiran dan riwayat permintaan. Refaktor controller attendance dengan penambahan logika baru untuk filter dan aggregasi data kehadiran. Menambahkan utilitas `resolveAdminFilter.js` untuk resolve filter admin pada query attendance.
  - **Frontend**: Membuat modal detail kehadiran (`AttendanceDetailModal`) dan detail permintaan (`AttendanceRequestDetailModal`) untuk menampilkan informasi lengkap. Menambahkan halaman baru untuk **Paid Leave** (Cuti Berbayar) dan **Permission** (Izin) beserta konfigurasi kolom tabelnya. Membuat komponen `UserAttendanceTabs` (325 baris) yang menampilkan riwayat kehadiran pada profil user/admin. Memperbarui navigasi Activities dengan submenu baru.
  - **i18n**: Menambahkan 42 key terjemahan (ID & EN) untuk modul attendance.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                             | Dampak Utama                                                                                                         |
| ----- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| #162  | WhatsApp Broadcast                | Fitur pengiriman pesan massal WhatsApp end-to-end (Backend + Cron-Worker + Frontend + WhatsApp API)                  |
| #165  | Attendance Management Enhancement | Peningkatan modul kehadiran dengan detail modal, halaman Paid Leave & Permission, dan integrasi tab pada profil user |

### Kemampuan Baru Pengguna/Admin

- Admin dapat membuat dan mengirim **broadcast WhatsApp** massal ke banyak penerima sekaligus dengan template yang dapat dikustomisasi
- Admin dapat memantau **status pengiriman broadcast** (terkirim, gagal, tertunda) per penerima melalui detail broadcast
- Admin dapat memilih penerima broadcast dengan **filter dan pencarian** yang fleksibel (RecipientPicker)
- Admin dapat melihat **preview pesan** sebelum mengirim broadcast
- Admin dapat mengelola **variable template** WhatsApp dan melihat dokumentasi penggunaannya
- Admin dapat melihat **detail kehadiran** harian karyawan melalui modal detail Attendance
- Admin dapat melihat **riwayat permintaan attendance** (izin, sakit, cuti) dengan detail lengkap
- Admin dapat mengakses halaman **Paid Leave** (Cuti Berbayar) dan **Permission** (Izin) yang sebelumnya tidak tersedia
- Admin dapat melihat **riwayat kehadiran** karyawan langsung dari profil user/admin melalui tab Attendance

### Bug Fix / Solusi Masalah

- Perbaikan pada komponen `TicketIconStatus` untuk icon status yang lebih akurat
- Penyesuaian field setting WhatsApp template dari halaman Application dipindah ke halaman System untuk konsistensi navigasi

### Menu/Fitur Baru

- **WhatsApp Broadcast** — Menu baru di bawah Customer Service untuk mengelola pengiriman pesan massal WhatsApp
- **Paid Leave** — Menu baru di bawah Activities untuk manajemen cuti berbayar
- **Permission** — Menu baru di bawah Activities untuk manajemen izin
- **Attendance Detail Modal** — Modal detail kehadiran pada halaman Attendance
- **User Attendance Tabs** — Tab kehadiran pada halaman profil user/admin

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### WhatsApp Broadcast

- **Penjelasan Fitur**: WhatsApp Broadcast adalah fitur pengiriman pesan WhatsApp secara massal ke banyak penerima sekaligus. Fitur ini menggunakan system antrian (BullMQ) untuk memastikan pengiriman yang reliable dan terjadwal. Admin dapat membuat broadcast dengan memilih template WhatsApp, memetakan variable template ke data sistem, menentukan penerima, dan mengirimkannya. Setiap pengiriman diproses secara asinkron oleh cron-worker, dan status pengiriman dapat dipantau melalui detail broadcast.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Customer Service → WhatsApp Broadcast** dari sidebar navigasi
  2. Klik tombol **Buat Broadcast** untuk membuka drawer pembuatan broadcast baru
  3. Pilih **Template WhatsApp** yang akan digunakan dari daftar template yang tersedia
  4. **Peta variable template** ke data sistem menggunakan TemplateVariableMapper (misal: `{{name}}` → field nama pelanggan)
  5. **Pilih penerima** menggunakan RecipientPicker — dapat difilter berdasarkan grup, lokasi, atau pencarian manual
  6. **Pratinjau pesan** untuk memastikan variable sudah terisi dengan benar
  7. Klik **Kirim** untuk menjadwalkan pengiriman broadcast
  8. Pantau **status pengiriman** melalui BroadcastDetailDrawer — setiap penerima memiliki status tersendiri (terkirim, gagal, tertunda)

### Attendance Management Enhancement

- **Penjelasan Fitur**: Modul Attendance Management telah ditingkatkan dengan penambahan detail modal, halaman Paid Leave dan Permission, serta integrasi tab kehadiran pada profil user. Admin kini dapat melihat detail kehadiran harian dan riwayat permintaan attendance dengan lebih lengkap.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Activities → Attendance** untuk melihat daftar kehadiran karyawan
  2. Klik pada baris kehadiran untuk membuka **Attendance Detail Modal** — menampilkan info lengkap: waktu check-in/out, lokasi, durasi kerja, dan status
  3. Untuk melihat riwayat izin/sakit/cuti, klik pada permintaan attendance untuk membuka **AttendanceRequestDetailModal**
  4. Buka menu **Activities → Paid Leave** untuk mengelola cuti berbayar karyawan
  5. Buka menu **Activities → Permission** untuk mengelola izin karyawan
  6. Untuk melihat riwayat kehadiran spesifik, buka profil user/admin dan buka tab **Attendance** — menampilkan grafik dan daftar kehadiran historis
