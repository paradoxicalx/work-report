# 📝 Daily Work Report - Dedy (2026-09-02)

---

## 📅 Laporan Harian - 2 September 2026

---

## 🌿 Branch: `issue-254` — Integrasi Monitoring Trafik RRD pada Layanan Dedicated Internet & Data Access, SNMP Direct Host Discovery, dan Mutex Lock RRD Polling

### 📌 Informasi Issue

- **Nomor Issue**: #254
- **Judul Issue**: Integrasi Pemantauan Trafik Antarmuka Berbasis RRDtool pada Layanan Dedicated Internet & Data Access (Direct Host SNMP Discovery, Service Traffic Target Linking, Mutex Lock Serialisasi Update RRD & Dynamic Timestamp Bumping)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan)

---

### ⏳ Pekerjaan Belum Di-commit (Working Tree Changes)

- **Komponen yang Berubah**:
  - [`backend/src/models/networkTrafficTarget.model.js`](backend/src/models/networkTrafficTarget.model.js) — Pembaruan skema target pemantauan trafik agar mendukung penautan langsung ke layanan (`service`):
    - **Dukungan Entitas Layanan**: Menambahkan relasi `service` (ObjectId mengarah ke model `Dedicated`), enum `service_type` (`device`, `dedicated_internet`, `data_access`), dan membuat relasi `device` menjadi opsional (`default: null`).
    - **Kredensial Direct Host SNMP**: Menambahkan konfigurasi SNMP kustom per target (`host`, `snmp_community`, `snmp_port`, `snmp_version`) sehingga pemantauan dapat diarahkan langsung ke alamat IP router/CPE pelanggan tanpa mewajibkan pendaftaran perangkat ke inventori master `NetworkDevice`.
    - **Compound Partial Unique Indexes**: Menambahkan indeks parsial unik `{ service: 1, interface_index: 1 }` dan `{ host: 1, interface_index: 1 }` untuk record aktif (`deleted: false`) guna mencegah duplikasi pemantauan antarmuka fisik yang sama di tingkat database.
  - [`backend/src/routes/networkTraffic.route.js`](backend/src/routes/networkTraffic.route.js) — Penambahan route API baru untuk kebutuhan monitoring layanan:
    - `POST /api/v1/network-traffic/discover-host` (privilege: `networkTraffic.create`): Menjalankan SNMP walk discovery antarmuka menggunakan alamat IP host kustom dan parameter SNMP dinamis.
    - `GET /api/v1/network-traffic/service/:serviceId` (privilege: `networkTraffic.read`): Mengambil daftar seluruh target trafik aktif yang ditautkan ke suatu layanan.
  - [`backend/src/controllers/networkTraffic.controller.js`](backend/src/controllers/networkTraffic.controller.js) — Implementasi controller `trafficTargetDiscoverHost` dan `trafficTargetGetByService`.
  - [`backend/src/services/networkTraffic.service.js`](backend/src/services/networkTraffic.service.js) — Logika bisnis pemantauan trafik layanan:
    - **Host Discovery**: Fungsi `discoverHostInterfaces()` untuk melakukan scanning OID antarmuka jaringan langsung ke host target via microservice `network-monitor`.
    - **Service Target Query**: Fungsi `findTrafficTargetsByService(serviceId)` dengan auto-populate device/service.
    - **Pembersihan Bersih (Safe Deletion)**: Fungsi `deleteServiceTrafficTargets(serviceId, deletedBy)` yang otomatis dieksekusi saat layanan dihapus; jika interface ditautkan dari perangkat fisik, relasi layanan dilepas secara anggun tanpa menghapus database RRD fisik.
    - **Dukungan Polling Multi-Sumber**: Memperbarui `pollAllTrafficTargets()` agar membaca alamat IP dan kredensial SNMP secara berjenjang dari `device`, `target.host`, atau `service.snmp_ip`.
    - **Redis Distributed Polling Lock**: Mengoptimalkan kunci siklus polling `lock:network-traffic:poll-cycle` dengan TTL 600 detik dan format timestamp ISO.
  - [`backend/src/controllers/productDataAccess.controller.js`](backend/src/controllers/productDataAccess.controller.js) & [`backend/src/controllers/productDedicatedInternet.controller.js`](backend/src/controllers/productDedicatedInternet.controller.js) — Memanggil `deleteServiceTrafficTargets()` pada siklus penghapusan data layanan Data Access dan Dedicated Internet.
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) — Menambahkan pesan bilingual: `hostRequired`, `interfaceSnmpMismatch`, `interfaceAlreadyMonitored`, dan `reuseSuccess`.
  - [`network-monitor/src/services/rrd.service.js`](network-monitor/src/services/rrd.service.js) — Penguatan konkurensi pembaruan data time-series RRD:
    - **Mutex Lock per File RRD (`withRrdLock`)**: Antrean Promise serialisasi berdasarkan target ID dan varian untuk mencegah eksekusi `rrdtool update` paralel pada berkas yang sama yang dapat memicu crash atau penolakan update.
    - **Dynamic Timestamp Bumping**: Menangani error *“illegal attempt to update using time”* dengan otomatis menggeser timestamp maju 1 detik dan mencoba ulang hingga 5 kali (`MAX_RRD_TIMESTAMP_BUMP = 5`) agar metrik tetap tersimpan tanpa data hilang.
  - [`network-monitor/src/routes/rrd.route.js`](network-monitor/src/routes/rrd.route.js) — Menambahkan rute alias `DELETE /delete/:targetId`.
  - [`frontend/src/app/pages/services/components/AddServiceMonitoringDrawer.jsx`](frontend/src/app/pages/services/components/AddServiceMonitoringDrawer.jsx) [NEW] — Drawer antarmuka penambahan monitoring trafik pada layanan:
    - Mendukung dua tab mode: **Input IP & SNMP Manual** (host, community, port, versi) dan **Pilih Perangkat Jaringan** (dropdown master router).
    - Memiliki fitur auto-discovery antarmuka via tombol scan SNMP, tabel daftar interface (nama, alias, kecepatan port, status UP/DOWN), indikator antarmuka yang sudah dipantau, serta pemilihan multi-antarmuka sekaligus.
  - [`frontend/src/app/pages/services/components/ServiceTrafficMonitoring.jsx`](frontend/src/app/pages/services/components/ServiceTrafficMonitoring.jsx) [NEW] — Panel monitoring grafik trafik RRD modern untuk halaman detail layanan:
    - Menampilkan kartu grafik time-series interaktif, bilah filter periode (`TrafficTimeFilterBar`), tombol tambah monitoring, modal zoom grafik (`GraphZoomModal`), modal perbaikan lonjakan anomali (`SpikeKillerModal`), modal edit target (`TrafficTargetEditModal`), dan modal konfirmasi hapus (`ConfirmModal`).
  - [`frontend/src/app/pages/services/dataAccess/detail.jsx`](frontend/src/app/pages/services/dataAccess/detail.jsx) & [`frontend/src/app/pages/services/dedicatedInternet/detail.jsx`](frontend/src/app/pages/services/dedicatedInternet/detail.jsx) — Mengganti panel monitoring lama (`MonitoringView`/`AddMonitoringDrawer`) dengan komponen baru `ServiceTrafficMonitoring`.
  - [`frontend/src/app/pages/network/traffic/components/TrafficTimeFilterBar.jsx`](frontend/src/app/pages/network/traffic/components/TrafficTimeFilterBar.jsx) — Penambahan mode prop `compact` untuk embedding di halaman detail layanan dan merapikan panel date picker kustom (hanya muncul saat preset kustom dipilih).
  - [`frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx`](frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx) — Dukungan mode tampilan ringkas (`compact`).
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Penambahan key translasi UI untuk form monitoring layanan.

---

### 📅 Rincian Commit

#### [`19af34e`](https://github.com/user/repo/commit/19af34e) - update changelog + gitignore - 2 September 2026, 11:29:01 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/releases/issue-252.json`](backend/src/data/changelog/releases/issue-252.json) [NEW] — Pembuatan berkas data rilis resmi untuk versi `v1.57.1` (Issue #252).
  - [`backend/src/data/changelog/index.json`](backend/src/data/changelog/index.json) — Pendaftaran rilis `issue-252` ke dalam indeks riwayat versi aplikasi.
  - [`.gitignore`](.gitignore) — Penyesuaian aturan ignore untuk berkas pelaporan internal.
  - [`_work-report/audit-report-issue-252.md`](_work-report/audit-report-issue-252.md) & [`_work-report/audit-task-issue-252.md`](_work-report/audit-task-issue-252.md) — Pembersihan berkas audit sementara pasca-verifikasi release.
- **Deskripsi Perubahan & Fungsi**:
  - Mendokumentasikan seluruh fitur, perbaikan bug, dan peningkatan sistem dari Issue #252 ke dalam sistem changelog internal Dekasimal agar dapat ditampilkan pada menu "What's New" di antarmuka pengguna.

---

## 🌿 Branch: `issue-259` — Reverse Proxy Nginx Multi-Service, Socket.IO Sticky Session, dan Radius Server gRPC Proto Integration

### 📌 Informasi Issue

- **Nomor Issue**: #259
- **Judul Issue**: Reverse Proxy Nginx Multi-Service Load Balancing (REST & gRPC), Socket.IO Consistent Hashing Sticky Session, Radius Server gRPC Proto Contract, serta Multi-Instance Network Monitor Production
- **Status Branch**: `Sudah di-merge` (Merge commit [`f4380d3`](https://github.com/user/repo/commit/f4380d3) ke `master` pada 2 September 2026, 17:41:34 WIB)

---

### 📅 Rincian Commit

#### [`f4380d3`](https://github.com/user/repo/commit/f4380d3) - resolve #259 - 2 September 2026, 17:41:34 WIB

- **Komponen yang Berubah**:
  - **Infrastruktur Reverse Proxy (Nginx)**:
    - [`reverse-proxy/Dockerfile`](reverse-proxy/Dockerfile) [NEW] — Pembuatan image Docker Nginx Alpine ringan dengan pembersihan otomatis `default.conf` bawaan agar tidak terjadi konflik listener IPv6 pada jaringan Docker.
    - [`reverse-proxy/conf.d/00-resolver.conf`](reverse-proxy/conf.d/00-resolver.conf) [NEW] — Deklarasi DNS resolver embedded Docker (`127.0.0.11 valid=10s`) dengan penomoran urut prioritas muat terdepan untuk mendukung dynamic domain re-resolution.
    - [`reverse-proxy/conf.d/backend.conf`](reverse-proxy/conf.d/backend.conf) [NEW] — Konfigurasi upstream load balancing Backend REST API (`backend_pool`) dan Socket.IO polling (`backend_pool_sticky`):
      - Menggunakan consistent hashing berbasis query parameter `$arg_sid` pada endpoint `/socket.io/` agar request polling lanjutan selalu diarahkan ke worker instance yang sama dengan penerbit handshake sesi.
      - Meneruskan header WebSocket upgrade (`$http_upgrade`, `Connection "upgrade"`), `Host`, dan IP klien asli.
    - [`reverse-proxy/conf.d/backend-grpc.conf`](reverse-proxy/conf.d/backend-grpc.conf) [NEW] — Konfigurasi reverse proxy gRPC HTTP/2 port 9090 (`backend_grpc_pool`) untuk melayani lalu lintas bidi-stream gRPC antara Radius Server dan Backend.
  - **Backend — Kontrak gRPC Protocol Buffer & Server**:
    - [`backend/proto/radius/v1/radius.proto`](backend/proto/radius/v1/radius.proto) [NEW] — Definisi protokol gRPC lengkap untuk sinkronisasi Radius Server (autentikasi PPPoE/Hotspot, pelaporan sesi akuntansi, push limit bandwidth, dan Disconnect/CoA message RFC 5176).
    - [`backend/src/grpc/server.js`](backend/src/grpc/server.js) & [`backend/src/grpc/streamRegistry.js`](backend/src/grpc/streamRegistry.js) — Pembaruan server gRPC backend untuk manajemen stream dua arah dan registrasi client aktif yang tangguh terhadap reconnect.
    - [`backend/src/server.js`](backend/src/server.js) — Integrasi inisialisasi server gRPC dan graceful shutdown terpadu.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan lapisan gerbang reverse proxy Nginx berkinerja tinggi di depan cluster Backend, memecahkan kendala session drop pada Socket.IO polling, dan memfasilitasi komunikasi bidi-stream gRPC terisolasi untuk modul Radius Server Go.

---

## 🌿 Branch: `issue-252` — Deteksi Wilayah Provinsi & Kabupaten Otomatis dan Penyempurnaan Dokumen PKS

### 📌 Informasi Issue

- **Nomor Issue**: #252
- **Judul Issue**: Deteksi Wilayah Geografis Provinsi & Kabupaten Otomatis Berbasis Koordinat, Penambahan Kolom Wilayah Data Pelanggan/Mitra, Integrasi Partner API, Penyempurnaan Dokumen PKS, dan Optimasi Komponen Form
- **Status Branch**: `Sudah di-merge` (Merge commit [`72f80ca`](https://github.com/user/repo/commit/72f80ca) ke branch kerja pada 2 September 2026, 11:04:30 WIB)

---

### 📅 Rincian Commit

#### [`72f80ca`](https://github.com/user/repo/commit/72f80ca) - resolve #252 - 2 September 2026, 11:04:30 WIB

- **Komponen yang Berubah**:
  - **Backend — Deteksi Wilayah Geografis Otomatis (Reverse Geocoding)**:
    - [`backend/src/utils/get-city.js`](backend/src/utils/get-city.js) — Utilitas resolusi batas wilayah geografis Indonesia berdasarkan titik koordinat (latitude & longitude) untuk mengekstrak nama Provinsi dan Kabupaten/Kota secara cepat dan offline.
    - [`backend/src/utils/update-customer-city.js`](backend/src/utils/update-customer-city.js) & [`backend/src/utils/update-partner-city.js`](backend/src/utils/update-partner-city.js) — Script migrasi dan sinkronisasi otomatis nama wilayah pada data historis pelanggan dan mitra.
    - [`backend/src/models/customer.model.js`](backend/src/models/customer.model.js), [`partner.model.js`](backend/src/models/partner.model.js), [`prospect.model.js`](backend/src/models/prospect.model.js) — Penambahan field `province` dan `city` pada skema database.
    - [`backend/src/controllers/prospect.controller.js`](backend/src/controllers/prospect.controller.js) & [`registration.controller.js`](backend/src/controllers/registration.controller.js) — Integrasi deteksi otomatis saat prospek baru atau registrasi mandiri disimpan.
    - [`backend/src/controllers/partnerApiCustomer.controller.js`](backend/src/controllers/partnerApiCustomer.controller.js) — Penyertaan informasi provinsi dan kabupaten pada endpoint Partner API untuk sinkronisasi mitra bisnis.
    - [`backend/src/routes/business.route.js`](backend/src/routes/business.route.js), [`customer.route.js`](backend/src/routes/customer.route.js), [`customerPartner.route.js`](backend/src/routes/customerPartner.route.js), [`partner.route.js`](backend/src/routes/partner.route.js), [`partnerApi.route.js`](backend/src/routes/partnerApi.route.js) — Penyesuaian query pencarian datatable dengan dukungan filter nama provinsi dan kota.
  - **Frontend — Tampilan Kolom Wilayah & Komponen Form**:
    - Penambahan kolom `Provinsi` dan `Kabupaten/Kota` dengan filter teks pada tabel Pelanggan, Pelanggan Mitra, Mitra Bisnis, dan Prospek ([`columns.jsx`](frontend/src/app/pages/users/customer/schema/columns.jsx)).
    - [`frontend/src/components/shared/form/Combobox.jsx`](frontend/src/components/shared/form/Combobox.jsx) — Perbaikan kendala visual flickering dan re-rendering berulang pada komponen pemilih data form.
  - **Frontend — Penyempurnaan Dokumen PKS (Perjanjian Kerja Sama)**:
    - [`frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx`](frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx) — Perbaikan total tata letak pratinjau dokumen PKS: penyesuaian otomatis jabatan penandatangan resmi, format tabel klausul pasal perjanjian, serta kerapian tampilan cetak PDF/kertas.
    - [`frontend/src/app/pages/users/customerPKS/create.jsx`](frontend/src/app/pages/users/customerPKS/create.jsx) & [`edit.jsx`](frontend/src/app/pages/users/customerPKS/edit.jsx) — Peningkatan form input penyusunan PKS agar lebih intuitif dan tervalidasi dengan skema Yup.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kemampuan otomatisasi pendataan wilayah administratif tanpa perlu input manual, memperluas kemampuan filter data geografis, menstabilkan form dropdown, serta menyajikan dokumen PKS siap cetak yang legal dan rapi.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #254  | Monitoring Trafik RRD Layanan Dedicated & Data Access, SNMP Host Discovery & RRD Mutex | Menghubungkan grafik trafik time-series RRDtool langsung ke halaman detail layanan Dedicated Internet & Data Access, menyediakan fitur SNMP discovery langsung ke IP host pelanggan tanpa wajib mendaftar master device, serta mengamankan update berkas RRD dengan mutex lock dan dynamic timestamp bumping. |
| #259  | Reverse Proxy Nginx Multi-Service, Socket.IO Sticky Session & gRPC Proto Radius | Menyediakan layer Nginx load balancer di depan Backend untuk REST API dan Socket.IO dengan sticky session `$arg_sid` yang stabil, mendukung HTTP/2 gRPC proxy port 9090 untuk Radius Server, dan mengorganisir dokumen monorepo ke direktori terpusat. |
| #252  | Deteksi Wilayah Geografis Otomatis & Penyempurnaan Dokumen PKS Pelanggan | Mengotomatiskan pengisian nama Provinsi dan Kabupaten/Kota dari koordinat peta GPS, menambahkan kolom wilayah pada tabel pelanggan dan mitra bisnis, menyempurnakan layout cetak dokumen PKS, dan mengatasi bug flickering form Combobox. |

---

### 🚀 Kemampuan Baru Pengguna/Admin

- **Pemantauan Trafik Langsung pada Layanan Pelanggan**: Admin dan tim teknis kini dapat memantau grafik penggunaan bandwidth secara real-time maupun historis langsung dari halaman detail layanan **Dedicated Internet** dan **Data Access**, tanpa harus berpindah ke menu Network Device.
- **Discovery Antarmuka Host Kustom**: Admin dapat menambahkan pemantauan trafik menggunakan alamat IP/host router pelanggan dan SNMP community kustom tanpa perlu mendaftarkan perangkat tersebut sebagai aset router di master inventori.
- **Filter Rentang Waktu Fleksibel pada Layanan**: Pengguna dapat melihat trafik dalam rentang 1 jam, 6 jam, 1 hari, 1 minggu, 1 bulan, 1 tahun, hingga filter tanggal dan jam kustom secara ringkas di halaman detail layanan.
- **Deteksi Wilayah Otomatis Tanpa Ketik Manual**: Saat mendaftarkan pelanggan, mitra, atau calon prospek, sistem secara otomatis mengisi Provinsi dan Kabupaten/Kota begitu titik lokasi pada peta dipilih.
- **Pencarian Data Pelanggan Berdasarkan Kota**: Admin dapat mencari dan menyaring data pelanggan, prospek, dan mitra berdasarkan nama kota atau provinsi langsung pada kotak filter tabel.
- **Penerbitan Dokumen PKS yang Rapi dan Sesuai Standar**: Dokumen Perjanjian Kerja Sama pelanggan kini memiliki format pratinjau yang presisi, mendukung klausul pasal terstruktur, dan siap dicetak secara profesional.

---

### 🛠️ Bug Fix / Solusi Masalah

- **Pencegahan Error Tabrakan Timestamp RRD (*Illegal Attempt to Update Using Time*)**: Microservice `network-monitor` kini dilengkapi dengan mutex lock per target dan mekanisme *Dynamic Timestamp Bumping* yang otomatis menggeser detik timestamp jika terjadi siklus polling yang tumpang tindih, menjamin data time-series RRD tidak pernah hilang/dibuang.
- **Pencegahan Duplikasi Interface Pemantauan**: Penerapan compound partial unique index pada database mencegah antarmuka fisik yang sama didaftarkan ganda oleh request yang terjadi bersamaan.
- **Penyelesaian Socket.IO Session Drop / Reconnect Loop**: Penggunaan consistent hash `$arg_sid` pada reverse proxy Nginx menjamin seluruh request polling sesi Socket.IO terarah ke container backend yang sama, mengeliminasi error *“Session ID unknown”*.
- **Pemberantasan Bug Flickering Komponen Combobox**: Komponen `Combobox.jsx` diperbaiki agar tidak melakukan re-render terus-menerus saat nilai form diperbarui dari luar.
- **Pencegahan Data Corrupt saat Hapus Layanan**: Saat layanan Dedicated/Data Access dihapus, sistem secara cerdas melepas tautan antarmuka trafik; berkas RRD hanya dihapus jika antarmuka tersebut tidak dipakai oleh perangkat lain.

---

### 📦 Menu/Fitur Baru

- **Panel Monitoring Trafik pada Detail Layanan**: Tersedia di **Layanan > Dedicated Internet > Detail** dan **Layanan > Data Access > Detail**.
- **Drawer Tambah Monitoring Layanan**: Drawer modern dengan opsi scan SNMP antarmuka manual maupun dari perangkat terdaftar.
- **Kolom Tabel Provinsi & Kota**: Tersedia pada tabel **Pengguna > Pelanggan**, **Pengguna > Mitra Bisnis**, **Pengguna > Pelanggan Mitra**, dan **Layanan > Prospek**.
- **Changelog Resmi Rilis v1.57.1**: Terdaftar pada sistem changelog backend untuk publikasi update ke pengguna.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Menambahkan Monitoring Trafik RRD pada Layanan Dedicated Internet / Data Access

- **Penjelasan Fitur**:
  - Fitur ini memungkinkan admin memantau penggunaan bandwidth pelanggan Dedicated Internet atau Data Access secara visual dengan grafik RRDtool.
  - Admin dapat memilih antarmuka dari router jaringan yang sudah ada, atau melakukan scan SNMP langsung ke IP router pelanggan (CPE).

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Layanan > Dedicated Internet** (atau **Data Access**) dan pilih salah satu pelanggan.
  2. Gulir ke bagian **Monitoring Trafik RRD**.
  3. Klik tombol **Tambah Monitoring Trafik**.
  4. Pada drawer yang terbuka, pilih mode penambahan:
     - **Input IP & SNMP Manual**: Masukkan IP address router/switch pelanggan, community SNMP (default: `public`), port (default: `161`), dan versi SNMP (`v2c`).
     - **Pilih Perangkat Jaringan**: Pilih perangkat router yang sudah terdaftar di master inventori Dekasimal.
  5. Klik tombol **Cari Antarmuka** (ikon kaca pembesar) untuk menjalankan auto-discovery OID antarmuka.
  6. Pilih satu atau beberapa antarmuka port yang ingin dipantau (misal: `ether1-uplink` atau `sfp-plus1`).
  7. Klik **Simpan Target Terpilih**. Grafik pemantauan akan langsung muncul dan diperbarui secara otomatis setiap siklus polling.

---

### 2. Pemanfaatan Fitur Deteksi Wilayah Otomatis pada Registrasi Pelanggan

- **Penjelasan Fitur**:
  - Saat menginput alamat pelanggan, admin tidak perlu memilih nama provinsi atau kabupaten/kota secara manual.
  - Sistem memanfaatkan reverse geocoding berbasis koordinat peta untuk mendeteksi wilayah administratif secara instan.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengguna > Pelanggan** lalu klik **Tambah Pelanggan** (atau saat melakukan registrasi prospek).
  2. Pada bagian informasi lokasi, tentukan titik koordinat pemasangan pada peta interaktif atau masukkan koordinat latitude & longitude.
  3. Sistem secara otomatis mendeteksi dan mengisi data **Provinsi** dan **Kabupaten/Kota**.
  4. Simpan data pelanggan. Data wilayah ini akan tampil pada kolom tabel dan dapat digunakan untuk penyaringan data operasional regional.

---

### 3. Konfigurasi Reverse Proxy Nginx untuk Socket.IO Sticky Session & gRPC

- **Penjelasan Fitur**:
  - Arsitektur reverse proxy Nginx baru mengelola dua upstream berbeda untuk Backend: pool round-robin standar untuk REST API dan pool sticky-session untuk Socket.IO polling.
  - Untuk modul Go Radius Server, reverse proxy menyediakan listener HTTP/2 gRPC pada port `9090`.

- **Langkah Verifikasi**:
  1. Pastikan reverse-proxy berjalan bersama stack container:
     ```bash
     docker compose -f docker-compose.prod.yml up -d reverse-proxy
     ```
  2. Buka DevTools pada browser di tab Network dan amati request `/socket.io/?EIO=4&transport=polling&sid=...`.
  3. Pastikan tidak ada respons error `400 Bad Request ("Session ID unknown")` saat polling berpindah antar-request.
