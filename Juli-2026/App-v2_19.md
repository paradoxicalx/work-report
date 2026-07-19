# 📝 Daily Work Report - Dedy S.N Putra (2026-07-19)

---

## 📅 Laporan Harian - 19 Juli 2026

---

## 🌿 Branch: `issue-150` — RADIUS Server Golang & MongoDB (Work In Progress)

### 📌 Informasi Issue

- **Nomor Issue**: #150
- **Judul Issue**: RADIUS Server Golang & MongoDB
- **Status Branch**: `Belum di-merge` (sedang dalam pengembangan aktif)

### 📅 Rincian Commit

#### [bcaf13f] - save #150 - 18 Juli 2026, 17:21

*(Commit dasar penyusunan arsitektur radius-server berbasis Go)*

#### [Pekerjaan Belum Di-commit / Working Tree Changes] - 19 Juli 2026

Berikut adalah daftar berkas baru dan modifikasi yang sedang dikerjakan hari ini untuk penyelesaian integrasi RADIUS Server Go dengan Backend Node.js dan Web Admin Frontend:

- **Komponen yang Berubah**:
  - `backend/package.json` & `backend/package-lock.json`
  - `backend/src/server.js`
  - `backend/src/lib/redisClient.js`
  - `backend/src/grpc/` [NEW] _(Inisialisasi handler & controller gRPC backend)_
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/models/radiusLogs.model.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/invoiceFreeze.service.js` [NEW]
  - `backend/src/services/radiusControl.service.js` [NEW]
  - `backend/src/services/radiusEvent.service.js` [NEW]
  - `backend/src/services/radiusServerRegistry.service.js` [NEW]
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `radius-server/.env.example` [NEW]
  - `radius-server/.gitignore` [NEW]
  - `radius-server/DOCUMENTATION.md` [NEW] _(Dokumentasi teknis lengkap RADIUS Server Go)_
  - `radius-server/Dockerfile` [NEW]
  - `radius-server/Makefile` [NEW]
  - `radius-server/README.md` [NEW]
  - `radius-server/cmd/radiusd/main.go`
  - `radius-server/cmd/radiusd/wire.go`
  - `radius-server/proto/radius/v1/radius.proto`
  - `radius-server/gen/radius/v1/radius.pb.go`
  - `radius-server/gen/radius/v1/radius_grpc.pb.go`
  - `radius-server/internal/domain/ports/sweep.go` [NEW]
  - `radius-server/internal/domain/sweep/` [NEW] _(Logika pembersihan sesi tersangkut & kedaluwarsa)_
  - `radius-server/internal/transport/shared/addr_holder.go` [NEW]
  - `radius-server/internal/transport/shared/rebind_runner.go` [NEW]
  - `radius-server/internal/transport/shared/rebind_runner_test.go` [NEW]
  - `radius-server/pkg/config/rebind_bus.go` [NEW]
  - `radius-server/pkg/config/rebind_bus_test.go` [NEW]
  - `radius-server/test/e2e/` [NEW]
  - `radius-server/test/testutil/fake_sweep_repos.go` [NEW]
  - _(Dan modifikasi repositori Mongo, CoA, gRPC client, listener PPPoE/Hotspot/RouterLogin di radius-server)_

- **Deskripsi Perubahan & Fungsi**:
  - **Penyusunan Integrasi Backend Express.js dengan RADIUS Go**:
    - **gRPC Server di Backend**: Menambahkan server gRPC di backend Express.js (`backend/src/grpc/`) untuk menangani komunikasi dua arah (bidi-stream) dengan client `radiusd`. Logika ini menerima stream event sesi (`RadiusEvent`) dari RADIUS server untuk dipetakan ke log MongoDB dan mengirimkan perintah kontrol (`ControlCommand`) seperti pemutusan sesi (CoA/Disconnect).
    - **Registry Server RADIUS**: Mengimplementasikan `radiusServerRegistry.service.js` di backend untuk mendata instances server RADIUS yang aktif.
    - **Manajemen Konfigurasi & Hot-Reload**: Mengintegrasikan tab **System Settings** di frontend dengan API settings backend untuk menyimpan konfigurasi RADIUS server, termasuk port, secret, dan mengaktifkan fungsionalitas hot-reload konfigurasi RADIUS secara langsung tanpa restart service melalui event bus.
    - **Freeze-Check Sweep**: Membuat logika sweep otomatis untuk membersihkan sesi yang tersangkut (stale sessions) dan voucher kedaluwarsa (`radius-server/internal/domain/sweep/`).
    - **Socket Rebinding & Dynamic Ports**: Menambahkan fungsionalitas `rebind_runner.go` di radius-server untuk menangani reload socket/port UDP secara dinamis ketika konfigurasi diubah oleh admin melalui dashboard.

---

## 🌿 Branch: `issue-147` — Daftar Sesi RADIUS (Finalization)

### 📌 Informasi Issue

- **Nomor Issue**: #147
- **Judul Issue**: Daftar Sesi RADIUS — Manajemen Sesi Online/Offline Pengguna
- **Status Branch**: `Belum di-merge` (Remote branch `origin/issue-147` memiliki commit progress terpisah)

### 📅 Rincian Commit

#### [3c6fa6c] - save #147 - 19 Juli 2026, 01:30

- **Komponen yang Berubah**:
  - `audit-report-issue-147.md` [NEW]
  - `audit-task-issue-147.md` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menyusun dokumen rencana audit dan tugas pengerjaan untuk penyelesaian akhir integrasi data log sesi RADIUS.

---

## 🌿 Branch: `issue-148` — Modul Manajemen Pengguna Hotspot (CRUD Hotspot Users)

### 📌 Informasi Issue

- **Nomor Issue**: #148
- **Judul Issue**: Modul Manajemen Pengguna Hotspot (Hotspot User Module)
- **Status Branch**: `Sudah di-merge` ke master (selesai 18 Juli 2026)

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #150  | RADIUS Server Golang & MongoDB | Penyelesaian seluruh arsitektur RADIUS server berbasis Go (M1-M5), integrasi dua arah via gRPC ke backend Node.js, pembersihan sesi periodik (sweep), hot-reload konfigurasi secara real-time dari Dashboard UI, dan dynamic UDP socket rebinding. |
| #147  | Daftar Sesi RADIUS | Penambahan rencana dokumen audit internal untuk pelacakan sesi pengguna RADIUS (PPPoE/Hotspot). |

### Kemampuan Baru Pengguna/Admin

- Admin kini dapat **mengubah konfigurasi port dan shared secret RADIUS Server** langsung dari Web Admin di tab **Pengaturan Sistem** (System Settings). Perubahan ini akan memicu hot-reload socket RADIUS server secara otomatis tanpa memutus service Go yang berjalan.
- Sistem backend Express.js kini siap menerima log stream sesi real-time dari RADIUS Go untuk memetakan statistik trafik dan masa aktif broadband user ke database MongoDB.
- Sesi pengguna yang tersangkut (stale sessions) atau voucher hotspot yang kedaluwarsa akan dibersihkan secara otomatis oleh routine cron sweep yang berjalan secara periodik.

### Bug Fix / Solusi Masalah

- **Pencegahan port conflict / socket leak**: Implementasi `rebind_runner` dengan thread-safe mutex memastikan socket lama ditutup dengan bersih sebelum socket baru dibuka pada port yang baru dikonfigurasi.
- **Sinkronisasi Uptime & Bandwidth**: Integrasi model data Mongo di repositori Go memastikan kalkulasi delta usage akuntansi (accounting) sinkron dengan data limit yang tercatat di backend.

### Menu/Fitur Baru

- **Form Pengaturan RADIUS** di halaman **Settings -> System** Web Admin (mengelola port, host, key, gRPC host, dll.).
- **Dokumentasi Teknis Lengkap** RADIUS Server Go di [DOCUMENTATION.md](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/DOCUMENTATION.md).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Dynamic Socket Rebinding & Hot-Reload** adalah fitur di mana RADIUS Server berbasis Go dapat membaca perubahan konfigurasi sistem (seperti perubahan port UDP PPPoE/Hotspot atau shared secret) secara real-time. Melalui event bus gRPC / local reload trigger, server Go akan menutup socket UDP lama dan membuka socket UDP baru pada port yang baru tanpa harus mematikan proses binary (`radiusd`), menjaga ketersediaan layanan (high availability) untuk user lain yang sedang terkoneksi.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka Web Admin Dekasimal V2.
  2. Buka menu **Settings** -> **System**.
  3. Cari bagian **RADIUS Server Configuration**.
  4. Ubah salah satu parameter, misalnya port accounting PPPoE dari `1813` menjadi `1815` atau ganti shared secret.
  5. Klik **Save Configuration**.
  6. Backend Express.js akan menyimpan konfigurasi ke MongoDB dan memancarkan gRPC control command ke server Go.
  7. Server Go (`radiusd`) menerima event tersebut, memicu hot-reload, dan melakukan rebinding socket UDP ke port `1815` secara instan. Status dan log rebinding dapat dipantau di console logger.
