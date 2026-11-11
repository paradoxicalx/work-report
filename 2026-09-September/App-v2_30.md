# 📝 Daily Work Report - Dedy S.N Putra (2026-09-30)

---

## 📅 Laporan Harian - 30 September 2026

> Laporan ini mencakup seluruh sesi pekerjaan per 30 September 2026, mulai dari finalisasi integrasi merge modul ACS TR-069 pada dini hari, implementasi standarisasi skema penagihan multi-bulan (Issue #345), stabilisasi arsitektur multi-instance backend Socket.IO & Redis (Issue #352), standarisasi validasi tanggal & perbaikan form (Issue #354), hingga penambahan konfigurasi topik tiket Telegram pada branch aktif `issue-346`.

---

## 🌿 Branch: `issue-346` — Penambahan Opsi Topik Tiket (Bisnis, Backbone, Backhaul) pada Pengaturan Notifikasi Telegram

### 📌 Informasi Issue

- **Nomor Issue**: #346
- **Judul Issue**: Penambahan Opsi Topik Tiket (Bisnis, Backbone, Backhaul) pada Pengaturan Notifikasi Grup & Topik Telegram
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-346`, pekerjaan dalam status _working directory changes_ / persiapan commit)

### 📅 Rincian Commit

#### [Work In Progress] - Penyelarasan Opsi Tipe Topik Tiket Frontend dengan Backend - 30 September 2026

- **Komponen yang Berubah**:
  - **Antarmuka Pengaturan Grup Telegram (`frontend/src/`)**:
    - [`frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx):
      - Menambahkan tipe topik tiket baru: `business` (`Tiket Bisnis`), `backbone` (`Tiket Backbone`), dan `backhaul` (`Tiket Backhaul`) ke dalam pemetaan objek `ticketTypeLabels`.
      - Menyelaraskan daftar pilihan topik di antarmuka web dengan konstanta `TICKET_TOPIC_TYPES` yang sudah didukung pada routing notifikasi backend ([`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js)).
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
      - Mendaftarkan kunci terjemahan bilingual untuk `"business"`, `"backbone"`, dan `"backhaul"` pada kelompok `settings.telegram.topicTypes`.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan administrator dapat memetakan dan mengarahkan notifikasi tiket operasional berkategori Bisnis, Backbone jaringan, maupun Backhaul transmisi ke topik/forum Telegram yang spesifik di grup Telegram perusahaan.
  - Mencegah hilangnya label teks atau kegagalan pemetaan jenis tiket baru saat admin mengonfigurasi routing notifikasi per-cabang.

---

## 🌿 Branch: `issue-354` — Standarisasi Parsing & Validasi Input Tanggal Menyeluruh (`date-input.js`), Penanganan Duplikasi Jadwal (HTTP 409 Conflict), Pemulihan Rules of Hooks & Tautan Profil pada `AdminLink`

### 📌 Informasi Issue

- **Nomor Issue**: #354
- **Judul Issue**: Standarisasi Parsing & Validasi Input Tanggal Menyeluruh (`date-input.js`), Penanganan Duplikasi Jadwal (HTTP 409 Conflict), Pemulihan Rules of Hooks & Tautan Profil pada `AdminLink`
- **Status Branch**: `Sudah di-merge` (Telah diselesaikan dan di-merge ke `origin/master` serta `origin/production` melalui merge commit [`5b3a93aa`](file:///home/dhedhy/Project/Dekasimal-V2) pada 30 September 2026, 22:14:50 WIB)

### 📅 Rincian Commit

#### [[`5b3a93aa`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #354 - 30 September 2026, 22:14:50 WIB

_(Merge commit penggabungan branch `issue-354` ke `master` & `production`)._

#### [[`3ee9134c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #354 - 30 September 2026, 21:38:37 WIB

- **Komponen yang Berubah**:
  - **Backend Utilitas & Validasi Tanggal Terpusat (`backend/src/`)**:
    - [`backend/src/utils/date-input.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/date-input.js) [NEW]:
      - Membuat utilitas parsing tanggal ketat (`parseDateInput` dan `requireDateInput`) yang menerima format tanggal-saja (`DD-MM-YYYY`, `D-M-YYYY`, `DD/MM/YYYY`, `D/M/YYYY`, `YYYY-MM-DD` -> diurai sebagai awal hari / tengah malam lokal) serta format ISO-8601 lengkap (`2026-09-29T17:00:00.000Z`) dari DatePicker frontend.
      - Menolak input tanggal tidak valid dengan HTTP 400 Bad Request secara eksplisit sebelum menyentuh lapisan database.
    - [`backend/src/utils/validation-data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/validation-data.js):
      - Mengintegrasikan `parseDateInput` ke dalam helper `resolveInvoiceActivatedDay` guna mencegah pergeseran tanggal aktivasi tagihan akibat perbedaan format zona waktu.
    - [`backend/src/controllers/scheduler.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/scheduler.controller.js) & [`backend/src/services/scheduler.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/scheduler.service.js):
      - Menerapkan fungsi `parseScheduleDate` untuk validasi format tanggal rencana kerja pada pembuatan (`createSchedule`) dan pembaruan (`updateSchedule`).
      - Menambahkan helper `throwIfDuplicateSchedule` yang menangkap error index unik MongoDB (E11000) pada field `date` (`date_1`) dan `name` (`name_1`), lalu mengembalikan status HTTP 409 Conflict dengan pesan i18n (`scheduler.dateExists`, `scheduler.nameExists`).
    - [`backend/src/controllers/admin.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/admin.controller.js) & [`backend/src/controllers/employee.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/employee.controller.js):
      - Mengganti parsing tanggal bergabung (`join_date`) yang sebelumnya kaku dengan `requireDateInput(data.join_date, req, res).toDate()`, mencakup endpoint pembuatan, penyuntingan tunggal, dan penyuntingan batch (`updateBatchAdmin`, `updateBatchEmployee`).
    - [`backend/src/controllers/productDataAccess.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDataAccess.controller.js) & [`backend/src/controllers/productDedicatedInternet.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDedicatedInternet.controller.js):
      - Memvalidasi parameter tanggal kontrak dan tanggal mulai layanan dengan `requireDateInput`.
    - [`backend/src/controllers/partnerApiRadius.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiRadius.controller.js) & [`backend/src/controllers/radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js):
      - Menyelaraskan parsing input tanggal aktivasi pelanggan broadband dan partner API RADIUS.
    - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json):
      - Menambahkan pesan terjemahan: `scheduler.dateExists` ("Jadwal pada tanggal tersebut sudah ada"), `scheduler.nameExists` ("Nama jadwal sudah digunakan"), dan `scheduler.dateInvalid` ("Format tanggal tidak valid").
  - **Antarmuka Pengguna & Komponen Tabel Frontend (`frontend/src/`)**:
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx):
      - _Perbaikan Rules of Hooks pada `AdminLink`_: Memindahkan pemanggilan `useHasPrivilege('admin.read')` dan `useHasPrivilege('employee.read')` ke level teratas komponen tanpa syarat (sebelumnya dipanggil bersyarat di dalam ekspresi ternary JSX yang melanggar aturan React Hooks).
      - _Perbaikan Endpoint Avatar Karyawan_: Mengarahkan sumber gambar avatar selalu ke `/file/admin-avatar/${id}` (karena karyawan dan admin disimpan dalam koleksi yang sama `Admin`), mengeliminasi status 404 pada tautan avatar karyawan.
      - _Pemisahan Hak Akses_: Hak akses kini murni mengendalikan kemampuan klik untuk membuka halaman profil (`canOpenProfile`), tanpa menyembunyikan atau merusak tampilan avatar visual.
    - **Penyelarasan Komponen Form & DatePicker Modal/Drawer**:
      - [`frontend/src/app/pages/activities/components/CreatePaidLeaveModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/components/CreatePaidLeaveModal.jsx) & [`frontend/src/app/pages/activities/components/CreatePermissionModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/components/CreatePermissionModal.jsx)
      - [`frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/components/TeamFormModal.jsx), [`create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/create.jsx), & [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/edit.jsx)
      - [`frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx), [`ExpenseDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/expenses/ExpenseDrawer.jsx), [`expenses/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/expenses/detail.jsx), [`InvoiceDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx), [`RecurringDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/recurring/RecurringDrawer.jsx), & [`BalanceSheet.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/BalanceSheet.jsx)
      - [`frontend/src/app/pages/users/admin/editBatch.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/admin/editBatch.jsx) & [`frontend/src/app/pages/users/employee/editBatch.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/employee/editBatch.jsx)
      - Memastikan payload tanggal yang dikirimkan ke backend terformat secara konsisten dan tidak menghasilkan nilai null atau string acak.
  - **Pengujian Unit & Integrasi (`backend/test/`)**:
    - [`backend/test/unit/dateInput.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/dateInput.test.js) [NEW]: 78 baris pengujian unit parser tanggal ketat mencakup berbagai variasi format, zona waktu, dan data tidak valid.
    - [`backend/test/integration/scheduler.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/scheduler.controller.test.js) [NEW]: 154 baris pengujian integrasi controller scheduler untuk penanganan duplikasi tanggal/nama (HTTP 409) dan validasi tanggal tidak valid (HTTP 400).
    - [`backend/test/integration/scheduler.officerAvatar.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/scheduler.officerAvatar.test.js) [NEW]: 44 baris pengujian resolusi avatar petugas scheduler.
    - [`backend/test/integration/radiusAuthentication.invoiceOnActivated.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthentication.invoiceOnActivated.test.js): Menambahkan 44 baris pengujian integrasi aktivasi langganan broadband dengan format tanggal fleksibel.
- **Deskripsi Perubahan & Fungsi**:
  - Menghilangkan bug kritis di mana pengiriman input tanggal berformat ISO dari frontend DatePicker menyebabkan parsing error atau tersimpan sebagai tanggal yang salah (pergeseran hari) di database.
  - Mencegah crash error 500 generik saat staf membuat jadwal rencana kerja dengan tanggal atau nama yang sama persis; sistem kini memberikan notifikasi konflik yang jelas dan ramah pengguna (HTTP 409).
  - Memperbaiki komponen sel tabel `AdminLink` agar patuh pada prinsip React Hooks dan memastikan avatar staf selalu tampil dengan tepat tanpa gangguan error 404.

---

## 🌿 Branch: `issue-352` — Sinkronisasi Socket.IO Multi-Instance Backend via Redis Adapter (`@socket.io/redis-adapter`), Migrasi Ring Buffer & State RADIUS ke Redis, Eliminasi False 'Printer Tidak Terhubung', Presensi Admin Obrolan WhatsApp Lintas Instance, serta Resiliensi Reconnect Baileys API

### 📌 Informasi Issue

- **Nomor Issue**: #352
- **Judul Issue**: Sinkronisasi Socket.IO Multi-Instance Backend via Redis Adapter (`@socket.io/redis-adapter`), Migrasi Ring Buffer & State RADIUS ke Redis, Eliminasi False 'Printer Tidak Terhubung', Presensi Admin Obrolan WhatsApp Lintas Instance, serta Resiliensi Reconnect Baileys API
- **Status Branch**: `Sudah di-merge` (Telah diselesaikan dan di-merge ke `origin/master` melalui merge commit [`79222401`](file:///home/dhedhy/Project/Dekasimal-V2) pada 30 September 2026, 22:09:09 WIB)

### 📅 Rincian Commit

#### [[`79222401`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #352 - 30 September 2026, 22:09:09 WIB

_(Merge commit penggabungan seluruh milestone perbaikan multi-instance issue #352 ke master)._

#### [[`3c51824a`](file:///home/dhedhy/Project/Dekasimal-V2)], [[`2759b36f`](file:///home/dhedhy/Project/Dekasimal-V2)], [[`601e19c8`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #352 - 30 September 2026, 20:34:59 WIB

- **Komponen yang Berubah**:
  - **Sinkronisasi Socket.IO Lintas Instance (`backend/src/sockets/`)**:
    - [`backend/package.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/package.json): Menambahkan pustaka `@socket.io/redis-adapter` v8.3.0.
    - [`backend/src/sockets/socket-io.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/sockets/socket-io.js):
      - Mengonfigurasi adapter Redis (`buildRedisAdapter`) dengan koneksi `pubClient` dan `subClient` terpisah untuk menyiarkan event WebSocket ke seluruh pod/instance backend.
      - Memecahkan masalah event hilang ketika request internal (mis. event QR dari Baileys atau gRPC radius) diterima oleh instance A namun koneksi WebSocket klien berada di instance B.
  - **Penyimpanan State & Ring Buffer RADIUS di Redis (`backend/src/services/radiusEvent.service.js`)**:
    - [`backend/src/services/radiusEvent.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusEvent.service.js):
      - Memigrasikan ring buffer memori `traceBuffer` dan `connectionLogBuffer` ke Redis List (`radius:trace-buffer` dan `radius:connlog-buffer`) dengan perintah `rpush` dan pembatasan kapasitas `ltrim`.
      - Memigrasikan health status per-server ke Redis Hash (`radius:health`).
      - Memigrasikan tracking IP NAS (`nasSeenMap`) ke Redis Sorted Set (`radius:nas-seen`), dengan TTL pangkas otomatis via `zremrangebyscore` untuk record di atas 24 jam.
      - Memastikan endpoint REST `GET /radius/snapshot` selalu menyajikan data lengkap meskipun request jatuh ke instance backend yang tidak terhubung langsung ke stream gRPC radius-server.
  - **Koreksi Presensi Print Station & Eliminasi False Disconnect (`backend/src/sockets/printHandler.js`)**:
    - [`backend/src/sockets/printHandler.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/sockets/printHandler.js), [`backend/src/services/print.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/print.service.js), & [`backend/src/controllers/print.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/print.controller.js):
      - Menghapus in-memory Map `printerStations`. Menjadikan room `printer-${userId}` sebagai sumber kebenaran status online melalui `io.in(room).fetchSockets()`.
      - Memastikan perintah cetak label barcode disebarkan ke room pengguna yang relevan secara akurat lintas instance server tanpa terganggu lokasi koneksi socket.
  - **Presensi Admin WhatsApp & Pencegahan Salah Auto-Reply (`backend/src/sockets/waChat.controller.js`)**:
    - [`backend/src/sockets/waChat.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/sockets/waChat.controller.js):
      - Mengganti tracking memori `inboxPresence` dengan pembacaan room cluster `io.in(WA_INBOX_ROOM).fetchSockets()`.
      - Memastikan webhook pesan masuk dari nomor pelanggan mengenali keberadaan staf admin yang sedang aktif membuka obrolan di instance manapun, mencegah pemicuan pesan balasan otomatis (auto-reply) yang salah saat obrolan sedang ditangani langsung oleh manusia.
  - **Resiliensi Koneksi & Anti-Ban Baileys API (`baileys-api/`)**:
    - [`baileys-api/src/services/sessionManager.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/sessionManager.service.js):
      - Menerapkan batasan maksimal 5 kali reconnect berturut-turut (`MAX_PREPAIR_RECONNECT_ATTEMPTS = 5`) untuk akun yang belum pernah berhasil tersambung (`session.everConnected === false`).
      - Menghentikan looping koneksi tanpa akhir jika proses pairing gagal berulang kali, melindungi nomor telepon WhatsApp perusahaan dari pemblokiran atau penandaan spam oleh pihak Meta.
    - [`baileys-api/src/services/authStateStore.js`](file:///home/dhedhy/Project/Dekasimal-V2/baileys-api/src/services/authStateStore.js):
      - Menambahkan fungsi `clearAuthState` untuk membersihkan kredensial auth MongoDB hanya pada kondisi logout eksplisit atau sesi korup, dan tidak menghapus sesi pada error kode 403 `forbidden`.
  - **Antarmuka Pengguna Frontend & Penanganan Pairing Gagal (`frontend/src/`)**:
    - [`frontend/src/app/pages/chats/components/ConnectAccountModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/chats/components/ConnectAccountModal.jsx):
      - Menambahkan status visual "Gagal Terhubung" dan tombol "Coba Lagi" saat proses pembuatan kode QR atau penyambungan akun mencapai batas timeout/percobaan, memberikan instruksi yang jelas kepada staf operasional.
    - [`frontend/src/i18n/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/) (`en` & `id`): Menambahkan kunci terjemahan `connectFailedTitle` dan `retryConnect`.
  - **Pengujian & Stub Redis Terdistribusi (`backend/test/`)**:
    - [`backend/test/setup/redis.stub.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/setup/redis.stub.js):
      - Memperkaya stub pengujian Redis in-memory dengan metode cluster: `rpush`, `ltrim`, `lrange`, `zadd`, `zcard`, `zremrangebyscore`, `hset`, dan `hgetall`.
    - [`backend/test/unit/radiusEventLiveDedup.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/radiusEventLiveDedup.test.js) & [`backend/test/unit/radiusEventConnectionLogBuffer.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/radiusEventConnectionLogBuffer.test.js):
      - Menyesuaikan pengujian deduplikasi event RADIUS dan ring buffer dengan metode penyimpanan Redis.
- **Deskripsi Perubahan & Fungsi**:
  - Menuntaskan permasalahan laten pada arsitektur server multi-instance (load balanced), di mana event real-time seperti QR code WhatsApp, notifikasi Print Station, dan pemantauan RADIUS sering kali "hilang di tengah jalan" akibat tersangkut di instance yang berbeda.
  - Memberikan perlindungan keamanan tinggi bagi nomor WhatsApp operasional agar tidak terkena ban otomatis dari WhatsApp akibat percobaan reconnect berulang tanpa henti saat gagal pairing.
  - Menjamin kelancaran operasional pencetakan barcode inventaris/stok gudang tanpa kendala "Printer tidak terhubung" palsu.

---

## 🌿 Branch: `issue-345` — Standarisasi Skema Periode Tagihan Otomatis (Billing Scheme) Multi-Bulan, Dukungan Mode Penerbitan Fleksibel (Awal Bulan vs Tanggal Aktivasi), Helper Periode Terpadu (`billingPeriod.js`), dan Deteksi Overlapping Invoice

### 📌 Informasi Issue

- **Nomor Issue**: #345
- **Judul Issue**: Standarisasi Skema Periode Tagihan Otomatis (Billing Scheme) Multi-Bulan, Dukungan Mode Penerbitan Fleksibel (Awal Bulan vs Tanggal Aktivasi), Helper Periode Terpadu (`billingPeriod.js`), dan Deteksi Overlapping Invoice
- **Status Branch**: `Sudah di-merge` (Telah diselesaikan dan di-merge ke `origin/master` melalui merge commit [`07a4dfab`](file:///home/dhedhy/Project/Dekasimal-V2) pada 30 September 2026, 19:38:30 WIB)

### 📅 Rincian Commit

#### [[`07a4dfab`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #345 - 30 September 2026, 19:38:30 WIB

_(Merge commit penggabungan implementasi skema penagihan multi-bulan issue #345 ke master)._

#### [[`247316de`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #345 - 30 September 2026, 19:37:27 WIB

- **Komponen yang Berubah**:
  - **Modul Inti Kalkulasi Periode Tagihan (`backend/src/utils/billingPeriod.js`)**:
    - [`backend/src/utils/billingPeriod.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/billingPeriod.js) [NEW]:
      - Pustaka kalkulasi murni (tanpa model DB) untuk standarisasi siklus penagihan broadband:
        - `getEffectiveBillingPeriod(auth)`: Membaca siklus efektif pelanggan (override akun menang atas default paket, fallback 1 bulan).
        - `getAnchorDate(auth)`: Mengambil tanggal jangkar (`activated`, fallback ke `created_at`).
        - `getIssueMode(auth)`: Membedakan mode penerbitan (`activation` vs `month_start`).
        - `resolveAnniversaryPeriodWindow(anchorDate, periodMonths, referenceDate)`: Menghitung rentang tanggal layanan `[start, end]` (inklusif) berdasarkan kelipatan bulan aktivasi, lengkap dengan perataan tanggal akhir bulan (_end-of-month clamping_ untuk bulan 28/29/30/31 hari).
        - `resolveMonthStartPeriodWindow(anchorDate, periodMonths, referenceDate)`: Menghitung rentang layanan berbasis tanggal 1 bulan kalender.
        - `isDueThisCycle(auth, today)` & `shouldCatchUp(auth, today)`: Menentukan jatuh tempo siklus tagihan dan penerbitan tagihan susulan yang terlewat.
  - **Layanan Pembuatan Tagihan Otomatis (`backend/src/services/financeAutoInvoice.service.js`)**:
    - [`backend/src/services/financeAutoInvoice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeAutoInvoice.service.js):
      - Memisahkan secara tegas konsep **Mode Terbit** (kapan tagihan terbit: awal bulan atau tanggal aktivasi) dengan **Skema Periode** (`billing_scheme`: `follow_auto_invoice` atau `activation_period`).
      - Mengganti fungsi validasi per-bulan kalender dengan `hasInvoiceOverlappingPeriod(refAuthId, start, end, { alsoMonthOf })` untuk mendeteksi tumpang-tindih periode layanan secara akurat, mengeliminasi risiko penagihan ganda (*double billing*).
      - Menyelaraskan pembuatan invoice prorata aktivasi (`createProratedInvoiceForRemainingMonth`) agar menggunakan rentang tanggal dari helper terpusat.
  - **Model Database & Pengaturan Keuangan (`backend/src/models/` & `backend/src/controllers/`)**:
    - [`backend/src/models/financeInvoice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeInvoice.model.js):
      - Menambahkan field `billing_period_start: { type: Date }` dan `billing_period_end: { type: Date }` pada dokumen Invoice untuk mencatat masa berlaku layanan yang ditagih, lengkap dengan indeks pencarian rentang waktu.
    - [`backend/src/controllers/financeSettings.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeSettings.controller.js) & [`backend/src/routes/financeSettings.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/financeSettings.route.js):
      - Mendaftarkan field pengaturan `billing_scheme` dengan pilihan `follow_auto_invoice` atau `activation_period`.
    - [`backend/src/services/invoiceFreeze.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/invoiceFreeze.service.js) & [`backend/src/services/financeLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeLedger.service.js):
      - Menyelaraskan pembukuan jurnal dan penanganan faktur beku dengan rentang periode tagihan.
  - **Antarmuka Pengguna & Peringatan Konfigurasi Langganan (`frontend/src/`)**:
    - [`frontend/src/app/pages/settings/sections/Finance.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/Finance.jsx):
      - Menambahkan opsi konfigurasi global "Skema Periode Tagihan" pada menu Pengaturan Keuangan.
      - Menampilkan peringatan resiko (*warning alert*) jika admin mengubah skema periode di tengah sistem yang sedang berjalan.
    - [`frontend/src/app/pages/services/broadband/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/edit.jsx) & [`create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/create.jsx):
      - Memperbarui validasi siklus tagihan (`billingCycleMismatch`) agar mode multi-bulan diakui valid baik untuk opsi "Sesuai tanggal aktivasi" maupun "Setiap awal bulan".
      - Menambahkan peringatan dinamis `billingCycleChange` saat admin mengubah mode atau siklus langganan yang sedang berjalan agar admin memeriksa faktur yang telah terbit.
    - [`frontend/src/i18n/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/) (`en` & `id`):
      - Menambahkan teks terjemahan konfigurasi skema penagihan dan peringatan pergeseran siklus langganan.
  - **Pengujian Unit & Integrasi Ekstensif (`backend/test/`)**:
    - [`backend/test/unit/billingPeriod.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/billingPeriod.test.js) [NEW]: 167 baris test kalkulasi periode anniversary, tanggal kabisat, perataan akhir bulan, dan siklus multi-bulan.
    - [`backend/test/unit/computeNextBillingDate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/computeNextBillingDate.test.js) [NEW]: 87 baris test kalkulasi tanggal tagihan berikutnya.
    - [`backend/test/unit/resolveInvoiceActivatedDay.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/resolveInvoiceActivatedDay.test.js) [NEW]: 43 baris test parsing tanggal aktivasi.
    - [`backend/test/integration/financeAutoInvoice.activationPeriod.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeAutoInvoice.activationPeriod.test.js) [NEW]: 494 baris test integrasi skema `activation_period`.
    - [`backend/test/integration/financeAutoInvoice.simulation.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeAutoInvoice.simulation.test.js) [NEW]: 387 baris test simulasi kronologis penerbitan tagihan tahunan/kuartalan.
    - [`backend/test/integration/financeSettings.billingScheme.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeSettings.billingScheme.test.js) [NEW]: 67 baris test update skema penagihan.
    - [`backend/test/integration/radiusAuthentication.invoiceOnActivated.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthentication.invoiceOnActivated.test.js) [NEW]: 123 baris test penerbitan invoice langsung saat aktivasi akun broadband.
    - [`backend/test/integration/financeAutoInvoice.prorata.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeAutoInvoice.prorata.test.js) & [`financeAutoInvoice.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeAutoInvoice.test.js): Memperluas pengujian prorata dan cron tagihan otomatis.
- **Deskripsi Perubahan & Fungsi**:
  - Menghadirkan fleksibilitas penagihan multi-bulan (paket 3 bulan, 6 bulan, 1 tahun, dsb.) dengan perhitungan masa aktif yang presisi hingga hari terakhir periode layanan.
  - Memberikan kendali penuh bagi perusahaan untuk memilih apakah periode tagihan mengikuti pola kalender bulanan atau selalu dihitung murni N bulan sejak tanggal aktivasi pelanggan.
  - Melindungi pelanggan dari risiko penagihan ganda (*overcharging*) ketika terjadi perubahan tanggal aktivasi atau penerbitan faktur mendahului tanggal awal periode.

---

## 🌿 Branch: `issue-175` — Finalisasi Integrasi Modul Monitoring & Remote Configuration ONT TR-069 (ACS) ke Master

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Implementasi Sistem Monitoring & Remote Configuration ONT TR-069 (ACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal Optik
- **Status Branch**: `Sudah di-merge` (Finalisasi commit [`ad80ce2d`](file:///home/dhedhy/Project/Dekasimal-V2) berhasil di-merge ke branch utama `master` melalui merge commit [`110cd48e`](file:///home/dhedhy/Project/Dekasimal-V2) pada 30 September 2026, 01:05:01 WIB)

### 📅 Rincian Commit

#### [[`110cd48e`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #175 - 30 September 2026, 01:05:01 WIB

_(Merge commit integrasi final penyelesaian issue #175 ke branch utama master)._

#### [[`ad80ce2d`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #175 - 30 September 2026, 00:34:30 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Menyatukan seluruh modul arsitektur microservice GenieACS TR-069, auto-provisioning parameter ONT, deteksi degradasi optik, manajemen multi-WAN, dan penanganan tabrakan username PPPoE ke dalam repositori utama.
  - Memastikan seluruh pengujian otomatis (unit & integrasi) berjalan sukses pada branch utama, mengukuhkan kesiapan rilis fitur monitoring modem jarak jauh pelanggan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #346  | Penambahan Opsi Topik Tiket Telegram (Bisnis, Backbone, Backhaul) | Memungkinkan admin mengarahkan notifikasi tiket operasional berkategori Bisnis, Backbone, dan Backhaul ke topik forum Telegram spesifik sesuai divisi penanganan. |
| #354  | Standarisasi Parsing Tanggal (`date-input.js`), Penanganan Duplikasi Jadwal (409), & Perbaikan `AdminLink` | Menyeragamkan validasi input tanggal (ISO/DatePicker), menangani duplikasi jadwal harian dengan status HTTP 409 Conflict ramah pengguna, serta memulihkan Rules of Hooks dan tautan avatar karyawan pada tabel. |
| #352  | Sinkronisasi Socket.IO Multi-Instance (Redis Adapter), Ring Buffer RADIUS di Redis, Presensi WhatsApp & Print Station Lintas Server | Menjamin event real-time (QR Code WA, perintah cetak barcode, snapshot RADIUS, presence obrolan) tersinkronisasi sempurna di semua pod/instance backend, mengeliminasi false disconnect pada printer dan salah auto-reply. |
| #345  | Standarisasi Skema Periode Tagihan Multi-Bulan & Deteksi Overlapping Invoice | Mendukung skema periode tagihan fleksibel (kalender vs anniversary aktivasi) untuk paket langganan multi-bulan serta mengeliminasi risiko penagihan ganda dengan deteksi tumpang-tindih periode. |
| #175  | Finalisasi Merge Modul Monitoring & Remote Config ONT TR-069 (ACS) ke Master | Mengintegrasikan seluruh arsitektur remote management modem pelanggan (CWMP/TR-069) ke cabang produksi utama master secara resmi. |

### Kemampuan Baru Pengguna/Admin

- **Pemetaan Notifikasi Tiket Spesifik di Telegram**: Admin dapat mengatur agar tiket pekerjaan Backbone, Backhaul, dan Bisnis terdistribusi otomatis ke sub-topik obrolan Telegram masing-masing divisi teknis.
- **Konfigurasi Skema Penagihan Fleksibel**: Bagian keuangan dapat memilih apakah tagihan otomatis dihitung mengikuti tanggal kalender atau murni N bulan penuh sejak tanggal aktivasi modem pelanggan.
- **Keandalan Cetak Barcode & Print Station**: Staf gudang dapat mencetak barcode barang kapan saja tanpa khawatir muncul kendala "Printer tidak terhubung" akibat perbedaan server penampung socket.
- **Interaksi WhatsApp Lebih Stabil & Aman**: Sesi pairing WhatsApp menampilkan status error dan tombol "Coba Lagi" yang informatif saat jaringan bermasalah, serta melindungi nomor dari pemblokiran massal oleh Meta.
- **Pencegahan Duplikasi Jadwal Kerja Tanpa Crash**: Pembuatan jadwal kerja dengan tanggal atau nama yang telah ada akan memberikan peringatan konflik yang jelas tanpa membuat aplikasi mengalami error sistem.

### Bug Fix / Solusi Masalah

- **Event Socket.IO Hilang pada Backend Multi-Instance**: Pemasangan `@socket.io/redis-adapter` memastikan seluruh broadcast Socket.IO menjangkau klien terlepas dari instance backend mana yang memproses request.
- **Snapshot RADIUS Kosong**: Memindahkan ring buffer trace dan status kesehatan server RADIUS ke Redis menjamin endpoint REST selalu membaca data terbaru dari semua instance gRPC.
- **Auto-Reply WhatsApp Salah Terpicu**: Menggunakan pembacaan room presence Socket.IO cluster mencegah bot membalas obrolan pelanggan saat admin sedang aktif membuka tab obrolan di server lain.
- **Looping Reconnect Baileys Tanpa Akhir**: Membatasi percobaan koneksi maksimal 5 kali pada akun yang belum pernah berhasil pairing guna menghindari ban akun WhatsApp.
- **Pergeseran Tanggal Input dari DatePicker**: Modul `date-input.js` mengurai format string ISO-8601 dan format lokal secara ketat, menghilangkan salah tafsir tanggal akibat offset zona waktu.
- **Pelanggaran Rules of Hooks pada Tabel**: Memindahkan hooks `useHasPrivilege` ke level atas komponen `AdminLink` dan memperbaiki tautan avatar karyawan ke `/file/admin-avatar/${id}`.
- **Tagihan Ganda pada Paket Multi-Bulan**: Helper `hasInvoiceOverlappingPeriod` memastikan tidak ada invoice yang terbit berulang untuk rentang masa aktif layanan yang sama.

### Menu/Fitur Baru

- **Pengaturan Skema Periode Tagihan**: Menu baru di `Pengaturan > Keuangan` untuk mengatur mode kalkulasi durasi tagihan langganan broadband secara global.
- **Peringatan Perubahan Siklus Langganan**: Kotak peringatan dinamis pada form edit broadband saat admin mengubah mode atau durasi siklus tagihan pelanggan aktif.
- **Tombol Coba Lagi Pairing WhatsApp**: Penanganan status gagal dan tombol retry pada modal penyambungan akun WhatsApp Baileys.
- **Pilihan Topik Tiket Bisnis, Backbone, & Backhaul**: Opsi penugasan topik baru pada menu `Pengaturan > Notifikasi Telegram > Kelola Grup & Topik`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Skema Periode Tagihan Multi-Bulan & Pengaturan Periode Fleksibel**
  Fitur ini memungkinkan ISP atau penyedia layanan internet mengelola pelanggan dengan paket langganan multi-bulan (contoh: paket 3 bulan, 6 bulan, atau 1 tahun) secara otomatis dan rapi. Sistem membedakan antara kapan faktur harus terbit (awal bulan atau tanggal aktivasi) dengan periode masa aktif yang ditagihkan. Pada skema *Anniversary Period*, jika pelanggan aktif pada tanggal 15 Januari untuk paket 3 bulan, tagihan akan mencakup periode 15 Januari s/d 14 April secara akurat, dan sistem mencegah terbitnya faktur kedua sebelum masa layanan tersebut berakhir.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan > Keuangan** (`/settings` bagian Keuangan).
  2. Gulir ke bagian **Skema Periode Tagihan**.
  3. Pilih salah satu skema:
     - **Ikuti opsi Tagihan Otomatis**: Periode tagihan mengikuti mode terbit kalender (tanggal 1 s/d akhir bulan) atau tanggal aktivasi.
     - **Periode dari tanggal aktivasi**: Periode tagihan selalu dihitung murni N bulan penuh sejak tanggal modem pelanggan diaktifkan, meskipun tagihan terbit lebih awal di awal bulan.
  4. Klik tombol **Simpan**. Sistem akan menampilkan konfirmasi perubahan skema.
  5. Buka menu **Layanan > Broadband** (`/services/broadband`), buat atau edit langganan pelanggan, lalu pilih durasi siklus pada paket internet atau override siklus tagihan khusus (misal 3 atau 12 bulan).
  6. Sistem penagihan otomatis (cron auto-invoice) akan secara berkala mengevaluasi jendela periode layanan dan hanya menerbitkan tagihan pada kelipatan siklus yang tepat tanpa resiko tagihan ganda.
