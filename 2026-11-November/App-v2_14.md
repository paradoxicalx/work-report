# 📝 Daily Work Report - Dedy S.N Putra (2026-11-14)

---

## 📅 Laporan Harian - 14 November 2026

---

## 🌿 Branch: `master` / `production` (Integrasi `issue-547`) — Finalisasi Restore Akun Broadband & RADIUS Non-Pelanggan (UI & Resolusi Konflik IP), Perbaikan Retry Pesan Baileys WhatsApp, & Merge `#547` ke Produksi

### 📌 Informasi Issue

- **Nomor Issue**: #547
- **Judul Issue**: Finalisasi Fitur Restore Akun Broadband & RADIUS Non-Pelanggan (Modal UI & Resolusi Konflik IP Statis), Cache Retry Pesan Baileys (`sentMessageCache`), In-Memory Auth Cache `radiusd` via gRPC, & Merge ke `master` / `production`
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` dan `origin/production` melalui commit `9b1bc574`)

### 📅 Rincian Commit

#### [9b1bc574] & [e786000d] - resolve #547 - 14 November 2026, 16:52:33 WIB

- **Komponen yang Berubah**:
  - **Baileys WhatsApp API (`baileys-api/*`)**:
    - `baileys-api/src/utils/sentMessageCache.js` [NEW]
    - `baileys-api/test/sentMessageCache.test.js` [NEW]
    - `baileys-api/src/services/authStateStore.js`
    - `baileys-api/src/services/sessionManager.service.js`
  - **Frontend Restore Akun RADIUS (`frontend/*`)**:
    - `frontend/src/components/shared/RestoreRadiusAuthModal.jsx` [NEW]
    - `frontend/src/components/shared/DeletedAccountNotice.jsx`
    - `frontend/src/app/pages/services/broadband/index.jsx`
    - `frontend/src/app/pages/services/broadband/detail.jsx`
    - `frontend/src/app/pages/services/broadband/schema/columns.jsx`
    - `frontend/src/app/pages/network/radiusNonCustomer/index.jsx`
    - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
    - `frontend/src/app/pages/network/radiusNonCustomer/schema/columns.jsx`
    - `frontend/src/i18n/locales/en/translations.json`
    - `frontend/src/i18n/locales/id/translations.json`
  - **Backend Restore & IP Reservation (`backend/*`)**:
    - `backend/src/services/networkIPv4Used.service.js`
    - `backend/src/services/radiusAuthentication.service.js`
    - `backend/src/controllers/radiusAuthentication.controller.js`
    - `backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`
    - `backend/src/routes/radiusAuthentication.route.js`
    - `backend/src/routes/radiusAuthenticationNonCustomer.route.js`
    - `backend/src/routes/oltConfig.route.js`
    - `backend/src/locales/en/translation.json`
    - `backend/src/locales/id/translation.json`
    - `backend/test/integration/radiusAuthenticationNonCustomer.delete.test.js`
  - **In-Memory Auth Cache `radiusd`, gRPC Keepalive, Status Code Controller, & Dokumentasi (bagian dari `#547` yang ikut di-merge ke `master`/`production`)**:
    - `AGENTS.md`, `documentations/CREATE_REPORT.md`
    - `backend/proto/radius/v1/radius.proto`, `radius-server/proto/radius/v1/radius.proto`, `radius-server/gen/radius/v1/radius.pb.go`
    - `backend/src/grpc/server.js`, `backend/src/grpc/streamRegistry.js`, `radius-server/internal/transport/grpcclient/client.go`
    - `backend/src/services/radiusAuthCache.service.js` [NEW], `backend/src/services/recovery.service.js`, `backend/test/unit/radiusAuthCache.service.test.js` [NEW]
    - `radius-server/cmd/radiusd/wire.go`, `radius-server/internal/repository/cache/cached_auth_repo.go` [NEW], `radius-server/internal/repository/cache/cached_auth_repo_test.go` [NEW]
    - `backend/src/controllers/ticket.controller.js`, `backend/test/integration/ticketStatusCodes.test.js` [NEW], `backend/test/unit/controllerStatusCode.test.js` [NEW]

- **Deskripsi Perubahan & Fungsi**:

  1. **Perbaikan Kegagalan Dekripsi / "Menunggu pesan ini..." pada Baileys API (`sentMessageCache`)**:
     - Menambahkan utilitas `sentMessageCache.js` (`createSentMessageCache`, `createUserDevicesCache`, `deduplicatePatchMessage`, `normalizeKeyStoreValue`, `runWithSocketMutex`) beserta unit test `sentMessageCache.test.js`.
     - Menyambungkan `getMessage` pada `makeWASocket` di `sessionManager.service.js` agar Baileys dapat mengambil ulang `proto.IMessage` dari cache pesan keluar (`fromMe`) saat perangkat penerima mengirim `<receipt type="retry">` setelah gagal mendekripsi pesan.
     - Menambahkan `patchMessageBeforeSending: deduplicatePatchMessage` agar satu JID tujuan tidak dienkripsi dua kali secara paralel dalam `Promise.all`, serta `userDevicesCache` untuk menjaga `device: 0` (HP utama) tetap tersedia lintas pengiriman 1:1 dan grup.
     - Menambahkan `normalizeKeyStoreValue` pada `authStateStore.js` saat membaca key store dari MongoDB.

  2. **UI & Resolusi Konflik IP Statis pada Restore Akun Broadband & RADIUS Non-Pelanggan**:
     - Membuat komponen modal bersama `RestoreRadiusAuthModal.jsx` yang dipakai di halaman daftar maupun detail **Broadband** (`services/broadband/*`) dan **RADIUS Non-Pelanggan** (`network/radiusNonCustomer/*`), serta memperbarui `DeletedAccountNotice.jsx` agar menampilkan tombol aksi pulihkan.
     - Jika proses restore awal mengembalikan HTTP `409` karena IP statis lama sudah dipakai akun lain, modal otomatis beralih ke langkah pemilihan **IP Pool & IP Statis pengganti** atau opsi **pulihkan tanpa IP statis** (`null` / `remove-data`).
     - Memperbarui `restoreAuthentication` & `restoreNonCustomerAuthentication` (beserta Swagger di `radiusAuthentication.route.js` & `radiusAuthenticationNonCustomer.route.js`) untuk menerima opsi `ip_pool` & `ip_static` pengganti, dan menambahkan `releaseStaticIpReservation` di `networkIPv4Used.service.js` sebagai kompensasi jika pemulihan gagal setelah reservasi dibuat.

  3. **Finalisasi & Merge `#547` ke `master` dan `production`**:
     - Memperbaiki kutipan deskripsi Swagger YAML pada `backend/src/routes/oltConfig.route.js`.
     - Mengintegrasikan seluruh paket `#547` (in-memory auth cache `radiusd` dengan invalidasi real-time gRPC + keepalive, restore akun soft-delete end-to-end, perbaikan `ticket.controller.js` status code 400, serta perbaikan Baileys `sentMessageCache`) ke branch `master` dan `production`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #547  | Finalisasi Restore Akun RADIUS (UI + Konflik IP), Baileys `sentMessageCache`, Auth Cache `radiusd`, & Deploy ke `master`/`production` | Admin dapat memulihkan akun Broadband & RADIUS Non-Pelanggan langsung dari UI termasuk saat IP statis bentrok; pesan WhatsApp Baileys tidak lagi nyangkut di "Menunggu pesan ini..." saat retry receipt; dan seluruh fitur `#547` resmi masuk ke `master` & `production` |

### Kemampuan Baru Pengguna/Admin

- Admin dapat memulihkan akun **Broadband** maupun **RADIUS Non-Pelanggan** yang ter-soft-delete langsung dari tabel daftar maupun halaman detail melalui modal `RestoreRadiusAuthModal`, termasuk memilih IP statis pengganti atau melepas IP statis apabila IP lama sudah digunakan akun lain.

### Bug Fix / Solusi Masalah

- **Pesan WhatsApp Nyangkut "Menunggu pesan ini..."**: Diatasi di `baileys-api` dengan menyimpan `proto.IMessage` pesan keluar pada `sentMessageCache` untuk callback `getMessage`, deduplikasi JID tujuan pada `patchMessageBeforeSending`, dan penjagaan `device: 0` di `userDevicesCache`.
- **Konflik IP Statis Saat Restore Akun**: Diatasi dengan dukungan payload `ip_pool` / `ip_static` pengganti (atau pelepasan IP statis) di backend serta alur pemulihan interaktif di frontend saat terjadi HTTP `409`, lengkap dengan kompensasi `releaseStaticIpReservation`.
- **Swagger YAML `oltConfig.route.js`**: Diperbaiki dengan mengapit string deskripsi respons `200` yang mengandung karakter titik dua (`:`) menggunakan tanda kutip ganda.

### Menu/Fitur Baru

- Aksi **Pulihkan Akun (Restore)** beserta modal konfirmasi & resolusi konflik IP pada:
  - **Layanan → Broadband** (halaman daftar & halaman detail akun terhapus)
  - **Jaringan → RADIUS Non-Pelanggan** (halaman daftar & halaman detail akun terhapus)

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Restore Akun RADIUS dengan Resolusi Konflik IP**: Akun Broadband atau RADIUS Non-Pelanggan yang sudah di-soft-delete dapat dikembalikan ke status aktif tanpa membuat ulang data dari nol. Jika IP statis sebelumnya masih tersedia dan pemilik akun masih aktif, akun langsung pulih. Jika IP statis tersebut sudah dipakai akun lain (HTTP `409`), admin langsung diarahkan di modal yang sama untuk memilih IP baru atau memulihkan akun tanpa IP statis.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Layanan → Broadband** atau **Jaringan → RADIUS Non-Pelanggan**, lalu aktifkan filter/tampilan akun terhapus (atau buka halaman detail akun yang berstatus terhapus).
  2. Klik tombol **Pulihkan** pada baris akun atau pada banner `DeletedAccountNotice`, lalu konfirmasi pada modal yang muncul.
  3. Jika muncul pemberitahuan konflik IP statis (409), pilih **IP Pool & IP Statis pengganti** yang tersedia atau centang opsi **Pulihkan tanpa IP statis**, lalu klik **Pulihkan** kembali.

