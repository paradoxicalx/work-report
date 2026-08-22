# Dokumentasi Spesifikasi API & Antarmuka Sistem POP ISP

Dokumen ini berisi arsitektur antarmuka pengguna (UI/UX) dan spesifikasi endpoint REST API untuk manajemen Point of Presence (POP) Internet Service Provider (ISP).

> **Status dokumen:** mencerminkan kode yang benar-benar berjalan di branch `issue-236` per audit keamanan terakhir (lihat §10). Baris bertanda ✅ sudah diimplementasikan; lihat §10 untuk daftar item dari `PARTNER_API_PLAN.md` yang **belum** diimplementasikan.

---

## 0. Jaminan Keamanan Lintas Modul (berlaku untuk SEMUA endpoint di bawah)

Poin-poin berikut adalah pola yang konsisten dipakai di seluruh Partner API — tidak diulang di tiap bagian modul, cukup dibaca sekali di sini.

1. **Scoping kepemilikan selalu server-side, di level query database** — bukan filter setelah data diambil, dan bukan hasil pengecekan terpisah yang bisa "lolos" di jalur lain. Field `partner` (atau `partner`+`ref_partner`) yang dikirim lewat body/query oleh role `partner` selalu diabaikan/dihapus sebelum diproses; nilai yang dipakai selalu `req.user._id` dari token yang sedang login.
2. **404 generik untuk "tidak ada" ATAU "bukan milik Anda"** — kedua kondisi ini SENGAJA tidak dibedakan (tidak pernah 403) di seluruh endpoint read/update/delete, supaya mitra tidak bisa memastikan keberadaan suatu record milik mitra lain hanya dari kode status (mencegah *data enumeration*).
3. **Filter kolom (`columnFilters`) pada endpoint datatable tidak bisa dipakai untuk menimpa/memintas scoping kepemilikan** — ditegakkan di level utilitas bersama `data-table.js`, bukan per-endpoint. Sebelum perbaikan audit issue-236, klien secara teknis bisa mengirim `columnFilters` yang menyasar field kepemilikan (`partner`, dsb.) dan menimpa filter server; sejak diperbaiki, filter kepemilikan server digabung via `$and` sebagai lapisan terakhir yang tidak bisa disentuh input klien apa pun.
4. **Field internal admin (`tech_support`, `pay_support`, dan sejenisnya)** tidak pernah dipopulate/diekspos ke role `partner` — hanya admin yang melihat identitas staf internal terkait. `.populate()` Mongoose tetap melekatkan data walau field-nya diberi `columnVisibility: false`, sehingga penyembunyian dilakukan dengan tidak menjalankan populate-nya sama sekali untuk role `partner`, bukan hanya menyembunyikan dari hasil akhir.
5. **Field finansial/administratif (`wallet`, `wallet_history`, `hsa_profit`, `blacklist`, `reseller`, `pid`, `created_by`, `ticket`)** selalu dihapus dari body request sebelum diproses untuk role `partner`, baik saat `create` maupun `update` — mencegah mass-assignment lewat field yang tidak seharusnya bisa diisi klien eksternal.

---

## 1. Pengguna - Pelanggan (Residential)

### Endpoint API

| Metode   | Endpoint                                  | Keterangan                                                             |
| :------- | :---------------------------------------- | :--------------------------------------------------------------------- |
| `POST`   | `/p-api/v1/customers/list`                | ✅ Mendapatkan daftar pelanggan (Datatable: paginasi, sorting, filter) |
| `GET`    | `/p-api/v1/customers/list-status`         | ✅ Metrik & ringkasan statistik pelanggan (`new`, `active`, dll.)      |
| `GET`    | `/p-api/v1/customers/read/{customerId}`   | ✅ Detail informasi pelanggan spesifik (ter-mask untuk data sensitif)  |
| `POST`   | `/p-api/v1/customers/create`              | ✅ Registrasi pelanggan baru (multipart: avatar, dokumen, geocoding)   |
| `PATCH`  | `/p-api/v1/customers/update`              | ✅ Pembaruan data pelanggan (sparse update, upload avatar & dokumen)   |
| `DELETE` | `/p-api/v1/customers/delete/{customerId}` | ✅ Menghapus (soft delete) pelanggan & validasi dependensi             |
| `PATCH`  | `/p-api/v1/customers/change-status`       | ✅ Mengubah status langganan (toggle / set boolean status aktif/pasif) |

---

### Komponen Tampilan

- **Tabel List Pelanggan:**
  - Nomor Pelanggan (`customer_id`)
  - Nama Lengkap (`name`)
  - Nomor HP / Kontak (`phone`)
  - Alamat Pemasangan (`address`) & Area (`area`, `city`)
  - Kategori / Tipe Layanan (`type`, contoh: `home`, `umkm`, dll.)
  - Status Langganan (`status`: Aktif / Nonaktif)
  - Koordinat Geografis (`coordinate`)
  - Tanggal Pembuatan (`created_at`)
  - Aktivitas Terakhir (`last_act`) & Login Terakhir (`last_login`)
  - Mitra Pemilik (`partner`, hanya tampil untuk role `admin`)
- **Kartu Statistik (Summary Cards):**
  - **Pelanggan Baru (`new`):** Pelanggan terdaftar dalam 30 hari terakhir.
  - **Aktivitas Terakhir (`activities`):** Pelanggan dengan aktivitas dalam 24 jam terakhir.
  - **Pelanggan Aktif (`active`):** Pelanggan berstatus langganan aktif (`status: true`).
  - **Pelanggan Non-Aktif (`nonActive`):** Pelanggan berstatus langganan non-aktif (`status: false`).
- **Informasi Tambahan (Halaman Detail):**
  - **Identitas:** Nama Lengkap, Nomor HP, Email, KTP (dimasker `1234xxxxxxxx0001`), NPWP (dimasker).
  - **Geografis:** Alamat lengkap, Area, Kota, dan Titik Koordinat.
  - **Catatan & Dokumen:** Catatan khusus (`notes`), berkas dokumen identitas (`documents`), foto profil avatar.
  - **Keamanan:** Password, saldo wallet, dan PID internal tidak diekspos ke klien.

---

### Catatan Operasi Tulis & Bisnis Logic

1. **Autentikasi & Scoping Hak Akses (`Bearer Token`):**
   - Menggunakan header `Authorization: Bearer <token>`.
   - **Role `partner` (Mitra Reseller):** Data secara otomatis terisolasi hanya untuk pelanggan miliknya sendiri (`partner: req.user._id`). Upaya membaca, mengubah, atau menghapus pelanggan pihak lain, pelanggan reguler (`pid: 'master'`), atau pelanggan blacklist akan selalu menghasilkan respons **`404 Not Found`** generik untuk mencegah kebocoran informasi.
   - **Role `admin`:** Memiliki akses global tanpa batasan mitra dan dapat mengelola seluruh pelanggan (reguler maupun mitra).
2. **Pendaftaran Pelanggan Baru (`POST /customers/create`):**
   - Tidak mewajibkan tiket instalasi internal (`ticket`).
   - Penautan mitra dilakukan otomatis berdasarkan sesi login mitra (`partner: req.user._id` dan `pid` milik mitra). Untuk role `admin`, field `partner` dapat diisi dengan `partner_id` publik mitra yang valid atau dikosongkan untuk pelanggan reguler (`pid: 'master'`).
   - Mendukung upload file `image` (avatar pelanggan, otomatis di-resize 300x300 PNG) dan `documents` (array berkas identitas) via `multipart/form-data`.
   - Geocoding nama kota (`updateCity`) dijalankan secara otomatis berbasis koordinat.
3. **Pembaruan Data Pelanggan (`PATCH /customers/update`):**
   - Menggunakan identifier `selectedCustomerId` (atau `customer_id`) di request body `multipart/form-data`.
   - Field internal/sensitif (`partner`, `pid`, `tech_support`, `pay_support`, `created_by`, `blacklist`, `ticket`) dilindungi dan diabaikan jika dikirim oleh role `partner`.
   - Mengirim nilai `'remove-data'` pada field string akan menghapus data tersebut ($unset).
   - Mendukung upload berkas baru `image` dan penambahan dokumen ke array `documents`.
4. **Penghapusan Pelanggan (`DELETE /customers/delete/{customerId}`):**
   - Menerapkan _soft delete_ (`Customer.delete(...)`) dan mengubah status langganan menjadi nonaktif (`status: false`).
   - **Validasi Integritas:** Penghapusan ditolak (**`400 Bad Request`**) jika:
     - Pelanggan masih memiliki saldo wallet aktif (`customer.wallet > 0`).
     - Pelanggan masih memiliki akun autentikasi RADIUS aktif (`RadiusAuthentication`).
     - Pelanggan masih memiliki tagihan yang belum terbayar (`FinanceInvoice` dengan `status: 'unpaid'`).
   - Cache Redis `customer:${id}` langsung dibersihkan setelah penghapusan berhasil.
5. **Perubahan Status Langganan (`PATCH /customers/change-status`):**
   - Mengubah status aktif/nonaktif pelanggan via body `{ "id": "<customer_id>" }`.
   - Jika field `status` disertakan sebagai boolean, status diset langsung sesuai nilai tersebut; jika tidak, status ditoggle secara otomatis (`!status`).

---

## 2. Pengguna - Pelanggan Bisnis (Corporate & Enterprise)

### Endpoint API

| Metode   | Endpoint                          | Keterangan                                                              |
| :------- | :-------------------------------- | :---------------------------------------------------------------------- |
| `POST`   | `/p-api/v1/business/list`         | ✅ Daftar pelanggan bisnis (Datatable: paginasi, sorting, filter kolom)  |
| `GET`    | `/p-api/v1/business/list-status`  | ✅ Metrik ringkasan status pelanggan bisnis (`total`, `active`, `inactive`)|
| `GET`    | `/p-api/v1/business/read/{id}`    | ✅ Detail profil lengkap pelanggan bisnis (KTP/NPWP dimasker)           |
| `POST`   | `/p-api/v1/business/create`       | ✅ Registrasi pelanggan bisnis baru (Upload avatar & dokumen pendukung) |
| `PATCH`  | `/p-api/v1/business/update`       | ✅ Pembaruan data profil pelanggan bisnis (Upload berkas & unset data)  |
| `DELETE` | `/p-api/v1/business/delete/{id}`  | ✅ Terminasi / soft delete pelanggan bisnis dengan validasi dependensi  |
| `PATCH`  | `/p-api/v1/business/change-status`| ✅ Mengubah status langganan aktif/nonaktif pelanggan bisnis            |

### Komponen Tampilan

- **Tabel List Pelanggan Bisnis:**
  - Nomor Pelanggan (`partner_id`) & Nama Perusahaan / Badan Usaha
  - Tipe Pelanggan (`type`: Corporate, Enterprise, Government, dll.)
  - NPWP & KTP (dimasker)
  - Data Kontak (Nomor Telepon & Email)
  - Lokasi Kantor (Alamat, Area, Kota, Koordinat Geografis)
  - Tanggal Bergabung & Status Langganan (*Active / Inactive*)
- **Kartu Statistik (Summary Cards):**
  - Total Pelanggan Bisnis
  - Total Pelanggan Aktif
  - Total Pelanggan Nonaktif
- **Drawer Detail & Form:**
  - Identitas Perusahaan & PIC
  - Berkas Legalitas & Dokumen Kontrak (`documents`)
  - Foto Profil / Avatar Perusahaan (`image`)

### Catatan Operasi Tulis & Bisnis Logic

1. **Autentikasi & Scoping Hak Akses (`Bearer Token`):**
   - **Role `partner` (Mitra Reseller):** Data secara otomatis terisolasi hanya untuk pelanggan bisnis miliknya sendiri (`partner: req.user._id`). Upaya membaca, mengubah, atau menghapus pelanggan pihak lain atau pelanggan reguler (`pid: 'master'`) akan menghasilkan respons **`404 Not Found`** generik.
   - **Role `admin`:** Memiliki akses global tanpa batasan mitra.
2. **Pendaftaran Pelanggan Bisnis Baru (`POST /business/create`):**
   - Penautan mitra dilakukan otomatis berdasarkan sesi login mitra (`partner: req.user._id`, `ref_partner: req.user._id`, dan `pid` milik mitra).
   - Mendukung upload file `image` (avatar, otomatis di-resize 300x300 PNG) dan `documents` (berkas legalitas) via `multipart/form-data`.
   - Geocoding nama kota (`updateCity`) dijalankan secara otomatis berbasis koordinat.
3. **Pembaruan Data Pelanggan Bisnis (`PATCH /business/update`):**
   - Menggunakan identifier `selectedPartnerId` (atau `partner_id` / `id`) di request body `multipart/form-data`.
   - Field internal/sensitif (`partner`, `ref_partner`, `pid`, `tech_support`, `pay_support`, `created_by`, `reseller`, `wallet`, `wallet_history`, `hsa_profit`) dilindungi dari modifikasi role `partner` — berlaku sama persis di `POST /business/create` (mencegah mass-assignment saldo wallet/asosiasi staf internal saat pendaftaran baru, bukan cuma saat update).
   - Mengirim nilai `'remove-data'` pada field string akan menghapus data tersebut ($unset).
   - Mendukung upload berkas baru `image` dan penambahan dokumen ke array `documents`.
   - Response `read`/`update` disanitasi (`removeSensitiveData` + field internal admin dihapus untuk role `partner`) — field `tech_support`/`pay_support` (populate nama staf admin) hanya tampil untuk role `admin`.
4. **Penghapusan Pelanggan Bisnis (`DELETE /business/delete/{id}`):**
   - Menerapkan _soft delete_ (`Business.delete(...)`).
   - **Validasi Integritas:** Penghapusan ditolak (**`400 Bad Request`**) jika:
     - Pelanggan masih memiliki tautan data access aktif (`ProductDataAccess`).
     - Pelanggan masih memiliki tagihan yang belum terbayar (`FinanceInvoice` dengan `status: 'unpaid'`).
     - Pelanggan masih memiliki saldo wallet aktif (`wallet > 0`).
   - Cache Redis `business:${id}` langsung dibersihkan setelah penghapusan berhasil.
5. **Perubahan Status Langganan (`PATCH /business/change-status`):**
   - Mengubah status aktif/nonaktif pelanggan via body `{ "id": "<partner_id>", "status": true/false }`.
   - Jika field `status` tidak disertakan, status ditoggle secara otomatis (`!status`).

---

## 3. Legal - Data POP (Read-Only)

> **Catatan Konteks Arsitektur:** Entitas **POP** pada sistem DEKASIMAL V2 merujuk langsung pada entitas **Mitra Bisnis** (`Partner` model).

### Endpoint API

| Metode | Endpoint                                  | Keterangan                                                           |
| :----- | :---------------------------------------- | :------------------------------------------------------------------- |
| `GET`  | `/p-api/v1/partners/profile`              | ✅ Profil POP / Mitra bisnis pengguna saat ini (berbasis token sesi) |
| `GET`  | `/p-api/v1/partners/read/{partnerId}`     | ✅ Detail informasi & legalitas spesifik POP / Mitra bisnis          |
| `GET`  | `/p-api/v1/partners/documents`            | ✅ Daftar dokumen perizinan & legalitas POP                          |
| `GET`  | `/p-api/v1/partners/documents/{filename}` | ✅ Unduh / Stream file dokumen legal (PDF / Gambar / Berkas)         |

---

### Komponen Tampilan (belum lengkap)

- **Profil POP:**
  - **Identitas:** Kode POP, Nama POP, Tipe POP (_Hub, Sub-POP, Mini-POP_), Status Operasional.
  - **Lokasi:** Alamat lengkap, Kode Pos, Mini Map (titik koordinat).
  - **Fasilitas & Properti:** Status Kepemilikan (Sewa/Milik Sendiri), Luas Area ($m^2$).
  - **Kelistrikan:** Kapasitas Daya PLN (kVA), Kapasitas Genset (kVA), Daya Cadangan UPS (Menit/Jam).
  - **Jaringan Upstream:** Nama Provider Upstream, Total Kapasitas, Jalur Redundansi.
  - **Inventaris Aset:** Total OLT, Router, Switch, OTB, ODC, ODP, dan Pelanggan terhubung.
  - **Operasional:** Nama PIC POP, Tanggal Mulai Operasional, Waktu Terakhir Diperbarui.
  - **Izin UI:** Flag `can_update: false` dan `can_delete: false` untuk menonaktifkan tombol manipulasi data.
- **Daftar Dokumen Legal:**
  - Judul Dokumen
  - Jenis Dokumen (_Sewa Lahan, IMB/PBG, Izin Lingkungan, UUG/HO, PKS, Sertifikasi Perangkat POSTEL, Kontrak PLN_)
  - Nomor Dokumen Resmi
  - Pihak Kedua / Instansi Terkait
  - Tanggal Terbit & Masa Berlaku
  - Badge Status Validitas (_Valid, Expiring Soon, Expired_) beserta hitung mundur sisa hari
  - Metadata Berkas (Ukuran file, jumlah halaman, tombol _View/Preview_)

### Catatan Keamanan & Bisnis Logic

1. **Autentikasi & Data Scoping:**
   - Role `partner`: `GET /partners/read/{partnerId}` hanya boleh mengembalikan profil diri sendiri. `{partnerId}` boleh berupa `partner_id` publik ATAU ObjectId Mongo — keduanya sama-sama menghasilkan **`404 Not Found`** generik bila menunjuk mitra lain (audit issue-236 menemukan & memperbaiki bug di mana jalur ObjectId sebelumnya secara tidak sengaja selalu mengembalikan profil milik diri sendiri alih-alih 404, akibat filter kepemilikan yang salah menimpa filter pencarian pada key yang sama).
   - Role `admin`: bebas melihat profil mitra mana pun, termasuk lewat query `?partner_id=` pada `GET /partners/profile`.
2. **Unduh Dokumen (`GET /partners/documents/{filename}`):**
   - Untuk role `partner`, server WAJIB memverifikasi `filename` benar-benar terdaftar di array `documents` milik mitra yang login sebelum men-stream file dari MinIO — mencegah IDOR (menebak/mengetahui nama file dokumen mitra lain).
3. **Data Wallet:** Saldo (`wallet`) ditampilkan apa adanya pada profil milik sendiri (`GET /partners/profile`) — ini keputusan produk yang disengaja (mitra perlu memantau saldo depositnya), berbeda dari modul lain yang selalu men-strip `wallet` dari response.

---

## 4 & 5. Data Teknis - Perangkat Jaringan (Aktif & Pasif)

> **Catatan Konteks Arsitektur:** Seluruh perangkat teknis aktif (Router, OLT, Switch, AP, Radio) dan perangkat pasif (ODC, ODP, OTB, dsb.) dikelola secara terpadu melalui model `NetworkDevice` pada koleksi `network_devices`.

### Endpoint API

| Metode   | Endpoint                                        | Keterangan                                                              |
| :------- | :---------------------------------------------- | :---------------------------------------------------------------------- |
| `POST`   | `/p-api/v1/network-devices/list`                | ✅ Mendapatkan daftar perangkat jaringan (Datatable: paginasi, filter)   |
| `GET`    | `/p-api/v1/network-devices/stats`               | ✅ Ringkasan metrik statistik KPI perangkat (`UP`, `DOWN`, latensi, dll.)|
| `GET`    | `/p-api/v1/network-devices/read/{deviceId}`     | ✅ Detail teknis lengkap spesifik perangkat                             |
| `POST`   | `/p-api/v1/network-devices/create`              | ✅ Menambahkan/mendaftarkan perangkat jaringan baru                     |
| `PATCH`  | `/p-api/v1/network-devices/update/{deviceId}`   | ✅ Memperbarui data dan konfigurasi perangkat                           |
| `DELETE` | `/p-api/v1/network-devices/delete/{deviceId}`   | ✅ Menghapus (soft delete) perangkat jaringan                           |

---

### Komponen Tampilan

- **Header & Statistik KPI Global:**
  - Status Breakdown: Total perangkat, Online (`UP`), Offline (`DOWN`), Warning (`WARN`).
  - Metrik Kinerja: Rata-rata Latensi (ms), Packet Loss (%), Distribusi Spektrum Frekuensi.
- **Tabel Daftar Perangkat (Datatable):**
  - Kolom: Nama Perangkat, IP Address, Tipe/Kategori Perangkat (`device_type`, `type`), Node POP (`node`), Status Operasional, Latensi, Waktu Polling Terakhir (`last_check`).
  - Aksi: Detail (Read), Edit (Update), Hapus (Delete).
- **Form Pendaftaran & Edit Perangkat:**
  - Field Identitas: Nama Perangkat (`name`), IP Address (`ip_address`), Tipe Perangkat (`device_type`), Model/Hardware (`type`), Node Lokasi (`node`), Grup (`group`).
  - Konfigurasi SNMP: Versi SNMP (`snmp_version`: 1, 2c), Komunitas (`snmp_community`), Port SNMP (`snmp_port`).
  - Custom OID (opsional): Interface Name, Interface Index, CPU OID, Suhu OID, Voltase OID.

---

### Catatan Keamanan & Bisnis Logic

1. **Autentikasi & Data Scoping:**
   - Role `partner`: Seluruh operasi (list, stats, read, create, update, delete) otomatis terisolasi pada perangkat milik mitra yang bersangkutan (`partner: req.user._id`).
   - Upaya membaca, mengubah, atau menghapus perangkat milik mitra lain akan menghasilkan respons **`404 Not Found`** generik untuk mencegah pengintaian data (*data enumeration*).
   - Saat membuat perangkat baru, field `partner` otomatis diset ke ID mitra yang sedang login.
2. **Role `admin`:**
   - Memiliki hak akses penuh secara global, atau dapat memfilter per mitra dengan menyertakan `partner_id`.

---

## 6. Data Teknis - Map Infrastruktur (Read-Only)

### Endpoint API

| Modul                  | Metode | Endpoint                                      | Keterangan                                                      |
| :--------------------- | :----- | :-------------------------------------------- | :-------------------------------------------------------------- |
| **Node & Marker Peta** | `GET`  | `/p-api/v1/map/markers`                       | ✅ Marker titik infrastruktur peta (filter `?types=pop,odc,odp`)|
|                        | `GET`  | `/p-api/v1/map/marker-types`                  | ✅ Daftar pilihan tipe marker/node unik yang tersedia           |
|                        | `GET`  | `/p-api/v1/map/nodes/read/{id}`               | ✅ Detail teknis konfigurasi node & perangkat terpasang         |
|                        | `GET`  | `/p-api/v1/map/nodes/report/{id}`             | ✅ Laporan utilisasi, kapasitas port, dan sambungan node        |
| **Kabel Fiber Optik**  | `GET`  | `/p-api/v1/map/cables/list`                   | ✅ Daftar jalur kabel optik (filter `?cores=...&search=...`)    |
|                        | `GET`  | `/p-api/v1/map/cables/core-capacities`        | ✅ Daftar pilihan kapasitas core kabel yang tersedia            |
|                        | `GET`  | `/p-api/v1/map/cables/read/{id}`              | ✅ Detail spesifik rute dan nodes terhubung kabel fiber         |
|                        | `GET`  | `/p-api/v1/map/cables/splices/by-cable/{id}`  | ✅ Matriks data sambungan splice core per kabel                 |
|                        | `GET`  | `/p-api/v1/map/cables/splices/by-node/{id}`   | ✅ Matriks data sambungan splice core per node                  |
|                        | `GET`  | `/p-api/v1/map/cables/node-topology/{id}`     | ✅ Visualisasi topologi interkoneksi kabel dan port pada node   |

---

### Komponen Tampilan

- **Peta Interaktif (Leaflet / Mapbox):**
  - Layer Titik Node: Ikon penanda POP, ODC, ODP, OTB, Pole, Joint Closure dengan warna dan status indikator.
  - Layer Garis Kabel: Jalur kabel fiber optik (*LineString*) dengan warna berbasis kapasitas core / status operasional kabel (*ACTIVE, CUT, MAINTENANCE*).
  - Filter Dinamis: Filter tipe node yang ditampilkan (*visibleNodeTypes*) dan filter kapasitas core kabel (*visibleCableCores*).
- **Drawer Detail Node:**
  - Identitas Node: Nama Node, Tipe (`type`), Kode Maps ID, Kelompok (`group`), Koordinat Geografis, Radius Cakupan, PIC Nama & Kontak.
  - Daftar Perangkat & Peralatan Terpasang: Equipment ID, Tipe (Splitter, ODP, ODC, Patch Panel), Port Status (*AVAILABLE, USED, BROKEN*).
  - Aksi: Buka Laporan Node (*Node Report*), Lihat Topologi Node (*Node Topology*).
- **Drawer Detail Kabel & Sambungan Splice:**
  - Identitas Kabel: Nama Kabel, Panjang Jalur ($m$), Kapasitas Core, Jumlah Tube, Status Jalur, Node Awal $\rightarrow$ Node Tujuan.
  - Matriks Sambungan Core (Splice Matrix): In Cable/Core $\rightarrow$ Out Cable/Core / Equipment Port.

---

### Catatan Keamanan & Bisnis Logic

1. **Autentikasi & Data Scoping:**
   - Role `partner`: Marker node dan kabel fiber optik otomatis terisolasi pada infrastruktur yang tertaut dengan mitra tersebut (`partner: req.user._id` atau node publik/terkait, `partner` kosong/null).
   - Upaya membaca detail node atau kabel milik mitra lain akan menghasilkan respons **`404 Not Found`** generik.
   - `GET /map/cables/core-capacities` juga ter-scope per mitra (hanya menampilkan kapasitas core dari kabel yang boleh dilihat mitra tersebut), bukan daftar global seluruh sistem.
2. **Batas Kepemilikan pada Data Relasional (Topologi & Splice)** — audit issue-236 menemukan & memperbaiki kebocoran di sini:
   - **Topologi Node (`GET /map/cables/node-topology/{id}`):** Traversal BFS berhenti di batas kepemilikan — kabel yang `partner`-nya terisi dan bukan milik mitra yang login DIANGGAP TIDAK ADA (tidak dipakai untuk splice/edge, dan node di ujung lainnya tidak ikut ditelusuri), bukan cuma memvalidasi node awal lalu menjelajahi seluruh graf infrastruktur tanpa filter.
   - **Splice per Kabel/Node (`.../splices/by-cable/{id}`, `.../splices/by-node/{id}`):** Referensi `in_cable`/`out_cable` pada tiap sambungan splice yang menunjuk ke kabel milik mitra lain disamarkan jadi `{_id}` saja (nama/kapasitas/warna disembunyikan), bukan ditampilkan apa adanya.
   - **Detail Kabel (`GET /map/cables/read/{id}`):** Field `nodes` (endpoint kabel) dibatasi ke field non-PII (`_id, name, type, maps_id, location, partner`) — TIDAK menyertakan `pic_name`/`pic_contact`/`notes`/`equipment` milik node di ujung kabel, walau node tersebut publik/milik pihak lain.
   - **Laporan Node (`GET /map/nodes/report/{id}`):** Kabel terhubung yang bukan milik mitra (dan bukan publik) dikeluarkan sepenuhnya dari daftar `cables` pada laporan.
3. **Karakter Read-Only:**
   - Modul ini bersifat murni penayangan data spasial dan topologi. Perubahan rute atau penyambungan splice dikelola terpisah melalui panel operasional teknis.

---

## 7. RADIUS - Autentikasi & Profil (CRUD & Monitoring)

### Endpoint API

| Modul                | Metode   | Endpoint                                     | Keterangan                                                              |
| :------------------- | :------- | :------------------------------------------- | :---------------------------------------------------------------------- |
| **Akun Autentikasi** | `POST`   | `/p-api/v1/radius/users/list`                | ✅ Daftar akun autentikasi PPPoE (Datatable: paginasi, sorting, filter)  |
|                      | `GET`    | `/p-api/v1/radius/users/list-status`         | ✅ Metrik ringkasan status akun (`total`, `active`, `inactive`, `online`)|
|                      | `GET`    | `/p-api/v1/radius/users/read/{id}`           | ✅ Detail akun autentikasi (kredensial terproteksi)                     |
|                      | `POST`   | `/p-api/v1/radius/users/create`              | ✅ Mendaftarkan akun autentikasi PPPoE baru                             |
|                      | `PATCH`  | `/p-api/v1/radius/users/update/{id}`         | ✅ Memperbarui data akun autentikasi                                    |
|                      | `DELETE` | `/p-api/v1/radius/users/delete/{id}`         | ✅ Menghapus (soft delete) akun autentikasi                             |
|                      | `PATCH`  | `/p-api/v1/radius/users/change-status`       | ✅ Mengubah status aktif / nonaktif akun autentikasi                    |
| **Sesi & Monitoring**| `POST`   | `/p-api/v1/radius/sessions/list`             | ✅ Daftar sesi PPPoE aktif / riwayat sesi milik mitra                   |
|                      | `POST`   | `/p-api/v1/radius/sessions/disconnect`       | ✅ Putus paksa sesi PPPoE aktif (CoA Disconnect / RFC 5176)             |
| **Log Autentikasi**  | `POST`   | `/p-api/v1/radius/logs/list/{id}`            | ✅ Riwayat log autentikasi akun tertentu                                |
| **Profil Bandwidth** | `POST`   | `/p-api/v1/radius/profiles/list`             | ✅ Daftar profil kecepatan RADIUS (Datatable)                           |
|                      | `POST`   | `/p-api/v1/radius/profiles/select`           | ✅ Pencarian opsi profil RADIUS untuk select dropdown                   |
|                      | `GET`    | `/p-api/v1/radius/profiles/read/{id}`        | ✅ Detail konfigurasi profil RADIUS & counter akun                      |
|                      | `POST`   | `/p-api/v1/radius/profiles/create`           | ✅ Membuat profil kecepatan RADIUS baru                                 |
|                      | `PATCH`  | `/p-api/v1/radius/profiles/update`           | ✅ Memperbarui profil kecepatan RADIUS                                  |
|                      | `PATCH`  | `/p-api/v1/radius/profiles/update-batch`     | ✅ Memperbarui batch beberapa profil kecepatan RADIUS                   |
|                      | `DELETE` | `/p-api/v1/radius/profiles/delete/{id}`      | ✅ Menghapus profil RADIUS (validasi keterikatan akun)                  |

---

### Komponen Tampilan

- **Tab Akun RADIUS:**
  - Kolom: Username, Nama Pelanggan, Profil Layanan (`profile`), Tipe/Produk (`bind_product`), Status (Aktif/Nonaktif), Indikator Online (`isOnline`), IP Statis/Pool, Waktu Login Terakhir (`last_login`), Durasi Sesi, Total Pemakaian Data (Upload & Download).
  - Aksi: Detail (Read), Edit (Update), Hapus (Delete), Toggle Status, Putus Sesi (Disconnect).
- **Tab Profil Bandwidth RADIUS:**
  - Kolom: Nama Profil, Limit Kecepatan (`speed_limit`), Limit Kuota (Upload / Download), Limit Waktu, Profil Mikrotik (`mikrotik_profile`), Pengaturan Burst.
  - Aksi: Tambah Profil, Edit Profil, Hapus Profil.
- **Kartu Statistik Akun:**
  - Total Akun, Total Akun Aktif, Total Akun Nonaktif/Isolir, Total Akun Sedang Online.
- **Tab Sesi Aktif:**
  - Session ID (`sessionID`), Username, Pelanggan, Framed IP Address, IP NAS, Waktu Mulai (`startTime`), Waktu Update Terakhir, Status Online.
  - Aksi: Tombol Putus Koneksi (Kirim PoD Disconnect).
- **Tab Riwayat Log Autentikasi:**
  - Timestamp Kejadian (`date`), Username, Status Hasil Autentikasi (`type`), Pesan/Alasan (`message`), IP Address (`address`), Topik (`topic`).

---

### Catatan Keamanan & Bisnis Logic

1. **Autentikasi & Data Scoping:**
   - Role `partner`: Seluruh operasi akun autentikasi, sesi, dan log otomatis terisolasi pada akun milik pelanggannya atau dirinya sendiri (`partner: req.user._id` / `ref_partner: req.user._id`).
   - Pada profil RADIUS, mitra dapat melihat profil miliknya sendiri dan profil template global sistem (`pid: 'master'`). Modifikasi dan penghapusan profil dibatasi hanya untuk profil milik mitra sendiri (`partner: req.user._id`).
   - Upaya mengakses, mengubah, menghapus, atau memutus sesi milik akun/profil pihak lain akan menghasilkan respons **`404 Not Found`** generik untuk mencegah pengintaian data (*data enumeration*).
2. **Validasi Kepemilikan Field Referensi (`customer`, `bind_product`, `profile`, `pop`) saat Create MAUPUN Update:**
   - Audit issue-236: sebelumnya validasi field referensi ini hanya berjalan saat `create`; endpoint `update` bisa dipakai mitra untuk mengarahkan ulang akun radius miliknya ke `customer` (atau `bind_product`/`profile`/POP node) milik mitra LAIN, yang kemudian membocorkan akun tersebut ke mitra lain lewat filter kepemilikan yang diturunkan dari field-field itu.
   - Sekarang KEDUANYA (create & update) memvalidasi ulang: field yang dikirim harus menunjuk resource yang benar-benar ada DAN (untuk role `partner`) miliknya sendiri atau bersifat publik/global — kalau tidak, request ditolak `400 Bad Request`, field pada dokumen tidak berubah.
3. **Validasi Keterikatan Integritas:**
   - Penghapusan profil RADIUS ditolak (**`400 Bad Request`**) jika masih ada akun autentikasi yang menggunakan profil tersebut.
4. **Keamanan Kredensial:**
   - Password akun tidak pernah dikembalikan dalam format apa pun pada response `GET` detail akun autentikasi.
5. **Pemutusan Sesi (CoA Disconnect):**
   - Mengirimkan paket Disconnect-Request (RFC 5176) ke router NAS terdaftar di `radius_nas` dan memutus koneksi sesi aktif pelanggan secara instan.

---

## 8. Layanan Broadband & Pengaturan Global (Katalog Produk & Pembayaran)

### Endpoint API

| Modul                 | Metode   | Endpoint                                     | Keterangan                                              |
| :-------------------- | :------- | :------------------------------------------- | :------------------------------------------------------ |
| **Paket Broadband**   | `POST`   | `/p-api/v1/products/broadband/list`          | ✅ DataTables daftar produk paket broadband             |
|                       | `GET`    | `/p-api/v1/products/broadband/select-list`   | ✅ Daftar paket broadband untuk form registrasi         |
|                       | `POST`   | `/p-api/v1/products/broadband/select`        | ✅ Pencarian paket broadband untuk select dropdown      |
|                       | `POST`   | `/p-api/v1/products/broadband/group-select`  | ✅ Pencarian kelompok/grup paket broadband              |
|                       | `GET`    | `/p-api/v1/products/broadband/read/{code}`   | ✅ Detail spesifikasi paket broadband & counter akun    |
|                       | `POST`   | `/p-api/v1/products/broadband/create`        | ✅ Membuat produk paket broadband baru                  |
|                       | `PATCH`  | `/p-api/v1/products/broadband/update`        | ✅ Memperbarui data paket broadband                     |
|                       | `PATCH`  | `/p-api/v1/products/broadband/update-batch`  | ✅ Memperbarui batch beberapa paket broadband           |
|                       | `DELETE` | `/p-api/v1/products/broadband/delete/{code}` | ✅ Menghapus paket broadband (validasi dependensi akun) |
| **Metode Pembayaran** | `GET`    | `/settings/payment-methods`                  | Daftar saluran pembayaran yang didukung                 |
|                       | `GET`    | `/settings/payment-methods/{methodId}`       | Detail konfigurasi metode pembayaran                    |

### Komponen Tampilan

- **Katalog Paket Layanan Broadband:**
  - _Tabel List:_ Kode Produk (`code`), Nama Paket, Kapasitas Bandwidth (`capacity`), Harga Bulanan (`price`), VLAN ID (`vlan_id`), Grup Paket (`group`), Ketersediaan Request (`available_request`), Profil RADIUS Terkait (`radius_profile`).
  - _Halaman Detail / Form:_ Form input nama, kapasitas, harga, VLAN, mapping profil RADIUS, flag ketersediaan permintaan pelanggan dan mitra.
  - _Aksi:_ Tambah Paket, Edit Paket, Update Batch, Hapus Paket.
- **Metode Pembayaran:**
  - _Tabel List & Detail:_ Kode & Nama Metode, Tipe Pembayaran (_Bank Transfer, Virtual Account, E-Wallet, QRIS, Tunai/Loket, Kartu Kredit, Direct Debit_), Nama Provider Payment Gateway, Aset Logo Resmi, Skema Biaya Admin (Nominal Flat dan/atau Persentase serta Pihak Penanggung Biaya), Nomor Rekening/Akun (dimasker), Target Segmen, Estimasi Waktu Settlement Dana, Status Aktif Kanal, Metrik Penggunaan di POP (Jumlah Transaksi Pelanggan & Market Share %).

### Catatan Keamanan & Bisnis Logic

1. **Scoping Akses Paket Layanan Broadband:**
   - Role `partner` dapat melihat produk broadband miliknya (`partner: req.user._id`) dan produk global yang berstatus `available_partner: true`.
   - Modifikasi dan penghapusan produk dibatasi hanya untuk produk yang dibuat oleh mitra tersebut (`partner: req.user._id`).
   - Penghapusan produk broadband ditolak (**`400 Bad Request`**) jika masih ada akun autentikasi pelanggan yang terikat pada produk tersebut (`bind_product`).
   - Field `available_partner` (penanda "produk global", tampil ke SEMUA mitra) TIDAK BISA di-set oleh role `partner` sendiri, baik saat `create` maupun `update`/`update-batch` — audit issue-236 menemukan mitra sebelumnya bisa men-declare produk privatnya sendiri jadi global tanpa persetujuan admin, membuatnya otomatis muncul di katalog seluruh mitra lain. Field ini kini hanya bisa diisi oleh role `admin`.
2. **Standar Keamanan Pembayaran:** Kredensial sensitif payment gateway (_Merchant Key, Server Key, Client Secret_) diisolasi di tingkat backend dan tidak pernah dikirimkan pada payload response untuk peran pengguna mana pun.

---

## 9. Ringkasan Audit Keamanan (issue-236)

Audit menyeluruh atas seluruh endpoint Partner API di atas menemukan & memperbaiki beberapa celah kebocoran data lintas-mitra sebelum dokumen ini ditulis ulang. Dicatat di sini sebagai riwayat, bukan tugas terbuka — semuanya sudah diperbaiki dan diberi test regresi:

| Modul | Temuan | Perbaikan |
| :--- | :--- | :--- |
| **Lintas modul** (`data-table.js`) | `columnFilters` klien bisa menimpa/mencampur filter kepemilikan server di endpoint `POST .../list` manapun (Customer, Business, NetworkDevice, ProductBroadband, dsb.) — IDOR lintas-tenant penuh. | Filter kepemilikan digabung via `$and` sebagai lapisan terakhir yang tidak tersentuh input klien; berlaku otomatis untuk semua pemanggil `dataTable()`. |
| **Business** | `create` tidak menghapus `wallet`/`tech_support`/`pay_support`/`hsa_profit` dari body untuk role partner (mass-assignment). `read` membocorkan nama staf admin internal via populate. | Field di-strip di create (sama seperti update); populate `tech_support`/`pay_support` di-skip untuk role partner. |
| **Map** | Traversal topologi node & splice tidak memfilter kepemilikan node/kabel yang ditemukan lewat penjelajahan graf — membocorkan infrastruktur mitra lain yang kebetulan tersambung. `read` kabel membocorkan PIC/kontak node lewat populate tanpa `.select()`. `core-capacities` tidak ter-scope sama sekali. | Traversal berhenti di batas kepemilikan; referensi kabel asing pada splice disamarkan; populate node dibatasi field non-PII; `core-capacities` menerima filter kepemilikan. |
| **ProductBroadband** | Mitra bisa men-set `available_partner: true` pada produk miliknya sendiri, otomatis membocorkannya sebagai "produk global" ke seluruh mitra lain. | Field `available_partner` dihapus dari body untuk role partner di `create`/`update`/`update-batch`. |
| **Radius** | Validasi kepemilikan `customer`/`profile`/`bind_product`/`pop` hanya berjalan di `create`; `update` bisa dipakai mengarahkan akun ke resource milik mitra lain. | Logic validasi diekstrak jadi fungsi bersama (`resolveOwnedRef`), dipanggil di create **dan** update. |
| **Partner/POP** | `read/:id` dengan ObjectId Mongo (bukan `partner_id` publik) salah mengembalikan profil diri sendiri (200) alih-alih 404 untuk ID milik mitra lain, akibat filter kepemilikan menimpa filter pencarian pada key `_id` yang sama. `console.log(this)` pada `pre('save')` model Partner membocorkan password hash/wallet/ktp/npwp mentah ke log server pada setiap create/update mitra (bug pra-eksisting, bukan dari perubahan branch ini). | Filter digabung via `$and` alih-alih flat-merge; `console.log` dihapus. |

---

## 10. Gap Analysis — Perbandingan dengan `PARTNER_API_PLAN.md`

`PARTNER_API_PLAN.md` adalah spesifikasi awal/aspirasional (ditulis dengan asumsi arsitektur berbasis `popId` sebagai entitas top-level). Implementasi aktual memetakan **POP → entitas `Partner`** (lihat catatan di §3) dan sudah mengimplementasikan sebagian besar cakupan fungsional plan tersebut, dengan penyesuaian konvensi (datatable POST alih-alih REST query param, prefix `/p-api/v1/...` alih-alih `/pop/{popId}/...`). Berikut item dari plan yang **BELUM** ada di implementasi saat ini, per modul:

1. **Pelanggan (Residential & Bisnis):**
   - Tidak ada endpoint **export CSV/Excel** (`GET .../export`).
   - Tidak ada `PUT` untuk update menyeluruh (hanya `PATCH` parsial) — sesuai konvensi backend ini (lihat AGENTS.md), bukan celah, tapi berbeda dari plan.
   - Tidak ada header `Idempotency-Key` untuk mencegah duplikasi submit pada `create`.
   - Tidak ada endpoint perubahan status terpisah dengan audit-log alasan (`POST .../status` dengan pencatatan alasan) — perubahan status saat ini (`PATCH .../change-status`) tidak mencatat alasan/audit trail.
   - Tidak ada validasi "status berubah jadi isolir → putus sesi aktif otomatis" secara eksplisit di endpoint `change-status` pelanggan (berbeda dari `radius/sessions/disconnect` yang memang ada tapi manual).
2. **Legal - Data POP:** Field profil POP yang diminta plan (Tipe POP, Kode Pos, Status Kepemilikan properti, Luas Area, Kelistrikan/kVA Genset/UPS, Provider Upstream, Inventaris Aset OLT/Router/Switch/ODC/ODP) **belum ada** di model `Partner` — profil saat ini hanya berisi field bisnis umum (alamat, koordinat, kontak, dokumen). Sudah ditandai eksplisit "(belum lengkap)" di §3.
3. **Data Teknis Perangkat Aktif:** Plan meminta sub-resource terpisah per jenis perangkat dengan detail spesifik — **OLT** (PON ports, daftar ONU per port, RX/TX power), **Router** (BGP neighbor, interface traffic real-time), **Switch** (VLAN database, port PoE). Implementasi saat ini hanya CRUD generik `NetworkDevice` tanpa sub-resource/tab detail per jenis perangkat tersebut.
4. **Data Teknis Perangkat Pasif:** Plan meminta endpoint terpisah per jenis (OTB, ODC, ODP, Kabel) dengan detail matriks port/splitter spesifik jenis. Implementasi saat ini sudah punya endpoint kabel (`/map/cables/*`) dan node generik (`/map/nodes/*`, mencakup ODC/ODP/OTB via `type` pada `LocationPoint`), tapi belum ada endpoint/tampilan matriks port khusus per jenis perangkat pasif (mis. matriks patching OTB, mapping port in/out splitter ODC).
5. **Map:** Plan meminta parameter `bbox` (viewport) dan `zoom` (clustering titik padat) untuk optimasi render peta skala besar — belum diimplementasikan (`GET /map/markers` saat ini hanya mendukung filter `?types=`).
6. **RADIUS:**
   - Tidak ada endpoint reset password terpisah (`PUT .../password`) dengan jejak audit sendiri — saat ini password diubah lewat `PATCH /radius/users/update/{id}` biasa, tercampur dengan field lain.
   - Tidak ada flag `apply_now`/`apply_to_radius` untuk memicu CoA (Change of Authorization) otomatis saat profil/paket diubah — perubahan profil kecepatan tidak otomatis disinkronkan ke sesi PPPoE aktif tanpa disconnect manual.
   - Tidak ada endpoint export log autentikasi (`GET .../export`).
7. **Pengaturan Global:** Endpoint **katalog paket layanan read-only lintas semua produk** (`/settings/service-packages`) dan **metode pembayaran** (`/settings/payment-methods`) yang disebut plan **belum diimplementasikan sama sekali** di Partner API — baris pada tabel §8 untuk metode pembayaran masih mengarah ke path `/settings/...` generik (bukan `/p-api/v1/...`) dan belum ada controller/route Partner API yang menanganinya.
