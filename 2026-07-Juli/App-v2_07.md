# 📝 Daily Work Report - Dedy Putra (2026-07-07)

---

## 📅 Laporan Harian - 7 Juli 2026

> Laporan ini mencakup semua branch aktif yang belum di-merge ke `master`.

---

## 🌿 Branch: `issue-132` — Manajemen Waktu & Laporan Tiket

### 📌 Informasi Issue
- **Nomor Issue**: #132
- **Judul Issue**: Manajemen Waktu & Laporan Tiket (Start Time, End Time, Completed At, PartnerReport)
- **Status Branch**: `Belum di-merge` — Telah di-push ke `origin/issue-132`

### 📅 Rincian Commit

#### [`071ac3a`] - resolve #132 - Selasa, 7 Juli 2026 pukul 23:52

- **Komponen yang Berubah (Backend)**:
  - `backend/src/controllers/ticket.controller.js` — Tambahan logika auto-fill `start_time` (dari `created_at`) dan `end_time` (waktu saat ditutup) jika kosong saat tiket di-close. Tambahan endpoint CRUD untuk manajemen waktu tiket.
  - `backend/src/routes/ticket.route.js` — Penambahan route baru untuk update `start_time` dan `end_time` tiket.
  - `backend/src/models/ticket.model.js` — Penambahan field `start_time` dan `end_time` pada skema Mongoose.
  - `backend/src/services/ticket.service.js` — Update service layer untuk mendukung field waktu baru.
  - `backend/src/utils/data-table.js` — Perbaikan utilitas datatable.
  - `backend/src/locales/en/translation.json` — Tambahan key i18n untuk fitur waktu tiket.
  - `backend/src/locales/id/translation.json` — Tambahan key i18n Bahasa Indonesia.

- **Komponen yang Berubah (Frontend)**:
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx` — Refactoring drawer detail tiket, penambahan input waktu mulai & selesai yang bisa diedit langsung.
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx` — Menampilkan kembali Waktu Mulai dan Waktu Selesai pada panel informasi umum.
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx` — Perbaikan bug form pesan update tidak muncul; menambahkan mapping hak akses untuk tipe tiket `dismantle`, `noc`, dan `other`.
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx` — Refactoring tampilan laporan partner: menambahkan grid detail waktu (Waktu Laporan, Waktu Selesai, Waktu Mulai Perbaikan, Waktu Selesai Perbaikan, Durasi Tiket, Durasi Tunggu, MTTR).
  - `frontend/src/app/pages/tickets/partner/schema/columns.jsx` — Penambahan kolom `completed_at`, `created_at`, dan `work_time` (menggabungkan start/end/durasi); pergeseran urutan kolom agar lebih intuitif.
  - `frontend/src/app/pages/tickets/backbone/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/customer/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/dismantle/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/installation/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/other/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/payment/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/app/pages/tickets/survey/schema/columns.jsx` — Penambahan & reposisi kolom `created_at` dan `completed_at`.
  - `frontend/src/components/shared/table/Table.jsx` — Perbaikan minor komponen tabel.
  - `frontend/src/i18n/locales/en/translations.json` — Penambahan 35 key i18n baru (label waktu, satuan hari/jam/menit, label laporan).
  - `frontend/src/i18n/locales/id/translations.json` — Penambahan 33 key i18n Bahasa Indonesia.

- **Deskripsi Perubahan & Fungsi**:
  - Sistem tiket kini mendukung pencatatan waktu kerja yang lengkap: `start_time` (waktu mulai perbaikan), `end_time` (waktu selesai perbaikan), dan `completed_at` (waktu tiket ditutup/closed).
  - Saat tiket ditutup, jika `start_time` dan `end_time` belum diisi, sistem secara otomatis mengisi `start_time` dengan `created_at` dan `end_time` dengan waktu saat ini.
  - Input waktu dapat diubah langsung dari Drawer Detail Tiket dengan validasi (waktu mulai tidak boleh lebih besar dari waktu selesai).
  - Setiap perubahan waktu otomatis mencatat pesan pada riwayat tiket dengan info detail jenis waktu yang diubah.
  - PartnerReport kini menampilkan panel lengkap: Durasi Tiket (created_at → closed), Durasi Tunggu (end_time → closed), dan MTTR (start_time → end_time).
  - DataTables di semua jenis tiket kini memiliki kolom `Waktu Dibuat` dan `Waktu Selesai` yang bisa diaktifkan dari pengaturan visibilitas kolom, diposisikan tepat setelah kolom Nama/Judul.
  - Tiket Partner juga memiliki kolom gabungan `MTTR/Durasi Pekerjaan` yang menampilkan start time, end time, dan durasi dalam satu sel dengan ikon kompak.

---

## 🌿 Branch: `issue-129` — Manajemen Kabel Serat Optik & Topologi Jaringan

### 📌 Informasi Issue
- **Nomor Issue**: #129
- **Judul Issue**: Fiber Cable — Splice Node Management & Topology Enhancement
- **Status Branch**: `Belum di-merge` — Telah di-push ke `origin/issue-129`

### 📅 Rincian Commit

#### [`a014955`] - resolve #129 - Selasa, 7 Juli 2026 pukul 23:54

- **Komponen yang Berubah (Backend)**:
  - `backend/scripts/migrate-splices-node.js` [NEW] — Script migrasi data untuk node splice fiber optic.
  - `backend/src/controllers/fiberCable.controller.js` — Update controller manajemen fiber cable.
  - `backend/src/models/fiberCable.model.js` — Penambahan field baru pada model Mongoose fiber cable.
  - `backend/src/services/fiberCable.service.js` — Logika bisnis baru untuk pengelolaan fiber cable dan splice.
  - `backend/src/services/fiberTrace.service.js` — Peningkatan service pelacakan jalur fiber.

- **Komponen yang Berubah (Frontend)**:
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomNode.jsx` — Redesain tampilan node topologi kabel serat optik dengan lebih banyak informasi dan interaksi.
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomEdge.jsx` — Perbaikan tampilan edge/koneksi antar node.
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyModal.jsx` — Pembaruan modal topologi untuk fitur splice.
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx` — Penyederhanaan komponen peta fiber.
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx` — Tambahan informasi node pada drawer detail.
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx` — Penambahan tools baru pada sidebar topologi.
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx` — Perbaikan komponen tray splice kabel.
  - `frontend/src/i18n/locales/en/translations.json` — Tambahan key i18n untuk fitur fiber cable baru.
  - `frontend/src/i18n/locales/id/translations.json` — Tambahan key i18n Bahasa Indonesia.

- **Deskripsi Perubahan & Fungsi**:
  - Fitur visualisasi topologi jaringan fiber optik ditingkatkan dengan tampilan custom node yang lebih informatif dan interaktif.
  - Penambahan manajemen splice (sambungan kabel serat) yang dapat dikelola secara visual langsung dari tampilan topologi.
  - Script migrasi data tersedia untuk membantu perpindahan data splice dari format lama ke format baru.
  - Pelacakan jalur fiber (`fiberTrace`) ditingkatkan untuk mendukung konfigurasi splice yang lebih kompleks.

---

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Status | Dampak Utama |
|-------|-------|--------|--------------|
| #132  | Manajemen Waktu Tiket | Belum di-merge | Pencatatan waktu kerja lengkap (mulai, selesai, durasi) pada semua tiket |
| #129  | Fiber Cable Topology | Belum di-merge | Visualisasi dan manajemen splice node serat optik yang lebih lengkap |

### Kemampuan Baru Pengguna/Admin
- **Admin Tiket** kini dapat melihat dan mengubah waktu mulai & selesai perbaikan langsung dari drawer detail tiket.
- **Auto-fill Waktu**: Saat tiket ditutup tanpa waktu kerja yang diisi, sistem otomatis mengisi berdasarkan waktu pembuatan dan penutupan tiket, sehingga data laporan selalu lengkap.
- **Laporan Partner** kini menampilkan metrik lengkap: Durasi Tiket, Durasi Tunggu, dan MTTR/Durasi Pekerjaan.
- **Admin Jaringan** kini dapat mengelola sambungan (splice) kabel serat optik secara visual dari tampilan topologi jaringan.

### Bug Fix / Solusi Masalah
- **Form Tambah Pesan Tidak Muncul**: Perbaikan pada `MessageUpdate.jsx` — form update pesan tiket sebelumnya tidak muncul meskipun user/admin sudah memiliki hak akses. Penyebabnya adalah mapping privilege yang tidak lengkap untuk tipe tiket `dismantle`, `noc`, dan `other`.
- **Validasi Waktu**: Penambahan validasi agar `start_time` tidak bisa lebih besar dari `end_time` dan sebaliknya, mencegah input data waktu yang tidak valid.

### Fitur & Tampilan Baru
- Kolom **Waktu Dibuat** dan **Waktu Selesai** pada semua DataTables tiket (8 jenis tiket), diposisikan tepat setelah kolom Nama/Judul tiket.
- Kolom **MTTR / Durasi Pekerjaan** khusus untuk DataTables Tiket Partner dengan tampilan ikon kompak (play, check, clock) yang hemat ruang.
- Panel detail waktu dan durasi yang lengkap di halaman Laporan Partner.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Fitur: Manajemen Waktu Perbaikan Tiket (#132)

**Penjelasan Fitur:**
Sistem kini melacak empat titik waktu penting pada setiap tiket:
1. **Waktu Laporan** (`created_at`): Waktu tiket pertama kali dibuat/dilaporkan.
2. **Waktu Mulai Perbaikan** (`start_time`): Waktu teknisi mulai mengerjakan tiket.
3. **Waktu Selesai Perbaikan** (`end_time`): Waktu teknisi selesai mengerjakan.
4. **Waktu Tiket Ditutup** (tanggal laporan): Waktu tiket resmi ditutup oleh sistem.

Dari keempat data ini, sistem menghitung otomatis:
- **MTTR** = `end_time` - `start_time` (Durasi Pekerjaan)
- **Durasi Tunggu** = Waktu closed - `end_time`
- **Durasi Tiket Total** = Waktu closed - `created_at`

**Langkah Penggunaan:**
1. Buka halaman daftar tiket (misal: Tiket Partner) → klik tiket yang ingin dilihat/diubah.
2. Di Drawer Detail Tiket, temukan bagian **Waktu Pekerjaan**.
3. Isi atau ubah **Waktu Mulai** dan **Waktu Selesai** sesuai kondisi lapangan.
4. Klik **Simpan** — sistem akan mencatat perubahan di riwayat pesan tiket secara otomatis beserta informasi waktu yang diubah.
5. Untuk melihat laporan lengkap (khusus Tiket Partner): buka tab **Laporan** di dalam drawer untuk melihat semua metrik waktu dan durasi.
