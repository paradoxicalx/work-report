# 📝 Daily Work Report - Dedy S.N Putra (2026-07-29)

---

## 📅 Laporan Harian - 29 Juli 2026

---

## 🌿 Branch: `master` — Resolve #169 (Public Select List Broadband)

### 📌 Informasi Issue

- **Nomor Issue**: #169
- **Judul Issue**: Public Select List Broadband Product
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`199cd87`](199cd8784bdd6332bdac0d57965b12f32c01f8b1) - resolve #169 - 29 Juli 2026, 10:00:07

- **Komponen yang Berubah**:
  - [`backend/src/controllers/productBroadband.controller.js`](backend/src/controllers/productBroadband.controller.js) [NEW]
  - [`backend/src/routes/productBroadband.route.js`](backend/src/routes/productBroadband.route.js)
  - [`frontend/src/components/shared/form/FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx)
- **Deskripsi Perubahan & Fungsi**:
  - **Backend Controller**: Membuat fungsi baru [`selectListPublicProductBroadband()`](backend/src/controllers/productBroadband.controller.js:103) yang menyediakan daftar produk broadband secara publik. Fungsi ini memfilter hanya produk dengan `available_request: true`, mengurutkan berdasarkan harga (ascending), dan mengembalikan data dalam format `{ name, id }` yang siap digunakan oleh dropdown/combo di frontend. Berbeda dari endpoint admin sebelumnya, endpoint ini tidak memerlukan autentikasi.
  - **Backend Route**: Menambahkan endpoint publik baru `GET /api/v1/product/broadband/public-select-list` tanpa middleware `protectedAdmin`, sehingga dapat diakses tanpa login. Dokumentasi Swagger juga diperbarui untuk endpoint admin yang ada (`/select-list`) agar lebih jelas deskripsinya.
  - **Frontend Form**: Memperbarui komponen [`InputProductBroadbandSelect`](frontend/src/components/shared/form/FormInput.jsx:718) pada [`FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx) untuk menggunakan endpoint baru `/public-select-list` alih-alih `/select-list`. Ini memastikan form produk broadband yang digunakan oleh pelanggan/publik hanya menampilkan paket yang tersedia untuk permintaan, bukan seluruh paket admin.

---

## 🌿 Branch: `issue-162` — WhatsApp Broadcast Enhancement (Work In Progress)

### 📌 Informasi Issue

- **Nomor Issue**: #162
- **Judul Issue**: WhatsApp Broadcast Enhancement
- **Status Branch**: `Belum di-merge` (ada perubahan uncommitted)

### 📅 Rincian Pekerjaan (Uncommitted Changes)

> **Catatan**: Pekerjaan ini masih dalam status WIP (Work In Progress) dan belum di-commit pada tanggal 29 Juli 2026. Terdapat **32 file** yang dimodifikasi dengan total **+1.506 / -223** baris perubahan.

- **Komponen yang Berubah**:

  **Backend (11 file):**
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/controllers/waBroadcast.controller.js`](backend/src/controllers/waBroadcast.controller.js)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/models/waBroadcast.model.js`](backend/src/models/waBroadcast.model.js)
  - [`backend/src/models/waBroadcastRecipient.model.js`](backend/src/models/waBroadcastRecipient.model.js)
  - [`backend/src/routes/waBroadcast.route.js`](backend/src/routes/waBroadcast.route.js)
  - [`backend/src/routes/waChat.route.js`](backend/src/routes/waChat.route.js)
  - [`backend/src/services/waBroadcast.service.js`](backend/src/services/waBroadcast.service.js)
  - [`backend/src/services/waBroadcastQueue.service.js`](backend/src/services/waBroadcastQueue.service.js)
  - [`backend/src/services/waSender.service.js`](backend/src/services/waSender.service.js)

  **Cron Worker (1 file):**
  - [`cron-worker/.env.example`](cron-worker/.env.example)

  **Frontend (20 file):**
  - [`frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx`](frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx)
  - [`frontend/src/app/pages/customerService/messageTemplate/index.jsx`](frontend/src/app/pages/customerService/messageTemplate/index.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/AccumulatedRecipientList.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/AccumulatedRecipientList.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/BroadcastDetailDrawer.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/BroadcastDetailDrawer.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/CreateBroadcastDrawer.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/CreateBroadcastDrawer.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/RecipientPicker.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/RecipientPicker.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableDocsModal.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableDocsModal.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableMapper.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableMapper.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/index.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/index.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/columns.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/columns.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/recipientColumns.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/recipientColumns.jsx)
  - [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/rows.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/rows.jsx)
  - [`frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx)
  - [`frontend/src/app/pages/customerService/whatsappChat/components/EmojiPopover.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/EmojiPopover.jsx)
  - [`frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx)
  - [`frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx`](frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx)
  - [`frontend/src/components/shared/table/RowActions.jsx`](frontend/src/components/shared/table/RowActions.jsx)
  - [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)

- **Deskripsi Perubahan & Fungsionalitas**:
  - **Backend**: Pengembangan fitur WhatsApp Broadcast meliputi penambahan privilege baru, perluasan model `waBroadcast` dan `waBroadcastRecipient`, penambahan endpoint baru pada controller dan route, serta peningkatan layanan broadcast queue dan sender.
  - **Frontend**: Peningkatan UI/UX modul WhatsApp Broadcast meliputi perbaikan komponen `CreateBroadcastDrawer`, `BroadcastDetailDrawer`, `RecipientPicker`, `AccumulatedRecipientList`, `TemplateVariableMapper`, dan `TemplateVariableDocsModal`. Penambahan kolom/aksi pada datatables (`columns.jsx`, `recipientColumns.jsx`, `rows.jsx`), penambahan komponen shared `status.js` untuk status badge, serta pembaruan komponen WhatsApp Chat (`ChatNotice`, `EmojiPopover`, `QuickTemplatePopover`).
  - **i18n**: Penambahan 17 key baru untuk locale `id` dan `en`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                          | Dampak Utama                                                   |
| ----- | ------------------------------ | -------------------------------------------------------------- |
| #169  | Public Select List Broadband   | Endpoint publik untuk daftar paket broadband tanpa autentikasi |
| #162  | WhatsApp Broadcast Enhancement | Peningkatan fitur broadcast WhatsApp (WIP, belum di-commit)    |

### Kemampuan Baru Pengguna/Admin

- **Public Access Broadband Select**: Form pemilihan paket broadband yang digunakan oleh pelanggan/publik sekarang hanya menampilkan paket yang tersedia untuk permintaan (`available_request: true`), bukan seluruh paket yang ada di sistem admin. Ini mencegah pelanggan melihat paket yang tidak ditujukan untuk mereka.
- **WhatsApp Broadcast (WIP)**: Peningkatan signifikan pada fitur broadcast WhatsApp meliputi manajemen penerima, template variabel, dan antarmuka pembuatan broadcast yang lebih baik.

### Bug Fix / Solusi Masalah

- **Solusi Keamanan Endpoint**: Endpoint select list sebelumnya menggunakan endpoint admin yang memerlukan autentikasi. Dengan adanya endpoint publik baru, form yang diakses oleh pelanggan/publik kini dapat berfungsi tanpa login, sementara data yang ditampilkan tetap terbatas hanya pada paket yang diizinkan.

### Menu/Fitur Baru

- **Endpoint Publik Broadband**: Menu/Tombol pilihan paket broadband pada form publik sekarang menggunakan endpoint terpisah yang lebih aman dan spesifik.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Endpoint `GET /api/v1/product/broadband/public-select-list` adalah endpoint baru yang dirancang khusus untuk kebutuhan publik (tanpa autentikasi). Endpoint ini mengambil semua produk broadband yang memiliki flag `available_request: true`, mengurutkan dari harga termurah, dan mengembalikan data dalam format dropdown (`{ name: "Paket Name - Rp 100.000", id: "KODE" }`). Endpoint ini digunakan oleh komponen [`InputProductBroadbandSelect`](frontend/src/components/shared/form/FormInput.jsx:718) pada form yang diakses oleh pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Akses halaman formulir publik yang menggunakan komponen pemilihan paket broadband (contoh: formulir registrasi pelanggan).
  2. Pada dropdown pemilihan paket, sistem akan secara otomatis memuat daftar paket dari endpoint publik.
  3. Hanya paket yang memiliki status `available_request: true` yang akan ditampilkan, diurutkan dari harga termurah.
  4. Untuk penggunaan API langsung: `GET /api/v1/product/broadband/public-select-list` (tanpa header Authorization).
