# 📝 Daily Work Report - Dedy S.N Putra (22 Oktober 2026)

---

## 📅 Laporan Harian - 22 Oktober 2026

---

## 🌿 Branch: `master` & `production` — Pembaruan Catatan Rilis (Changelog v1.98.6 – v1.98.20) & Sinkronisasi Rilis Produksi

### 📌 Informasi Pembaruan

- **Nomor Versi**: `v1.98.6` s/d `v1.98.20` (Total 15 rilis tercakup)
- **Judul Rilis**: Sinkronisasi Catatan Rilis Komprehensif Dekasimal V2 & Deploy Produksi
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `production` pada Kamis, 22 Oktober 2026, 21:01:04 WIB via commit `7f8f3b56`)

### 📅 Rincian Commit

#### [7f8f3b56] - Merge branch 'master' into production - Kamis, 22 Oktober 2026, 21:01:04 WIB

_(Merge commit menyinkronkan seluruh pembaruan kode, fitur, dan perbaikan dari branch `master` ke branch `production`)_

---

#### [88bdd589] - update changelog - Kamis, 22 Oktober 2026, 20:58:23 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-433.json` [NEW]
  - `backend/src/data/changelog/releases/issue-434.json` [NEW]
  - `backend/src/data/changelog/releases/issue-435.json` [NEW]
  - `backend/src/data/changelog/releases/issue-437.json` [NEW]
  - `backend/src/data/changelog/releases/issue-442.json` [NEW]
  - `backend/src/data/changelog/releases/issue-444.json` [NEW]
  - `backend/src/data/changelog/releases/issue-445.json` [NEW]
  - `backend/src/data/changelog/releases/issue-448.json` [NEW]
  - `backend/src/data/changelog/releases/issue-450.json` [NEW]
  - `backend/src/data/changelog/releases/issue-453.json` [NEW]
  - `backend/src/data/changelog/releases/issue-455.json` [NEW]
  - `backend/src/data/changelog/releases/issue-458.json` [NEW]
  - `backend/src/data/changelog/releases/issue-460.json` [NEW]
  - `backend/src/data/changelog/releases/issue-462.json` [NEW]
  - `backend/src/data/changelog/releases/issue-464.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Generasi Berkas Detail Rilis Changelog**:
    - Menghasilkan 15 berkas rilis terstruktur berbasis JSON di direktori `backend/src/data/changelog/releases/` untuk issue #435, #434, #433, #442, #437, #448, #450, #444, #455, #458, #445, #453, #460, #462, dan #464.
    - Setiap berkas rilis mendokumentasikan fitur baru, perbaikan bug, dan peningkatan kinerja menggunakan bahasa ramah pengguna (non-teknis) yang bebas dari jargon kode teknis (tanpa ekstensi berkas, nama model database, controller, maupun path API).
  - **Pembaruan Indeks Metadata Rilis (`index.json`)**:
    - Memperbarui berkas `backend/src/data/changelog/index.json` dengan menyisipkan 15 entri ringkas di indeks paling atas (`0`–`14`) berurutan secara kronologis terbalik (rilis terbaru #464 di indeks `0`).
    - Menyusun field `searchText` komprehensif pada setiap entri untuk mendukung pencarian instan pada UI Catatan Rilis admin.

---

## 🌿 Branch: `issue-464` — Issue #464: Dukungan Obrolan Grup WhatsApp dan Arsip Riwayat Sesi

### 📌 Informasi Issue

- **Nomor Issue**: #464
- **Judul Issue**: Dukungan Obrolan Grup WhatsApp dan Arsip Riwayat Sesi
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 20:29:15 WIB via commit `10862739`)

### 📅 Rincian Commit

#### [10862739] - resolve #464 - Kamis, 22 Oktober 2026, 20:29:15 WIB

_(Merge commit menggabungkan branch `issue-464` ke branch `master`)_

---

#### [4c7af0f9] - resolve #464 - Kamis, 22 Oktober 2026, 20:17:38 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/scripts/migrateWaMessageWamidIndex.js` [NEW]
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/baileysInternal.controller.js`
  - `backend/src/controllers/waChat.controller.js`
  - `backend/src/controllers/waContact.controller.js`
  - `backend/src/controllers/waTicket.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/models/baileysAccount.model.js`
  - `backend/src/models/waConversation.model.js`
  - `backend/src/models/waMessage.model.js`
  - `backend/src/routes/waChat.route.js`
  - `backend/src/services/baileysAccount.service.js`
  - `backend/src/services/baileysSender.service.js`
  - `backend/src/services/dashboard.service.js`
  - `backend/src/services/waChannelRouter.service.js`
  - `backend/src/services/waContact.service.js`
  - `backend/src/services/waConversation.service.js`
  - `backend/src/services/waMessage.service.js`
  - `backend/src/services/waReport.service.js`
  - `backend/src/sockets/waChat.controller.js`
  - `backend/src/utils/waChatSerializer.js`
  - `backend/src/utils/waChatUtils.js`
  - `backend/src/utils/waGroup.js` [NEW]
  - `backend/src/utils/waMessageIndexMigration.js` [NEW]
  - `backend/src/utils/waTicketAccess.js`
  - `backend/test/integration/baileysAccount.groupsEnabled.test.js` [NEW]
  - `backend/test/integration/baileysInternal.group.test.js` [NEW]
  - `backend/test/integration/waChannelRouter.group.test.js` [NEW]
  - `backend/test/integration/waChatSerializer.group.test.js` [NEW]
  - `backend/test/integration/waChatSocket.group.test.js` [NEW]
  - `backend/test/integration/waGroup.access.test.js` [NEW]
  - `backend/test/integration/waGroup.activeList.test.js` [NEW]
  - `backend/test/integration/waGroup.guards.test.js` [NEW]
  - `backend/test/integration/waGroup.rejections.test.js` [NEW]
  - `backend/test/integration/waMessage.wamidScope.test.js` [NEW]
  - `backend/test/integration/waMessageHistory.test.js` [NEW]
  - `backend/test/integration/waSendMessage.group.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js`
  - `backend/test/unit/waConversationReaders.guard.test.js` [NEW]
  - `backend/test/unit/waGroup.test.js` [NEW]
  - `backend/test/unit/waUnrepliedCount.service.test.js`
  - `baileys-api/src/controllers/send.controller.js`
  - `baileys-api/src/models/baileysAccount.model.js`
  - `baileys-api/src/services/sessionManager.service.js`
  - `baileys-api/src/services/webhookRetry.core.js`
  - `baileys-api/src/utils/groupMessage.js` [NEW]
  - `baileys-api/test/groupMessage.test.js` [NEW]
  - `baileys-api/test/sendTarget.test.js` [NEW]
  - `baileys-api/test/webhookRetry.test.js`
  - `frontend/src/app/pages/customerService/baileysAccount/components/ConnectAccountModal.jsx`
  - `frontend/src/app/pages/customerService/baileysAccount/edit.jsx`
  - `frontend/src/app/pages/customerService/baileysAccount/schema/editSchema.js`
  - `frontend/src/app/pages/customerService/chatHistory/schema/rows.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationAvatar.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationItem.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/GroupDisplay.test.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/GroupSenderLabel.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/MessageBubble.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/MessageList.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useMessages.js`
  - `frontend/src/app/pages/customerService/whatsappChat/index.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/utils/archiveTimeline.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/utils/archiveTimeline.test.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/utils/groupChat.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/utils/groupChat.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Dukungan Pesan Grup WhatsApp Penuh (`baileys-api` & `backend`)**:
    - Mendukung penerimaan dan pemrosesan pesan dari grup WhatsApp (`@g.us`). Percakapan grup dicatat sebagai sesi `WaConversation` dengan flag `is_group: true` dan `wa_id` berupa JID grup WhatsApp.
    - Menambahkan saklar per akun `groups_enabled` ("Terima pesan grup") pada pengaturan akun WhatsApp Baileys. Jika nonaktif, pesan grup langsung diabaikan sebelum mengunduh media, mencegah lonjakan beban penyimpanan.
    - Menyusun pembatasan logis grup: grup dikecualikan dari buku kontak/phonebook, tiket otomatis, rating kepuasan pelanggan, dan perhitungan KPI antrean pelanggan, namun admin yang berhak tetap dapat memantau dan membalas pesan grup selama sesi terbuka.
    - Mengubah keunikan indeks pesan `wamid` menjadi lingkup `{ conversation, wamid }` via skrip migrasi `migrateWaMessageWamidIndex.js` agar ID pesan grup yang sama pada multi-akun tidak saling bertabrakan.
  - **Antarmuka Obrolan Grup WhatsApp (`frontend`)**:
    - Menambahkan label nama dan nomor pengirim anggota grup (`GroupSenderLabel.jsx`) pada setiap balon pesan masuk grup.
    - Menambahkan lencana visual grup dan avatar grup khusus pada daftar percakapan.
    - Menambahkan tombol filter jenis percakapan ("Semua", "Pribadi", "Grup") di atas daftar percakapan untuk memudahkan pemilahan pesan.
    - Memisahkan kuota tampilan inbox antara percakapan 1:1 dan grup agar pesan grup yang padat tidak menggeser antrean pelanggan pribadi.
  - **Arsip Riwayat Sesi Sebelumnya (Archive Timeline)**:
    - Menambahkan tombol "Muat riwayat sesi sebelumnya" pada header obrolan ketika kontak atau grup memiliki percakapan di sesi lampau yang telah selesai/ditutup.
    - Menyusun visualisasi garis pemisah sesi arsip (`archiveTimeline.js`) yang mengelompokkan pesan per sesi lengkap dengan penanda tanggal sesi serta nama admin yang menangani sesi tersebut.

---

## 🌿 Branch: `issue-462` — Issue #462: Standarisasi Pemilihan Pihak Penerima Faktur Tagihan (Party Picker Scope)

### 📌 Informasi Issue

- **Nomor Issue**: #462
- **Judul Issue**: Standarisasi pemilihan pihak penerima faktur tagihan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 20:16:00 WIB via commit `eb2bb1f1`)

### 📅 Rincian Commit

#### [eb2bb1f1] - resolve #462 - Kamis, 22 Oktober 2026, 20:16:00 WIB

_(Merge commit menggabungkan branch `issue-462` ke branch `master`)_

---

#### [323c011b] - resolve #462 - Kamis, 22 Oktober 2026, 17:18:21 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/test/integration/invoiceRecipientSelect.test.js` [NEW]
  - `backend/test/integration/partnerApiBusiness.test.js`
  - `frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx`
  - `frontend/src/components/shared/form/PartyPicker.jsx`
  - `frontend/src/components/shared/form/partyTypes.js` [NEW]
  - `frontend/src/components/shared/form/partyTypes.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyelarasan Klasifikasi Pihak Penerima Tagihan**:
    - Memisahkan dan menstandarisasi tipe pihak lawan transaksi khusus penerima faktur (`partyTypes.js`) agar selaras dengan kategori di navigasi menu Pengguna:
      - `customerRegular`: Pelanggan internal reguler (`/customer/user-select-master`, `pid === 'master'`).
      - `business`: Mitra Bisnis korporat (`/business/user-select`, `reseller === false`).
      - `reseller`: Mitra Reseller internet (`/partner/user-select`, `reseller === true`).
      - `customerPartner`: Pelanggan milik Mitra (`/customer-partner/user-select`, `pid !== 'master'`).
  - **Pencegahan Pencampuran Data Pihak pada Form Pembuatan Tagihan**:
    - Mengisolasi pencarian pihak penerima faktur pada `InvoiceDrawer.jsx` sehingga ketika admin memilih salah satu kategori, pencarian AJAX hanya mengambil record yang relevan dengan kategori tersebut.
    - Menyediakan suite pengujian integrasi `invoiceRecipientSelect.test.js` dan unit test `partyTypes.test.js` untuk memastikan backward compatibility pada form keuangan lainnya (mutasi kas, voucher, transaksi umum).

---

## 🌿 Branch: `issue-460` — Issue #460: Status Pelaporan pada Tabel Pengguna & Integrasi Riwayat Pelanggan

### 📌 Informasi Issue

- **Nomor Issue**: #460
- **Judul Issue**: Status Pelaporan pada Tabel Pengguna dan Integrasi Riwayat Pelanggan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 19:08:10 WIB via commit `aee436ff`)

### 📅 Rincian Commit

#### [aee436ff] - resolve #460 - Kamis, 22 Oktober 2026, 19:08:10 WIB

_(Merge commit menggabungkan branch `issue-460` ke branch `master`)_

---

#### [9306a438] - resolve #460 - Kamis, 22 Oktober 2026, 16:02:49 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/src/controllers/partnerApiRadius.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/partnerApiBody.middleware.js` [NEW]
  - `backend/src/routes/business.route.js`
  - `backend/src/routes/customer.route.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/business.service.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/taxReport.service.js`
  - `backend/src/utils/customerSubscriptionState.js` [NEW]
  - `backend/src/utils/data-table.js`
  - `backend/test/integration/businessTaxReported.test.js`
  - `backend/test/integration/customerTaxReported.test.js`
  - `backend/test/integration/partnerApiCustomer.archived.test.js` [NEW]
  - `backend/test/integration/partnerApiCustomer.create.test.js`
  - `frontend/src/app/pages/users/blacklist/index.jsx`
  - `frontend/src/app/pages/users/blacklist/schema/columns.jsx`
  - `frontend/src/app/pages/users/business/schema/columns.jsx`
  - `frontend/src/app/pages/users/customer/schema/columns.jsx`
  - `frontend/src/app/pages/users/pasif/index.jsx`
  - `frontend/src/app/pages/users/pasif/schema/columns.jsx`
  - `frontend/src/app/pages/users/unsubscribe/index.jsx`
  - `frontend/src/app/pages/users/unsubscribe/schema/columns.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyajian Status Pelaporan pada Tabel Pengguna Khusus**:
    - Menambahkan kolom status Pelaporan (`tax_reported`) pada tabel Pelanggan Berhenti Berlangganan (`unsubscribe`), Pelanggan Blacklist (`blacklist`), dan Pelanggan Pasif (`pasif`).
    - Menyempurnakan mesin query datatable (`data-table.js`) untuk mendukung filter dan pengurutan pada kolom turunan (kolom yang nilainya dihitung dari relasi layanan pelanggan dan tidak tersimpan langsung di dokumen pelanggan).
  - **Integrasi Pembacaan Pelanggan Nonaktif/Blacklist pada Partner API**:
    - Mengizinkan pembacaan riwayat pelanggan internal yang berstatus berhenti berlangganan atau blacklist pada portal mitra bisnis jika pelanggan tersebut memiliki layanan terlapor, guna keperluan pelaporan dan arsip keuangan.
    - Menambahkan proteksi ketat pada rute ubah/tulis (`throwCustomerNotWritable`): pelanggan terhapus/blacklist hanya dapat dibaca (`read-only`) dan penolakan manipulasi data dibalas dengan status HTTP `422 Unprocessable Entity` ("Pelanggan sudah berhenti berlangganan atau di-blacklist, datanya hanya bisa dibaca").
    - Menambahkan middleware `partnerApiBody.middleware.js` untuk membersihkan field sensitif status (`blacklist`, `pasif`, `deleted`) dari payload kiriman mitra.

---

## 🌿 Branch: `issue-453` — Issue #453: Penyempurnaan Modul Umpan Balik Asisten AI (Answer Feedback Refactor)

### 📌 Informasi Issue

- **Nomor Issue**: #453
- **Judul Issue**: Penyempurnaan modul umpan balik asisten AI
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 16:55:14 WIB via commit `ca45e350`)

### 📅 Rincian Commit

#### [ca45e350] - resolve #453 - Kamis, 22 Oktober 2026, 16:55:14 WIB

_(Merge commit menggabungkan branch `issue-453` ke branch `master`)_

---

#### [afa9cc8c] - resolve #453 - Kamis, 22 Oktober 2026, 16:47:33 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `frontend/src/components/template/RightSidebar/AnswerFeedback.jsx`
  - `frontend/src/components/template/RightSidebar/answerFeedbackUtils.js` [NEW / RENAME]
  - `frontend/src/components/template/RightSidebar/answerFeedbackUtils.test.js` [NEW / RENAME]
  - `frontend/src/components/template/RightSidebar/index.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Refactor Penamaan dan Modularitas Modul Feedback AI**:
    - Menstandarisasi penamaan berkas helper dari `answerFeedback.js` menjadi `answerFeedbackUtils.js` beserta berkas pengujiannya `answerFeedbackUtils.test.js` agar selaras dengan pola penamaan utilitas proyek.
    - Memperbarui import dan referensi fungsi `buildFeedbackUrl` dan `canRateMessage` pada komponen sidebar asisten AI (`RightSidebar/index.jsx` dan `AnswerFeedback.jsx`).
    - Memperbarui katalog fitur pengetahuan sistem (`features.json`) untuk mendokumentasikan endpoint peninjauan umpan balik asisten AI.

---

## 🌿 Branch: `issue-445` — Issue #445: Pencarian Tiket Berdasarkan Nama Pelanggan pada Formulir Gudang

### 📌 Informasi Issue

- **Nomor Issue**: #445
- **Judul Issue**: Pencarian tiket berdasarkan nama pelanggan pada formulir gudang
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 16:40:23 WIB via commit `b09626ee`)

### 📅 Rincian Commit

#### [b09626ee] - resolve #445 - Kamis, 22 Oktober 2026, 16:40:23 WIB

_(Merge commit menggabungkan commit penyelesaian `8c555b7e` ke branch `master`)_

---

#### [8c555b7e] - resolve #445 - Selasa, 20 Oktober 2026, 18:02:46 WIB / committer 22 Oktober 2026

- **Komponen yang Berubah**:
  - `backend/src/routes/ticket.route.js`
  - `backend/src/services/ticket.service.js`
  - `backend/test/integration/ticketOpenedSelect.test.js` [NEW]
  - `telegram-apps/src/components/shared/AsyncSelect.jsx`
  - `telegram-apps/src/pages/warehouse/installItem.jsx`
  - `telegram-apps/src/pages/warehouse/installSiteItem.jsx`
  - `telegram-apps/src/pages/warehouse/takeItem.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Pencarian Tiket Multi-Kriteria pada Telegram Mini Apps**:
    - Memperluas query pencarian tiket terbuka (`findTicketOpenedSelect`) di backend agar dapat mencocokkan nama pelanggan (`customer.name`) dan nama mitra (`partner.name`), di samping ID tiket dan judul tiket.
    - Sanitasi karakter regex pada string pencarian agar simbol pencarian umum tidak merusak eksekusi query MongoDB atau memicu ReDoS.
  - **Peningkatan Visualisasi Opsi Tiket pada Formulir Transaksi Gudang**:
    - Memperbarui komponen `AsyncSelect.jsx` pada aplikasi Telegram Mini Apps agar opsi tiket menampilkan format ringkasan: `[Nama Pelanggan/Mitra] - [Tipe Tiket] - [Tanggal Tiket]`.
    - Memperbarui placeholder pencarian pada halaman Ambil Barang (`takeItem.jsx`), Pasang Barang Pelanggan (`installItem.jsx`), dan Pasang Barang Site (`installSiteItem.jsx`) menjadi *"Cari ID tiket, nama tiket, atau pelanggan"*.

---

## 🌿 Branch: `issue-458` — Issue #458: Tampilan Responsif Tagihan Publik dan Tautan Interaktif Berkas Faktur

### 📌 Informasi Issue

- **Nomor Issue**: #458
- **Judul Issue**: Tampilan responsif tagihan publik dan tautan interaktif berkas faktur
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 16:39:38 WIB via commit `da635b9a`)

### 📅 Rincian Commit

#### [da635b9a] - resolve #458 - Kamis, 22 Oktober 2026, 16:39:38 WIB

_(Merge commit menggabungkan branch `issue-458` ke branch `master`)_

---

#### [79cff4d2] - resolve #458 - Kamis, 22 Oktober 2026, 12:13:33 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/public/PublicInvoiceDocument.jsx`
  - `frontend/src/components/shared/ExternalLink.jsx` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penskalaan Otomatis Dokumen Tagihan Publik pada Layar Mobile**:
    - Mengimplementasikan mekanisme penskalaan proporsional berbasis `transform: scale(...)` pada `PublicInvoiceDocument.jsx` yang otomatis menghitung rasio lebar layar pengguna terhadap lebar standar dokumen faktur (A4 / 794px).
    - Menghilangkan keharusan pengguna ponsel menggulir halaman secara horizontal (scroll samping) saat meninjau rincian tagihan publik tanpa merusak format tata letak cetak (*print stylesheet*).
  - **Anotasi Tautan Interaktif pada Unduhan Berkas PDF Faktur**:
    - Mengintegrasikan penumpangan anotasi link jsPDF pada posisi persis setiap elemen tautan `<a href>` dokumen (misalnya tautan bayar online di bawah kode QR) saat PDF dihasilkan, sehingga tautan pada dokumen PDF hasil unduhan dapat diklik secara interaktif oleh pelanggan.
  - **Komponen Standar Tautan Eksternal (`ExternalLink.jsx`)**:
    - Membuat komponen `ExternalLink.jsx` dengan atribut keamanan `rel="noopener noreferrer"` untuk membuka tautan eksternal pembayaran secara aman.

---

## 🌿 Branch: `issue-455` — Issue #455: Pengaturan Tampilan Grafik Global dan Penyegaran Otomatis Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #455
- **Judul Issue**: Pengaturan tampilan grafik global dan penyegaran otomatis layanan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 22 Oktober 2026, 01:53:57 WIB via commit `26042d33`, dan di-merge ke `production` via commit `ce9e9909`)

### 📅 Rincian Commit

#### [ce9e9909] - Merge branch 'master' into production - Kamis, 22 Oktober 2026, 01:54:39 WIB

_(Merge commit rilis malam sinkronisasi fitur Issue #455 ke branch `production`)_

---

#### [26042d33] - resolve #455 - Kamis, 22 Oktober 2026, 01:53:57 WIB

_(Merge commit menggabungkan branch `issue-455` ke branch `master`)_

---

#### [6778264e] - resolve #455 - Kamis, 22 Oktober 2026, 01:52:28 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/networkDisplaySetting.controller.js` [NEW]
  - `backend/src/controllers/partnerApiRadius.controller.js`
  - `backend/src/controllers/radiusControl.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/networkLatency.route.js`
  - `backend/src/routes/networkTraffic.route.js`
  - `backend/src/services/networkDisplaySetting.service.js` [NEW]
  - `backend/src/services/radiusControl.service.js`
  - `backend/src/utils/networkDisplaySetting.js` [NEW]
  - `backend/src/utils/radiusSessionState.js` [NEW]
  - `backend/test/integration/networkDisplaySetting.controller.test.js` [NEW]
  - `backend/test/integration/networkDisplaySetting.service.test.js` [NEW]
  - `backend/test/integration/partnerApiRadius.test.js`
  - `backend/test/unit/networkDisplaySetting.validation.test.js` [NEW]
  - `backend/test/unit/radiusControl.disconnectBestEffort.test.js`
  - `backend/test/unit/radiusControl.disconnectSession.controller.test.js` [NEW]
  - `backend/test/unit/streamRegistry.crossInstance.test.js` [NEW]
  - `frontend/src/app/pages/network/latency/components/LatencySettingsModal.jsx`
  - `frontend/src/app/pages/network/latency/index.jsx`
  - `frontend/src/app/pages/network/latency/utils/latencyGraphStyle.js`
  - `frontend/src/app/pages/network/latency/utils/latencyGraphStyle.test.js` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
  - `frontend/src/app/pages/network/shared/displaySetting.js` [NEW]
  - `frontend/src/app/pages/network/shared/displaySetting.test.js` [NEW]
  - `frontend/src/app/pages/network/shared/useNetworkDisplaySetting.js` [NEW]
  - `frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx`
  - `frontend/src/app/pages/network/traffic/components/TrafficSettingsModal.jsx`
  - `frontend/src/app/pages/network/traffic/index.jsx`
  - `frontend/src/app/pages/network/traffic/utils/serviceTargetStatus.js` [NEW]
  - `frontend/src/app/pages/network/traffic/utils/serviceTargetStatus.test.js` [NEW]
  - `frontend/src/app/pages/network/traffic/utils/trafficGraphStyle.js` [NEW]
  - `frontend/src/app/pages/network/traffic/utils/trafficGraphStyle.test.js` [NEW]
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/app/pages/services/components/ServiceTrafficMonitoring.jsx`
  - `frontend/src/app/pages/services/dataAccess/detail.jsx`
  - `frontend/src/app/pages/services/dedicatedInternet/detail.jsx`
  - `frontend/src/components/shared/acs/OntDeviceCard.jsx`
  - `frontend/src/hooks/useAutoRefresh.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/autoRefresh.js` [NEW]
  - `frontend/src/utils/autoRefresh.test.js` [NEW]
  - `frontend/src/utils/invoiceDueDay.js` [NEW]
  - `frontend/src/utils/invoiceDueDay.test.js` [NEW]
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/domain/accounting/handlers.go`
  - `radius-server/internal/domain/accounting/handlers_test.go`
  - `radius-server/internal/domain/hotspot/accounting.go`
  - `radius-server/internal/domain/hotspot/accounting_test.go`
  - `radius-server/internal/domain/ports/repositories.go`
  - `radius-server/internal/domain/ports/types.go`
  - `radius-server/internal/domain/sweep/sweep.go`
  - `radius-server/internal/domain/sweep/sweep_test.go`
  - `radius-server/internal/repository/mongo/authentication_repo.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/internal/transport/coa/client.go`
  - `radius-server/internal/transport/coa/client_test.go`
  - `radius-server/internal/transport/coa/wire_test.go` [NEW]
  - `radius-server/internal/transport/grpcclient/client.go`
  - `radius-server/internal/transport/grpcclient/client_test.go`
  - `radius-server/internal/transport/pppoe/auth_listener.go`
  - `radius-server/internal/transport/pppoe/last_session_race_test.go` [NEW]
  - `radius-server/test/integration/mongo_repo_test.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyimpanan Pengaturan Tampilan Grafik Global (Trafik & Latensi)**:
    - Membangun model dan service `networkDisplaySetting` untuk menyimpan interface target terpilih dan konfigurasi visual grafik secara global bagi seluruh admin sistem.
    - Menambahkan modal pengaturan tampilan grafik (`TrafficSettingsModal.jsx` dan `LatencySettingsModal.jsx`) dengan pemisahan wewenang: admin berhak dapat menyimpan default bersama untuk semua pengguna, sedangkan user non-privileged hanya menerapkan preferensi di sesi aktifnya.
  - **Hook Penyegaran Otomatis (Auto-Refresh) pada Halaman Detail Layanan**:
    - Mengimplementasikan `useAutoRefresh.js` pada halaman detail layanan Broadband, Dedicated Internet, dan Data Access agar metrik trafik bandwidth dan status ONT termutakhirkan secara berkala tanpa me-refresh seluruh halaman browser.
  - **Dukungan Jatuh Tempo Tagihan Akhir Bulan (`endOfMonth`)**:
    - Menambahkan utilitas `invoiceDueDay.js` untuk mengelola tanggal jatuh tempo fleksibel pada hari terakhir bulan berjalan (mis. 28, 29, 30, atau 31).
  - **Peningkatan Verifikasi Pemutusan Sesi (Disconnect/CoA) Radius**:
    - Memperkuat integrasi gRPC backend ke radius-server: pemutusan sesi kini diverifikasi langsung dari status sesi database dengan penanganan koneksi putus tangguh dan proteksi balapan sesi (*session race condition*).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| :--- | :--- | :--- |
| **#464** | Dukungan Obrolan Grup WhatsApp dan Arsip Riwayat Sesi | Admin dapat menerima dan membalas pesan grup WhatsApp langsung dari aplikasi serta membuka riwayat percakapan dari sesi-sesi sebelumnya. |
| **#462** | Standarisasi Pemilihan Pihak Penerima Faktur Tagihan | Kategori pilihan pihak penerima faktur diselaraskan dengan navigasi menu Pengguna (Pelanggan, Mitra Bisnis, Mitra, Pelanggan Mitra) untuk pencarian yang tepat. |
| **#460** | Status Pelaporan pada Tabel Pengguna & Riwayat Pelanggan | Status pelaporan layanan kini tampil lengkap di tabel Unsubscribe, Blacklist, dan Pasif, serta pelanggan nonaktif dapat dibaca secara aman di portal mitra. |
| **#453** | Penyempurnaan Modul Umpan Balik Asisten AI | Standarisasi berkas utilitas umpan balik jawaban asisten AI dan dokumentasi katalog pengetahuan sistem. |
| **#445** | Pencarian Tiket Berdasarkan Nama Pelanggan pada Form Gudang | Teknisi dapat mencari tiket gangguan di Telegram Mini Apps menggunakan nama pelanggan atau nama mitra. |
| **#458** | Tampilan Responsif Tagihan Publik dan Tautan Interaktif PDF | Dokumen tagihan publik otomatis menyesuaikan ukuran layar ponsel tanpa scroll samping dan tautan pembayaran PDF dapat diklik langsung. |
| **#455** | Pengaturan Tampilan Grafik Global dan Auto-Refresh Layanan | Interface target grafik trafik/latensi dapat disimpan bersama untuk semua admin, serta halaman detail layanan otomatis menyegarkan data berkala. |
| **Changelog** | Pembaruan Catatan Rilis (v1.98.6 s/d v1.98.20) | Dokumentasi komprehensif 15 rilis sistem dalam format ramah pengguna dan terintegrasi pada antarmuka catatan rilis admin. |

### Kemampuan Baru Pengguna/Admin

- **Interaksi Grup WhatsApp**: Admin customer service kini dapat membaca dan merespons pesan grup WhatsApp secara real-time langsung dari panel Obrolan WhatsApp, lengkap dengan keterangan nama pengirim dan filter jenis percakapan.
- **Arsip Percakapan Lampau**: Admin dapat meninjau riwayat obrolan dari sesi-sesi sebelumnya dengan kontak atau grup yang sama untuk memahami konteks keluhan pelanggan di masa lalu.
- **Pencarian Tiket Fleksibel**: Teknisi gudang dapat mencari tiket penanganan gangguan cukup dengan mengetikkan nama pelanggan atau nama mitra di Telegram Mini Apps.
- **Tampilan Grafik Jaringan Bersama**: Administrator jaringan dapat mengatur interface penting dan gaya grafik trafik/latensi yang langsung berlaku sebagai tampilan baku bagi seluruh staf operasional.
- **Pemeriksaan Faktur di Ponsel**: Pelanggan yang membuka tagihan publik melalui tautan WhatsApp/SMS di ponsel dapat membaca seluruh rincian faktur secara proporsional tanpa perlu menggulir ke samping.

### Bug Fix / Solusi Masalah

- **Pencampuran Pihak Faktur**: Menyelesaikan masalah tercampurnya entitas pelanggan reguler dan mitra pada pilihan pihak lawan transaksi faktur tagihan.
- **Filter Kolom Turunan**: Memperbaiki kegagalan pengurutan dan penyaringan pada kolom status pelaporan di tabel pelanggan berhenti berlangganan, blacklist, dan pasif.
- **Tautan Pembayaran PDF**: Mengatasi masalah tautan pembayaran pada berkas unduhan PDF tagihan yang sebelumnya berupa gambar mati dan tidak dapat diklik.
- **Verifikasi Perintah Radius**: Memperbaiki kepastian status keberhasilan pemutusan sesi PPPoE/Hotspot antara backend dan radiusd agar tidak terjadi status menggantung.

### Menu/Fitur Baru

- **Filter Obrolan Grup**: Opsi filter *"Semua"*, *"Pribadi"*, dan *"Grup"* pada menu Obrolan WhatsApp.
- **Tombol Muat Riwayat Sesi**: Tombol arsip percakapan pada header Obrolan WhatsApp.
- **Pengaturan Grafik Default Bersama**: Tab pengaturan tampilan grafik global pada menu Jaringan > Trafik Monitor dan Latency Monitor.
- **Kolom Pelaporan Tambahan**: Kolom pelaporan baru pada halaman Pelanggan Berhenti Berlangganan, Pelanggan Blacklist, dan Pelanggan Pasif.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penanganan Obrolan Grup WhatsApp & Akses Riwayat Percakapan Lampau

- **Penjelasan Fitur**:
  - Fitur ini memungkinkan nomor WhatsApp perusahaan yang tergabung dalam grup koordinasi atau grup pelanggan dapat dimonitor dan direspons langsung dari aplikasi admin.
  - Pesan grup menampilkan nama pengirim di dalam grup, dan pesan yang dikirim admin akan diteruskan ke grup WhatsApp terkait.
  - Sesi obrolan lama yang sudah ditutup tidak lagi hilang; admin dapat membuka riwayat percakapan sebelumnya untuk melihat kronologi penanganan terdahulu.

- **Langkah Penggunaan (Tutorial)**:
  1. **Mengaktifkan Pesan Grup**:
     - Buka menu **Customer Service > Akun WhatsApp**.
     - Klik **Ubah** pada akun WhatsApp yang diinginkan.
     - Centang opsi **"Terima pesan grup"** lalu simpan.
  2. **Membuka dan Membalas Obrolan Grup**:
     - Buka menu **Customer Service > Obrolan WhatsApp**.
     - Pada bilah pencarian percakapan, klik filter **"Grup"** untuk menampilkan seluruh grup aktif.
     - Pilih grup yang dituju, kunci percakapan (*Take Over / Lock*), ketikkan tanggapan pada kotak pesan, lalu tekan **Kirim**.
  3. **Melihat Arsip Riwayat Sesi**:
     - Pada percakapan aktif yang memiliki riwayat obrolan terdahulu, klik tombol **"Muat riwayat sesi sebelumnya"** di bagian atas ruang percakapan.
     - Riwayat pesan terdahulu akan dimuat lengkap dengan garis pemisah tanggal sesi dan nama admin yang bertugas saat itu.

---

### 2. Menyimpan Tampilan Default Grafik Trafik Jaringan untuk Seluruh Admin

- **Penjelasan Fitur**:
  - Administrator kini dapat menetapkan daftar interface jaringan prioritas (misal: Uplink Utama, Distribusi Server) serta preferensi visual grafik agar langsung muncul saat staf lain membuka menu Trafik Monitor atau Latency Monitor.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan > Trafik Monitor**.
  2. Klik ikon roda gigi (**Pengaturan Tampilan & Filter**) di pojok kanan atas.
  3. Pada tab **Target Interface**, pilih interface-interface penting yang ingin dipantau bersama.
  4. Pada tab **Tampilan Grafik**, atur gaya visual (skala, warna, atau jenis grafik).
  5. Klik tombol **"Simpan untuk Semua Pengguna"** untuk menetapkan konfigurasi tersebut sebagai standar tampilan global.
