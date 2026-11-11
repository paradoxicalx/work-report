# 📝 Daily Work Report - Dedy S.N Putra (2026-07-21)

---

## 📅 Laporan Harian - 21 Juli 2026

---

## 🌿 Branch: `master` — Resolve #22 & #146

### 📌 Informasi Issue

- **Nomor Issue**: #22 & #146
- **Judul Issue**: Auth Token Refresh & Manajemen Pelanggan Pasif/Blacklist
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [6acc63d] - resolve #22 - 2026-07-21 22:21:54

- **Komponen yang Berubah**:
  - [`backend/src/controllers/auth.controller.js`](backend/src/controllers/auth.controller.js) _(Perubahan signifikan +104 baris)_
  - [`backend/src/routes/admin.route.js`](backend/src/routes/admin.route.js)
  - [`backend/src/routes/mobileCustomer.route.js`](backend/src/routes/mobileCustomer.route.js)
  - [`frontend/src/app/contexts/auth/Provider.jsx`](frontend/src/app/contexts/auth/Provider.jsx)
  - [`frontend/src/utils/jwt.js`](frontend/src/utils/jwt.js) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Implementasi mekanisme **auth token refresh** yang memungkinkan sistem memperpanjang masa aktif token JWT secara otomatis saat token hampir kedaluwarsa.
  - Penambahan utility [`jwt.js`](frontend/src/utils/jwt.js) untuk menangani decode dan validasi token di sisi frontend.
  - Modifikasi [`Provider.jsx`](frontend/src/app/contexts/auth/Provider.jsx) untuk mengintegrasikan logika refresh token ke dalam auth context.
  - Penambahan route endpoint pada [`admin.route.js`](backend/src/routes/admin.route.js) dan [`mobileCustomer.route.js`](backend/src/routes/mobileCustomer.route.js) untuk mendukung alur autentikasi baru.

#### [36c8077] - resolve #146 - 2026-07-21 13:14:24

- **Komponen yang Berubah**:
  - [`backend/src/constants/customer.constant.js`](backend/src/constants/customer.constant.js)
  - [`backend/src/controllers/customer.controller.js`](backend/src/controllers/customer.controller.js) _(+120 baris)_
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/models/customer.model.js`](backend/src/models/customer.model.js)
  - [`backend/src/routes/customer.route.js`](backend/src/routes/customer.route.js) _(+180 baris)_
  - [`backend/src/services/customer.service.js`](backend/src/services/customer.service.js) _(+187 baris)_
  - [`backend/src/services/customerPartner.service.js`](backend/src/services/customerPartner.service.js)
  - [`backend/src/services/hotspotUser.service.js`](backend/src/services/hotspotUser.service.js)
  - [`backend/src/services/radiusSession.service.js`](backend/src/services/radiusSession.service.js)
  - [`frontend/src/app/navigation/users.js`](frontend/src/app/navigation/users.js)
  - [`frontend/src/app/pages/users/blacklist/index.jsx`](frontend/src/app/pages/users/blacklist/index.jsx) [NEW]
  - [`frontend/src/app/pages/users/blacklist/schema/columns.jsx`](frontend/src/app/pages/users/blacklist/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/users/customer/edit.jsx`](frontend/src/app/pages/users/customer/edit.jsx)
  - [`frontend/src/app/pages/users/customer/profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx) _(+246 baris)_
  - [`frontend/src/app/pages/users/pasif/index.jsx`](frontend/src/app/pages/users/pasif/index.jsx) [NEW]
  - [`frontend/src/app/pages/users/pasif/schema/columns.jsx`](frontend/src/app/pages/users/pasif/schema/columns.jsx) [NEW]
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/router/users/blacklistRoute.jsx`](frontend/src/app/router/users/blacklistRoute.jsx) [NEW]
  - [`frontend/src/app/router/users/pasifRoute.jsx`](frontend/src/app/router/users/pasifRoute.jsx) [NEW]
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
- **Deskripsi Perubahan & Fungsi**:
  - Implementasi fitur **manajemen pelanggan pasif** dan **blacklist** (daftar hitam) untuk pengelolaan status pelanggan yang lebih granular.
  - Penambahan model field pada [`customer.model.js`](backend/src/models/customer.model.js) untuk mendukung status pasif dan blacklist.
  - Pembuatan halaman baru [`blacklist/index.jsx`](frontend/src/app/pages/users/blacklist/index.jsx) dan [`pasif/index.jsx`](frontend/src/app/pages/users/pasif/index.jsx) dengan konfigurasi kolom tabel masing-masing.
  - Penambahan route baru [`blacklistRoute.jsx`](frontend/src/app/router/users/blacklistRoute.jsx) dan [`pasifRoute.jsx`](frontend/src/app/router/users/pasifRoute.jsx) ke dalam router aplikasi.
  - Pembaruan navigasi [`users.js`](frontend/src/app/navigation/users.js) untuk menambahkan akses ke menu blacklist dan pasif.
  - Penambahan 120 baris logic bisnis pada [`customer.controller.js`](backend/src/controllers/customer.controller.js) untuk memproses permintaan perubahan status pelanggan.
  - Penambahan 187 baris data access layer pada [`customer.service.js`](backend/src/services/customer.service.js) untuk query database terkait status pelanggan.
  - Pembaruan [`customer/profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx) dengan penambahan 246 baris untuk menampilkan informasi status pelanggan (pasif/blacklist).
  - Penambahan 180 baris route endpoint pada [`customer.route.js`](backend/src/routes/customer.route.js) untuk API manajemen pelanggan.
  - Penambahan 24 translation key pada file i18n untuk mendukung multi-bahasa.

---

## 🌿 Branch: `issue-150` — Resolve #150

### 📌 Informasi Issue

- **Nomor Issue**: #150
- **Judul Issue**: Implementasi Radius Server (gRPC Integration)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [b4f9d2f] - resolve #150 - 2026-07-18 17:21:10

- **Komponen yang Berubah**:
  - [`backend/src/controllers/radiusControl.controller.js`](backend/src/controllers/radiusControl.controller.js) [NEW]
  - [`backend/src/grpc/radiusControl.handler.js`](backend/src/grpc/radiusControl.handler.js) [NEW]
  - [`backend/src/grpc/server.js`](backend/src/grpc/server.js) [NEW]
  - [`backend/src/grpc/streamRegistry.js`](backend/src/grpc/streamRegistry.js) [NEW]
  - [`backend/src/routes/radiusControl.route.js`](backend/src/routes/radiusControl.route.js) [NEW]
  - [`backend/src/services/radiusControl.service.js`](backend/src/services/radiusControl.service.js) [NEW]
  - [`backend/src/services/radiusEvent.service.js`](backend/src/services/radiusEvent.service.js) [NEW]
  - [`backend/src/services/radiusIdentity.service.js`](backend/src/services/radiusIdentity.service.js) [NEW]
  - [`backend/src/services/radiusServerRegistry.service.js`](backend/src/services/radiusServerRegistry.service.js) [NEW]
  - [`backend/src/services/invoiceFreeze.service.js`](backend/src/services/invoiceFreeze.service.js) [NEW]
  - [`radius-server/`](radius-server/) _(Modul baru lengkap: 200+ file Go)_
  - [`frontend/src/app/pages/dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx)
  - [`frontend/src/app/pages/services/broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx)
  - [`frontend/src/utils/axios.js`](frontend/src/utils/axios.js)
  - [`telegram-apps/src/lib/axiosClient.js`](telegram-apps/src/lib/axiosClient.js)
- **Deskripsi Perubahan & Fungsi**:
  - Implementasi penuh **Radius Server** berbasis Go dengan arsitektur gRPC untuk autentikasi PPPoE dan Hotspot.
  - Pembentukan modul [`radius-server/`](radius-server/) yang mencakup:
    - Transport layer: PPPoE auth/acct listener, Hotspot auth/acct listener, RouterLogin listener
    - Domain logic: autentikasi, accounting, sweep session, isolir rules
    - Repository: MongoDB, Redis cache, WAL (Write-Ahead Log) untuk durability
    - gRPC client untuk komunikasi dengan Backend
    - COA (Change of Action) client untuk disconnect/reconnect user
    - Protobuf definition untuk interface gRPC
    - Unit tests dan integration tests komprehensif
  - Penambahan **gRPC server** pada Backend untuk menerima event dari Radius Server secara real-time.
  - Implementasi **stream registry** untuk mengelola koneksi gRPC streaming.
  - Penambahan **invoice freeze service** untuk menangguhkan tagihan saat pelanggan diisolir.
  - Pembaruan dashboard radius di [`frontend`](frontend/src/app/pages/dashboards/radius/index.jsx) untuk menampilkan status koneksi real-time.
  - Pembaruan detail broadband service untuk integrasi status radius.
  - Modifikasi axios interceptor pada frontend dan telegram-apps untuk header autentikasi yang diperbarui.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                     | Dampak Utama                                                       |
| ----- | ------------------------- | ------------------------------------------------------------------ |
| #22   | Auth Token Refresh        | Sistem autentikasi lebih robust dengan auto-refresh token          |
| #146  | Pelanggan Pasif/Blacklist | Pengelolaan status pelanggan lebih granular (pasif & daftar hitam) |
| #150  | Radius Server Integration | Infrastruktur autentikasi PPPoE/Hotspot berbasis Go dengan gRPC    |

### Kemampuan Baru Pengguna/Admin

- **Auto-refresh token**: Sistem secara otomatis memperpanjang masa aktif token JWT saat hampir kedaluwarsa, sehingga pengguna tidak perlu login ulang secara频繁.
- **Manajemen pelanggan pasif**: Admin dapat menandai pelanggan sebagai "pasif" untuk pengelolaan sementara tanpa menghapus data.
- **Blacklist pelanggan**: Admin dapat memasukkan pelanggan ke dalam daftar hitam untuk membatasi akses layanan.
- **Dashboard radius real-time**: Admin dapat memantau status koneksi PPPoE/Hotspot secara real-time melalui dashboard.

### Bug Fix / Solusi Masalah

- Perbaikan mekanisme autentikasi yang sebelumnya memerlukan login ulang secara频繁 saat token kedaluwarsa.
- Solusi untuk pengelolaan pelanggan yang memerlukan penangguhan sementara layanan tanpa menghapus data pelanggan.

### Menu/Fitur Baru

- **Menu Users > Pasif**: Halaman daftar pelanggan dengan status pasif.
- **Menu Users > Blacklist**: Halaman daftar pelanggan yang masuk dalam daftar hitam.
- **Dashboard Radius**: Halaman monitoring status koneksi radius (PPPoE/Hotspot).
- **Radius Server Module**: Modul baru Go-based untuk autentikasi dan accounting RADIUS.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Fitur utama hari ini adalah implementasi **Auth Token Refresh** yang memungkinkan sesi pengguna tetap aktif tanpa perlu login ulang, serta penambahan fitur **Pelanggan Pasif/Blacklist** untuk pengelolaan status pelanggan yang lebih fleksibel. Selain itu, branch `issue-150` berisi implementasi penuh **Radius Server** berbasis Go yang akan diintegrasikan untuk autentikasi PPPoE dan Hotspot.

- **Langkah Penggunaan (Tutorial)**:
  1. **Auth Token Refresh**: Fitur ini berjalan otomatis di latar belakang. Pengguna tidak perlu melakukan apa-apa — token akan diperpanjang secara otomatis saat masih dalam masa aktif.
  2. **Pelanggan Pasif**: Buka menu **Users > Pasif** → Klik pelanggan yang ingin diubah statusnya → Ubah status menjadi "Pasif" → Simpan.
  3. **Blacklist**: Buka menu **Users > Blacklist** → Klik "Tambah Blacklist" → Pilih pelanggan → Masukkan alasan → Simpan.
  4. **Dashboard Radius** _(setelah integrasi)_: Buka menu **Dashboard > Radius** → Pantau status koneksi PPPoE/Hotspot secara real-time.
