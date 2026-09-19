# 📝 Daily Work Report - Dedy S.N Putra (2026-09-19)

---

## 📅 Laporan Harian - 19 September 2026

---

## 🌿 Branch: `claude/focused-jang-66eeb9` — Perbaikan Tampilan Badge Navigasi Sidebar Collapsible

### 📌 Informasi Issue

- **Nomor Issue**: Bugfix / Non-Issue (Terkait Navigasi UI Monorepo)
- **Judul Issue**: Perbaikan Badge Counter Sub-Item Sidebar Menu Collapsible
- **Status Branch**: `Belum di-merge` (Branch aktif lokal berbasis `master`)

### 📅 Rincian Commit

#### [`bdea6ca6`](file:///home/dhedhy/Project/Dekasimal-V2) - fix: badge counter tidak muncul di sub-item sidebar collapsible - 19 September 2026, 18:07:31 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/CollapsibleItem/MenuItem.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/CollapsibleItem/MenuItem.jsx) — Penambahan penanganan badge counter dengan fallback ke hook `useTicketBadge` untuk tema sidebar PrimePanel.
  - [`frontend/src/app/layouts/MainLayout/Sidebar/Sideblock/Sidebar/Menu/Group/CollapsibleItem/MenuItem.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/Sideblock/Sidebar/Menu/Group/CollapsibleItem/MenuItem.jsx) — Penyesuaian serupa pada sub-item collapsible tema Sideblock.
- **Deskripsi Perubahan & Fungsi**:
  - Memperbaiki masalah pre-existing di mana sub-item dalam menu grup collapsible (`NAV_TYPE_COLLAPSE`) tidak menampilkan badge counter (selalu bernilai `undefined`). Hal ini terjadi karena komponen sub-item hanya membaca data dari `useRouteLoaderData('root')` tanpa loader route yang terdefinisi.
  - Menambahkan integrasi fallback ke hook terpusat `useTicketBadge`, menyelaraskan perilakunya dengan `MenuItem` top-level dan panel sidebar saat dalam keadaan ter-collapse. Dengan perbaikan ini, seluruh counter notifikasi (seperti tiket gangguan, antrean WhatsApp, serta indikator tindak lanjut Syslog baru) tampil konsisten saat menu collapsible dibuka.

---

## 🌿 Branch: `issue-301` — Implementasi Syslog Server (RFC 3164/5424, Microservice Terisolasi, Realtime Stream, Pola Pesan AI-Ready, Analisis Otonom & Fitur Tindak Lanjut)

### 📌 Informasi Issue

- **Nomor Issue**: #301
- **Judul Issue**: Implementasi Syslog Server
- **Status Branch**: `Belum di-merge` (Branch aktif lokal unggul 22 commit di depan `origin/issue-301`, dengan perubahan aktif pada berkas pengujian integration)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - Isolasi Timer Pengujian Unit & Integrasi Syslog Service - 19 September 2026

- **Komponen yang Berubah**:
  - [`backend/test/integration/syslog.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslog.service.test.js) — Membatasi cakupan fake timer Vitest pada pengujian `recordSyslogFingerprintForFloodDetection` menjadi `vi.useFakeTimers({ toFake: ['Date'] })`.
- **Deskripsi Perubahan & Fungsi**:
  - Mengatasi potensi *flaky test* dan *deadlock* pada eksekusi test suite backend berskala besar (~1900 pengujian). Sebelumnya, pemanggilan `vi.useFakeTimers()` secara umum membajak seluruh fungsi timer JavaScript global (`setTimeout`, `setInterval`) yang juga dibutuhkan oleh driver internal MongoDB/Mongoose (seperti *server selection*, *heartbeat*, dan *socket timeout*).
  - Pembatasan fake timer hanya pada objek `Date` memastikan query Mongoose tetap responsif tanpa mengalami timeout zombie promise yang dapat mengganggu pengujian di berkas lain.

---

### 📅 Rincian Commit

#### [`538ed6a4`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): indikator & counter tindak lanjut belum selesai di menu sidebar - 19 September 2026, 17:33:46 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/layouts/Root.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/Root.jsx) — Memanggil dispatch inisialisasi `fetchSyslogFollowupCount()` saat aplikasi pertama kali dimuat (dilindungi hak akses privilege `syslog.update`).
  - [`frontend/src/app/pages/network/syslog/components/SyslogFollowupStatusModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogFollowupStatusModal.jsx) — Memperbarui store Redux secara instan pasca update status berhasil agar counter sidebar berkurang tanpa perlu refresh halaman.
  - [`frontend/src/features/syslogFollowupSlice.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/features/syslogFollowupSlice.js) [NEW] — Slice Redux Toolkit baru untuk mengelola state counter tindak lanjut Syslog berstatus `open` via endpoint API `POST /syslog/patterns/ai-analysis/followup` (`pageSize: 1`).
  - [`frontend/src/hooks/useTicketBadge.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useTicketBadge.js) — Memecah percabangan badge grup Jaringan menjadi 3 bagian (`networks.devices`, `networks.syslog`, dan `networks` sebagai agregasi/penjumlahan keduanya untuk indikator titik merah pada menu induk saat ter-collapse).
  - [`frontend/src/store.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/store.js) — Mendaftarkan reducer `syslogFollowup` ke root store aplikasi.
- **Deskripsi Perubahan & Fungsi**:
  - Mengintegrasikan indikator numerik badge pada sidebar navigasi untuk menu Syslog. Operator NOC dapat langsung memantau jumlah insiden/rekomendasi AI yang masih berstatus terbuka (`open`) langsung dari sidebar menu utama.

#### [`0940a665`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): tab Tindak Lanjut di halaman Syslog + badge notifikasi belum ditindak - 19 September 2026, 15:58:33 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/index.jsx) — Penambahan tab ke-4 "Tindak Lanjut" pada tampilan utama halaman Syslog beserta badge counter notifikasi merah dan integrasi TanStack Table.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Penambahan entri translasi untuk label tab dan status tindak lanjut.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan tampilan sentral bagi tim NOC untuk melihat daftar seluruh rekomendasi analisa AI otonom yang diteruskan ke Telegram dan membutuhkan aksi tindak lanjut teknis.

#### [`0a8f1d6e`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): kolom tabel Tindak Lanjut (jenis, risiko, ringkasan, status, aksi) - 19 September 2026, 15:56:29 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/schema/columns.jsx) — Implementasi fungsi pembangun kolom `getFollowupColumns()`.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Entri i18n untuk header kolom, status badge, dan tombol aksi.
- **Deskripsi Perubahan & Fungsi**:
  - Mendefinisikan kolom tabel tindak lanjut: jenis/cakupan analisis (Hourly, Daily, Flooding), tingkat risiko dengan badge warna semantik, ringkasan AI, status tindak lanjut (`open`, `done`, `dismissed`), info petugas/waktu pembaruan, serta tombol aksi untuk memicu modal ubah status dan membuka detail drawer AI.

#### [`7a98bcbc`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): modal update status tindak lanjut notifikasi AI - 19 September 2026, 15:55:47 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/components/SyslogFollowupStatusModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogFollowupStatusModal.jsx) [NEW] — Komponen modal interaktif pembaruan status tindak lanjut berbasis Headless UI Dialog.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — String i18n untuk label modal, opsi status, validasi form, dan pesan konfirmasi.
- **Deskripsi Perubahan & Fungsi**:
  - Memfasilitasi teknisi/admin dalam memperbarui status rekomendasi: status `open`, `done` (selesai), atau `dismissed` (abaikan).
  - Menerapkan validasi wajib isi untuk catatan penanganan saat status diubah menjadi `done`, memastikan akuntabilitas teknis sebelum insiden ditandai selesai.

#### [`5230480f`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): endpoint list & update tindak lanjut notifikasi AI - 19 September 2026, 15:55:04 WIB

- **Komponen yang Berubah**:
  - [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json) — Memperluas deskripsi dan cakupan endpoint untuk privilege `syslog.update`.
  - [`backend/src/controllers/syslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/syslog.controller.js) — Menambahkan controller `listSyslogAiFollowups` dan `updateSyslogAiFollowupStatus`.
  - [`backend/src/routes/syslog.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/syslog.route.js) — Mendaftarkan endpoint `POST /syslog/patterns/ai-analysis/followup` dan `PATCH /syslog/patterns/ai-analysis/:id/followup`.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan jalur API terproteksi untuk memuat dan memperbarui data tindak lanjut menggunakan hak akses operasional `syslog.update` tanpa memerlukan hak akses administratif sensitif.

#### [`c269b206`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): model & service tindak lanjut notifikasi AI (list + update status) - 19 September 2026, 15:54:07 WIB

- **Komponen yang Berubah**:
  - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) — Pesan error validasi status dan catatan tindak lanjut.
  - [`backend/src/models/syslogAiAnalysis.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogAiAnalysis.model.js) — Menambahkan field skema `followup_status`, `followup_note`, `followup_updated_by`, dan `followup_updated_at`.
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Mengembangkan service `findSyslogAiFollowupList` dan `updateSyslogAiFollowup`.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Menambahkan 8 skenario pengujian integrasi terkait daftar dan mutasi status tindak lanjut.
- **Deskripsi Perubahan & Fungsi**:
  - Mengelola siklus hidup tindak lanjut insiden syslog berbasis AI. Service membatasi entri yang dapat ditindaklanjuti hanya pada analisis otonom yang dikirimkan ke Telegram (`notified_telegram: true`), menjaga integritas data riwayat.

#### [`901049fb`](file:///home/dhedhy/Project/Dekasimal-V2) - docs: spec desain fitur Tindak Lanjut notifikasi AI syslog - 19 September 2026, 15:40:22 WIB

- **Komponen yang Berubah**:
  - [`docs/superpowers/specs/2026-09-19-syslog-ai-followup-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-19-syslog-ai-followup-design.md) [NEW] — Berkas spesifikasi teknis dan desain arsitektur fitur Tindak Lanjut notifikasi AI Syslog.
- **Deskripsi Perubahan & Fungsi**:
  - Mendokumentasikan keputusan teknis arsitektur: perluasan model `SyslogAiAnalysis` tanpa membuat koleksi terpisah, terminologi formal "Tindak Lanjut", penentuan hak akses RBAC `syslog.update`, integrasi tab tabel utama, dan perancangan indikator sidebar.

#### [`296873f1`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): ambang notifikasi Telegram ringkasan per jam bisa dikonfigurasi - 19 September 2026, 14:55:18 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js) — Menambahkan opsi default `syslog_ai_hourly_min_risk` (nilai standar: `'medium'`).
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Menyesuaikan fungsi `runSyslogHourlyDigest` untuk membaca konfigurasi ambang batas risiko sebelum mengirimkan alert ke Telegram NOC.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Pengujian integrasi batas ambang notifikasi per jam.
  - [`frontend/src/app/pages/settings/schema/systemSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/systemSchema.js) & [`sections/System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx) — Form pengaturan tingkat risiko minimum ringkasan per jam di tab Pengaturan Sistem.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — String i18n opsi ambang notifikasi.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan fleksibilitas bagi administrator untuk mengatur sensitivitas alert Telegram per jam: opsi `low`, `medium`, `high`, atau `critical` guna mencegah notifikasi berlebih pada jaringan dengan lalu lintas stabil.

#### [`4d1368a3`](file:///home/dhedhy/Project/Dekasimal-V2) - fix(syslog): ganti nama scope 'looping' jadi 'flooding' - 19 September 2026, 14:39:46 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/syslogAiAnalysis.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogAiAnalysis.model.js) — Perubahan enum scope model.
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) & [`test/integration/syslog.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslog.service.test.js) — Penyesuaian nama fungsi dan filter service menjadi `recordSyslogFingerprintForFloodDetection`.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Penyesuaian test case scope `flooding`.
  - [`frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx) — Filter tombol riwayat drawer.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Pemutakhiran string label: "Flooding" / "Log Beruntun".
- **Deskripsi Perubahan & Fungsi**:
  - Menyelaraskan istilah teknis standar industri: menggunakan kata `flooding` untuk merefleksikan kondisi banjir/lonjakan pesan log yang berulang dalam jendela waktu singkat.

#### [`0c33b636`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): badge indikator hasil analisa AI otonom belum ditinjau - 19 September 2026, 14:21:38 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/index.jsx) — Indikator badge numerik merah pada tombol buka drawer Analisa AI.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Teks bantuan tooltip indikator badge.
- **Deskripsi Perubahan & Fungsi**:
  - Menampilkan jumlah analisa AI otonom berisiko menengah ke atas yang belum ditinjau langsung pada toolbar halaman Syslog.

#### [`13bfa88d`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): filter jenis & paginasi di tab Riwayat Analisa AI - 19 September 2026, 14:19:52 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx) — Penambahan segmented filter jenis analisis, sistem paginasi bertahap, dan badge penanda status "Baru".
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — String i18n tombol filter dan pagination.
- **Deskripsi Perubahan & Fungsi**:
  - Meningkatkan navigasi tab riwayat drawer analisa AI: pengguna dapat memfilter riwayat berdasarkan jenis `Manual`, `Per Jam`, `Harian`, atau `Log Beruntun`, serta memuat dokumen riwayat secara bertahap (5 item per muatan).

#### [`e2b8bf92`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): deteksi pola berulang (looping) real-time + trigger AI - 19 September 2026, 14:17:40 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Logika *sliding-window* in-memory per fingerprint pesan log.
  - [`backend/test/integration/syslog.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslog.service.test.js) — 8 skenario pengujian komprehensif untuk deteksi lonjakan log dan mekanisme cooldown.
- **Deskripsi Perubahan & Fungsi**:
  - Mendeteksi anomali *log flooding* secara real-time: apabila sebuah pola pesan yang sama muncul >= 10 kali dalam kurun waktu 5 menit, sistem secara otonom memanggil LLM untuk menganalisis akar masalah dan mengirim peringatan ke Telegram NOC dengan proteksi cooldown 10 menit.

#### [`e60df321`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): daftarkan job cron-worker syslogDailyReport (tiap hari 00:00 WIB) - 19 September 2026, 14:12:10 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/cronSettings.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/cronSettings.service.js) — Mendaftarkan nama cron job ke daftar konfigurasi sistem.
  - [`cron-worker/src/jobs/processors/syslogDailyReport.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/syslogDailyReport.js) [NEW] — Processor BullMQ untuk mengeksekusi job laporan harian.
  - [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js) & [`worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js) — Penjadwalan berulang cron ekspresi `0 0 * * *` (pukul 00:00 WIB).
  - [`cron-worker/src/services/api.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/services/api.service.js) — Pemanggilan endpoint internal backend dengan header otentikasi `x-internal-key`.
- **Deskripsi Perubahan & Fungsi**:
  - Menjadwalkan pemicu otomatis pembuatan laporan harian AI secara terisolasi pada microservice `cron-worker`.

#### [`98529224`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): endpoint internal untuk trigger laporan AI harian - 19 September 2026, 14:11:17 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) — Handler controller `triggerSyslogDailyReport`.
  - [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js) — Pendaftaran rute API `POST /internal/cron/syslog-daily-report`.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan endpoint internal aman yang hanya dapat diakses oleh worker internal untuk memicu proses agregasi laporan 24 jam.

#### [`4d45e4c1`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): logika inti laporan AI harian (runSyslogDailyReport) - 19 September 2026, 14:10:48 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Implementasi fungsi agregasi `runSyslogDailyReport()`.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Pengujian integrasi agregasi ringkasan 24 jam dan format laporan.
- **Deskripsi Perubahan & Fungsi**:
  - Mengompilasi seluruh *hourly digest* dalam 24 jam terakhir menjadi satu dokumen komprehensif tingkat ringkasan manajemen. Laporan harian secara rutin diteruskan ke Telegram NOC sebagai resume stabilitas jaringan.

#### [`ba2af69d`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): daftarkan job cron-worker syslogHourlyDigest (tiap jam) - 19 September 2026, 14:05:44 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/cronSettings.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/cronSettings.service.js) — Registrasi job `syslogHourlyDigest` pada whitelist cron sistem.
  - [`cron-worker/src/jobs/processors/syslogHourlyDigest.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/syslogHourlyDigest.js) [NEW] — Processor BullMQ penjadwal per jam.
  - [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js) & [`worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js) — Penjadwalan ekspresi cron `0 * * * *` (menit ke-0 setiap jam).
  - [`cron-worker/src/services/api.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/services/api.service.js) — Integrasi HTTP internal call ke backend.
- **Deskripsi Perubahan & Fungsi**:
  - Otomasi penjadwalan ringkasan syslog per jam oleh `cron-worker`.

#### [`a99d5258`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): endpoint internal untuk trigger ringkasan AI per jam - 19 September 2026, 14:03:43 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) — Handler `triggerSyslogHourlyDigest`.
  - [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js) — Rute internal `POST /internal/cron/syslog-hourly-digest`.
- **Deskripsi Perubahan & Fungsi**:
  - Menerima sinyal periodik dari cron worker untuk memulai jendela evaluasi pola log 1 jam ke belakang.

#### [`060ebba7`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): logika inti ringkasan AI per jam (runSyslogHourlyDigest) - 19 September 2026, 14:03:01 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Implementasi fungsi `runSyslogHourlyDigest({ windowStart, windowEnd })`.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — 5 unit pengujian logika evaluasi jendela waktu dan penapisan pola.
- **Deskripsi Perubahan & Fungsi**:
  - Mengambil pola log aktif pada kurun waktu 1 jam terakhir berdasarkan `last_seen`, melakukan ranking dan agregasi, memanggil LLM untuk ringkasan ancaman, serta menyimpan dokumen ke `SyslogAiAnalysis` dengan `scope: 'hourly'`.

#### [`9b97a13d`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): sakelar aktif/nonaktif AI otonom di pengaturan Syslog - 19 September 2026, 13:56:44 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js) — Menambahkan default opsi `syslog_ai_autonomous_enabled` (default `false`).
  - [`frontend/src/app/pages/settings/schema/systemSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/systemSchema.js) & [`sections/System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx) — Komponen UI switch sakelar aktif/nonaktif fitur otonom pada Pengaturan Sistem.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — String i18n label dan deskripsi sakelar.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kendali penuh bagi administrator untuk mengaktifkan atau menonaktifkan seluruh komputasi AI otonom dan pengiriman notifikasi Telegram terjadwal.

#### [`b62331e5`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): endpoint jumlah entri analisa AI otonom yang belum ditinjau - 19 September 2026, 13:54:46 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/syslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/syslog.controller.js) — Controller `countUnacknowledgedSyslogPatternsAiAnalysis`.
  - [`backend/src/routes/syslog.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/syslog.route.js) — Endpoint `GET /syslog/patterns/ai-analysis/unacknowledged-count`.
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Fungsi service `countUnacknowledgedSyslogAiAnalysis`.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Pengujian integrasi query hitung badge.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan endpoint efisien untuk menghitung analisis otonom berisiko menengah/tinggi/kritis yang belum dibuka oleh admin (`acknowledged_at: null`).

#### [`62a85c30`](file:///home/dhedhy/Project/Dekasimal-V2) - feat(syslog): perluas model SyslogAiAnalysis untuk scope otonom + acknowledged_at - 19 September 2026, 13:49:09 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/syslogAiAnalysis.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogAiAnalysis.model.js) — Penambahan nilai enum scope (`hourly`, `daily`, `flooding`) serta field `window_start`, `window_end`, `source_digest_ids`, `trigger_fingerprint`, `notified_telegram`, dan `acknowledged_at`.
  - [`backend/src/services/syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) — Pembaruan otomatis `acknowledged_at` saat detail analisa diakses pertama kali.
  - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — 3 unit pengujian untuk inisialisasi dan persistensi `acknowledged_at`.
- **Deskripsi Perubahan & Fungsi**:
  - Fondasi skema database untuk mendukung arsitektur analisa AI mandiri/otonom dan pencatatan audit peninjauan admin.

#### [`0bcd2fdc`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #301 - 19 September 2026, 13:28:43 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Arsitektur**:
    - [`docs/superpowers/specs/2026-09-18-syslog-server-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-18-syslog-server-design.md) [NEW] — Cetak biru arsitektur lengkap Syslog Server.
    - [`docs/superpowers/specs/2026-09-19-syslog-ai-autonomous-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-19-syslog-ai-autonomous-design.md) [NEW] — Spesifikasi teknis rancangan fitur AI otonom.
  - **Microservice Syslog Server (`/syslog-server`)**:
    - [`syslog-server/package.json`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/package.json) [NEW], [`Dockerfile`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/Dockerfile) [NEW], [`eslint.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/eslint.config.js) [NEW] — Inisialisasi microservice baru berbasis Node.js ESM.
    - [`syslog-server/src/server.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/server.js) [NEW] & [`app.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/app.js) [NEW] — Entry point server dengan graceful shutdown.
    - [`syslog-server/src/listeners/udp.listener.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/listeners/udp.listener.js) [NEW] & [`tcp.listener.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/listeners/tcp.listener.js) [NEW] — Listener soket UDP dan TCP pada port 514.
    - [`syslog-server/src/parsers/envelope.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/envelope.js) [NEW], [`rfc3164.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/rfc3164.js) [NEW], [`rfc5424.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/rfc5424.js) [NEW] — Ekstraksi PRI, Facility, Severity, timestamp, dan parsing RFC standar.
    - [`syslog-server/src/parsers/vendors/`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/parsers/vendors/) [NEW] — Parser spesifik perangkat jaringan MikroTik, Cisco, dan Huawei.
    - [`syslog-server/src/services/ingest.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/ingest.service.js) [NEW], [`fingerprint.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/fingerprint.service.js) [NEW], [`rateLimiter.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/rateLimiter.service.js) [NEW], [`hostnameResolver.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/hostnameResolver.service.js) [NEW] — Pipeline pemrosesan log: normalisasi pesan, token bucket rate limiter, resolusi hostname statis & dinamis, serta fingerprinting pola log.
    - [`syslog-server/src/services/backendNotifier.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/src/services/backendNotifier.service.js) [NEW] — Penerusan event log ke backend secara terisolasi via HTTP internal.
    - [`syslog-server/test/`](file:///home/dhedhy/Project/Dekasimal-V2/syslog-server/test/) [NEW] — Rangkaian unit test dan integration test lengkap berbasis Vitest (semua lulus pengujian).
  - **Backend Core (`/backend`)**:
    - Skema Mongoose: [`syslog.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslog.model.js) [NEW], [`syslogPattern.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogPattern.model.js) [NEW], [`syslogAiAnalysis.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/syslogAiAnalysis.model.js) [NEW].
    - Service & Controller: [`syslog.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/syslog.service.js) [NEW], [`syslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/syslog.controller.js) [NEW], [`internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js), [`internalSyslog.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internalSyslog.controller.js) [NEW].
    - Rute & Keamanan: [`syslog.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/syslog.route.js) [NEW], [`privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json), [`privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json).
    - Notifikasi & Utilitas: [`telegramAlert.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegramAlert.js), [`validation-data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/validation-data.js).
    - Test Suites Backend: [`syslog.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslog.service.test.js) [NEW], [`syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) [NEW], [`telegramAlert.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/telegramAlert.test.js) [NEW].
  - **Frontend SPA (`/frontend`)**:
    - Navigasi & Routing: [`networks.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/networks.js), [`networkSyslogRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/network/networkSyslogRoute.jsx) [NEW], [`protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx).
    - Halaman & Drawer: [`index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/index.jsx) [NEW], [`columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/schema/columns.jsx) [NEW], [`SyslogDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogDetailDrawer.jsx) [NEW], [`SyslogLiveView.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx) [NEW], [`HostnameMappingModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/HostnameMappingModal.jsx) [NEW], [`SyslogAiAnalysisDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx) [NEW].
    - Custom Hook Stream: [`useSyslogStream.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/syslog/hooks/useSyslogStream.js) [NEW].
    - Sel Tabel & Pengaturan: [`rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx), [`Table.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/Table.jsx), [`Application.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/Application.jsx), [`System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx).
    - Internasionalisasi: [`translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) (id/en).
- **Deskripsi Perubahan & Fungsi**:
  - Commit utama penyelesaian implementasi Issue #301 secara menyeluruh: integrasi *full-stack* microservice penerima log jaringan UDP/TCP, pipeline ingestion dan deduplikasi pola log berbasis AI, drawer analisis interaktif LLM dengan kemampuan chat lanjutan, pemantauan log konsol langsung via Socket.IO, dan integrasi notifikasi Telegram darurat.

---

## 🌿 Branch: `master` — Pemutakhiran dan Sinkronisasi Internasionalisasi (i18n)

### 📌 Informasi Issue

- **Nomor Issue**: Non-Issue (Pemeliharaan i18n Monorepo)
- **Judul Issue**: Sinkronisasi dan Standardisasi Bahasa Sistem (Indonesian & English)
- **Status Branch**: `Sudah di-merge` (Aktif pada `master` dan `origin/master`)

### 📅 Rincian Commit

#### [`0c56083b`](file:///home/dhedhy/Project/Dekasimal-V2) - fix language - 19 September 2026, 10:44:47 WIB

- **Komponen yang Berubah**:
  - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) — Penambahan dan koreksi 234 entri translasi respon API bahasa Inggris.
  - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) — Penambahan 21 entri translasi respon API bahasa Indonesia.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) — Penambahan dan pembaruan 130 kunci i18n antarmuka pengguna bahasa Inggris.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Penyempurnaan frasa pada translasi antarmuka bahasa Indonesia.
- **Deskripsi Perubahan & Fungsi**:
  - Memperbaiki inkonsistensi bahasa dan melengkapi kunci-kunci terjemahan yang hilang di seluruh modul sistem (termasuk modul inventaris, pengaturan, tiket, dan respon error backend), menjamin dukungan multi-bahasa yang rapi dan profesional bagi pengguna berbahasa Indonesia maupun Inggris.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #301  | Implementasi Syslog Server | Menghadirkan subsistem observabilitas jaringan kelas enterprise: penerima log UDP/TCP port 514 multi-vendor (MikroTik, Cisco, Huawei), pipeline deduplikasi pola log, streaming konsol real-time via Socket.IO, analisis insiden otonom berbasis LLM (ringkasan per jam, laporan 24 jam, deteksi flooding seketika), sistem pelacakan tindak lanjut insiden NOC (*follow-up workflow*), serta indikator badge counter terintegrasi di sidebar menu. |
| Hotfix | Perbaikan Badge Sidebar Collapsible | Memperbaiki bug komponen navigasi sub-item collapsible agar counter notifikasi (tiket, pesan, dan tindak lanjut Syslog) tampil konsisten di semua varian sidebar. |
| i18n  | Sinkronisasi Bahasa Monorepo | Standardisasi kamus translasi Bahasa Indonesia dan Inggris pada frontend dan backend, memastikan seluruh respon sistem memiliki teks lokal yang tepat tanpa fallback teks mentah. |

### Kemampuan Baru Pengguna/Admin

- **Pemantauan Log Real-time Berkecepatan Tinggi**: Teknisi NOC dapat memantau log jaringan secara langsung (*live streaming terminal*) dengan fitur jeda/lanjutkan (*pause/resume*), filter keparahan instan (*severity filter*), pencarian cepat pada buffer memori, dan auto-scroll.
- **Analisis AI Terpandu & Tanya-Jawab Interaktif**: Pengguna dapat meminta AI menganalisis ribuan pola log sekaligus, memperoleh estimasi risiko jaringan, serta berdiskusi interaktif dengan asisten AI dalam drawer obrolan untuk mencari solusi masalah.
- **Manajemen Tindak Lanjut Insiden Jaringan**: Admin dan staf NOC dapat mengelola rekomendasi masalah yang dikirimkan oleh AI ke Telegram langsung dari Tab "Tindak Lanjut", mengubah status penanganan (`open`, `done`, `dismissed`), dan mendokumentasikan catatan resolusi insiden.
- **Notifikasi Multi-Kanal Otomatis**: Integrasi bot Telegram NOC yang mengirimkan alert instan saat terjadi lonjakan log mencurigakan (*log flooding*), ringkasan per jam terfilter, serta laporan harian kesehatan jaringan pukul 00:00 WIB.
- **Pemetaan Hostname Dinamis & Statis**: Administrator dapat memetakan alamat IP perangkat jaringan ke alias nama perangkat yang mudah dikenali melalui modal dialog yang bersih dan responsif.
- **Konfigurasi Fleksibel Parameter AI & Syslog**: Pengaturan retensi data log (hari), nomor port listener, sakelar fitur AI otonom, dan batas ambang risiko alert Telegram dapat diatur langsung dari menu Pengaturan Sistem.

### Bug Fix / Solusi Masalah

- **Sub-Item Sidebar Collapsible Badge Failure**: Mengatasi hilangnya badge counter pada sub-menu collapsible dengan menerapkan fallback ke hook `useTicketBadge`.
- **Vitest FakeTimers Hijacking MongoDB Driver**: Memperbaiki pembajakan global timer proses oleh `vi.useFakeTimers()` pada pengujian integrasi syslog service dengan membatasi fake timer hanya pada objek `Date`, mencegah kegagalan timeout acak pada test suite berskala besar.
- **Penyelarasan Terminologi 'Flooding'**: Mengganti scope 'looping' menjadi 'flooding' guna merefleksikan kondisi lonjakan frekuensi log berulang secara tepat sesuai terminologi standar telekomunikasi.
- **Inkonsistensi Kamus Bahasa i18n**: Memperbaiki kunci terjemahan yang hilang dan melengkapi teks respon backend dan antarmuka frontend pada Bahasa Indonesia dan Inggris.

### Menu/Fitur Baru

- **Menu Navigasi Syslog**: Menu baru di bawah grup **Jaringan** (`/networks/syslog`) dengan hak akses granular RBAC `syslog.list`, `syslog.create`, `syslog.update`, dan `syslog.changeSensitive`.
- **Tab Navigasi 4-in-1 Halaman Syslog**:
  1. *Riwayat Log*: Eksplorasi log historis terstruktur dengan TanStack Table, paginasi, dan filter multi-kolom.
  2. *Live Stream*: Konsol terminal interaktif untuk inspeksi log real-time.
  3. *Pola Pesan*: Agregasi pola unik log ternormalisasi yang siap dianalisis.
  4. *Tindak Lanjut*: Daftar rekomendasi perbaikan insiden dengan filter status penanganan dan badge counter aktif.
- **Badge Counter Sidebar Syslog**: Indikator angka pada menu sidebar Syslog dan titik merah (*red dot*) pada ikon induk Jaringan saat sidebar dalam mode collapsed yang menghitung jumlah tugas tindak lanjut yang belum selesai.
- **Pengaturan Syslog & AI Otonom**: Bagian konfigurasi Syslog pada Pengaturan Sistem untuk mengatur retensi log, aktivasi AI otonom, dan batas sensitivitas alert Telegram.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penjelasan Fitur: Manajemen & Analisis Syslog Terpadu dengan AI

Modul Syslog DEKASIMAL V2 dirancang untuk menangkap, menormalkan, dan menginterpretasikan log dari ratusan perangkat jaringan ISP (seperti router MikroTik, OLT ZTE/Huawei, dan switch Cisco). Dengan arsitektur microservice terisolasi, beban lalu lintas log UDP/TCP port 514 diproses tanpa membebani server backend utama. Integrasi AI tingkat lanjut menyediakan:
- **Analisis On-Demand**: Admin memilih pola log tertentu di tabel, lalu AI menganalisis anomali dan merekomendasikan langkah mitigasi.
- **Analisis Otonom**: Worker di latar belakang memantau lonjakan log (*flooding*), menghasilkan resume performa per jam (*hourly digest*), serta menyusun laporan eksekutif harian (*daily report*) ke grup Telegram NOC.
- **Workflow Tindak Lanjut**: Setiap temuan insiden penting yang diteruskan ke Telegram dicatat secara otomatis dalam tab "Tindak Lanjut" untuk dipantau hingga selesai ditangani oleh teknisi.

---

### 2. Langkah Penggunaan (Tutorial Singkat)

#### A. Memantau Aliran Log Real-Time (Live Stream):
1. Buka menu **Jaringan** pada bilah sisi navigasi, lalu pilih **Syslog**.
2. Klik tab **Live Stream** di bagian atas halaman untuk membuka antarmuka terminal hitam interaktif.
3. Gunakan tombol **Jeda (Pause)** jika ingin menahan aliran log saat menganalisis baris tertentu, atau tombol **Lanjutkan (Resume)** untuk melanjutkan streaming.
4. Gunakan filter tingkat keparahan (*ALL*, *CRIT*, *ERR*, *WARN*, *INFO*) atau ketik kata kunci pada kolom pencarian in-memory untuk memfilter baris log secara seketika.

#### B. Menjalankan Analisis Pola Log dengan AI:
1. Pindah ke tab **Pola Pesan**.
2. Pilih satu atau beberapa baris pola log menggunakan kotak centang di sisi kiri tabel.
3. Klik tombol **Analisa AI** di sudut kanan atas toolbar tabel.
4. Drawer Analisa AI akan terbuka dari sisi kanan layar, menampilkan evaluasi tingkat risiko (*Critical/High/Medium/Low*), ringkasan akar masalah, dan rekomendasi perbaikan teknis.
5. Gunakan kolom obrolan di bagian bawah drawer untuk mengajukan pertanyaan teknis lanjutan ke AI mengenai penanganan pola log tersebut.

#### C. Menyelesaikan Tindak Lanjut Insiden AI (Follow-up):
1. Klik tab **Tindak Lanjut** (perhatikan angka badge merah yang menunjukkan jumlah insiden terbuka).
2. Temukan baris insiden yang hendak diselesaikan, lalu klik tombol **Ubah Status** (ikon pensil/checklist) di kolom Aksi.
3. Pada modal dialog yang muncul:
   - Pilih opsi status **Selesai (Done)**.
   - Masukkan ringkasan tindakan teknis yang telah dilakukan pada kolom **Catatan Penanganan** (wajib diisi).
   - Klik **Simpan Pembaruan**.
4. Status baris akan berubah menjadi badge hijau (*Done*), modal tertutup, dan counter badge di tab serta sidebar menu navigasi akan langsung berkurang secara otomatis.
