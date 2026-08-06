# 📝 Daily Work Report - Dedy (2026-08-06)

---

## 📅 Laporan Harian - 6 Agustus 2026

---

## 🌿 Branch: `master` (merged from issue-195) — Perbaikan & Peningkatan Sistem Absensi (Attendance Device Sync & Tampilan Kalender Absensi)

### 📌 Informasi Issue

- **Nomor Issue**: #195
- **Judul Issue**: Perbaikan & Peningkatan Sistem Absensi (Attendance Device Sync & Tampilan Kalender Absensi)
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [5770285] - resolve #195 - Kamis, 6 Agustus 2026 19:42:51

- **Komponen yang Berubah**:
  - [`backend/src/services/attendanceDeviceSync.service.js`](backend/src/services/attendanceDeviceSync.service.js)
  - [`frontend/src/components/attendance/components/ManualCheckOutDrawer.jsx`](frontend/src/components/attendance/components/ManualCheckOutDrawer.jsx)
  - [`frontend/src/app/pages/activities/attendance/index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
- **Deskripsi Perubahan & Fungsionalitas**:

  **Backend — [`attendanceDeviceSync.service.js`](backend/src/services/attendanceDeviceSync.service.js):**
  1. **Cooldown Pulse Logic (Scan Berulang):** Menambahkan mekanisme cooldown (`PUNCH_COOLDOWN_SEC`, default 60 detik) untuk menangani scan berulang dari mesin absen.
     - **Saat Check-Out:** Jika karyawan melakukan scan lagi dalam jeda 1 menit setelah check-out, waktu check-out diperbarui ke timestamp scan terakhir (bukan membuat check-in baru). Ini mencegah entri absensi ganda akibat scan berulang.
     - **Saat Check-In:** Jika karyawan melakukan scan lagi dalam jeda 1 menit setelah check-in, waktu check-in diperbarui ke timestamp scan terakhir (bukan membuat check-out). Ini memperbaiki masalah di mana scan ganda menciptakan absensi palsu.
  2. **Perubahan Referensi `checkin_id`:** Pada proses check-out, field `checkin_id` tidak lagi di-reset ke `null` (hanya `checkin` yang diset `false`). Ini mempertahankan referensi ke kehadiran terakhir, memudahkan pelacakan riwayat.
  3. **Pencarian Presensi Terakhir:** Menambahkan logika fallback untuk mencari presensi terakhir jika `checkin_id` tidak tersedia — menggunakan query ke collection `Presence` dengan `out.time` yang ada, diurutkan berdasarkan waktu check-out terbaru.
  4. **Penambahan Import Model:** Mengimport model `Presence` (`attendancePresence.model.js`) untuk query langsung.

  **Frontend — [`index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx):**
  1. **Refactor Total Logika Render Kolom Kalender:** Mengubah pendekatan rendering kolom kalender dari metode `effectiveStatusType` per-hari menjadi pendekatan berbasis item tercakup (`coveredItems`) dengan colSpan dinamis.
  2. **Fungsi Helper `getItemEndDay()`:** Menambahkan fungsi baru untuk menghitung hari berakhirnya kehadiran lintas hari (misal check-in pada tanggal 5, check-out pada tanggal 7 → colSpan 3 hari).
  3. **Perhitungan ColSpan Dinamis:** Algoritma baru memeriksa beberapa hari ke depan untuk menentukan rentang hari yang tercakup oleh kehadiran, izin, atau cuti — memungkinkan tampilan yang lebih akurat untuk kehadiran lintas hari.
  4. **Pengumpulan Item Tercakup (`coveredItems`):** Setiap hari yang tercakup dalam rentang colSpan dikumpulkan ke dalam array `coveredItems` yang berisi tipe item (presence, permission, vacation), dengan posisi CSS (`marginLeft`, `width`) yang dihitung berdasarkan proporsi rentang hari.
  5. **Skip-Day Tracking:** Menggunakan `Set` untuk menandai hari-hari yang sudah ter-render dalam rentang colSpan, mencegah rendering ganda.
  6. **Refactor Rendering Presence/Permission/Vacation:** Setiap tipe item (presence, permission, vacation) di-render dalam `<div>` dengan posisi CSS yang proporsional, memungkinkan tampilan yang lebih fleksibel dan akurat untuk kasus kehadiran lintas hari.

  **Frontend — [`ManualCheckOutDrawer.jsx`](frontend/src/components/attendance/components/ManualCheckOutDrawer.jsx):**
  - Perubahan minor (2 baris) — perbaikan kecil pada komponen drawer.

  **Frontend — [`translations.json`](frontend/src/i18n/locales/en/translations.json) (EN & ID):**
  - Menambahkan key `employee.name` (Employee Name / Nama Karyawan) di kedua file terjemahan.

#### [32077ab] - resolve #195 - Kamis, 6 Agustus 2026 19:45:07

- **Komponen yang Berubah**: Sama dengan commit di atas (squash merge ke `master`)
- **Deskripsi Perubahan & Fungsionalitas**: Squash merge dari commit sebelumnya ke branch `master`. Tidak ada perubahan tambahan — hanya penyatuan commit menjadi satu.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                  | Dampak Utama                                                                                                                    |
| ----- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| #195  | Perbaikan & Peningkatan Sistem Absensi | Perbaikan logika scan berulang pada mesin absen dan peningkatan tampilan kalender absensi untuk mendukung kehadiran lintas hari |

### Kemampuan Baru Pengguna/Admin

- **Toleransi Scan Berulang Mesin Absen:** Karyawan yang melakukan scan ganda dalam jeda 1 menit setelah check-in/check-out akan diperlakukan sebagai pembaruan waktu (bukan entri ganda). Ini mengurangi data absensi palsu yang disebabkan oleh mesin absen yang mengirimkan log berulang.
- **Tampilan Kalender Absensi yang Lebih Akurat:** Kehadiran yang berlangsung lintas hari (misal check-in Kamis pagi, check-out Jumat malam) kini ditampilkan sebagai blok yang terbentang di beberapa hari kalender secara proporsional, bukan hanya di hari check-in.
- **Penanganan Overlapping Data:** Izin dan cuti yang terjadi bersamaan dengan kehadiran pada hari yang sama kini dapat ditampilkan secara bersamaan dalam satu sel kalender, meningkatkan kejelasan informasi.

### Bug Fix / Solusi Masalah

- **Bug Scan Ganda Mesin Absen:** Sebelumnya, scan berulang dari mesin absen dalam waktu dekat (kurang dari 1 menit) akan membuat entri absensi ganda (check-in + check-out palsu). Sekarang, scan ulang diperlakukan sebagai pembaruan waktu yang sah.
- **Bug Tampilan Kolom Kalender:** Tampilan kolom kalender sebelumnya hanya menampilkan data per hari tanpa memperhitungkan rentang hari yang tercakup oleh kehadiran lintas hari. Sekarang, setiap kehadiran yang melintasi beberapa hari ditampilkan dengan benar menggunakan colSpan dinamis.

### Menu/Fitur Baru

- **Fitur Pembaruan Waktu Absensi Otomatis:** Sistem kini secara otomatis memperbarui waktu check-in/check-out jika ada scan berulang dalam jeda cooldown (1 menit), mengurangi kebutuhan intervensi manual oleh admin.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur:** Fitur utama yang dikerjakan hari ini adalah perbaikan logika sinkronisasi perangkat absen (attendance device sync) pada backend dan peningkatan tampilan kalender absensi pada frontend. Backend kini menggunakan mekanisme cooldown untuk menangani scan berulang dari mesin absen, sementara frontend menggunakan algoritma colSpan dinamis untuk menampilkan kehadiran lintas hari secara proporsional dalam tabel kalender.
- **Langkah Penggunaan (Tutorial):**
  1. **Backend — Cooldown Logic:** Karyawan melakukan scan di mesin absen. Jika scan berulang terjadi dalam 1 menit setelah check-out, waktu check-out diperbarui (bukan check-in baru). Jika scan berulang terjadi dalam 1 menit setelah check-in, waktu check-in diperbarui (bukan check-out). Interval cooldown dapat dikonfigurasi via environment variable `ATTENDANCE_MIN_PUNCH_INTERVAL_SEC` (default: 60 detik).
  2. **Frontend — Tampilan Kalender Absensi:** Buka halaman Attendance (Activities → Attendance). Setiap kehadiran lintas hari akan ditampilkan sebagai blok yang terbentang di beberapa hari kolom. Izin dan cuti yang bersamaan dengan kehadiran juga akan ditampilkan secara bersamaan dalam sel yang sama. Klik pada tombol kehadiran untuk melihat detail atau mengubah status.
