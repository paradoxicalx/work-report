# 📝 Daily Work Report - Dedy S.N Putra (27 September 2026)

---

## 📅 Laporan Harian - 27 September 2026

---

## 🌿 Branch: `issue-329` — Issue #329: Penyempurnaan Akuisisi Lokasi GPS & Tampilan Jam Kerja Telegram Mini App

### 📌 Informasi Issue

- **Nomor Issue**: #329
- **Judul Issue**: Penyempurnaan Akuisisi Lokasi GPS (Robust Location Strategy) & Tampilan Jam Kerja Telegram Mini App
- **Status Branch**: `Belum di-merge` (Branch aktif `issue-329`, commit `37ec72b7`)

### 📅 Rincian Commit

#### [37ec72b7] - resolve #329 - Minggu, 27 September 2026, 23:50:43 WIB

- **Komponen yang Berubah**:
  - `telegram-apps/src/hooks/useTelegramLocation.js`
  - `telegram-apps/src/pages/MyWorkHours.jsx`
  - `telegram-apps/src/pages/attendance/CheckIn.jsx`
  - `telegram-apps/src/pages/attendance/CheckOut.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Overhaul Strategi Akuisisi Lokasi GPS (`useTelegramLocation.js`)**: Menulis ulang total hook lokasi GPS menjadi strategi akuisisi bertingkat (_two-tier fallback_) yang jauh lebih tangguh untuk lingkungan Telegram Mini App lintas platform (Android, iOS, Desktop):
    - **Telegram LocationManager + Safety Timeout**: Jika Bot API ≥ 8.0, gunakan `WebApp.LocationManager` native Telegram dengan batas waktu pengaman 4 detik. Jika native tidak merespons (bug IPC Android / client lama), otomatis fallback ke Geolocation API browser.
    - **Two-Tier Browser Fallback**: Tahap 1 mencoba GPS presisi tinggi (`enableHighAccuracy: true`, timeout 7 detik). Jika gagal atau posisi tidak tersedia (misal indoor/gedung), Tahap 2 mencoba ulang dengan akurasi rendah/seluler/Wi-Fi (`enableHighAccuracy: false`, timeout 7 detik).
    - **Konvergensi Akurasi**: Jika posisi didapat namun akurasinya kasar (> 50m), menjalankan `watchPosition` singkat untuk memperhalus koordinat bila GPS tersedia.
    - **Fungsi Retry Manual**: Menyediakan `retry()` untuk memicu ulang akuisisi lokasi secara manual dari UI.
    - **Session Isolation**: Menggunakan `activeSessionRef` untuk memastikan callback dari percobaan lama tidak menimpa state saat retry.
    - **Return Value Diperluas**: Mengembalikan `coords`, `accuracy`, `isReady`, `isLoading`, `error`, dan `retry`.
  - **Penyempurnaan Tampilan Jam Kerja (`MyWorkHours.jsx`)**: Mengubah format tampilan total jam kerja dari satuan jam bulat menjadi format jam + menit yang lebih presisi:
    - Total akumulasi jam kerja kini menampilkan `{jam} Jam {menit} Menit` (sebelumnya hanya jam bulat).
    - Jam kerja bulan ini juga menampilkan `{jam} Jam {menit} Menit`.
    - Layout header responsif dengan `flex-wrap` agar tidak pecah pada layar kecil.
  - **Penyempurnaan Halaman Check-In (`CheckIn.jsx`)**:
    - Mengintegrasikan nilai baru dari hook (`accuracy`, `isLoading`, `retry`) ke UI.
    - Menambahkan tombol **Perbarui Lokasi** (`RefreshCw` icon) agar pengguna dapat memicu ulang akuisisi GPS jika lokasi belum terkunci.
    - Memperbaiki pesan error lokasi menjadi lebih deskriptif: _"Lokasi belum terkunci. Pastikan GPS aktif lalu tekan tombol Perbarui Lokasi."_
    - Menghilangkan fallback lokasi hardcoded (Monas) sebelumnya — lokasi `null` sampai GPS benar-benar terkunci.
    - Menambahkan haptic feedback (`impactOccurred('light')`) saat tombol retry ditekan.
  - **Penyempurnaan Halaman Check-Out (`CheckOut.jsx`)**: Menerapkan perubahan identik dengan Check-In — tombol retry, pesan error deskriptif, eliminasi fallback lokasi hardcoded, dan haptic feedback.

---

## 🌿 Branch: `master` — Issue #337: Pengajuan Izin & Cuti per Pengajuan dengan Persetujuan per Hari & Developer Recovery Tool

### 📌 Informasi Issue

- **Nomor Issue**: #337
- **Judul Issue**: Pengajuan Izin & Cuti per Pengajuan dengan Persetujuan per Hari, Migrasi Data Induk & Developer Recovery Tool
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Minggu, 27 September 2026, 21:25:20 WIB via commit `1ee1128a`)

### 📅 Rincian Commit

#### [1ee1128a] - resolve #337 - Minggu, 27 September 2026, 21:25:20 WIB

_(Merge commit menggabungkan branch issue-329 / commit `3a3114d1`)_

- **Komponen yang Berubah**:
  - `backend/scripts/backfill-attendance-requests.js`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-337.json` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/attendanceAbsence.model.js`
  - `backend/src/models/attendanceAbsenceRequest.model.js` [NEW]
  - `backend/src/models/attendancePermit.model.js`
  - `backend/src/models/attendancePermitRequest.model.js` [NEW]
  - `backend/src/routes/attendance.route.js`
  - `backend/src/services/attendance.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/attendanceRecovery.test.js`
  - `backend/test/integration/attendanceRequestList.scope.test.js`
  - `backend/test/integration/attendanceRequestReview.test.js` [NEW]
  - `backend/test/integration/attendanceSelfList.test.js`
  - `backend/test/unit/attendancePendingCount.service.test.js`
  - `frontend/src/app/pages/activities/attendance/components/AttendanceRequestDetailModal.jsx`
  - `frontend/src/app/pages/activities/paidLeave/index.jsx`
  - `frontend/src/app/pages/activities/paidLeave/schema/columns.jsx`
  - `frontend/src/app/pages/activities/permission/index.jsx`
  - `frontend/src/app/pages/activities/permission/schema/columns.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryDryRunModal.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/app/pages/users/components/UserAttendanceTabs.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Model Pengajuan Izin & Cuti Baru (`attendanceAbsenceRequest.model.js`, `attendancePermitRequest.model.js`)**: Membuat model induk pengajuan izin (`AttendanceAbsenceRequest`) dan cuti (`AttendancePermitRequest`) sebagai dokumen聚合 yang mengelompokkan hari-hari individual izin/cuti ke dalam satu pengajuan tunggal dengan field `total_days`, `accepted_days`, `rejected_days`, dan status dinamis (`pending`, `accepted`, `rejected`, `partial`).
  - **Skrip Backfill Migrasi Data Induk (`scripts/backfill-attendance-requests.js`)**: Mengembangkan skrip migrasi CLI untuk memigrasikan data izin/cuti harian warisan (_legacy_) yang belum memiliki referensi ke dokumen induk pengajuan, dengan mekanisme pengelompokan cerdas berdasarkan identitas pegawai, kesamaan judul, dan kedekatan rentang tanggal.
  - **Layanan Pemulihan & Migrasi (`services/recovery.service.js`)**: Membangun mesin pemulihan data dengan mode simulasi (_dry-run_) dan eksekusi riil untuk menggabungkan hari-hari izin/cuti yatim menjadi dokumen induk pengajuan terpadu, dilengkapi pengaman konfirmasi kata sandi `RECOVERY`.
  - **Controller & Routing Attendance Diperluas (`controllers/attendance.controller.js`, `routes/attendance.route.js`)**: Menambahkan endpoint recovery database (`POST /api/v1/attendance/recovery-db`) yang dilindungi middleware `protectedDeveloper`, serta memperbarui endpoint pengajuan izin/cuti untuk mendukung arsitektur induk-harian baru.
  - **Penyempurnaan UI Modal Detail Pengajuan (`AttendanceRequestDetailModal.jsx`)**: Menulis ulang modal detail pengajuan izin/cuti untuk menampilkan daftar hari per pengajuan dengan status persetujuan individual per hari, memungkinkan persetujuan/penolakan per hari.
  - **Developer Recovery Tool UI (`recoveryRoutes.config.js`, `RecoveryDryRunModal.jsx`)**: Menambahkan kategori pemulihan baru 'Kehadiran & Absensi' pada panel Recovery Tools developer dengan mode simulasi dan proteksi kata sandi konfirmasi.
  - **Pengujian Komprehensif**: Menambahkan uji integrasi untuk review pengajuan izin (`attendanceRequestReview.test.js`), pemulihan data (`attendanceRecovery.test.js`), serta pembaruan uji scope yang ada agar kompatibel dengan arsitektur baru.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                                     | Dampak Utama                                                                                                                                                              |
| ----- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #329  | Penyempurnaan Akuisisi Lokasi GPS & Tampilan Jam Kerja Telegram Mini App                  | Strategi GPS bertingkat yang tangguh untuk Telegram Mini App lintas platform, tampilan jam kerja presisi jam+menit, tombol retry lokasi manual.                           |
| #337  | Pengajuan Izin & Cuti per Pengajuan dengan Persetujuan per Hari & Developer Recovery Tool | Migrasi database izin/cuti harian lama ke dokumen induk pengajuan baru via Developer Recovery Tool dengan mode simulasi (_dry-run_) dan pengamanan konfirmasi kata kunci. |

### Kemampuan Baru Pengguna/Admin

- **Akuisisi Lokasi GPS yang Lebih Andal di Telegram Mini App**: Pengguna kini mendapatkan lokasi GPS yang lebih akurat dan konsisten lintas platform (Android, iOS, Desktop) berkat strategi akuisisi bertingkat yang secara otomatis beralih antara Telegram LocationManager, GPS presisi tinggi, dan fallback jaringan seluler/Wi-Fi.
- **Tombol Perbarui Lokasi Manual**: Pengguna dapat memicu ulang akuisisi GPS kapan saja dengan menekan tombol Perbarui Lokasi pada halaman Check-In/Check-Out jika lokasi belum terkunci atau ingin memperbarui posisi.
- **Tampilan Jam Kerja Lebih Presisi**: Halaman Jam Kerja Saya kini menampilkan total akumulasi dalam format jam + menit (misal: "120 Jam 35 Menit"), bukan hanya jam bulat.
- **Persetujuan Izin & Cuti per Hari**: Admin dapat menyetujui atau menolak izin/cuti secara individual per hari dalam satu pengajuan, memberikan fleksibilitas yang lebih besar dalam pengelolaan ketidakhadiran.
- **Pemulihan Data Pengajuan Izin & Cuti (Developer Tools)**: Tim developer dapat menjalankan simulasi _dry-run_ dan eksekusi migrasi penggabungan hari-hari izin/cuti yatim menjadi dokumen induk pengajuan terpadu melalui panel Recovery Tools dengan pengaman kata sandi.

### Bug Fix / Solusi Masalah

- **Pencegahan Prompt Izin Lokasi Berulang di Telegram Desktop**: Strategi akuisisi baru dengan `WebApp.LocationManager` native Telegram dan safety timeout menghilangkan masalah izin lokasi yang muncul berulang kali pada Telegram Desktop karena izin tidak tersimpan lintas kunjungan halaman.
- **Eliminasi Fallback Lokasi Hardcoded yang Menyesatkan**: Menghilangkan fallback lokasi default (Monas/Jakarta) yang sebelumnya membuat form Check-In/Check-Out tampak siap meskipun GPS sebenarnya belum terkunci — kini form secara eksplisit menunggu lokasi aktual sebelum mengizinkan submit.
- **Akurasi GPS Kasar di Indoor**: Strategi konvergensi akurasi (_watchPosition_ singkat) membantu memperhalus koordinat GPS saat posisi awal memiliki akurasi kasar (> 50m), misal di dalam gedung.

### Menu/Fitur Baru

- **Tombol Perbarui Lokasi di Halaman Check-In/Check-Out (`/check-in`, `/check-out`)**: Tombol `RefreshCw` untuk memicu ulang akuisisi GPS secara manual.
- **Panel Pemulihan Data Pengajuan Izin & Cuti (`/settings/developer/recovery-tools`)**: Opsi pemulihan data absensi baru pada kelompok 'Kehadiran & Absensi' untuk migrasi pengajuan izin dan cuti.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penggunaan Tombol Perbarui Lokasi pada Check-In/Check-Out

- **Penjelasan Fitur**: Tombol Perbarui Lokasi memungkinkan pengguna memicu ulang proses akuisisi GPS jika lokasi belum terkunci, akurasi masih kasar, atau ingin memperbarui posisi sebelum mengirim data presensi.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka halaman **Check-In** atau **Check-Out** dari Telegram Mini App.
  2. Perhatikan indikator lokasi di bagian atas form. Jika masih menampilkan _"Mengambil lokasi..."_ atau _"Gagal mengambil lokasi"_, GPS sedang dalam proses atau gagal.
  3. Tekan tombol **Perbarui Lokasi** (ikon refresh `RefreshCw`) di sebelah indikator lokasi untuk memicu ulang akuisisi GPS.
  4. Tunggu hingga koordinat dan alamat terisi otomatis. Jika masih gagal, pastikan GPS perangkat aktif dan izin lokasi telah diberikan.
  5. Setelah lokasi terkunci, lengkapi foto dan tekan **Kirim** untuk menyelesaikan presensi.

### 2. Melihat Total Jam Kerja Presisi (Jam + Menit)

- **Penjelasan Fitur**: Halaman Jam Kerja Saya kini menampilkan total akumulasi kehadiran dalam format yang lebih presisi — tidak hanya jam bulat, tetapi juga menit tersisa.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka Telegram Mini App dan navigasi ke halaman **Jam Kerja Saya** (`/my-work-hours`).
  2. Pada kartu "Jam Kerja Bulan Ini", total kehadiran ditampilkan dalam format `{jam} Jam {menit} Menit`.
  3. Pada bagian bawah, "Total Akumulasi Jam Kerja" juga menampilkan format yang sama untuk seluruh riwayat 60 hari terakhir.

### 3. Pengelolaan Pengajuan Izin & Cuti per Hari (Admin)

- **Penjelasan Fitur**: Setiap pengajuan izin atau cuti kini mencakup daftar hari individual yang dapat disetujui atau ditolak secara terpisah oleh admin.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Aktivitas → Izin** atau **Aktivitas → Cuti** dari sidebar navigasi.
  2. Klik baris pengajuan yang ingin ditinjau untuk membuka **Modal Detail Pengajuan**.
  3. Pada modal, tinjau daftar hari yang diajukan beserta status masing-masing.
  4. Setujui atau tolak per hari sesuai kebijakan, atau gunakan aksi persetujuan massal jika seluruh hari memiliki keputusan yang sama.
