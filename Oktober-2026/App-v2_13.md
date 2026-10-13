# 📝 Daily Work Report - Dedy S.N Putra (2026-10-13)

---

## 📅 Laporan Harian - 13 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, perancangan modul penerbitan Surat Perintah Kerja (SPK) dari tiket layanan pelanggan, penguatan keandalan monitoring trafik jaringan dan filter anomali lonjakan data (*Spike Rejection Alert*), penyelarasan notifikasi bot Telegram topik gudang, penggabungan fitur lapor pajak langganan layanan, pemutakhiran berkas changelog rilis sistem, hingga rilis penggabungan produksi pada monorepo DEKASIMAL V2 per 13 Oktober 2026. Fokus dan pencapaian utama hari ini meliputi:
> 1. **Penerbitan & Pengelolaan SPK dari Tiket Layanan (`issue-spk-ticket`)**: Pembangunan modul ujung-ke-ujung (end-to-end) penerbitan Surat Perintah Kerja (SPK) lapangan langsung dari drawer tiket bantuan penanganan gangguan/permintaan pelanggan (`CreateTicketWorkOrderDrawer.jsx`). Fitur dilengkapi alokasi nomor sequence atomik berbasis tipe tiket dan mitra (`WO/{kode}/{seq}/{MM}/{YYYY}`), normalisasi dan sanitasi ketat payload teknisi/material, sinkronisasi otomatis timbal balik penanggung jawab tiket (`lead`) dengan tim teknisi SPK, pembatalan berantai saat tiket dibatalkan, penanganan isolasi log tanpa kebocoran PII, notifikasi pesan Telegram HTML-safe ke tim teknisi/lead, hingga dokumen lembar cetak resmi Surat Perintah Kerja berstandar format perusahaan (`TicketWorkOrderSheet.jsx` & `TicketWorkOrderDocument.jsx`).
> 2. **Penyesuaian Manual Kecepatan Interface & Deteksi Penolakan Lonjakan Trafik (`issue-408`)**: Penyempurnaan sistem monitoring trafik jaringan (`network-monitor` dan `networkTraffic`) melalui penambahan konfigurasi manual kecepatan antarmuka (`interface_speed`), penyetelan batas file time-series RRD secara dinamis, filter penolakan sampel anomali lonjakan data (*rate* > 2× kapasitas interface), lencana peringatan interaktif *Data Ditolak* (*Spike Rejection Alert*) pada kartu grafik monitoring, serta peringatan counter 32-bit SNMP untuk antarmuka berkecepatan tinggi.
> 3. **Penyelarasan Notifikasi Telegram Topik Gudang & Pengiriman Massal (`issue-397`)**: Peningkatan resolusi identifikasi obrolan Telegram untuk pesan topik spesifik modul inventaris gudang pada supergrup, eliminasi duplikasi pesan ke grup utama, dan penjaminan pengiriman notifikasi pembaruan data barang gudang secara kolektif.
> 4. **Penggabungan Fitur Lapor Pajak Layanan Pelanggan & Bisnis (`issue-394`)**: Penggabungan fitur seleksi dan penandaan status lapor pajak per layanan internet (broadband, dedicated, data access), aksi pembaruan massal pada tabel pelanggan, dan sinkronisasi filter data ke antarmuka integrasi aplikasi mitra.
> 5. **Pemutakhiran Dokumentasi & Riwayat Rilis Changelog Monorepo (`issue-385`)**: Pembaruan file indeks changelog dan penerbitan berkas riwayat rilis terstruktur untuk rilis versi v1.92.0 (`issue-384`), v1.93.0 (`issue-387`), v1.94.0 (`issue-394`), v1.94.1 (`issue-396`), dan v1.94.1 (`issue-397`).
> 6. **Pengujian Otomatis & Sinkronisasi Rilis Produksi**: Seluruh pengujian unit dan integrasi baru untuk siklus hidup SPK tiket (33 test di backend, 23 test di frontend) serta pengujian penyesuaian kecepatan trafik lulus 100%. Branch `issue-394`, `issue-397`, dan `issue-408` telah digabungkan ke `master` dan dideploy secara penuh ke branch `production`.

---

## 🌿 Branch: `issue-spk-ticket` — Penerbitan & Pengelolaan Surat Perintah Kerja (SPK) dari Tiket Layanan, Sinkronisasi Tim Lapangan, & Lembar Cetak Dokumen SPK

### 📌 Informasi Issue

- **Nomor Issue**: #SPK
- **Judul Issue**: Penerbitan & Pengelolaan Surat Perintah Kerja (SPK) dari Tiket Layanan, Sinkronisasi Lead Tim Lapangan, Notifikasi Telegram Terproteksi, & Lembar Cetak Dokumen SPK
- **Status Branch**: `Belum di-merge` (Branch aktif pada worktree `/home/dhedhy/Project/Dekasimal-V2-spk`)

### 📅 Rincian Commit

#### [[`ba1e0fb6`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: drawer terbit dan ubah SPK dari tiket - 13 Oktober 2026, 19:33:18 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/tickets/components/CreateTicketWorkOrderDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/tickets/components/CreateTicketWorkOrderDrawer.jsx) [NEW]:
    - Komponen drawer antarmuka responsif untuk menerbitkan SPK baru maupun mengedit data SPK tiket yang sedang berjalan.
    - Dilengkapi form dinamis: nama target/kategori sasaran, alamat lokasi pekerjaan, koordinat lokasi GPS, narahubung/PIC lapangan (nama & telepon), instruksi kerja terperinci, catatan pekerjaan, tabel input dinamis kebutuhan material (tambah baris barang, kuantitas, satuan, dan keterangan), serta daftar tim teknisi multi-pilih dengan penunjukan Lead teknisi.
  - [`frontend/src/app/pages/tickets/utils/spkTechnicians.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/tickets/utils/spkTechnicians.js) [NEW]:
    - Utilitas penyaring dan penata urutan teknisi lapangan, isolasi teknisi yang memiliki peran Lead ke urutan teratas, dan normalisasi struktur teknisi sebelum dikirimkan ke server.
  - [`frontend/src/app/pages/tickets/utils/spkTechnicians.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/tickets/utils/spkTechnicians.test.js) [NEW]:
    - Pengujian unit untuk fungsi manipulasi dan penataan urutan teknisi.
- **Deskripsi Perubahan & Fungsi**:
  - Membuka akses interaktif bagi staf operasional dan helpdesk untuk langsung menerbitkan atau merevisi surat tugas penanganan lapangan dari detail tiket, lengkap dengan rincian material dan tim pelaksana teknis.

#### [[`5eaedc54`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: dokumen cetak Surat Perintah Kerja untuk SPK tiket - 13 Oktober 2026, 19:29:33 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/services/workOrder/TicketWorkOrderDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/services/workOrder/TicketWorkOrderDocument.jsx) [NEW]:
    - Wrapper dokumen cetak Surat Perintah Kerja berbasis tiket.
  - [`frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.jsx`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.jsx) [NEW]:
    - Komponen template lembar cetak fisik formal Surat Perintah Kerja (SPK) berstandar perusahaan: kop surat resmi, barcode penomoran SPK, nomor referensi tiket, identitas pelanggan/sasaran tugas, alamat dan koordinat lokasi, PIC, instruksi kerja, tabel daftar kebutuhan material/perangkat (tanpa memunculkan harga atau data komersial), susunan tim teknisi pelaksana (Lead & Anggota), serta kolom tanda tangan pengesahan (Pemberi Tugas, Pelaksana Teknisi, dan Pelanggan/Penerima Pekerjaan).
  - [`frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.test.jsx) [NEW]:
    - Pengujian render lembar dokumen SPK tiket dan penjaminan bebas nilai finansial/harga.
  - [`frontend/src/components/shared/DocumentPreviewModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/components/shared/DocumentPreviewModal.jsx):
    - Penyesuaian modal pratinjau dokumen agar mendukung rendering tipe dokumen cetak SPK tiket secara instan.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan dokumen fisik siap cetak berformat PDF/kertas A4 resmi untuk dibawa oleh teknisi saat bertugas ke lokasi pelanggan atau perangkat jaringan.

#### [[`90259818`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: util form SPK tiket dan terjemahan - 13 Oktober 2026, 19:26:48 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/tickets/utils/ticketSpk.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/tickets/utils/ticketSpk.js) [NEW]:
    - Utilitas pemetaan payload form SPK tiket: konversi tipe tiket (`customer`, `partner`, `backbone`, `noc`, dsb.) ke kategori target SPK, pra-pengisian (*pre-fill*) otomatis nama sasaran, alamat, dan narahubung berdasarkan data tiket aktif.
  - [`frontend/src/app/pages/tickets/utils/ticketSpk.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/app/pages/tickets/utils/ticketSpk.test.js) [NEW]:
    - Pengujian unit untuk fungsi pemetaan data awal form dan validasi payload SPK.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2-spk/frontend/src/i18n/locales/id/translations.json):
    - Penambahan teks lokalisasi lengkap untuk modul SPK tiket (label formulir, peringatan pembatalan, validasi material, dan konfirmasi penerbitan).
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan antarmuka pengguna dwibahasa memiliki konsistensi istilah dan pengalaman pengisian formulir yang cepat berkat pra-pengisian data otomatis dari tiket.

#### [[`74be16f2`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: segarkan kamus privilege untuk endpoint SPK tiket - 13 Oktober 2026, 19:24:49 WIB

- **Komponen yang Berubah**:
  - [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/config/privilegeDictionary.json):
    - Pendaftaran kamus privilege untuk endpoint SPK tiket pada otorisasi RBAC sistem: pemetaan `workOrder.create`, `workOrder.update`, `workOrder.cancel` pada rute tiket.
- **Deskripsi Perubahan & Fungsi**:
  - Mengamankan seluruh rute API SPK tiket agar tunduk pada kontrol hak akses peran admin (*Role-Based Access Control*).

#### [[`0ad95dc7`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: tiket batal ikut membatalkan SPK; sinkron penanggung jawab SPK - 13 Oktober 2026, 19:14:40 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/ticket.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/controllers/ticket.controller.js):
    - Penambahan logika berantai (*cascading cancellation*): apabila status tiket dialihkan menjadi dibatalkan (`canceled`), sistem secara otomatis membatalkan SPK tiket terkait yang masih aktif dan mencatat timeline pembatalan.
    - Sinkronisasi penanggung jawab tiket saat ada pembaruan tiket agar lead teknisi pada SPK tetap selaras.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah timbulan SPK gantung (*orphan work order*) saat tiket dibatalkan oleh helpdesk atau pelanggan.

#### [[`e927cb4d`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: gerbang baca tiket pada batal/ubah SPK, presisi status balapan, perkuat test - 13 Oktober 2026, 19:12:45 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/workOrder.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/controllers/workOrder.controller.js):
    - Pengecekan gerbang privilege pembacaan tiket sebelum mengizinkan aksi ubah atau batal SPK tiket (`ticket.<type>.read`).
    - Penegakan kode status HTTP presisi: HTTP 409 Conflict saat status tiket atau SPK sedang bentrok/sudah dibatalkan pihak lain, dan HTTP 422 Unprocessable Entity saat pelanggaran aturan bisnis terjadi.
  - [`backend/test/integration/workOrderTicket.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.controller.test.js):
    - Penambahan pengujian verifikasi otorisasi silang dan pencegahan race condition.
- **Deskripsi Perubahan & Fungsi**:
  - Memperkokoh keamanan otorisasi controller dan kepatuhan standar kode status HTTP monorepo Dekasimal.

#### [[`b82bdb7f`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: endpoint terbit, batal, dan ubah SPK dari tiket - 13 Oktober 2026, 19:09:49 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/workOrder.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/controllers/workOrder.controller.js):
    - Handler `issueTicketWorkOrderHandler`, `cancelTicketWorkOrderHandler`, dan `updateTicketWorkOrderHandler` berbalut `asyncHandler`.
  - [`backend/src/routes/workOrder.route.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/routes/workOrder.route.js):
    - Rute API RESTful: `POST /ticket/:type/:ticketId/work-order`, `PATCH /ticket/:type/:ticketId/work-order`, dan `POST /ticket/:type/:ticketId/work-order/cancel`.
  - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/locales/id/translation.json):
    - Definisi pesan lokalisasi error dan sukses untuk endpoint SPK tiket.
  - [`backend/test/integration/workOrderTicket.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.controller.test.js) [NEW]:
    - Pengujian integrasi controller untuk alur penerbitan, pengubahan, dan pembatalan SPK tiket.
- **Deskripsi Perubahan & Fungsi**:
  - Mengekspos endpoint API backend resmi yang terdokumentasi dan terproteksi untuk operasi SPK tiket.

#### [[`c64416db`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: penerima notifikasi tim SPK dan pesan Telegram aman HTML - 13 Oktober 2026, 19:06:40 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/notification.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/notification.service.js):
    - Resolusi penerima notifikasi: mengumpulkan seluruh anggota tim teknisi dan penanggung jawab yang memiliki akun Telegram aktif terdaftar.
  - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/utils/telegram.js) & [`backend/src/utils/workOrderTelegramMessage.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/utils/workOrderTelegramMessage.js) [NEW]:
    - Pembangun pesan notifikasi Telegram berbasis template HTML dengan sanitasi escape karakter khusus (`escapeHtml`) agar format pesan tidak rusak akibat karakter input pengguna.
  - [`backend/test/unit/workOrderRecipients.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/unit/workOrderRecipients.test.js) [NEW] & [`backend/test/unit/workOrderTelegramMessage.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/unit/workOrderTelegramMessage.test.js) [NEW]:
    - Pengujian unit resolusi penerima dan pembentukan template notifikasi Telegram.
- **Deskripsi Perubahan & Fungsi**:
  - Mengirimkan ringkasan tugas penugasan lapangan secara instan ke bot Telegram teknisi sesaat setelah SPK diterbitkan atau diubah.

#### [[`da83d847`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: perbaiki sinkron lead (admin tak ditemukan, SPK selesai), telegram_id, $unset kosong - 13 Oktober 2026, 19:04:13 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/workOrder.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/workOrder.service.js):
    - Penanganan kasus tepi saat data admin lead tidak ditemukan di database, penguncian agar SPK yang sudah berstatus selesai (`completed`) tidak ditimpa oleh sinkronisasi lead tiket, penanganan operator `$unset` Mongoose yang aman, dan resolusi ID Telegram staf teknisi.
  - [`backend/test/integration/workOrderTicket.lifecycle.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.lifecycle.test.js):
    - Pengujian skenario tepi sinkronisasi lead.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan kestabilan transaksi basis data Mongoose dan mencegah kegagalan operasi akibat data referensi tidak lengkap.

#### [[`d84697b9`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: batal, sinkron lead, ubah SPK tiket, statistik tanpa SPK tiket - 13 Oktober 2026, 19:01:54 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/ticket.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/ticket.service.js):
    - Penyesuaian pembaruan status tiket dan sinkronisasi referensi SPK.
  - [`backend/src/services/workOrder.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/workOrder.service.js):
    - Implementasi fungsi `cancelTicketWorkOrder`, `updateTicketWorkOrder`, dan `syncTicketLeadToWorkOrder`.
    - Isolasi kueri agregasi statistik Work Order reguler (`origin !== 'ticket'`) agar metrik instalasi/survey tidak terdistorsi oleh pekerjaan insidental tiket.
  - [`backend/test/integration/workOrderTicket.lifecycle.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.lifecycle.test.js) [NEW]:
    - Pengujian siklus hidup lengkap SPK tiket dari terbit, perubahan rincian tugas, hingga pembatalan.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan fungsionalitas manajemen perubahan data SPK tiket secara penuh dan menjaga keakuratan metrik statistik WO perusahaan.

#### [[`3da6c9cf`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: perbaiki klaim SPK tiket (interleaving, retry nomor, log tanpa PII) - 13 Oktober 2026, 18:58:35 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/workOrder.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/workOrder.service.js):
    - Penanganan tabrakan sequence nomor urut SPK dengan skema retry otomatis saat terjadi duplikasi kunci (`E11000`).
    - Penjaminan kepatuhan *zero-secrets/zero-PII* logging: informasi pribadi pelanggan (nomor HP PIC, nama PIC, alamat detil) dibersihkan dari log sebelum pencatatan Winston.
  - [`backend/test/integration/workOrderTicket.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.service.test.js):
    - Pengujian simulasi race condition nomor urut SPK dan verifikasi payload log.
- **Deskripsi Perubahan & Fungsi**:
  - Menghindari kegagalan transaksi penerbitan SPK pada trafik tinggi dan melindungi kerahasiaan data pribadi pelanggan sesuai panduan AGENTS.md.

#### [[`8890c77c`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: service penerbitan SPK dari tiket dengan klaim atomik - 13 Oktober 2026, 18:54:34 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/workOrder.service.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/services/workOrder.service.js):
    - Fungsi inti `issueTicketWorkOrder`: klaim atomik bersyarat pada dokumen Tiket (`work_order: { $exists: false }`, status tiket belum dibatalkan/ditutup).
    - Menghasilkan dokumen SPK baru, mengisi `ticket.work_order`, dan mencatat riwayat pembaruan tiket.
  - [`backend/test/integration/workOrderTicket.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicket.service.test.js) [NEW]:
    - Pengujian integrasi service penerbitan SPK, klaim atomik ganda, dan alokasi sequence.
- **Deskripsi Perubahan & Fungsi**:
  - Menjamin satu tiket gangguan/permintaan hanya dapat memiliki tepat satu SPK aktif, mengeliminasi risiko duplikasi surat perintah kerja.

#### [[`e9f786d9`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: helper murni SPK tiket (nomor, normalisasi, lead) - 13 Oktober 2026, 18:51:15 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/ticketWorkOrder.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/utils/ticketWorkOrder.js) [NEW]:
    - Modul fungsi murni (pure functions) pembantu:
      - `buildTicketWorkOrderNumber`: pembentuk nomor SPK standar (`WO/{kode}/{seq}/{MM}/{YYYY}`).
      - `normalizeTicketWorkOrderInput`: sanitasi input ketat (memangkas string, membatasi panjang teks, memvalidasi format koordinat GPS, membatasi maksimal 50 material dan 20 teknisi, membuang field terlarang seperti harga atau created_by).
      - `applyLeadToTeam`: penjaminan keberadaan tepat satu peran `lead` di dalam tim teknisi.
  - [`backend/test/unit/ticketWorkOrder.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/unit/ticketWorkOrder.test.js) [NEW]:
    - 15 pengujian unit untuk seluruh kasus normalisasi input, penomoran, dan manajemen peran tim teknisi.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan fondasi logika bisnis yang terisolasi, mudah diuji, dan aman dari manipulasi payload klien.

#### [[`4673caa1`](file:///home/dhedhy/Project/Dekasimal-V2-spk)] - save #SPK: skema WorkOrder untuk SPK dari tiket - 13 Oktober 2026, 18:49:31 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/workOrder.model.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/src/models/workOrder.model.js):
    - Penambahan dukungan field pada skema Mongoose: `origin: 'ticket'`, relasi `ticket` (ObjectId ke ref Ticket), `ticket_type`, `target_category`, array `materials` (nama, qty, unit, notes), array `technicians` (admin ObjectId, role `lead`|`member`, snapshot nama, posisi, divisi), dan indeks unik parsial `ticket_unique` (`origin: 'ticket'`, `status: { $ne: 'canceled' }`).
  - [`backend/test/integration/workOrderTicketModel.test.js`](file:///home/dhedhy/Project/Dekasimal-V2-spk/backend/test/integration/workOrderTicketModel.test.js) [NEW]:
    - Pengujian integritas model schema, indeks unik parsial, dan validasi tipe field.
- **Deskripsi Perubahan & Fungsi**:
  - Mempersiapkan struktur basis data MongoDB untuk menampung seluruh atribut operasional SPK yang diterbitkan dari tiket layanan.

---

## 🌿 Branch: `issue-408` — Penyesuaian Manual Kecepatan Interface Monitoring Trafik Jaringan (`interface_speed`), Deteksi Penolakan Lonjakan Trafik (*Spike Rejection Alert*), dan Penanganan Counter 32-bit SNMP

### 📌 Informasi Issue

- **Nomor Issue**: #408
- **Judul Issue**: Penyesuaian Manual Kecepatan Interface Monitoring Trafik Jaringan (`interface_speed`), Deteksi Penolakan Lonjakan Trafik (*Spike Rejection Alert*), dan Penanganan Counter 32-bit SNMP
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`74ee7339`](file:///home/dhedhy/Project/Dekasimal-V2) dan dideploy ke branch `production` via commit [`98ae704b`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`74ee7339`](file:///home/dhedhy/Project/Dekasimal-V2)] & [[`81a3842d`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #408 - 13 Oktober 2026, 18:43:45 / 18:44:22 WIB

- **Komponen yang Berubah**:
  - **Model & Layanan Backend (`backend/src/`)**:
    - [`backend/src/models/networkTrafficTarget.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/networkTrafficTarget.model.js):
      - Penambahan field schema `interface_speed` (angka dalam satuan bps, divalidasi antara 1 Mbps hingga 10 Tbps), `spike_rejected_at` (stempel waktu penolakan lonjakan anomali terakhir), dan `spike_rejected_bps` (kecepatan rate bps anomali yang ditolak).
    - [`backend/src/services/networkTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkTraffic.service.js):
      - Fungsi `updateTrafficTarget`: validasi nilai `interface_speed`, pembersihan stempel `spike_rejected_at` dan `spike_rejected_bps` saat kecepatan diperbarui secara manual, serta pemanggilan penyesuaian kapasitas file time-series RRD (*tune RRD max rate*) pada microservice network-monitor.
    - [`backend/src/services/recovery.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/recovery.service.js) & [`backend/src/routes/networkTraffic.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkTraffic.route.js):
      - Penyesuaian rute dan layanan pemulihan perangkat jaringan untuk mempertahankan pengaturan kecepatan antarmuka.
    - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json):
      - Terjemahan validasi kecepatan interface antarmuka jaringan.
  - **Microservice Monitoring Jaringan (`network-monitor/`)**:
    - [`network-monitor/src/services/trafficPoller.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/trafficPoller.service.js):
      - Filter penolakan sampel anomali lonjakan data (*spike rejection*): membuang data point sampel polling bila *rate* trafik melebihi 2× `interface_speed` agar grafik time-series tidak mendatar/rusak skalanya, serta mencatat `spike_rejected_at` dan `spike_rejected_bps` pada target terkait.
    - [`network-monitor/test/trafficPoller.rate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/test/trafficPoller.rate.test.js):
      - Pengujian unit untuk logika filter spike poller.
  - **Antarmuka Pengguna & Komponen Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/network/traffic/components/TrafficTargetEditModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/TrafficTargetEditModal.jsx) & [`frontend/src/app/pages/network/traffic/schema/editSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/schema/editSchema.js):
      - Penambahan input form *Kecepatan Interface (Mbps)* dengan petunjuk konfigurasi untuk antarmuka bundel/LAG (total kapasitas agregat) serta validasi rentang angka 1 s/d 10.000.000 Mbps.
    - [`frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx):
      - Penambahan lencana peringatan dinamis:
        - *Data Ditolak (Spike Rejection Alert)*: Menampilkan peringatan oranye bila ada sampel trafik yang baru saja ditolak dalam jendela 3 jam terakhir, memberi tahu administrator bahwa kapasitas kecepatan interface kemungkinan disetel terlalu rendah sehingga data sah terbuang.
        - *Counter 32-bit*: Indikator peringatan pada interface berkecepatan tinggi yang hanya mendukung counter SNMP 32-bit (rawan counter rollover/wrap pada trafik > 114 Mbps).
    - [`frontend/src/app/pages/network/traffic/utils/spikeRejection.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/utils/spikeRejection.js) [NEW] & [`frontend/src/app/pages/network/traffic/utils/spikeRejection.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/utils/spikeRejection.test.js) [NEW]:
      - Utilitas evaluasi jendela aktif penolakan lonjakan data (`SPIKE_REJECTION_WINDOW_MS = 3 jam`).
    - [`frontend/src/app/pages/services/dataAccess/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/dataAccess/detail.jsx) & [`frontend/src/app/pages/services/dedicatedInternet/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/dedicatedInternet/detail.jsx):
      - Penyesuaian tampilan monitoring trafik pada rincian layanan internet dedicated dan data access.
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
      - Penambahan kamus teks informasi kecepatan interface, lencana peringatan lonjakan data, dan peringatan counter 32-bit.
  - **Pengujian Otomatis (`backend/test/`)**:
    - [`backend/test/integration/trafficInterfaceSpeedUpdate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/trafficInterfaceSpeedUpdate.test.js) [NEW]
    - [`backend/test/integration/trafficServiceLookup.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/trafficServiceLookup.test.js) [NEW]
    - [`backend/test/unit/buildPollResultFields.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/buildPollResultFields.test.js) [NEW]
    - [`backend/test/integration/trafficRecovery.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/trafficRecovery.test.js)
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan fleksibilitas penuh kepada admin jaringan untuk menyesuaikan kapasitas interface (terutama antarmuka agregat LACP/LAG yang tidak terdeteksi otomatis oleh SNMP), sekaligus menjaga grafik RRD bebas dari anomali lonjakan palsu dan memberi peringatan proaktif bila sampel data terbuang.

---

## 🌿 Branch: `issue-385` — Pemutakhiran Dokumentasi & Riwayat Rilis Changelog Monorepo (v1.92.0 - v1.94.1)

### 📌 Informasi Issue

- **Nomor Issue**: #385
- **Judul Issue**: Pemutakhiran Indeks Changelog Sistem dan Dokumentasi Riwayat Rilis Fitur Baru
- **Status Branch**: `Belum di-merge` (Branch dokumentasi aktif)

### 📅 Rincian Commit

#### [[`1bb8e106`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 13 Oktober 2026, 13:44:19 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/warehouseItem.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/warehouseItem.controller.js):
    - Pembersihan baris instruksi yang tidak terpakai.
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json):
    - Penambahan metadata 5 rilis terbaru ke dalam indeks riwayat perubahan aplikasi monorepo.
  - [`backend/src/data/changelog/releases/issue-384.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-384.json) [NEW]:
    - Dokumentasi rilis v1.92.0: Modul Arsip Surat Keluar & Pengelolaan Dokumen Terunggah.
  - [`backend/src/data/changelog/releases/issue-387.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-387.json) [NEW]:
    - Dokumentasi rilis v1.93.0: Rekap Harian Obrolan WhatsApp & Analisis KPI Layanan.
  - [`backend/src/data/changelog/releases/issue-394.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-394.json) [NEW]:
    - Dokumentasi rilis v1.94.0: Fitur Penandaan Lapor Pajak Layanan Pelanggan & Bisnis.
  - [`backend/src/data/changelog/releases/issue-396.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-396.json) [NEW]:
    - Dokumentasi rilis v1.94.1: Penegakan Hak Akses Akun Pembayaran Sisi Server & Datatables Mode Client-Side.
  - [`backend/src/data/changelog/releases/issue-397.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-397.json) [NEW]:
    - Dokumentasi rilis v1.94.1: Penyelarasan Notifikasi Telegram Topik Gudang & Pengiriman Pembaruan Barang Kolektif.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan modal *What's New* dan dokumentasi audit monorepo mencerminkan seluruh rilis fitur, perbaikan bug, dan peningkatan performa yang telah diselesaikan.

---

## 🌿 Branch: `issue-397` — Penyelarasan Notifikasi Telegram Topik Gudang & Pengiriman Pembaruan Barang Kolektif

### 📌 Informasi Issue

- **Nomor Issue**: #397
- **Judul Issue**: Penyelarasan Notifikasi Telegram Topik Gudang & Pengiriman Pembaruan Barang Kolektif
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`ea19d046`](file:///home/dhedhy/Project/Dekasimal-V2) dan dideploy ke branch `production` via commit [`0935d5cc`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`ea19d046`](file:///home/dhedhy/Project/Dekasimal-V2)] & [[`b0a351f5`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #397 - 13 Oktober 2026, 13:34:29 / 13:35:38 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/warehouseItem.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/warehouseItem.controller.js):
    - Penanganan pengiriman notifikasi pembaruan data barang gudang secara kolektif saat terjadi perubahan stok atau mutasi barang.
  - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js) & [`backend/src/services/warehouseReport.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/warehouseReport.service.js):
    - Penyesuaian opsi preferensi notifikasi gudang.
  - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js):
    - Penyempurnaan resolusi identifikasi tujuan notifikasi: mencegah pengiriman ganda ke obrolan utama apabila pesan sudah dialihkan ke topik spesifik di dalam grup supergrup Telegram.
  - [`backend/test/integration/telegramTopicRegistration.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/telegramTopicRegistration.test.js) & [`backend/test/unit/ticketTelegramClose.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/ticketTelegramClose.test.js):
    - Pengujian registrasi topik notifikasi Telegram dan pencegahan duplikasi pengiriman.
  - [`frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx):
    - Penyesuaian label antarmuka dan opsi pemilihan grup/topik Telegram untuk modul gudang.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
    - Terjemahan label pengaturan notifikasi Telegram gudang.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan informasi mutasi dan penambahan stok barang terkirim ke topik Telegram gudang yang tepat tanpa membanjiri obrolan utama grup staf.

---

## 🌿 Branch: `issue-394` — Fitur Penandaan Lapor Pajak Layanan Pelanggan & Bisnis serta Sinkronisasi Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #394
- **Judul Issue**: Fitur Seleksi & Penandaan Status Lapor Pajak Layanan Internet Pelanggan dan Pelanggan Bisnis
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`1d8ad60e`](file:///home/dhedhy/Project/Dekasimal-V2) dan dideploy ke branch `production` via commit [`0935d5cc`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`1d8ad60e`](file:///home/dhedhy/Project/Dekasimal-V2)] & [[`d2059c8b`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #394 - 13 Oktober 2026, 13:32:09 / 13:33:35 WIB

- **Komponen yang Berubah**:
  - Penggabungan perubahan modul penandaan status lapor pajak per layanan (Internet Broadband, Dedicated Internet, Data Access) ke cabang `master`.
  - Integrasi kartu lapor pajak formulir pelanggan, aksi pembaruan lapor pajak massal (*bulk tax report status*), dan penyelarasan sinkronisasi API mitra.
- **Deskripsi Perubahan & Fungsi**:
  - Membantu tim keuangan dan akuntansi mengkategorikan layanan pelanggan yang telah memenuhi syarat pelaporan faktur pajak secara cepat dan akurat.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #SPK  | Penerbitan & Pengelolaan SPK dari Tiket Layanan | Helpdesk/operasional dapat menerbitkan surat perintah kerja resmi lapangan langsung dari drawer tiket, menugaskan tim teknisi dengan penetapan Lead, merinci kebutuhan material, mengirim notifikasi instan ke Telegram teknisi, dan mencetak dokumen SPK PDF formal. |
| #408  | Penyesuaian Manual Kecepatan Interface & Deteksi Spike | Administrator jaringan dapat menyesuaikan kapasitas interface secara manual (terutama bundel LAG), grafik terproteksi dari lonjakan anomali palsu, dan terdapat peringatan visual bila data sah terbuang akibat batas terlalu kecil. |
| #385  | Pemutakhiran Dokumentasi Changelog Monorepo | Seluruh pembaruan rilis v1.92.0 hingga v1.94.1 terdata rapi pada berkas changelog terstruktur untuk modal *What's New* dan audit sistem. |
| #397  | Penyelarasan Notifikasi Telegram Topik Gudang | Notifikasi pergerakan stok barang terkirim akurat ke topik khusus supergrup tanpa duplikasi pesan di ruang obrolan utama. |
| #394  | Fitur Penandaan Lapor Pajak Layanan | Mempermudah penandaan status siap lapor pajak untuk seluruh ragam layanan internet pelanggan dan bisnis, baik satuan maupun massal. |

### Kemampuan Baru Pengguna/Admin

- **Menerbitkan SPK dari Tiket Langsung**: Admin helpdesk atau teknis dapat mengklik tombol terbit SPK pada drawer rincian tiket untuk membuka `CreateTicketWorkOrderDrawer`, memilih sasaran pekerjaan, narahubung lapangan, menyusun rincian kebutuhan material, dan menunjuk teknisi pelaksana dengan penunjukan tepat satu Leader.
- **Cetak Lembar Dokumen SPK Fisik**: Admin dan teknisi dapat melakukan pratinjau dan mengunduh berkas PDF lembar Surat Perintah Kerja formal berstandar perusahaan yang memuat nomor resmi, instruksi, material, dan kolom tanda tangan pengesahan tiga pihak.
- **Atur Kecepatan Interface Monitoring Trafik Manual**: Admin jaringan dapat membuka modal edit target monitoring untuk mengatur kapasitas Mbps interface secara akurat, berguna untuk port LAG/LACP agregat yang kapasitasnya melebihi satu antarmuka fisik.
- **Notifikasi Telegram Otomatis SPK**: Tim teknisi lapangan yang ditugaskan otomatis menerima pesan notifikasi Telegram berisi detail tugas segera setelah SPK diterbitkan atau diperbarui.

### Bug Fix / Solusi Masalah

- **Pencegahan SPK Ganda & Balapan Data**: Penerbitan SPK dari tiket kini menggunakan kueri atomik bersyarat dan mekanisme auto-retry penomoran sequence, mengeliminasi risiko duplikasi surat perintah kerja saat dua admin menekan tombol bersamaan.
- **Pembersihan SPK Gantung saat Tiket Batal**: Tiket yang dibatalkan statusnya oleh helpdesk secara otomatis membatalkan SPK terkait yang masih aktif.
- **Grafik Monitoring Terdistorsi Lonjakan Palsu**: Poller trafik kini membuang sampel anomali (> 2× kecepatan interface) sehingga grafik RRD tidak mendatar akibat satu data point ekstrem.
- **Deteksi Data Trafik Terbuang**: Sistem kini menampilkan lencana oranye *Data Ditolak* jika ada data point sah yang terpotong oleh filter lonjakan akibat konfigurasi interface speed yang terlalu kecil.
- **Notifikasi Telegram Ganda di Supergrup**: Menghilangkan duplikasi pengiriman pesan ke obrolan utama saat notifikasi topik gudang diaktifkan.

### Menu/Fitur Baru

- **Drawer & Modal Terbit SPK Tiket**: `CreateTicketWorkOrderDrawer.jsx` terintegrasi pada modul Layanan Bantuan / Tiket.
- **Komponen Lembar Cetak SPK Tiket**: `TicketWorkOrderSheet.jsx` pada menu Layanan > Work Order.
- **Input Kecepatan Interface & Indikator Spike/Counter 32-bit**: Form edit target dan kartu grafik pada modul Jaringan > Monitoring Trafik (`/network/traffic`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penerbitan & Pengelolaan Surat Perintah Kerja (SPK) dari Tiket Layanan

- **Penjelasan Fitur**:
  Fitur ini menghubungkan modul Tiket Bantuan Pelanggan dengan modul Surat Perintah Kerja (Work Order). Ketika penanganan tiket memerlukan kunjungan lapangan (misalnya pengecekan redaman kabel FO, perbaikan koneksi, penggantian modem/ONT, atau inspeksi perangkat), admin tidak perlu lagi membuat Work Order terpisah secara manual. Cukup terbitkan SPK langsung dari tiket terkait; sistem akan mengalokasikan nomor urut resmi `WO/{kode}/{seq}/{MM}/{YYYY}`, menyalin identitas lokasi dan PIC, menetapkan tim teknisi lapangan, mengirim ringkasan tugas ke bot Telegram teknisi, serta menyediakan dokumen cetak SPK siap tanda tangan.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Layanan Bantuan > Tiket** (`/tickets`), lalu buka rincian salah satu tiket gangguan atau permintaan yang memerlukan tindakan teknisi lapangan.
  2. Klik tombol **Terbitkan SPK** untuk membuka drawer `CreateTicketWorkOrderDrawer`.
  3. Periksa data target sasaran, alamat lokasi, dan kontak PIC lapangan yang sudah terisi otomatis dari data tiket.
  4. Isi **Instruksi Kerja** dan target tanggal penyelesaian pekerjaan.
  5. Pada bagian **Kebutuhan Material**, klik *Tambah Material* jika teknisi membutuhkan material dari gudang (masukkan nama barang, jumlah, satuan, dan keterangan).
  6. Pada bagian **Tim Teknisi**, centang teknisi yang ditugaskan ke lokasi, lalu pilih salah satu teknisi sebagai **Lead / Penanggung Jawab**.
  7. Klik tombol **Terbitkan SPK**. Sistem akan memvalidasi data, mengalokasikan nomor SPK resmi, dan mengirimkan notifikasi Telegram ke tim teknisi yang bertugas.
  8. Untuk mencetak berkas fisik, klik tombol **Cetak SPK** pada kartu informasi SPK di tiket untuk membuka pratinjau dokumen dan mengunduh versi cetak/PDF formal.

### 2. Pengaturan Kecepatan Interface & Penanganan Lonjakan Monitoring Trafik

- **Penjelasan Fitur**:
  Monitoring grafik antarmuka jaringan SNMP terkadang mengalami anomali nilai trafik palsu ekstrem (misal lonjakan puluhan Gbps saat router reboot atau counter wrap), yang merusak skala grafik dan membuatnya tampak gepeng di bagian bawah. Fitur ini memungkinkan admin menentukan kapasitas kecepatan interface secara manual (`interface_speed`). Nilai ini menjadi batas filter: seluruh sampel trafik di atas 2× kecepatan interface akan ditolak secara aman, dan jika penolakan terjadi, kartu grafik akan memunculkan lencana peringatan *Data Ditolak* agar admin menyadari perlunya penyesuaian nilai kapasitas.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan > Monitoring Trafik** (`/network/traffic`).
  2. Pilih perangkat atau target interface yang ingin diatur (misalnya antarmuka uplink atau bundel LACP/LAG).
  3. Klik ikon pengaturan (*gear*) atau menu titik tiga pada kartu grafik, lalu pilih **Edit Target & Grafik**.
  4. Pada kolom **Kecepatan Interface (Mbps)**, masukkan kapasitas total antarmuka (contoh: isi `2000` untuk bundel 2× 1 Gbps, atau `10000` untuk port 10G SFP+).
  5. Klik tombol **Terapkan Pengaturan**. Sistem akan memperbarui database, menyetel ulang batas toleransi file RRD pada background service network-monitor, dan membersihkan penanda penolakan lama.
  6. Bila kartu grafik menampilkan lencana oranye *Data Ditolak*, arahkan kursor ke lencana tersebut untuk membaca rincian *rate* trafik yang ditolak dan segera sesuaikan nilai kecepatan interface ke kapasitas yang lebih tinggi.
