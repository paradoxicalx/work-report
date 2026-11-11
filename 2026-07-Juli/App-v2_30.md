# 📝 Daily Work Report - Dedy S.N Putra (2026-07-30)

---

## 📅 Laporan Harian - 30 Juli 2026

---

## 🌿 Branch: `master` — Manajemen Perangkat Absensi (Attendance Device)

### 📌 Informasi Issue

- **Nomor Issue**: #176
- **Judul Issue**: Manajemen Perangkat Absensi (Attendance Device Management)
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`906464d`](906464d) - resolve #176 - 30 Juli 2026, 21:46

- **Komponen yang Berubah**:
  - [`backend/src/models/attendanceDevice.model.js`](backend/src/models/attendanceDevice.model.js) [NEW]
  - [`backend/src/models/attendanceDeviceLog.model.js`](backend/src/models/attendanceDeviceLog.model.js) [NEW]
  - [`backend/src/controllers/attendanceDevice.controller.js`](backend/src/controllers/attendanceDevice.controller.js) [NEW]
  - [`backend/src/services/attendanceDevice.service.js`](backend/src/services/attendanceDevice.service.js) [NEW]
  - [`backend/src/services/attendanceDeviceLog.service.js`](backend/src/services/attendanceDeviceLog.service.js) [NEW]
  - [`backend/src/services/attendanceDeviceSync.service.js`](backend/src/services/attendanceDeviceSync.service.js) [NEW]
  - [`backend/src/lib/attendanceDeviceClient.js`](backend/src/lib/attendanceDeviceClient.js) [NEW]
  - [`backend/src/routes/attendanceDevice.route.js`](backend/src/routes/attendanceDevice.route.js) [NEW]
  - [`backend/src/services/cronWorkerControl.service.js`](backend/src/services/cronWorkerControl.service.js) [NEW]
  - [`backend/src/services/admin.service.js`](backend/src/services/admin.service.js)
  - [`backend/src/services/attendance.service.js`](backend/src/services/attendance.service.js)
  - [`backend/src/models/admin.model.js`](backend/src/models/admin.model.js)
  - [`backend/src/models/attendancePresence.model.js`](backend/src/models/attendancePresence.model.js)
  - [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js)
  - [`backend/src/controllers/settings.controller.js`](backend/src/controllers/settings.controller.js)
  - [`backend/src/routes/internal.route.js`](backend/src/routes/internal.route.js)
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/app.js`](backend/src/app.js)
  - [`backend/src/server.js`](backend/src/server.js)
  - [`backend/patches/zkteco-js+1.7.2.patch`](backend/patches/zkteco-js+1.7.2.patch) [NEW]
  - [`cron-worker/src/jobs/processors/attendanceDeviceSync.js`](cron-worker/src/jobs/processors/attendanceDeviceSync.js) [NEW]
  - [`cron-worker/src/config/db.js`](cron-worker/src/config/db.js) [NEW]
  - [`cron-worker/src/config/env.js`](cron-worker/src/config/env.js) [NEW]
  - [`cron-worker/src/models/option.model.js`](cron-worker/src/models/option.model.js) [NEW]
  - [`cron-worker/src/controllers/cron.controller.js`](cron-worker/src/controllers/cron.controller.js)
  - [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js)
  - [`cron-worker/src/jobs/worker.js`](cron-worker/src/jobs/worker.js)
  - [`cron-worker/src/routes/cron.routes.js`](cron-worker/src/routes/cron.routes.js)
  - [`cron-worker/src/services/api.service.js`](cron-worker/src/services/api.service.js)
  - [`cron-worker/src/app.js`](cron-worker/src/app.js)
  - [`frontend/src/app/pages/activities/attendanceDevice/index.jsx`](frontend/src/app/pages/activities/attendanceDevice/index.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/create.jsx`](frontend/src/app/pages/activities/attendanceDevice/create.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/edit.jsx`](frontend/src/app/pages/activities/attendanceDevice/edit.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/mapping.jsx`](frontend/src/app/pages/activities/attendanceDevice/mapping.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/schema/deviceColumns.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/deviceColumns.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/schema/deviceSchema.js`](frontend/src/app/pages/activities/attendanceDevice/schema/deviceSchema.js) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/schema/DeviceExtraActions.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/DeviceExtraActions.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx) [NEW]
  - [`frontend/src/app/pages/activities/attendanceDevice/schema/mappingSchema.js`](frontend/src/app/pages/activities/attendanceDevice/schema/mappingSchema.js) [NEW]
  - [`frontend/src/app/router/activities/attendanceDevice.jsx`](frontend/src/app/router/activities/attendanceDevice.jsx) [NEW]
  - [`frontend/src/app/navigation/activities.js`](frontend/src/app/navigation/activities.js)
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/pages/activities/attendance/index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx)
  - [`frontend/src/app/pages/settings/sections/System.jsx`](frontend/src/app/pages/settings/sections/System.jsx)
  - [`frontend/src/app/pages/settings/schema/systemSchema.js`](frontend/src/app/pages/settings/schema/systemSchema.js)
  - [`frontend/src/components/shared/form/LocationPickerInput.jsx`](frontend/src/components/shared/form/LocationPickerInput.jsx)
  - [`frontend/src/components/shared/table/RowActions.jsx`](frontend/src/components/shared/table/RowActions.jsx)
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
  - [`frontend/src/utils/hasPrivilege.js`](frontend/src/utils/hasPrivilege.js)

- **Deskripsi Perubahan & Fungsi**:
  - **Implementasi penuh modul Manajemen Perangkat Absensi** dari backend hingga frontend.
  - **Backend**: Membuat model database [`attendanceDevice.model.js`](backend/src/models/attendanceDevice.model.js) dan [`attendanceDeviceLog.model.js`](backend/src/models/attendanceDeviceLog.model.js) untuk menyimpan data perangkat absensi dan log aktivitasnya. Membuat service layer ([`attendanceDevice.service.js`](backend/src/services/attendanceDevice.service.js), [`attendanceDeviceLog.service.js`](backend/src/services/attendanceDeviceLog.service.js)) untuk akses data, serta [`attendanceDeviceSync.service.js`](backend/src/services/attendanceDeviceSync.service.js) untuk mensinkronkan data perangkat dari ZKTeco secara otomatis. Membuat [`attendanceDeviceClient.js`](backend/src/lib/attendanceDeviceClient.js) sebagai client gRPC/HTTP untuk berkomunikasi langsung dengan perangkat absensi ZKTeco. Membuat controller [`attendanceDevice.controller.js`](backend/src/controllers/attendanceDevice.controller.js) dan route [`attendanceDevice.route.js`](backend/src/routes/attendanceDevice.route.js) dengan endpoint lengkap (CRUD perangkat, sinkronisasi, log). Patch untuk library `zteco-js` ditambahkan di [`patches/zkteco-js+1.7.2.patch`](backend/patches/zkteco-js+1.7.2.patch).
  - **Cron Worker**: Menambahkan job [`attendanceDeviceSync.js`](cron-worker/src/jobs/processors/attendanceDeviceSync.js) untuk sinkronisasi berkala perangkat absensi. Memisahkan konfigurasi database ([`db.js`](cron-worker/src/config/db.js)) dan environment ([`env.js`](cron-worker/src/config/env.js)) ke modul tersendiri.
  - **Frontend**: Membuat halaman utama daftar perangkat ([`index.jsx`](frontend/src/app/pages/activities/attendanceDevice/index.jsx)), form tambah ([`create.jsx`](frontend/src/app/pages/activities/attendanceDevice/create.jsx)), form edit ([`edit.jsx`](frontend/src/app/pages/activities/attendanceDevice/edit.jsx)), dan halaman pemetaan lokasi ([`mapping.jsx`](frontend/src/app/pages/activities/attendanceDevice/mapping.jsx)). Membuat schema kolom tabel ([`deviceColumns.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/deviceColumns.jsx), [`mappingColumns.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx)), validasi form ([`deviceSchema.js`](frontend/src/app/pages/activities/attendanceDevice/schema/deviceSchema.js), [`mappingSchema.js`](frontend/src/app/pages/activities/attendanceDevice/schema/mappingSchema.js)), dan komponen aksi tambahan ([`DeviceExtraActions.jsx`](frontend/src/app/pages/activities/attendanceDevice/schema/DeviceExtraActions.jsx)) untuk eksekusi perangkat. Route [`attendanceDevice.jsx`](frontend/src/app/router/activities/attendanceDevice.jsx) ditambahkan untuk navigasi.
  - **Integrasi**: Endpoint internal untuk komunikasi cron worker → backend ditambahkan di [`internal.controller.js`](backend/src/controllers/internal.controller.js) dan [`internal.route.js`](backend/src/routes/internal.route.js). Pengaturan URL perangkat absensi ditambahkan di halaman Settings → System. Privilege akses [`attendanceDevice.*`](backend/src/config/privilege.json) ditambahkan ke konfigurasi izin. Komponen [`RowActions`](frontend/src/components/shared/table/RowActions.jsx) dan [`rows.jsx`](frontend/src/components/shared/table/rows.jsx) diperbarui untuk mendukung aksi eksekusi perangkat. [`hasPrivilege.js`](frontend/src/utils/hasPrivilege.js) diperbarui untuk mendukung pengecekan privilese yang lebih fleksibel.

---

## 🌿 Branch: `issue-174` — Sistem Kalender & Scheduler

### 📌 Informasi Issue

- **Nomor Issue**: #174
- **Judul Issue**: Sistem Kalender & Scheduler (Calendar & Scheduler System)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`1cafd03`](1cafd03) - save #174 - 30 Juli 2026, 21:14

- **Komponen yang Berubah**:
  - [`backend/src/models/calendar.model.js`](backend/src/models/calendar.model.js) [NEW]
  - [`backend/src/models/schedule.model.js`](backend/src/models/schedule.model.js) [NEW]
  - [`backend/src/controllers/calendar.controller.js`](backend/src/controllers/calendar.controller.js) [NEW]
  - [`backend/src/controllers/scheduler.controller.js`](backend/src/controllers/scheduler.controller.js) [NEW]
  - [`backend/src/services/calendar.service.js`](backend/src/services/calendar.service.js) [NEW]
  - [`backend/src/services/scheduler.service.js`](backend/src/services/scheduler.service.js) [NEW]
  - [`backend/src/routes/calendar.route.js`](backend/src/routes/calendar.route.js) [NEW]
  - [`backend/src/routes/scheduler.route.js`](backend/src/routes/scheduler.route.js) [NEW]
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/app.js`](backend/src/app.js)
  - [`frontend/src/app/pages/activities/calendar/index.jsx`](frontend/src/app/pages/activities/calendar/index.jsx) [NEW]
  - [`frontend/src/app/pages/activities/calendar/components/EventModal.jsx`](frontend/src/app/pages/activities/calendar/components/EventModal.jsx) [NEW]
  - [`frontend/src/app/pages/activities/calendar/components/UnscheduledTicketsSidebar.jsx`](frontend/src/app/pages/activities/calendar/components/UnscheduledTicketsSidebar.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/index.jsx`](frontend/src/app/pages/activities/scheduler/index.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/create.jsx`](frontend/src/app/pages/activities/scheduler/create.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/edit.jsx`](frontend/src/app/pages/activities/scheduler/edit.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/detail.jsx`](frontend/src/app/pages/activities/scheduler/detail.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/schema/columns.jsx`](frontend/src/app/pages/activities/scheduler/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/components/TelegramMessageModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TelegramMessageModal.jsx) [NEW]
  - [`frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx) [NEW]
  - [`frontend/src/app/router/activities/calendar.jsx`](frontend/src/app/router/activities/calendar.jsx) [NEW]
  - [`frontend/src/app/router/activities/scheduler.jsx`](frontend/src/app/router/activities/scheduler.jsx) [NEW]
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/navigation/activities.js`](frontend/src/app/navigation/activities.js)
  - [`frontend/src/styles/app/vendors/fullcalendar.css`](frontend/src/styles/app/vendors/fullcalendar.css) [NEW]
  - [`frontend/src/utils/priorityHelpers.js`](frontend/src/utils/priorityHelpers.js) [NEW]
  - [`frontend/src/components/shared/ConfirmModal.jsx`](frontend/src/components/shared/ConfirmModal.jsx)
  - [`frontend/src/components/shared/table/Table.jsx`](frontend/src/components/shared/table/Table.jsx)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
  - [`frontend/vite.config.js`](frontend/vite.config.js)

- **Deskripsi Perubahan & Fungsi**:
  - **Implementasi penuh modul Kalender dan Scheduler** dari backend hingga frontend.
  - **Backend — Kalender**: Membuat model [`calendar.model.js`](backend/src/models/calendar.model.js) untuk event kalender, controller [`calendar.controller.js`](backend/src/controllers/calendar.controller.js) untuk operasi CRUD event, service [`calendar.service.js`](backend/src/services/calendar.service.js) untuk logika bisnis, dan route [`calendar.route.js`](backend/src/routes/calendar.route.js) untuk endpoint API kalender.
  - **Backend — Scheduler**: Membuat model [`scheduler.model.js`](backend/src/models/schedule.model.js) untuk data jadwal penugasan teknisi, controller [`scheduler.controller.js`](backend/src/controllers/scheduler.controller.js), service [`scheduler.service.js`](backend/src/services/scheduler.service.js), dan route [`scheduler.route.js`](backend/src/routes/scheduler.route.js) untuk manajemen jadwal kerja.
  - **Frontend — Kalender**: Membuat halaman kalender interaktif ([`calendar/index.jsx`](frontend/src/app/pages/activities/calendar/index.jsx)) menggunakan library FullCalendar dengan komponen [`EventModal.jsx`](frontend/src/app/pages/activities/calendar/components/EventModal.jsx) untuk detail event dan [`UnscheduledTicketsSidebar.jsx`](frontend/src/app/pages/activities/calendar/components/UnscheduledTicketsSidebar.jsx) untuk menampilkan tiket yang belum dijadwalkan. CSS khusus FullCalendar ditambahkan di [`fullcalendar.css`](frontend/src/styles/app/vendors/fullcalendar.css).
  - **Frontend — Scheduler**: Membuat halaman daftar jadwal ([`scheduler/index.jsx`](frontend/src/app/pages/activities/scheduler/index.jsx)), form buat ([`create.jsx`](frontend/src/app/pages/activities/scheduler/create.jsx)), form edit ([`edit.jsx`](frontend/src/app/pages/activities/scheduler/edit.jsx)), dan halaman detail ([`detail.jsx`](frontend/src/app/pages/activities/scheduler/detail.jsx)). Membuat komponen pendukung: [`TeamFormModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx) untuk form tim teknisi, [`TelegramMessageModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TelegramMessageModal.jsx) untuk notifikasi Telegram, dan [`TicketAssignModal.jsx`](frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx) untuk penugasan tiket ke jadwal.
  - **Utilitas**: [`priorityHelpers.js`](frontend/src/utils/priorityHelpers.js) ditambahkan sebagai helper untuk menentukan prioritas tiket. Library `@fullcalendar/core`, `@fullcalendar/daygrid`, `@fullcalendar/timegrid`, `@fullcalendar/interaction`, dan `@fullcalendar/react` ditambahkan ke dependensi frontend.

---

## 🌿 Branch: `issue-172` — Audit & Perbaikan Hak Akses

### 📌 Informasi Issue

- **Nomor Issue**: #172
- **Judul Issue**: Audit & Perbaikan Hak Akses (Privilege Audit & Fix)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`1adb0a2`](1adb0a2) - save #172 - 30 Juli 2026, 13:37

- **Komponen yang Berubah**:
  - [`AUDIT_HAK_AKSES.md`](AUDIT_HAK_AKSES.md) [NEW]
  - [`TASK_PERBAIKAN_HAK_AKSES.md`](TASK_PERBAIKAN_HAK_AKSES.md) [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - **Dokumentasi Audit Hak Akses**: Membuat dokumen [`AUDIT_HAK_AKSES.md`](AUDIT_HAK_AKSES.md) yang berisi analisis dan catatan audit lengkap terhadap sistem privilege/izin yang ada di seluruh modul DEKASIMAL V2. Dokumen ini memetakan semua hak akses yang terdaftar di [`privilege.json`](backend/src/config/privilege.json) dan mengidentifikasi celah atau inkonsistensi pada pengecekan hak akses di frontend dan backend.
  - **Dokumen Tugas Perbaikan**: Membuat [`TASK_PERBAIKAN_HAK_AKSES.md`](TASK_PERBAIKAN_HAK_AKSES.md) yang memuat daftar tugas konkret (action items) untuk perbaikan sistem hak akses, termasuk penambahan privilege yang belum terdaftar, perbaikan pengecekan di frontend, dan standarisasi naming privilege di seluruh modul.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                       | Dampak Utama                                                                                                                                                          |
| ----- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #176  | Manajemen Perangkat Absensi | Modul lengkap pengelolaan perangkat absensi ZKTeco: daftar, tambah, edit, hapus, sinkronisasi data otomatis, pemetaan lokasi, dan monitoring log aktivitas perangkat. |
| #174  | Sistem Kalender & Scheduler | Modul kalender interaktif untuk visualisasi event/tiket dan sistem scheduler untuk penugasan jadwal kerja teknisi dengan integrasi notifikasi Telegram.               |
| #172  | Audit & Perbaikan Hak Akses | Dokumentasi audit dan rencana perbaikan sistem privilege agar seluruh modul memiliki pengecekan hak akses yang konsisten dan komprehensif.                            |

### Kemampuan Baru Pengguna/Admin

- Admin dapat mengelola perangkat absensi (tambah, edit, hapus, mapping lokasi) melalui menu **Aktivitas → Perangkat Absensi**.
- Admin dapat melakukan sinkronisasi data perangkat absensi ZKTeco secara manual atau otomatis (melalui cron worker).
- Admin dapat melihat log aktivitas perangkat absensi untuk monitoring status dan riwayat koneksi.
- Admin dapat melihat dan mengelola event di kalender interaktif (FullCalendar) yang menampilkan tiket dan jadwal kerja.
- Admin dapat membuat jadwal kerja (scheduler) untuk tim teknisi, menugaskan tiket ke jadwal, dan mengirim notifikasi Telegram terkait jadwal.
- Admin dapat melihat tiket yang belum dijadwalkan melalui sidebar di halaman kalender.

### Bug Fix / Solusi Masalah

- Patch [`zteco-js+1.7.2.patch`](backend/patches/zkteco-js+1.7.2.patch) dibuat untuk memperbaiki isu kompatibilitas pada library ZKTeco JS yang digunakan untuk komunikasi dengan perangkat absensi.
- Pemisahan konfigurasi database dan environment di cron worker untuk mengatasi dependensi yang sebelumnya tercampur.

### Menu/Fitur Baru

- **Menu Perangkat Absensi** (Aktivitas → Perangkat Absensi): Halaman daftar perangkat, form tambah/edit, halaman mapping lokasi, dan tombol eksekusi perangkat.
- **Menu Kalender** (Aktivitas → Kalender): Tampilan kalender interaktif dengan event dan sidebar tiket yang belum dijadwalkan.
- **Menu Scheduler** (Aktivitas → Scheduler): Halaman daftar jadwal, form buat/edit jadwal, halaman detail jadwal, dan modal penugasan tiket/notifikasi.
- **Pengaturan URL Perangkat Absensi** di menu Settings → System untuk mengonfigurasi alamat IP/host perangkat ZKTeco.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Fitur Manajemen Perangkat Absensi

- **Penjelasan Fitur**: Modul ini memungkinkan admin mengelola perangkat absensi ZKTeco yang terhubung ke sistem. Setiap perangkat dapat diidentifikasi dengan nama, IP address, tipe perangkat, dan lokasi. Sistem secara otomatis melakukan sinkronisasi data presensi dari perangkat ke database melalui cron worker. Admin juga dapat melakukan sinkronisasi manual dan memantau log aktivitas setiap perangkat.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Aktivitas → Perangkat Absensi** untuk melihat daftar perangkat.
  2. Klik **Tambah Perangkat** untuk mendaftarkan perangkat baru (isi nama, IP, tipe, lokasi).
  3. Buka halaman **Mapping** untuk memetakan perangkat ke lokasi fisik di peta.
  4. Klik tombol **Eksekusi** pada baris perangkat untuk melakukan sinkronisasi data presensi secara manual.
  5. Lihat log aktivitas perangkat untuk memantau status koneksi dan riwayat sinkronisasi.
  6. Konfigurasi URL perangkat absensi di **Settings → System** jika diperlukan.

### Fitur Sistem Kalender & Scheduler

- **Penjelasan Fitur**: Kalender menampilkan seluruh event dan tiket dalam format kalender interaktif (FullCalendar), memudahkan admin melihat jadwal secara visual. Scheduler memungkinkan admin membuat jadwal kerja untuk tim teknisi, menugaskan tiket ke jadwal tertentu, dan mengirim notifikasi melalui Telegram kepada teknisi yang ditugaskan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Aktivitas → Kalender** untuk melihat tampilan kalender.
  2. Klik pada tanggal tertentu untuk membuat event baru atau melihat detail event yang sudah ada.
  3. Gunakan sidebar untuk melihat tiket yang belum dijadwalkan dan seret ke kalender untuk menjadwalkannya.
  4. Buka menu **Aktivitas → Scheduler** untuk mengelola jadwal kerja teknisi.
  5. Klik **Buat Jadwal** untuk membuat jadwal baru, pilih tim teknisi, dan tentukan tanggal/waktu.
  6. Gunakan **Modal Penugasan Tiket** untuk menetapkan tiket tertentu ke jadwal.
  7. Klik **Kirim Notifikasi Telegram** untuk memberitahu teknisi tentang jadwal yang sudah dibuat.
