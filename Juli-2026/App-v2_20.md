# 📝 Daily Work Report - Dedy S.N Putra (2026-07-20)

---

## 📅 Laporan Harian - 20 Juli 2026

---

## 🌿 Branch: `issue-150` — Integrasi Radius Server & Kontrol PPPoE/Hotspot

### 📌 Informasi Issue

- **Nomor Issue**: #150
- **Judul Issue**: Integrasi Radius Server & Kontrol PPPoE/Hotspot
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`5fb1abd`](issue-150) - `resolve #150` - 20 Juli 2026, 23:07

- **Komponen yang Berubah**:
  - [`backend/src/controllers/radiusControl.controller.js`](backend/src/controllers/radiusControl.controller.js) [NEW]
  - [`backend/src/routes/radiusControl.route.js`](backend/src/routes/radiusControl.route.js) [NEW]
  - [`backend/src/services/radiusIdentity.service.js`](backend/src/services/radiusIdentity.service.js) [NEW]
  - [`backend/src/services/radiusEvent.service.js`](backend/src/services/radiusEvent.service.js)
  - [`backend/src/services/radiusServerRegistry.service.js`](backend/src/services/radiusServerRegistry.service.js)
  - [`backend/src/grpc/radiusControl.handler.js`](backend/src/grpc/radiusControl.handler.js)
  - [`backend/src/grpc/server.js`](backend/src/grpc/server.js)
  - [`backend/src/grpc/streamRegistry.js`](backend/src/grpc/streamRegistry.js)
  - [`backend/src/sockets/socket-io.js`](backend/src/sockets/socket-io.js)
  - [`backend/src/app.js`](backend/src/app.js)
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/.env.example`](backend/.env.example)
  - [`frontend/src/app/pages/dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx)
  - [`frontend/src/app/pages/services/broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx)
  - [`frontend/src/app/pages/services/broadband/schema/columns.jsx`](frontend/src/app/pages/services/broadband/schema/columns.jsx)
  - [`frontend/src/app/navigation/dashboards.js`](frontend/src/app/navigation/dashboards.js)
  - [`radius-server/internal/transport/shared/trace.go`](radius-server/internal/transport/shared/trace.go) [NEW]
  - [`radius-server/internal/domain/ports/types.go`](radius-server/internal/domain/ports/types.go)
  - [`radius-server/pkg/radiusproto/messageauth.go`](radius-server/pkg/radiusproto/messageauth.go) [NEW]
  - [`radius-server/pkg/radiusproto/messageauth_test.go`](radius-server/pkg/radiusproto/messageauth_test.go) [NEW]
  - [`radius-server/pkg/radiusproto/decode.go`](radius-server/pkg/radiusproto/decode.go)
  - [`radius-server/pkg/radiusproto/encode.go`](radius-server/pkg/radiusproto/encode.go)
  - [`radius-server/internal/transport/pppoe/auth_listener.go`](radius-server/internal/transport/pppoe/auth_listener.go)
  - [`radius-server/internal/transport/pppoe/auth_listener_test.go`](radius-server/internal/transport/pppoe/auth_listener_test.go) [NEW]
  - [`radius-server/internal/transport/hotspot/auth_listener.go`](radius-server/internal/transport/hotspot/auth_listener.go)
  - [`radius-server/internal/transport/routerlogin/listener.go`](radius-server/internal/transport/routerlogin/listener.go)
  - [`radius-server/internal/domain/accounting/handlers.go`](radius-server/internal/domain/accounting/handlers.go)
  - [`radius-server/internal/domain/accounting/usage.go`](radius-server/internal/domain/accounting/usage.go)
  - [`radius-server/internal/domain/auth/authenticate.go`](radius-server/internal/domain/auth/authenticate.go)
  - [`radius-server/internal/domain/hotspot/usage.go`](radius-server/internal/domain/hotspot/usage.go)
  - [`radius-server/internal/repository/mongo/authentication_repo.go`](radius-server/internal/repository/mongo/authentication_repo.go)
  - [`radius-server/internal/repository/mongo/voucher_repo.go`](radius-server/internal/repository/mongo/voucher_repo.go)
  - [`radius-server/internal/transport/grpcclient/publisher.go`](radius-server/internal/transport/grpcclient/publisher.go)
  - [`radius-server/cmd/radiusd/wire.go`](radius-server/cmd/radiusd/wire.go)
  - [`radius-server/gen/radius/v1/radius.pb.go`](radius-server/gen/radius/v1/radius.pb.go)
  - [`radius-server/proto/radius/v1/radius.proto`](radius-server/proto/radius/v1/radius.proto)
  - [`radius-server/RENCANA_PENGUJIAN_LAB.md`](radius-server/RENCANA_PENGUJIAN_LAB.md) [NEW]
  - [`radius-server/.gitignore`](radius-server/.gitignore)
  - [`backend/src/services/fiberCable.service.js`](backend/src/services/fiberCable.service.js)
  - [`backend/src/services/radiusServerRegistry.service.js`](backend/src/services/radiusServerRegistry.service.js)
  - [`audit-report-issue-137.md`](audit-report-issue-137.md) _(Dihapus)_
  - [`audit-task-issue-137.md`](audit-task-issue-137.md) _(Dihapus)_
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
- **Deskripsi Perubahan & Fungsionalitas**:
  - **Backend — Radius Control**: Menambahkan controller [`radiusControl.controller.js`](backend/src/controllers/radiusControl.controller.js) dan route [`radiusControl.route.js`](backend/src/routes/radiusControl.route.js) baru untuk mengelola kontrol radius server (CoA/Disconnect). Menambahkan service [`radiusIdentity.service.js`](backend/src/services/radiusIdentity.service.js) untuk manajemen identitas radius.
  - **Backend — gRPC Stream**: Memperbarui [`streamRegistry.js`](backend/src/grpc/streamRegistry.js) dan [`radiusControl.handler.js`](backend/src/grpc/radiusControl.handler.js) untuk mendukung streaming real-time status koneksi PPPoE/Hotspot dari radius server ke backend.
  - **Backend — Socket.io**: Menambahkan event emitter di [`socket-io.js`](backend/src/sockets/socket-io.js) untuk push notifikasi real-time koneksi user ke frontend.
  - **Frontend — Dashboard Radius**: Memperbarui halaman [`dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx) untuk menampilkan data real-time koneksi radius.
  - **Frontend — Broadband Detail**: Memperbarui [`broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx) dan [`columns.jsx`](frontend/src/app/pages/services/broadband/schema/columns.jsx) untuk integrasi data radius pada detail layanan broadband.
  - **Radius Server — MessageAuth**: Menambahkan dukungan RFC 2869 Message-Authentication pada [`messageauth.go`](radius-server/pkg/radiusproto/messageauth.go) dengan test lengkap untuk verifikasi integritas paket RADIUS.
  - **Radius Server — Trace System**: Menambahkan sistem trace/logging baru di [`trace.go`](radius-server/internal/transport/shared/trace.go) untuk melacak alur autentikasi dan accounting di seluruh transport layer.
  - **Radius Server — PPPoE Auth**: Memperbarui [`auth_listener.go`](radius-server/internal/transport/pppoe/auth_listener.go) dengan peningkatan handling autentikasi PPPoE, termasuk test case baru.
  - \*\*Radius Server — Proto Updates]: Menambahkan field baru pada [`radius.proto`](radius-server/proto/radius/v1/radius.proto) untuk mendukung protokol yang lebih kaya.
  - **Dokumentasi**: Membuat [`RENCANA_PENGUJIAN_LAB.md`](radius-server/RENCANA_PENGUJIAN_LAB.md) berisi rencana pengujian lab untuk radius server.
  - **Cleanup**: Menghapus file audit lama (`audit-report-issue-137.md`, `audit-task-issue-137.md`).

---

## 🌿 Branch: `issue-146` — Manajemen Blacklist & Pelanggan Pasif

### 📌 Informasi Issue

- **Nomor Issue**: #146
- **Judul Issue**: Manajemen Blacklist & Pelanggan Pasif
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`d1f85dd`](issue-146) - `resolve #146` - 20 Juli 2026, 23:06

- **Komponen yang Berubah**:
  - [`backend/src/controllers/customer.controller.js`](backend/src/controllers/customer.controller.js)
  - [`backend/src/routes/customer.route.js`](backend/src/routes/customer.route.js)
  - [`backend/src/services/customer.service.js`](backend/src/services/customer.service.js)
  - [`backend/src/services/customerPartner.service.js`](backend/src/services/customerPartner.service.js)
  - [`backend/src/models/customer.model.js`](backend/src/models/customer.model.js)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`frontend/src/app/pages/users/blacklist/index.jsx`](frontend/src/app/pages/users/blacklist/index.jsx) [NEW]
  - [`frontend/src/app/pages/users/blacklist/schema/columns.jsx`](frontend/src/app/pages/users/blacklist/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/users/pasif/index.jsx`](frontend/src/app/pages/users/pasif/index.jsx) [NEW]
  - [`frontend/src/app/pages/users/pasif/schema/columns.jsx`](frontend/src/app/pages/users/pasif/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/users/customer/edit.jsx`](frontend/src/app/pages/users/customer/edit.jsx)
  - [`frontend/src/app/pages/users/customer/profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx)
  - [`frontend/src/app/router/users/blacklistRoute.jsx`](frontend/src/app/router/users/blacklistRoute.jsx) [NEW]
  - [`frontend/src/app/router/users/pasifRoute.jsx`](frontend/src/app/router/users/pasifRoute.jsx) [NEW]
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/navigation/users.js`](frontend/src/app/navigation/users.js)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
- **Deskripsi Perubahan & Fungsionalitas**:
  - **Backend — Customer Controller & Routes**: Menambahkan endpoint baru untuk manajemen blacklist dan pelanggan pasif pada [`customer.controller.js`](backend/src/controllers/customer.controller.js) dan [`customer.route.js`](backend/src/routes/customer.route.js). Termasuk operasi CRUD untuk status blacklist dan pasif.
  - **Backend — Customer Service**: Memperluas [`customer.service.js`](backend/src/services/customer.service.js) dengan query dan logic untuk filtering pelanggan berdasarkan status blacklist/pasif. Memperbarui [`customerPartner.service.js`](backend/src/services/customerPartner.service.js) untuk mendukung relasi pelanggan blacklist.
  - **Backend — Model Extensions**: Menambahkan field baru pada [`customer.model.js`](backend/src/models/customer.model.js) untuk mendukung status blacklist dan pasif.
  - **Frontend — Halaman Blacklist**: Membuat halaman baru [`blacklist/index.jsx`](frontend/src/app/pages/users/blacklist/index.jsx) dengan datatable untuk menampilkan daftar pelanggan yang diblacklist, termasuk kolom konfigurasi [`columns.jsx`](frontend/src/app/pages/users/blacklist/schema/columns.jsx).
  - **Frontend — Halaman Pasif**: Membuat halaman baru [`pasif/index.jsx`](frontend/src/app/pages/users/pasif/index.jsx) dengan datatable untuk menampilkan daftar pelanggan pasif, termasuk kolom konfigurasi [`columns.jsx`](frontend/src/app/pages/users/pasif/schema/columns.jsx).
  - **Frontend — Profil Pelanggan**: Memperbarui [`profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx) untuk menampilkan dan mengelola status blacklist/pasif pada profil pelanggan. Memperbarui [`edit.jsx`](frontend/src/app/pages/users/customer/edit.jsx) untuk form edit yang mendukung field baru.
  - **Frontend — Routing & Navigasi**: Menambahkan route baru [`blacklistRoute.jsx`](frontend/src/app/router/users/blacklistRoute.jsx) dan [`pasifRoute.jsx`](frontend/src/app/router/users/pasifRoute.jsx). Memperbarui [`navigation/users.js`](frontend/src/app/navigation/users.js) dan [`protected.jsx`](frontend/src/app/router/protected.jsx) untuk registrasi menu dan proteksi route.
  - **i18n**: Menambahkan 23 key terjemahan baru di [`translations.json`](frontend/src/i18n/locales/id/translations.json) untuk label dan pesan terkait blacklist dan pasif.

---

## 🌿 Branch: `issue-148` — Manajemen Hotspot User

### 📌 Informasi Issue

- **Nomor Issue**: #148
- **Judul Issue**: Manajemen Hotspot User
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`616973e`](master) - `resolve #148` - 20 Juli 2026, 13:39 (squash ke `5254129`)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/hotspotUser.controller.js`](backend/src/controllers/hotspotUser.controller.js) [NEW]
  - [`backend/src/routes/hotspotUser.route.js`](backend/src/routes/hotspotUser.route.js) [NEW]
  - [`backend/src/services/hotspotUser.service.js`](backend/src/services/hotspotUser.service.js) [NEW]
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/app.js`](backend/src/app.js)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`frontend/src/app/pages/services/hotspotUser/create.jsx`](frontend/src/app/pages/services/hotspotUser/create.jsx) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/detail.jsx`](frontend/src/app/pages/services/hotspotUser/detail.jsx) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/edit.jsx`](frontend/src/app/pages/services/hotspotUser/edit.jsx) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/index.jsx`](frontend/src/app/pages/services/hotspotUser/index.jsx) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/schema/columns.jsx`](frontend/src/app/pages/services/hotspotUser/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/schema/createSchema.js`](frontend/src/app/pages/services/hotspotUser/schema/createSchema.js) [NEW]
  - [`frontend/src/app/pages/services/hotspotUser/schema/editSchema.js`](frontend/src/app/pages/services/hotspotUser/schema/editSchema.js) [NEW]
  - [`frontend/src/app/router/services/hotspotUserRoute.jsx`](frontend/src/app/router/services/hotspotUserRoute.jsx) [NEW]
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/navigation/services.js`](frontend/src/app/navigation/services.js)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
  - [`audit-report-issue-137.md`](audit-report-issue-137.md) _(Dihapus)_
  - [`audit-task-issue-137.md`](audit-task-issue-137.md) _(Dihapus)_
- **Deskripsi Perubahan & Fungsionalitas**:
  - **Backend — Hotspot User**: Membuat modul lengkap untuk manajemen hotspot user meliputi [`hotspotUser.controller.js`](backend/src/controllers/hotspotUser.controller.js) (business logic & validasi), [`hotspotUser.route.js`](backend/src/routes/hotspotUser.route.js) (endpoint REST API), dan [`hotspotUser.service.js`](backend/src/services/hotspotUser.service.js) (data access layer). Modul ini mengelola CRUD hotspot user profile, termasuk autentikasi voucher/hotspot.
  - **Frontend — CRUD Halaman**: Membuat 4 halaman utama: [`index.jsx`](frontend/src/app/pages/services/hotspotUser/index.jsx) (daftar dengan datatable), [`create.jsx`](frontend/src/app/pages/services/hotspotUser/create.jsx) (form pembuatan), [`edit.jsx`](frontend/src/app/pages/services/hotspotUser/edit.jsx) (form edit), dan [`detail.jsx`](frontend/src/app/pages/services/hotspotUser/detail.jsx) (tampilan detail).
  - **Frontend — Schema & Validasi**: Membuat schema kolom tabel [`columns.jsx`](frontend/src/app/pages/services/hotspotUser/schema/columns.jsx), serta schema validasi Yup untuk create [`createSchema.js`](frontend/src/app/pages/services/hotspotUser/schema/createSchema.js) dan edit [`editSchema.js`](frontend/src/app/pages/services/hotspotUser/schema/editSchema.js).
  - **Frontend — Routing & Navigasi**: Menambahkan route [`hotspotUserRoute.jsx`](frontend/src/app/router/services/hotspotUserRoute.jsx) dan memperbarui [`navigation/services.js`](frontend/src/app/navigation/services.js) untuk menu Hotspot User.
  - **Privilege**: Menambahkan privilege baru untuk hotspot user di [`privilege.json`](backend/src/config/privilege.json).
  - **i18n**: Menambahkan 23 key terjemahan baru untuk modul hotspot user.
  - **Cleanup**: Menghapus file audit lama.

---

## 🌿 Branch: `issue-147` — Manajemen Radius Session

### 📌 Informasi Issue

- **Nomor Issue**: #147
- **Judul Issue**: Manajemen Radius Session
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`f599ac6`](master) - `resolve #147` - 20 Juli 2026, 12:17 (squash ke `61a4335`)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/radiusSession.controller.js`](backend/src/controllers/radiusSession.controller.js)
  - [`backend/src/routes/radiusSession.route.js`](backend/src/routes/radiusSession.route.js)
  - [`backend/src/services/radiusSession.service.js`](backend/src/services/radiusSession.service.js)
  - [`backend/src/utils/data-table.js`](backend/src/utils/data-table.js)
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`frontend/src/app/pages/network/radiusSession/index.jsx`](frontend/src/app/pages/network/radiusSession/index.jsx) [NEW]
  - [`frontend/src/app/pages/network/radiusSession/schema/columns.jsx`](frontend/src/app/pages/network/radiusSession/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/services/broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx)
  - [`frontend/src/app/pages/services/broadbandProfile/detail.jsx`](frontend/src/app/pages/services/broadbandProfile/detail.jsx)
  - [`frontend/src/app/router/network/radiusSession.jsx`](frontend/src/app/router/network/radiusSession.jsx) [NEW]
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/navigation/networks.js`](frontend/src/app/navigation/networks.js)
  - [`frontend/src/components/shared/Badge.jsx`](frontend/src/components/shared/Badge.jsx)
  - [`frontend/src/components/shared/form/FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx)
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
  - [`telegram-api/eslint.config.js`](telegram-api/eslint.config.js) [NEW]
  - [`telegram-api/.eslintrc.json`](telegram-api/.eslintrc.json) _(Dihapus)_
  - [`telegram-api/package.json`](telegram-api/package.json)
  - [`telegram-api/package-lock.json`](telegram-api/package-lock.json)
  - [`audit-report-issue-137.md`](audit-report-issue-137.md) _(Dihapus)_
  - [`audit-task-issue-137.md`](audit-task-issue-137.md) _(Dihapus)_
- **Deskripsi Perubahan & Fungsionalitas**:
  - **Backend — Radius Session**: Memperluas [`radiusSession.controller.js`](backend/src/controllers/radiusSession.controller.js) dengan endpoint tambahan untuk menampilkan data session aktif, history session, dan statistik. Memperbarui [`radiusSession.route.js`](backend/src/routes/radiusSession.route.js) dan [`radiusSession.service.js`](backend/src/services/radiusSession.service.js) dengan query lanjutan untuk filtering dan aggregasi data session.
  - **Backend — Data Table Utility**: Memperbarui [`data-table.js`](backend/src/utils/data-table.js) untuk mendukung fitur filtering baru yang dibutuhkan oleh tabel radius session.
  - **Frontend — Halaman Radius Session**: Membuat halaman baru [`radiusSession/index.jsx`](frontend/src/app/pages/network/radiusSession/index.jsx) di bawah modul Network untuk menampilkan daftar session radius aktif dan historis dengan kolom konfigurasi [`columns.jsx`](frontend/src/app/pages/network/radiusSession/schema/columns.jsx).
  - **Frontend — Broadband Detail Integration**: Memperbarui [`broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx) dan [`broadbandProfile/detail.jsx`](frontend/src/app/pages/services/broadbandProfile/detail.jsx) untuk menampilkan data radius session yang terkait dengan layanan broadband.
  - **Frontend — Shared Components**: Memperbarui [`Badge.jsx`](frontend/src/components/shared/Badge.jsx), [`FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx), dan [`rows.jsx`](frontend/src/components/shared/table/rows.jsx) untuk mendukung kebutuhan tampilan radius session.
  - **Frontend — Routing & Navigasi**: Menambahkan route [`radiusSession.jsx`](frontend/src/app/router/network/radiusSession.jsx) dan memperbarui [`navigation/networks.js`](frontend/src/app/navigation/networks.js) untuk menu Radius Session.
  - **Telegram API — ESLint Migration**: Migrasi konfigurasi ESLint dari `.eslintrc.json` ke [`eslint.config.js`](telegram-api/eslint.config.js) (flat config format) dan memperbarui dependencies.
  - **Privilege**: Menambahkan privilege untuk radius session di [`privilege.json`](backend/src/config/privilege.json).
  - **i18n**: Menambahkan 10 key terjemahan baru untuk modul radius session.
  - **Cleanup**: Menghapus file audit lama.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                           | Dampak Utama                                                                                                                           |
| ----- | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| #150  | Integrasi Radius Server & Kontrol PPPoE/Hotspot | Sistem radius server terintegrasi penuh dengan backend dan frontend; real-time monitoring koneksi; dukungan MessageAuth & trace system |
| #146  | Manajemen Blacklist & Pelanggan Pasif           | Halaman dan API baru untuk mengelola pelanggan blacklist dan pasif; ekstensi profil pelanggan                                          |
| #148  | Manajemen Hotspot User                          | Modul CRUD lengkap untuk hotspot user (create, read, update, delete) dengan validasi form                                              |
| #147  | Manajemen Radius Session                        | Halaman monitoring radius session aktif dan historis; integrasi data session ke detail layanan broadband                               |

### Kemampuan Baru Pengguna/Admin

- **Monitoring Koneksi Real-Time**: Admin dapat memantau koneksi PPPoE dan Hotspot secara real-time melalui dashboard radius yang terintegrasi dengan streaming data dari radius server via gRPC.
- **Kontrol Radius (CoA/Disconnect)**: Admin dapat melakukan Change of Authorization dan disconnect koneksi user secara remote melalui API kontrol radius.
- **Manajemen Blacklist**: Admin dapat memblokir pelanggan tertentu dengan status blacklist dan melihat daftar pelanggan yang diblacklist.
- **Pelanggan Pasif**: Admin dapat mengelola pelanggan dengan status pasif (tidak aktif sementara) dan memantau daftar pelanggan pasif.
- **CRUD Hotspot User**: Admin dapat membuat, melihat, mengedit, dan menghapus hotspot user profiles (voucher/hotspot authentication).
- **Monitoring Radius Session**: Admin dapat melihat daftar session radius aktif, history session, dan statistik penggunaan bandwidth per session.

### Bug Fix / Solusi Masalah

- Peningkatan reliability pada autentikasi PPPoE dengan penambahan dukungan Message-Authentication (RFC 2869) pada radius server untuk verifikasi integritas paket.
- Perbaikan stream registry dan gRPC handler untuk komunikasi yang lebih stabil antara radius server dan backend.
- Migrasi konfigurasi ESLint telegram-api ke flat config format (`.eslintrc.json` → `eslint.config.js`) untuk kompatibilitas dengan ESLint versi terbaru.

### Menu/Fitur Baru

- **Dashboard Radius** (`/dashboards/radius`): Halaman dashboard untuk monitoring real-time koneksi radius server.
- **Radius Session** (`/network/radius-session`): Halaman daftar session radius aktif dan historis.
- **Hotspot User** (`/services/hotspot-user`): Modul CRUD lengkap untuk manajemen hotspot user.
- **User Blacklist** (`/users/blacklist`): Halaman daftar pelanggan yang diblacklist.
- **User Pasif** (`/users/pasif`): Halaman daftar pelanggan dengan status pasif.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Fitur: Monitoring Radius Session & Dashboard Real-Time

- **Penjelasan Fitur**: Sistem monitoring radius session memungkinkan admin untuk melihat semua koneksi PPPoE dan Hotspot yang aktif secara real-time. Data dikirim dari radius server ke backend melalui gRPC streaming, kemudian didistribusikan ke frontend melalui Socket.io. Admin dapat melihat informasi seperti user yang terhubung, durasi koneksi, penggunaan bandwidth, dan status session.

- **Langkah Penggunaan (Tutorial)**:
  1. Akses menu **Dashboard → Radius** untuk melihat ringkasan koneksi aktif secara real-time.
  2. Akses menu **Network → Radius Session** untuk melihat daftar lengkap semua session radius (aktif dan historis).
  3. Gunakan filter pada tabel untuk mencari session berdasarkan user, NAS, atau status tertentu.
  4. Pada halaman **Services → Broadband → Detail**, scroll ke bagian radius session untuk melihat koneksi yang terkait dengan layanan broadband tertentu.
  5. Untuk manajemen hotspot user, akses **Services → Hotspot User** dan gunakan tombol "Tambah" untuk membuat user baru, atau klik nama user untuk melihat detail dan mengedit.
