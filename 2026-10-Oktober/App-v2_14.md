# 📝 Daily Work Report - Dedy S.N Putra (2026-10-14)

---

## 📅 Laporan Harian - 14 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, perancangan modul manajemen tiket terbuka pada Telegram Mini App, optimasi kuota dan frekuensi polling gateway pembayaran daring (*Finance Gateway Call Meter & Sweeper*), standardisasi dialog konfirmasi aktivasi/nonaktivasi data tabel, rilis akhir penerbitan Surat Perintah Kerja (SPK) tiket dan penyelarasan error handler, hingga pemutakhiran riwayat changelog dan sinkronisasi rilis produksi pada monorepo DEKASIMAL V2 per 14 Oktober 2026. Fokus dan pencapaian utama hari ini meliputi:
> 1. **Modul Tiket Terbuka Telegram Mini App (`issue-347`)**: Pembangunan antarmuka manajemen tiket bantuan dan operasional lapangan berbasis Telegram Mini App (`TicketList.jsx`, `TicketCard.jsx`, `TicketDetail.jsx`). Menyediakan endpoint backend teroptimasi (`POST /ticket/mobile/list`), fitur *Smart Pinning* otomatis untuk tiket yang ditugaskan ke teknisi login (`is_assigned`) atau pernah direspons (`is_replied`), kemampuan pin manual berbasis penyimpanan lokal per admin, pencarian responsif (*debounced*), filter jenis tiket, visualisasi prioritas/kendala, timeline komentar percakapan tiket, form penambahan pesan/komentar instan dari ponsel teknisi, hingga mekanisme pencegahan *infinite alert loop* pada transport Telegram Logger.
> 2. **Penghematan Pengecekan Pembayaran Gateway & Rate-Limit Guard (`issue-411`)**: Rekayasa ulang alur verifikasi transaksi pembayaran iPaymu (`gatewayCallMeter.js`, `financeGateway.service.js`). Mengimplementasikan *sliding-window call meter* berbasis Redis untuk membatasi frekuensi panggilan API agar terhindar dari pemblokiran IP oleh provider payment gateway, penjadwalan verifikasi transaksi menggantung secara bertahap (*exponential/staggered backoff*), penundaan otomatis saat terdeteksi *rate-limiting*, pengecekan transaksi massal (*bulk query*), serta pelonggaran interval cron sweeper cadangan menjadi 15 menit.
> 3. **Aksi Aktif/Nonaktif Terkonfirmasi pada Menu Baris Tabel (`issue-405`)**: Standardisasi mekanisme perubahan status data master (pengguna, pelanggan, mitra, karyawan, layanan broadband, hotspot, vendor, akun finansial, dsb.) dengan menghapus saklar toggle langsung yang rentan tersentuh tanpa sengaja di perangkat mobile/layar sentuh, menggantikannya dengan aksi menu baris berdialog konfirmasi eksplisit yang aman dan tersinkronisasi status server terkini.
> 4. **Penyempurnaan Modul SPK Tiket & Pencegahan Error Logging Loop (`issue-385`)**: Penggabungan penuh alur penerbitan SPK langsung dari detail tiket, tanda tangan digital penerbit, lembar cetak dokumen fisik berstandar resmi perusahaan (`TicketWorkOrderSheet.jsx`), penyesuaian logging sistem agar status validasi (400, 404, 422) tidak memicu spam notifikasi error kritis Telegram, dan penambahan kartu sinkronisasi Waktu Server pada menu pengaturan developer.
> 5. **Pemutakhiran Dokumentasi Changelog & Sinkronisasi Rilis Produksi (`issue-415`)**: Penyusunan berkas changelog rilis terstruktur untuk versi v1.94.2, v1.94.3, v1.95.0, dan v1.95.1, penggabungan ke branch `master`, serta pembaruan langsung ke branch `production`.

---

## 🌿 Branch: `issue-347` — Modul Tiket Terbuka pada Telegram Mini App, Smart Pinning Teknisi, Timeline Komentar, & Backend Mobile API

### 📌 Informasi Issue

- **Nomor Issue**: #347
- **Judul Issue**: Modul Manajemen Tiket Terbuka pada Telegram Mini App, Smart Pinning Berbasis Penugasan & Respon, Riwayat Komentar, Form Tambah Pesan Mobile, dan Isolasi Log Notifikasi Telegram
- **Status Branch**: `Belum di-merge` (Branch aktif pada direktori utama `/home/dhedhy/Project/Dekasimal-V2`)

### 📅 Rincian Commit & Perubahan Staging

#### [[`99a89110`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps)] - resolve #347 - 14 Oktober 2026, 19:08:34 WIB

- **Komponen yang Berubah**:
  - [`telegram-apps/src/pages/tickets/TicketList.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketList.jsx) [NEW]:
    - Halaman utama daftar tiket terbuka untuk teknisi lapangan dan staf helpdesk.
    - Dilengkapi bilah pencarian responsif (nomor tiket, nama pelanggan, alamat, atau nomor telepon), filter cepat per kategori tiket (*Semua, Pelanggan, Survei, Pasang Baru, Mitra, Pembongkaran, Pembayaran, Backbone, Lainnya*), dan indikator jumlah tiket aktif.
    - Mengintegrasikan mekanisme pengurutan tiket: tiket yang disematkan (*pinned*) berada di posisi paling atas dengan pembatas visual yang jelas, diikuti tiket reguler berdasar tingkat prioritas dan tanggal laporan.
  - [`telegram-apps/src/pages/tickets/TicketCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketCard.jsx) [NEW]:
    - Komponen kartu ringkasan tiket berdesain modern dengan dukungan mode gelap (*dark mode*).
    - Menampilkan lencana ID tiket, label tipe tiket berwarna, lencana prioritas dinamis (Kritis, Tinggi, Sedang, Rendah), status pending/tunda, nama pelanggan/mitra, alamat lokasi penanganan, ringkasan cuplikan komentar terbaru, hitungan pesan, serta tombol pin interaktif dengan *haptic feedback* Telegram WebApp.
  - [`telegram-apps/src/pages/tickets/TicketDetail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketDetail.jsx) [NEW]:
    - Halaman rincian tiket lengkap: identitas pemohon, nomor kontak telepon (dengan tautan panggilan langsung `tel:`), tautan rute lokasi peta Google Maps dari koordinat GPS, deskripsi kendala teknis, status teknisi penanggung jawab (*assignment*), dan status pending.
    - Menampilkan linimasa obrolan/komentar internal tiket secara kronologis dengan pemisahan visual yang jelas antara pesan admin, sistem, dan teknisi.
    - Menyediakan form input komentar cepat di bagian bawah halaman untuk mengirim tanggapan/progres penanganan langsung dari Telegram Mini App tanpa harus membuka dashboard web desktop.
  - [`telegram-apps/src/utils/pinnedTickets.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/utils/pinnedTickets.js) [NEW]:
    - Modul utilitas penyematan tiket cerdas (*Smart Pinning*).
    - Menyimpan daftar pin manual di `localStorage` per ID admin teknisi (`pinned_tickets_${adminId}`), serta menghitung status pin otomatis apabila tiket telah ditugaskan ke akun teknisi bersangkutan (`is_assigned`) atau pernah dibalas oleh teknisi tersebut (`is_replied`).
  - [`telegram-apps/src/utils/ticketConstants.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/utils/ticketConstants.js) [NEW]:
    - Definisi skema warna lencana, label bahasa Indonesia yang ramah pengguna, dan ikon representatif untuk setiap kategori tiket dan tingkat urgensi prioritas.
  - [`telegram-apps/src/pages/Home.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/Home.jsx):
    - Penambahan kartu menu navigasi baru "Daftar Tiket Terbuka" pada beranda Telegram Mini App dengan proteksi hak akses *multi-privilege*.
  - [`telegram-apps/src/routes/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/routes/index.jsx):
    - Pendaftaran rute navigasi `/tickets/list` dan `/tickets/detail/:type/:id`.
  - [`telegram-apps/src/utils/hasPrivilege.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/utils/hasPrivilege.js):
    - Peningkatan parser otorisasi hak akses agar mendukung evaluasi logika OR pemisah pipa (`|`).
  - [`backend/src/controllers/ticket.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/ticket.controller.js):
    - Penambahan controller `getMobileOpenTicketsHandler` dibungkus `asyncHandler` untuk menyajikan data tiket terbuka khusus kebutuhan aplikasi mobile Telegram.
  - [`backend/src/routes/ticket.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/ticket.route.js):
    - Pendaftaran rute API `POST /ticket/mobile/list` dengan middleware autentikasi JWT dan proteksi hak akses `ticketCustomer.list|...`.
  - [`backend/src/services/ticket.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/ticket.service.js):
    - Pembangunan fungsi data access `findMobileOpenTickets(params, userId)` yang efisien: memfilter tiket yang belum tuntas (`complete: { $ne: true }`), belum dibatalkan, dan belum dihapus; memproyeksikan hanya kolom data esensial untuk menghemat bandwidth; mempopulasikan relasi teknisi dan pelanggan; serta memperkaya payload dengan flag `is_assigned`, `is_replied`, hitungan pesan, dan pesan terakhir yang telah dibersihkan dari tag HTML.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan pengalaman operasional lapangan yang jauh lebih cepat dan terpadu bagi teknisi ISP di lapangan langsung dari aplikasi Telegram ponsel pintar mereka.

#### [[`Pekerjaan Staging / Working Tree`](file:///home/dhedhy/Project/Dekasimal-V2)] - Penyempurnaan Filter Query, Pengamanan Array Pesan, Debounce Pencarian, State Error Interaktif, dan Isolasi Log Telegram Transport

- **Komponen yang Berubah**:
  - [`backend/src/services/ticket.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/ticket.service.js):
    - Penyempurnaan filter Mongoose `complete: { $ne: true }` agar tetap menjaring tiket lama yang field `complete`-nya belum terdefinisi atau bernilai `null`.
    - Penambahan opsi `{ withDeleted: true }` pada populate `assignment`, `customer`, dan `partner` agar nama teknisi atau pelanggan yang telah diarsipkan tetap tampil utuh pada histori tiket.
    - Pengamanan normalisasi pesan: mendukung struktur `messages` baik dalam format array maupun objek terindeks Mongoose (`Object.values(t.messages || {})`).
    - Penanganan fleksibel isi pesan terakhir (`last?.detail || last?.text`) sebelum dipotong 120 karakter.
  - [`backend/src/utils/logger.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/logger.js), [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js), & [`backend/src/utils/telegramAlert.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegramAlert.js):
    - **Pencegahan Infinite Alert Loop**: Penambahan flag `fromTelegramTransport: true` dan inspeksi konteks `telegram`/pesan `[Telegram]` pada Winston `TelegramAlertTransport` dan fungsi pelacak lonjakan error `recordErrorForSurgeTracking`/`sendTelegramErrorAlert`. Apabila terjadi kegagalan jaringan atau timeout saat bot Telegram mengirimkan notifikasi, kegagalan tersebut tidak akan memicu pengiriman alert Telegram baru secara berulang tanpa akhir yang dapat menghabiskan kuota atau membebani CPU server.
  - [`backend/test/unit/ticketMobile.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/ticketMobile.service.test.js) [NEW]:
    - Pengujian unit komprehensif untuk fungsi `findMobileOpenTickets`: verifikasi kriteria query, penandaan akurat `is_assigned`/`is_replied`, pemotongan tag HTML pada cuplikan pesan, pagination, serta perlindungan regex injection saat melakukan pencarian dengan karakter khusus.
  - [`telegram-apps/src/pages/tickets/TicketList.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketList.jsx):
    - Penerapan isolasi *debounce* 300ms khusus pada input pencarian teks (`debouncedSearch`) sehingga pergantian tab jenis tiket dieksekusi secara instan tanpa jeda tunda.
    - Penambahan penanganan error jaringan/server interaktif lengkap dengan pesan informatif dan tombol **Coba Lagi** (*retry*).
    - Integrasi tombol kembali Telegram WebApp (`useTelegramBackButton('/home')`) yang mengarahkan pengguna kembali ke beranda dengan konsisten.
  - [`telegram-apps/src/utils/pinnedTickets.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/utils/pinnedTickets.js):
    - Penambahan penjaga tipe (*type guard*) `manuallyPinnedIds && typeof manuallyPinnedIds.has === 'function'` untuk mencegah galat runtime saat membaca data pin.
- **Deskripsi Perubahan & Fungsi**:
  - Menghilangkan bug loop notifikasi bot, mengamankan kestabilan komponen aplikasi mobile saat kondisi sinyal lemah, dan memvalidasi kebenaran logika backend dengan cakupan test 100%.

---

## 🌿 Branch: `issue-411` — Penghematan Pengecekan Pembayaran Online ke Gateway (iPaymu Rate-Limit Protection & Staggered Sweep)

### 📌 Informasi Issue

- **Nomor Issue**: #411
- **Judul Issue**: Penghematan Frekuensi Pengecekan Pembayaran Online ke Penyedia Gateway (iPaymu Call Meter, Staggered Polling, & Bulk History Check)
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `925d0fa9` dan ke `production`)

### 📅 Rincian Commit

#### [[`6416fdcc`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #411 - 14 Oktober 2026, 17:41:29 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/gatewayCallMeter.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/gatewayCallMeter.js) [NEW]:
    - Modul penghitung dan pengontrol anggaran panggilan API (*Call Budget Meter*) gateway berbasis Redis.
    - Melacak jumlah panggilan per menit, jam, dan harian; mendeteksi dan mengklasifikasikan respons error (status 429 *rate-limited*, respons `invalid ip`/pemblokiran, dan kegagalan umum).
    - Mencatat ringkasan metrik statistik panggilan secara otomatis pada setiap pergantian jam dan memberikan peringatan dini (*warning*) jika terjadi pembatasan frekuensi oleh server gateway.
  - [`backend/src/services/financeGateway.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeGateway.service.js):
    - Perombakan alur sweep transaksi menggantung (*pending transactions*):
      1. **Bulk History Query**: Menggantikan pengecekan satuan (*one-by-one check*) dengan pembacaan riwayat transaksi massal dari endpoint gateway iPaymu, sehingga puluhan transaksi tertunda dapat diperbarui statusnya hanya dalam 1 panggilan API.
      2. **Staggered Interval Schedule**: Penjadwalan pengecekan per transaksi disesuaikan dengan usia tagihan: transaksi baru (0-15 menit) dicek lebih sering, transaksi sedang (15-60 menit) dilonggarkan, dan transaksi lama dicek dengan jeda yang makin renggang.
      3. **Circuit Breaker / Cooldown**: Apabila gateway mengembalikan respons penolakan atau *rate-limit*, sistem segera menghentikan sementara antrean pengecekan selama periode tertentu guna mencegah pemblokiran alamat IP server secara permanen.
      4. **Anti-Spam Manual Check**: Tombol pengecekan manual oleh pelanggan pada invoice kini diproteksi dengan jeda minimal (*cooldown* per transaksi), menghindari penembakan berulang ke server iPaymu.
  - [`backend/src/services/paymentGateways/ipaymu.gateway.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/paymentGateways/ipaymu.gateway.js):
    - Integrasi metering pada setiap eksekusi panggilan API iPaymu dan penyesuaian penanganan galat tanda tangan HMAC.
  - [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js):
    - Penyesuaian jadwal eksekusi periodik background job `financeGatewaySweep` dari setiap beberapa menit menjadi tiap 15 menit, mengingat notifikasi webhook instan tetap menjadi jalur utama konfirmasi pembayaran.
  - [`backend/test/unit/gatewayCallMeter.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/gatewayCallMeter.test.js) [NEW] & [`backend/test/integration/financeGateway.sweep.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeGateway.sweep.test.js) [NEW]:
    - Rangkaian pengujian unit dan integrasi untuk memverifikasi akurasi anggaran panggilan Redis, isolasi kegagalan Redis, dan keandalan pemutakhiran status pembayaran massal.
- **Deskripsi Perubahan & Fungsi**:
  - Melindungi server ISP dari risiko pemblokiran IP oleh penyedia gateway pembayaran daring tanpa mengorbankan kecepatan verifikasi invoice lunas pelanggan.

---

## 🌿 Branch: `issue-405` — Aksi Aktif/Nonaktif Terkonfirmasi pada Menu Aksi Tabel

### 📌 Informasi Issue

- **Nomor Issue**: #405
- **Judul Issue**: Standardisasi Aksi Aktif/Nonaktif Melalui Menu Aksi Baris Terkonfirmasi pada Seluruh Datatable Sistem
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `5d2600f6` dan ke `production`)

### 📅 Rincian Commit

#### [[`5d2600f6`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #405 (Merge commit dari branch `issue-405`) - 14 Oktober 2026, 17:12:06 WIB

- **Komponen yang Berubah**:
  - Kolom status dan menu aksi pada tabel-tabel utama frontend: Pengguna Admin, Pelanggan Broadband, Akun Hotspot, Karyawan, Mitra Bisnis, Vendor, Perangkat Absensi, Akun Keuangan, Bagan Akun (COA), dan Transaksi Berulang.
- **Deskripsi Perubahan & Fungsi**:
  - Menggantikan saklar toggle langsung pada kolom tabel dengan lencana status statis yang bersih dan memindahkan perintah ubah status ke dalam menu aksi baris titik-tiga dengan jendela modal konfirmasi.
  - Mencegah insiden pemutusan layanan pelanggan atau penonaktifan akun secara tidak sengaja akibat salah sentuh layar perangkat mobile.

---

## 🌿 Branch: `issue-385` — Penerbitan SPK Langsung dari Tiket, Lembar Dokumen Cetak, & Penyesuaian Logging

### 📌 Informasi Issue

- **Nomor Issue**: #385
- **Judul Issue**: Alur Penerbitan Surat Perintah Kerja (SPK) Tiket, Lembar Cetak Dokumen, Pengesahan Tanda Tangan Digital, dan Pencegahan Notifikasi Error Spam
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `058b54f8` dan ke `production`)

### 📅 Rincian Commit

#### [[`390c6540`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #385 - 14 Oktober 2026, 10:34:11 WIB
#### [[`4f2bfdcc`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #385 - fixbug - 14 Oktober 2026, 16:23:20 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/workOrder.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/workOrder.controller.js) & [`backend/src/services/workOrder.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/workOrder.service.js):
    - Layanan dan controller lengkap untuk penerbitan SPK tiket: alokasi nomor surat berurutan, sanitasi material, penugasan teknisi dan penunjukan lead, penandatanganan digital penerbit, serta pembatalan berantai saat tiket dibatalkan.
  - [`frontend/src/app/pages/tickets/components/CreateTicketWorkOrderDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/tickets/components/CreateTicketWorkOrderDrawer.jsx):
    - Antarmuka drawer penerbitan SPK pada seluruh jenis tiket.
  - [`frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/workOrder/TicketWorkOrderSheet.jsx):
    - Template lembar cetak fisik A4 resmi Surat Perintah Kerja siap cetak dengan barcode penomoran SPK dan kolom tanda tangan pengesahan.
  - [`backend/src/middlewares/error.middleware.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/middlewares/error.middleware.js) & [`frontend/src/utils/clientErrorFilter.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/clientErrorFilter.js):
    - Penyesuaian error logging middleware: galat yang berasal dari kesalahan input pengguna (status 400, 404, 422) diturunkan level pencatatannya menjadi `warn`/`info` agar tidak memicu notifikasi peringatan error kritis pada bot Telegram pengembang.
  - [`frontend/src/app/pages/settings/sections/developer/ServerTimeCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/ServerTimeCard.jsx):
    - Penambahan kartu pemantauan Waktu Server pada menu Developer Settings untuk mempermudah audit selisih waktu NTP antara server, database, dan peramban pengguna.
- **Deskripsi Perubahan & Fungsi**:
  - Mengintegrasikan manajemen penugasan fisik lapangan dengan tiket layanan pelanggan serta menjaga ketenangan saluran notifikasi Telegram dari alarm error palsu.

---

## 🌿 Branch: `issue-415`, `master`, & `production` — Pemutakhiran Riwayat Changelog & Sinkronisasi Rilis Sistem

### 📌 Informasi Issue

- **Nomor Issue**: #415
- **Judul Issue**: Pemutakhiran Dokumen Changelog Rilis v1.94.2, v1.94.3, v1.95.0, v1.95.1 dan Deployment Penggabungan Produksi
- **Status Branch**: `Sudah di-merge` (Commit `b762e7ee` pada master & commit merge `a1e8def4` pada production)

### 📅 Rincian Commit

#### [[`b762e7ee`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 14 Oktober 2026, 17:48:25 WIB
#### [[`a1e8def4`](file:///home/dhedhy/Project/Dekasimal-V2)] - Merge branch 'master' into production - 14 Oktober 2026, 17:48:48 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json)
  - [`backend/src/data/changelog/releases/issue-385.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-385.json) [NEW] (v1.95.0)
  - [`backend/src/data/changelog/releases/issue-405.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-405.json) [NEW] (v1.94.3)
  - [`backend/src/data/changelog/releases/issue-408.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-408.json) [NEW] (v1.94.2)
  - [`backend/src/data/changelog/releases/issue-411.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-411.json) [NEW] (v1.95.1)
- **Deskripsi Perubahan & Fungsi**:
  - Mendokumentasikan seluruh pembaruan fitur ke dalam berkas metadata changelog sistem yang dapat diakses dari dashboard aplikasi dan melakukan sinkronisasi kode secara penuh ke branch rilis produksi.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #347  | Modul Tiket Terbuka Telegram Mini App | Teknisi lapangan dapat memantau, mencari, menyematkan (*pin*), membaca detail, dan membalas komentar tiket penanganan gangguan langsung dari aplikasi Telegram ponsel pintar mereka. |
| #411  | Penghematan Pengecekan Pembayaran Gateway | Panggilan API ke gateway iPaymu dikurangi drastis melalui *call budgeting*, *bulk check*, dan *staggered polling*, mencegah IP server diblokir penyedia gateway. |
| #405  | Aksi Aktif/Nonaktif Terkonfirmasi pada Tabel | Menghilangkan insiden kesalahan aktivasi/nonaktivasi data pelanggan dan layanan akibat ketidaksengajaan sentuh pada layar ponsel pintar atau tablet. |
| #385  | Penerbitan SPK dari Tiket & Lembar Cetak | Menghubungkan tiket pelanggan dengan penerbitan Surat Perintah Kerja fisik berstandar perusahaan yang dapat dicetak dan ditandatangani. |
| #415  | Changelog Rilis & Sinkronisasi Produksi | Pemutakhiran riwayat versi sistem v1.94.2 s/d v1.95.1 dan penjaminan stabilitas deploy branch produksi. |

### Kemampuan Baru Pengguna/Admin

- **Teknisi & Staf Operasional di Lapangan**:
  - Dapat membuka menu "Daftar Tiket Terbuka" di Telegram Mini App kapan pun dan di mana pun.
  - Tiket yang ditugaskan ke teknisi secara otomatis disematkan di posisi teratas daftar (*Smart Pinning*).
  - Dapat menyematkan tiket tertentu secara manual agar tidak terlewatkan dari pantauan.
  - Dapat langsung menekan nomor telepon pelanggan untuk melakukan panggilan seluler, atau menekan tombol navigasi peta untuk membuka titik koordinat tiang/rumah pelanggan di Google Maps.
  - Dapat mengirimkan komentar atau laporan kemajuan pekerjaan teknis langsung dari Telegram tanpa perlu membuka laptop.
- **Admin Keuangan & Billing**:
  - Pemeriksaan status tagihan online iPaymu berjalan lebih aman, teratur, dan tidak lagi membebani batas kuota API penyedia pembayaran.
- **Admin Helpdesk & Operasional**:
  - Menerbitkan SPK penugasan teknisi dari tiket pelanggan hanya dengan beberapa ketukan, lengkap dengan susunan material dan lembar cetak dokumen resmi bertanda tangan.

### Bug Fix / Solusi Masalah

- **Pencegahan Infinite Alert Loop Notifikasi Telegram**: Memutus rekursi error logging pada transport Telegram bot saat terjadi kegagalan pengiriman pesan ke grup pengembang.
- **Eliminasi Kesalahan Sentuh Status**: Menghilangkan saklar switch langsung pada datatable yang kerap memicu perubahan status pelanggan tanpa sengaja.
- **Perlindungan Rate-Limit IP Payment Gateway**: Mencegah penolakan request pembayaran akibat polling otomatis yang terlalu rapat.
- **Pembersihan Log Palsu**: Kesalahan input form (status 400/404/422) tidak lagi dicatat sebagai *fatal system error*.

### Menu/Fitur Baru

- **Telegram Mini App**: Menu baru **Tiket > Daftar Tiket Terbuka** (`/tickets/list`) dan **Detail Tiket** (`/tickets/detail/:type/:id`).
- **Dashboard Web Pengaturan**: Kartu **Waktu Server** pada menu Pengaturan > Developer.
- **Halaman Tiket Web**: Tombol dan Drawer **Terbitkan SPK** serta dokumen cetak Surat Perintah Kerja.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Tiket Terbuka via Telegram Mini App (`issue-347`)

- **Penjelasan Fitur**:
  Fitur ini dirancang khusus untuk mempermudah teknisi jaringan yang beraktivitas di lapangan. Melalui Telegram Mini App, teknisi dapat melihat seluruh tiket terbuka yang membutuhkan penanganan, mencari tiket dengan cepat, melihat lokasi GPS pelanggan, serta melaporkan temuan lapangan via kolom komentar tiket secara seketika (*real-time*).
- **Langkah Penggunaan (Tutorial)**:
  1. Buka Bot Telegram Dekasimal dari ponsel pintar dan luncurkan **Telegram Mini App**.
  2. Pada halaman Beranda (*Home*), temukan seksi **Tiket** dan klik menu **Daftar Tiket Terbuka**.
  3. Gunakan filter kategori di bagian atas (*Pelanggan, Pasang Baru, Backbone*, dsb.) atau ketik nama/nomor tiket pada bilah pencarian.
  4. Untuk menandai tiket penting, tekan ikon **Pin** pada kartu tiket. Tiket akan berpindah ke seksi teratas.
  5. Klik kartu tiket untuk membuka halaman **Detail Tiket**:
     - Klik tautan telepon untuk menghubungi pelanggan secara langsung.
     - Klik tautan **Lihat di Peta** untuk membuka rute navigasi Google Maps menuju lokasi gangguan.
     - Gulir ke seksi **Riwayat Percakapan** untuk membaca catatan petugas sebelumnya.
     - Ketik pembaruan progres pada kolom komentar di bagian bawah dan klik tombol **Kirim**.

### 2. Penerbitan & Pencetakan SPK dari Tiket Layanan (`issue-385`)

- **Penjelasan Fitur**:
  Menghubungkan eskalasi tiket gangguan ke penerbitan Surat Perintah Kerja (SPK) formal bagi teknisi lapangan lengkap dengan nomor surat resmi, daftar material yang dibawa, dan tanda tangan digital.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke Dashboard Web Dekasimal dan buka menu **Tiket** (misalnya Tiket Pelanggan).
  2. Buka salah satu tiket yang membutuhkan kunjungan teknisi lapangan.
  3. Klik tombol **Terbitkan SPK** di sudut kanan atas detail tiket.
  4. Lengkapi formulir SPK (data lokasi dan pelanggan akan terisi otomatis dari tiket):
     - Masukkan nama PIC dan instruksi penanganan.
     - Tambahkan daftar kebutuhan material kabel, adaptor, atau ONT yang perlu diambil dari gudang.
     - Pilih tim teknisi dan tentukan salah satu teknisi sebagai *Lead*.
     - Bubuhkan tanda tangan digital penerbit atau centang opsi tanda tangan manual saat dicetak.
  5. Klik **Simpan & Terbitkan SPK**.
  6. Setelah SPK terbit, tekan tombol **Cetak SPK** untuk melihat pratinjau dokumen A4 resmi dan mencetaknya atau mengekspornya menjadi berkas PDF.
