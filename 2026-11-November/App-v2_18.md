# 📝 Daily Work Report - Dedy S.N Putra (2026-11-17)

---

## 📅 Laporan Harian - 17 November 2026

---

## 🌿 Branch: `master` (Integrasi `issue-556`) — Penyempurnaan Responsivitas Layout, Autentikasi Token, & Interceptor API pada `mobile-frontend`

### 📌 Informasi Issue

- **Nomor Issue**: #556
- **Judul Issue**: Penyempurnaan Tata Letak Responsif Tablet/Desktop, Pembersihan Penyimpanan Token & Validasi Profil Login, serta Penyelarasan Interceptor Bahasa/Auth di `mobile-frontend`
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `bfce3b6b`)

### 📅 Rincian Commit

#### [bfce3b6b] & [f2e31135] - resolve #556 - 17 November 2026, 17:46:25 WIB

- **Komponen yang Berubah**:
  - `mobile-frontend/src/App.jsx`
  - `mobile-frontend/src/components/ChunkLoadFailed.jsx`
  - `mobile-frontend/src/components/MobileShell.jsx`
  - `mobile-frontend/src/components/PwaUpdatePrompt.jsx`
  - `mobile-frontend/src/features/authSlice.js`
  - `mobile-frontend/src/features/authSlice.test.js`
  - `mobile-frontend/src/hooks/useConfig.js`
  - `mobile-frontend/src/i18n/locales/en/translations.json`
  - `mobile-frontend/src/i18n/locales/id/translations.json`
  - `mobile-frontend/src/pages/CustomersPage.jsx`
  - `mobile-frontend/src/pages/FinancePage.jsx`
  - `mobile-frontend/src/pages/HomePage.jsx`
  - `mobile-frontend/src/pages/LoginPage.jsx`
  - `mobile-frontend/src/pages/NetworkPage.jsx`
  - `mobile-frontend/src/pages/ProductsPage.jsx`
  - `mobile-frontend/src/utils/apiClient.js`
  - `mobile-frontend/src/utils/authStorage.js`

- **Deskripsi Perubahan & Fungsi**:

  1. **Penyempurnaan Responsivitas Shell & Halaman `mobile-frontend`**:
     - Memperluas batas lebar kontainer utama dan bottom navigation pada `MobileShell.jsx` dari `max-w-[430px]` menjadi `max-w-5xl` agar tampilan tetap proporsional di layar tablet/desktop, mengganti deteksi offline manual dengan hook `useOnline()`, mengoptimalkan kalkulasi menu dengan `useMemo`, serta menutup drawer saat tombol `Escape` ditekan.
     - Memperbarui `CustomersPage.jsx` agar daftar pelanggan tampil dalam grid 2 kolom pada layar `md` ke atas, serta menjadikan dialog detail pelanggan dapat ditutup lewat klik backdrop atau tombol `Escape` dengan posisi modal di tengah pada layar desktop.

  2. **Pembersihan Auth Storage, Validasi Profil Login, & Interceptor API**:
     - Menghapus penyimpanan `refreshToken` yang tidak digunakan di `authStorage.js` dan `authSlice.js`, serta menambahkan validasi hasil `normalizeUser` (`missing-profile`) pada thunk `login` beserta pembaruan unit test di `authSlice.test.js`.
     - Menyelaraskan header `client-language` pada `apiClient.js` dengan bahasa aktif `i18n.language`, memperketat pencocokan URL endpoint auth pada interceptor `401` (`path === ...` / `path.endsWith(...)`), dan memperbarui kunci terjemahan `en` & `id` beserta penyesuaian minor di `App.jsx`, `ChunkLoadFailed.jsx`, `PwaUpdatePrompt.jsx`, `useConfig.js`, serta halaman-halaman terkait.

---

## 🌿 Branch: `master` / `production` (Integrasi `issue-558`) — Pembuatan & Edit Permintaan Perubahan Layanan oleh Admin (`serviceChangeRequest` & `mobileServiceChange`) serta Inisialisasi `mcp-server` Read-Only

### 📌 Informasi Issue

- **Nomor Issue**: #558
- **Judul Issue**: Penambahan Fitur Buat & Edit Permintaan Perubahan Layanan oleh Admin (Web & Mobile) serta Pembangunan `mcp-server` Read-Only Berbasis Streamable HTTP
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` dan `origin/production` melalui commit `951bea6e`)

### 📅 Rincian Commit

#### [951bea6e] & [9365113b] - resolve #558 - 17 November 2026, 12:27:42 WIB

- **Komponen yang Berubah**:
  - **Backend Permintaan Perubahan Layanan (`backend/*`)**:
    - `backend/src/config/privilege.json`
    - `backend/src/config/privilegeDictionary.json`
    - `backend/src/controllers/customer.controller.js`
    - `backend/src/controllers/mobileServiceChange.controller.js`
    - `backend/src/controllers/serviceChangeRequest.controller.js`
    - `backend/src/locales/en/translation.json`
    - `backend/src/locales/id/translation.json`
    - `backend/src/models/serviceChangeWebRequest.model.js`
    - `backend/src/routes/mobileServiceChange.route.js`
    - `backend/src/routes/serviceChangeRequest.route.js`
    - `backend/src/server.js`
    - `backend/src/services/mobileServiceChange.service.js`
    - `backend/src/services/radiusAuthentication.service.js`
    - `backend/src/services/serviceChangeRequest.service.js`
    - `backend/src/utils/serviceChangeRequestInput.js`
    - `backend/src/utils/serviceChangeWebRequestIndexMigration.js` [NEW]
    - `backend/test/integration/mobileServiceChange.service.test.js`
    - `backend/test/integration/serviceChangeRequest.admin.test.js`
    - `backend/test/unit/serviceChangeRequestInput.test.js`
  - **Frontend Permintaan Perubahan Layanan (`frontend/*`)**:
    - `frontend/src/app/pages/mobileApp/serviceChange/components/CreateWebRequestModal.jsx` [NEW]
    - `frontend/src/app/pages/mobileApp/serviceChange/schema/adminCreateSchema.js` [NEW]
    - `frontend/src/app/pages/mobileApp/serviceChange/components/MobileRequestsTab.jsx`
    - `frontend/src/app/pages/mobileApp/serviceChange/components/ReviewDrawer.jsx`
    - `frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestDrawer.jsx`
    - `frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestsTab.jsx`
    - `frontend/src/i18n/locales/en/translations.json`
    - `frontend/src/i18n/locales/id/translations.json`
  - **Dekasimal V2 Read-Only MCP Server (`mcp-server/*`)**:
    - `mcp-server/.env.example` [NEW]
    - `mcp-server/README.md` [NEW]
    - `mcp-server/package.json` [NEW], `mcp-server/package-lock.json` [NEW]
    - `mcp-server/src/config.js` [NEW]
    - `mcp-server/src/backendClient.js` [NEW]
    - `mcp-server/src/swaggerClient.js` [NEW]
    - `mcp-server/src/repoInspector.js` [NEW]
    - `mcp-server/src/mcpServer.js` [NEW]
    - `mcp-server/src/server.js` [NEW]
    - `mcp-server/test/mcp.test.js` [NEW]

- **Deskripsi Perubahan & Fungsi**:

  1. **Pembuatan & Pembaruan Permintaan Perubahan Layanan oleh Admin (`serviceChangeRequest` & `mobileServiceChange`)**:
     - Menambahkan endpoint & logika backend untuk admin membuat permintaan perubahan layanan web secara langsung (`adminCreateServiceChangeRequest`), memperbarui data permintaan web dengan *optimistic locking* `__v` (`updateServiceChangeRequest`), serta memperbarui permintaan perubahan layanan dari aplikasi mobile (`updateMobileServiceChangeRequest`) termasuk dukungan perubahan `product` pada `respondServiceChangeRequest`.
     - Menambahkan `parseAdminCreateInput` & `parseUpdateInput` di `serviceChangeRequestInput.js`, migrasi indeks koleksi `serviceChangeWebRequestIndexMigration.js`, pembaruan hak akses di `privilege.json` & `privilegeDictionary.json`, serta cakupan pengujian unit dan integrasi.
     - Membangun modal `CreateWebRequestModal.jsx` beserta skema validasi `adminCreateSchema.js` dan memperbarui `WebRequestsTab.jsx`, `WebRequestDrawer.jsx`, `MobileRequestsTab.jsx`, serta `ReviewDrawer.jsx` di frontend agar admin dapat membuat dan menyunting permintaan langsung dari dashboard.

  2. **Pembangunan `mcp-server` Read-Only Berbasis Streamable HTTP**:
     - Membuat workspace baru `mcp-server/` sebagai remote MCP Server (*Model Context Protocol*) berbasis Streamable HTTP (`GET /health`, `POST /mcp`) yang membaca spesifikasi OpenAPI/Swagger backend secara dinamis.
     - Menyediakan 4 tools utama (`list_endpoints`, `get_endpoint_spec`, `call_backend_read` dengan sensor otomatis field sensitif seperti `password`/`token`/`secret`, serta `inspect_module_files` untuk memindai struktur `backend/src/`), lengkap dengan unit test `mcp.test.js`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #556  | Penyempurnaan Responsivitas, Auth Storage, & Interceptor `mobile-frontend` | Tampilan `mobile-frontend` lebih nyaman di layar tablet/desktop, token auth lebih bersih, validasi profil login lebih aman, dan header bahasa API sinkron dengan UI |
| #558  | Admin Create/Update Permintaan Perubahan Layanan & `mcp-server` Read-Only | Admin dapat membuat dan mengedit permintaan perubahan layanan langsung dari dashboard; tim/AI client kini memiliki remote MCP Server read-only untuk mengeksplorasi API & struktur modul backend |

### Kemampuan Baru Pengguna/Admin

- **Admin Helpdesk** dapat membuat permintaan perubahan layanan web baru untuk pelanggan tertentu melalui modal **Buat Permintaan** serta menyunting detail permintaan web maupun mobile yang sudah masuk.
- **Developer / AI Client** dapat terhubung ke **Dekasimal V2 MCP Server** (`mcp-server`) untuk mencari spesifikasi endpoint Swagger dan memanggil endpoint `GET` secara aman (read-only dengan redaksi field sensitif).

### Bug Fix / Solusi Masalah

- **Pencocokan Endpoint Auth & Header Bahasa di `mobile-frontend`**: Memperketat deteksi URL login/logout pada interceptor `401` agar tidak salah mencocokkan path parsial, menyelaraskan `client-language` dengan bahasa UI aktif, dan mencegah sesi login berhasil bila data profil tidak valid (`missing-profile`).
- **Indeks Koleksi `service_change_web_requests`**: Menambahkan utilitas migrasi indeks `serviceChangeWebRequestIndexMigration.js` saat startup server agar aturan indeks unik tetap konsisten.

### Menu/Fitur Baru

- Modal **Buat Permintaan Perubahan Layanan** (`CreateWebRequestModal`) serta aksi edit di drawer detail **Permintaan Perubahan Layanan** (`frontend`).
- Layanan baru **`mcp-server/`** (Remote MCP Server read-only berbasis Streamable HTTP).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Buat & Edit Permintaan Perubahan Layanan (`#558`)**: Memungkinkan admin mencatat permintaan perubahan paket/layanan atas nama pelanggan langsung dari panel admin (tanpa harus menunggu pelanggan mengisi form publik) serta memperbarui data permintaan secara aman dengan pengecekan versi (`__v`).
  - **Dekasimal V2 MCP Server (`#558`)**: Server MCP terpisah yang mengekspos spesifikasi endpoint backend dan akses baca (`GET`) terproteksi bearer token untuk membantu otomasi atau asisten AI.
- **Langkah Penggunaan (Tutorial)**:
  1. **Membuat Permintaan Perubahan Layanan oleh Admin**: Buka menu **Mobile App → Permintaan Perubahan Layanan**, pilih tab **Web**, klik tombol **Buat Permintaan**, pilih pelanggan dan layanan aktifnya, isi detail permintaan, lalu simpan.
  2. **Menjalankan `mcp-server`**: Masuk ke direktori `mcp-server/`, salin `.env.example` ke `.env`, isi `MCP_API_KEY` dan `BACKEND_TOKEN`, lalu jalankan `npm install && npm start` (endpoint tersedia di `http://localhost:3100/mcp`).
