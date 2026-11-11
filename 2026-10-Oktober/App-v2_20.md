# 📝 Daily Work Report - Dedy S.N Putra (20 Oktober 2026)

---

## 📅 Laporan Harian - 20 Oktober 2026

---

## 🌿 Branch: `issue-448` — Issue #448: Perbaikan Transfer Antar Akun Bank dan Kas

### 📌 Informasi Issue

- **Nomor Issue**: #448
- **Judul Issue**: Perbaikan transfer antar akun bank dan kas
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Selasa, 20 Oktober 2026, 19:50:01 WIB via commit `6ecf659b`, di-merge ke branch `production` via commit `d601ae79`, dan issue GitHub #448 resmi ditutup / `CLOSED`)

### 📅 Rincian Commit

#### [d601ae79] - Merge branch 'master' into production - Selasa, 20 Oktober 2026, 19:51:36 WIB

_(Merge commit menyinkronkan branch `master` terbaru yang memuat perbaikan Issue #448 langsung ke branch `production`)_

---

#### [6ecf659b] - resolve #448 - Selasa, 20 Oktober 2026, 19:50:01 WIB

_(Merge commit menggabungkan commit penyelesaian `9b1b3410` ke branch `master`)_

---

#### [9b1b3410] - resolve #448 - Selasa, 20 Oktober 2026, 19:49:19 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/financeAccount.controller.js`
  - `backend/src/controllers/financeTransfer.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/financeAccount.route.js`
  - `backend/src/services/financeTransfer.service.js`
  - `backend/test/integration/financeAccount.optionsPrivilege.test.js` [NEW]
  - `backend/test/integration/financeTransfer.approval.test.js`
  - `frontend/src/app/pages/finance/transfers/TransferDrawer.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `partner-frontend/src/app/pages/finance/transfers/TransferDrawer.jsx`
  - `partner-frontend/src/components/shared/form/FormInput.jsx`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/transport/pppoe/auth_listener.go`
  - `radius-server/internal/transport/pppoe/auth_listener_test.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Pencegahan Penarikan Kas Ilegal Antar Admin (`financeTransfer.service.js`)**:
    - Menambahkan validasi kepemilikan kas personal pada pembuatan transfer: admin biasa (`isPrivilegedUser === false`) dilarang keras mentransfer dari akun kas personal milik admin lain (`accountFrom.owner_admin !== user`).
    - Jika dilanggar, sistem menolak transaksi dengan kode error `FINANCE_ERROR.ACCOUNT_NOT_ALLOWED` dan pesan ramah `finance.transfer.fromAccountNotAllowed` ("Anda tidak memiliki hak untuk mentransfer dari kas admin lain").
  - **Aturan Persetujuan (Approval) Transfer Akun Kantor vs Kas Pribadi**:
    - Menata ulang syarat transfer yang membutuhkan persetujuan (*pending approval*):
      1. Transfer dari kas personal admin (`owner_admin != null`) selalu membutuhkan approval.
      2. Transfer dari akun kantor (`owner_admin == null`) yang diajukan oleh admin non-privileged (`isPrivilegedUser === false`) otomatis dialihkan ke status `pending_approval`.
      3. Transfer dari akun kantor yang diajukan oleh admin berwenang kelola akun kantor (`isPrivilegedUser === true`) langsung berstatus `completed` tanpa perlu approval ganda.
  - **Scope Akses Opsi Akun Keuangan (`financeAccount.route.js`, `financeAccount.controller.js`)**:
    - Menambahkan hak akses `financeTransfer.create` dan `financeTransfer.list` ke route `GET /finance/account/options` agar staf yang bertugas membuat transfer dapat melihat opsi akun tanpa harus memiliki wewenang penuh `financeAccount.list`.
    - Mendukung parameter query `scope=transfer`: memungkinkan penyajian akun bank kantor dan seluruh kas operasional khusus untuk formulir transfer kas.
  - **Dukungan Scope pada Formulir Frontend & Partner-Frontend (`TransferDrawer.jsx`, `FormInput.jsx`)**:
    - Menambahkan properti `scope="transfer"` pada komponen `InputFinanceAccountSelect` di drawer formulir transfer kas Dekasimal utama dan antarmuka mitra (*partner-frontend*).
  - **Penyesuaian PPPoE Static IP saat Isolir/Blokir pada Radius Server (`auth_listener.go`, `auth_listener_test.go`)**:
    - Memperbaiki pengiriman atribut RADIUS `Framed-IP-Address` pada saat akun PPPoE berstatus isolir (`IsolirApplied`) atau blokir (`BlockedApplied`): IP statis **tidak dikirimkan** ke NAS MikroTik.
    - RouterOS memprioritaskan `Framed-IP-Address` di atas IP pool profil PPP; jika IP statis tetap dikirim saat drop/block profile, pelanggan tetap memegang IP statis publik/lokal lamanya dan lolos dari firewall captive portal/drop isolir. Dengan ditiadakannya atribut ini saat isolir, NAS dipaksa memberikan IP dari remote-address pool isolir (misal `10.x.x.x`).

---

## 🌿 Branch: `issue-444` — Issue #444: Penataan dan Pemetaan Kunci Antar Microservice (Service Keys Management & Architecture)

### 📌 Informasi Issue

- **Nomor Issue**: #444
- **Judul Issue**: Penataan dan Pemetaan kunci antar microservice
- **Status Branch**: `Belum di-merge` (Branch kerja aktif `issue-444`, ahead 26 commit dari `origin/issue-444` dan `master`; seluruh rangkaian backend, pustaka bersama, integrasi microservice, dan suite unit/integration test telah selesai diimplementasikan)

### 📅 Rincian Commit

#### [45e295b4] - save #444: Network Monitor service keys loader & route probe key check - Selasa, 20 Oktober 2026, 18:37:19 WIB

- **Komponen yang Berubah**:
  - `network-monitor/package-lock.json`
  - `network-monitor/package.json`
  - `network-monitor/src/middlewares/auth.middleware.js`
  - `network-monitor/src/middlewares/requireApiKey.factory.js` [NEW]
  - `network-monitor/src/routes/probe.route.js`
  - `network-monitor/src/server.js`
  - `network-monitor/src/utils/logger.js`
  - `network-monitor/src/utils/serviceKeys.js` [NEW]
  - `network-monitor/test/requireApiKey.factory.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Service Keys Loader pada Network Monitor (`network-monitor/src/utils/serviceKeys.js`)**:
    - Mengintegrasikan loader kunci dinamis yang otomatis menyinkronkan `internal_api_key` dari MongoDB `system_settings` setiap 60 detik dengan fallback aman ke `process.env.INTERNAL_API_KEY`.
  - **Middleware Pabrik `requireApiKey.factory.js` & Endpoint Status Kunci**:
    - Membangun pabrik middleware autentikasi yang memeriksa header `x-api-key` menggunakan perbandingan `timingSafeEqual`.
    - Menambahkan rute `GET /probe/key-check` yang dilindungi API key untuk memverifikasi kecocokan kunci dari backend Dekasimal.

---

#### [50beebf7] - save #444: Telegram API service keys loader & route key check - Selasa, 20 Oktober 2026, 18:34:02 WIB

- **Komponen yang Berubah**:
  - `telegram-api/package-lock.json`
  - `telegram-api/package.json`
  - `telegram-api/src/authenticateAPI.factory.js` [NEW]
  - `telegram-api/src/botManager.js`
  - `telegram-api/src/middleware.js`
  - `telegram-api/src/routes.js`
  - `telegram-api/src/server.js`
  - `telegram-api/src/utils/logger.js`
  - `telegram-api/src/utils/serviceKeys.js` [NEW]
  - `telegram-api/test/authenticateAPI.factory.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Integrasi Service Keys pada Layanan Telegram API**:
    - Mengimplementasikan loader `serviceKeys.js` di `telegram-api` untuk memuat `telegram_bot_api_key` langsung dari database MongoDB.
    - Mengganti middleware autentikasi lama dengan `authenticateAPI.factory.js` yang mendukung pengecekan masa tenggang (*grace period*).
    - Menambahkan rute `GET /key-check` untuk health check status kunci layanan.

---

#### [fea4edd4] - save #444: Baileys API authenticateInternal factory & service keys - Selasa, 20 Oktober 2026, 18:31:05 WIB

- **Komponen yang Berubah**:
  - `baileys-api/src/middleware/authenticateInternal.factory.js` [NEW]
  - `baileys-api/src/middleware/authenticateInternal.js`
  - `baileys-api/src/routes/internal.route.js`
  - `baileys-api/src/server.js`
  - `baileys-api/src/services/backendBridge.service.js`
  - `baileys-api/src/utils/logger.js`
  - `baileys-api/src/utils/serviceKeys.js` [NEW]
  - `baileys-api/test/authenticateInternal.factory.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Autentikasi Internal & Jembatan Backend Baileys API**:
    - Menghubungkan `baileys-api` ke modul `serviceKeys.js` untuk membaca `baileys_api_client_key` secara dinamis.
    - Memperbarui `backendBridge.service.js` agar selalu menyertakan header `x-service-name: baileys-api` pada setiap panggilan ke backend Dekasimal.
    - Menambahkan endpoint `GET /internal/key-check` dan pengujian unit factory autentikasi internal.

---

#### [7303e1bd] - save #444: WhatsApp API auth factories & internal service keys - Selasa, 20 Oktober 2026, 18:28:07 WIB

- **Komponen yang Berubah**:
  - `whatsapp-api/package.json`
  - `whatsapp-api/src/controllers/internal.controller.js`
  - `whatsapp-api/src/middleware/authFactories.js` [NEW]
  - `whatsapp-api/src/middleware/authenticateAPI.js`
  - `whatsapp-api/src/middleware/authenticateInternal.js`
  - `whatsapp-api/src/routes/internal.route.js`
  - `whatsapp-api/src/server.js`
  - `whatsapp-api/src/services/backendBridge.service.js`
  - `whatsapp-api/src/utils/logger.js`
  - `whatsapp-api/src/utils/serviceKeys.js` [NEW]
  - `whatsapp-api/test/authFactories.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pembaruan Lapisan Autentikasi WhatsApp API (Go / Node Gateway)**:
    - Membangun `authFactories.js` untuk memvalidasi `client_api_key` dan `internal_api_key` secara timing-safe.
    - Memasang loader `serviceKeys.js` dengan penyegaran berkala dari MongoDB.
    - Menambahkan rute `GET /internal/key-check` dan pengujian unit `authFactories.test.js`.

---

#### [a4dffc3d] - save #444: Cron Worker apiKeyAuth factory refinement & tests - Selasa, 20 Oktober 2026, 18:25:46 WIB

- **Komponen yang Berubah**:
  - `cron-worker/src/middlewares/apiKeyAuth.factory.js`
  - `cron-worker/src/utils/serviceKeys.js`
  - `cron-worker/test/apiKeyAuth.factory.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Penyempurnaan penanganan header `x-api-key` case-insensitive dan penambahan suite test integrasi pada `cron-worker`.

---

#### [a92d1bdb] - save #444: Cron Worker service keys loader & route cron key check - Selasa, 20 Oktober 2026, 18:23:19 WIB

- **Komponen yang Berubah**:
  - `cron-worker/package.json`
  - `cron-worker/src/app.js`
  - `cron-worker/src/config/env.js`
  - `cron-worker/src/controllers/cron.controller.js`
  - `cron-worker/src/middlewares/apiKeyAuth.factory.js` [NEW]
  - `cron-worker/src/middlewares/auth.middleware.js`
  - `cron-worker/src/routes/cron.routes.js`
  - `cron-worker/src/services/api.service.js`
  - `cron-worker/src/utils/serviceKeys.js` [NEW]
  - `cron-worker/test/apiKeyAuth.factory.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Dukungan Service Keys Dinamis pada Cron Worker**:
    - Mengintegrasikan loader `serviceKeys.js` di `cron-worker` untuk membaca `internal_api_key` terkini tanpa perlu restart container.
    - Memperbarui `api.service.js` agar menyertakan header `x-service-name: cron-worker` pada seluruh panggilan REST ke backend Dekasimal.
    - Menambahkan endpoint `GET /cron/key-check`.

---

#### [f559ebcb] - save #444: Tool sinkronisasi pustaka core service keys lintas microservice - Selasa, 20 Oktober 2026, 18:20:27 WIB

- **Komponen yang Berubah**:
  - `baileys-api/src/utils/serviceKeys.core.js`
  - `cron-worker/src/utils/serviceKeys.core.js`
  - `network-monitor/src/utils/serviceKeys.core.js`
  - `syslog-server/src/utils/serviceKeys.core.js`
  - `telegram-api/src/utils/serviceKeys.core.js`
  - `tools/service-keys/serviceKeys.core.js`
  - `tools/service-keys/serviceKeys.core.test.js`
  - `tools/service-keys/sync.mjs`
  - `whatsapp-api/src/utils/serviceKeys.core.js`
- **Deskripsi Perubahan & Fungsi**:
  - Peningkatan pustaka `serviceKeys.core.js` untuk penanganan fallbacks nilai kosong, parsing proyeksi Mongo, serta penambahan unit test verifikasi sinkronisasi antar microservice via `tools/service-keys/sync.mjs`.

---

#### [dccccd82] - save #444: Pustaka zero-dependency serviceKeys.core, mongoReader & sync script - Selasa, 20 Oktober 2026, 18:17:26 WIB

- **Komponen yang Berubah**:
  - `baileys-api/src/utils/serviceKeys.core.js` [NEW]
  - `cron-worker/src/utils/serviceKeys.core.js` [NEW]
  - `network-monitor/src/utils/serviceKeys.core.js` [NEW]
  - `network-monitor/src/utils/serviceKeys.mongoReader.js` [NEW]
  - `package.json`
  - `syslog-server/src/utils/serviceKeys.core.js` [NEW]
  - `telegram-api/src/utils/serviceKeys.core.js` [NEW]
  - `telegram-api/src/utils/serviceKeys.mongoReader.js` [NEW]
  - `tools/service-keys/package.json` [NEW]
  - `tools/service-keys/serviceKeys.core.js` [NEW]
  - `tools/service-keys/serviceKeys.core.test.js` [NEW]
  - `tools/service-keys/serviceKeys.mongoReader.js` [NEW]
  - `tools/service-keys/serviceKeys.mongoReader.test.js` [NEW]
  - `tools/service-keys/sync.mjs` [NEW]
  - `tools/service-keys/sync.test.js` [NEW]
  - `whatsapp-api/src/utils/serviceKeys.core.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pembangunan Pustaka Inti Kunci Layanan Bersama (`tools/service-keys/`)**:
    - Menciptakan modul murni *zero-dependency* `serviceKeys.core.js` yang menangani logika pencocokan kunci timing-safe, pengelolaan masa tenggang, dan cache nilai.
    - Menciptakan `serviceKeys.mongoReader.js` untuk membaca konfigurasi `system_settings` dan `service_key_rotation` langsung dari database MongoDB dengan query proyeksi minimal.
    - Membangun skrip `sync.mjs` dan script npm `npm run sync:service-keys` pada root repo untuk menyebarkan pustaka identik ke seluruh microservice monorepo secara otomatis.

---

#### [cfa73207] - save #444: Penyesuaian networkDevice & tes integrasi senders - Selasa, 20 Oktober 2026, 17:39:34 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/services/networkDevice.service.js`
  - `backend/test/integration/serviceKeys.auth.test.js`
  - `backend/test/integration/serviceKeys.controller.test.js`
  - `backend/test/integration/serviceKeys.list.test.js`
  - `backend/test/integration/serviceKeys.senders.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui integrasi pemanggilan ke `network-monitor` pada `networkDevice.service.js` agar menggunakan kunci layanan yang dinamis.
  - Memperkuat test integrasi untuk seluruh skenario pengiriman kunci antar-layanan (*senders*).

---

#### [85dcc8cd] - save #444: Telegram webhook sighting & rotasi kunci Telegram bot API - Selasa, 20 Oktober 2026, 17:39:34 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/serviceKeys.service.js`
  - `backend/src/services/settingsSideEffects.service.js`
  - `backend/test/integration/serviceKeys.match.test.js`
  - `backend/test/integration/serviceKeys.rotate.test.js`
  - `backend/test/integration/serviceKeys.telegramWebhook.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pencatatan Sighting pada Rute Webhook Telegram (`public.route.js`)**:
    - Memperbarui handler webhook Telegram `/telegram-webhook` agar memvalidasi token via `matchServiceKey('telegram_bot_api_key')`.
    - Mencatat status pengamatan (*sighting*): panggilan berhasil dicatat sebagai `telegram-api`, sedangkan penolakan dicatat sebagai `unknown` karena rute bersifat publik.

---

#### [f5bfeb6b] - save #444: Sanitasi body logging zero-secrets untuk rute service keys - Selasa, 20 Oktober 2026, 17:39:34 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/services/option.service.js`
  - `backend/test/integration/serviceKeys.settings.test.js`
  - `backend/test/unit/loggerSanitizer.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Jaminan Nol Kredensial pada Log Server (`logger.middleware.js`, `BODY_REDACTED_PREFIXES`)**:
    - Menambahkan prefix `/service-key/rotate` dan `/service-key/reveal` ke dalam daftar `BODY_REDACTED_PREFIXES` pada middleware logging Winston.
    - Memastikan payload body yang memuat string kunci rahasia mentah otomatis diredaksi menjadi `[REDACTED]` pada access log.

---

#### [1e1f1d81] - save #444: Sinkronisasi aturan arsitektur kunci layanan di AGENTS.md - Selasa, 20 Oktober 2026, 17:17:30 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui panduan baku monorepo di `AGENTS.md` terkait batasan panjang karakter rotasi kunci (24–256 karakter ASCII tercetak).

---

#### [d2bba7b7] - save #444: Dokumentasi arsitektur aturan #29 di AGENTS.md - Selasa, 20 Oktober 2026, 17:13:46 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/test/integration/serviceKeys.list.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menuliskan klausul aturan arsitektur baku monorepo Bab 2.A Aturan 29 mengenai standarisasi Kunci Antar-Layanan (*Service Keys*), format catalog, grace period 24 jam, dan audit pembukaan kunci.

---

#### [b277bd4b] - save #444: Controller & router serviceKey di Backend - Selasa, 20 Oktober 2026, 17:03:02 WIB

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/controllers/serviceKey.controller.js` [NEW]
  - `backend/src/routes/serviceKey.route.js` [NEW]
  - `backend/test/integration/serviceKeys.controller.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Membangun REST API Endpoint Kunci Layanan (`serviceKey.route.js`, `serviceKey.controller.js`)**:
    - Menyediakan endpoint lengkap di bawah prefix `/service-key` yang dilindungi hak akses `protectedDeveloper`:
      - `GET /service-key/list`: Mengembalikan katalog seluruh kunci, pemakai, status pengamatan (*sightings*), dan status tenggang rotasi.
      - `PATCH /service-key/rotate`: Merotasi nilai kunci ke nilai baru dengan masa tenggang 24 jam (atau opsional pemutusan langsung jika `revoke_previous: true`).
      - `POST /service-key/end-grace`: Mencabut seketika kunci lama yang masih dalam masa tenggang.
      - `POST /service-key/reveal`: Menampilkan nilai kunci rahasia mentah dengan pencatatan audit log tanpa nilai kuncinya.
      - `POST /service-key/check`: Menguji koneksi langsung ke seluruh endpoint `/key-check` microservice yang dituju.

---

#### [9d8c046e] - save #444: Pengujian status check service keys - Selasa, 20 Oktober 2026, 17:00:49 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/serviceKeys.service.js`
  - `backend/test/integration/serviceKeys.check.test.js`
  - `backend/test/integration/serviceKeys.list.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Penyempurnaan mapping status hasil pengecekan konektivitas microservice (`ok`, `previous`, `rejected`, `unreachable`, `unsupported`, `not_configured`, `error`).

---

#### [8242f194] - save #444: Service check & list status microservice penerima - Selasa, 20 Oktober 2026, 16:58:39 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/serviceKeys.service.js`
  - `backend/test/integration/serviceKeys.check.test.js` [NEW]
  - `backend/test/integration/serviceKeys.list.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan fungsi `checkServiceKeys` menggunakan klien axios dengan `maxRedirects: 0` agar API key tidak bocor lintas host saat pengujian.
  - Mengimplementasikan fungsi `listServiceKeys` yang menggabungkan registri statis dengan pengamatan live di database `service_key_sightings`.

---

#### [a5726d7a] - save #444: Resolver resolveServiceKey dinamis pada pemanggil Baileys, Telegram & Network - Selasa, 20 Oktober 2026, 16:55:40 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/baileysControl.service.js`
  - `backend/src/services/baileysSender.service.js`
  - `backend/src/services/networkDevice.service.js`
  - `backend/src/services/networkTraffic.service.js`
  - `backend/src/utils/telegram.js`
  - `backend/test/integration/serviceKeys.senders.test.js` [NEW]
  - `backend/test/unit/ticketTelegramClose.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Mengganti seluruh pembacaan statis `process.env.*` menjadi fungsi resolver dinamis `resolveServiceKey(id)` yang memprioritaskan database Settings dengan fallback ke environment.
  - Memastikan klien axios tidak meng-cache header kunci saat inisialisasi awal, melainkan menyematkannya per request via interceptor.

---

#### [c92cf5f6] - save #444: Middleware internalApiKeyAuth validasi masa tenggang & pencatatan sighting - Selasa, 20 Oktober 2026, 16:49:33 WIB

- **Komponen yang Berubah**:
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/routes/public.route.js`
  - `backend/test/integration/serviceKeys.auth.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui middleware `internalApiKeyAuth` agar memverifikasi kunci via `matchServiceKey('internal_api_key', presentedKey)`.
  - Menerima kunci terkini maupun kunci lama yang masih dalam jendela toleransi 24 jam.
  - Membaca header `x-service-name` untuk mencatat identitas microservice pemanggil ke koleksi database `service_key_sightings`.

---

#### [b1e5e91a] - save #444: Logika rotasi kunci rotateServiceKey dengan grace period 24 jam - Selasa, 20 Oktober 2026, 16:46:06 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/services/serviceKeys.service.js`
  - `backend/src/services/settingsSideEffects.service.js`
  - `backend/test/integration/serviceKeys.rotate.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan fungsi `rotateServiceKey`: menyimpan nilai lama ke dokumen `service_key_rotation` dengan masa tenggang `previous_until = now + 24 jam` sebelum menulis nilai baru ke `system_settings`.
  - Mencegah downtime atau 401 massal pada microservice saat admin memperbarui API key di antarmuka pengaturan.

---

#### [05503fd7] - save #444: Pencatatan pengamatan (sighting) inter-service dengan debouncing memori - Selasa, 20 Oktober 2026, 16:42:50 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/serviceKeys.service.js`
  - `backend/test/integration/serviceKeys.sighting.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun mekanisme pencatatan pengamatan microservice dengan peredam frekuensi (*in-memory debounce* 30 detik per kombinasi layanan/state).
  - Menghindari lonjakan penulisan ke database MongoDB akibat ribuan panggilan periodik dari cron-worker atau ACS.

---

#### [ce89f9bd] & [055b5279] - save #444: Verifikasi matchServiceKey timing-safe comparison - Selasa, 20 Oktober 2026, 16:38 - 16:40 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/serviceKeys.service.js`
  - `backend/test/integration/serviceKeys.match.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan `matchServiceKey`: verifikasi kunci rahasia menggunakan hashing SHA-256 dan perbandingan waktu-konstan `crypto.timingSafeEqual` untuk mencegah serangan *timing attack*.

---

#### [70950e04] - save #444: Model Mongoose ServiceKeySighting dengan TTL 30 hari - Selasa, 20 Oktober 2026, 16:36:03 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/serviceKeySighting.model.js` [NEW]
  - `backend/test/integration/serviceKeys.model.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membuat model Mongoose `ServiceKeySighting` untuk merekam jejak interaksi antar microservice (`service`, `key_id`, `state`, `last_seen_at`, `last_rejected_at`, `rejected_since`).
  - Menetapkan TTL index 30 hari pada field `updated_at` untuk pembersihan riwayat otomatis.

---

#### [1eb951a6] - save #444: Registri backup pengaturan & sanitasi field system_service_keys - Selasa, 20 Oktober 2026, 16:34:03 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/option.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/sanitize.js`
  - `backend/test/integration/serviceKeys.settings.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Mendaftarkan dua kunci baru (`baileys_api_client_key` dan `telegram_bot_api_key`) ke grup backup `system_service_keys` dan menetapkannya sebagai field sensitif `secretKeys`.

---

#### [d2eceaad] - save #444: Registri statis katalog kunci layanan serviceKeys.registry.js - Selasa, 20 Oktober 2026, 16:31:46 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/serviceKeys.registry.js` [NEW]
  - `backend/test/unit/serviceKeys.registry.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menetapkan satu sumber kebenaran katalog seluruh kunci antar-layanan monorepo Dekasimal: `internal_api_key`, `client_api_key`, `baileys_api_client_key`, `telegram_bot_api_key`, dan `grpc_auth_token`.

---

#### [c9418521] - resolve #444: Mekanisme webhook retry Baileys API & counter cap RRD Network Monitor - Selasa, 20 Oktober 2026, 16:10:15 WIB

- **Komponen yang Berubah**:
  - `baileys-api/.env.example`
  - `baileys-api/package-lock.json`
  - `baileys-api/package.json`
  - `baileys-api/src/config.js`
  - `baileys-api/src/server.js`
  - `baileys-api/src/services/backendBridge.service.js`
  - `baileys-api/src/services/sessionManager.service.js`
  - `baileys-api/src/services/webhookRetry.core.js` [NEW]
  - `baileys-api/src/services/webhookRetry.service.js` [NEW]
  - `baileys-api/src/utils/forwardError.js` [NEW]
  - `baileys-api/test/webhookRetry.test.js` [NEW]
  - `network-monitor/src/services/rrd.service.js`
  - `network-monitor/src/utils/rrdValue.js` [NEW]
  - `network-monitor/test/rrdCounterCap.integration.test.js` [NEW]
  - `network-monitor/test/rrdValue.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Antrean Percobaan Ulang Webhook Baileys API (`webhookRetry.service.js`)**:
    - Membangun sistem antrean retry in-memory yang tangguh pada `baileys-api` ketika backend mengembalikan error (misal respon 401 saat kunci tidak cocok atau 500 saat backend restart), mencegah pesan WhatsApp masuk hilang tanpa jejak.
  - **Pencegahan Spike Trafik Jaringan RRD (`rrdValue.js`, `rrd.service.js`)**:
    - Membangun utilitas `sanitizeRrdRate` untuk menangani lonjakan anomali trafik pada database RRD saat counter byte SNMP 32-bit / 64-bit perangkat jaringan melakukan reset atau wrap-around.

---

## 🌿 Branch: `issue-437` — Issue #437: Tambahan & Bug Apps

### 📌 Informasi Issue

- **Nomor Issue**: #437
- **Judul Issue**: Tambahan & Bug Apps
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Selasa, 20 Oktober 2026, 14:28:08 WIB via commit `feb17e72`)

### 📅 Rincian Commit

#### [feb17e72] - resolve #437 - Selasa, 20 Oktober 2026, 14:28:08 WIB

_(Merge commit menggabungkan branch `issue-437` ke branch `master`)_

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/privilege.controller.js`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/controllers/ticketQuickMessage.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/privilege.model.js`
  - `backend/src/models/ticketQuickMessage.model.js` [NEW]
  - `backend/src/routes/ticket.route.js`
  - `backend/src/services/privilege.service.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/services/ticketQuickMessage.service.js` [NEW]
  - `backend/src/utils/validation-data.js`
  - `backend/test/integration/privilegeNameCaseInsensitive.test.js` [NEW]
  - `backend/test/integration/ticketAddMessagePending.test.js` [NEW]
  - `backend/test/integration/ticketQuickMessage.test.js` [NEW]
  - `backend/test/unit/capitalizeWords.test.js` [NEW]
  - `backend/test/unit/ticketMobile.service.test.js`
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx`
  - `frontend/src/app/pages/tickets/components/PendingActionModal.jsx` [NEW]
  - `frontend/src/app/pages/tickets/utils/pendingMessage.js` [NEW]
  - `frontend/src/app/pages/tickets/utils/pendingMessage.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `telegram-apps/src/pages/tickets/TicketList.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Status Pending Tiket Mitra & Bisnis dari Pesan Kronologi (`PendingActionModal.jsx`, `MessageUpdate.jsx`)**:
    - Memisahkan aksi penundaan status tiket (*pending ticket*) dari kolom komentar kronologi biasa agar pencatatan alasan pending tersimpan secara eksplisit dan terstruktur.
  - **Pesan Cepat Balasan Tiket (*Ticket Quick Message*)**:
    - Membangun model, service, controller, dan route untuk mengelola template pesan cepat tiket (`ticketQuickMessage.model.js`, `ticketQuickMessage.service.js`) sehingga agen customer service dapat membalas tiket lebih cepat.
  - **Pencarian Nama Pelanggan pada Seluruh Menu Pilih Tiket TMA (`TicketList.jsx`)**:
    - Mengintegrasikan fitur pencarian berdasarkan nama pelanggan pada Telegram Mini App (TMA) saat teknisi memilih tiket penanganan di lapangan.
  - **Perbaikan Huruf Kapital Konversi & Edit Tiket (`validation-data.js`, `capitalizeWords`)**:
    - Memperbaiki bug validasi nama pelanggan saat konversi tiket prospek ke pelanggan resmi, memastikan konsistensi huruf kapital sesuai dengan input pengguna tanpa memicu error validasi nama.

---

## 🌿 Branch: `issue-442` — Issue #442: Fix Bug - Status Pembayaran pada Radius Authentication

### 📌 Informasi Issue

- **Nomor Issue**: #442
- **Judul Issue**: Fix Bug - Status pembayaran pada radius authentication
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Selasa, 20 Oktober 2026, 14:26:12 WIB via commit `808a0b46`, di-merge ke branch `production` via commit `e05acf4e`, dan issue GitHub #442 resmi ditutup / `CLOSED`)

### 📅 Rincian Commit

#### [e05acf4e] - Merge branch 'master' into production - Selasa, 20 Oktober 2026, 14:26:35 WIB

_(Merge commit menyinkronkan branch `master` terbaru ke branch `production`)_

---

#### [808a0b46] - resolve #442 - Selasa, 20 Oktober 2026, 14:26:12 WIB

_(Merge commit menggabungkan commit penyelesaian `9064a159` ke branch `master`)_

---

#### [9064a159] - resolve #442 - Selasa, 20 Oktober 2026, 14:25:35 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/models/plugins/waitPaymentReset.plugin.js` [NEW]
  - `backend/src/models/radiusAuthentication.model.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/services/invoiceFreeze.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/invoiceFreeze.test.js`
  - `backend/test/integration/waitPaymentRecovery.test.js` [NEW]
  - `backend/test/integration/waitPaymentReset.plugin.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `partner-frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/domain/accounting/handlers_test.go`
  - `radius-server/internal/domain/auth/authenticate_test.go`
  - `radius-server/internal/domain/isolir/restriction.go`
  - `radius-server/internal/domain/isolir/restriction_test.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Pemberesan Logika Isolir `wait_payment` pada Radius Server (`restriction.go`, `authenticate_test.go`)**:
    - Memperbaiki penentuan pembatasan akun: `wait_payment` bukan merupakan pembatas mandiri, melainkan label alasan bagi akun yang berstatus nonaktif (`status: false`).
    - Akun aktif (`status: true`) yang secara tidak sengaja masih menyimpan penanda basi `wait_payment: true` (misal diaktifkan ulang lewat form manual lama) **tidak lagi dianggap terisolir** dan tidak lagi dilempar ke drop-profile MikroTik.
    - Sesi interim MikroTik pada akun aktif yang terlanjur berada di drop-profile otomatis dilepas (*released*) dan diputus agar menyambung kembali ke profil normal.
  - **Plugin Otomatisasi Pembersihan Penanda Isolir (`waitPaymentReset.plugin.js`)**:
    - Memasang Mongoose plugin pada model `RadiusAuthentication` yang otomatis menghapus penanda `wait_payment: false` dan `limit_reason: null` setiap kali akun disimpan dengan status aktif (`status: true`).
  - **Alat Pemulihan Pengembang (*Recovery Tool*) Akun Basi (`recovery.service.js`)**:
    - Menambahkan tool pemulihan `radiusStaleWaitPayment` di menu **Pengaturan → Developer → Recovery Tools** (`POST /broadband/recovery-wait-payment`) untuk memindai dan membersihkan penanda isolir tersangkut pada akun yang sudah aktif di database dengan dukungan mode pratinjau (*dry-run*).
  - **Perbaikan Tampilan Dokumen Invoice Lebar (`InvoiceDocument.jsx`)**:
    - Menerapkan `table-fixed` dan penataan lebar kolom berbasis persentase pada tabel rincian invoice frontend dan partner-frontend.
    - Memasang wrapping CSS `[overflow-wrap:anywhere] !whitespace-normal` pada kolom nama barang agar deskripsi item yang sangat panjang tidak memotong layout tabel atau meluap keluar dari halaman cetak invoice.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #448  | Perbaikan transfer antar akun bank dan kas | Memperketat otorisasi penarikan kas personal antar admin, menata alur persetujuan (*approval*) transfer akun kantor vs kas pribadi, memperluas akses opsi akun keuangan, serta memastikan IP statis PPPoE tidak dikirim saat isolir agar captive portal MikroTik berfungsi sempurna. |
| #444  | Penataan dan Pemetaan kunci antar microservice | Merancang dan mengimplementasikan arsitektur *Service Keys Management* terpusat lintas microservice monorepo dengan dukungan masa tenggang 24 jam (*zero 401 downtime*), pencatatan jejak interaksi (*sightings*), verifikasi timing-safe, pustaka core mandiri, serta tool sinkronisasi otomatis. |
| #437  | Tambahan & Bug Apps | Memisahkan modal aksi status pending pada tiket mitra & bisnis, menambahkan fitur pesan cepat (*quick message*) tiket, memperbaiki pencarian nama pelanggan pada menu pilih tiket TMA, serta menuntaskan bug kapitalisasi huruf pada konversi tiket ke pelanggan. |
| #442  | Fix Bug - Status pembayaran pada radius authentication | Mengoreksi pembatasan isolir akun RADIUS agar flag `wait_payment` basi pada akun aktif tidak mengurung pelanggan di drop-profile, menyediakan plugin pembersih otomatis, menghadirkan recovery tool di panel developer, serta merapikan tabel invoice agar tidak meluap saat dicetak. |

### Kemampuan Baru Pengguna/Admin

- **Kontrol Transfer Akun Kas Lebih Aman**: Admin kas/keuangan kini terlindungi dari penarikan saldo kas personal oleh staf lain tanpa wewenang; transfer dari akun bank kantor oleh admin umum secara transparan diarahkan ke antrean persetujuan (*approval*) atasan.
- **Pemeriksaan & Rotasi Kunci Microservice Tanpa Downtime**: Pengembang dan sysadmin kini memiliki API terpusat untuk memantau status kecocokan kunci antar microservice (`internal_api_key`, `client_api_key`, `baileys_api_client_key`, `telegram_bot_api_key`), merotasi kunci baru dengan jendela toleransi 24 jam tanpa menyebabkan penolakan 401 pada webhook/job latar, serta menguji konektivitas langsung via endpoint `/key-check`.
- **Fitur Pesan Cepat Tiket**: Petugas helpdesk/customer service dapat membalas tiket pelanggan lebih efisien menggunakan kumpulan pesan cepat yang telah disiapkan sebelumnya.
- **Pencarian Tiket TMA Lebih Fleksibel**: Teknisi di lapangan dapat langsung mencari tiket berdasarkan nama pelanggan langsung melalui antarmuka Telegram Mini App tanpa harus menghafal nomor tiket.
- **Recovery Tool Penanda Isolir Tersangkut**: Sysadmin dapat mendeteksi dan memulihkan akun pelanggan aktif yang terisolir akibat sisa data penagihan lama melalui antarmuka panel Developer Recovery Tools dengan aman via pratinjau (*dry-run*).

### Bug Fix / Solusi Masalah

- **Solusi Lolosnya Firewall Isolir pada Pelanggan IP Statis PPPoE**: Menuntaskan masalah di mana pelanggan PPPoE ber-IP statis yang diisolir tetap dapat mengakses internet normal karena RouterOS memprioritaskan `Framed-IP-Address` di atas IP pool profil isolir.
- **Solusi 401 Massal Akibat Rotasi API Key Antar Microservice**: Menghilangkan insiden kegagalan komunikasi internal di mana perubahan kunci di database memutus ribuan event dari Baileys API, Telegram API, Network Monitor, atau Syslog Server.
- **Solusi Terisolirnya Pelanggan Aktif yang Tidak Menunggak**: Memperbaiki logika domain isolir pada Radius Server sehingga flag `wait_payment` yang tersangkut tidak lagi memotong koneksi internet pelanggan yang status langganannya sudah aktif.
- **Solusi Kerusakan Layout Tabel Invoice**: Menghilangkan bug tampilan pada cetak faktur/invoice di mana nama barang yang panjang merusak struktur tabel dan keluar dari batas kertas.
- **Solusi Gagal Konversi Tiket Akibat Validasi Nama**: Menghilangkan kendala teknis saat staf mengubah tiket prospek menjadi pelanggan resmi yang dipicu oleh inkonsistensi huruf kapital pada nama pelanggan.

### Menu/Fitur Baru

- **Modul Service Keys Backend & REST API**: Controller, router, dan model `serviceKey` pada backend (`/service-key/list`, `/service-key/rotate`, `/service-key/end-grace`, `/service-key/reveal`, `/service-key/check`).
- **Pustaka Inti & Tool Sinkronisasi Kunci Layanan**: `tools/service-keys/` yang memuat pustaka *zero-dependency* `serviceKeys.core.js`, `serviceKeys.mongoReader.js`, dan skrip sinkronisasi lintas microservice `sync.mjs`.
- **Endpoint Status Kunci pada Seluruh Microservice**: Endpoint `/key-check` pada `network-monitor`, `telegram-api`, `baileys-api`, `whatsapp-api`, dan `cron-worker`.
- **Fitur Template Pesan Cepat Tiket**: CRUD pesan cepat balasan tiket pada modul helpdesk Dekasimal.
- **Recovery Tool RADIUS Stale Wait Payment**: Fitur pemulihan `radiusStaleWaitPayment` di menu Developer Recovery Tools.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Tata Kelola & Rotasi Kunci Layanan Antar Microservice (*Service Keys*)

- **Penjelasan Fitur**: Arsitektur pengelolaan kredensial API antar microservice terpusat di Dekasimal. Ketika sebuah kunci layanan (misal `internal_api_key` atau `baileys_api_client_key`) diperbarui, backend mencatat kunci lama ke dalam masa tenggang (*grace period*) selama 24 jam. Seluruh microservice penerima secara bertahap memperbarui kuncinya dari MongoDB dalam ≤ 60 detik tanpa mengalami pemutusan koneksi atau error 401.
- **Langkah Penggunaan (Tutorial)**:
  1. **Melihat Status Kunci & Pengamatan Microservice**:
     - Kirim request `GET /service-key/list` menggunakan token pengembang (`protectedDeveloper`).
     - Sistem menampilkan daftar kunci, sumber konfigurasi (`settings` atau `env`), status pengamatan terakhir per microservice (`current`, `previous`, atau `rejected`), serta sisa waktu masa tenggang.
  2. **Melakukan Rotasi Kunci Layanan**:
     - Kirim request `PATCH /service-key/rotate` dengan payload:
       ```json
       {
         "key_id": "internal_api_key",
         "value": "KunciBaruAcakMinimal24KarakterAscii123456",
         "revoke_previous": false
       }
       ```
     - Sistem akan menyimpan kunci lama ke dokumen `service_key_rotation` dengan masa berlaku 24 jam ke depan, lalu menuliskan nilai baru ke konfigurasi aktif.
     - Seluruh layanan (cron-worker, network-monitor, baileys-api, dll.) akan menyinkronkan kunci baru secara mulus dalam siklus refresh 60 detik.
  3. **Menguji Kesehatan Konektivitas Seluruh Layanan**:
     - Kirim request `POST /service-key/check`.
     - Backend akan memanggil endpoint `/key-check` ke seluruh microservice dan mengembalikan status koneksi masing-masing (`ok`, `previous`, `rejected`, atau `unreachable`).

### 2. Aturan Pembuatan Transfer Kas Personal vs Akun Kantor

- **Penjelasan Fitur**: Mekanisme kontrol ganda pada pencatatan transfer saldo keuangan. Fitur ini melindungi kas operasional personal yang dipegang oleh staf administrasi/lapangan agar tidak bisa ditarik sepihak oleh admin lain, sekaligus mengarahkan pengeluaran dari akun kantor ke alur persetujuan manajerial jika diajukan oleh staf umum.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan** → **Transfer Saldo** di panel Dekasimal.
  2. Klik tombol **Buat Transfer**.
  3. Pada pilihan **Akun Asal**:
     - Jika staf memilih kas personal miliknya sendiri, transfer akan tersimpan dengan status `pending_approval` menunggu persetujuan atasan.
     - Jika staf mencoba memilih kas personal milik staf lain, sistem secara otomatis menolak dan memblokir transaksi demi keamanan.
     - Jika admin keuangan yang berwenang memilih akun bank kantor, transaksi langsung berstatus `completed` dan saldo langsung terdebit di buku besar keuangan.

### 3. Pemulihan Penanda Isolir Tersangkut (*Stuck Wait Payment Cleanup*)

- **Penjelasan Fitur**: Alat bantu perbaikan data pada menu Pengaturan Pengembang untuk menghapus sisa penanda `wait_payment` dan `limit_reason` pada akun pelanggan broadband yang langganannya telah aktif kembali. Fitur ini memastikan pelanggan tidak terisolir secara salah di router MikroTik.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan** → **Developer** → **Recovery Tools**.
  2. Cari bagian **Broadband & RADIUS**, lalu pilih kartu **Pembersihan Penanda Isolir Tagihan Tersangkut** (`radiusStaleWaitPayment`).
  3. Klik tombol **Pratinjau (Dry Run)** terlebih dahulu:
     - Sistem akan menampilkan daftar akun aktif yang masih menyimpan sisa flag `wait_payment`, beserta informasi apakah akun tersebut memiliki tunggakan riil atau aman dipulihkan.
  4. Jika hasil pratinjau sudah sesuai, ketik kata konfirmasi `RECOVERY` pada dialog konfirmasi.
  5. Klik tombol **Jalankan Pemulihan**:
     - Sistem akan membersihkan penanda isolir basi tersebut secara massal dan memperbarui status akun di database secara aman.
