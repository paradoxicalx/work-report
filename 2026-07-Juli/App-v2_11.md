# 📝 Daily Work Report - Dedy Putra (2026-07-11)

---

## 📅 Laporan Harian - 11 Juli 2026

---

## 🌿 Branch: `issue-137` — Pembuatan Modul Baru: Dokumen PIR (Post Incident Report)

### 📌 Informasi Issue
- **Nomor Issue**: #137
- **Judul Issue**: Pembuatan Modul Baru Dokumen PIR (Post Incident Report)
- **Status Branch**: `Belum di-merge` *(dalam pengerjaan — WIP)*

### 📅 Rincian Commit

#### [[d6355c8]] - save #137 - Sabtu, 11 Jul 2026 19:50:51

- **Komponen yang Berubah** *(35 files changed, 745 insertions(+), 977 deletions(-))*:

  **Backend:**
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/ticket.model.js`
  - `backend/src/routes/ticket.route.js`
  - `backend/src/services/ticket.service.js`

  **Frontend — Network:**
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomEdge.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx`
  - `frontend/src/app/pages/network/fiberCable/index.jsx`

  **Frontend — Services:**
  - `frontend/src/app/pages/services/activation/components/SOReviewDrawer.jsx`
  - `frontend/src/app/pages/services/salesOrder/create.jsx`
  - `frontend/src/app/pages/services/salesOrder/edit.jsx`
  - `frontend/src/app/pages/services/vendorManagement/schema/ticketColumns.jsx`

  **Frontend — Modul Tiket (PIR & Peningkatan):**
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx`
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx`
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`
  - `frontend/src/app/pages/tickets/backbone/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/customer/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/dismantle/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/installation/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/other/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx`
  - `frontend/src/app/pages/tickets/partner/create.jsx`
  - `frontend/src/app/pages/tickets/partner/edit.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/createSchema.js`
  - `frontend/src/app/pages/tickets/payment/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/survey/schema/columns.jsx`

  **Frontend — Komponen Shared & Konfigurasi:**
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/constants/fiberColors.constant.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/styles/index.css`

- **Deskripsi Perubahan & Fungsi**:

  - **Backend — Ticket Model (PIR Fields)**: Penambahan field-field baru pada schema Mongoose `ticket.model.js` untuk mendukung data PIR, yaitu:
    - `time_of_incident` — Waktu kejadian insiden
    - `time_of_report` — Waktu pelaporan insiden
    - `pending` (Boolean, default: `false`) — Status suspended/on-hold tiket

  - **Backend — Ticket Service**: Pembaruan service untuk menyertakan field-field PIR baru (`time_of_incident`, `time_of_report`, `pending`) dalam whitelist field yang dapat diperbarui.

  - **Backend — Ticket Controller (Suspend/Resume)**: Penambahan logika controller untuk fitur **Suspend & Resume tiket**:
    - Jika `pendingAction === 'suspend'` → tiket diubah ke status `pending: true`
    - Jika `pendingAction === 'resume'` → tiket diubah ke status `pending: false`
    - Auto-resume: jika tiket sedang `pending` dan ada pesan masuk baru, status otomatis kembali aktif
    - Response pesan sistem otomatis dikirim saat perubahan status terjadi (`sysHold`, `sysResume`, `sysAutoResume`)

  - **Backend — i18n (EN & ID)**: Penambahan key terjemahan baru untuk pesan sistem tiket:
    - `sysHold` — Pesan saat tiket di-suspend
    - `sysResume` — Pesan saat tiket di-resume
    - `sysAutoResume` — Pesan saat tiket otomatis di-resume karena pesan baru masuk

  - **Frontend — GeneralInformation.jsx (PIR Display)**: Penambahan tampilan field PIR pada tab Informasi Umum tiket. Field `time_of_incident` dan `time_of_report` ditampilkan dalam format `DD MMMM YYYY, HH:mm` dan dikecualikan jika nilainya kosong.

  - **Frontend — TicketDetailDrawer.jsx (PIR Fields)**: Penambahan rendering kondisional untuk field `time_of_incident` dan `time_of_report` dalam drawer detail tiket. Field datetime ini diformat dengan tepat dan hanya ditampilkan jika memiliki nilai.

  - **Frontend — MessageUpdate.jsx (Suspend/Resume UI)**: Penambahan fitur antarmuka Suspend & Resume tiket pada panel pembaruan pesan:
    - Banner peringatan oranye ditampilkan saat tiket sedang dalam status **Pending (Suspended)**
    - Tombol **Suspend Ticket** / **Resume Ticket** untuk admin/operator
    - Fungsi `handleTogglePending()` yang mengirim `pendingAction` ke backend
    - Toast notifikasi berbeda untuk aksi suspend, resume, dan pengiriman pesan biasa
    - Validasi: tombol disabled jika area pesan kosong

  - **Frontend — Partner Ticket Create & Edit (PIR Form)**: Penambahan field input PIR pada form pembuatan dan pengeditan tiket partner:
    - `InputDatePicker` untuk `time_of_incident` (Waktu Insiden) dengan `enableTime: true`
    - `InputDatePicker` untuk `time_of_report` (Waktu Pelaporan) dengan `enableTime: true`
    - Data dikirim dalam format ISO 8601 ke backend

  - **Frontend — rows.jsx (CompletedAtCell)**: Penambahan komponen cell tabel baru `CompletedAtCell` yang menampilkan tanggal penyelesaian tiket dengan fitur khusus: jika tiket berstatus `pending` dan belum selesai, cell menampilkan badge **"Hold"** (warning) alih-alih tanggal.

  - **Frontend — Kolom Tabel Tiket (Multi-jenis)**: Pembaruan schema kolom tabel untuk berbagai jenis laporan tiket (backbone, customer, dismantle, installation, other, partner, payment, survey) agar menggunakan komponen `CompletedAtCell` yang baru dan mendukung tampilan status PIR/pending.

  - **Frontend — i18n (EN & ID)**: Penambahan key terjemahan baru:
    - `ticket.detail.suspendTicket` / `resumeTicket`
    - `ticket.detail.pendingBanner` — Label banner suspended
    - `ticket.detail.suspendSuccess` / `resumeSuccess`
    - `ticket.detail.holdBadge` / `unholdBadge`
    - `ticket.fields.time_of_incident` — Label "Waktu Insiden"
    - `ticket.status.pending` — Label status "Pending Approval"

---

## ✅ Branch: `issue-136` — Telegram Mini App: Halaman Poin, Jam Kerja & Perbaikan Fiber Cable

### 📌 Informasi Issue
- **Nomor Issue**: #136
- **Judul Issue**: Pengembangan fitur Telegram Mini App (halaman poin reward, jam kerja) dan perbaikan backend fiber cable/attendance
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [[b3cd616]] - resolve #136 - Sabtu, 11 Jul 2026 15:10:53 *(commit dari komputer lain — Dedy Putra)*

- **Komponen yang Berubah**:
  - `backend/scripts/migrate-splices-node.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/fiberCable.controller.js`
  - `backend/src/controllers/locationPoint.controller.js`
  - `backend/src/routes/attendance.route.js`
  - `backend/src/services/attendance.service.js`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/fiberTrace.service.js`
  - `telegram-apps/src/pages/MyPoint.jsx`
  - `telegram-apps/src/pages/MyWorkHours.jsx` [NEW]
  - `telegram-apps/src/pages/Profile.jsx`
  - `telegram-apps/src/pages/warehouse/stock.jsx`
  - `telegram-apps/src/pages/warehouse/stockDetail.jsx`
  - `telegram-apps/src/routes/index.jsx`

- **Deskripsi Perubahan & Fungsi**:
  - **Telegram Mini App — Halaman MyWorkHours** *(BARU, +237 baris)*: Halaman baru yang menampilkan rekap jam kerja karyawan langsung dari Telegram Mini App. Menampilkan total jam kerja harian, jam masuk/keluar, dan status hadir/absen.
  - **Telegram Mini App — Halaman MyPoint**: Pengembangan signifikan (+186 baris) pada halaman Poin Reward karyawan. Kini menampilkan total poin, riwayat transaksi poin, dan status reward secara real-time.
  - **Telegram Mini App — Warehouse Stock**: Perbaikan tampilan dan logika halaman stok gudang (+70 perubahan), termasuk detail stok dengan UI yang lebih baik.
  - **Backend — Attendance Service & Controller**: Penambahan endpoint dan logika service baru untuk mendukung pengambilan data jam kerja karyawan yang dipakai oleh Telegram Mini App (halaman MyWorkHours).
  - **Backend — FiberCable & FiberTrace Service**: Refactoring dan perbaikan logika layanan fiber cable (+46 perubahan) dan fiber trace (+16 perubahan) untuk konsistensi data dan performa.
  - **Backend — Migrate Splices Node Script**: Perbaikan script migrasi data splice node pada jaringan fiber (+48 perubahan).
  - **Backend — Privilege Config**: Penambahan/penyesuaian hak akses (privilege) untuk endpoint-endpoint baru.
  - **Backend — Routes Attendance**: Penambahan route baru untuk endpoint attendance yang melayani kebutuhan data Telegram Mini App.

#### [[0e53c8d]] - resolve #136 - Sabtu, 11 Jul 2026 15:22:43 *(squash & push ke master — Dedy S.N Putra)*

> Commit ini merupakan squash final dari branch `issue-136` yang di-push ke `origin/master`. Perubahan identik dengan commit `b3cd616` di atas, sesuai aturan **Single Commit Policy**.

---

## 🌿 Branch: `issue-115` — Pengaturan Aplikasi: Halaman System & Application Settings

### 📌 Informasi Issue
- **Nomor Issue**: #115
- **Judul Issue**: Implementasi halaman pengaturan aplikasi (System Settings & Application Settings)
- **Status Branch**: `Belum di-merge` *(dalam pengerjaan — WIP, dikerjakan dari komputer lain)*

### 📅 Rincian Commit

#### [[b409d5b]] - save #115 - Sabtu, 11 Jul 2026 17:22:56 *(Dedy S.N Putra)*

- **Komponen yang Berubah** *(17 files changed, 4.681 insertions(+), 5.909 deletions(-))*:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/options.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/options.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/pages/settings/Layout.jsx`
  - `frontend/src/app/pages/settings/sections/Appearance.jsx`
  - `frontend/src/app/pages/settings/sections/Application.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/SettingsContext.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/System.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Frontend — Halaman Application Settings** *(BARU, +513 baris)*: Implementasi halaman pengaturan aplikasi yang komprehensif, mencakup konfigurasi batas operasional, notifikasi, integrasi, dan preferensi sistem.
  - **Frontend — Halaman System Settings** *(BARU, +316 baris)*: Halaman pengaturan sistem level admin untuk parameter teknis (timezone, format tanggal, bahasa default, dll.).
  - **Frontend — SettingsContext** *(BARU, +78 baris)*: Shared React Context baru untuk manajemen state pengaturan antar semua seksi halaman Settings.
  - **Frontend — Navigation Settings**: Pembaruan navigasi dengan submenu baru (System & Application) di sidebar Settings.
  - **Frontend — FormInput Component**: Peningkatan komponen (+35 baris) untuk mendukung tipe input baru di halaman Settings (switch toggle, range slider, dll.).
  - **Backend — Options Controller** *(BARU, +85 baris)*: Controller baru untuk endpoint CRUD pengaturan aplikasi.
  - **Backend — Options Routes** *(BARU, +76 baris)*: Route baru `/api/options` dengan endpoint GET, PUT, dan manajemen hak akses per role.
  - **Backend — Option Service**: Pembaruan service layer (+58 baris) untuk logika bisnis penyimpanan dan pengambilan konfigurasi dari database.
  - **Internasionalisasi (i18n)**: Pembaruan masif file terjemahan backend dan frontend untuk semua teks UI halaman Settings baru.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Status | Dampak Utama |
|-------|-------|--------|--------------|
| #136 | Telegram Mini App: Poin & Jam Kerja | ✅ Merged | Karyawan dapat melihat poin reward dan rekap jam kerja langsung dari Telegram |
| #137 | Modul PIR (Post Incident Report) | 🚧 WIP | Admin/Operator dapat membuat, mencatat, dan mengelola laporan pasca-insiden pada tiket; tiket dapat di-suspend dan di-resume |
| #115 | System & Application Settings | 🚧 WIP | Admin dapat mengonfigurasi parameter aplikasi dan sistem secara terpusat melalui UI |

### Kemampuan Baru Pengguna/Admin

- **Karyawan via Telegram Mini App**: Dapat mengakses halaman **Jam Kerja** (`/my-work-hours`) untuk melihat rekap jam kerja harian langsung dari Telegram.
- **Karyawan via Telegram Mini App**: Dapat mengakses halaman **Poin Reward** (`/my-point`) untuk memantau saldo dan riwayat poin reward.
- **Admin/Operator Tiket**: Dapat **men-suspend (hold)** tiket yang sedang dalam kondisi menunggu tindak lanjut, kemudian **me-resume** kembali saat siap. Tiket juga dapat auto-resume saat pesan baru masuk.
- **Admin/Operator Tiket**: Dapat mencatat **Waktu Insiden** (`time_of_incident`) dan **Waktu Pelaporan** (`time_of_report`) pada tiket partner sebagai bagian dari data PIR (Post Incident Report).
- **Semua pengguna**: Tabel daftar tiket kini menampilkan badge **"Hold"** pada tiket yang sedang dalam status suspended.

### Bug Fix / Solusi Masalah

- Perbaikan script migrasi data **splice node** pada jaringan fiber yang sebelumnya memiliki logika tidak konsisten.
- Refactoring besar **FiberCable & FiberTrace Service** untuk konsistensi data peta topologi jaringan.
- Cleanup masif pada `TicketDetailDrawer.jsx` (-205 baris) dan `ticket.controller.js` (-145 baris) untuk menghilangkan kode redundan dan meningkatkan maintainability.

### Menu/Fitur Baru

- **Halaman MyWorkHours** di Telegram Mini App — Rekap jam kerja karyawan.
- **Field PIR pada Form Tiket Partner** — Input `Waktu Insiden` dan `Waktu Pelaporan` dengan date-time picker.
- **Fitur Suspend/Resume Tiket** — Tombol hold/unhold pada panel pesan tiket dengan banner status.
- **CompletedAtCell** — Komponen cell tabel baru dengan dukungan tampilan badge "Hold" untuk tiket suspended.
- **Settings → System** & **Settings → Application** — Dua seksi pengaturan baru (dalam pengerjaan).
- **API Endpoint `/api/options`** — Endpoint backend baru untuk manajemen konfigurasi aplikasi.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 🎯 Fitur Utama Hari Ini: Dokumen PIR & Fitur Suspend/Resume Tiket (Issue #137)

**Penjelasan Fitur**:
Modul PIR (Post Incident Report) ditambahkan ke sistem tiket untuk mendukung pencatatan insiden secara terstruktur. Fitur ini mencakup dua aspek utama: (1) **Field data PIR** berupa waktu kejadian dan waktu pelaporan insiden yang dapat diisi saat membuat/mengedit tiket partner, dan (2) **Fitur Suspend/Resume** yang memungkinkan operator men-hold tiket saat menunggu respons/tindak lanjut, lengkap dengan notifikasi sistem otomatis dan tampilan status di seluruh tabel tiket.

**Langkah Penggunaan (Tutorial)**:

**Untuk Mengisi Data PIR pada Tiket Partner:**
1. Login ke Frontend sebagai Admin atau Operator
2. Navigasi ke menu **Tiket → Partner**
3. Klik **Buat Tiket** atau edit tiket yang sudah ada
4. Pada form, isi field **Waktu Insiden** (pilih tanggal & jam kapan insiden terjadi)
5. Isi field **Waktu Pelaporan** (pilih tanggal & jam kapan insiden dilaporkan)
6. Simpan tiket — data PIR tersimpan dan tampil di tab Informasi Umum tiket

**Untuk Men-suspend (Hold) Tiket:**
1. Buka tiket yang ingin di-hold melalui drawer detail tiket
2. Pada panel **Pembaruan Pesan**, tuliskan catatan alasan tiket di-hold
3. Klik tombol **Suspend Ticket** (ikon pause)
4. Banner oranye bertulisan "Tiket ini sedang dalam status Suspended" akan muncul
5. Di tabel tiket, kolom tanggal selesai akan berubah menjadi badge **"Hold"**

**Untuk Me-resume Tiket yang Di-suspend:**
1. Buka tiket yang berstatus Hold
2. Tuliskan pesan update pada panel Pembaruan Pesan
3. Klik **Resume Ticket** — tiket kembali aktif; ATAU
4. Kirim pesan biasa → sistem otomatis me-resume tiket dan mencatat di riwayat pesan
