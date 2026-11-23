# 📝 Daily Work Report - Dedy S.N Putra (2026-11-23)

---

## 📅 Laporan Harian - 23 November 2026

---

## 🌿 Branch: `issue-580` — Optimasi Peta Infrastruktur Mobile (`bbox`), DTO Kabel & Aksesibilitas Peta

### 📌 Informasi Issue

- **Nomor Issue**: #580
- **Judul Issue**: Optimasi Peta Infrastruktur Mobile (`bbox`), DTO Kabel & Aksesibilitas Peta
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [ea55855f] - resolve #580 - 2026-11-23 19:00:25

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiMap.controller.js`
  - `backend/src/controllers/partnerApiNetworkDevice.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/locationPoint.service.js`
  - `backend/src/services/networkDevice.service.js`
  - `backend/test/integration/partnerApiMap.test.js`
  - `backend/test/integration/partnerApiNetworkDevice.test.js`
  - `mobile-frontend/src/components/map/NetworkMap.jsx`
  - `mobile-frontend/src/features/network/mapBounds.js` [NEW]
  - `mobile-frontend/src/features/network/mapBounds.test.js` [NEW]
  - `mobile-frontend/src/i18n/locales/en/translations.json`
  - `mobile-frontend/src/i18n/locales/id/translations.json`
  - `mobile-frontend/src/pages/network/DeviceFormPage.jsx`
  - `mobile-frontend/src/pages/network/NodeFormPage.jsx`
  - `mobile-frontend/src/pages/network/SiteFormPage.jsx`
  - `mobile-frontend/src/pages/network/components/DevicesTab.jsx`
  - `mobile-frontend/src/pages/network/components/MapTab.jsx`
  - `mobile-frontend/src/pages/network/components/SitesTab.jsx`
  - `mobile-frontend/src/services/networkApi.js`
  - `mobile-frontend/src/utils/keyboardActivate.js` [NEW]
  - `mobile-frontend/src/utils/keyboardActivate.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan dukungan filter batas wilayah `bbox` pada endpoint Partner API `listPartnerAppMapCables` (menggunakan `parseBbox` yang sama dengan API web) serta mode proyeksi `forPartnerApi` pada `getMapMarkers` dan `listFiberCables`.
  - Menerapkan `toCableDto` pada `readPartnerAppMapCable` untuk menyaring field internal (`partner`, `updated_by`, `slack_loops`) sebelum dikirim ke klien Partner API setelah pemeriksaan akses `canReadMapDoc`.
  - Mengoptimalkan `MapTab.jsx` dan `NetworkMap.jsx` di `mobile-frontend` menggunakan utilitas baru `mapBounds.js` (`bboxParam`, `boxContains`, `padBox`) dan debounce 400ms agar data kabel hanya dimuat untuk wilayah viewport peta yang tampil (ditambah padding) dan tidak memuat ulang saat geser kecil.
  - Menambahkan utilitas `keyboardActivate.js` (`activateOnKey`) untuk aktivasi keyboard yang konsisten pada kartu daftar perangkat, site, dan kabel, serta memperbarui terjemahan dan menambahkan unit/integration test (`mapBounds.test.js`, `keyboardActivate.test.js`, `partnerApiMap.test.js`, `partnerApiNetworkDevice.test.js`).

---

## 🌿 Branch: `issue-583` — Perbaikan Format Burst Profil Turunan Mitra & Alat Recovery Burst

### 📌 Informasi Issue

- **Nomor Issue**: #583
- **Judul Issue**: Perbaikan Format Burst Profil Turunan Mitra & Alat Recovery Burst
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [72cb0fe8] - resolve #583 - 2026-11-23 18:15:50

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/radiusProfile.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/routes/radiusProfile.route.js`
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/derivedProfileBurstFix.js` [NEW]
  - `backend/src/utils/mikrotikBurst.js` [NEW]
  - `backend/src/utils/partnerProductInput.js`
  - `backend/test/integration/derivedProfileBurstRecovery.test.js` [NEW]
  - `backend/test/unit/partnerProductInput.burst.test.js` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/components/ReviewRequestDrawer.jsx`
  - `frontend/src/app/pages/services/partnerPackage/utils/burstDetail.js` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/utils/burstDetail.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan utilitas `mikrotikBurst.js` dan memperbarui `partnerProductInput.js` untuk menyusun serta memvalidasi string `speed_limit` Mikrotik lengkap dengan parameter burst.
  - Menambahkan endpoint & service alat pemulihan data `recoveryDerivedProfileBurst` (`recoveryDerivedProfileBurstData` di `recovery.service.js` dengan utilitas `derivedProfileBurstFix.js`) untuk memperbaiki profil RADIUS turunan mitra (`is_derived: true, use_burst: true`) yang `speed_limit`-nya hanya berisi max-limit akibat format `speed_option` lama, lengkap dengan mode `dryRun` dan konfirmasi `'RECOVERY'`.
  - Mendaftarkan rute recovery baru pada `recoveryRoutes.config.js` serta menambahkan utilitas `burstDetail.js` untuk menampilkan detail konfigurasi burst (`burstLimit`, `burstThreshold`, `limitAt`, `burstTime`, `priority`) pada `ReviewRequestDrawer.jsx` di frontend.
  - Menambahkan unit & integration test (`derivedProfileBurstRecovery.test.js`, `partnerProductInput.burst.test.js`, `burstDetail.test.js`), terjemahan ID/EN, dan memperbarui dokumentasi di `AGENTS.md`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                    | Dampak Utama                                                                                                                |
| ----- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| #580  | Optimasi Peta Infrastruktur Mobile (`bbox`), DTO Kabel & Aksesibilitas   | Pemuatan kabel di peta mobile jauh lebih ringan karena dibatasi viewport (`bbox`), serta respon detail kabel Partner API lebih aman. |
| #583  | Perbaikan Format Burst Profil Turunan Mitra & Alat Recovery Burst        | Profil RADIUS turunan mitra dengan burst kini membentuk string `speed_limit` Mikrotik yang valid dan tersedia alat recovery datanya. |

### Kemampuan Baru Pengguna/Admin

- Admin Developer dapat menjalankan alat recovery `recoveryDerivedProfileBurst` (dengan pratinjau `dryRun` dan konfirmasi eksekusi) untuk memperbaiki profil RADIUS turunan mitra yang konfigurasi burst-nya belum masuk ke `speed_limit`.
- Admin dapat melihat detail parameter burst secara lengkap (termasuk `limitAt`, `burstTime`, dan `priority`) pada drawer review permintaan paket mitra (`ReviewRequestDrawer`).

### Bug Fix / Solusi Masalah

- Mengatasi masalah profil RADIUS turunan mitra (`use_burst: true`) yang sebelumnya tidak mengirimkan parameter burst ke Mikrotik karena `speed_limit` hanya menyimpan max-limit dan burst tersimpan di format `speed_option` lama.
- Mengatasi beban unduhan data kabel yang terlalu besar di `mobile-frontend` dengan memuat kabel berdasarkan wilayah viewport peta (`bbox`) beserta penyangga padding dan debounce.
- Memastikan field internal (`partner`, `updated_by`, `slack_loops`) tidak terekspos pada endpoint `readPartnerAppMapCable`.

### Menu/Fitur Baru

- Opsi alat pemulihan data baru untuk perbaikan burst profil turunan mitra pada bagian **Recovery Tools** di Pengaturan Developer (`recoveryRoutes.config.js`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Alat recovery burst profil turunan mitra memindai profil broadband turunan (`is_derived: true, use_burst: true`) yang `speed_limit`-nya belum memuat parameter burst, lalu menyusun ulang `speed_limit` berformat Mikrotik dari konfigurasi burst yang ada secara aman dan idempoten.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengaturan > Developer > Recovery Tools** di dashboard admin.
  2. Pilih alat recovery **Perbaikan Burst Profil Turunan Mitra**.
  3. Jalankan terlebih dahulu dalam mode **Dry Run** untuk meninjau jumlah profil yang bisa diperbaiki otomatis (`fixable`) maupun yang perlu ditangani manual (`manual`).
  4. Jika hasil pratinjau sudah sesuai, masukkan konfirmasi `RECOVERY` lalu jalankan ulang untuk menerapkan perbaikan ke database.
