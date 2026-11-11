# 📝 Daily Work Report - Dedy S.N Putra (2026-10-05)

---

## 📅 Laporan Harian - 5 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, perbaikan infrastruktur pemantauan jaringan, penguatan resiliensi komunikasi WhatsApp, dan rilis produksi pada monorepo DEKASIMAL V2 per 5 Oktober 2026. Fokus dan pencapaian utama hari ini meliputi:
> 1. **Buku Telepon Terpusat & Sensor Kata Kasar Obrolan WhatsApp (`issue-370`)**: Pembangunan model data buku telepon terpadu (`wa_contacts`) lintas seluruh admin dan channel (Meta & Baileys) dengan dukungan penautan ke entitas pelanggan/mitra, mekanisme *optimistic locking* atomik `__v`, sinkronisasi *snapshot* sesi percakapan seketika, mesin penyensor kata kasar *fail-closed* berkinerja tinggi berbasis ekspresi reguler toleran leetspeak/imbuhan bahasa Indonesia tanpa mengubah data mentah di database, penyingkapan kata asli (*reveal message*) ber-audit trail terstruktur, serta peningkatan resiliensi sesi dan penarikan foto profil WhatsApp pada Baileys API.
> 2. **Perbaikan Batas Counter RRD Traffic & Penambahan RRA MAX pada RRD Latency Network Monitor (`issue-368`)**: Penyelarasan satuan batas data source RRD `COUNTER` ke Byte/detik dengan kalkulasi overhead Ethernet, pemanfaatan OID SNMP `ifHighSpeed` untuk port berkecepatan tinggi (Gigabit/10G+), restrukturisasi file RRD latency dengan konsolidasi RRA `MAX` agar lonjakan RTT dan packet loss tidak terdegradasi pada grafik historis, serta perkakas pemulihan massal (*Recovery Tools*) di backend dan frontend web admin dengan opsi simulasi *dry-run*.
> 3. **Penggabungan Penuh (*Merge & Release*) Modul Manajemen Jaringan Fiber Optik ke Master & Production (`issue-365`)**: Integrasi penuh arsitektur baru jaringan fiber optik berbasis *Single Source of Truth* `fiber_connections`, invarian geometri server-side, pemuatan data spasial berbasis *viewport bounding box*, perutean waypoint, dan modularisasi komponen antarmuka peta setelah seluruh 158 pengujian otomatis terverifikasi lulus 100%.

---

## 🌿 Branch: `issue-370` — Buku Telepon Obrolan WhatsApp, Sensor Kata Kasar Masuk, & Resiliensi Baileys API

### 📌 Informasi Issue

- **Nomor Issue**: #370
- **Judul Issue**: Buku Telepon Obrolan WhatsApp (`wa_contacts`), Penautan Identitas Pelanggan/Mitra, Sensor Kata Kasar Masuk (Profanity Filter), Resiliensi Baileys API, & Penarikan Foto Profil WhatsApp
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`d6630554`](file:///home/dhedhy/Project/Dekasimal-V2), seluruh pengujian unit & integrasi terverifikasi lulus)

### 📅 Rincian Commit

#### [[`d6630554`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #370 (Merge commit ke master) - 5 Oktober 2026, 22:00:37 WIB
#### [[`7bf5f618`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #370 - 5 Oktober 2026, 21:59:03 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Aturan Sistem (`AGENTS.md` & `docs/`)**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md): Penambahan aturan baku Bab 2.A nomor 20 (Standar Arsitektur Phonebook Obrolan WhatsApp `wa_contacts`) dan nomor 21 (Standar Sensor Kata Kasar Obrolan WhatsApp).
    - [`docs/superpowers/specs/2026-10-05-wa-phonebook-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-05-wa-phonebook-design.md) [NEW]: Spesifikasi teknis desain arsitektur buku telepon WhatsApp, resolusi identitas bertingkat, penautan data master, dan sinkronisasi snapshot percakapan.
    - [`docs/superpowers/specs/2026-10-05-wa-profanity-filter-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-05-wa-profanity-filter-design.md) [NEW]: Spesifikasi teknis mesin sensor kata kasar, pencegahan ReDoS, kebijakan fail-closed, dan hak akses penyingkapan pesan sensitif.
  - **Model & Konstanta Backend (`backend/src/models/` & `backend/src/constants/`)**:
    - [`backend/src/models/waContact.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/waContact.model.js) [NEW]: Skema Mongoose `wa_contacts` dengan kunci unik nomor telepon kanonik `phone`, `name`, `notes`, relasi penautan polimorfik `link` (`customer_id`, `model`), tracking user `created_by`/`updated_by`, dan *optimistic locking* `__v`.
    - [`backend/src/models/waConversation.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/waConversation.model.js): Penambahan field referensi `contact_source` (`phonebook`, `master`, `none`) dan pelengkap field snapshot identitas kontak.
    - [`backend/src/constants/waContact.constant.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/constants/waContact.constant.js) [NEW]: Definisi konstanta model-model yang dapat ditautkan (`Customer`, `PartnerCustomer`, `Partner`, `BusinessCustomer`) dan tipe sumber kontak.
    - [`backend/src/constants/waProfanity.constant.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/constants/waProfanity.constant.js) [NEW]: Daftar baku awal kosakata kata kasar bahasa Indonesia dan batasan konfigurasi filter.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json): Penambahan hak akses granular baru `whatsappChat.readSensitive` untuk otorisasi melihat teks asli pesan tersensor.
  - **Utilitas & Engine Pemrosesan Teks Backend (`backend/src/utils/`)**:
    - [`backend/src/utils/profanityFilter.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/profanityFilter.js) [NEW]: Engine murni fungsional tanpa I/O:
      - Normalisasi daftar kata masukan admin (sanitasi, deduplikasi, batasan panjang 2–40 karakter, maksimal 500 kata).
      - Penyusunan ekspresi reguler gabungan dengan variasi leetspeak (`[a4@]`, `[i1!]`, `[e3]`, `[o0]`, `[s5$]`, `[t7]`), toleransi spasi/tanda hubung internal, pengulangan karakter (`+`), dan imbuhan enklitik bahasa Indonesia (`-nya`, `-mu`, `-ku`, `-lah`, `-kah`, `-in`, `-an`, `2`).
      - Mitigasi ReDoS dengan pembatasan run dan struktur penjangkaran awalan kelas karakter.
      - Penggantian karakter penuh menjadi tanda bintang `*` sesuai jumlah karakter yang cocok.
    - [`backend/src/utils/waPhone.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waPhone.js) [NEW]: Fungsi normalisasi nomor telepon kanonik `toCanonicalPhone` untuk memastikan keseragaman format nomor internasional (E.164 tanpa `+`).
    - [`backend/src/utils/waChatSerializer.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waChatSerializer.js): Menerapkan filter kata kasar *fail-closed* `censorForAdmin` pada serialisasi pesan (`text`, `caption`, `reply_to.text`) dan percakapan (`last_message_preview`).
    - [`backend/src/utils/waChatAccess.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waChatAccess.js) [NEW]: Pengecekan otorisasi akses channel WhatsApp dan hak baca konten sensitif.
    - [`backend/src/utils/waChatUtils.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waChatUtils.js): Penyesuaian proyeksi dan normalisasi atribut sesi chat.
  - **Layanan Bisnis Backend (`backend/src/services/`)**:
    - [`backend/src/services/waContact.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waContact.service.js) [NEW]: Logika CRUD kontak buku telepon, penanganan *optimistic locking* `__v`, serta fungsi `syncConversationsToContact` untuk merefleksikan perubahan nama/catatan ke sesi-sesi percakapan aktif maupun arsip.
    - [`backend/src/services/waContactLink.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waContactLink.service.js) [NEW]: Penautan nomor telepon tak terdaftar ke entitas data master pelanggan/mitra yang sudah ada, validasi konflik nomor data master, dan pelepasan tautan secara aman.
    - [`backend/src/services/waContactInfo.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waContactInfo.service.js) [NEW]: Agregasi data komprehensif untuk dialog Info Kontak (informasi teknis nomor, foto profil, paket langganan aktif, status tagihan, tiket aktif, dan riwayat interaksi).
    - [`backend/src/services/waProfanity.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waProfanity.service.js) [NEW]: Manajemen cache filter kata kasar in-memory dengan auto-reload 60 detik atau sinkronisasi langsung saat pengaturan sistem disimpan.
    - [`backend/src/services/waProfilePicture.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waProfilePicture.service.js) [NEW]: Layanan jembatan penarikan foto profil WhatsApp kontak secara asinkron dari Baileys API dengan mekanisme caching.
    - [`backend/src/services/waChatContact.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waChatContact.service.js): Refactoring fungsi resolusi identitas kontak `resolveContactByWaId` dengan hierarki bertingkat: Data Master (pencocokan nomor langsung) → Tautan Buku Telepon ke Data Master → Nama Buku Telepon → Pushname Profil WhatsApp → Nomor Telepon Mentah.
    - [`backend/src/services/waConversation.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waConversation.service.js): Penyesuaian query datatable `findListWaSessionForTable` agar menyensor preview pesan terakhir secara konsisten.
    - [`backend/src/services/waMessage.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waMessage.service.js): Logika penyingkapan pesan asli (*reveal message*) dan pencatatan audit log terstruktur tanpa membocorkan konten teks ke log file.
    - [`backend/src/services/baileysControl.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/baileysControl.service.js) [NEW]: Integrasi kontrol koneksi dan pemanggilan endpoint internal Baileys API.
    - [`backend/src/services/waBroadcast.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waBroadcast.service.js) & [`waChatSweep.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waChatSweep.service.js): Penyelarasan identitas sesi pengiriman broadcast dan penutupan sesi otomatis.
  - **Kontroler & Rute API Backend (`backend/src/controllers/` & `backend/src/routes/`)**:
    - [`backend/src/controllers/waContact.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waContact.controller.js) [NEW]: Kontroler endpoint kontak (`getContact`, `upsertContact`, `deleteContact`, `linkContact`, `unlinkContact`, `getContactInfo`).
    - [`backend/src/controllers/waMessage.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waMessage.controller.js) [NEW]: Kontroler endpoint penyingkapan kata asli `revealMessage`.
    - [`backend/src/controllers/waChat.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waChat.controller.js), [`waInternal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waInternal.controller.js), [`baileysInternal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/baileysInternal.controller.js), [`settings.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/settings.controller.js), [`ticket.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/ticket.controller.js), [`workOrder.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/workOrder.controller.js): Penyelarasan penanganan error `res.status()`, delegasi status code, dan validasi kontak.
    - [`backend/src/routes/waChat.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/waChat.route.js): Pendaftaran rute-rute baru kontak dan pesan WhatsApp.
    - [`backend/src/sockets/waChat.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/sockets/waChat.controller.js): Emit event realtime socket (`wa:notify`, `wa:message`) dengan payload yang sudah tersensor rapi.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json): Penambahan ratusan kunci i18n untuk fitur phonebook, sensor kata kasar, dan manajemen kontak.
  - **Peningkatan Modul Baileys API (`baileys-api/`)**:
    - [`baileys-api/src/services/authStatePersister.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/authStatePersister.js) [NEW] & [`authStateStore.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/authStateStore.js): Persistensi kredensial autentikasi Baileys dengan sinkronisasi atomik untuk mencegah korupsi berkas sesi saat restart mendadak.
    - [`baileys-api/src/services/disconnectPolicy.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/disconnectPolicy.js) [NEW]: Kebijakan penanganan diskoneksi cerdas dengan *exponential backoff* untuk mencegah putus-nyambung berulang (*reconnection storm*).
    - [`baileys-api/src/services/offlineQueueGuard.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/offlineQueueGuard.service.js) [NEW]: Pengendali antrean pesan keluar saat koneksi offline untuk menghindari penumpukan memori dan pengiriman ganda pasca-reconnect.
    - [`baileys-api/src/services/contactProfile.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/contactProfile.service.js) [NEW]: Layanan internal untuk menarik URL foto profil dan status pengguna WhatsApp secara langsung dari socket Baileys.
    - [`baileys-api/src/utils/jidIdentity.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/utils/jidIdentity.js) [NEW]: Normalisasi dan ekstraksi JID WhatsApp (`@s.whatsapp.net`).
    - [`baileys-api/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/controllers/internal.controller.js) & [`routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/routes/internal.route.js): Pengeksposan endpoint internal aman bagi backend untuk membaca profil kontak dan mengelola sesi.
  - **Komponen & Antarmuka Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/whatsappChat/components/PhonebookSection.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/PhonebookSection.jsx) [NEW]: Bagian formulir buku telepon pada modal detail kontak untuk menyimpan nama, catatan bersama, dan mengelola tautan data master pelanggan/mitra.
    - [`frontend/src/app/pages/whatsappChat/components/ContactInfoModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ContactInfoModal.jsx) [NEW] & [`ContactInfoContent.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ContactInfoContent.jsx) [NEW]: Dialog komprehensif penampil info kontak, foto profil resolusi tinggi, status tautan pelanggan, paket langganan, dan riwayat pesan.
    - [`frontend/src/app/pages/whatsappChat/components/SaveContactModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/SaveContactModal.jsx) [NEW]: Dialog cepat (*quick save modal*) untuk menyimpan nomor tak dikenal langsung dari antarmuka obrolan.
    - [`frontend/src/app/pages/whatsappChat/components/CensoredText.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/CensoredText.jsx) [NEW]: Komponen penampil teks pesan tersensor yang dilengkapi tombol interaktif "Tampilkan" untuk membuka kata asli bagi admin yang memiliki privilege `whatsappChat.readSensitive`.
    - [`frontend/src/app/pages/whatsappChat/components/UnregisteredBadge.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/UnregisteredBadge.jsx) [NEW]: Lencana visual penanda bahwa nomor kontak belum tersimpan di buku telepon maupun data master.
    - [`frontend/src/app/pages/whatsappChat/components/ConversationAvatar.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ConversationAvatar.jsx) [NEW]: Komponen avatar percakapan dengan dukungan foto profil WhatsApp dinamis dan inisial cadangan (*fallback*).
    - [`frontend/src/app/pages/whatsappChat/components/ChatHeader.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ChatHeader.jsx), [`ConversationItem.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ConversationItem.jsx), dan [`MessageBubble.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/MessageBubble.jsx): Integrasi avatar, lencana buku telepon, tombol simpan kontak, dan sensor kata kasar pada gelembung percakapan.
    - [`frontend/src/app/pages/settings/sections/ProfanitySettingsFields.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/ProfanitySettingsFields.jsx) [NEW] & [`System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx): Antarmuka pengaturan daftar kata kasar pada menu Pengaturan Sistem (textarea per-baris, tombol simpan, dan tombol reset ke default).
    - Custom Hooks: [`useContactInfo.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/hooks/useContactInfo.js) [NEW], [`usePhonebook.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/hooks/usePhonebook.js) [NEW], [`useRevealMessage.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/hooks/useRevealMessage.js) [NEW].
    - Utilitas Frontend: [`contactKinds.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/utils/contactKinds.js) [NEW] dan [`phonebook.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/utils/phonebook.js) [NEW].
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json): Penambahan 160+ kunci terjemahan bilingual.
  - **Rangkaian Pengujian Otomatis Lengkap**:
    - Backend Integration & Unit Tests: [`waContact.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContact.controller.test.js), [`waContact.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContact.service.test.js), [`waContactInfo.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContactInfo.controller.test.js), [`waContactInfo.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContactInfo.service.test.js), [`waContactLink.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContactLink.service.test.js), [`waContactSync.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waContactSync.test.js), [`waProfanity.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waProfanity.service.test.js), [`profanityFilter.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/profanityFilter.test.js), [`waPhone.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/waPhone.test.js), [`settingsProfanity.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/settingsProfanity.test.js), [`waMessageReveal.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waMessageReveal.controller.test.js), [`waProfilePicture.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waProfilePicture.service.test.js), [`waChatSerializer.censor.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waChatSerializer.censor.test.js), [`waSessionListCensor.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waSessionListCensor.test.js).
    - Baileys API Tests: [`authStatePersister.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/test/authStatePersister.test.js), [`contactProfile.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/test/contactProfile.test.js), [`disconnectPolicy.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/test/disconnectPolicy.test.js), [`offlineQueueGuard.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/test/offlineQueueGuard.test.js), [`jidIdentity.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/test/jidIdentity.test.js).
    - Frontend Component & Hook Tests: [`CensoredText.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/CensoredText.test.jsx), [`ContactInfoContent.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ContactInfoContent.test.jsx), [`PhonebookSection.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/PhonebookSection.test.jsx), [`UnregisteredBadge.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/UnregisteredBadge.test.jsx), [`ConversationAvatar.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/whatsappChat/components/ConversationAvatar.test.jsx), [`ProfanitySettingsFields.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/ProfanitySettingsFields.test.jsx).
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan direktori kontak terpusat lintas admin dan channel obrolan sehingga nomor pelanggan baru atau nomor alternatif langsung memiliki identitas resmi dan catatan riwayat yang dapat diakses tim operasional secara kolaboratif.
  - Memungkinkan penautan nomor kontak ke data master (Retail, Reseller, Mitra, Bisnis) untuk menampilkan konteks layanan pelanggan secara instan saat sesi percakapan berlangsung.
  - Melindungi staf admin dan agen *customer service* dari paparan ujaran kebencian / kata-kata kotor melalui penyensoran otomatis yang cerdas dan aman dari serangan komputasi regex (ReDoS), sembari tetap menjaga integritas bukti hukum percakapan pada basis data.

---

## 🌿 Branch: `issue-365` — Penggabungan Modul Manajemen Fiber Optik ke Master & Production

### 📌 Informasi Issue

- **Nomor Issue**: #365
- **Judul Issue**: Perombakan Menyeluruh Manajemen Jaringan Fiber Optik (Single Source of Truth Sambungan `fiber_connections`, Geometri Invarian Presisi, Arsitektur UI Modular, Viewport Map Loader, & Otomasi Splice/Merge/Trace)
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` dan `production` via commit [`bf82dbe7`](file:///home/dhedhy/Project/Dekasimal-V2) pada 5 Oktober 2026, 12:48:47 WIB)

### 📅 Rincian Commit

#### [[`bf82dbe7`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #365 (Merge branch 'issue-365' ke master & production) - 5 Oktober 2026, 12:48:47 WIB

- **Komponen yang Berubah**:
  - Seluruh rangkaian pembaruan arsitektur fiber optik pada 84 berkas (skema `fiber_connections`, utilitas Turf.js `fiberGeometry.js`, modul RESTful singular kebab-case, adaptasi data lama `fiberLegacyAdapter.js`, komponen peta modular Leaflet, dan 158 test suite) resmi dipublikasikan ke branch utama dan lingkungan produksi.
- **Deskripsi Perubahan & Fungsi**:
  - Menuntaskan fase pengujian dan review kode; seluruh fungsionalitas peta fiber optik generasi baru kini aktif di seluruh sistem.
  - Menghilangkan anomali visual ujung kabel bergeser dari node, mencegah bentrok data sambungan optik secara atomik melalui multikey unique index MongoDB, serta melipatgandakan performa rendering peta pada area padat kabel melalui teknik pemuatan data spasial berbasis *bounding box viewport*.

---

## 🌿 Branch: `issue-368` — Perbaikan Batas Counter RRD Traffic & Latency RRA MAX Network Monitor

### 📌 Informasi Issue

- **Nomor Issue**: #368
- **Judul Issue**: Penyesuaian Batas Counter RRD Traffic (Data Source Max Limit Tune) & Penambahan RRA MAX pada RRD Latency Network Monitor (dengan Fitur Pemulihan Sistem & Auto-Discovery)
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`407329f3`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`407329f3`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #368 (Merge commit ke master) - 5 Oktober 2026, 11:45:59 WIB
#### [[`edaf814d`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #368 - 5 Oktober 2026, 11:45:24 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi Monorepo (`AGENTS.md`)**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md): Dokumentasi aturan testing `network-monitor`, konvensi satuan Byte/detik untuk batas RRD DS `COUNTER`, dan isolasi database saat pengujian.
  - **Layanan Microservice Network Monitor (`network-monitor/`)**:
    - [`network-monitor/src/utils/rrdLimits.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/utils/rrdLimits.js) [NEW]: Fungsi perhitungan batas maksimum aman `COUNTER` RRD: konversi bps ke Byte/detik dengan margin toleransi *framing overhead* Ethernet 20% (`bpsToRrdCounterMax`) dan normalisasi string kecepatan (`normalizeSpeedBps`).
    - [`network-monitor/src/utils/snmpInterface.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/utils/snmpInterface.js): Penambahan deteksi kapasitas antarmuka via OID 64-bit `ifHighSpeed` (Mbps) untuk router berkecepatan Gigabit ke atas (10G/40G) ketika OID klasik 32-bit `ifSpeed` mengalami overflow (*cap* di 4.29 Gbps) atau bernilai 0.
    - [`network-monitor/src/services/rrd.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/rrd.service.js): Peningkatan fungsi pembuatan dan *tuning* file RRD traffic dengan batas `max` dinamis yang akurat.
    - [`network-monitor/src/controllers/rrd.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/controllers/rrd.controller.js): Penambahan endpoint tuning `POST /rrd/tune/:target_id` dengan flag `onlyIncrease` dan `dryRun`.
    - [`network-monitor/src/services/latencyRrd.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/latencyRrd.service.js): Pembuatan struktur RRD latency baru dengan Round Robin Archive (RRA) tipe `MAX` untuk DS `rtt` dan `loss`, serta fungsi upgrade berkas RRD lama `upgradeLatencyRrdToIncludeMax` tanpa kehilangan data riwayat konsolidasi sebelumnya.
    - [`network-monitor/src/controllers/latency.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/controllers/latency.controller.js) & [`routes/latency.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/routes/latency.route.js): Pengeksposan endpoint upgrade berkas RRD latency `POST /latency/upgrade/:target_id`.
    - [`network-monitor/src/services/trafficPoller.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/trafficPoller.service.js) & [`snmp.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/snmp.service.js): Penyesuaian kalkulasi laju traffic polling dan penanganan nilai counter wrap-around.
  - **Layanan Pemulihan & Orkestrasi Backend (`backend/src/`)**:
    - [`backend/src/services/recovery.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/recovery.service.js):
      - `recoveryTrafficTargetData`: Orkestrasi pemulihan massal target traffic: (1) Menyelaraskan kecepatan antarmuka broadband pelanggan berdasarkan batas kecepatan paket internet aktif (`parseSpeedLimitBps`), (2) Menjalankan penemuan otomatis (*auto-discovery*) kapasitas antarmuka perangkat jaringan melalui query SNMP Walk paralel, (3) Memanggil endpoint tuning RRD di microservice network-monitor untuk menaikkan batas DS secara aman.
      - `recoveryLatencyTargetData`: Orkestrasi pemindaian seluruh target latency dan eksekusi batch upgrade RRD agar memiliki arsip RRA `MAX`.
      - Dukungan parameter `dryRun` pada kedua layanan untuk menampilkan rekapitulasi data dan sampel perubahan sebelum eksekusi riil.
    - [`backend/src/utils/validation-data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/validation-data.js): Utilitas `parseSpeedLimitBps` untuk mengonversi format teks batas kecepatan (misal `50M`, `100M/100M`, `1G`) menjadi besaran bit per detik (bps) numerik.
    - [`backend/src/controllers/networkTraffic.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkTraffic.controller.js) & [`networkLatency.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkLatency.controller.js): Penambahan handler endpoint recovery administratif berotorisasi.
    - [`backend/src/routes/networkTraffic.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkTraffic.route.js) & [`networkLatency.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkLatency.route.js): Pendaftaran rute API pemulihan sistem `/api/v1/network-traffic/recovery` dan `/api/v1/network-latency/recovery`.
    - [`backend/src/services/broadbandTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/broadbandTraffic.service.js) & [`networkTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkTraffic.service.js): Pengecekan batas kecepatan broadband dan sinkronisasi kapasitas target.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json): Penambahan pesan i18n untuk status recovery network monitor.
  - **Antarmuka Frontend Pengembang (`frontend/src/`)**:
    - [`frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js): Penambahan konfigurasi rute aksi pemulihan data target traffic dan latency pada menu *Developer Recovery Tools*.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json): Kunci teks bilingual untuk antarmuka tombol simulasi dan eksekusi pemulihan.
  - **Pengujian Otomatis Sistem (`test/`)**:
    - Microservice Network Monitor: [`latencyRrd.integration.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/latencyRrd.integration.test.js), [`rrdCounterCap.integration.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/rrdCounterCap.integration.test.js), [`rrdLimits.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/rrdLimits.test.js), [`snmpInterface.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/snmpInterface.test.js), [`trafficPoller.rate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/trafficPoller.rate.test.js).
    - Backend Integration & Unit: [`trafficRecovery.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/trafficRecovery.test.js), [`latencyRecovery.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/latencyRecovery.test.js), [`parseSpeedLimitBps.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/parseSpeedLimitBps.test.js).
- **Deskripsi Perubahan & Fungsi**:
  - Mengeliminasi *bug* pemotongan nilai grafik lalu lintas data (*traffic flatlining / capping*) pada antarmuka router jaringan berkapasitas besar dan paket internet pelanggan berkecepatan tinggi.
  - Memastikan grafik riwayat latensi jangka panjang menampilkan puncak lonjakan (*spike*) secara akurat dan tidak lagi terkompresi menjadi garis rata oleh fungsi konsolidasi `AVERAGE`.
  - Memberikan alat otomasi bagi administrator jaringan untuk memindai, mendiagnosis, dan memulihkan ratusan target pemantauan RRD yang bermasalah secara aman melalui simulasi *dry-run* dari panel admin.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #370  | Buku Telepon Obrolan WhatsApp & Sensor Kata Kasar Masuk | Pengenalan identitas nomor tak dikenal, penautan data master pelanggan/mitra, perlindungan tim CS dari kata kasar, dan stabilitas koneksi Baileys. |
| #368  | Perbaikan Batas Counter RRD Traffic & Latency RRA MAX | Akurasi visualisasi grafik lalu lintas data 10G+ tanpa *capping* dan preservasi lonjakan latency jangka panjang via RRA MAX & Recovery Tools. |
| #365  | Penggabungan Modul Fiber Optik ke Master & Production | Integrasi penuh arsitektur Single Source of Truth sambungan optik, geometri presisi, dan performa rendering peta berbasis viewport. |

### Kemampuan Baru Pengguna/Admin

- **Identifikasi Cepat & Penamaan Kontak WhatsApp**: Admin kini dapat langsung menyimpan nama dan catatan untuk nomor telepon baru yang menghubungi CS dari tombol simpan di *Chat Header* atau *Info Kontak*. Nama dan catatan tersebut langsung terlihat oleh seluruh admin lain secara serentak.
- **Penautan Nomor Kontak ke Pelanggan/Mitra**: Admin dapat menghubungkan nomor kontak WhatsApp alternatif pelanggan ke akun resminya di sistem (Retail, Reseller, Mitra, atau Bisnis). Segera setelah ditautkan, panel chat langsung memunculkan detail paket langganan, status tagihan, dan tiket aktif tanpa perlu beralih halaman.
- **Sensor Otomatis Kata Kasar Pelanggan**: Tampilan pesan masuk yang memuat kata-kata kotor secara otomatis disamarkan menjadi tanda bintang (`******`) pada seluruh komponen UI obrolan, kutipan balasan, pratinjau daftar, notifikasi realtime, dan unduhan transkrip.
- **Penyingkapan Kata Asli Selektif (Audit Trail)**: Supervisor atau admin dengan hak akses khusus `whatsappChat.readSensitive` dapat mengklik tombol "Tampilkan" pada gelembung pesan untuk membaca teks asli pelanggan jika diperlukan investigasi, dengan pencatatan audit log otomatis tanpa membocorkan kata kasar ke file log.
- **Kustomisasi Daftar Kosakata Terlarang**: Administrator dapat menyesuaikan (menambah, menghapus, atau mereset ke daftar baku) kosakata sensor melalui antarmuka Pengaturan Sistem tanpa perlu melakukan deploy ulang kode backend.
- **Perkakas Pemulihan Jaringan (*Network Recovery Tools*)**: Administrator jaringan dapat menjalankan simulasi (*dry-run*) dan perbaikan massal batas RRD traffic serta pemutakhiran file RRD latency langsung dari menu *Settings > Developer Tools*.

### Bug Fix / Solusi Masalah

- **Masalah Batas DS RRD Traffic Terpotong (*Capped Traffic*)**: Memperbaiki kekeliruan asumsi satuan batas maksimum DS RRD `COUNTER` (yang seharusnya berukuran Byte/detik, bukan bps). Menghilangkan anomali grafik traffic yang mendatar (*flatline*) saat traffic melampaui 125 MB/s pada antarmuka 10G/40G.
- **Masalah Hilangnya Spike Latensi Jangka Panjang**: Menambahkan Round Robin Archive (RRA) tipe `MAX` pada RRD latency sehingga puncak lonjakan latensi dan kehilangan paket (*packet loss*) mingguan/bulanan/tahunan tetap terlihat jelas.
- **Masalah Tabrakan Data Modifikasi Kontak (*Race Condition*)**: Menerapkan mekanisme Mongoose *optimistic locking* (`__v`) sehingga jika dua admin mencoba memperbarui kontak atau catatan yang sama secara bersamaan, konflik akan ditolak secara elegan (HTTP 409) dan mencegah *last-write-wins*.
- **Pencegahan Eksploitasi ReDoS pada Teks Obrolan**: Mengamankan engine pencocokan ekspresi reguler kata kasar dari risiko *Catastrophic Backtracking* melalui sanitasi input, pemangkasan kuantifier berulang, dan struktur penjangkaran kelas karakter.
- **Pencegahan Diskoneksi Beruntun Baileys**: Menerapkan kebijakan *reconnection backoff*, persistensi state atomik, dan pembatas antrean pesan offline untuk menjaga kestabilan sesi WhatsApp Web.

### Menu/Fitur Baru

- **Modal Info Kontak & Buku Telepon WhatsApp**: Antarmuka terpadu pada Obrolan WhatsApp untuk mengelola nama kontak, catatan operasional, penautan ke database pelanggan, dan melihat foto profil WhatsApp.
- **Pengaturan Sensor Kata Kasar (System Settings)**: Bagian konfigurasi baru pada panel *Settings > System* untuk mengatur daftar kosakata yang disensor pada pesan masuk.
- **Tombol Penyingkapan Pesan Sensitif (*Reveal Sensitive Text*)**: Tombol interaktif pada komponen *Message Bubble* obrolan pelanggan bagi pengguna yang memiliki otorisasi `whatsappChat.readSensitive`.
- **Panel Developer Recovery Tools**: Menu pemulihan data infrastruktur jaringan untuk menyelaraskan target pemantauan traffic dan latensi secara massal.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Buku Telepon & Penautan Identitas Kontak WhatsApp

- **Penjelasan Fitur**:
  Fitur Buku Telepon WhatsApp memungkinkan tim operasional memberikan identitas resmi dan catatan bersama untuk nomor-nomor yang belum terdaftar di sistem. Selain menyimpan nama, nomor tersebut dapat ditautkan ke data master pelanggan/mitra sehingga admin dapat langsung memverifikasi paket internet, status koneksi, atau nomor tiket pelanggan saat sedang melayani obrolan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **WhatsApp > Obrolan** di Web Admin.
  2. Pilih salah satu percakapan dari nomor yang belum dikenal (ditandai dengan lencana oranye *Belum Terdaftar*).
  3. Klik nama/nomor kontak pada bagian atas (*Chat Header*) untuk membuka dialog **Info Kontak**.
  4. Masukkan **Nama Kontak** dan **Catatan** (misal: "Nomor alternatif Ibu Siti - PIC Keuangan").
  5. Jika nomor tersebut merupakan nomor kedua dari pelanggan yang sudah berlangganan:
     - Klik tombol **Tautkan ke Data Master**.
     - Pilih kategori entitas (*Pelanggan Retail*, *Pelanggan Mitra*, *Mitra Bisnis*, atau *Pelanggan Bisnis*).
     - Cari nama atau ID pelanggan pada kotak pencarian, lalu pilih data pelanggan yang sesuai.
  6. Klik tombol **Simpan Kontak**.
  7. Seketika nama kontak, ID pelanggan, area, serta paket langganan akan muncul di header percakapan dan daftar sesi obrolan tanpa perlu memuat ulang halaman.

---

### 2. Pengelolaan Sensor Kata Kasar & Penyingkapan Pesan Asli

- **Penjelasan Fitur**:
  Sensor kata kasar bekerja secara otomatis di sisi server untuk menyaring kata-kata tidak pantas dari pesan masuk pelanggan. Supervisor yang memerlukan pembuktian keluhan atau investigasi tetap dapat melihat teks asli secara aman dan terkontrol.
- **Langkah Penggunaan (Tutorial)**:
  1. **Melihat Pesan Tersensor**:
     - Setiap pesan pelanggan yang memuat kosakata kasar akan otomatis tampil dalam format tanda bintang (contoh: `******`).
     - Jika akun Anda memiliki hak akses `whatsappChat.readSensitive`, di sebelah kanan teks tersensor akan muncul tombol kecil bergambar mata atau tautan **Tampilkan**.
     - Klik tombol **Tampilkan** untuk memuat dan melihat kata asli pesan tersebut.
  2. **Menyesuaikan Daftar Kosakata Terlarang**:
     - Masuk ke menu **Pengaturan > Sistem**.
     - Gulir ke bagian **Sensor Kata Kasar WhatsApp**.
     - Tambahkan kata baru pada baris baru dalam kotak teks, atau hapus kata yang tidak ingin disensor.
     - Klik **Simpan Pengaturan**. Daftar filter akan segera diperbarui secara realtime ke seluruh sistem backend.

---

### 3. Pemulihan Massal Batas RRD Traffic & Upgrade RRA Latency

- **Penjelasan Fitur**:
  Fasilitas pemulihan sistem pada menu Pengembang memungkinkan penyelarasan otomatis konfigurasi RRD microservice network monitor dengan kapasitas *real-world* router dan paket broadband pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan > Developer Tools > Recovery Tools**.
  2. Pada kartu **Recovery Traffic Target**:
     - Klik tombol **Simulasi (Dry Run)** untuk memeriksa berapa banyak target broadband dan SNMP yang kecepatan antarmukanya akan diperbarui serta file RRD yang perlu dinaikkan batas DS-nya.
     - Periksa ringkasan output simulasi.
     - Jika data telah sesuai, klik tombol **Jalankan Pemulihan (Eksekusi)** untuk menerapkan perubahan ke database dan menala (*tune*) file RRD di network monitor.
  3. Pada kartu **Recovery Latency Target**:
     - Klik tombol **Simulasi (Dry Run)** untuk menghitung berkas RRD ping monitor yang belum memiliki arsip RRA `MAX`.
     - Klik **Jalankan Upgrade** untuk mengompilasi penambahan RRA `MAX` ke berkas-berkas RRD yang ada.
