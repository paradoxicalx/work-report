# 📝 Daily Work Report - Dedy (2026-10-23)

---

## 📌 Informasi Issue

- **Nomor Issue**: #377
- **Judul Issue**: Master Switch Penonaktifan Seluruh Cron Job RADIUS & Hotspot via Environment Variable (`DISABLE_RADIUS_CRON`)

## 📅 Laporan Harian - 23 Oktober 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

_Tidak ada pekerjaan yang belum di-commit (working tree bersih). Seluruh perubahan kode telah di-commit ke repositori dan di-merge ke branch utama (`main`)._

### 📅 Rincian Commit

#### [c789500](https://github.com/paradoxicalx/ISPF_V1/commit/c789500) & [e9b594a](https://github.com/paradoxicalx/ISPF_V1/commit/e9b594a) - resolve #377 (#377 - Master Switch Penonaktifan Seluruh Cron Job RADIUS & Hotspot via Environment Variable)

- **Komponen yang Berubah**:
  - `[backend/helpers/migration.help.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/helpers/migration.help.js)`
  - `[backend/cron/1_minute.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_minute.cron.js)`
  - `[backend/cron/1_hour.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_hour.cron.js)`
  - `[backend/cron/1_day.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_day.cron.js)`
  - `[backend/index.core.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/index.core.js)`
  - `[backend/env.example](file:///home/dhedhy/Project/Dekasimal-V1/backend/env.example)`
- **Deskripsi Perubahan & Fungsi**:
  - **`[backend/helpers/migration.help.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/helpers/migration.help.js)`**:
    - Menambahkan fungsi helper `isRadiusCronDisabled()` yang mendeteksi konfigurasi environment variable `DISABLE_RADIUS_CRON === 'true'` atau `RADIUS_CRON_ACTIVE === 'false'`.
    - Berfungsi sebagai kontrol sentral (master switch) untuk menonaktifkan seluruh background cron job yang mengelola sesi, retensi log, buka isolir otomatis, dan isolir expired pada modul RADIUS dan Hotspot Dekasimal v1.
  - **`[backend/cron/1_minute.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_minute.cron.js)`**:
    - Membungkus proses penanganan berkala per menit dengan pengecekan `!isRadiusCronDisabled()`:
      - Isolir voucher hotspot kadaluwarsa (`Voucher.updateMany`).
      - Pembaruan sesi hotspot stale / timeout 1 menit (`HSSession.updateMany`).
      - Pemutusan sesi accounting RADIUS stale / timeout 5 menit (`Session.updateMany`).
      - Pembukaan isolir otomatis akun RADIUS pasca pembayaran invoice via IPAYmu (`RadAuth.updateMany wait_payment: false, status: true`).
  - **`[backend/cron/1_hour.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_hour.cron.js)`**:
    - Menambahkan pembacaan environment variable `dotenv` di bagian awal berkas.
    - Menambahkan helper lokal `isRadiusZombieCheckActive()` dengan dukungan konfigurasi granular (`CHECK_RADIUS_ZOMBIE` / `DISABLE_RADIUS_ZOMBIE`) serta terikat dengan master switch `isRadiusCronDisabled()`.
    - Menambahkan proteksi pada pembersihan sesi voucher kosong (`HSSession.deleteMany`) dan penandaan offline sesi hotspot 1 jam (`HSSession.updateMany`).
    - Menambahkan kondisional pada pemanggilan `recheckAuth()` (pengecekan dan pemutusan sesi RADIUS zombie via Mikrotik API) agar dapat dimatikan sepenuhnya dan menampilkan status log di terminal backend saat dinonaktifkan.
  - **`[backend/cron/1_day.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_day.cron.js)`**:
    - Mengintegrasikan pengecekan `!isRadiusCronDisabled()` pada serangkaian rutinitas harian modul RADIUS & Hotspot:
      - Isolir voucher kadaluwarsa dan penghapusan voucher/sesi isolir lama (`Voucher` & `HSSession`).
      - Perhitungan dan pembaruan counter koneksi otentikasi RADIUS (`authentication_counter`).
      - Pembersihan log otentikasi RADIUS berumur lebih dari 30 hari (`RadLogs.deleteMany`).
      - Pembersihan histori sesi RADIUS nonaktif lebih dari 60 hari (`RadSession.deleteMany`).
      - Pembersihan data histori trafik RADIUS (`RadTraffic.deleteMany`).
      - Penonaktifan/isolir akun RADIUS pelanggan yang telah jatuh tempo/telat bayar (`offAuthIfExp()`).
  - **`[backend/index.core.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/index.core.js)`**:
    - Menambahkan pesan log informatif saat startup server backend jika `DISABLE_RADIUS_CRON=true` aktif: `"DISABLE_RADIUS_CRON=true : seluruh cron job sesi, retensi, dan manajemen RADIUS v1 dimatikan"`.
  - **`[backend/env.example](file:///home/dhedhy/Project/Dekasimal-V1/backend/env.example)`**:
    - Menambahkan dokumentasi konfigurasi variabel lingkungan baru `DISABLE_RADIUS_CRON = false` beserta penjelasan lengkap cakupan cron job yang terdampak.

---

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - **Kontrol Penuh Transisi & Migrasi RADIUS**: Administrator atau tim DevOps kini dapat dengan mudah mematikan seluruh background job RADIUS & Hotspot di Dekasimal v1 cukup dengan menyetel `DISABLE_RADIUS_CRON=true` pada `.env` backend v1 saat workload manajemen RADIUS dialihkan ke sistem Dekasimal v2.
  - **Transparansi Log Startup & Cron**: Menampilkan log yang jelas di terminal saat backend dijalankan dan saat interval cron 1 jam berjalan, memudahkan verifikasi apakah background job RADIUS sedang aktif atau dimatikan via environment variable.
- **Bug Fix / Solusi Masalah**:
  - **Mencegah Konflik Operasional Ganda (Split-Brain / Double Handling)**: Mengeliminasi potensi benturan antara cron job v1 dan v2 yang mengelola database/NAS yang sama, seperti benturan saat membuka/menutup isolir akun, pemutusan sesi aktif secara sepihak oleh v1 (`recheckAuth`), dan duplikasi pembersihan data retensi sesi/log/trafik RADIUS.
- **Menu/Tombol Baru**:
  - Tidak ada menu atau tombol UI baru di frontend; perubahan difokuskan pada infrastruktur background worker, scheduler cron, dan konfigurasi environment variable backend.
