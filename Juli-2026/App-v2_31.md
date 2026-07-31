# 📝 Daily Work Report - Dedy S.N Putra (2026-07-31)

---

## 📅 Laporan Harian - 31 Juli 2026

---

## 🌿 Branch: `issue-172` — Perbaikan Hak Akses Privilege (Resolve)

### 📌 Informasi Issue

- **Nomor Issue**: #172
- **Judul Issue**: Perbaikan Hak Akses Privilege (Audit & Refactor Sistem Privilege)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`82098f1`](../../commit/82098f1) - resolve #172 - 31 Juli 2026, 23:10 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md` — Dokumentasi arsitektur monorepo
  - `TASK_PERBAIKAN_HAK_AKSES.md` — Dokumen tugas perbaikan hak akses (diperbarui)
  - `backend/src/config/privilege.json` — Konfigurasi privilege yang sudah diperbarui & disederhanakan
  - `backend/src/middlewares/privilegeTicket.middleware.js` **[NEW]** — Middleware baru untuk pengecekan privilege tiket berdasarkan type
  - `backend/src/models/ticket.model.js` — Penambahan index pada field `ticket_id` untuk optimasi query
  - `backend/src/routes/customerSO.route.js` — Penyesuaian privilege check pada route Customer SO
  - `backend/src/routes/files.route.js` — Penyesuaian privilege check pada route file
  - `backend/src/routes/locationPoint.route.js` — Penyesuaian privilege check pada route lokasi
  - `backend/src/routes/ticket.route.js` — Penggantian `checkPrivilege('ticket.changeStatus')` menjadi `checkTicketPrivilegeByType('changeStatus')` untuk dinamis berdasarkan type tiket
  - `backend/src/utils/migrate-privilege-172.js` **[NEW]** — Skrip migrasi privilege untuk issue #172
  - `backend/src/utils/migrate-privilege-ticket-generic.js` **[NEW]** — Skrip migrasi privilege tiket generik
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx` — Penyesuaian privilege check di drawer tiket
  - `frontend/src/app/pages/tickets/installation/detail.jsx` — Penyesuaian privilege check di detail instalasi
  - `frontend/src/app/pages/users/business/profile.jsx` — Penyesuaian privilege check di profil bisnis
  - `frontend/src/app/pages/users/customer/profile.jsx` — Penyesuaian privilege check di profil pelanggan
  - `frontend/src/app/pages/users/customerSalesOrder/create.jsx` — Penyesuaian privilege check di pembuatan SO
  - `frontend/src/app/pages/users/partner/profile.jsx` — Penyesuaian privilege check di profil mitra
  - `frontend/src/app/pages/users/privilege/create.jsx` — Peningkatan halaman buat privilege: penambahan pencarian modul, deskripsi privilege per bahasa
  - `frontend/src/app/pages/users/privilege/detail.jsx` — Peningkatan halaman detail privilege: tampilan deskripsi privilege interaktif
  - `frontend/src/app/pages/users/privilege/edit.jsx` — Peningkatan halaman edit privilege: fitur copy privilege dari konfigurasi lain (mode replace/merge), pencarian modul, deskripsi privilege
  - `frontend/src/constants/privilegeDescriptions.en.json` **[NEW]** — Deskripsi privilege dalam Bahasa Inggris (253 key)
  - `frontend/src/constants/privilegeDescriptions.id.json` **[NEW]** — Deskripsi privilege dalam Bahasa Indonesia (253 key)
  - `frontend/src/i18n/locales/en/translations.json` — Penambahan key i18n baru untuk privilege
  - `frontend/src/i18n/locales/id/translations.json` — Penambahan key i18n baru untuk privilege

- **Deskripsi Perubahan & Fungsi**:
  - **Middleware Privilege Tiket Dinamis**: Membuat middleware baru [`checkTicketPrivilegeByType()`](backend/src/middlewares/privilegeTicket.middleware.js:9) yang secara otomatis menentukan privilege tiket berdasarkan URL param `:type` (survey, installation, dismantle, dll). Contoh: route `/ticket/survey/change-priority` akan secara otomatis mengecek privilege `ticketSurvey.changeStatus`. Sebelumnya semua type tiket menggunakan privilege generik `ticket.changeStatus` yang tidak spesifik.
  - **Privilege `ticketFiles.read`**: Mengganti privilege `ticket.read`/`ticket.update`/`ticket.changeStatus` generik menjadi `ticketFiles.read` yang lebih spesifik untuk akses file tiket.
  - **Penghapusan Privilege Usang**: Menghapus `customerSO.withoutPo`, `customerSO.send`, `networkSite.report` yang sudah tidak relevan.
  - **Deskripsi Privilege Lengkap**: Membuat 253 deskripsi privilege dalam format JSON untuk Bahasa Indonesia dan Inggris, yang ditampilkan secara interaktif di UI halaman privilege.
  - **Fitur Copy Privilege**: Menambahkan modal copy privilege di halaman edit yang memungkinkan admin menyalin konfigurasi role dari privilege lain dengan mode replace atau merge.
  - **Pencarian Modul**: Menambahkan fitur pencarian/filter pada tabel privilege agar admin dapat dengan cepat menemukan modul yang dicari.
  - **Index Database**: Menambahkan index pada field `ticket_id` di model Ticket untuk optimasi performa query pencarian tiket.
  - **Migration Scripts**: Membuat 2 skrip migrasi untuk memperbarui data privilege di database agar sesuai dengan konfigurasi baru.

---

## 🌿 Branch: `issue-174` — Modul Calendar & Scheduler (WIP)

### 📌 Informasi Issue

- **Nomor Issue**: #174
- **Judul Issue**: Modul Kalender & Scheduler (Rencana Kerja Harian)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`61efb1d`](../../commit/61efb1d) - save #174 - 31 Juli 2026, 22:44 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md` — Dokumentasi arsitektur monorepo (disederhanakan)
  - `backend/package.json` — Penambahan dependency baru
  - `backend/src/app.js` — Pendaftaran route baru untuk calendar dan scheduler
  - `backend/src/config/privilege.json` — Penambahan privilege untuk calendar dan scheduler
  - `backend/src/controllers/calendar.controller.js` **[NEW]** — Controller untuk operasi calendar (CRUD event, tiket belum terjadwal)
  - `backend/src/controllers/scheduler.controller.js` **[NEW]** — Controller untuk operasi scheduler (CRUD rencana kerja, tim, tiket)
  - `backend/src/locales/en/translation.json` — Penambahan key i18n untuk calendar & scheduler
  - `backend/src/locales/id/translation.json` — Penambahan key i18n untuk calendar & scheduler
  - `backend/src/models/calendar.model.js` **[NEW]** — Model Mongoose untuk event kalender
  - `backend/src/models/schedule.model.js` **[NEW]** — Model Mongoose untuk rencana kerja harian (dengan auto-increment ID)
  - `backend/src/routes/calendar.route.js` **[NEW]** — Route API untuk calendar (GET, POST, PATCH, DELETE)
  - `backend/src/routes/scheduler.route.js` **[NEW]** — Route API untuk scheduler (CRUD, draft team, telegram message, tiket list)
  - `backend/src/services/calendar.service.js` **[NEW]** — Service layer untuk akses data calendar
  - `backend/src/services/scheduler.service.js` **[NEW]** — Service layer untuk akses data scheduler
  - `frontend/package.json` — Penambahan dependency: `@fullcalendar/core`, `@fullcalendar/daygrid`, `@fullcalendar/interaction`, `@fullcalendar/list`, `@fullcalendar/react`, `@fullcalendar/timegrid`
  - `frontend/src/app/navigation/activities.js` — Pendaftaran menu navigasi untuk Calendar dan Scheduler
  - `frontend/src/app/pages/activities/calendar/index.jsx` **[NEW]** — Halaman utama Kalender dengan FullCalendar (dayGrid, timeGrid, list view), navigasi bulan, sidebar tiket belum terjadwal
  - `frontend/src/app/pages/activities/calendar/components/EventModal.jsx` **[NEW]** — Modal detail/edit event kalender
  - `frontend/src/app/pages/activities/calendar/components/UnscheduledTicketsSidebar.jsx` **[NEW]** — Sidebar daftar tiket yang belum dijadwalkan (drag-and-drop ke kalender)
  - `frontend/src/app/pages/activities/scheduler/index.jsx` **[NEW]** — Halaman daftar rencana kerja harian (datatable)
  - `frontend/src/app/pages/activities/scheduler/create.jsx` **[NEW]** — Halaman pembuatan rencana kerja baru (tim + tiket)
  - `frontend/src/app/pages/activities/scheduler/edit.jsx` **[NEW]** — Halaman edit rencana kerja
  - `frontend/src/app/pages/activities/scheduler/detail.jsx` **[NEW]** — Halaman detail rencana kerja (read-only)
  - `frontend/src/app/pages/activities/scheduler/schema/columns.jsx` **[NEW]** — Konfigurasi kolom datatable untuk scheduler
  - `frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx` **[NEW]** — Modal form susunan tim (nama tim, anggota)
  - `frontend/src/app/pages/activities/scheduler/components/TelegramMessageModal.jsx` **[NEW]** — Modal untuk mengirim pesan notifikasi Telegram ke tim
  - `frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx` **[NEW]** — Modal untuk assign tiket ke tim tertentu
  - `frontend/src/app/pages/activities/scheduler/components/UnscheduledTicketsSection.jsx` **[NEW]** — Bagian daftar tiket belum terjadwal di form scheduler
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx` — Integrasi drawer tiket dengan modul scheduler
  - `frontend/src/app/router/activities/calendar.jsx` **[NEW]** — Definisi route untuk halaman Calendar
  - `frontend/src/app/router/activities/scheduler.jsx` **[NEW]** — Definisi route untuk halaman Scheduler (list, create, detail, edit)
  - `frontend/src/app/router/protected.jsx` — Pendaftaran route calendar & scheduler ke routing utama
  - `frontend/src/components/shared/ConfirmModal.jsx` — Penambahan dukungan untuk modal konfirmasi
  - `frontend/src/components/shared/form/FormInput.jsx` — Peningkatan komponen form input (dukungan untuk scheduler)
  - `frontend/src/components/shared/table/Table.jsx` — Penyesuaian komponen tabel
  - `frontend/src/i18n/locales/en/translations.json` — Penambahan 144 key i18n untuk Calendar & Scheduler
  - `frontend/src/i18n/locales/id/translations.json` — Penambahan 143 key i18n untuk Calendar & Scheduler
  - `frontend/src/styles/app/index.css` — Penambahan style untuk modul baru
  - `frontend/src/styles/app/vendors/fullcalendar.css` **[NEW]** — Customisasi style FullCalendar
  - `frontend/src/utils/priorityHelpers.js` **[NEW]** — Utilitas untuk menentukan warna berdasarkan prioritas tiket
  - `frontend/vite.config.js` — Konfigurasi Vite untuk optimasi build (manual chunks untuk fullcalendar)

- **Deskripsi Perubahan & Fungsi**:
  - **Modul Kalender (Calendar)**: Membuat halaman kalender interaktif menggunakan library FullCalendar yang menampilkan seluruh event/kegiatan dalam berbagai tampilan (bulanan, mingguan, harian, dan daftar). Admin dapat membuat, mengedit, dan menghapus event kalender. Terdapat sidebar tiket yang belum dijadwalkan yang dapat di-drag ke kalender untuk penjadwalan cepat. Event kalender mendukung warna berdasarkan prioritas tiket.
  - **Modul Scheduler (Rencana Kerja Harian)**: Membuat modul perencanaan kerja harian yang memungkinkan admin membuat rencana kerja (schedule) dengan susunan tim dan assign tiket ke masing-masing tim. Fitur meliputi: CRUD rencana kerja, pengelolaan susunan tim (nama tim + anggota), assign tiket ke tim, detail rencana kerja read-only, dan pengiriman notifikasi Telegram ke tim terkait.
  - **Model Data Baru**: [`Calendar`](backend/src/models/calendar.model.js:3) menyimpan event kalender dengan field title, start/end date, color, ticket reference, dan created_by. [`Schedule`](backend/src/models/schedule.model.js:3) menyimpan rencana kerja dengan auto-increment ID, nama, tanggal, tim (object), daftar tiket, dan officer.
  - **API Endpoints**: Membuat REST API lengkap untuk Calendar (`GET/POST/PATCH/DELETE /api/v1/calendar`) dan Scheduler (`GET/POST/PATCH/DELETE /api/v1/scheduler` termasuk sub-endpoint untuk draft team, ticket list, dan Telegram message).
  - **Integrasi Tiket**: Modul scheduler terintegrasi dengan tiket yang ada — admin dapat melihat daftar tiket aktif yang belum selesai dan mengassign-nya ke tim dalam rencana kerja. Tiket yang sudah di-assign akan otomatis hilang dari daftar tersedia.
  - **Notifikasi Telegram**: Fitur mengirim pesan notifikasi ke tim melalui Telegram langsung dari halaman scheduler.
  - **Lazy Loading & Code Splitting**: Seluruh halaman baru menggunakan lazy loading melalui React Router untuk optimasi performa.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                         | Dampak Utama                                                                                                                          |
| ----- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| #172  | Perbaikan Hak Akses Privilege | Refactor sistem privilege tiket menjadi dinamis berdasarkan type, penambahan deskripsi privilege interaktif, dan fitur copy privilege |
| #174  | Modul Calendar & Scheduler    | Fitur baru kalender interaktif dan perencanaan kerja harian dengan manajemen tim dan assign tiket                                     |

### Kemampuan Baru Pengguna/Admin

- Admin dapat melihat **deskripsi lengkap** setiap privilege (253 hak akses) dalam Bahasa Indonesia/Inggris langsung di halaman manajemen privilege, sehingga lebih mudah memahami fungsi setiap hak akses
- Admin dapat **menyalin konfigurasi privilege** dari role lain (mode replace atau merge) untuk mempercepat pengaturan hak akses baru
- Admin dapat **mencari modul** pada tabel privilege menggunakan fitur pencarian/filter
- Admin dapat mengakses **halaman Kalender** untuk melihat dan mengelola seluruh event/kegiatan dalam tampilan kalender interaktif (bulanan, mingguan, harian, daftar)
- Admin dapat mengakses **modul Scheduler** untuk membuat rencana kerja harian, menyusun tim, dan mengassign tiket ke masing-masing tim
- Admin dapat **mengirim notifikasi Telegram** ke tim terkait langsung dari halaman scheduler
- Admin dapat melihat **tiket yang belum dijadwalkan** dan menyeretnya ke kalender untuk penjadwalan cepat

### Bug Fix / Solusi Masalah

- **Privilege tiket sebelumnya menggunakan satu key generik** (`ticket.changeStatus`) untuk semua type tiket (survey, installation, dismantle, dll), sehingga tidak bisa membedakan hak akses per type. Solusi: membuat middleware [`checkTicketPrivilegeByType()`](backend/src/middlewares/privilegeTicket.middleware.js:9) yang secara dinamis menentukan privilege berdasarkan type tiket.
- **Field `ticket_id` tidak ter-index** di database MongoDB, menyebabkan query pencarian tiket lambat pada volume data besar. Solusi: menambahkan index pada field `ticket_id` di model Ticket.

### Menu/Fitur Baru

- **Menu Activities > Kalender** (`/activities/calendar`) — Halaman kalender interaktif dengan FullCalendar
- **Menu Activities > Scheduler** (`/activities/scheduler`) — Halaman daftar rencana kerja harian
- **Menu Activities > Scheduler > Buat Baru** (`/activities/scheduler/create`) — Form pembuatan rencana kerja
- **Menu Activities > Scheduler > Detail** (`/activities/scheduler/detail/:id`) — Detail rencana kerja (read-only)
- **Menu Activities > Scheduler > Edit** (`/activities/scheduler/edit/:id`) — Form edit rencana kerja

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Modul Kalender

- **Penjelasan Fitur**: Modul Kalender menyediakan tampilan visual interaktif dari seluruh event dan kegiatan perusahaan. Menggunakan library FullCalendar, admin dapat beralih antara tampilan bulanan, mingguan, harian, dan daftar. Setiap event ditampilkan dengan warna berdasarkan prioritas tiket. Di sebelah kanan terdapat sidebar daftar tiket yang belum dijadwalkan, yang dapat diseret (drag-and-drop) ke kalender untuk penjadwalan cepat.
- **Langkah Penggunaan**:
  1. Buka menu **Activities > Kalender**
  2. Gunakan tombol navigasi (< >) atau dropdown bulan/tahun untuk berpindah periode
  3. Klik tombol **+** untuk membuat event baru — isi judul, tanggal, warna, dan deskripsi
  4. Klik event yang sudah ada untuk melihat detail, mengedit, atau menghapus
  5. Di sidebar kanan, lihat daftar tiket yang belum dijadwalkan — seret tiket ke tanggal yang diinginkan di kalender

### Modul Scheduler (Rencana Kerja Harian)

- **Penjelasan Fitur**: Modul Scheduler memungkinkan admin membuat rencana kerja harian dengan menyusun tim dan mengassign tiket ke masing-masing tim. Setiap rencana kerja memiliki nama, tanggal, susunan tim (dengan nama tim dan anggota), serta daftar tiket yang dikerjakan. Admin juga dapat mengirim notifikasi Telegram ke tim terkait.
- **Langkah Penggunaan**:
  1. Buka menu **Activities > Scheduler**
  2. Klik **Buat Baru** untuk membuat rencana kerja
  3. Isi nama rencana kerja dan pilih tanggal
  4. Klik **+ Tim** untuk menambahkan susunan tim — isi nama tim dan pilih anggota dari daftar admin
  5. Di bagian tiket, pilih tiket yang ingin diassign ke tim tertentu
  6. Klik **Simpan** untuk menyimpan rencana kerja
  7. Untuk melihat detail, klik baris rencana kerja di tabel → halaman detail (read-only) akan terbuka
  8. Untuk mengirim notifikasi Telegram, klik ikon Telegram pada halaman detail rencana kerja

---
