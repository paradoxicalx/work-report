# 📝 Daily Work Report - Dedy S.N Putra (2026-07-17)

---

## 📅 Laporan Harian - 17 Juli 2026

---

## 🌿 Branch: `master` / `issue-147` — Perbaikan Autentikasi Telegram Mini App

### 📌 Informasi Issue

- **Nomor Issue**: #144
- **Judul Issue**: Perbaikan Autentikasi Telegram Mini App
- **Status Branch**: `Sudah di-merge` ke master

### 📅 Rincian Commit

#### [a904c8a] - resolve #144 - 17 Juli 2026, 09:49

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/sockets/socket-io.js`
  - `telegram-apps/src/components/ProtectedRoute.jsx`
  - `telegram-apps/src/context/AuthContext.jsx`
  - `telegram-apps/src/lib/axiosClient.js`
  - `telegram-apps/src/pages/Unauthorized.jsx`
  - `telegram-apps/src/routes/index.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Perbaikan alur autentikasi Telegram Mini App** pada sisi backend dan frontend:
    - **Backend**: Konfigurasi ulang route CORS dan socket.io agar kompatibel dengan mekanisme autentikasi Telegram yang baru.
    - **Frontend (Telegram Apps)**:
      - **ProtectedRoute.jsx**: Perbaikan logika pengecekan autentikasi — menambahkan penanganan redirect yang tepat saat user belum terautentikasi atau session expired.
      - **AuthContext.jsx**: Peningkatan signifikan pada konteks autentikasi — menambahkan mekanisme refresh token, penanganan error yang lebih robust, dan validasi `initData` Telegram secara lebih ketat.
      - **axiosClient.js**: Penyesuaian header dan interceptor agar mengirim Telegram auth data secara konsisten.
      - **Unauthorized.jsx**: Redesign halaman "Unauthorized" dengan UI yang lebih informatif, menampilkan panduan kepada user tentang cara mengakses aplikasi melalui Telegram Bot yang benar.
      - **routes/index.jsx**: Penyesuaian route guard untuk mendukung alur autentikasi yang baru.

---

## 🌿 Branch: `issue-147` — Daftar Sesi RADIUS (Work In Progress)

### 📌 Informasi Issue

- **Nomor Issue**: #147
- **Judul Issue**: Daftar Sesi RADIUS — Manajemen Sesi Online/Offline Pengguna
- **Status Branch**: `Belum di-merge` (sedang dalam pengembangan)

### 📅 Rincian Perubahan (Uncommitted Changes)

Perubahan ini merupakan fitur baru **Radius Session List** yang memungkinkan admin melihat dan memantau seluruh sesi koneksi RADIUS pengguna (online & offline) dengan data yang terpopulate.

#### Modifikasi Backend

**`backend/src/config/privilege.json`**

- Menambahkan hak akses baru `radiusSession.list` dengan kode `RADIUSSESSION_LIST` untuk mengontrol akses halaman daftar sesi RADIUS.

**`backend/src/controllers/radiusSession.controller.js`**

- Menambahkan controller baru `listRadiusSessions` yang menangani request pengambilan data sesi RADIUS untuk halaman DataTable.
- Controller mengembalikan data dengan format standar DataTable (pagination, sorting, filtering).

**`backend/src/services/radiusSession.service.js`**

- Menambahkan fungsi `findRadiusSessionsForTable` dengan kemampuan:
  - **Populate relasi** — mengambil data `user` (termasuk `customer` dan `partner`) serta `profile` broadband untuk setiap sesi.
  - **Pencarian global & filtering** — mendefinisikan kolom non-sensitif (`sessionID`, `isOnline`, `accounting`, `startTime`, `endTime`, `last_update`, `created_at`) untuk pencarian.
  - **Konsolidasi logging** — menggabungkan definisi kolom logs dan sessions ke dalam konstanta terpusat (`RADIUS_SESSION_NON_SENSITIVE` dan `RADIUS_LOGS_NON_SENSITIVE`).
- Perbaikan `findRadiusLogsForTable` — menggunakan model `RadLogs` yang lebih konsisten dan pesan error yang sesuai.

**`backend/src/routes/radiusSession.route.js`**

- Menambahkan endpoint baru `POST /api/v1/radius-sessions/list` dengan:
  - Middleware `protectedAdmin` dan `checkPrivilege('radiusSession.list')`.
  - Dokumentasi Swagger lengkap termasuk request body schema, response codes, dan contoh payload.

**`backend/src/locales/en/translation.json`**

- Menambahkan pesan error `radius.session.getListFailed` dan `radius.session.failedGetList` untuk penanganan error yang lebih jelas.

**`backend/src/utils/data-table.js`**

- Menambahkan `.allowDiskUse(true)` pada query MongoDB untuk menangani dataset besar yang melebihi batas memori sort, meningkatkan stabilitas dan performa.

#### Modifikasi Frontend

**`frontend/src/app/navigation/networks.js`**

- Menambahkan item navigasi **"Radius Session"** di bawah menu **Jaringan** (Networks) dengan:
  - Path: `/network/radius-sessions`
  - Ikon: `RadioIcon` (Hero Icons)
  - Hak akses: `radiusSession.list`

**`frontend/src/app/router/protected.jsx`**

- Mengimpor dan mendaftarkan route `radiusSessionRoute` ke dalam daftar protected routes.

**`frontend/src/components/shared/table/rows.jsx`**

- Perbaikan pada komponen `OnlineCell` — menambahkan fallback `?? false` pada `getValue()` untuk mencegah error runtime jika data `isOnline` bernilai `null` atau `undefined`.

**`frontend/src/i18n/locales/en/translations.json` & `id/translations.json`**

- Menambahkan terjemahan untuk:
  - Navigasi: `radiusSession` → "Radius Session" / "Sesi Radius"
  - Halaman: `radiusSessionList` → "Radius Session List" / "Daftar Sesi Radius"
  - Label kolom: `sessionID`, `statusOnline`, `accounting`, `startTime`, `endTime`, `lastUpdate` (dalam Bahasa Inggris dan Indonesia)

#### File Baru (NEW)

**`frontend/src/app/pages/network/radiusSession/index.jsx`**

- Halaman utama **Radius Session List** yang menggunakan komponen `Datatables` bawaan dengan:
  - Default sorting: sesi online di atas, diurutkan berdasarkan waktu terbaru.
  - Selected key: `sessionID`.
  - API endpoint: `/radius-sessions/list`.

**`frontend/src/app/pages/network/radiusSession/schema/columns.jsx`**

- Konfigurasi kolom tabel yang menampilkan:
  - **Select** (checkbox untuk bulk action)
  - **Status Online** — badge indikator online/offline dengan filter select.
  - **Session ID** — ID unik sesi RADIUS.
  - **Username** — link langsung ke halaman detail autentikasi broadband (menggunakan `LinkBadge`).
  - **Customer/Partner** — nama customer atau partner dengan link ke halaman profil (menggunakan `LinkBadge`).
  - **Profile** — nama broadband profile dengan drawer detail (menggunakan `BroadbandProfileDetailDrawer`).
  - **Connected (startTime)**, **Ended (endTime)**, **Updated (last_update)** — kolom tanggal dengan filter date range.

**`frontend/src/app/router/network/radiusSession.jsx`**

- Definisi route untuk halaman Radius Session dengan lazy loading dan privilege check `radiusSession.list`.

#### File Dihapus (CLEANUP)

- `audit-report-issue-137.md` — Laporan audit issue #137 (cleanup dari branch sebelumnya).
- `audit-task-issue-137.md` — Daftar tugas audit issue #137 (cleanup dari branch sebelumnya).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #144 | Perbaikan Autentikasi Telegram Mini App | Alur autentikasi Telegram Mini App diperbaiki — token handling lebih robust, redirect unauthenticated lebih jelas, dan halaman unauthorized lebih informatif |
| #147 | Daftar Sesi RADIUS | Fitur baru memantau seluruh sesi koneksi RADIUS pengguna (online/offline) dengan data customer, partner, dan profile yang terpopulate |

### Kemampuan Baru Pengguna/Admin

- Admin dapat **memantau sesi koneksi aktif (online) dan riwayat sesi (offline)** seluruh pengguna broadband dari satu halaman terpadu.
- Admin dapat **mengklik langsung username** pada tabel sesi untuk melihat detail autentikasi broadband pengguna.
- Admin dapat **mengklik nama customer/partner** pada tabel sesi untuk melihat profil pengguna.
- Admin dapat **melihat detail broadband profile** langsung dari kolom profile menggunakan drawer.
- User Telegram Mini App mendapatkan **pengalaman autentikasi yang lebih stabil** dengan penanganan error yang lebih jelas.

### Bug Fix / Solusi Masalah

- **Fix null pointer pada `OnlineCell`** — komponen indikator online/offline sekarang menangani data `null`/`undefined` tanpa error runtime.
- **Fix `allowDiskUse` pada DataTable** — query MongoDB dengan dataset besar tidak lagi gagal karena batas memori sorting.
- **Fix autentikasi Telegram Mini App** — session handling, token refresh, dan redirect flow telah diperbaiki untuk mencegah user terjebak di halaman unauthorized.

### Menu/Fitur Baru

- **Menu "Sesi Radius"** di bawah kategori **Jaringan** (Networks) — menyediakan akses ke halaman daftar sesi RADIUS dengan tabel interaktif lengkap (sorting, filtering, pencarian, paginasi).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Halaman **Radius Session List** adalah fitur pemantauan real-time untuk semua sesi koneksi RADIUS pengguna broadband. Setiap baris merepresentasikan satu sesi koneksi, lengkap dengan informasi status (online/offline), ID sesi, username, data customer/partner, broadband profile, waktu koneksi, waktu putus, dan waktu pembaruan terakhir. Data diambil dari MongoDB dengan populate relasi untuk menampilkan informasi terkait dalam satu tampilan.

- **Langkah Penggunaan (Tutorial)**:
  1. Login ke panel admin Dekasimal V2.
  2. Buka menu **Jaringan** → **Sesi Radius** di sidebar.
  3. Tabel akan menampilkan seluruh sesi RADIUS, dengan sesi **online berada di urutan paling atas**.
  4. Gunakan **filter** pada setiap kolom untuk menyaring data — misalnya filter "Status Online" untuk melihat hanya sesi aktif, atau filter tanggal pada "Connected" untuk rentang waktu tertentu.
  5. Gunakan **pencarian global** di atas tabel untuk mencari berdasarkan session ID, username, atau data lainnya.
  6. **Klik nama username** untuk langsung menuju halaman detail autentikasi broadband pengguna.
  7. **Klik nama customer/partner** untuk melihat profil pengguna.
  8. **Klik nama profile** pada kolom Profile untuk membuka drawer detail broadband profile.
