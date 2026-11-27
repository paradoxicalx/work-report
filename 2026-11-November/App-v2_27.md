# 📝 Daily Work Report - Dedy S.N Putra (2026-11-27)

---

## 📅 Laporan Harian - 27 November 2026

---

## 🌿 Branch: `issue-592` — Fitur Peta Sebaran Pelanggan (Customer Map)

### 📌 Informasi Issue

- **Nomor Issue**: #592
- **Judul Issue**: Fitur Peta Sebaran Pelanggan (Customer Map)
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [6cdf792c] - resolve #592 - 2026-11-27 14:11:17

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/constants/customerMap.constant.js` [NEW]
  - `backend/src/controllers/customerMap.controller.js` [NEW]
  - `backend/src/data/aiNavigationRoutes.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/business.route.js`
  - `backend/src/routes/customer.route.js`
  - `backend/src/routes/customerPartner.route.js`
  - `backend/src/services/customerMap.service.js` [NEW]
  - `backend/src/services/operationalDashboard.service.js`
  - `backend/src/utils/coordinate.js` [NEW]
  - `backend/src/utils/customerMap.js` [NEW]
  - `backend/test/integration/customerMap.service.test.js` [NEW]
  - `backend/test/unit/cronSettings.service.test.js`
  - `backend/test/unit/customerMap.test.js` [NEW]
  - `frontend/src/app/navigation/users.js`
  - `frontend/src/app/pages/users/customerMap/index.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/constants.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/utils.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/utils.test.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/pinMarkup.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/pinMarkup.test.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/placeSearch.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/placeSearch.test.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/waitForPin.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/waitForPin.test.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/hooks/useCustomerMapData.js` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/CustomerMapTab.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/CustomerMapView.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/CustomerPopup.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/FilterCombobox.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/MapSearchBox.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/WithoutCoordinateModal.jsx` [NEW]
  - `frontend/src/app/pages/users/customerMap/components/mapIcons.js` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/users/customerMapRoute.jsx` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Membangun modul backend **Customer Map** (`customerMap.controller.js`, `customerMap.service.js`, `customerMap.constant.js`, `utils/customerMap.js`) yang menyediakan endpoint peta untuk tiga jenis pelanggan: pelanggan reguler (`/customer/map`), pelanggan mitra (`/customer-partner/map`), dan mitra bisnis (`/business/map`), lengkap dengan daftar pelanggan yang belum memiliki koordinat (`without_coordinate`).
  - Mengekstrak utilitas koordinat `parseCoordinate` dari `operationalDashboard.service.js` ke `backend/src/utils/coordinate.js` bersama fungsi jarak haversine `distanceMeters`.
  - Mendaftarkan route Swagger, kamus hak akses (`privilegeDictionary.json`), rute navigasi AI (`aiNavigationRoutes.json`), serta terjemahan ID/EN di backend dan frontend.
  - Membangun halaman frontend **Customer Map** di `frontend/src/app/pages/users/customerMap/` (rute `/users/customer-map`, navigasi di `navigation/users.js`) dengan fitur tab per jenis pelanggan, tampilan peta interaktif (`CustomerMapView`), popup info pelanggan (`CustomerPopup`), filter area/status (`FilterCombobox`), pencarian pelanggan & lokasi (`MapSearchBox`, `placeSearch.js`), serta modal daftar pelanggan tanpa koordinat (`WithoutCoordinateModal`).
  - Menambahkan integration test dan unit test komprehensif di backend (`customerMap.service.test.js`, `customerMap.test.js`) dan frontend (`utils.test.js`, `pinMarkup.test.js`, `placeSearch.test.js`, `waitForPin.test.js`).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue / Branch                    | Judul                                                           | Dampak Utama                                                                                                                |
| --------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `claude/frosty-chatterjee-9606b0` | Penyesuaian Unit Test `CRON_JOB_NAMES` (`oltAutoSync`)          | Unit test pengaturan cron kini lolos dan selaras dengan total 25 job scheduler termasuk `oltAutoSync`.                       |
| #592                              | Fitur Peta Sebaran Pelanggan (Customer Map)                     | Admin kini dapat memvisualisasikan sebaran lokasi seluruh pelanggan reguler, pelanggan mitra, dan mitra bisnis dalam satu peta. |

### Kemampuan Baru Pengguna/Admin

- Admin dapat membuka halaman **Customer Map** untuk melihat titik lokasi pelanggan reguler, pelanggan mitra, dan mitra bisnis di peta, menyaring berdasarkan area/status, mencari pelanggan atau tempat, serta meninjau daftar pelanggan yang belum memiliki koordinat.

### Bug Fix / Solusi Masalah

- Memperbaiki kegagalan unit test `cronSettings.service.test.js` yang masih mengharapkan 24 job setelah penambahan `oltAutoSync`.

### Menu/Fitur Baru

- Menu baru **Customer Map** di bawah grup navigasi **Users** (`/users/customer-map`), dengan hak akses `customer.list|customerPartner.list|business.list`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Modul **Customer Map** memetakan koordinat pelanggan aktif (non-blacklist, non-pasif) berdasarkan data pelanggan maupun koordinat layanannya (`RadAuth`), serta memisahkan data yang belum memiliki koordinat valid ke daftar khusus agar mudah ditindaklanjuti.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Users > Customer Map** di dashboard admin (`frontend`).
  2. Pilih tab jenis pelanggan (**Pelanggan Reguler**, **Pelanggan Mitra**, atau **Mitra Bisnis**) sesuai hak akses yang dimiliki.
  3. Gunakan kotak pencarian atau filter di bagian atas untuk menemukan pelanggan/area tertentu, klik pin pada peta untuk melihat detail pada popup, atau klik tombol daftar **Tanpa Koordinat** untuk melihat pelanggan yang belum memiliki titik koordinat.
