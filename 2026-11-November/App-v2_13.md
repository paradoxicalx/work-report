# 📝 Daily Work Report - Dedy S.N Putra (2026-11-13)

---

## 📅 Laporan Harian - 13 November 2026

---

## 🌿 Branch: `issue-547` — In-Memory Auth Cache RADIUS dengan Invalidasi Real-Time gRPC, Pemulihan (Restore) Akun Soft-Delete, & Penyelarasan HTTP Status Code Controller

### 📌 Informasi Issue

- **Nomor Issue**: #547
- **Judul Issue**: In-Memory Auth Cache `radiusd` dengan Invalidasi Real-Time gRPC (dan Keepalive Stream), Fitur Restore Akun Broadband & RADIUS Non-Pelanggan yang Ter-soft-delete, serta Penyelarasan HTTP Status Code pada Controller & Panduan Agen
- **Status Branch**: `Belum di-merge` (aktif di `origin/issue-547`, belum di-merge ke `origin/master`)

### 📅 Rincian Commit

#### [da3c5569] - resolve #547 - 13 November 2026, 16:10:00 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `documentations/CREATE_REPORT.md`
  - **Protokol & gRPC (`backend` & `radius-server`)**:
    - `backend/proto/radius/v1/radius.proto`
    - `radius-server/proto/radius/v1/radius.proto`
    - `radius-server/gen/radius/v1/radius.pb.go`
    - `backend/src/grpc/server.js`
    - `backend/src/grpc/streamRegistry.js`
    - `radius-server/internal/transport/grpcclient/client.go`
  - **RADIUS Auth Cache (`radius-server` Go & `backend` Node.js)**:
    - `radius-server/internal/repository/cache/cached_auth_repo.go` [NEW]
    - `radius-server/internal/repository/cache/cached_auth_repo_test.go` [NEW]
    - `radius-server/cmd/radiusd/wire.go`
    - `backend/src/services/radiusAuthCache.service.js` [NEW]
    - `backend/src/services/radiusAuthentication.service.js`
    - `backend/src/services/recovery.service.js`
    - `backend/test/unit/radiusAuthCache.service.test.js` [NEW]
  - **Restore Akun Broadband & RADIUS Non-Pelanggan**:
    - `backend/src/controllers/radiusAuthentication.controller.js`
    - `backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`
    - `backend/src/routes/radiusAuthentication.route.js`
    - `backend/src/routes/radiusAuthenticationNonCustomer.route.js`
    - `backend/src/locales/en/translation.json`
    - `backend/src/locales/id/translation.json`
    - `backend/test/integration/radiusAuthenticationNonCustomer.delete.test.js`
  - **Penyelarasan Error Handling & Status Code Controller**:
    - `backend/src/controllers/ticket.controller.js`
    - `backend/test/integration/ticketStatusCodes.test.js` [NEW]
    - `backend/test/unit/controllerStatusCode.test.js` [NEW]

- **Deskripsi Perubahan & Fungsi**:

  1. **In-Memory Auth Cache `radiusd` dengan Invalidasi Real-Time gRPC (`CachedAuthRepository`)**:
     - Menambahkan pembungkus repository `CachedAuthRepository` (`radius-server/internal/repository/cache/cached_auth_repo.go`) di `wire.go` yang melakukan caching LRU in-memory untuk `FindAuthSnapshotByUsername` dengan `DefaultAuthTTL = 5 menit` sebagai jaring pengaman.
     - Cache **hanya aktif saat stream gRPC ke backend terhubung** (`SetStreamConnected(true)`). Begitu stream gRPC terputus (`SetStreamConnected(false)`), seluruh cache langsung dikosongkan dan pengisian cache dinonaktifkan agar perubahan akun di backend (isolir, hapus, ganti password) tidak pernah tertahan data basi.
     - Menambahkan pesan `InvalidateAuthCache` (`usernames`, `flush_all`) pada `ControlCommand` di `radius.proto` serta `broadcastCommand` di `backend/src/grpc/streamRegistry.js` (termasuk lintas instance backend via Redis pub/sub `radius:control-command`) dan `invalidateRadiusAuthCache` (`backend/src/services/radiusAuthCache.service.js`, fire-and-forget).
     - Memasang pemicu invalidasi cache di `radiusAuthentication.service.js` (saat buat, ubah, hapus, pulihkan, ubah status/isolir, atau update batch) dan `recovery.service.js` (saat recovery massal wait-payment, quota-limit, dan frozen-session).
     - Menambahkan konfigurasi gRPC keepalive (`Time: 30s`, `Timeout: 10s`, `PermitWithoutStream: true`) di sisi `radius-server` (`grpcclient/client.go`) dan `backend` (`grpc/server.js`) agar koneksi TCP/HTTP2 *half-open* lintas kota segera terdeteksi tanpa ditolak `ENHANCE_YOUR_CALM`.

  2. **Pemulihan (Restore) Akun Broadband & RADIUS Non-Pelanggan yang Ter-soft-delete**:
     - Menambahkan fungsi service `restoreAuthenticationById` di `backend/src/services/radiusAuthentication.service.js` yang memvalidasi akun benar-benar berstatus `deleted: true` (409 jika belum dihapus), memastikan pemilik (`customer`, `partner`, atau `ref_partner`) masih aktif dan belum terhapus (422 jika pemilik terhapus), mereservasi ulang IP statis (`syncStaticIpReservation`, 409 jika IP sudah dipakai akun lain), memulihkan dokumen secara atomik dengan `findOneAndUpdateDeleted`, menyegarkan cache Redis + menginvalidasi auth cache `radiusd`, serta memastikan target trafik broadband (`ensureBroadbandTrafficTarget`).
     - Menambahkan endpoint `PATCH /api/v1/broadband/restore/:id` (`broadband.create`) di `radiusAuthentication.route.js` & `radiusAuthentication.controller.js`, serta `PATCH /api/v1/radius-non-customer/restore/:id` (`radiusNonCustomer.create`) di `radiusAuthenticationNonCustomer.route.js` & `radiusAuthenticationNonCustomer.controller.js` lengkap dengan dokumentasi Swagger dan terjemahan i18n (`en` & `id`).

  3. **Perbaikan HTTP Status Code & Error Propagation pada Controller**:
     - Memperbaiki `createTicket` di `backend/src/controllers/ticket.controller.js` agar tidak menelan error saat lookup `dedicated` atau `authentication` gagal dan memastikan `res.status(400)` diteruskan dengan benar ke global `errorHandler`.
     - Menambahkan unit test `controllerStatusCode.test.js` dan integration test `ticketStatusCodes.test.js` untuk mengunci perilaku `errorHandler` yang membaca `res.statusCode` (bukan `err.statusCode`).

  4. **Pembaruan Dokumentasi & Aturan AI Agent**:
     - Memperbarui `AGENTS.md` terkait penegasan aturan `res.status(code)` sebelum `throw` dan `documentations/CREATE_REPORT.md` untuk aturan *strict filtering* penulis commit pada laporan harian.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #547  | Auth Cache `radiusd` via gRPC, Restore Akun Soft-Delete, & Perbaikan Status Code | Autentikasi RADIUS lebih cepat & tahan lonjakan query MongoDB dengan invalidasi real-time, akun Broadband/Non-Customer yang terhapus kini dapat dipulihkan aman beserta IP statisnya, dan error HTTP 400 pada pembuatan tiket tidak lagi tertelan |

### Kemampuan Baru Pengguna/Admin

- Admin dapat memulihkan akun **Broadband** maupun **RADIUS Non-Pelanggan** yang sebelumnya di-soft-delete melalui endpoint `PATCH /api/v1/broadband/restore/:id` dan `PATCH /api/v1/radius-non-customer/restore/:id`, lengkap dengan validasi status pemilik dan reservasi ulang IP statis otomatis.

### Bug Fix / Solusi Masalah

- **Stale Auth Cache & Koneksi Half-Open gRPC**: Mencegah data autentikasi basi di `radiusd` dengan mengosongkan dan menonaktifkan cache otomatis saat stream gRPC terputus serta menambahkan gRPC HTTP/2 keepalive agar koneksi *half-open* segera terdeteksi.
- **Error Lookup Tiket Tertelan**: Memperbaiki `createTicket` agar kegagalan pencarian `dedicated` maupun `authentication` langsung mengembalikan HTTP `400` dengan pesan i18n yang jelas alih-alih melanjutkan proses atau jatuh ke `500`.

### Menu/Fitur Baru

- Endpoint API baru:
  - `PATCH /api/v1/broadband/restore/:id` (memerlukan hak akses `broadband.create`)
  - `PATCH /api/v1/radius-non-customer/restore/:id` (memerlukan hak akses `radiusNonCustomer.create`)

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **In-Memory Auth Cache `radiusd`**: `radiusd` menyimpan snapshot autentikasi di memori (LRU cache, TTL 5 menit) selama koneksi stream gRPC ke backend aktif. Setiap kali ada perubahan akun di backend (edit password, isolir, hapus, restore, atau recovery massal), backend mengirim perintah gRPC `InvalidateAuthCache` ke seluruh instance `radiusd` secara real-time. Jika koneksi gRPC putus, `radiusd` otomatis mengosongkan cache dan membaca langsung dari MongoDB sampai stream tersambung kembali.
  - **Restore Akun RADIUS**: Memungkinkan pemulihan akun soft-delete tanpa membuat akun baru dari awal, sambil menjaga integritas IP statis dan kepemilikan pelanggan/mitra.
- **Langkah Penggunaan (Tutorial)**:
  1. **Memulihkan Akun Broadband / Non-Pelanggan**: Panggil `PATCH /api/v1/broadband/restore/<authentication_id>` atau `PATCH /api/v1/radius-non-customer/restore/<authentication_id>` menggunakan token admin yang memiliki hak akses `broadband.create` / `radiusNonCustomer.create`.
  2. Jika pemilik akun masih aktif dan IP statis belum dipakai akun lain, akun akan kembali aktif (`deleted: false`, `isOnline: false`) dan cache `radiusd` langsung ter-invalidasi otomatis.

