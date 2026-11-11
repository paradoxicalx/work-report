# 📝 Daily Work Report - Dedy S.N Putra (2026-07-22)

---

## 📅 Laporan Harian - 22 Juli 2026

---

## 🌿 Branch: `issue-161` — Integrasi WhatsApp Chat (Customer Service)

### 📌 Informasi Issue

- **Nomor Issue**: #161
- **Judul Issue**: Integrasi WhatsApp Chat — Customer Service Real-time Chat
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`6f06131`](../../commit/6f06131) — resolve #161 — Rabu, 22 Juli 2026, 23:41

- **Komponen yang Berubah**:
  - `backend/.env.example` [NEW]
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/waChat.controller.js` [NEW]
  - `backend/src/controllers/waInternal.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/waConversation.model.js` [NEW]
  - `backend/src/models/waMessage.model.js` [NEW]
  - `backend/src/models/waTemplate.model.js` [NEW]
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/waChat.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `backend/src/services/waChatContact.service.js` [NEW]
  - `backend/src/services/waChatLock.service.js` [NEW]
  - `backend/src/services/waChatSweep.service.js` [NEW]
  - `backend/src/services/waConversation.service.js` [NEW]
  - `backend/src/services/waMessage.service.js` [NEW]
  - `backend/src/services/waSender.service.js` [NEW]
  - `backend/src/services/waTemplate.service.js` [NEW]
  - `backend/src/sockets/admin.controller.js`
  - `backend/src/sockets/socket-io.js`
  - `backend/src/sockets/waChat.controller.js` [NEW]
  - `backend/src/utils/minio.js` [NEW]
  - `backend/src/utils/waChatSerializer.js` [NEW]
  - `backend/src/utils/waChatUtils.js` [NEW]
  - `backend/src/utils/waPhone.js` [NEW]
  - `cron-worker/src/jobs/processors/whatsappSweep.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/public/sounds/notification.wav` [NEW]
  - `frontend/src/app/navigation/baseNavigation.js`
  - `frontend/src/app/navigation/customerService.js` [NEW]
  - `frontend/src/app/navigation/index.js`
  - `frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx` [NEW]
  - `frontend/src/app/pages/customerService/chatHistory/index.jsx` [NEW]
  - `frontend/src/app/pages/customerService/chatHistory/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/customerService/chatHistory/schema/rows.jsx` [NEW]
  - `frontend/src/app/pages/customerService/messageTemplate/TemplateDrawer.jsx` [NEW]
  - `frontend/src/app/pages/customerService/messageTemplate/index.jsx` [NEW]
  - `frontend/src/app/pages/customerService/messageTemplate/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/customerService/messageTemplate/schema/formSchema.js` [NEW]
  - `frontend/src/app/pages/customerService/messageTemplate/schema/rows.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationItem.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/MessageBubble.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/MessageComposer.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/MessageList.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useChatLock.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useChatNotification.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useConversations.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useMessages.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/index.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/utils.js` [NEW]
  - `frontend/src/app/pages/users/customer/edit.jsx`
  - `frontend/src/app/router/customerService/chatHistoryRoute.jsx` [NEW]
  - `frontend/src/app/router/customerService/messageTemplateRoute.jsx` [NEW]
  - `frontend/src/app/router/customerService/whatsappChatRoute.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `whatsapp-api/.env.example`
  - `whatsapp-api/CHAT_IMPLEMENTATION_PLAN.md` [NEW]
  - `whatsapp-api/src/config.js`
  - `whatsapp-api/src/controllers/message.controller.js`
  - `whatsapp-api/src/controllers/webhook.controller.js`
  - `whatsapp-api/src/routes/sendMessage.route.js`
  - `whatsapp-api/src/server.js`
  - `whatsapp-api/src/services/backendBridge.service.js` [NEW]
  - `whatsapp-api/src/services/sendMediaMessage.service.js` [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - Implementasi lengkap fitur **WhatsApp Chat Real-time** untuk modul Customer Service, mencakup 3 modul utama (Backend, Frontend, WhatsApp API) dengan total **75 file** yang diubah/ditambahkan (**8.671 baris kode baru**).
  - **Backend**: Membuat 3 model Mongoose baru ([`waConversation`](backend/src/models/waConversation.model.js), [`waMessage`](backend/src/models/waMessage.model.js), [`waTemplate`](backend/src/models/waTemplate.model.js)) beserta 8 service layer ([`waChatContact`](backend/src/services/waChatContact.service.js), [`waChatLock`](backend/src/services/waChatLock.service.js), [`waChatSweep`](backend/src/services/waChatSweep.service.js), [`waConversation`](backend/src/services/waConversation.service.js), [`waMessage`](backend/src/services/waMessage.service.js), [`waSender`](backend/src/services/waSender.service.js), [`waTemplate`](backend/src/services/waTemplate.service.js)) dan 2 controller utama ([`waChat.controller`](backend/src/controllers/waChat.controller.js), [`waInternal.controller`](backend/src/controllers/waInternal.controller.js)). Membangun sistem chat locking (agent lock), sweep/mechanism untuk auto-assign conversation, notifikasi real-time via Socket.io ([`waChat.controller.js`](backend/src/sockets/waChat.controller.js)), serta utilitas pendukung seperti phone number normalization ([`waPhone`](backend/src/utils/waPhone.js)), serializer ([`waChatSerializer`](backend/src/utils/waChatSerializer.js)), dan helper functions ([`waChatUtils`](backend/src/utils/waChatUtils.js)).
  - **Frontend**: Membangun halaman WhatsApp Chat dengan arsitektur komponen modular — [`ConversationList`](frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx), [`MessageBubble`](frontend/src/app/pages/customerService/whatsappChat/components/MessageBubble.jsx), [`MessageComposer`](frontend/src/app/pages/customerService/whatsappChat/components/MessageComposer.jsx), [`ChatHeader`](frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx), [`QuickTemplatePopover`](frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx). Custom hooks untuk isolasi logika: [`useConversations`](frontend/src/app/pages/customerService/whatsappChat/hooks/useConversations.js), [`useMessages`](frontend/src/app/pages/customerService/whatsappChat/hooks/useMessages.js), [`useChatLock`](frontend/src/app/pages/customerService/whatsappChat/hooks/useChatLock.js), [`useChatNotification`](frontend/src/app/pages/customerService/whatsappChat/hooks/useChatNotification.js). Juga membuat halaman **Chat History** ([`chatHistory/index.jsx`](frontend/src/app/pages/customerService/chatHistory/index.jsx)) dengan [`TranscriptDrawer`](frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx) dan halaman **Message Template** ([`messageTemplate/index.jsx`](frontend/src/app/pages/customerService/messageTemplate/index.jsx)) dengan [`TemplateDrawer`](frontend/src/app/pages/customerService/messageTemplate/TemplateDrawer.jsx).
  - **WhatsApp API**: Menambahkan service [`backendBridge`](whatsapp-api/src/services/backendBridge.service.js) untuk komunikasi internal ke Backend dan [`sendMediaMessage`](whatsapp-api/src/services/sendMediaMessage.service.js) untuk pengiriman file/media. Memperluas route [`sendMessage.route.js`](whatsapp-api/src/routes/sendMessage.route.js) dan memperbarui webhook controller.
  - **Cron Worker**: Menambahkan job [`whatsappSweep`](cron-worker/src/jobs/processors/whatsappSweep.js) untuk scheduled cleanup/conversation sweep.
  - **Privilege**: Menambahkan 12 privilege baru terkait WhatsApp Chat (read, write, send, template, history, lock, dll).
  - **i18n**: Menambahkan 107 translation keys di frontend (id & en) dan 50 keys di backend.

---

## 🌿 Branch: `issue-156` — Sistem Manajemen Attendance

### 📌 Informasi Issue

- **Nomor Issue**: #156
- **Judul Issue**: Sistem Manajemen Attendance (Kehadiran)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`d918e55`](../../commit/d918e55) — save #156 — Rabu, 22 Juli 2026, 20:02

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/constants/customer.constant.js`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/auth.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/admin.route.js`
  - `backend/src/routes/attendance.route.js` [NEW]
  - `backend/src/routes/mobileCustomer.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/attendance.service.js`
  - `frontend/src/app/navigation/activities.js` [NEW]
  - `frontend/src/app/navigation/index.js`
  - `frontend/src/app/pages/activities/attendance/index.jsx` [NEW]
  - `frontend/src/app/pages/services/hotspotUser/detail.jsx`
  - `frontend/src/app/pages/users/customer/edit.jsx`
  - `frontend/src/app/router/activities/attendance.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/hooks/index.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - Implementasi halaman **Attendance** (Kehadiran) untuk admin/internal. Membuat route backend [`attendance.route.js`](backend/src/routes/attendance.route.js) dan memperluas [`attendance.controller.js`](backend/src/controllers/attendance.controller.js) dengan 162+ baris logika baru, serta menambahkan service [`attendance.service.js`](backend/src/services/attendance.service.js) (89 baris).
  - **Frontend**: Membuat halaman utama Attendance di [`attendance/index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx) (714 baris) dengan navigasi baru di [`activities.js`](frontend/src/app/navigation/activities.js) dan route [`attendance.jsx`](frontend/src/app/router/activities/attendance.jsx). Menambahkan custom hook baru di [`hooks/index.js`](frontend/src/hooks/index.js).
  - Memperbarui privilege config dan auth controller untuk mendukung fitur attendance.
  - **i18n**: Menambahkan 63 translation keys di frontend (id & en) dan 16 keys di backend.
  - **Status**: Work-in-progress (save) — belum di-merge.

---

## 🌿 Branch: `master` — Resolve #160 — Konfigurasi WhatsApp API & Settings

### 📌 Informasi Issue

- **Nomor Issue**: #160
- **Judul Issue**: Konfigurasi WhatsApp API & Settings System
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`53f0837`](../../commit/53f0837) — resolve #160 — Rabu, 22 Juli 2026, 19:11

#### [`b7b5c04`](../../commit/b7b5c04) — resolve #160 (merge) — Rabu, 22 Juli 2026, 19:15

- **Komponen yang Berubah**:
  - `backend/.env.example`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/routes/settings.route.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/whatsappControl.service.js` [NEW]
  - `frontend/src/app/pages/settings/schema/applicationSchema.js`
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx` [NEW]
  - `frontend/src/components/shared/form/Listbox.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `whatsapp-api/.dockerignore`
  - `whatsapp-api/.env.example`
  - `whatsapp-api/DOCUMENTATION.md` [NEW]
  - `whatsapp-api/Dockerfile`
  - `whatsapp-api/docker-compose.yml`
  - `whatsapp-api/src/config.js`
  - `whatsapp-api/src/controllers/internal.controller.js` [NEW]
  - `whatsapp-api/src/controllers/message.controller.js`
  - `whatsapp-api/src/controllers/profile.controller.js`
  - `whatsapp-api/src/controllers/reportWebhook.controller.js`
  - `whatsapp-api/src/controllers/template.controller.js`
  - `whatsapp-api/src/controllers/webhook.controller.js`
  - `whatsapp-api/src/database.js`
  - `whatsapp-api/src/middleware/authenticateAPI.js`
  - `whatsapp-api/src/middleware/authenticateInternal.js` [NEW]
  - `whatsapp-api/src/models/option.model.js` [NEW]
  - `whatsapp-api/src/routes/internal.route.js` [NEW]
  - `whatsapp-api/src/routes/profile.route.js`
  - `whatsapp-api/src/routes/sendMessage.route.js`
  - `whatsapp-api/src/routes/sendTemplate.route.js`
  - `whatsapp-api/src/routes/webhook.route.js`
  - `whatsapp-api/src/server.js`
  - `whatsapp-api/src/services/customer.service.js`
  - `whatsapp-api/src/services/isCustomer.service.js`
  - `whatsapp-api/src/services/notCustomer.service.js`
  - `whatsapp-api/src/services/profile.service.js`
  - `whatsapp-api/src/services/saveMessages.service.js`
  - `whatsapp-api/src/services/sendTemplateMessage.service.js`
  - `whatsapp-api/src/services/sendTextMessage.service.js`
  - `whatsapp-api/src/services/template.service.js` [NEW]
  - `whatsapp-api/src/swagger.js`
  - `whatsapp-api/src/utils/cleanObject.js`
  - `whatsapp-api/src/utils/generateTemplateComponents.js`
  - `whatsapp-api/src/utils/whatsappAPI.js`

- **Deskripsi Perubahan & Fungsi**:
  - Implementasi **sistem konfigurasi WhatsApp API** yang komprehensif. Di backend, menambahkan service [`whatsappControl.service.js`](backend/src/services/whatsappControl.service.js) (120 baris) dan memperluas [`settings.controller.js`](backend/src/controllers/settings.controller.js) dengan 121+ baris logika baru, serta route [`settings.route.js`](backend/src/routes/settings.route.js) (54 baris).
  - **Frontend**: Memperluas halaman Settings > System section dengan 287+ baris konfigurasi WhatsApp, menambahkan komponen preview template [`WhatsappTemplatePreview.jsx`](frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx) (159 baris), dan memperbarui [`Listbox.jsx`](frontend/src/components/shared/form/Listbox.jsx) untuk form handling yang lebih baik.
  - **WhatsApp API**: Refactor besar-besaran — menambahkan middleware autentikasi internal ([`authenticateInternal.js`](whatsapp-api/src/middleware/authenticateInternal.js)), model option ([`option.model.js`](whatsapp-api/src/models/option.model.js)), service template ([`template.service.js`](whatsapp-api/src/services/template.service.js)), dan route internal ([`internal.route.js`](whatsapp-api/src/routes/internal.route.js)). Memperbarui seluruh controller dan service yang ada untuk kompatibilitas dengan sistem konfigurasi baru. Menambahkan dokumentasi lengkap ([`DOCUMENTATION.md`](whatsapp-api/DOCUMENTATION.md), 550 baris) dan konfigurasi Docker yang diperbarui.
  - **i18n**: Menambahkan 43 translation keys di frontend.

---

## 🌿 Branch: `master` — Resolve #150 (Follow-up) — Radius Server Integration

### 📌 Informasi Issue

- **Nomor Issue**: #150
- **Judul Issue**: Integrasi Radius Server & gRPC
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`980944f`](../../commit/980944f) — resolve #150 — Rabu, 22 Juli 2026, 08:24

#### [`a58b044`](../../commit/a58b044) — resolve #150 (merge) — Rabu, 22 Juli 2026, 08:26

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`

- **Deskripsi Perubahan & Fungsi**:
  - Perbaikan follow-up untuk issue #150 — menambahkan 3 translation keys di backend (en & id) yang sebelumnya terlewat pada commit utama.

---

## 🌿 Branch: `issue-123` — Refactor Customer PO/SO (Audit)

### 📌 Informasi Issue

- **Nomor Issue**: #123
- **Judul Issue**: Refactor Customer Purchase Order & Sales Order (Audit)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`e9d480e`](../../commit/e9d480e) — resolve #123 (audit) — Rabu, 22 Juli 2026, 01:20

- **Komponen yang Berubah**:
  - `backend/package-lock.json`
  - `backend/package.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/constants/customer.constant.js`
  - `backend/src/controllers/customerPO.controller.js` [NEW]
  - `backend/src/controllers/customerQuotation.controller.js`
  - `backend/src/controllers/customerSO.controller.js` _(renamed from `customerDocument.controller.js`)_
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/publicCustomerPO.controller.js`
  - `backend/src/controllers/publicCustomerSO.controller.js`
  - `backend/src/controllers/publicQuotation.controller.js`
  - `backend/src/routes/customerDocument.route.js` _(deleted)_
  - `backend/src/routes/customerPO.route.js` [NEW]
  - `backend/src/routes/customerQuotation.route.js`
  - `backend/src/routes/customerSO.route.js` [NEW]
  - `backend/src/routes/public.route.js`
  - `backend/src/services/customerPO.service.js` _(renamed from `customerDocument.service.js`)_
  - `backend/src/services/customerQuotation.service.js`
  - `backend/src/services/customerSO.service.js` [NEW]
  - `frontend/package.json`
  - `frontend/src/app/pages/public/PublicCustomerSODocument.jsx`
  - `frontend/src/app/pages/public/ReviewCustomerPOPage.jsx`
  - `frontend/src/app/pages/public/ReviewCustomerSOPage.jsx`
  - `frontend/src/app/pages/users/business/profile.jsx`
  - `frontend/src/app/pages/users/customerSalesOrder/create.jsx`
  - `frontend/src/app/pages/users/customerSalesOrder/edit.jsx`
  - `frontend/src/app/pages/users/partner/profile.jsx`
  - `frontend/src/app/pages/users/quotation/create.jsx`
  - `frontend/src/components/shared/form/SelectCards.jsx` [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - Refactor besar-besaran dokumen Customer — **memisahkan** module PO (Purchase Order) dan SO (Sales Order) yang sebelumnya digabung dalam satu controller/service (`customerDocument`) menjadi module terpisah.
  - **Backend**: Membuat [`customerPO.controller.js`](backend/src/controllers/customerPO.controller.js) (397 baris) dan [`customerSO.controller.js`](backend/src/controllers/customerSO.controller.js) sebagai pengganti `customerDocument.controller.js`. Route [`customerDocument.route.js`](backend/src/routes/customerDocument.route.js) dihapus dan digantikan oleh [`customerPO.route.js`](backend/src/routes/customerPO.route.js) (394 baris) dan [`customerSO.route.js`](backend/src/routes/customerSO.route.js) (526 baris). Service [`customerSO.service.js`](backend/src/services/customerSO.service.js) (250 baris) dibuat terpisah. Menambahkan 36 privilege baru.
  - **Frontend**: Memperbarui halaman publik review PO/SO, profile business/partner, form create/edit SO dan quotation. Menambahkan komponen [`SelectCards.jsx`](frontend/src/components/shared/form/SelectCards.jsx) (46 baris).
  - Total: **1.898 baris baru, 1.169 baris dihapus** — bersihkan technical debt dari struktur document yang terlalu monolitis.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                     | Dampak Utama                                                                      |
| ----- | ------------------------- | --------------------------------------------------------------------------------- |
| #161  | Integrasi WhatsApp Chat   | Sistem chat real-time WhatsApp antara admin/customer service dan pelanggan        |
| #156  | Sistem Attendance         | Halaman manajemen kehadiran karyawan/admin                                        |
| #160  | Konfigurasi WhatsApp API  | Pengaturan koneksi WhatsApp API dan manajemen template pesan dari dashboard admin |
| #150  | Radius Server Integration | Perbaikan follow-up integrasi radius server (translation keys)                    |
| #123  | Refactor Customer PO/SO   | Pemisahan dokumen PO dan SO menjadi modul terpisah yang lebih terorganisir        |

### Kemampuan Baru Pengguna/Admin

- **Chat WhatsApp Real-time**: Admin/customer service sekarang dapat melakukan percakapan langsung dengan pelanggan melalui WhatsApp dari dalam dashboard. Mendukung fitur agent lock (mencegah beberapa admin membalas chat yang sama), notifikasi suara, dan pengiriman file/media.
- **Manajemen Attendance**: Admin dapat mengelola dan memantau kehadiran karyawan melalui halaman Attendance yang baru.
- **Konfigurasi WhatsApp dari Dashboard**: Admin dapat mengatur koneksi WhatsApp API, mengelola template pesan, dan mempratinjau template langsung dari halaman Settings > System.
- **Refactored PO/SO Documents**: Dokumen Purchase Order dan Sales Order kini dikelola secara terpisah dengan API dan UI yang lebih terorganisir, mengurangi kompleksitas codebase.

### Bug Fix / Solusi Masalah

- Perbaikan translation keys yang terlewat pada integrasi Radius Server (issue #150).
- Refactor dokumen PO/SO dari monolitik menjadi modular untuk mengurangi technical debt dan meningkatkan maintainability.

### Menu/Fitur Baru

- **Customer Service > WhatsApp Chat**: Halaman chat real-time dengan daftar percakapan, bubbles pesan, composer, dan quick template.
- **Customer Service > Chat History**: Halaman riwayat percakapan dengan drawer transkrip.
- **Customer Service > Message Template**: Halaman manajemen template pesan WhatsApp.
- **Activities > Attendance**: Halaman manajemen kehadiran karyawan.
- **Settings > System > WhatsApp Configuration**: Bagian konfigurasi WhatsApp API di halaman Settings.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### WhatsApp Chat (Issue #161)

- **Penjelasan Fitur**: Fitur WhatsApp Chat memungkinkan admin/customer service berkomunikasi langsung dengan pelanggan melalui WhatsApp secara real-time dari dalam dashboard DEKASIMAL. Sistem menggunakan arsitektur WebSocket (Socket.io) untuk pesan real-time, dengan mekanisme agent lock untuk mencegah konflik balasan, dan sweep mechanism untuk auto-assign percakapan yang belum ditangani.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Customer Service > WhatsApp Chat** di sidebar navigation.
  2. Panel kiri menampilkan daftar percakapan aktif — klik untuk membuka chat.
  3. Ketik pesan di kolom composer di bagian bawah, atau gunakan **Quick Template** (ikon template) untuk pesan cepat.
  4. Untuk mengirim file/gambar, gunakan tombol lampirkan di composer.
  5. Notifikasi suara akan berbunyi saat ada pesan masuk dari pelanggan.
  6. Untuk melihat riwayat percakapan lengkap, buka **Customer Service > Chat History**.

### Attendance (Issue #156)

- **Penjelasan Fitur**: Halaman Attendance menyediakan interface untuk memantau dan mengelola kehadiran karyawan/admin dengan tampilan tabel yang terintegrasi dengan filter dan pencarian.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Activities > Attendance** di sidebar navigation.
  2. Gunakan filter di tabel untuk mencari data kehadiran berdasarkan nama, tanggal, atau status.
  3. Klik baris tertentu untuk melihat detail kehadiran.
