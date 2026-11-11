# 📝 Daily Work Report - Dedy S.N Putra (2026-09-18)

---

## 📅 Laporan Harian - 18 September 2026

---

## 🌿 Branch: `issue-301` — Implementasi Syslog Server (RFC 3164/5424, Microservice Terisolasi, Realtime Stream, Pola Pesan AI-Ready & Frontend Monitoring)

### 📌 Informasi Issue

- **Nomor Issue**: #301
- **Judul Issue**: Implementasi Syslog Server
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan lokal dan remote `origin/issue-301`, unggul 24 commit dari remote dengan berkas kerja aktif / uncommitted)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - Pemolesan UI Frontend: HostnameMappingModal, SyslogDetailDrawer, Peningkatan Live View, Tab Pola Pesan, dan Kolom Standar - 18 September 2026

- **Komponen yang Berubah**:
  - **Frontend — Komponen Dialog, Drawer Detail, Terminal Live, dan Tata Letak**:
    - [`frontend/src/app/pages/network/syslog/components/HostnameMappingModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/HostnameMappingModal.jsx) [NEW] — Transformasi panel pemetaan inline lama menjadi modal dialog Headless UI (`@headlessui/react`) yang bersih, terisolasi, dan mudah diakses via tombol toolbar tabel. Mendukung penambahan IP statis router/server ke alias hostname dan penghapusan mapping dengan notifikasi toast.
    - [`frontend/src/app/pages/network/syslog/components/HostnameMappingPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/HostnameMappingPanel.jsx) [DELETED] — Dihapus dan digantikan sepenuhnya oleh `HostnameMappingModal.jsx` untuk menyederhanakan halaman utama.
    - [`frontend/src/app/pages/network/syslog/components/SyslogDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogDetailDrawer.jsx) [NEW] — Komponen drawer detail syslog untuk inspeksi mendalam pesan log jaringan:
      - Menampilkan rincian terstruktur: Payload mentah (*raw message*), Tingkat Keparahan (*Severity*) dengan badge warna semantik, Fasilitas (*Facility*), Hostname sumber, Alamat IP pengirim, Vendor perangkat (*Cisco*, *Huawei*, *Mikrotik*, atau *Generic*), Tag/proses aplikasi, dan Waktu kejadian (*timestamp* terformat).
      - Tombol salin pesan log satu-klik (*One-click copy to clipboard*) dengan umpan balik visual instan.
    - [`frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx) — Peningkatan drastis pengalaman pemantauan aliran log secara langsung (*real-time console*):
      - **Kontrol Aliran (Pause / Resume)**: Tombol jeda/lanjutkan aliran stream dengan indikator lingkaran berkedip (*ping animation*) hijau saat aktif dan kuning saat dijeda.
      - **Kunci Gulir Otomatis (Auto-Scroll)**: Opsi toggle auto-scroll ke baris paling bawah saat pesan baru tiba.
      - **Pencarian Cepat dalam Buffer**: Kolom input pencarian instan untuk menyaring log di memori buffer tanpa me-reload data.
      - **Penyaring Tingkat Keparahan Cepat**: Tombol filter segmented (*ALL*, *CRIT*, *ERR*, *WARN*, *INFO*) untuk memfilter tampilan baris console secara dinamis.
      - **Terminal Viewport Modern**: Desain bernuansa dark terminal profesional lengkap dengan indikator port UDP:514 / TCP:514, counter buffer, dan tombol pembersihan buffer (*Clear*).
    - [`frontend/src/app/pages/network/syslog/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/index.jsx) — Perombakan halaman utama modul Syslog:
      - **4 Kartu Metrik Ringkasan 24 Jam**: Total Log 24 Jam, Log Kritis & Darurat (*Critical/Emergency*), Log Peringatan & Error (*Warning/Error*), serta Log Informasi & Notifikasi (*Info/Notice*).
      - **Pengalih Mode Tampilan 3-in-1**:
        - *Tabel Riwayat*: Menampilkan riwayat log tersimpan via TanStack Table dengan paginasi, pengurutan, dan filter multi-kolom.
        - *Live Stream*: Tampilan konsol terminal langsung berbasis Socket.IO.
        - *Pola Pesan*: Tampilan agregasi template log unik yang siap digunakan untuk analisis pola / AI.
      - Integrasi tombol pemetaan hostname modal dan tombol penyegaran manual.
    - [`frontend/src/app/pages/network/syslog/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/schema/columns.jsx) — Standarisasi kolom tabel syslog:
      - Integrasi `SyslogSeverityBadgeCell` dari `rows.jsx` untuk konsistensi badge severity.
      - Penambahan kolom aksi `RowActions` dengan opsi *View* yang men-trigger pembukaan `SyslogDetailDrawer`.
      - Penambahan definisi kolom `getPatternColumns` untuk tabel Pola Pesan: menampilkan tingkat keparahan terburuk (*Worst Severity*), template pesan ternormalisasi, vendor, tag, frekuensi kemunculan (*Count*), waktu pertama terlihat (*First Seen*), dan waktu terakhir terlihat (*Last Seen*).
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) — Penambahan komponen pembungkus sel tabel `SyslogSeverityBadgeCell` dengan pemetaan warna semantik badge: `emergency`/`alert`/`critical` (merah), `error` (oranye/merah), `warning` (kuning), `notice` (biru), `info` (ungu/sekunder), `debug` (abu-abu/netral).
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) — Penambahan key translasi i18n untuk fitur live stream, jeda aliran, penyaring severity, modal pemetaan hostname, drawer detail, dan tabel pola pesan.

---

### 📅 Rincian Commit

#### [`ee66cd9`](file:///home/dhedhy/Project/Dekasimal-V2) - fix(syslog-server): mock dotenv in env.test.js to prevent secret leakage - 18 September 2026, 17:52:23 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/test/unit/env.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/test/unit/env.test.js) — Melakukan mock pada pustaka `dotenv` dalam pengujian unit environment config agar variabel `.env` lokal tidak mencemari lingkungan pengujian CI/CD dan mencegah kebocoran kredensial rahasia saat pengujian dijalankan.

#### [`f0d01e6`](file:///home/dhedhy/Project/Dekasimal-V2) - fix(syslog-server): add missing eslint.config.js (npm run lint was broken) - 18 September 2026, 16:20:17 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/eslint.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/eslint.config.js) [NEW] — Penambahan berkas konfigurasi ESLint standar untuk modul `syslog-server` sehingga perintah `npm run lint` dapat berjalan tanpa error dan selaras dengan standar linting monorepo.

#### [`cbdea9f`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(frontend): add custom hostname mapping panel to Syslog page - 18 September 2026, 16:19:16 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/components/HostnameMappingPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/HostnameMappingPanel.jsx) [NEW] — Pembuatan komponen pengelolaan pemetaan hostname kustom untuk memetakan alamat IP perangkat jaringan ke nama ramah pengguna (*friendly hostname*).

#### [`dd5860d`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(frontend): add live mode to Syslog page via socket.io - 18 September 2026, 16:18:21 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx) [NEW] — Komponen penampil aliran pesan log real-time dengan terminal emulator.
  - [`frontend/src/app/pages/network/syslog/hooks/useSyslogStream.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/hooks/useSyslogStream.js) [NEW] — Custom React hook yang berlangganan event Socket.IO `syslog:event`, mengelola buffer in-memory melingkar (*circular buffer*) maksimum 200 log, dan menyediakan fungsionalitas pembersihan log.

#### [`232ec7b`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(frontend): add Syslog page with history table - 18 September 2026, 16:17:33 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/index.jsx) [NEW] — Halaman utama manajemen dan penelusuran log syslog jaringan dengan TanStack Table, filter pencarian per field, dan paginasi server-side.
  - [`frontend/src/app/pages/network/syslog/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/schema/columns.jsx) [NEW] — Definisi kolom tabel log historis.

#### [`88f27b9`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(frontend): add Syslog tab to System Settings (port + retention) - 18 September 2026, 16:16:25 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/developer/SyslogSettingsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/SyslogSettingsTab.jsx) [NEW] — Tab konfigurasi Syslog pada Pengaturan Sistem untuk mengelola masa simpan data (*retention days*) dan nomor port listening syslog server.
  - [`frontend/src/app/pages/settings/sections/Developer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/Developer.jsx) — Penautan tab Syslog ke navigasi pengaturan pengembang.

#### [`bff65fc`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(frontend): add Syslog nav entry and route (page not yet implemented) - 18 September 2026, 16:14:25 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/navigation/networks.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/networks.js) — Penambahan item menu sidebar `Syslog` di bawah kategori Jaringan dengan perlindungan hak akses `syslog.list`.
  - [`frontend/src/app/router/network/syslogRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/network/syslogRoute.jsx) [NEW] — Pendaftaran rute URL `/networks/syslog` pada router aplikasi.

#### [`0aa33cf`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(backend): add /internal/syslog-events endpoint for realtime broadcast + Telegram alert - 18 September 2026, 16:13:29 WIB

- **Komponen yang Berubah**:
  - [`backend/src/routes/internalSyslog.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internalSyslog.route.js) [NEW] & [`backend/src/controllers/internalSyslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internalSyslog.controller.js) [NEW] — Endpoint internal `POST /internal/syslog-events` yang diproteksi secret key: menerima dorongan pesan log dari `syslog-server`, menyiarkannya ke frontend via Socket.IO room `syslog:event`, dan meneruskan alert log berstatus darurat/kritis ke bot Telegram grup operasional NOC.

#### [`b534198`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(backend): add syslog routes, controller, and privilege entries - 18 September 2026, 16:08:23 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/syslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/syslog.controller.js) [NEW] — Controller REST API: `getSyslogList`, `getSyslogPatterns`, `getSyslogStats`, `getHostnameMapping`, `upsertHostnameMapping`, `deleteHostnameMapping`.
  - [`backend/src/routes/syslog.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/syslog.route.js) [NEW] — Pendaftaran rute API terproteksi `/api/v1/syslog/*` lengkap dengan dokumentasi OpenAPI/Swagger.
  - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) & [`privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json) — Pendaftaran hak akses RBAC granular `syslog` (`list`, `create`, `update`, `delete`).

#### [`aca457f`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(backend): add syslog service (list, patterns, stats, hostname CRUD) - 18 September 2026, 16:07:15 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) [NEW] — Business logic query: pencarian datatable dengan paginasi dan filter regex efisien, agregasi statistik 24 jam (`total`, `critical`, `warning`, `info`), pemuatan pola unik, dan manipulasi mapping hostname pada koleksi `options`.

#### [`bb7e578`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(backend): add syslog_retention_days setting and syslog i18n strings - 18 September 2026, 16:03:08 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js) — Penambahan default opsi sistem untuk `syslog_retention_days` (standar 30 hari).
  - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) — String i18n respon API syslog.

#### [`5f6ca01`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(backend): add read-side syslog and syslog_patterns models - 18 September 2026, 16:02:28 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/syslog.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslog.model.js) [NEW] — Skema Mongoose read-side untuk koleksi `syslogs` dengan index compound.
  - [`backend/src/models/syslogPattern.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogPattern.model.js) [NEW] — Skema Mongoose read-side untuk koleksi agregasi `syslog_patterns`.

#### [`54b93ce`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add UDP/TCP listeners and bootstrap server.js - 18 September 2026, 16:01:58 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/server.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/server.js) [NEW] — Bootstrap entrypoint service: menginisialisasi listener UDP (`dgram`) dan TCP (`net`) pada port 514, server status HTTP Express pada port 3050, serta penanganan *graceful shutdown* sinyal SIGINT/SIGTERM.
  - [`syslog-server/src/listeners/udp.listener.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/listeners/udp.listener.js) [NEW] & [`tcp.listener.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/listeners/tcp.listener.js) [NEW] — Socket listener berperforma tinggi untuk menerima datagram UDP dan stream TCP dengan pemisahan baris (*framing delimiter* `\n`).

#### [`f743656`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add backend notifier and ingest orchestration with pattern upsert - 18 September 2026, 15:48:19 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/services/ingest.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/ingest.service.js) [NEW] — Orkestrator utama penerimaan pesan log: menjalankan validasi rate-limit, parsing amplop RFC, enrichment vendor, resolusi hostname, penyimpanan Mongo secara asinkron, pembaruan statistik pola pesan (*pattern upsert*), dan pemicuan notifikasi ke Backend.
  - [`syslog-server/src/services/backendNotifier.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/backendNotifier.service.js) [NEW] — Pengirim HTTP POST asinkron ke endpoint internal backend dengan mekanisme non-blocking buffer agar performa ingest UDP tidak terhambat.

#### [`a24fb8d`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add hostname resolver with NetworkDevice + custom host fallback - 18 September 2026, 15:46:26 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/services/hostnameResolver.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/hostnameResolver.service.js) [NEW] — Resolusi nama host otomatis: membaca inventaris perangkat jaringan `NetworkDevice` (Router, Switch, OLT) dari MongoDB, menyimpannya di cache memori lokal dengan *TTL refresh*, dan mencocokkan IP pengirim log dengan fallback ke pemetaan manual `Option.syslog_custom_hosts`.

#### [`e6ff401`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add per-IP token bucket rate limiter - 18 September 2026, 15:45:44 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/services/rateLimiter.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/rateLimiter.service.js) [NEW] — Proteksi banjir log (*log storm / flooding attack*) menggunakan algoritma *Token Bucket* per alamat IP sumber, membatasi pesan masuk per detik dan mencatat drop count untuk keamanan sistem.

#### [`f5e0ebc`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add Mongoose models for syslogs, syslog_patterns, Option, NetworkDevice - 18 September 2026, 15:45:15 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/models/syslog.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/models/syslog.model.js) [NEW] — Skema model dokumen log dengan index TTL otomatis (`expireAfterSeconds`) berdasarkan konfigurasi retensi hari.
  - [`syslog-server/src/models/syslogPattern.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/models/syslogPattern.model.js) [NEW] — Model penyimpanan ringkasan pola pesan teragregasi.
  - [`syslog-server/src/models/networkDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/models/networkDevice.model.js) [NEW] & [`option.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/models/option.model.js) [NEW] — Referensi model inventaris dan opsi.

#### [`e1ea632`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add Cisco/Huawei/Mikrotik vendor enrichment parsers - 18 September 2026, 15:44:38 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/parsers/vendor.parser.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/vendor.parser.js) [NEW] — Parser lapis kedua spesifik vendor:
    - **Cisco**: Mengekstrak pola `%FACILITY-SEVERITY-MNEMONIC: message`.
    - **Huawei**: Mengekstrak pola `%%ddDEVICE/SEVERITY/MNEMONIC(l): message`.
    - **Mikrotik**: Mengekstrak tag topik berjenjang (`system,info,account`, `interface,warning`, dll.).

#### [`3203ccd`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add message fingerprint/normalization service - 18 September 2026, 15:43:22 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/services/fingerprint.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/fingerprint.service.js) [NEW] — Service normalisasi teks log: menghapus variabel dinamis (IP address, MAC address, angka hex, ID angka, timestamp, path interface) menjadi placeholder `<VAR>`, lalu mengomputasi SHA-256 hash sebagai sidik jari (*fingerprint*) unik untuk klastering pola log dan persiapan analisa kecerdasan buatan (AI).

#### [`cf40f5f`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): add RFC3164/RFC5424 envelope parser - 18 September 2026, 15:42:44 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/src/parsers/envelope.parser.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/envelope.parser.js) [NEW] — Parser amplop syslog standar internet RFC 3164 (BSD Syslog) dan RFC 5424 (IETF Syslog): menghitung facility dan severity numerik dari PRI header (`<PRI>`), mengekstrak timestamp ISO/BSD, hostname, app-name, procid, dan structured data.

#### [`aedf516`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog-server): scaffold service with env config, db connection, logger, health endpoint - 18 September 2026, 15:41:43 WIB

- **Komponen yang Berubah**:
  - [`syslog-server/package.json`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/package.json) [NEW], [`src/config/env.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/config/env.js) [NEW], [`src/config/db.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/config/db.js) [NEW], [`src/utils/logger.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/utils/logger.js) [NEW] — Pondasi microservice baru `syslog-server` dengan konfigurasi lingkungan terisolasi, koneksi MongoDB, sistem logging Winston terstruktur, dan endpoint `/health`.

#### [`f4e99fe`](file:///home/dhedhy/Project/Dekasimal-V2) - docs: revisi mekanisme realtime syslog dari Change Stream ke push HTTP - 18 September 2026, 14:59:22 WIB
#### [`da0855f`](file:///home/dhedhy/Project/Dekasimal-V2) - docs: siapkan struktur data syslog untuk analisa AI di masa depan - 18 September 2026, 14:34:04 WIB
#### [`27cdfec`](file:///home/dhedhy/Project/Dekasimal-V2) - docs: tambah spec desain syslog server (issue #301) - 18 September 2026, 14:28:37 WIB

- **Komponen yang Berubah**:
  - [`docs/superpowers/specs/2026-09-18-syslog-server-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-18-syslog-server-design.md) [NEW] — Dokumen spesifikasi arsitektur komprehensif implementasi syslog server v2 mandiri, mencakup diagram aliran data, perbandingan arsitektur Change Stream vs Push HTTP, perancangan skema data ber-TTL, normalisasi template pesan, algoritma rate limiting token bucket, dan antarmuka monitoring frontend.

---

## 🌿 Branch: `issue-321` — Penambahan Kolom Alamat dan Area pada Daftar Tiket Belum Terjadwal di Scheduler

### 📌 Informasi Issue

- **Nomor Issue**: #321
- **Judul Issue**: Penambahan Kolom Alamat dan Area pada Daftar Tiket Belum Terjadwal di Scheduler
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` dan `production`)

---

### 📅 Rincian Commit

#### [`106eca5`](file:///home/dhedhy/Project/Dekasimal-V2) & [`d2c3421`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #321 - 18 September 2026, 19:47:03 & 19:47:37 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Versi Rilis**:
    - [`backend/src/data/changelog/releases/issue-321.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-321.json) [NEW] — Berkas rilis v1.79.1 mendokumentasikan fitur penambahan alamat/area tiket belum terjadwal, perbaikan badge jenis tiket, dan perbaikan tombol tutup drawer tiket.
  - **Backend Core — Service Scheduler & Integrasi Notifikasi Telegram**:
    - [`backend/src/services/scheduler.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/scheduler.service.js):
      - Memperluas fungsi pengambilan tiket tak terjadwal (`findUnscheduledTickets`) dengan memuat data relasi `address` dan `area` secara mendalam dari model pelanggan (`Customer`) maupun mitra (`Partner`/`CustomerPartner`).
      - Menjamin informasi lokasi geografis pelanggan tersedia untuk penjadwalan efisien tim teknisi lapangan.
    - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) — Penambahan penanganan topik forum Telegram untuk tipe tiket `backbone`, `business`, dan `backhaul` agar notifikasi broadcast tiket masuk ke thread/topik yang relevan.
  - **Frontend — Modul Scheduler & Tiket Operasional**:
    - [`frontend/src/app/pages/activities/scheduler/components/UnscheduledTicketsSection.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/components/UnscheduledTicketsSection.jsx):
      - Penambahan kolom **Alamat** dan **Area** pada tabel 'Daftar Tiket Belum Terjadwal' di halaman pembuatan (`/create`) dan pengeditan (`/edit`) jadwal kegiatan harian.
      - Mendukung penyaringan dan pencarian tiket instan berdasarkan kata kunci alamat atau wilayah area.
    - [`frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/components/TicketAssignModal.jsx):
      - Menampilkan informasi rincian alamat dan area pada modal penugasan tiket teknisi sebelum tiket disematkan ke tim kerja tertentu.
    - [`frontend/src/app/pages/activities/scheduler/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/create.jsx) & [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/edit.jsx):
      - Penyempurnaan tata letak kartu tim kerja dengan menyertakan detail alamat dan area pada setiap tiket yang ditugaskan.
      - Perbaikan tombol hapus tiket dari kartu tim: penggunaan ikon tempat sampah `FaTrash` yang konsisten, penambahan proteksi `shrink-0`, dan pemotongan teks judul tiket panjang agar tombol aksi tidak terdesak atau terpotong.
    - [`frontend/src/app/pages/activities/scheduler/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/detail.jsx):
      - Menambahkan informasi jenis/tipe tiket serta garis pembatas pemisah yang rapi antar tim kerja pada pratinjau modal format pesan teks (WhatsApp / Telegram).
    - [`frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/tickets/TicketDetailDrawer.jsx):
      - Perbaikan tombol silang (*close button*) yang sebelumnya tidak menutup drawer ketika dibuka dalam mode *controlled* (seperti pada modul scheduler kegiatan harian).
    - [`frontend/src/components/shared/Badge.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/Badge.jsx) & [`frontend/src/components/shared/table/status.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/status.js):
      - Menambahkan registrasi opsi status dan warna badge untuk tipe tiket `backbone` (indigo), `business` (purple), `backhaul` (cyan), `maintenance` (warning), dan `troubleshoot` (rose), serta menyematkan penanganan fallback yang aman agar badge tidak kosong.
    - [`frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx):
      - Penambahan pemetaan id topik Telegram untuk tipe tiket backbone, business, dan backhaul.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
      - Penambahan translasi untuk kolom alamat dan area tiket belum terjadwal.

---

## 🌿 Branch: `issue-320` — Pemantauan Sesi Internet Terputus & Proteksi Alokasi IP Statis

### 📌 Informasi Issue

- **Nomor Issue**: #320
- **Judul Issue**: Pemantauan Sesi Internet Terputus & Proteksi Alokasi IP Statis
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` dan `production`)

---

### 📅 Rincian Commit

#### [`b5e34ff`](file:///home/dhedhy/Project/Dekasimal-V2) & [`a44745c`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #320 - 18 September 2026, 10:38:35 & 10:39:09 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Versi Rilis**:
    - [`backend/src/data/changelog/releases/issue-320.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-320.json) [NEW] — Berkas rilis v1.79.0 mendokumentasikan panel pemutusan sesi PPPoE dan proteksi sinkronisasi alokasi IP statis.
  - **Backend Core — Manajemen Alokasi IPv4 & Pencegahan Kebocoran IP**:
    - [`backend/src/services/networkIPv4Used.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkIPv4Used.service.js) [NEW] — Layanan khusus untuk mengelola lifecycle pemakaian alamat IPv4 statis: memastikan penandaan IP `used` dan `usedby` konsisten dengan akun PPPoE aktif, serta pembebasan otomatis IP saat akun diubah/dihapus.
    - [`backend/src/controllers/radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js) & [`radiusAuthenticationNonCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthenticationNonCustomer.controller.js):
      - Integrasi transaksi pembaruan IP statis: saat pelanggan broadband atau staf non-pelanggan diberi IP statis baru, IP lama otomatis dibebaskan dan IP baru ditandai sebagai terpakai.
      - Menghapus celah *IP allocation leak* yang sebelumnya membuat IP yang sedang terpakai keliru muncul sebagai IP kosong pada pemilihan IP pool.
    - [`backend/scripts/diagnose-broadband-ip-leak.mjs`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/diagnose-broadband-ip-leak.mjs) [NEW] & [`backend/scripts/diagnose-noncustomer-ip-leak.mjs`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/diagnose-noncustomer-ip-leak.mjs) [NEW] — Skrip diagnostik pemindaian database untuk mengidentifikasi dan mereparasi inkonsistensi alokasi IP statis pada seluruh akun aktif.
  - **Backend Core — Agregasi Metrik Disconnect & Realtime Event**:
    - [`backend/src/services/radiusStats.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusStats.service.js) [NEW] — Agregasi statistik pemutusan sesi internet PPPoE (alasan pemutusan `User-Request`, `Lost-Carrier`, `Admin-Reset`, `Session-Timeout`), tren disconnect 24 jam, serta identifikasi pelanggan dengan koneksi paling sering gagal tersambung.
    - [`backend/src/services/radiusEvent.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusEvent.service.js) — Penambahan buffer log koneksi dan mekanisme deduplikasi event real-time agar aliran event bersih dan bebas dari event kembar (*duplicate stream event*).
    - [`backend/src/controllers/radiusControl.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusControl.controller.js) & [`backend/src/routes/radiusControl.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/radiusControl.route.js) — Penambahan endpoint REST API untuk mengambil statistik pemutusan sesi dan riwayat disconnect.
    - Pengujian Integrasi & Unit: [`backend/test/integration/networkIPv4Used.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/networkIPv4Used.service.test.js) [NEW], [`radiusAuthentication.controller.updateIp.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthentication.controller.updateIp.test.js) [NEW], [`radiusAuthenticationNonCustomer.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthenticationNonCustomer.controller.test.js) [NEW], [`radiusStats.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusStats.service.test.js) [NEW], [`radiusEventDedup.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/radiusEventDedup.test.js) [NEW], [`radiusEventLiveDedup.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/radiusEventLiveDedup.test.js) [NEW].
  - **Frontend — Dashboard Autentikasi RADIUS**:
    - [`frontend/src/app/pages/dashboards/radius/components/PppoeDisconnectPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/radius/components/PppoeDisconnectPanel.jsx) [NEW] — Panel baru yang menampilkan daftar riwayat pemutusan sesi PPPoE lengkap dengan nama pengguna, waktu pemutusan, dan badge alasan pemutusan.
    - [`frontend/src/app/pages/dashboards/radius/components/DualAreaChart.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/radius/components/DualAreaChart.jsx) [NEW] — Grafik visualisasi area bertingkat (*Dual Area Chart*) untuk membandingkan tren pemutusan sesi yang diinisiasi oleh pengguna (*User Request*) versus masalah jaringan/sistem (*Lost Carrier/Timeout*) dalam 24 jam terakhir.
    - [`frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx) — Pemisahan tab aktivitas antara pelanggan yang paling sering terhubung (*Top Connected*) dan yang paling sering mengalami kegagalan autentikasi (*Top Failed Attempts*).
    - [`frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx) & [`hooks/useRadiusStream.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js) — Deduplikasi event live stream dan penataan ulang tampilan log real-time.
  - **Radius Server Daemon (Go)**:
    - [`radius-server/proto/radius/v1/radius.proto`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/proto/radius/v1/radius.proto) & gRPC generated stubs — Pembaruan interface gRPC bidi-stream untuk notifikasi event disconnect terperinci.
    - [`radius-server/internal/domain/sweep/sweep.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/sweep/sweep.go) — Peningkatan mekanisme penyapuan sesi kedaluwarsa (*stale session sweep*) dan penanganan perangkat NAS yang belum terdaftar (*unknown NAS*).

---

## 🌿 Branch: `issue-319` — Pengaturan Fleksibilitas Tiket Formulir Pelanggan & Broadband

### 📌 Informasi Issue

- **Nomor Issue**: #319
- **Judul Issue**: Pengaturan Fleksibilitas Tiket Formulir Pelanggan & Broadband
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` dan `production`)

---

### 📅 Rincian Commit

#### [`c97a02b`](file:///home/dhedhy/Project/Dekasimal-V2) & [`76572ae`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #319 - 18 September 2026, 09:04:59 & 09:05:38 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Versi Rilis**:
    - [`backend/src/data/changelog/releases/issue-319.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-319.json) [NEW] — Berkas rilis v1.78.0 mendokumentasikan fitur kendali konfigurasi khusus pengembang untuk fleksibilitas tiket instalasi.
  - **Backend Core — Endpoint Konfigurasi Pembuatan & Penyesuaian Validasi**:
    - [`backend/src/routes/customer.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/customer.route.js), [`customerPartner.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/customerPartner.route.js), [`radiusAuthentication.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/radiusAuthentication.route.js):
      - Penambahan endpoint `GET /api/v1/customer/create-config`, `GET /api/v1/customer-partner/create-config`, dan `GET /api/v1/broadband/create-config` untuk mengembalikan status fleksibilitas konfigurasi sistem kepada frontend form.
    - [`backend/src/controllers/customer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customer.controller.js), [`customerPartner.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerPartner.controller.js), [`radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js):
      - Penyesuaian aturan pembuatan data: jika opsi fleksibilitas aktif, pembuatan record diizinkan tanpa menyertakan ID tiket instalasi (`ticket_registration`).
    - [`backend/src/controllers/warehouseItem.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/warehouseItem.controller.js):
      - Penyesuaian alur pengambilan dan alokasi perangkat gudang ketika data pelanggan dibuat tanpa tiket pemasangan awal.
    - [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json):
      - Pendaftaran endpoint baru `create-config` pada izin akses pelanggan, mitra, dan broadband.
  - **Frontend — Pengaturan Pengembang & Formulir Pelanggan/Broadband**:
    - [`frontend/src/app/pages/settings/sections/developer/SpecialSettingsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/SpecialSettingsTab.jsx) [NEW]:
      - Pembuatan tab **Pengaturan Khusus** baru pada menu Pengaturan Pengembang (`/settings` -> Developer).
      - Dua sakelar kontrol interaktif dengan status visual (*Wajib* vs *Opsional*):
        - **Wajib Tiket Instalasi Pelanggan**: Opsi sakelar `disableCustomerTicketMandatory` untuk mengubah pemilihan tiket instalasi menjadi opsional saat pendaftaran pelanggan baru (reguler maupun mitra).
        - **Wajib Tiket Instalasi Layanan Broadband**: Opsi sakelar `disableBroadbandTicketMandatory` untuk membuat pemilihan tiket instalasi menjadi opsional saat pendaftaran layanan internet broadband baru.
      - Dilengkapi kotak penjelasan teknis latar belakang penggunaan (*use case* migrasi data pelanggan lama tanpa tiket).
    - [`frontend/src/app/pages/users/customer/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/create.jsx) & [`schema/createShema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/schema/createShema.js):
      - Menghubungi endpoint `create-config` saat halaman dibuka, dan menyesuaikan skema validasi Yup secara dinamis: jika dinonaktifkan, field tiket registrasi menjadi `.optional().nullable()`.
    - [`frontend/src/app/pages/services/broadband/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/create.jsx) & [`schema/createShema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/schema/createShema.js):
      - Pengondisian dinamis validasi tiket registrasi broadband menjadi opsional sesuai preferensi pengaturan pengembang.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
      - Penambahan teks translasi untuk Pengaturan Khusus, status opsional, dan catatan panduan sakelar.

#### [`2d0c646`](file:///home/dhedhy/Project/Dekasimal-V2) - update changelog - 18 September 2026, 10:44:46 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json) — Pendaftaran riwayat rilis baru v1.78.0 (`issue-319.json`) dan v1.79.0 (`issue-320.json`) ke daftar rilis sistem monorepo.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #301  | Implementasi Syslog Server (RFC 3164/5424 & AI Ready) | Menggantikan penerima syslog sederhana v1 dengan microservice mandiri `syslog-server` (UDP/TCP 514, rate limiting, parser amplop RFC + vendor enrichment Cisco/Huawei/Mikrotik, auto-resolve hostname, TTL retention, live stream Socket.IO, dan agregasi pola pesan untuk analisa AI). |
| #321  | Penambahan Kolom Alamat & Area pada Scheduler | Menambahkan informasi alamat dan area pada tabel tiket belum terjadwal di modul scheduler untuk mempermudah pemetaan lokasi kerja teknisi, memperbaiki warna/opsi badge tipe tiket, dan memperbaiki tombol tutup drawer tiket. |
| #320  | Pemantauan Sesi PPPoE Disconnect & Proteksi IP Statis | Menghadirkan panel riwayat sesi internet terputus dan grafik tren 24 jam pada dashboard autentikasi, serta memperbaiki kebocoran alokasi alamat IPv4 statis melalui sinkronisasi atomik otomatis antara akun aktif dan IP pool. |
| #319  | Pengaturan Fleksibilitas Tiket Formulir Pelanggan & Broadband | Menyediakan sakelar khusus di Pengaturan Pengembang untuk menjadikan kolom tiket instalasi opsional saat pendaftaran pelanggan baru dan layanan broadband, memfasilitasi onboarding pelanggan migrasi tanpa alur tiket teknisi. |

### Kemampuan Baru Pengguna/Admin

- **Pemantauan Log Jaringan Terpusat (Syslog Monitoring)**: Tim NOC dan administrator jaringan kini dapat menerima dan memantau log aktivitas dari seluruh perangkat router, switch, OLT, dan server (Cisco, Huawei, Mikrotik, Linux) secara terpusat dengan filter keparahan (*Emergency* hingga *Debug*), resolusi hostname otomatis berdasarkan inventaris perangkat, dan penjelajahan pola log.
- **Konsol Aliran Log Interaktif**: Administrator dapat memantau kedatangan log secara langsung (*live stream* layaknya console CLI), menjeda aliran log untuk analisis baris tertentu (*Pause*), mengaktifkan/menonaktifkan gulir otomatis (*Auto-scroll*), dan mencari teks log instan langsung dari peramban web.
- **Inspeksi Mendalam Log Jaringan**: Klik baris log pada tabel langsung membuka `SyslogDetailDrawer` yang menampilkan payload mentah, data terstruktur RFC, tag proses, serta menyediakan tombol salin log cepat.
- **Kemudahan Penugasan Tiket Berbasis Lokasi**: Admin dispatcher jadwal kini dapat melihat alamat lengkap dan area calon pelanggan secara langsung pada tabel tiket belum terjadwal di halaman scheduler, sehingga pengelompokan pekerjaan tim teknisi per wilayah geografis menjadi jauh lebih akurat dan hemat waktu perjalanan.
- **Analisis Sesi Internet Terputus**: Tim operasional jaringan dapat mendiagnosis penyebab pelanggan sering mengalami gangguan internet melalui panel pemutusan sesi PPPoE dan grafik komparasi pemutusan sisi pengguna (*User-Request*) versus kendala fisik jaringan (*Lost-Carrier* / *Timeout*).
- **Fleksibilitas Onboarding Pelanggan Tanpa Tiket**: Administrator dapat mengaktifkan mode opsional tiket pada menu Pengaturan Pengembang sehingga staf administrasi dapat mendaftarkan pelanggan migrasi atau pelanggan eksisting secara langsung tanpa harus membuat tiket instalasi teknisi buatan.

### Bug Fix / Solusi Masalah

- **Pencegahan Kebocoran Alokasi Alamat IPv4 (Issue #320)**: Menghapus celah *IP allocation leak* di mana alamat IP statis yang sedang digunakan oleh pelanggan broadband atau staf non-pelanggan keliru terdeteksi sebagai alamat IP kosong yang bebas dialokasikan; siklus hidup IP kini tersinkronisasi otomatis saat akun dibuat, diubah, dinonaktifkan, maupun dihapus.
- **Perbaikan Tombol Tutup TicketDetailDrawer (Issue #321)**: Tombol silang (*close button*) pada laci detail tiket kini merespons dengan benar saat drawer dipanggil dalam mode *controlled* pada modul jadwal kegiatan harian.
- **Kelengkapan Badge Jenis Tiket (Issue #321)**: Tipe tiket khusus seperti `backbone`, `business`, `backhaul`, `maintenance`, dan `troubleshoot` kini memiliki styling badge semantik yang terdaftar lengkap dan tidak lagi menampilkan badge kosong/keliru.
- **Deduplikasi Log Real-Time (Issue #320 & #301)**: Menerapkan buffer deduplikasi pada aliran event Socket.IO untuk mencegah pesan kembar membanjiri antarmuka pemantauan pengguna.
- **Pencegahan Banjir Log / Flooding (Issue #301)**: Implementasi algoritma *Token Bucket Rate Limiter* per-IP pada service penerima syslog mencegah perangkat yang *looping error* memenuhi kapasitas penyimpanan database dan membebani server backend.

### Menu/Fitur Baru

- **Menu & Halaman Baru Syslog**: Rute `/networks/syslog` pada menu sidebar Jaringan -> Syslog, lengkap dengan 3 mode peninjauan (*Tabel Riwayat*, *Live Stream*, *Pola Pesan*), kartu ringkasan status 24 jam, dan modal pemetaan hostname kustom.
- **Tab Pengaturan Syslog**: Tab baru pada menu Pengaturan Pengembang untuk mengatur masa retensi log (hari) dan nomor port listener.
- **Panel Disconnect & Dual Area Chart pada Dashboard Radius**: Widget pemantauan sesi PPPoE yang terputus beserta tren 24 jam dan breakdown alasan pemutusan sesi.
- **Tab Pengaturan Khusus Pengembang**: Tab kontrol sakelar fleksibilitas kewajiban tiket pemasangan untuk formulir pendaftaran pelanggan dan layanan broadband.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  Modul **Syslog Server (v2)** merupakan arsitektur pemantauan log perangkat jaringan monorepo Dekasimal V2 yang beroperasi sebagai microservice mandiri (`syslog-server`). Layanan ini mendengarkan datagram UDP dan TCP pada port standar 514, memproses log melalui parser dua lapis (lapis 1: ekstraksi amplop RFC 3164/5424; lapis 2: ekstraksi pola spesifik vendor Cisco, Huawei, dan Mikrotik), memetakan IP pengirim ke nama perangkat router/switch/OLT secara otomatis via cache inventaris perangkat `NetworkDevice`, serta menyimpan pesan ke MongoDB dengan indeks masa kedaluwarsa otomatis (*TTL Index*). Log berstatus darurat (*Emergency* / *Alert*) otomatis dipancarkan ke grup Telegram NOC, sementara seluruh aliran log dapat dipantau langsung di browser via Socket.IO.

- **Langkah Penggunaan (Tutorial Pemantauan Syslog)**:
  1. Masuk ke aplikasi web Dekasimal V2 menggunakan akun administrator yang memiliki izin `syslog.list`.
  2. Buka menu sidebar **Jaringan** -> pilih menu **Syslog** (`/networks/syslog`).
  3. Pada tampilan utama, amati 4 kartu ringkasan status (*Total Log 24 Jam*, *Kritis/Darurat*, *Peringatan/Error*, *Info/Notifikasi*).
  4. Untuk memantau aliran log perangkat secara langsung:
     - Klik tab tombol **Live Stream**.
     - Perhatikan baris-baris log yang masuk secara instan pada terminal console gelap.
     - Gunakan tombol **Jeda Stream** untuk menghentikan gulir saat ingin menganalisis pesan penting, atau ketik kata kunci pada kotak **Cari Log** untuk menyaring pesan tertentu (mis. nama interface atau IP).
     - Klik tombol **Lanjutkan Stream** untuk kembali mengikuti aliran log terkini.
  5. Untuk meninjau riwayat log tersimpan:
     - Klik tab tombol **Tabel Log**.
     - Gunakan filter per-kolom (Tingkat Keparahan, Hostname, Alamat IP, Vendor, Tag, atau Pesan).
     - Klik tombol ikon mata pada kolom **Aksi** untuk membuka **Drawer Detail Log** guna melihat rincian amplop RFC, payload mentah, dan menyalin isi pesan log.
  6. Untuk memetakan IP router yang belum memiliki nama perangkat di sistem:
     - Klik tombol **Pemetaan Hostname** di kanan atas tabel.
     - Masukkan Alamat IP perangkat dan Nama Host yang diinginkan, lalu klik **Tambah Pemetaan**. Log mendatang dari IP tersebut akan langsung menampilkan nama yang telah dipetakan.
