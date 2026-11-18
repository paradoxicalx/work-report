# 📝 Daily Work Report - Dedy S.N Putra (2026-11-18)

---

## 📅 Laporan Harian - 18 November 2026

---

## 🌿 Branch: `master` (Integrasi `issue-561`) — Modul Layanan RADIUS (`mobile-frontend`), Komponen/Hook Bersama, & Penyempurnaan Error/Field `networkIPv4`

### 📌 Informasi Issue

- **Nomor Issue**: #561
- **Judul Issue**: Implementasi Modul C: Layanan RADIUS (Akun PPPoE, Sesi Online, Profil RADIUS) di `mobile-frontend` serta Penyelarasan Response & Status Code `networkIPv4`
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `5ac852dc`)

### 📅 Rincian Commit

#### [5ac852dc] & [d28ddf3f] - resolve #561 - 18 November 2026, 19:14:34 WIB

- **Komponen yang Berubah**:
  - **Halaman & Komponen RADIUS (`mobile-frontend/src/pages/radius/*`)**:
    - `mobile-frontend/src/pages/RadiusPage.jsx`
    - `mobile-frontend/src/pages/radius/AccountDetailPage.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/AccountFormPage.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/ProfileDetailPage.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/ProfileFormPage.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/AccountsTab.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/ProfilesTab.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/SessionsTab.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/AccountInfoTab.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/AccountUsageTab.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/AccountHistoryTabs.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/SpeedFields.jsx` [NEW]
    - `mobile-frontend/src/pages/radius/components/useDisconnect.jsx` [NEW]
  - **Fitur, Service, Hook, & Komponen Bersama (`mobile-frontend/src/*`)**:
    - `mobile-frontend/src/features/radius/accountDisplay.js` [NEW], `accountPayload.js` [NEW], `accountSchema.js` [NEW], `accountStatus.js` [NEW], `cacheKeys.js` [NEW], `listSort.js` [NEW], `profilePayload.js` [NEW], `profileSchema.js` [NEW], `speedLimit.js` [NEW], `usage.js` [NEW], `radius.test.js` [NEW]
    - `mobile-frontend/src/services/radiusApi.js` [NEW], `mobile-frontend/src/services/endpoints.js`
    - `mobile-frontend/src/components/CollapsibleSection.jsx` [NEW], `CustomerPicker.jsx` [NEW], `InfoCard.jsx` [NEW], `InfoRow.jsx`, `LocationCard.jsx`, `form/PasswordField.jsx` [NEW], `form/SearchSelectField.jsx` [NEW], `form/fields.jsx`, `form/CoordinateField.jsx`
    - `mobile-frontend/src/hooks/useAutoRefresh.js` [NEW], `autoRefresh.test.js` [NEW], `useListParams.js` [NEW], `usePagedList.js`
    - `mobile-frontend/src/utils/money.js` [NEW], `password.js` [NEW], `time.js` [NEW], `time.test.js` [NEW]
    - `mobile-frontend/src/App.jsx`, `mobile-frontend/src/data/mockData.js`, `mobile-frontend/src/pages/customers/CustomerDetailPage.jsx`, `mobile-frontend/src/pages/customers/components/CustomerServicesTab.jsx`, `mobile-frontend/src/i18n/locales/en/translations.json`, `mobile-frontend/src/i18n/locales/id/translations.json`
    - `mobile-frontend/PRD.md`, `mobile-frontend/README.md`, `AGENTS.md`
  - **Backend `networkIPv4`**:
    - `backend/src/controllers/networkIPv4.controller.js`
    - `backend/src/services/networkIPv4.service.js`
    - `backend/src/services/networkIPv4Used.service.js`

- **Deskripsi Perubahan & Fungsi**:

  1. **Modul Layanan RADIUS Lengkap di `mobile-frontend` (`RadiusPage`, Akun PPPoE, Sesi Online, & Profil)**:
     - Mengimplementasikan halaman daftar bertab (`AccountsTab`, `ProfilesTab`, `SessionsTab`) dengan paginasi server, pencarian, filter status/mitra, dan *auto-refresh* opsional (`useAutoRefresh`).
     - Membangun halaman detail akun PPPoE (`AccountDetailPage`) dengan tab Informasi (`AccountInfoTab`), Pemakaian (`AccountUsageTab`), dan Riwayat (`AccountHistoryTabs`), serta fitur putus sesi online (*disconnect* via `useDisconnect`).
     - Membangun form tambah/ubah akun PPPoE (`AccountFormPage`) yang mendukung pemilihan pelanggan (`CustomerPicker`), paket/profil, IP Pool/IP Statis (`SearchSelectField`), serta form detail & tambah/ubah profil RADIUS (`ProfileDetailPage`, `ProfileFormPage`, `SpeedFields`).
     - Memusatkan logika payload, konversi satuan kecepatan Mikrotik & Partner API, skema validasi Yup, status akun, dan pemakaian di `features/radius/*` lengkap dengan unit test (`radius.test.js`, `autoRefresh.test.js`, `time.test.js`).

  2. **Penyelarasan Response & Error Handling `networkIPv4` di Backend**:
     - Menambahkan `pickIPv4Fields` di `networkIPv4.controller.js` untuk membatasi field respons `createIPv4` & `updateIPv4`, mengembalikan HTTP `409` pada duplikasi blok (`11000`) dan `500` untuk kegagalan tak terduga, serta helper `logAndThrow` di `networkIPv4.service.js`.

---

## 🌿 Branch: `master` (Integrasi `issue-566`) — Pengelompokan Scope Pemilik IPv4 (`partner` vs Internal), Partner API Blok IPv4, & Migrasi Scope Data

### 📌 Informasi Issue

- **Nomor Issue**: #566
- **Judul Issue**: Pemisahan Scope Kepemilikan Blok IPv4 (Internal vs Mitra), Endpoint Manajemen Blok IPv4 di Partner API (`/p-api/v1`), & Tool Migrasi Scope IPv4
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `540be53a`)

### 📅 Rincian Commit

#### [540be53a] & [b0a20860] - resolve #566 - 18 November 2026, 17:41:51 WIB

- **Komponen yang Berubah**:
  - **Backend IPv4 Scope & Partner API (`backend/*`)**:
    - `backend/src/models/networkIPv4.model.js`
    - `backend/src/models/networkIPv4Used.model.js`
    - `backend/src/utils/ip4.js`
    - `backend/src/controllers/networkIPv4.controller.js`
    - `backend/src/controllers/partnerApiIPv4.controller.js`
    - `backend/src/controllers/productDataAccess.controller.js`
    - `backend/src/controllers/productDedicatedInternet.controller.js`
    - `backend/src/routes/networkIPv4.route.js`
    - `backend/src/routes/partnerApi.route.js`
    - `backend/src/services/networkIPv4.service.js`
    - `backend/src/services/networkIPv4Used.service.js`
    - `backend/src/services/recovery.service.js`
    - `backend/scripts/migrateIpv4PartnerScope.js` [NEW]
    - `backend/src/locales/en/translation.json`, `backend/src/locales/id/translation.json`
    - `backend/test/integration/networkIPv4Owner.test.js`
    - `backend/test/integration/partnerApiIPv4.test.js`
    - `backend/test/integration/partnerApiRadius.test.js`
    - `backend/test/integration/olt.controller.test.js`, `backend/test/integration/oltOnt.controller.test.js`
  - **Frontend IPv4 Management & Recovery Tools (`frontend/*`)**:
    - `frontend/src/app/pages/network/ipv4Management/index.jsx`
    - `frontend/src/app/pages/network/ipv4Management/create.jsx`
    - `frontend/src/app/pages/network/ipv4Management/edit.jsx`
    - `frontend/src/app/pages/network/ipv4Management/components/PoolOwnerSelect.jsx`
    - `frontend/src/app/pages/developer/recoveryTools/recoveryRoutes.config.js`
    - `frontend/src/i18n/locales/en/translations.json`, `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:

  1. **Pemisahan Scope Kepemilikan IPv4 per Pemilik (`partner` vs Internal)**:
     - Memperbarui model `network_ipv4` dan `network_ipv4_used` sehingga keunikan blok dan reservasi IP berlaku per pemilik (`partner: null` untuk internal, atau `partner: ObjectId` untuk mitra), sehingga dua mitra berbeda dapat mengelola rentang IP lokal/privat yang sama tanpa saling bentrok.
     - Menambahkan helper validasi blok di `utils/ip4.js` dan memperbarui query/tree-rebuild di `networkIPv4.controller.js` & `networkIPv4.service.js`.

  2. **Endpoint Manajemen Blok IPv4 di Partner API (`partnerApiIPv4.controller.js`)**:
     - Menambahkan operasi manajemen blok IPv4 untuk aplikasi eksternal/mitra pada `/p-api/v1` (daftar, tambah, detail, ubah, hapus blok IPv4 dalam scope pemilik) dengan filter field aman (`PARTNER_APP_BLOCK_FIELDS`), dilengkapi integration test `partnerApiIPv4.test.js` dan `networkIPv4Owner.test.js`.

  3. **Migrasi Data Scope IPv4 & Pembaruan UI Frontend**:
     - Menambahkan `migrateIpv4PartnerScopeData` di `recovery.service.js`, script CLI `backend/scripts/migrateIpv4PartnerScope.js`, dan rute recovery di **Developer → Recovery Tools** (`recoveryRoutes.config.js`) untuk melakukan backfill field `partner` serta membersihkan indeks unik global lama.
     - Memperbarui halaman **Network → IPv4 Management** (`index.jsx`, `create.jsx`, `edit.jsx`, `PoolOwnerSelect.jsx`) agar pengelompokan dan filter pemilik konsisten di UI admin.

---

## 🌿 Branch: `master` (Integrasi `issue-559`) — Penyempurnaan Filter & Status Pelanggan pada `mobile-frontend`

### 📌 Informasi Issue

- **Nomor Issue**: #559
- **Judul Issue**: Penyempurnaan Filter Pelanggan, Normalisasi Hitungan Status (`parseListStatusCounts`), & Penyesuaian UI Detail/Form Pelanggan di `mobile-frontend`
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `68c6b045`)

### 📅 Rincian Commit

#### [68c6b045] & [b6eb2da0] - resolve #559 - 18 November 2026, 16:33:11 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `mobile-frontend/package-lock.json`
  - `mobile-frontend/src/components/MobileShell.jsx`
  - `mobile-frontend/src/components/PartnerPicker.jsx`
  - `mobile-frontend/src/components/form/CoordinateField.jsx`
  - `mobile-frontend/src/components/map/MapPickerSheet.jsx`
  - `mobile-frontend/src/configs/menu.js`
  - `mobile-frontend/src/features/authSlice.test.js`
  - `mobile-frontend/src/features/customers/customerPayload.js`, `customerSchema.js`, `customerStatus.js`, `customers.test.js`
  - `mobile-frontend/src/hooks/usePagedList.js`
  - `mobile-frontend/src/i18n/locales/en/translations.json`, `mobile-frontend/src/i18n/locales/id/translations.json`
  - `mobile-frontend/src/pages/CustomersPage.jsx`
  - `mobile-frontend/src/pages/customers/CustomerDetailPage.jsx`, `CustomerFormPage.jsx`, `components/CustomerDocumentsTab.jsx`
  - `mobile-frontend/src/services/customerApi.js`, `mobile-frontend/src/services/endpoints.js`, `mobile-frontend/src/utils/dataTable.js`

- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan `parseListStatusCounts` di `customerStatus.js` (beserta unit test di `customers.test.js`) untuk menormalisasi respons `/list-status` dari pelanggan `regular`, `partner`, maupun `business` menjadi `{ total, active, inactive }`.
  - Memperbarui `CustomersPage.jsx` dengan panel filter `BottomSheet` (jenis pelanggan, status, pemilih mitra `PartnerPicker`), serta menyempurnakan `CustomerDetailPage.jsx`, `CustomerFormPage.jsx`, `CustomerDocumentsTab.jsx`, `customerApi.js`, `usePagedList.js`, dan dokumentasi aturan `mobile-frontend` di `AGENTS.md`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #561  | Modul Layanan RADIUS di `mobile-frontend` & `networkIPv4` | Mitra dan admin kini dapat mengelola akun PPPoE, memantau sesi online (termasuk *disconnect*), serta mengelola profil RADIUS langsung dari aplikasi web *mobile-first* |
| #566  | Scope Kepemilikan IPv4 per Pemilik & Partner API IPv4 | Blok IPv4 kini terisolasi per pemilik (Internal vs Mitra) sehingga rentang IP antar mitra tidak saling bentrok; Partner API kini mendukung manajemen blok IPv4 |
| #559  | Penyempurnaan Filter & Status Pelanggan di `mobile-frontend` | Daftar pelanggan di `mobile-frontend` mendukung filter jenis, status, dan mitra melalui `BottomSheet` serta ringkasan hitungan status yang akurat |

### Kemampuan Baru Pengguna/Admin

- **Mitra & Admin (`mobile-frontend`)** dapat melihat, menambah, mengubah, mengaktifkan/menonaktifkan, dan memutus sesi akun PPPoE serta mengelola profil RADIUS dari perangkat mobile.
- **Admin Jaringan & Mitra (`/p-api/v1`)** dapat mengelola blok dan sub-blok IPv4 dalam scope kepemilikannya masing-masing tanpa konflik rentang IP lintas mitra.
- **Developer** dapat menjalankan migrasi scope pemilik IPv4 melalui CLI (`migrateIpv4PartnerScope.js`) maupun halaman **Developer → Recovery Tools**.

### Bug Fix / Solusi Masalah

- **Konflik Rentang IPv4 Antar Mitra**: Diatasi dengan mengubah indeks unik `network_ipv4` dan `network_ipv4_used` menjadi komposit per `partner` serta menyediakan tool migrasi `migrateIpv4PartnerScopeData`.
- **Respons & Status Code `networkIPv4`**: `createIPv4` dan `updateIPv4` kini membatasi field respons via `pickIPv4Fields` dan mengembalikan status `409` untuk duplikasi blok.

### Menu/Fitur Baru

- Modul **Layanan RADIUS** lengkap di `mobile-frontend` (tab **Akun PPPoE**, **Profil**, **Sesi Online**, halaman detail, dan form tambah/ubah).
- Opsi migrasi **Scope Pemilik IPv4** di **Developer → Recovery Tools**.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Modul Layanan RADIUS (`mobile-frontend`, `#561`)**: Menyediakan antarmuka mobile untuk seluruh siklus pengelolaan layanan PPPoE (pembuatan akun, pemilihan IP Pool/IP Statis, monitoring sesi aktif, pemutusan sesi, dan pengaturan limit kecepatan profil RADIUS) melalui `/p-api/v1/*`.
  - **Scope Kepemilikan IPv4 (`#566`)**: Memastikan setiap blok IPv4 dan reservasi IP terikat ke pemiliknya (`partner` atau internal), sehingga pengelolaan IP oleh mitra tidak mengganggu blok internal maupun mitra lain.
- **Langkah Penggunaan (Tutorial)**:
  1. **Mengelola Akun PPPoE di `mobile-frontend`**: Buka tab **RADIUS** di navigasi bawah, pilih tab **Akun**, **Sesi**, atau **Profil**, lalu ketuk **+** untuk membuat akun baru atau ketuk kartu akun untuk melihat detail, grafik pemakaian, dan memutus sesi online.
  2. **Menjalankan Migrasi Scope IPv4 (sekali jalan saat deploy)**: Jalankan `node scripts/migrateIpv4PartnerScope.js` di folder `backend/`, atau gunakan tombol **Dry Run** / **Eksekusi** pada baris migrasi scope IPv4 di **Developer → Recovery Tools**.
