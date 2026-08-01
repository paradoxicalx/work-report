# 📝 Daily Work Report - Dedy S.N Putra (2026-08-01)

---

## 📅 Laporan Harian - 1 Agustus 2026

---

## 🌿 Branch: `issue-173` — Resolve #154 (Frontend Refactoring & Improvement)

### 📌 Informasi Issue

- **Nomor Issue**: #154
- **Judul Issue**: Frontend Refactoring & Improvement
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`8034d4b`](8034d4b4a93c192ca97a468f076a395d2f344aff) - resolve #154 - 1 Agustus 2026, 11:51:16

- **Komponen yang Berubah**:

  **Frontend — Radius Dashboard (8 file):**
  - [`frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx`](frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx)
  - [`frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx)
  - [`frontend/src/app/pages/dashboards/radius/components/ServerList.jsx`](frontend/src/app/pages/dashboards/radius/components/ServerList.jsx)
  - [`frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx`](frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx)
  - [`frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx)
  - [`frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js`](frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js)
  - [`frontend/src/app/pages/dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx)
  - [`frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx`](frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx)

  **Frontend — Work Order (9 file):**
  - [`frontend/src/app/pages/services/workOrder/WorkOrderDetailDrawer.jsx`](frontend/src/app/pages/services/workOrder/WorkOrderDetailDrawer.jsx)
  - [`frontend/src/app/pages/services/workOrder/WorkOrderDocument.jsx`](frontend/src/app/pages/services/workOrder/WorkOrderDocument.jsx)
  - [`frontend/src/app/pages/services/workOrder/create.jsx`](frontend/src/app/pages/services/workOrder/create.jsx)
  - [`frontend/src/app/pages/services/workOrder/detail.jsx`](frontend/src/app/pages/services/workOrder/detail.jsx)
  - [`frontend/src/app/pages/services/workOrder/edit.jsx`](frontend/src/app/pages/services/workOrder/edit.jsx)
  - [`frontend/src/app/pages/services/workOrder/index.jsx`](frontend/src/app/pages/services/workOrder/index.jsx) _(dihapus)_
  - [`frontend/src/app/pages/services/workOrder/schema/columns.jsx`](frontend/src/app/pages/services/workOrder/schema/columns.jsx)
  - [`frontend/src/app/pages/services/workOrder/schema/createSchema.js`](frontend/src/app/pages/services/workOrder/schema/createSchema.js) **[NEW]**
  - [`frontend/src/app/pages/services/workOrder/schema/editSchema.js`](frontend/src/app/pages/services/workOrder/schema/editSchema.js) **[NEW]**

  **Frontend — Profile & User Pages (4 file):**
  - [`frontend/src/app/pages/users/business/profile.jsx`](frontend/src/app/pages/users/business/profile.jsx)
  - [`frontend/src/app/pages/users/partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/CustomerPODocumentPreview.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/CustomerPODocumentPreview.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/create.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/create.jsx)

  **Frontend — Shared Components & Utils (8 file):**
  - [`frontend/src/app/pages/users/quotation/QuotationDetailDrawer.jsx`](frontend/src/app/pages/users/quotation/QuotationDetailDrawer.jsx)
  - [`frontend/src/app/pages/activities/attendance/components/ManualCheckInDrawer.jsx`](frontend/src/app/pages/activities/attendance/components/ManualCheckInDrawer.jsx)
  - [`frontend/src/app/pages/activities/attendance/index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx)
  - [`frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx`](frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx)
  - [`frontend/src/components/shared/Badge.jsx`](frontend/src/components/shared/Badge.jsx)
  - [`frontend/src/components/shared/DocumentPreviewModal.jsx`](frontend/src/components/shared/DocumentPreviewModal.jsx)
  - [`frontend/src/components/shared/form/FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx)
  - [`frontend/src/components/shared/table/DocumentActionsMenu.jsx`](frontend/src/components/shared/table/DocumentActionsMenu.jsx)
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx)
  - [`frontend/src/styles/base.css`](frontend/src/styles/base.css)
  - [`frontend/src/utils/attendanceExcelExporter.js`](frontend/src/utils/attendanceExcelExporter.js)
  - [`frontend/src/utils/axios.js`](frontend/src/utils/axios.js)

- **Deskripsi Perubahan & Fungsi**:
  - **Radius Dashboard**: Melakukan refactor menyeluruh pada seluruh komponen dashboard Radius — [`EventsChart`](frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx), [`LiveLogPanel`](frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx), [`ServerList`](frontend/src/app/pages/dashboards/radius/components/ServerList.jsx), [`StatsRow`](frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx), [`UserActivityPanel`](frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx). Memperbaiki formatasi kode (indentasi, line breaks), konsistensi styling Tailwind, dan optimasi rendering pada komponen real-time.
  - **Work Order**: Refactor halaman Work Order secara menyeluruh — [`create.jsx`](frontend/src/app/pages/services/workOrder/create.jsx), [`edit.jsx`](frontend/src/app/pages/services/workOrder/edit.jsx), [`detail.jsx`](frontend/src/app/pages/services/workOrder/detail.jsx). Membuat file schema terpisah ([`createSchema.js`](frontend/src/app/pages/services/workOrder/schema/createSchema.js), [`editSchema.js`](frontend/src/app/pages/services/workOrder/schema/editSchema.js)) untuk Yup validation schema yang sebelumnya inline. Field `topic` diubah dari input teks biasa menjadi Combobox tags yang terhubung ke endpoint `/ticket/{type}/type-select`. Detail page direfactor menggunakan komponen Badge baru ([`LinkBadge`](frontend/src/components/shared/Badge.jsx), [`TicketTypeBadge`](frontend/src/components/shared/Badge.jsx), [`StringBadge`](frontend/src/components/shared/Badge.jsx)) untuk menampilkan data relasi.
  - **Business & Partner Profile**: Merombak halaman profile [`business/profile.jsx`](frontend/src/app/pages/users/business/profile.jsx) dan [`partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx) — menambahkan kolom nomor urut (No.) pada tabel Quotation, memperbaiki layout tabel agar lebih responsif, dan menambahkan icon `InboxIcon` untuk tab baru.
  - **Shared Components**: [`Badge`](frontend/src/components/shared/Badge.jsx) — menambahkan `type="button"` dan `stopPropagation` pada tombol [`BadgeDownload`](frontend/src/components/shared/Badge.jsx) untuk mencegah event bubbling. [`DocumentPreviewModal`](frontend/src/components/shared/DocumentPreviewModal.jsx) — menambahkan `createPortal` ke `document.body` agar modal selalu render di root, memperbaiki class `dark:bg-dark-700` → `dark:bg-dark-750`. [`DocumentActionsMenu`](frontend/src/components/shared/table/DocumentActionsMenu.jsx) — menambahkan `e.stopPropagation()` pada tombol aksi agar tidak trigger row click. [`rows.jsx`](frontend/src/components/shared/table/rows.jsx) — perbaikan indentasi pada komponen `CollaboratorsCell`. [`FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx) — perbaikan formatasi pada `InputListboxSelect`.
  - **Base CSS**: Mengubah `h-full` → `min-h-full` pada `html` dan `body` di [`base.css`](frontend/src/styles/base.css) untuk mencegah layout issue pada halaman dengan konten panjang. Menambahkan `appearance: textfield` standar (selain vendor prefix) untuk hide number spinner.
  - **Axios Interceptor**: Perbaikan formatasi kode pada [`axios.js`](frontend/src/utils/axios.js) — konsistensi spacing, arrow function formatting, dan Promise chain readability pada token refresh logic.
  - **Attendance**: Perbaikan formatasi kode pada [`ManualCheckInDrawer`](frontend/src/app/pages/activities/attendance/components/ManualCheckInDrawer.jsx) dan [`index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx) — termasuk perbaikan responsive class order (`md:col-span-1`) dan penghapusan baris kosong berlebih.

#### [`ddbc4ab`](ddbc4ab28e9f6ff23c50bd3746907f58060e0952) - Merge branch 'master' into issue-173 - 1 Agustus 2026, 12:25:28

- **Deskripsi**: Merge branch `master` (yang sudah berisi resolve #178) ke dalam `issue-173` untuk menyelesaikan konflik sebelum merge ke master.

#### [`5dbb82b`](5dbb82b4d2e7f089e6efc32230327fb983d6bcaf) - resolve #154 (merge commit) - 1 Agustus 2026, 12:26:28

- **Deskripsi**: Merge commit `issue-173` ke `master`. Branch `issue-173` sekarang sudah sepenuhnya ter-merge.

---

## 🌿 Branch: `master` — Resolve #178 (WhatsApp Auto-Reply System Improvement)

### 📌 Informasi Issue

- **Nomor Issue**: #178
- **Judul Issue**: WhatsApp Auto-Reply System Improvement
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`d8c5bde`](d8c5bde0e1baea460e5f1602c34c5c2378b8ea64) - resolve #178 - 1 Agustus 2026, 11:09:21

- **Komponen yang Berubah**:

  **Backend (6 file):**
  - [`backend/src/controllers/waInternal.controller.js`](backend/src/controllers/waInternal.controller.js)
  - [`backend/src/locales/createNewLang.js`](backend/src/locales/createNewLang.js) **[NEW]**
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/services/admin.service.js`](backend/src/services/admin.service.js)
  - [`backend/src/services/waChatSweep.service.js`](backend/src/services/waChatSweep.service.js)
  - [`backend/src/sockets/waChat.controller.js`](backend/src/sockets/waChat.controller.js)
  - [`backend/src/utils/waChatUtils.js`](backend/src/utils/waChatUtils.js)

  **Telegram Apps (3 file):**
  - [`telegram-apps/src/context/AuthContext.jsx`](telegram-apps/src/context/AuthContext.jsx)
  - [`telegram-apps/src/lib/axiosClient.js`](telegram-apps/src/lib/axiosClient.js)
  - [`telegram-apps/src/pages/Profile.jsx`](telegram-apps/src/pages/Profile.jsx)

  **WhatsApp API (5 file):**
  - [`whatsapp-api/.env.example`](whatsapp-api/.env.example)
  - [`whatsapp-api/DOCUMENTATION.md`](whatsapp-api/DOCUMENTATION.md)
  - [`whatsapp-api/src/config.js`](whatsapp-api/src/config.js)
  - [`whatsapp-api/src/controllers/template.controller.js`](whatsapp-api/src/controllers/template.controller.js)
  - [`whatsapp-api/src/controllers/webhook.controller.js`](whatsapp-api/src/controllers/webhook.controller.js)
  - [`whatsapp-api/src/routes/sendTemplate.route.js`](whatsapp-api/src/routes/sendTemplate route.js) **[NEW]**

- **Deskripsi Perubahan & Fungsi**:
  - **Backend — Auto-Reply Sistem**: Refactor besar pada sistem auto-reply WhatsApp. Menambahkan deteksi tombol otomatis (`isAutoReplyButtonId`) pada [`waInternal.controller.js`](backend/src/controllers/waInternal.controller.js) agar penekanan tombol pada pesan otomatis (seperti "Lihat Menu Bantuan" dan "Tunggu Admin") tidak mereset timer `auto_reply_sent_at` dan tidak menyebabkan balasan berulang. Menambahkan fungsi `sendAutoTextMessage` pada [`waChatSweep.service.js`](backend/src/services/waChatSweep.service.js) untuk mengirim pesan teks biasa (tanpa tombol) sebagai balasan tombol "Tunggu Admin". Mengubah `i18nx()` → `i18nCustomer()` pada seluruh alur sweep agar teks otomatis selalu dalam bahasa Indonesia (bukan bahasa panel admin). Menambahkan konstanta `WA_OFFLINE_SHOW_MENU_ID` dan `WA_OFFLINE_WAIT_ADMIN_ID` pada [`waChatUtils.js`](backend/src/utils/waChatUtils.js) untuk identifikasi tombol otomatis. Menambahkan fungsi `hasWaInboxPresence` pada [`waChat.controller.js`](backend/src/sockets/waChat.controller.js) untuk mendeteksi apakah ada admin yang sedang membuka halaman obrolan WhatsApp — digunakan sebagai pemicu instant auto-reply saat tidak ada admin online.
  - **Backend — i18n Customer**: Membuat file [`createNewLang.js`](backend/src/locales/createNewLang.js) dengan fungsi `i18nCustomer()` yang mengembalikan instance i18n khusus pelanggan (selalu bahasa Indonesia), terpisah dari `i18nx()` yang mengikuti bahasa panel admin.
  - **WhatsApp API — Template & Webhook**: Menambahkan route baru `POST /send-template` pada [`sendTemplate.route.js`](whatsapp-api/src/routes/sendTemplate route.js) untuk pengiriman template WhatsApp secara langsung dari API. Memperluas [`template.controller.js`](whatsapp-api/src/controllers/template.controller.js) dengan fungsi untuk mengelola template WhatsApp. Menambahkan konfigurasi `MAX_TEMPLATE_PARAMS` pada [`config.js`](whatsapp-api/src/config.js). Memperbarui webhook controller untuk menangani event template.
  - **Telegram Apps**: Memperbaiki [`AuthContext.jsx`](telegram-apps/src/context/AuthContext.jsx) dengan menambahkan penanganan error yang lebih baik. Menyederhanakan [`axiosClient.js`](telegram-apps/src/lib/axiosClient.js) dengan mengurangi kompleksitas interceptor. Memperbarui [`Profile.jsx`](telegram-apps/src/pages/Profile.jsx).

#### [`6d9eef4`](6d9eef40dac9a6dbc41f8281da33be8e6f846dce) - resolve #178 (merge commit) - 1 Agustus 2026, 11:09:50

- **Deskripsi**: Merge commit yang menggabungkan issue-172 dan commit d8c5bde ke master.

---

## 🌿 Branch: `issue-172` — Resolve #172 (Privilege System Overhaul)

### 📌 Informasi Issue

- **Nomor Issue**: #172
- **Judul Issue**: Privilege System Overhaul
- **Status Branch**: `Sudah di-merge` ke `master`

### 📅 Rincian Commit

#### [`4e45d84`](4e45d841c128420281b9e8db20ba5d0d077df484) - resolve #172 - 30 Juli 2026, 13:37:45

- **Komponen yang Berubah**:

  **Backend (8 file):**
  - [`AGENTS.md`](AGENTS.md)
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/middlewares/privilegeTicket.middleware.js`](backend/src/middlewares/privilegeTicket.middleware.js) **[NEW]**
  - [`backend/src/models/ticket.model.js`](backend/src/models/ticket.model.js)
  - [`backend/src/routes/customerSO.route.js`](backend/src/routes/customerSO.route.js)
  - [`backend/src/routes/files.route.js`](backend/src/routes/files.route.js)
  - [`backend/src/routes/locationPoint.route.js`](backend/src/routes/locationPoint.route.js)
  - [`backend/src/routes/ticket.route.js`](backend/src/routes/ticket.route.js)
  - [`backend/src/utils/migrate-privilege-172.js`](backend/src/utils/migrate-privilege-172.js) **[NEW]**
  - [`backend/src/utils/migrate-privilege-ticket-generic.js`](backend/src/utils/migrate-privilege-ticket-generic.js) **[NEW]**

  **Frontend (12 file):**
  - [`frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`](frontend/src/app/pages/tickets/TicketDetailDrawer.jsx)
  - [`frontend/src/app/pages/tickets/installation/detail.jsx`](frontend/src/app/pages/tickets/installation/detail.jsx)
  - [`frontend/src/app/pages/users/business/profile.jsx`](frontend/src/app/pages/users/business/profile.jsx)
  - [`frontend/src/app/pages/users/customer/profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx)
  - [`frontend/src/app/pages/users/customerSalesOrder/create.jsx`](frontend/src/app/pages/users/customerSalesOrder/create.jsx)
  - [`frontend/src/app/pages/users/partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx)
  - [`frontend/src/app/pages/users/privilege/create.jsx`](frontend/src/app/pages/users/privilege/create.jsx)
  - [`frontend/src/app/pages/users/privilege/detail.jsx`](frontend/src/app/pages/users/privilege/detail.jsx)
  - [`frontend/src/app/pages/users/privilege/edit.jsx`](frontend/src/app/pages/users/privilege/edit.jsx)
  - [`frontend/src/constants/privilegeDescriptions.en.json`](frontend/src/constants/privilegeDescriptions.en.json) **[NEW]**
  - [`frontend/src/constants/privilegeDescriptions.id.json`](frontend/src/constants/privilegeDescriptions.id.json) **[NEW]**
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)

- **Deskripsi Perubahan & Fungsi**:
  - **Backend — Privilege Middleware**: Membuat middleware baru [`privilegeTicket.middleware.js`](backend/src/middlewares/privilegeTicket.middleware.js) yang menggantikan `checkPrivilege('ticket.changeStatus')` generik dengan `checkTicketPrivilegeByType()` — fungsi yang secara dinamis menentukan privilege key berdasarkan jenis tiket (`installation`, `survey`, `customer`, dll.), sehingga setiap jenis tiket memiliki kontrol akses yang lebih presisi.
  - **Backend — Privilege Key Migration**: Membuat script migrasi [`migrate-privilege-172.js`](backend/src/utils/migrate-privilege-172.js) untuk mentransisi privilege key lama (`customerSO.send` → `customerSO.changeStatus`, `networkSite.report` → `networkSite.read`, `customerSO.withoutPo` → dihapus). Membuat script [`migrate-privilege-ticket-generic.js`](backend/src/utils/migrate-privilege-ticket-generic.js) untuk menghapus module privilege generik `ticket` dan mendistribusikannya ke `ticket<Type>.update` dan `ticket<Type>.changeStatus` per jenis tiket.
  - **Backend — Route Updates**: Memperbarui [`ticket.route.js`](backend/src/routes/ticket.route.js) menggunakan `checkTicketPrivilegeByType` alih-alih `checkPrivilege` generik. Memperbarui [`customerSO.route.js`](backend/src/routes/customerSO.route.js), [`files.route.js`](backend/src/routes/files.route.js), dan [`locationPoint.route.js`](backend/src/routes/locationPoint.route.js) dengan privilege key yang sudah disesuaikan.
  - **Backend — AGENTS.md**: Menambahkan Bab 4.11 "Hak Akses (Privilege) & Access Control" yang mendokumentasikan format key, vocabulary aksi yang diizinkan, kewajiban sinkronisasi frontend↔backend, dan cara verifikasi privilege.
  - **Frontend — Privilege CRUD**: Memperluas halaman [`privilege/create.jsx`](frontend/src/app/pages/users/privilege/create.jsx), [`privilege/detail.jsx`](frontend/src/app/pages/users/privilege/detail.jsx), dan [`privilege/edit.jsx`](frontend/src/app/pages/users/privilege/edit.jsx) dengan deskripsi privilege yang lebih informatif, tampilan yang lebih baik, dan dukungan untuk privilege key baru.
  - **Frontend — Privilege Descriptions**: Membuat file konstanta [`privilegeDescriptions.en.json`](frontend/src/constants/privilegeDescriptions.en.json) dan [`privilegeDescriptions.id.json`](frontend/src/constants/privilegeDescriptions.id.json) yang berisi deskripsi lengkap untuk setiap privilege key dalam bahasa Inggris dan Indonesia.
  - **Frontend — Ticket & CSO Updates**: Memperbarui [`TicketDetailDrawer.jsx`](frontend/src/app/pages/tickets/TicketDetailDrawer.jsx), [`installation/detail.jsx`](frontend/src/app/pages/tickets/installation/detail.jsx), dan [`customerSalesOrder/create.jsx`](frontend/src/app/pages/users/customerSalesOrder/create.jsx) dengan privilege check yang menggunakan key baru.

#### [`78f867a`](78f867a) - Merge branch 'issue-174' - 1 Agustus 2026, 09:26:48

- **Deskripsi**: Merge branch `issue-174` ke dalam `issue-172`.

#### [`edb664a`](edb664a) - Merge branch 'master' into issue-172 - 1 Agustus 2026, 09:34:32

- **Deskripsi**: Merge branch `master` ke dalam `issue-172` untuk menyelesaikan konflik.

#### [`e13f7b2`](e13f7b2ac07458398e4fb528b584034a3fda3937) - resolve #172 (merge commit) - 1 Agustus 2026, 09:34:55

- **Deskripsi**: Merge commit yang menggabungkan issue-172 ke dalam alur menuju master.

---

## 🌿 Branch: `issue-173` — Isolir Batch (Work In Progress)

### 📌 Informasi Issue

- **Nomor Issue**: #173
- **Judul Issue**: Isolir Batch
- **Status Branch**: `Belum di-merge` (ada perubahan uncommitted)

### 📅 Rincian Pekerjaan (Uncommitted Changes)

> **Catatan**: Pekerjaan ini masih dalam status WIP (Work In Progress) dan belum di-commit pada tanggal 1 Agustus 2026. Terdapat **8 file** yang dimodifikasi dan **1 direktori baru** dengan total **+220 / -4** baris perubahan.

- **Komponen yang Berubah**:

  **Backend (4 file):**
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json)
  - [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js)
  - [`backend/src/routes/radiusAuthentication.route.js`](backend/src/routes/radiusAuthentication.route.js)
  - [`backend/src/services/radiusAuthentication.service.js`](backend/src/services/radiusAuthentication.service.js)

  **Frontend (5 file + 1 direktori baru):**
  - [`frontend/src/app/navigation/utilities.js`](frontend/src/app/navigation/utilities.js)
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx)
  - [`frontend/src/app/pages/utilities/isolirBatch/index.jsx`](frontend/src/app/pages/utilities/isolirBatch/index.jsx) **[NEW]**
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)

- **Deskripsi Perubahan & Fungsi**:
  - **Backend — Isolir Candidates Endpoint**: Membuat endpoint baru `POST /broadband/isolir-candidates` pada [`radiusAuthentication.route.js`](backend/src/routes/radiusAuthentication.route.js) dengan privilege `broadband.isolirCandidate`. Controller [`readIsolirCandidates`](backend/src/controllers/radiusAuthentication.controller.js) menerima parameter `type` (customer/partner/authentication), `count` (jumlah minimum invoice unpaid), `pay_support`, dan `partner`. Service [`findIsolirCandidates`](backend/src/services/radiusAuthentication.service.js) melakukan aggregation pipeline pada koleksi `FinanceInvoice` untuk mencari entitas dengan jumlah invoice unpaid ≥ `count`, lalu menghubungkan dengan data `RadiusAuthentication` terkait. Mendukung 3 mode pencarian: berdasarkan pelanggan, mitra, atau autentikasi langsung.
  - **Backend — Privilege Update**: Menambahkan privilege key `broadband.isolirCandidate` ke [`privilege.json`](backend/src/config/privilege.json).
  - **Frontend — Isolir Batch Page**: Membuat halaman baru [`isolirBatch/index.jsx`](frontend/src/app/pages/utilities/isolirBatch/index.jsx) dengan fitur pencarian kandidat isolir massal. Pengguna dapat memilih dasar pencarian (pelanggan/mitra/autentikasi), jumlah minimum invoice unpaid, pay support, dan mitra. Hasil pencarian ditampilkan dalam tabel dengan kolom username (link ke broadband), pengguna (link ke profil), produk, jumlah tagihan, dan tombol hapus. Pengguna dapat memilih status target (Nonaktif / Nonaktif Hingga Pembayaran) dan mengubah status secara massal melalui endpoint `/broadband/update-batch`.
  - **Frontend — Navigation & Router**: Menambahkan menu "Isolir Batch" pada navigasi [`utilities.js`](frontend/src/app/navigation/utilities.js) dengan icon `IoMdLock` dan privilege `broadband.isolirCandidate`. Menambahkan route lazy-loaded pada [`protected.jsx`](frontend/src/app/router/protected.jsx).
  - **Frontend — i18n**: Menambahkan translation keys untuk halaman Isolir Batch di [`en/translations.json`](frontend/src/i18n/locales/en/translations.json) dan [`id/translations.json`](frontend/src/i18n/locales/id/translations.json).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                  | Dampak Utama                                                                                                                               |
| ----- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| #154  | Frontend Refactoring & Improvement     | Refactor menyeluruh frontend — Radius Dashboard, Work Order, Profile pages, shared components, CSS, dan Axios interceptor                  |
| #178  | WhatsApp Auto-Reply System Improvement | Sistem auto-reply WhatsApp lebih cerdas — deteksi tombol otomatis, balasan "Tunggu Admin" tanpa tombol, pemicu instan saat tidak ada admin |
| #172  | Privilege System Overhaul              | Sistem hak akses lebih presisi — privilege per jenis tiket, migrasi key lama, deskripsi privilege di UI, dokumentasi AGENTS.md             |
| #173  | Isolir Batch (WIP)                     | Fitur isolir massal berdasarkan jumlah invoice unpaid — backend endpoint + frontend page                                                   |

### Kemampuan Baru Pengguna/Admin

- **Isolir Massal**: Admin dapat mencari dan mengubah status autentikasi broadband secara massal berdasarkan jumlah invoice yang belum lunas — mendukung pencarian per pelanggan, mitra, atau autentikasi langsung.
- **Auto-Reply WhatsApp Lebih Cerdas**: Sistem auto-reply kini mengenali tombol otomatis ("Lihat Menu Bantuan", "Tunggu Admin") dan merespons secara berbeda — tombol "Lihat Menu" mengirim ulang menu, tombol "Tunggu Admin" mengirim konfirmasi teks tanpa tombol, sehingga pelanggan tidak terjebak dalam lingkaran balasan robot.
- **Privilege per Jenis Tiket**: Kontrol akses tiket kini spesifik per jenis (Installation, Survey, Customer, Partner, dll.) alih-alih generik — admin dapat mengatur hak akses "ubah prioritas" dan "ubah PIC" secara terpisah untuk setiap jenis tiket.

### Bug Fix / Solusi Masalah

- **Event Bubbling pada Tombol Aksi Tabel**: Memperbaiki bug di mana klik tombol aksi pada baris tabel (DocumentActionsMenu, BadgeDownload) juga memicu event click pada row induk — ditambahkan `e.stopPropagation()` untuk mencegah navigasi tak terduga.
- **Modal Render di Dalam Container Terbatas**: [`DocumentPreviewModal`](frontend/src/components/shared/DocumentPreviewModal.jsx) kini menggunakan `createPortal` ke `document.body` agar modal selalu render di root, mengatasi masalah overflow/positioning saat modal dibuka di dalam drawer atau container dengan `overflow: hidden`.
- **Layout Height Issue**: Mengubah `h-full` → `min-h-full` pada `html` dan `body` di [`base.css`](frontend/src/styles/base.css) untuk mencegah konten terpotong pada halaman dengan tinggi konten melebihi viewport.
- **Work Order Validation Schema**: Memindahkan Yup schema Work Order dari inline di [`create.jsx`](frontend/src/app/pages/services/workOrder/create.jsx) ke file terpisah ([`createSchema.js`](frontend/src/app/pages/services/workOrder/schema/createSchema.js), [`editSchema.js`](frontend/src/app/pages/services/workOrder/schema/editSchema.js)) untuk reusability dan maintainability.

### Menu/Fitur Baru

- **Isolir Batch** ([`/utilities/isolirBatch`](frontend/src/app/pages/utilities/isolirBatch/index.jsx)): Menu baru di bawah Utilitas untuk pencarian dan pengubahan status autentikasi broadband secara massal. Dapat diakses oleh admin dengan privilege `broadband.isolirCandidate`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Fitur **Isolir Batch** memungkinkan admin untuk mencari semua autentikasi broadband yang memiliki sejumlah minimum invoice belum lunas, kemudian mengubah statusnya secara massal (Nonaktif atau Nonaktif Hingga Pembayaran). Pencarian dapat dilakukan berdasarkan pelanggan, mitra, atau autentikasi langsung, dengan opsi filter pay support dan mitra tertentu.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Utilitas → Isolir Batch** di sidebar.
  2. Pilih **Dasar Pencarian** (Pelanggan / Mitra / Autentikasi).
  3. Isi **Minimal Invoice Belum Lunas** (contoh: 2 untuk mencari yang memiliki 2+ invoice unpaid).
  4. (Opsional) Pilih **Pay Support** dan **Mitra** untuk mempersempit pencarian.
  5. Klik tombol **Cari** — daftar kandidat akan ditampilkan dalam tabel.
  6. (Opsional) Hapus kandidat tertentu dari daftar dengan mengklik tombol hapus (🗑️) di baris yang bersangkutan.
  7. Pilih **Ubah Status Menjadi** (Nonaktif / Nonaktif Hingga Pembayaran).
  8. Klik tombol **Ubah Status X Autentikasi** untuk menerapkan perubahan secara massal.
