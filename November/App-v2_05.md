# 📝 Daily Work Report - Dedy S.N Putra (2026-11-05)

---

## 📅 Laporan Harian - 5 November 2026

---

## 🌿 Branch: `issue-526` — Pemantauan ONT Terjadwal, Deteksi Gangguan Massal, Operasi Tulis Konfigurasi OLT (Irisan A), serta Manajemen Service-Port & VLAN (Irisan B)

### 📌 Informasi Issue

- **Nomor Issue**: #526
- **Judul Issue**: Pemantauan Berkala ONT, Deteksi Gangguan Massal Port/OLT, Operasi Tulis Konfigurasi OLT (Registrasi, Hapus, Simpan, Deskripsi), serta Inventori & Pengelolaan Service-Port dan VLAN OLT
- **Status Branch**: `Belum di-merge` (Dalam pengembangan aktif di branch `issue-526`, ahead 12 commit dari `origin/issue-526`, seluruh pengujian unit & integrasi lulus)

### 📅 Rincian Commit

---

### 🚀 Bagian 1: Irisan B (Slice B) — Manajemen Service-Port & VLAN OLT

#### [f5d53e8c] - save #526 gateway: inject vlanSyncService dan servicePortSyncService pada server.js - 05 November 2026, 21:10:02 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/server.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menyuntikkan `vlanSyncService` dan `servicePortSyncService` dari kontainer dependensi ke dalam bootstrap Express `createApp` pada `server.js` OLT Gateway, memastikan endpoint internal gateway siap melayani sinkronisasi VLAN dan Service-Port secara langsung.

#### [4e39051a] - save #526 backend: bersihkan unused import di oltConfig.service (lint fix) - 05 November 2026, 20:44:01 WIB
- **Komponen yang Berubah**:
  - `backend/src/services/oltConfig.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - Membersihkan *unused imports* pada `oltConfig.service.js` agar mematuhi standar linter proyek Dekasimal V2.

#### [d1c575d0] - save #526 docs: perbarui AGENTS.md untuk inventori service-port dan rute gateway (LB2) - 05 November 2026, 20:42:34 WIB
- **Komponen yang Berubah**:
  - `AGENTS.md`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui panduan arsitektur `AGENTS.md` pada klausul OLT Gateway: mendokumentasikan rute internal baru (`POST /internal/olts/:olt_id/sync-vlans` dan `POST /internal/olts/:olt_id/sync-service-ports`), batas waktu sinkronisasi, serta pemisahan koleksi `olt_service_ports`.

#### [8f29b32c] - save #526 frontend: tab manajemen VLAN dan terjemahan i18n Irisan B (FB3 & FB4) - 05 November 2026, 20:38:01 WIB
- **Komponen yang Berubah**:
  - `frontend/src/app/pages/network/oltManagement/components/OltVlanSection.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/detail.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Tab Manajemen VLAN OLT (`OltVlanSection.jsx`)**: Menghadirkan antarmuka pengelolaan VLAN pada detail OLT. Admin dapat melihat daftar VLAN aktif, tipe VLAN (*smart/mux/standard*), deskripsi, melakukan sinkronisasi live dari OLT, serta membuat atau menghapus VLAN melalui alur konfirmasi plan-execute.
  - **Lokalisasi Menyeluruh**: Menambahkan entri terjemahan i18n lengkap (Bahasa Indonesia & Bahasa Inggris) untuk seluruh status, peringatan, tabel, dan form aksi VLAN dan Service-Port.

#### [dd9be8f3] - save #526 frontend: drawer dan tabel Service-Port pada detail ONT (FB2) - 05 November 2026, 20:35:01 WIB
- **Komponen yang Berubah**:
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailBody.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailBody.test.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailDrawer.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/OntServicePortSection.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/config/ServicePortCreateDrawer.jsx` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Tabel Service-Port pada Detail ONT (`OntServicePortSection.jsx`)**: Menampilkan daftar alokasi service-port yang terikat ke ONT terpilih (VLAN, Gemport, User-VLAN, RX/TX traffic table, status operasional *up/down*).
  - **Drawer Pembuatan Service-Port (`ServicePortCreateDrawer.jsx`)**: Menyediakan formulir pembuatan service-port baru untuk ONT dengan validasi alokasi ID otomatis (*auto-assign*) atau spesifik, pemilihan VLAN terdaftar, serta konfirmasi alur dua tahap plan-execute.
  - **Aksi Hapus Service-Port**: Menyertakan tombol hapus service-port individual dengan proteksi konfirmasi ketik nama OLT.

#### [b0254190] - save #526 frontend: perluas configApi dan test untuk service-port dan VLAN (FB1) - 05 November 2026, 20:33:06 WIB
- **Komponen yang Berubah**:
  - `frontend/src/app/pages/network/oltOnt/components/config/configApi.js`
  - `frontend/src/app/pages/network/oltOnt/components/config/configApi.test.js`
  - `frontend/src/app/pages/network/oltOnt/components/config/utils/configActions.js`
- **Deskripsi Perubahan & Fungsi**:
  - Memperluas pustaka client API `configApi.js` dengan fungsi sinkronisasi VLAN (`syncVlans`), sinkronisasi service-port (`syncServicePorts`), serta pengambilan daftar service-port per ONT (`getOntServicePorts`).
  - Menyertakan unit test komprehensif `configApi.test.js` untuk menguji parameter request dan handling response.

#### [1d0a4bcb] - save #526 backend: layanan, rute, dan controller service-port & VLAN (BB3 & BB4) - 05 November 2026, 20:31:46 WIB
- **Komponen yang Berubah**:
  - `backend/src/controllers/oltConfig.controller.js`
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/services/oltGateway.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Controller & Service Layer Irisan B**:
    - `planServicePortCreate` & `planServicePortDelete`: Perencanaan perintah CLI pembentukan dan penghapusan service-port.
    - `planVlanCreate` & `planVlanDelete`: Perencanaan perintah pembuatan dan pembersihan VLAN di OLT.
    - `syncVlans` & `syncServicePorts`: Orkestrasi sinkronisasi data dari gateway ke koleksi MongoDB backend.
  - **Proxy Client Gateway (`oltGateway.service.js`)**: Menambahkan pemanggilan aman ke OLT gateway untuk sinkronisasi VLAN dan service-port via REST dengan header API key internal.

#### [bd3f7ad3] - save #526 backend: konstanta dan validasi input service-port dan VLAN (BB2) - 05 November 2026, 20:28:35 WIB
- **Komponen yang Berubah**:
  - `backend/src/utils/oltConfig.constants.js`
  - `backend/src/utils/oltConfigInput.js`
  - `backend/test/unit/oltConfigInput.sliceB.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mendefinisikan konstanta operasi konfigurasi baru: `SERVICE_PORT_CREATE`, `SERVICE_PORT_DELETE`, `VLAN_CREATE`, dan `VLAN_DELETE`.
  - Menerapkan fungsi validasi input ketat `validateServicePortInput` (rentang VLAN 1..4094, Gemport 0..7, User-VLAN) dan `validateVlanInput` (tipe Smart/Mux/Standard) serta unit test `oltConfigInput.sliceB.test.js`.

#### [7343634c] - save #526 backend: model OltServicePort dan field vlans pada OltDevice (BB1) - 05 November 2026, 20:27:34 WIB
- **Komponen yang Berubah**:
  - `backend/src/models/oltDevice.model.js`
  - `backend/src/models/oltServicePort.model.js` [NEW]
  - `backend/test/unit/oltServicePort.model.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Model `OltServicePort`**: Membuat model Mongoose terisolasi untuk menyimpan rekonsiliasi service-port OLT (service_port_id, vlan_id, frame, slot, port, ont_id, gemport, user_vlan, state, rx/tx traffic table).
  - **Field `vlans` pada `OltDevice`**: Menambahkan array embedded `vlans` pada schema `OltDevice` untuk menampung ringkasan VLAN yang dikonfigurasi pada perangkat OLT beserta waktu sinkronisasi `vlans_fetched_at`.

#### [efb472df] - save #526 gateway: layanan sinkronisasi VLAN dan service-port serta rute internal (GB3) - 05 November 2026, 20:26:34 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/app.js`
  - `olt-gateway/src/container.js`
  - `olt-gateway/src/controllers/internal.controller.js`
  - `olt-gateway/src/models/oltServicePort.model.js` [NEW]
  - `olt-gateway/src/repositories/oltRepo.js`
  - `olt-gateway/src/repositories/oltServicePortRepo.js` [NEW]
  - `olt-gateway/src/routes/internal.route.js`
  - `olt-gateway/src/services/servicePortSync.service.js` [NEW]
  - `olt-gateway/src/services/vlanSync.service.js` [NEW]
  - `olt-gateway/test/syncSliceB.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Layanan Sinkronisasi OLT Gateway**:
    - `vlanSync.service.js`: Mengeksekusi pembacaan seluruh VLAN via CLI, mem-parse output, dan mengupdate field `vlans` pada dokumen OLT device secara atomik.
    - `servicePortSync.service.js`: Membaca seluruh service-port di OLT, memvalidasi integritas data, dan melakukan rekonsiliasi *bulk write* ke koleksi `olt_service_ports`.
  - **Rute Internal Gateway**: Menyediakan endpoint terproteksi API key `/internal/olts/:olt_id/sync-vlans` dan `/internal/olts/:olt_id/sync-service-ports`.

#### [05f5de4e] - save #526 gateway: perintah dan resep service-port dan VLAN (GB2) - 05 November 2026, 20:22:29 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/test/operationPlan.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Mendeklarasikan resep perintah CLI pada dialect Huawei MA5600:
    - Resep pembuatan service-port: `service-port [id] vlan [vlan] gpon [frame]/[slot]/[port] ont [ont_id] gemport [gemport] multi-service user-vlan [user_vlan]`.
    - Resep penghapusan service-port: `undo service-port [service_port_id]`.
    - Resep pembuatan VLAN: `vlan [vlan_id] smart` & penamaan/deskripsi VLAN.
    - Resep penghapusan VLAN: `undo vlan [vlan_id]`.
  - Memperbarui `operationPlan.test.js` untuk memvalidasi perakitan perintah yang aman bebas karakter berbahaya.

#### [f6873627] - save #526 gateway: parser VLAN dan service-port dari fixture nyata lab (GB1) - 05 November 2026, 20:16:49 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/dialects/huawei/parsers.js`
  - `olt-gateway/test/fixtures/huawei/service-port-all-lab.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/service-port-ont-missing-lab.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/service-port-ont-present-lab.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/vlan-all-lab.txt` [NEW]
  - `olt-gateway/test/parsers.b.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan fungsi parser berkinerja tinggi `parseVlanAll` dan `parseServicePortAll` berbasis keluaran nyata OLT lab Huawei MA5683T.
  - Menambahkan fixture keluaran CLI nyata dan pengujian unit `parsers.b.test.js` dengan penanganan variasi baris kosong dan pager OLT.

---

### 🛡️ Bagian 2: Irisan A (Slice A) — Operasi Tulis Konfigurasi OLT & Alur Plan-Execute

#### [f6d1d16c] & [e03a484a] - resolve #526: operasi tulis konfigurasi OLT (slice A) - 05 November 2026, 19:46:38 WIB
- **Komponen yang Berubah**:
  - Arsitektur OLT Gateway (`olt-gateway/*`):
    - `src/safety/commandGuard.js`, `src/operations/operationPlan.js`, `src/services/oltConfig.service.js`, `src/services/profileSync.service.js`, `src/transport/cliSession.js`
  - Backend Dekasimal (`backend/*`):
    - `src/models/oltConfigAudit.model.js`, `src/controllers/oltConfig.controller.js`, `src/routes/oltConfig.route.js`, `src/services/oltConfig.service.js`, `src/services/oltConfigNotify.js`
    - `src/config/privilege.json`, `src/config/privilegeDictionary.json`
  - Frontend Dekasimal (`frontend/*`):
    - Drawer operasi konfigurasi (`ConfigOperationDrawer.jsx`, `OntRegisterDrawer.jsx`, `SaveConfigDrawer.jsx`, `AuditDetailDrawer.jsx`)
    - Komponen status (`ConfigStatusBanner.jsx`, `ConfigWriteCard.jsx`, `PlanPreview.jsx`, `ResultPanel.jsx`, `ConfigHistoryTab.jsx`)
- **Deskripsi Perubahan & Fungsi**:
  - **Sistem Saklar Pengaman Ganda (*Dual Safety Switch*)**:
    - Operasi tulis ke perangkat OLT fisik diproteksi oleh dua saklar: saklar lingkungan proses `OLT_WRITE_ENABLED` pada OLT Gateway dan saklar per-perangkat `config_write` di database/backend. Jika salah satu nonaktif, operasi tulis ditolak seketika (*fail-closed*).
  - **Alur Dua Tahap Terverifikasi (Plan → Execute)**:
    - **Tahap Plan**: Server menyusun rencana perintah CLI persis yang akan dikirim ke OLT, memvalidasi parameter, dan menyimpannya di Redis dengan TTL 600 detik (10 menit). Pengguna disajikan pratinjau perintah mentah di antarmuka web.
    - **Tahap Execute**: Eksekusi hanya diproses jika pengguna mengetikkan nama OLT secara persis sama sebagai konfirmasi sadar (*explicit typing confirmation*).
  - **Katalog Operasi Konfigurasi Didukung**:
    - `ONT_REGISTER`: Pendaftaran ONT baru dari tabel autofind dengan pengikatan Line Profile, Service Profile, dan Serial Number.
    - `ONT_SET_DESCRIPTION`: Pembaruan nama/deskripsi pelanggan pada ONT di OLT.
    - `ONT_RESET`: Pengiriman sinyal reboot ONT dari jarak jauh.
    - `ONT_DELETE`: Pelepasan konfigurasi ONT dari port GPON.
    - `CONFIG_SAVE`: Penyimpanan konfigurasi memori OLT ke flash non-volatile (`save`).
  - **Audit Trail Mutlak & Append-Only (`olt_config_audits`)**:
    - Setiap rencana dan eksekusi dicatat tanpa kemungkinan diubah/dihapus, mencakup identitas admin, parameter input, teks perintah CLI terencana, output mentah OLT, durasi eksekusi, serta status keberhasilan.
    - Redaksi otomatis: data sensitif (seperti password PPPoE) disensor sebelum masuk log audit.
  - **Antrean & Idempotensi Per-OLT**: Mencegah tabrakan sesi CLI konkuren saat beberapa admin atau proses background beroperasi pada OLT yang sama.

---

### 🔬 Bagian 3: Pengujian Lab & Tinjauan Redaman Optik On-Demand

#### [c6fddf73], [6407be40], [1a60ab42], [fdfc5d63] - save #526 pengujian lab & tinjauan redaman optik ONT - 05 November 2026, 15:10:52 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/transport/cliSession.js`
  - `olt-gateway/src/services/ontDetail.service.js`
  - `olt-gateway/test/cliSession.test.js`
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailDrawer.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Sesi CLI Ber-Scope `config-if-gpon`**: Menyempurnakan pembacaan parameter optik (*optical power*) ONT pada OLT Huawei yang mewajibkan navigasi ke sub-mode interface GPON (`interface gpon 0/x`). Sesi dikelola dengan scope terlindungi dan otomatis kembali ke prompt utama (`quit`).
  - **Robustness Kanal & Fallback Error**: Jika langkah pertama pembukaan sub-mode interface gagal, kanal transport langsung ditutup dan ditandai bersih, mencegah sesi OLT tersangkut (*stuck session*).
  - **Dukungan Firmware Versi Khusus Lab**: Memperbarui parser autofind agar mengenali header firmware lama OLT Huawei MA5683T lab (`EquipmentID`, `SoftwareVersion`, `VendorID`).
  - **Pesan Status Jelas di UI**: Memberikan indikasi status informatif `unavailable` saat ONT dalam kondisi offline atau pembacaan redaman tidak didukung oleh modul ONT pelanggan.

---

### ⏱️ Bagian 4: Pemantauan ONT Terjadwal & Deteksi Gangguan Massal

#### [a741caa2] s/d [e8816a82] (14 commit) - save #526 pemantauan berkala & deteksi gangguan massal ONT - 05 November 2026, 09:37:21 WIB
- **Komponen yang Berubah**:
  - `cron-worker/src/jobs/oltOntPoll.job.js` [NEW]
  - `backend/src/models/oltOntEvent.model.js` [NEW]
  - `backend/src/models/oltOutage.model.js` [NEW]
  - `backend/src/models/oltDailyAggregate.model.js` [NEW]
  - `backend/src/services/oltOntMonitor.service.js` [NEW]
  - `backend/src/controllers/oltOntMonitor.controller.js` [NEW]
  - `backend/src/routes/oltOntMonitor.route.js` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/OutageBanner.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/OntEventHistoryTab.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/OutageHistoryTab.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/DailyStatsTab.jsx` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Worker Terjadwal BullMQ (`oltOntPoll`)**: Menjadwalkan pemindaian status ONT berkala setiap 5 menit untuk seluruh OLT yang mengaktifkan saklar pemantauan `ont_monitor`.
  - **Deteksi Event & Analisis Gangguan Cerdas**:
    - Menganalisis transisi status ONT (`online` → `offline`) dan mendeteksi alasan kegagalan: *dying-gasp* (mati listrik pelanggan), *LOS* (Loss of Signal / kabel optik putus), atau *deactivated*.
    - **Korelasi Gangguan Massal (*Mass Outage Detection*)**: Jika dalam satu port GPON terdapat ≥3 ONT padam bersamaan karena *LOS* atau *dying-gasp*, sistem secara otomatis menetapkan status **Gangguan Massal Port / Kabel Distribusi** dan memicu notifikasi peringatan ke Telegram NOC dengan waktu berformat WIB.
  - **Agregat Kinerja Harian**: Mengalkulasi metrik uptime dan rasio gangguan per hari yang disimpan pada `olt_daily_aggregates` untuk keperluan analisis SLA jaringan.
  - **Komponen Antarmuka Pengguna Pemantauan**:
    - **Spanduk Gangguan Massal (*Outage Banner*)**: Banner peringatan menyala di bagian atas halaman ONT saat insiden aktif terdeteksi.
    - **Tab Riwayat Event & Gangguan**: Menampilkan jejak pemadaman tiap ONT beserta histori gangguan massal yang telah pulih.

---

## 🌿 Branch: `issue-527` — Pembaruan Portal Mitra (Partner API & Partner Frontend: Manajemen Dokumen Pelanggan/Pelanggan Mitra, Refactoring Tampilan PPPoE & Detail Pelanggan, Keamanan Password Sensitif)

### 📌 Informasi Issue

- **Nomor Issue**: #527
- **Judul Issue**: Pembaruan Portal Mitra: Endpoint Dokumen Pelanggan & Pelanggan Mitra, Refactoring Tampilan PPPoE & Detail Pelanggan, Keamanan Password Sensitif, serta Peningkatan Form Pengaturan Gateway
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `b4ea180f`)

### 📅 Rincian Commit

#### [b4ea180f] & [fd52b6f3] - resolve #527 - 05 November 2026, 15:43:42 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/business.controller.js`
  - `backend/src/controllers/partner.controller.js`
  - `backend/src/controllers/partnerApiBusiness.controller.js`
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/ownerDeleteBlockers.service.js`
  - `backend/test/integration/ownerDeleteBlockers.test.js`
  - `backend/test/integration/partnerApiBusiness.test.js`
  - `backend/test/integration/partnerApiCustomer.create.test.js`
  - `backend/test/integration/partnerApiCustomer.documents.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/finance/gatewayProviders/IpaymuGatewaySettings.jsx`
  - `frontend/src/app/pages/settings/sections/finance/gatewayProviders/IpaymuGatewaySettings.test.jsx`
  - `frontend/src/app/pages/settings/sections/finance/gatewayProviders/XenditGatewaySettings.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/form/FormInput.test.jsx` [NEW]
  - `frontend/src/styles/app/vendors/quill.css`
  - `partner-frontend/src/app/pages/customers/business/view.jsx`
  - `partner-frontend/src/app/pages/customers/customer/view.jsx`
  - `partner-frontend/src/app/pages/services/pppoe/view.jsx`
  - `partner-frontend/src/app/pages/services/broadband/detail.jsx`
  - `partner-frontend/src/components/shared/DeletedAccountNotice.jsx` [NEW]
  - `partner-frontend/src/components/shared/SensitivePassword.jsx` [NEW]
  - `partner-frontend/src/hooks/useAutoRefresh.js` [NEW]
  - `partner-frontend/src/utils/autoRefresh.js` [NEW]
  - `partner-frontend/src/utils/radiusConnectionStatus.js` [NEW]
  - Pembersihan berkas modul usang: `broadbandProfile/*` di portal mitra
- **Deskripsi Perubahan & Fungsi**:
  - **Manajemen Dokumen Pelanggan pada Partner API (`partnerApi.route.js` & `partnerApiCustomer.controller.js`)**:
    - Menambahkan endpoint REST lengkap untuk melihat daftar dokumen (`GET /customers/:id/documents`), mengunduh dokumen (`GET /customers/:id/documents/:filename`), dan menghapus dokumen (`DELETE /customers/:id/documents/:filename`), berlaku untuk entitas Pelanggan maupun Pelanggan Mitra.
    - Mengakomodasi fleksibilitas pembuatan pelanggan mitra oleh role admin: parameter `partner` kini dapat berupa `partner_id` publik atau `ObjectId`, serta otomatis memilih mitra aktif jika form dikosongkan.
  - **Peningkatan Tampilan & UX Portal Mitra (`partner-frontend`)**:
    - **Refactoring Komprehensif Halaman Detail**: Memperbarui tampilan detail pelanggan personal (`customer/view.jsx`), pelanggan bisnis (`business/view.jsx`), dan akun PPPoE (`services/pppoe/view.jsx`) dengan tata letak informasi modern, badge status koneksi RADIUS live, dan indikator tanggal jatuh tempo invoice.
    - **Proteksi Password Sensitif (`SensitivePassword.jsx`)**: Mengaburkan kata sandi akun PPPoE/kredensial secara default dengan tombol klik untuk membuka (*reveal*) yang diaudit.
    - **Pemberitahuan Akun Terhapus (`DeletedAccountNotice.jsx`)**: Menampilkan banner visual peringatan jika akun/layanan terkait telah dihapus (*soft-deleted*).
    - **Hook Auto-Refresh Cerdas (`useAutoRefresh.js`)**: Mengotomatiskan pembaruan data berkala di portal mitra tanpa mengganggu interaksi form aktif.
    - **Pembersihan Modul Usang**: Menghapus modul `broadbandProfile` di portal mitra yang sudah tidak relevan demi menjaga kebersihan kode (*clean codebase*).
  - **Peningkatan Integritas Formulir Frontend Utama**:
    - Menyempurnakan komponen `FormInput.jsx` dengan penanganan atribut autofill browser dan validasi status field error server.
    - Memperbarui komponen pengaturan gateway pembayaran Xendit dan Ipaymu di tab Pengaturan Keuangan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #526  | Pemantauan ONT, Deteksi Gangguan Massal, Operasi Konfigurasi Tulis (Irisan A), serta Service-Port & VLAN OLT (Irisan B) | Menyelesaikan kapabilitas penuh pengelolaan fisik OLT Huawei: pemantauan otomatis 5 menit, korelasi gangguan massal kabel/listrik, operasi tulis terproteksi plan-execute (registrasi, hapus, reset, simpan), serta inventori dan provisi service-port dan VLAN. |
| #527  | Pembaruan Portal Mitra, Manajemen Dokumen, & Keamanan Kredensial | Modernisasi portal mitra dengan fitur unduh/hapus dokumen pelanggan, penyegaran tampilan detail akun PPPoE, sensor password sensitif, serta peningkatan form gateway keuangan. |

---

### Kemampuan Baru Pengguna/Admin

- **Manajemen Alokasi Service-Port & VLAN**: Admin jaringan dapat melihat seluruh VLAN yang dikonfigurasi di OLT, membuat VLAN baru, serta mengalokasikan service-port ke ONT pelanggan langsung dari antarmuka web Dekasimal.
- **Eksekusi Konfigurasi OLT Terkendali & Aman**: Admin NOC dapat mendaftarkan ONT baru dari tabel autofind, mengganti deskripsi ONT, melakukan reboot ONT, atau menghapus ONT dengan pratinjau perintah CLI dan konfirmasi wajib pengetikan nama OLT.
- **Deteksi Otomatis Gangguan Massal**: Admin menerima peringatan instan di antarmuka sistem dan Telegram ketika terjadi putusnya kabel fiber distribusi optik (LOS massal) atau pemadaman listrik massal (dying-gasp massal) pada suatu port PON.
- **Audit Trail Konfigurasi Tanpa Celah**: Setiap instruksi penulisan ke perangkat OLT tersimpan dalam rekaman audit append-only lengkap dengan teks perintah CLI dan balasan perangkat.
- **Pengelolaan Dokumen Lengkap di Portal Mitra**: Mitra bisnis kini dapat melihat, mengunduh, dan mengelola dokumen identitas/legal pelanggan mereka secara mandiri melalui portal mitra.

---

### Bug Fix / Solusi Masalah

- **Pencegahan Sesi CLI Menggantung di OLT Huawei**: Penanganan transisi mode interface GPON (`config-if-gpon`) yang fail-safe, memastikan kanal koneksi segera ditutup jika langkah navigasi sub-mode gagal.
- **Pencegahan Perubahan Konfigurasi Tidak Disengaja**: Mencegah salah sasaran OLT dengan mewajibkan verifikasi teks nama OLT sebelum perintah tulis dieksekusi oleh OLT Gateway.
- **Eliminasi Gangguan Semu / Outage Yatim**: Penyelarasan logika evaluasi gangguan massal dengan stempel waktu pemindaian terakhir OLT (`last_synced_at`), memastikan insiden ditutup secara akurat begitu seluruh ONT kembali online.
- **Pembersihan Komponen Usang Portal Mitra**: Menghilangkan rute dan komponen usang `broadbandProfile` di portal mitra yang berpotensi membingungkan pengguna mitra.

---

### Menu/Fitur Baru

- **Tab Manajemen VLAN OLT**: Tersedia pada menu detail OLT di `/network/olt` untuk pemantauan dan pembuatan VLAN fisik OLT.
- **Bagian Service-Port pada Drawer ONT**: Tabel alokasi service-port dan drawer pembuatan service-port baru pada detail ONT.
- **Drawer Operasi Konfigurasi OLT**: Antarmuka registrasi ONT, ubah deskripsi, simpan konfigurasi, dan tab riwayat audit.
- **Banner Gangguan Massal Jaringan**: Spanduk peringatan insiden optik aktif di bagian atas halaman monitoring ONT.
- **Endpoint Dokumen Pelanggan di Partner API**: Rute `/p-api/v1/customers/:id/documents` dan `/p-api/v1/customer-partners/:id/documents`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - Fitur **Alur Tulis Konfigurasi OLT (Plan-Execute)** adalah mekanisme keamanan mutlak untuk mencegah kesalahan manusia saat mengonfigurasi perangkat OLT fisik di jaringan produksi.
  - Setiap kali admin menekan tombol aksi (misal: Daftarkan ONT atau Buat Service-Port), sistem tidak langsung mengeksekusi ke perangkat. Sebaliknya, sistem masuk ke tahap **Plan** di mana perintah CLI Huawei yang valid disusun dan ditampilkan sebagai pratinjau.
  - Eksekusi hanya berlangsung pada tahap **Execute** setelah admin memasukkan nama OLT dengan benar, dan seluruh hasilnya dicatat secara permanen pada log audit sistem.

- **Langkah Penggunaan (Tutorial Mendaftarkan ONT & Service-Port Baru)**:
  1. **Buka Menu ONT**: Navigasikan ke **Jaringan > ONT**, lalu buka tab **Autofind**.
  2. **Pilih ONT Baru**: Temukan perangkat ONT yang baru terhubung ke port PON fisik, lalu klik tombol **Registrasi**.
  3. **Isi Konfigurasi Registrasi**:
     - Masukkan atau verifikasi Serial Number (SN) ONT.
     - Pilih Line Profile dan Service Profile yang sesuai.
     - Masukkan deskripsi nama pelanggan.
     - Klik **Lanjutkan ke Pratinjau**.
  4. **Konfirmasi & Eksekusi Perintah**:
     - Tinjau perintah CLI yang dirakit oleh sistem (contoh: `ont add 3 sn-auth "48575443..." omci ont-lineprofile-name "..."`).
     - Ketikkan nama perangkat OLT pada kolom konfirmasi untuk membuka kunci eksekusi.
     - Klik tombol **Eksekusi Konfigurasi**.
  5. **Tambahkan Service-Port**:
     - Buka detail ONT yang baru terdaftar, lalu pilih tab **Service-Port**.
     - Klik **Tambah Service-Port**.
     - Tentukan VLAN ID (misal: `100`), Gemport (`1`), dan User-VLAN (`100`).
     - Tinjau perintah CLI `service-port ...` dan lakukan konfirmasi nama OLT untuk menerapkan alokasi port layanan internet ke pelanggan.

