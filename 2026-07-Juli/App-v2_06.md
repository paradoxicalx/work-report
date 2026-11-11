# 📝 Daily Work Report - Dedy Putra (2026-07-06)

---

## 📌 Informasi Issue
- **Nomor Issue**: #130
- **Judul Issue**: Fitur Hide Information pada BAA, Input Tanggal BAA, Logging Missing i18n, dan Remediasi Audit Kode

## 📅 Laporan Harian - 6 Juli 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

*(Tidak ada — seluruh pekerjaan sudah di-commit, di-push, dan di-merge ke branch `master`)*

### 📅 Rincian Commit

#### [a614cbb] - resolve #130 (#130 - Fitur Hide Information BAA, Tanggal BAA, Missing i18n Logger & Remediasi Audit)

- **Komponen yang Berubah**:
  - [`.gitignore`](file:///d:/Project/DEKASIMAL_V2/.gitignore)
  - [`backend/src/app.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/app.js)
  - [`backend/src/controllers/document.controller.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/document.controller.js)
  - [`backend/src/controllers/log.controller.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/log.controller.js) **[NEW]**
  - [`backend/src/controllers/productDedicatedInternet.controller.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/productDedicatedInternet.controller.js)
  - [`backend/src/locales/config.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/config.js)
  - [`backend/src/locales/en/translation.json`](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)
  - [`backend/src/models/document.model.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/models/document.model.js)
  - [`backend/src/routes/document.route.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/document.route.js)
  - [`backend/src/routes/log.route.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/log.route.js) **[NEW]**
  - [`backend/src/services/document.service.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/services/document.service.js)
  - [`backend/src/utils/missing-i18n-logger.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/utils/missing-i18n-logger.js) **[NEW]**
  - [`frontend/src/app/pages/public/PublicBAADocument.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/public/PublicBAADocument.jsx)
  - [`frontend/src/app/pages/services/activation/components/BAADocumentPreview.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/activation/components/BAADocumentPreview.jsx)
  - [`frontend/src/app/pages/services/activation/components/EditBAADrawer.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/activation/components/EditBAADrawer.jsx)
  - [`frontend/src/app/pages/services/activation/components/ReviewDrawer.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/activation/components/ReviewDrawer.jsx)
  - [`frontend/src/app/pages/services/dedicatedInternet/createBAA.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/dedicatedInternet/createBAA.jsx)
  - [`frontend/src/i18n/config.js`](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/config.js)
  - [`frontend/src/i18n/locales/en/translations.json`](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)

- **Deskripsi Perubahan & Fungsi**:

  **A. Fitur Baru — Hide Information pada Dokumen BAA**
  - Menambahkan field `hide_information` (tipe `[String]`) pada model `Document` untuk menyimpan daftar kolom informasi yang ingin disembunyikan saat cetak BAA.
  - Membuat endpoint baru `PATCH /documents/baa/hide-info/:id` di backend dengan Swagger documentation, dilindungi privilege `serviceActivation.update`.
  - Menambahkan controller `updateHideInfo` dan service `updateDocumentHideInfoById` sebagai handler dan data access layer.
  - Mengimplementasikan UI checkbox di `ReviewDrawer.jsx` agar admin bisa memilih kolom mana saja yang disembunyikan (Perusahaan/Perorangan, ID Pelanggan, Jenis Layanan, ID Layanan, Produk, Kapasitas, Abonemen, Keterangan).
  - Mengubah tampilan `BAADocumentPreview.jsx` dan `PublicBAADocument.jsx` agar kolom yang tercantum di `hide_information` tidak dirender pada preview & halaman publik.

  **B. Fitur Baru — Input Tanggal BAA (baaDate)**
  - Menambahkan komponen `InputDatePicker` pada form pembuatan BAA (`createBAA.jsx`) dan form edit BAA (`EditBAADrawer.jsx`) sehingga admin bisa menentukan tanggal BAA secara manual.
  - Backend (`productDedicatedInternet.controller.js`) kini menerima field `baaDate` dan menyimpannya sebagai `created_at` pada dokumen.
  - Service `updateBAADocumentById` juga mendukung update `created_at` melalui field `baaDate`.

  **C. Fitur Baru — Missing i18n Key Logger**
  - Membuat utility `missing-i18n-logger.js` yang mencatat setiap key terjemahan yang hilang ke file `logs/missing-translations.json` dalam format terstruktur (bahasa, namespace, key, sumber, waktu pertama kali terdeteksi).
  - Mengaktifkan `saveMissing: true` dan `missingKeyHandler` pada konfigurasi i18next di backend (`locales/config.js`) agar key yang hilang otomatis tercatat.
  - Membuat controller baru `log.controller.js` dan route baru `log.route.js` (`POST /logs/i18n-missing`) untuk menerima laporan missing key dari frontend.
  - Mengaktifkan `saveMissing: true` dan `missingKeyHandler` pada konfigurasi i18next di frontend (`i18n/config.js`) yang mengirim POST request ke endpoint backend saat menemukan key hilang.
  - Mendaftarkan `LogRoute` pada `app.js` backend.

  **D. Remediasi Audit Kode — Data Minimization & Error Handling**
  - **[document.controller.js]** Menghilangkan eksposur atribut internal Mongoose `__v` pada endpoint `getBAA` dengan menerapkan DTO (destructuring `{ __v, ...documentData }`).
  - **[document.controller.js]** Menghapus fallback string hardcode `'Invalid input'` pada fungsi `updateHideInfo`, kini murni menggunakan `req.t('document.invalidInput')`.
  - **[document.controller.js]** Mengganti pesan sukses hardcode `'Request preview sent successfully'` menjadi `req.t('document.previewSent')`.
  - **[log.controller.js]** Mengganti lemparan error hardcode `'Missing required fields: lng, key'` menjadi `req.t('error.missing_fields')`.
  - **[document.service.js]** Memperbaiki fallback `.select()` Mongoose dari `[]` (mengambil semua field) menjadi `'-__v'` untuk mencegah over-fetching data.
  - **[createBAA.jsx]** Menambahkan validasi `if (err.error)` sebelum iterasi `Object.entries(err.error)` untuk mencegah crash runtime saat API mengembalikan respons tanpa objek error terstruktur.

  **E. Penambahan Key i18n**
  - Backend (id/en): `document.previewSent`, `error.missing_fields`.
  - Frontend (id/en): `document.invalidInput`, `document.previewSent`, `error.missing_fields`, `activation.hideInformationTitle`, `activation.hideInformationDesc`, `activation.hideInfoFailed`, `alert.dataBAAUpdated`, `date`.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin kini dapat **menyembunyikan kolom informasi tertentu** (seperti ID Pelanggan, Abonemen, dsb.) pada dokumen BAA sebelum dicetak atau dibagikan ke pelanggan melalui link publik. Pengaturan ini dilakukan lewat panel checkbox di drawer review BAA.
  - Admin kini dapat **menentukan tanggal BAA secara manual** saat membuat maupun mengedit dokumen BAA, tidak lagi terkunci pada tanggal pembuatan otomatis.
  - Sistem kini secara otomatis **mencatat key terjemahan (i18n) yang hilang** dari backend maupun frontend ke file log JSON, sehingga memudahkan developer untuk mendeteksi string yang belum diterjemahkan.

- **Bug Fix / Solusi Masalah**:
  - **Crash Prevention**: Memperbaiki potensi *white screen crash* pada halaman pembuatan BAA (`createBAA.jsx`) ketika API mengembalikan error tanpa objek `error` terstruktur.
  - **Data Minimization**: Menghilangkan atribut internal Mongoose (`__v`) dari respons API untuk mencegah kebocoran metadata database ke frontend.
  - **Over-fetching**: Memperbaiki query Mongoose yang mengambil semua field database ketika parameter `select` kosong.
  - **i18n Compliance**: Menghilangkan seluruh fallback string hardcode pada controller backend, memastikan semua pesan error dan sukses menggunakan fungsi lokalisasi `req.t()` sesuai standar AGENTS.md.

- **Menu/Tombol Baru**:
  - **Checkbox "Sembunyikan Informasi pada Cetak BAA"** pada drawer review BAA — panel berisi 8 checkbox untuk setiap kolom informasi yang dapat disembunyikan.
  - **Input Date Picker "Tanggal"** pada form pembuatan dan edit BAA.

## 📖 Informasi & Tutorial Singkat Fitur

- **Penjelasan Fitur**:
  Fitur utama dalam issue ini adalah **Hide Information BAA** — admin dapat memilih kolom-kolom mana saja pada dokumen Berita Acara Aktivasi (BAA) yang ingin disembunyikan saat dicetak atau dilihat di halaman publik. Data tetap tersimpan di database, namun tidak ditampilkan secara visual. Selain itu, admin kini bisa menentukan tanggal BAA secara manual melalui input date picker.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Layanan > Dedicated Internet** atau **Aktivasi Layanan**.
  2. Pilih layanan yang memiliki dokumen BAA, lalu klik tombol review/detail BAA.
  3. Pada drawer review, scroll ke bagian bawah untuk menemukan panel **"Sembunyikan Informasi pada Cetak BAA"**.
  4. Centang kolom-kolom yang ingin disembunyikan (misalnya: Abonemen, ID Pelanggan).
  5. Perubahan akan tersimpan otomatis secara real-time (tanpa tombol submit tambahan).
  6. Buka preview BAA atau link publik untuk memverifikasi bahwa kolom yang dicentang sudah tidak tampil.
  7. Untuk mengubah tanggal BAA, gunakan input **Tanggal** pada form pembuatan atau edit BAA.

---

> **✅ Status:** Pekerjaan pada issue #130 sudah **selesai sepenuhnya** dan sudah **di-merge ke branch `master`**.
