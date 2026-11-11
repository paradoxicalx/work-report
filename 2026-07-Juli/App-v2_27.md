# 📝 Daily Work Report - Dedy S.N Putra (2026-07-27)

---

## 📅 Laporan Harian - 27 Juli 2026

---

## 🌿 Branch: `master` (via `issue-162`) — WhatsApp Chat Feature

### 📌 Informasi Issue

- **Nomor Issue**: #161
- **Judul Issue**: WhatsApp Chat — Sistem Real-Time Chat WhatsApp Terintegrasi
- **Status Branch**: `Sudah di-merge` ke master

### 📅 Rincian Commit

#### [`79eddc6`](https://github.com/user/dekasimal-v2/commit/79eddc6) - resolve #161 - 27 Juli 2026, 07:11 WIB

- **Komponen yang Berubah** (126 berkas, +12.290 / -294 baris):

  **Backend — Model & Schema:**
  - [`backend/src/models/waConversation.model.js`](backend/src/models/waConversation.model.js) [NEW]
  - [`backend/src/models/waMessage.model.js`](backend/src/models/waMessage.model.js) [NEW]
  - [`backend/src/models/waTemplate.model.js`](backend/src/models/waTemplate.model.js) [NEW]

  **Backend — Controller & Route:**
  - [`backend/src/controllers/waChat.controller.js`](backend/src/controllers/waChat.controller.js) [NEW] _(849 baris — WebSocket event handler untuk chat real-time)_
  - [`backend/src/controllers/waInternal.controller.js`](backend/src/controllers/waInternal.controller.js) [NEW] _(464 baris — endpoint internal untuk integrasi WhatsApp API)_
  - [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js) _(penambahan endpoint cron untuk WhatsApp sweep)_
  - [`backend/src/controllers/settings.controller.js`](backend/src/controllers/settings.controller.js) _(penambahan pengaturan WhatsApp)_
  - [`backend/src/controllers/auth.controller.js`](backend/src/controllers/auth.controller.js) _(penyesuaian autentikasi)_
  - [`backend/src/routes/waChat.route.js`](backend/src/routes/waChat.route.js) [NEW] _(577 baris — definisi route lengkap untuk WhatsApp Chat)_
  - [`backend/src/routes/internal.route.js`](backend/src/routes/internal.route.js) _(penambahan route internal)_
  - [`backend/src/routes/settings.route.js`](backend/src/routes/settings.route.js) _(penambahan route pengaturan)_

  **Backend — Service Layer:**
  - [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) [NEW] _(303 baris — manajemen percakapan)_
  - [`backend/src/services/waMessage.service.js`](backend/src/services/waMessage.service.js) [NEW] _(203 baris — manajemen pesan)_
  - [`backend/src/services/waTemplate.service.js`](backend/src/services/waTemplate.service.js) [NEW] _(133 baris — manajemen template)_
  - [`backend/src/services/waTemplateVariable.service.js`](backend/src/services/waTemplateVariable.service.js) [NEW] _(190 baris — pemetaan variabel template)_
  - [`backend/src/services/waChatSweep.service.js`](backend/src/services/waChatSweep.service.js) [NEW] _(459 baris — sweeping pesan WhatsApp dari Meta API)_
  - [`backend/src/services/waChatLock.service.js`](backend/srcservices/waChatLock.service.js) [NEW] _(172 baris — mekanisme locking percakapan)_
  - [`backend/src/services/waChatContact.service.js`](backend/src/services/waChatContact.service.js) [NEW] _(68 baris — manajemen kontak chat)_
  - [`backend/srcservices/waSender.service.js`](backend/src/services/waSender.service.js) _(penambahan fungsi pengiriman)_
  - [`backend/src/services/whatsappControl.service.js`](backend/src/services/whatsappControl.service.js) _(penyesuaian kontrol WhatsApp)_

  **Backend — Socket & Utilitas:**
  - [`backend/src/sockets/waChat.controller.js`](backend/src/sockets/waChat.controller.js) [NEW] _(240 baris — event socket.io untuk chat real-time)_
  - [`backend/src/sockets/socket-io.js`](backend/src/sockets/socket-io.js) _(penambahan namespace WhatsApp)_
  - [`backend/src/sockets/admin.controller.js`](backend/src/sockets/admin.controller.js) _(penyesuaian socket admin)_
  - [`backend/src/utils/waChatSerializer.js`](backend/src/utils/waChatSerializer.js) [NEW] _(144 baris — serialisasi data chat)_
  - [`backend/src/utils/waChatUtils.js`](backend/src/utils/waChatUtils.js) [NEW] _(362 baris — utilitas pendukung chat)_
  - [`backend/src/utils/waPhone.js`](backend/src/utils/waPhone.js) [NEW] _(75 baris — normalisasi nomor telepon)_
  - [`backend/src/utils/minio.js`](backend/src/utils/minio.js) _(penyesuaian upload media)_
  - [`backend/src/middlewares/auth.middleware.js`](backend/src/middlewares/auth.middleware.js) _(penyesuaian autentikasi)_
  - [`backend/src/grpc/server.js`](backend/src/grpc/server.js) _(penyesuaian gRPC)_
  - [`backend/src/grpc/streamRegistry.js`](backend/src/grpc/streamRegistry.js) _(penyesuaian stream)_

  **Backend — Konfigurasi:**
  - [`backend/.env.example`](backend/.env.example) _(penambahan variabel environment WhatsApp)_
  - [`backend/src/app.js`](backend/src/app.js) _(registrasi route baru)_
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) _(65 key baru)_
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) _(65 key baru)_

  **Frontend — Halaman WhatsApp Chat:**
  - [`frontend/src/app/pages/customerService/whatsappChat/index.jsx`](frontend/src/app/pages/customerService/whatsappChat/index.jsx) [NEW] _(250 baris — halaman utama chat)_
  - [`frontend/src/app/pages/customerService/whatsappChat/utils.js`](frontend/src/app/pages/customerService/whatsappChat/utils.js) [NEW] _(293 baris — utilitas chat)_
  - [`frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/ConversationItem.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ConversationItem.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/EmojiPopover.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/EmojiPopover.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/MediaLightbox.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/MediaLightbox.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/MessageBubble.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/MessageBubble.jsx) [NEW] _(379 baris — komponen gelembung pesan)_
  - [`frontend/src/app/pages/customerService/whatsappChat/components/MessageComposer.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/MessageComposer.jsx) [NEW] _(323 baris — area penulisan pesan)_
  - [`frontend/src/app/pages/customerService/whatsappChat/components/MessageList.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/MessageList.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/OnlineAdminsFooter.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/OnlineAdminsFooter.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/QuickTemplatePopover.jsx) [NEW]

  **Frontend — Hook WhatsApp Chat:**
  - [`frontend/src/app/pages/customerService/whatsappChat/hooks/useChatLock.js`](frontend/src/app/pages/customerService/whatsappChat/hooks/useChatLock.js) [NEW] _(160 baris — mekanisme locking percakapan)_
  - [`frontend/src/app/pages/customerService/whatsappChat/hooks/useChatNotification.js`](frontend/src/app/pages/customerService/whatsappChat/hooks/useChatNotification.js) [NEW] _(101 baris — notifikasi chat)_
  - [`frontend/src/app/pages/customerService/whatsappChat/hooks/useConversations.js`](frontend/src/app/pages/customerService/whatsappChat/hooks/useConversations.js) [NEW] _(163 baris — manajemen daftar percakapan)_
  - [`frontend/src/app/pages/customerService/whatsappChat/hooks/useMessages.js`](frontend/src/app/pages/customerService/whatsappChat/hooks/useMessages.js) [NEW] _(166 baris — manajemen pesan)_
  - [`frontend/src/app/pages/customerService/whatsappChat/hooks/useWaAccount.js`](frontend/src/app/pages/customerService/whatsappChat/hooks/useWaAccount.js) [NEW]

  **Frontend — Chat History & Message Template:**
  - [`frontend/src/app/pages/customerService/chatHistory/index.jsx`](frontend/src/app/pages/customerService/chatHistory/index.jsx) [NEW] _(110 baris)_
  - [`frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx`](frontend/src/app/pages/customerService/chatHistory/TranscriptDrawer.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/chatHistory/schema/columns.jsx`](frontend/src/app/pages/customerService/chatHistory/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/chatHistory/schema/rows.jsx`](frontend/src/app/pages/customerService/chatHistory/schema/rows.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/index.jsx`](frontend/src/app/pages/customerService/messageTemplate/index.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/TemplateDrawer.jsx`](frontend/src/app/pages/customerService/messageTemplate/TemplateDrawer.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/schema/columns.jsx`](frontend/src/app/pages/customerService/messageTemplate/schema/columns.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/schema/rows.jsx`](frontend/src/app/pages/customerService/messageTemplate/schema/rows.jsx) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/schema/formSchema.js`](frontend/src/app/pages/customerService/messageTemplate/schema/formSchema.js) [NEW]
  - [`frontend/src/app/pages/customerService/messageTemplate/schema/TemplateContentCell.jsx`](frontend/src/app/pages/customerService/messageTemplate/schema/TemplateContentCell.jsx) [NEW]

  **Frontend — Routing & Navigasi:**
  - [`frontend/src/app/navigation/customerService.js`](frontend/src/app/navigation/customerService.js) _(penambahan menu WhatsApp Chat, Chat History, Message Template)_
  - [`frontend/src/app/navigation/baseNavigation.js`](frontend/src/app/navigation/baseNavigation.js) _(penyesuaian navigasi)_
  - [`frontend/src/app/navigation/index.js`](frontend/src/app/navigation/index.js) _(registrasi modul baru)_
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) _(registrasi route WhatsApp)_
  - [`frontend/src/app/router/customerService/whatsappChatRoute.jsx`](frontend/src/app/router/customerService/whatsappChatRoute.jsx) [NEW]
  - [`frontend/src/app/router/customerService/messageTemplateRoute.jsx`](frontend/src/app/router/customerService/messageTemplateRoute.jsx) [NEW]
  - [`frontend/src/app/router/customerService/chatHistoryRoute.jsx`](frontend/src/app/router/customerService/chatHistoryRoute.jsx) [NEW]

  **Frontend — Socket & Context:**
  - [`frontend/src/app/contexts/socket/Provider.jsx`](frontend/src/app/contexts/socket/Provider.jsx) _(penambahan event WhatsApp)_
  - [`frontend/src/app/layouts/MainLayout/Header/index.jsx`](frontend/src/app/layouts/MainLayout/Header/index.jsx) _(penambahan indicator chat)_

  **Frontend — Dashboard Radius (Optimasi):**
  - [`frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx`](frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/components/ServerList.jsx`](frontend/src/app/pages/dashboards/radius/components/ServerList.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx`](frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js`](frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx) _(refactor)_
  - [`frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx`](frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx) _(refactor)_

  **Frontend — Pengaturan & I18n:**
  - [`frontend/src/app/pages/settings/schema/systemSchema.js`](frontend/src/app/pages/settings/schema/systemSchema.js) _(penambahan field WhatsApp)_
  - [`frontend/src/app/pages/settings/sections/System.jsx`](frontend/src/app/pages/settings/sections/System.jsx) _(222 baris tambahan — pengaturan WhatsApp)_
  - [`frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx`](frontend/src/app/pages/settings/sections/WhatsappTemplatePreview.jsx) _(penyesuaian)_
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) _(180+ key baru)_
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) _(180+ key baru)_
  - [`frontend/src/styles/app/components/whatsappChat.css`](frontend/src/styles/app/components/whatsappChat.css) [NEW] _(20 baris — gaya chat)_
  - [`frontend/src/styles/app/index.css`](frontend/src/styles/app/index.css) _(registrasi style baru)_
  - [`frontend/src/assets/wa-chat-bg.png`](frontend/src/assets/wa-chat-bg.png) [NEW] _(wallpaper chat mode terang)_
  - [`frontend/src/assets/wa-chat-bg-dark.png`](frontend/src/assets/wa-chat-bg-dark.png) [NEW] _(wallpaper chat mode gelap)_
  - [`frontend/public/sounds/notification.wav`](frontend/public/sounds/notification.wav) [NEW] _(suara notifikasi)_

  **Cron Worker:**
  - [`cron-worker/src/jobs/processors/whatsappSweep.js`](cron-worker/src/jobs/processors/whatsappSweep.js) [NEW] _(processor untuk sweeping pesan)_
  - [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js) _(penambahan jadwal sweep)_
  - [`cron-worker/src/jobs/worker.js`](cron-worker/src/jobs/worker.js) _(penambahan worker handler)_
  - [`cron-worker/src/services/api.service.js`](cron-worker/src/services/api.service.js) _(penambahan API call ke backend)_

  **WhatsApp API:**
  - [`whatsapp-api/src/controllers/message.controller.js`](whatsapp-api/src/controllers/message.controller.js) _(penyesuaian handler pesan)_
  - [`whatsapp-api/src/controllers/webhook.controller.js`](whatsapp-api/src/controllers/webhook.controller.js) _(penyesuaian webhook)_
  - [`whatsapp-api/src/routes/sendMessage.route.js`](whatsapp-api/src/routes/sendMessage.route.js) _(penambahan endpoint)_
  - [`whatsapp-api/src/services/backendBridge.service.js`](whatsapp-api/src/services/backendBridge.service.js) [NEW] _(169 baris — jembatan komunikasi ke Backend)_
  - [`whatsapp-api/src/services/sendMediaMessage.service.js`](whatsapp-api/src/services/sendMediaMessage.service.js) [NEW] _(175 baris — pengiriman media)_
  - [`whatsapp-api/src/config.js`](whatsapp-api/src/config.js) _(penyesuaian konfigurasi)_
  - [`whatsapp-api/src/database.js`](whatsapp-api/src/database.js) _(penyesuaian koneksi DB)_
  - [`whatsapp-api/src/server.js`](whatsapp-api/src/server.js) _(penyesuaian server)_
  - [`whatsapp-api/.env.example`](whatsapp-api/.env.example) _(template environment)_
  - [`whatsapp-api/CHAT_IMPLEMENTATION_PLAN.md`](whatsapp-api/CHAT_IMPLEMENTATION_PLAN.md) [NEW] _(dokumentasi rencana implementasi)_
  - [`whatsapp-api/scripts/sim-webhook.sh`](whatsapp-api/scripts/sim-webhook.sh) [NEW] _(167 baris — skrip simulasi webhook untuk testing)_

  **Root:**
  - [`package.json`](package.json) _(penambahan skrip)_

- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan fitur **WhatsApp Chat Real-Time** yang terintegrasi penuh ke dalam sistem DEKASIMAL V2.
  - **Backend**: Membangun arsitektur chat end-to-end meliputi model percakapan (`WaConversation`), pesan (`WaMessage`), dan template (`WaTemplate`), beserta layanan pendukung untuk sweeping pesan dari Meta API, locking percakapan, manajemen kontak, dan serialisasi data. Endpoint WebSocket (`waChat.route.js` — 577 baris) dan controller internal (`waInternal.controller.js` — 464 baris) dibangun untuk komunikasi real-time dan integrasi webhook.
  - **Frontend**: Membangun antarmuka chat lengkap menyerupai WhatsApp Web dengan komponen `MessageBubble`, `MessageComposer`, `ConversationList`, `EmojiPopover`, `MediaLightbox`, dan `QuickTemplatePopover`. Sistem hook (`useConversations`, `useMessages`, `useChatLock`, `useChatNotification`) mengelola state chat secara terisolasi. Ditambahkan juga halaman **Chat History** (riwayat transkrip percakapan) dan **Message Template** (manajemen template pesan).
  - **Cron Worker**: Menambahkan job `whatsappSweep` untuk melakukan sweeping berkala pesan masuk dari Meta WhatsApp API ke database lokal.
  - **WhatsApp API**: Mengembangkan layanan `backendBridge` untuk menjembatani komunikasi antara WhatsApp API dan Backend utama, serta `sendMediaMessage` untuk pengiriman file media (gambar, dokumen, video).
  - **Dashboard Radius**: Melakukan refactor komponen dashboard untuk peningkatan performa dan konsistensi kode.

---

## 🌿 Branch: `issue-162` (Work in Progress) — WhatsApp Broadcast & Pengaturan Notifikasi

### 📌 Informasi Issue

- **Nomor Issue**: #162
- **Judul Issue**: WhatsApp Broadcast — Siaran Pesan Massal & Pengaturan Notifikasi Tagihan
- **Status Branch**: `Belum di-merge` (Pekerjaan sedang berlangsung — belum di-commit)

### 📋 Rincian Perubahan (Uncommitted Changes)

> **Catatan**: Pekerjaan ini merupakan kelanjutan dari issue #161 dan masih dalam tahap pengembangan (Work in Progress). Seluruh perubahan di bawah ini belum di-commit.

#### Komponen yang Berubah (29 berkas dimodifikasi + 14 berkas baru):

**Backend — WhatsApp Broadcast (Baru):**

- [`backend/src/controllers/waBroadcast.controller.js`](backend/src/controllers/waBroadcast.controller.js) [NEW] _(controller untuk operasi broadcast)_
- [`backend/src/models/waBroadcast.model.js`](backend/src/models/waBroadcast.model.js) [NEW] _(model data siaran)_
- [`backend/src/models/waBroadcastRecipient.model.js`](backend/src/models/waBroadcastRecipient.model.js) [NEW] _(model penerima siaran)_
- [`backend/src/routes/waBroadcast.route.js`](backend/src/routes/waBroadcast.route.js) [NEW] _(route API broadcast)_
- [`backend/src/services/waBroadcast.service.js`](backend/src/services/waBroadcast.service.js) [NEW] _(layanan bisnis broadcast)_
- [`backend/src/services/waBroadcastQueue.service.js`](backend/src/services/waBroadcastQueue.service.js) [NEW] _(layanan antrian broadcast)_
- [`backend/src/lib/queueConnection.js`](backend/src/lib/queueConnection.js) [NEW] _(koneksi BullMQ/Redis untuk queue)_
- [`backend/src/utils/serviceStatusLabels.js`](backend/src/utils/serviceStatusLabels.js) [NEW] _(label status layanan)_

**Backend — Modifikasi:**

- [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js) _(penambahan endpoint cron untuk dispatch, send, dan reminder check broadcast)_
- [`backend/src/controllers/settings.controller.js`](backend/src/controllers/settings.controller.js) _(penambahan field pengaturan notifikasi WhatsApp)_
- [`backend/src/services/option.service.js`](backend/src/services/option.service.js) _(penyesuaian layanan opsi pengaturan)_
- [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) _(penyesuaian)_
- [`backend/src/services/waSender.service.js`](backend/src/services/waSender.service.js) _(penambahan fungsi `sendWhatsappTemplate` untuk mengirim template generik)_
- [`backend/src/services/waTemplateVariable.service.js`](backend/src/services/waTemplateVariable.service.js) _(penyesuaian variabel template)_
- [`backend/src/routes/internal.route.js`](backend/src/routes/internal.route.js) _(penambahan route cron broadcast)_
- [`backend/src/models/waConversation.model.js`](backend/srcmodels/waConversation.model.js) _(penyesuaian skema)_
- [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) _(8 key baru)_
- [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) _(8 key baru)_
- [`backend/src/app.js`](backend/src/app.js) _(registrasi route broadcast)_

**Frontend — WhatsApp Broadcast (Baru):**

- [`frontend/src/app/pages/customerService/whatsappBroadcast/index.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/index.jsx) [NEW] _(halaman utama daftar siaran)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/CreateBroadcastDrawer.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/CreateBroadcastDrawer.jsx) [NEW] _(drawer pembuatan siaran baru)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/BroadcastDetailDrawer.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/BroadcastDetailDrawer.jsx) [NEW] _(drawer detail siaran)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/RecipientPicker.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/RecipientPicker.jsx) [NEW] _(pemilihan penerima siaran)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/AccumulatedRecipientList.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/AccumulatedRecipientList.jsx) [NEW] _(daftar akumulasi penerima)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableMapper.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableMapper.jsx) [NEW] _(pemetaan variabel template)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableDocsModal.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/components/TemplateVariableDocsModal.jsx) [NEW] _(dokumentasi variabel template)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/columns.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/columns.jsx) [NEW] _(kolom tabel siaran)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/recipientColumns.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/recipientColumns.jsx) [NEW] _(kolom tabel penerima)_
- [`frontend/src/app/pages/customerService/whatsappBroadcast/schema/rows.jsx`](frontend/src/app/pages/customerService/whatsappBroadcast/schema/rows.jsx) [NEW] _(wrapper sel tabel)_
- [`frontend/src/app/router/customerService/whatsappBroadcastRoute.jsx`](frontend/src/app/router/customerService/whatsappBroadcastRoute.jsx) [NEW] _(route halaman broadcast)_

**Frontend — Modifikasi:**

- [`frontend/src/app/navigation/customerService.js`](frontend/src/app/navigation/customerService.js) _(penambahan menu WhatsApp Broadcast dengan ikon Megaphone)_
- [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) _(registrasi route broadcast)_
- [`frontend/src/app/pages/settings/sections/Application.jsx`](frontend/src/app/pages/settings/sections/Application.jsx) _(penghapusan bagian pengaturan notifikasi WhatsApp dari halaman Aplikasi)_
- [`frontend/src/app/pages/settings/sections/System.jsx`](frontend/src/app/pages/settings/sections/System.jsx) _(penambahan bagian pengaturan notifikasi WhatsApp: hari pengingat, waktu pengingat, toggle aktifkan pengingat & pembayaran)_
- [`frontend/src/app/pages/settings/schema/applicationSchema.js`](frontend/src/app/pages/settings/schema/applicationSchema.js) _(penghapusan field notifikasi WhatsApp)_
- [`frontend/src/app/pages/settings/schema/systemSchema.js`](frontend/src/app/pages/settings/schema/systemSchema.js) _(penambahan field notifikasi WhatsApp)_
- [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) _(125 key baru)_
- [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) _(165 key baru)_

**Cron Worker — Broadcast Jobs (Baru):**

- [`cron-worker/src/jobs/processors/waBroadcastDispatch.js`](cron-worker/src/jobs/processors/waBroadcastDispatch.js) [NEW] _(processor dispatch siaran — mengevaluasi penerima dan membuat job pengiriman)_
- [`cron-worker/src/jobs/processors/waBroadcastSend.js`](cron-worker/src/jobs/processors/waBroadcastSend.js) [NEW] _(processor pengiriman template ke satu penerima)_
- [`cron-worker/src/jobs/processors/waBroadcastReminderCheck.js`](cron-worker/src/jobs/processors/waBroadcastReminderCheck.js) [NEW] _(processor pengecekan reminder tagihan)_
- [`cron-worker/src/jobs/waBroadcastWorkers.js`](cron-worker/src/jobs/waBroadcastWorkers.js) [NEW] _(inisialisasi worker broadcast)_
- [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js) _(penambahan jadwal cron reminder check tiap 5 menit)_
- [`cron-worker/src/jobs/worker.js`](cron-worker/src/jobs/worker.js) _(penambahan handler worker broadcast)_
- [`cron-worker/src/services/api.service.js`](cron-worker/src/services/api.service.js) _(58 baris tambahan — API calls untuk broadcast)_

**WhatsApp API — Refactor Template:**

- [`whatsapp-api/src/controllers/template.controller.js`](whatsapp-api/src/controllers/template.controller.js) _(mengubah endpoint `sendTemplateBilling` menjadi `sendTemplate` yang generik — tidak terikat ke satu template tertentu)_
- [`whatsapp-api/src/routes/sendTemplate.route.js`](whatsapp-api/src/routes/sendTemplate.route.js) _(mengubah route `/send-billing` menjadi `/send` dengan parameter `template_name` dan `language`)_
- [`whatsapp-api/src/config.js`](whatsapp-api/src/config.js) _(penghapusan konfigurasi `TEMPLATE_BILLING`/`LANGUAGE_BILLING`)_
- [`whatsapp-api/.env.example`](whatsapp-api/.env.example) _(penghapusan variabel template billing)_

- **Deskripsi Perubahan & Fungsi**:
  - **Fitur WhatsApp Broadcast**: Mengembangkan sistem siaran pesan massal WhatsApp yang memungkinkan admin mengirim template pesan Meta yang sudah disetujui ke daftar penerima yang dipilih secara manual (pelanggan, mitra, atau berdasarkan tagihan). Sistem ini dibangun di atas arsitektur queue (BullMQ) untuk pengiriman yang reliable dengan mekanisme retry.
  - **Dispatcher & Worker Pattern**: Cron worker menjalankan job dispatch tiap interval untuk mengevaluasi ulang penerima dan membuat job pengiriman individual. Setiap job pengiriman (`waBroadcastSend`) mengirim template ke satu penerima dengan dukungan retry dan backoff.
  - **Pengaturan Notifikasi Tagihan**: Memindahkan pengaturan notifikasi WhatsApp (hari pengingat, waktu pengingat, toggle aktif) dari halaman **Aplikasi** ke halaman **Sistem** untuk organisasi yang lebih baik. Input waktu pengingat menggunakan `DatePicker` dengan format 24 jam.
  - **WhatsApp API — Template Generik**: Mengubah endpoint `/template/send-billing` menjadi `/template/send` yang bersifat generik, menerima parameter `template_name` dan `language` dari pemanggil (backend), sehingga dapat digunakan oleh berbagai keperluan (siaran kustom, reminder tagihan, dll).
  - **Sweep Otomatis**: Menambahkan mekanisme `sweepStuckBroadcastRecipients` yang berjalan bersamaan dengan pengecekan reminder untuk memulihkan penerima yang tersangkut dalam status `processing`.
  - **Frontend — CreateBroadcastDrawer**: Membangun drawer pembuatan siaran dengan pemilihan penerima (daftar pelanggan/mitra), pemetaan variabel template, dan pratinjau sebelum mengirim.

---

## 🌿 Branch: `issue-165` — Attendance Detail, Paid Leave & Permission

### 📌 Informasi Issue

- **Nomor Issue**: #165
- **Judul Issue**: Modul Attendance — Detail Permintaan Izin/Cuti & Halaman Paid Leave & Permission
- **Status Branch**: `Belum di-merge` (Sudah di-push ke remote)

### 📅 Rincian Commit

#### [`e1004ee`](https://github.com/user/dekasimal-v2/commit/e1004ee) - resolve #165 - 27 Juli 2026, 23:43 WIB

- **Komponen yang Berubah** (21 berkas, +1.061 / -5 baris):

  **Backend — Controller & Route (Baru):**
  - [`backend/src/controllers/attendance.controller.js`](backend/src/controllers/attendance.controller.js) [NEW] _(72 baris — endpoint untuk data permintaan izin/cuti)_
  - [`backend/src/controllers/files.controller.js`](backend/src/controllers/files.controller.js) [NEW] _(23 baris — endpoint pengambilan file lampiran)_
  - [`backend/src/routes/attendance.route.js`](backend/src/routes/attendance.route.js) [NEW] _(116 baris — route API attendance lengkap)_
  - [`backend/src/routes/files.route.js`](backend/src/routes/files.route.js) [NEW] _(27 baris — route untuk pengambilan file)_

  **Backend — Service:**
  - [`backend/src/services/attendance.service.js`](backend/src/services/attendance.service.js) [NEW] _(56 baris — layanan data attendance)_
  - [`backend/src/config/privilege.json`](backend/src/config/privilege.json) _(penambahan privilege baru)_

  **Frontend — Modal Detail Attendance (Baru):**
  - [`frontend/src/app/pages/activities/attendance/components/AttendanceRequestDetailModal.jsx`](frontend/src/app/pages/activities/attendance/components/AttendanceRequestDetailModal.jsx) [NEW] _(313 baris — modal detail permintaan izin/cuti dengan informasi lengkap pohon izin)_

  **Frontend — Halaman Paid Leave & Permission (Baru):**
  - [`frontend/src/app/pages/activities/paidLeave/index.jsx`](frontend/src/app/pages/activities/paidLeave/index.jsx) [NEW] _(44 baris — halaman daftar cuti berbayar)_
  - [`frontend/src/app/pages/activities/paidLeave/schema/columns.jsx`](frontend/src/app/pages/activities/paidLeave/schema/columns.jsx) [NEW] _(83 baris — konfigurasi kolom tabel cuti)_
  - [`frontend/src/app/pages/activities/permission/index.jsx`](frontend/src/app/pages/activities/permission/index.jsx) [NEW] _(44 baris — halaman daftar izin)_
  - [`frontend/src/app/pages/activities/permission/schema/columns.jsx`](frontend/src/app/pages/activities/permission/schema/columns.jsx) [NEW] _(83 baris — konfigurasi kolom tabel izin)_

  **Frontend — Routing:**
  - [`frontend/src/app/router/activities/paidLeave.jsx`](frontend/src/app/router/activities/paidLeave.jsx) [NEW] _(14 baris — route halaman cuti)_
  - [`frontend/src/app/router/activities/permission.jsx`](frontend/src/app/router/activities/permission.jsx) [NEW] _(14 baris — route halaman izin)_
  - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) _(registrasi route baru)_

  **Frontend — Navigasi:**
  - [`frontend/src/app/navigation/activities.js`](frontend/src/app/navigation/activities.js) _(penambahan menu Paid Leave & Permission)_
  - [`frontend/src/app/pages/activities/attendance/index.jsx`](frontend/src/app/pages/activities/attendance/index.jsx) _(penyesuaian halaman attendance)_

  **Frontend — Komponen Shared:**
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) _(83 baris — penambahan wrapper sel baru untuk status attendance)_
  - [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js) [NEW] _(19 baris — konfigurasi status badge untuk attendance)_

  **Frontend — Utilitas & I18n:**
  - [`frontend/src/utils/timeFormatter.js`](frontend/src/utils/timeFormatter.js) [NEW] _(16 baris — utilitas pemformatan waktu)_
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) _(14 key baru)_
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) _(14 key baru)_

- **Deskripsi Perubahan & Fungsi**:
  - **Attendance Request Detail Modal**: Membangun modal detail permintaan izin/cuti yang menampilkan informasi lengkap meliputi data pelamar, jenis permintaan (izin sakit, izin pribadi, cuti), periode waktu, status approval, dan catatan. Modal ini diintegrasikan ke dalam halaman Attendance untuk memudahkan admin melihat detail tanpa meninggalkan halaman.
  - **Halaman Paid Leave (Cuti Berbayar)**: Membangun halaman baru untuk mengelola daftar cuti berbayar karyawan dengan tabel yang menampilkan informasi karyawan, jenis cuti, tanggal, durasi, dan status.
  - **Halaman Permission (Izin)**: Membangun halaman baru untuk mengelola daftar izin karyawan dengan tabel yang menampilkan informasi karyawan, jenis izin, tanggal, durasi, dan status.
  - **Backend Attendance API**: Membangun endpoint REST API untuk data attendance (`attendance.route.js` — 116 baris) beserta service layer untuk pengambilan data dari database.
  - **Files API**: Membangun endpoint untuk pengambilan file lampiran terkait permintaan izin/cuti.
  - **Shared Components**: Menambahkan wrapper sel dan konfigurasi status badge baru ke dalam komponen tabel bersama (`rows.jsx`, `status.js`) untuk mendukung visualisasi status attendance.
  - **Utilitas**: Menambahkan `timeFormatter.js` untuk pemformatan waktu yang konsisten di seluruh modul attendance.

---

## � Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                              | Dampak Utama                                                                                                                                       |
| ----- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| #161  | WhatsApp Chat — Sistem Real-Time Chat Terintegrasi | Sistem chat WhatsApp real-time penuh dengan manajemen percakapan, pesan, template, riwayat, dan notifikasi                                         |
| #162  | WhatsApp Broadcast & Pengaturan Notifikasi Tagihan | Kemampuan siaran pesan massal via WhatsApp template, pengaturan reminder tagihan yang lebih fleksibel, dan refactor endpoint template WhatsApp API |
| #165  | Attendance Detail, Paid Leave & Permission         | Modal detail permintaan izin/cuti, halaman cuti berbayar, halaman izin, dan backend attendance API                                                 |

### Kemampuan Baru Pengguna/Admin

- **Chat WhatsApp Real-Time**: Admin kini dapat melakukan percakapan langsung dengan pelanggan/mitra melalui WhatsApp langsung dari panel admin DEKASIMAL, dengan dukungan pesan teks, media (gambar/dokumen/video), emoji, dan template pesan cepat.
- **Manajemen Percakapan**: Admin dapat melihat daftar percakapan aktif, melakukan locking percakapan agar hanya satu admin yang menangani, dan melihat status admin online yang sedang aktif.
- **Riwayat Chat (Chat History)**: Admin dapat mencari dan melihat transkrip percakapan WhatsApp yang sudah selesai/ditutup, termasuk detail pesan dan lampiran.
- **Template Pesan**: Admin dapat membuat, mengedit, dan mengelola template pesan WhatsApp yang sudah disetujui Meta, termasuk pemetaan variabel dinamis (nama pelanggan, jumlah tagihan, tanggal jatuh tempo, dll).
- **WhatsApp Broadcast**: Admin dapat membuat siaran pesan massal dengan memilih penerima dari daftar pelanggan/mitra atau berdasarkan tagihan, memilih template, memetakan variabel, dan mengirim ke banyak penerima sekaligus.
- **Pengaturan Notifikasi Tagihan**: Admin dapat mengkonfigurasi pengingat tagihan otomatis via WhatsApp, termasuk menentukan berapa hari sebelum jatuh tempo, jam pengiriman, dan mengaktifkan/menonaktifkan fitur pengingat serta notifikasi pembayaran.
- **Detail Permintaan Izin/Cuti**: Admin dapat melihat detail lengkap permintaan izin atau cuti karyawan melalui modal detail yang menampilkan data pelamar, jenis permintaan, periode, status approval, dan catatan.
- **Halaman Paid Leave & Permission**: Admin dapat mengelola daftar cuti berbayar dan izin karyawan melalui halaman terpisah dengan tabel yang informatif dan terstruktur.

### Bug Fix / Solusi Masalah

- **Pesan Tersangkut (Stuck Messages)**: Dijalankan mekanisme sweeping berkala (`whatsappSweep`) untuk mengambil pesan masuk dari Meta API yang belum tercatat di database, mencegah kehilangan pesan.
- **Recipient Broadcast Tersangkut**: Dijalankan mekanisme `sweepStuckBroadcastRecipients` untuk memulihkan penerima yang tersangkut dalam status `processing` akibat gangguan jaringan atau timeout.

### Menu/Fitur Baru

- **WhatsApp Chat** (`/customer-service/whatsapp-chat`): Halaman utama percakapan WhatsApp real-time.
- **Chat History** (`/customer-service/chat-history`): Halaman riwayat transkrip percakapan.
- **Message Template** (`/customer-service/message-template`): Halaman manajemen template pesan WhatsApp.
- **WhatsApp Broadcast** (`/customer-service/whatsapp-broadcast`): Halaman manajemen siaran pesan massal WhatsApp.
- **Pengaturan Notifikasi WhatsApp** (di halaman System Settings): Bagian baru untuk konfigurasi pengingat tagihan otomatis via WhatsApp.
- **Paid Leave** (`/activities/paid-leave`): Halaman daftar cuti berbayar karyawan.
- **Permission** (`/activities/permission`): Halaman daftar izin karyawan.
- **Attendance Request Detail Modal**: Modal detail permintaan izin/cuti yang diintegrasikan ke halaman Attendance.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### WhatsApp Broadcast — Siaran Pesan Massal

- **Penjelasan Fitur**: WhatsApp Broadcast memungkinkan admin mengirim pesan template WhatsApp (yang sudah disetujui Meta) secara massal ke daftar penerima. Sistem menggunakan arsitektur queue (BullMQ) untuk pengiriman yang reliable, dengan mekanisme retry otomatis jika pengiriman gagal. Setiap siaran memiliki status pelacakan untuk setiap penerima (terkirim, gagal, pending, dll).

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Customer Service → WhatsApp Broadcast**.
  2. Klik tombol **Buat Siaran** untuk membuka drawer pembuatan siaran baru.
  3. Pilih **template pesan** dari daftar template yang sudah disetujui Meta.
  4. **Pilih penerima** dari daftar pelanggan atau mitra yang tersedia (bisa dicari berdasarkan nama, kode, atau filter).
  5. **Petakan variabel template** — isi nilai untuk setiap variabel dinamis yang ada di template (misal: `%customer_name%`, `%amount%`, `%due_date%`). Gunakan tombol **Dokumentasi Variabel** untuk melihat panduan format.
  6. **Pratinjau** pesan yang akan dikirim.
  7. Klik **Kirim** untuk memulai proses siaran. Status pengiriman akan ditampilkan untuk setiap penerima.

### WhatsApp Chat — Percakapan Real-Time

- **Penjelasan Fitur**: WhatsApp Chat menyediakan antarmuka chat real-time yang terintegrasi langsung dengan akun WhatsApp bisnis. Percakapan muncul secara otomatis ketika pelanggan mengirim pesan, dan admin dapat merespons langsung dari panel admin. Fitur ini mendukung pesan teks, media (gambar/dokumen/video), emoji, dan template pesan cepat.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Customer Service → WhatsApp Chat**.
  2. Daftar percakapan aktif akan muncul di panel kiri, diurutkan berdasarkan pesan terakhir.
  3. Klik pada percakapan untuk membuka chat.
  4. Ketik pesan di area penulisan di bagian bawah. Gunakan tombol **emoji** untuk menambahkan emoji, atau tombol **lampiran** untuk mengirim file.
  5. Untuk menggunakan template pesan cepat, klik tombol **template** di area penulisan.
  6. Pesan akan dikirim secara real-time dan statusnya (terkirim/dibaca) akan ditampilkan.
  7. Admin lain yang sedang online akan melihat percakapan yang sedang ditangani di footer **Admin Online**.

---
