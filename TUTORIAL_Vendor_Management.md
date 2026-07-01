# 📖 Tutorial Lengkap: Modul Vendor & Purchase Order (PO)

> **Dokumen ini menjelaskan langkah demi langkah cara menggunakan modul Vendor Management dan Purchase Order (PO) di sistem ISPF V2, berikut aturan dan larangan yang berlaku.**

---

## 📋 Daftar Isi

1. [Gambaran Umum Modul](#1-gambaran-umum-modul)
2. [Hak Akses (Privilege)](#2-hak-akses-privilege)
3. [CRUD Vendor](#3-crud-vendor)
4. [CRUD Item/Layanan Vendor](#4-crud-itemlayanan-vendor)
5. [CRUD Purchase Order (PO)](#5-crud-purchase-order-po)
6. [Alur Persetujuan PO](#6-alur-persetujuan-po)
7. [Tanda Tangan Digital PO](#7-tanda-tangan-digital-po)
8. [Aturan & Larangan Sistem](#8-aturan--larangan-sistem)
9. [Referensi Cepat](#9-referensi-cepat)

---

## 1. Gambaran Umum Modul

Modul **Vendor Management** dan **Purchase Order (PO)** digunakan untuk mengelola relasi bisnis dengan pihak ketiga (vendor/supplier). Alur kerjanya adalah:

```
Vendor → Item/Layanan Vendor → Purchase Order (PO) → Persetujuan → Tanda Tangan
```

| Entitas       | Deskripsi                                                          |
|---------------|--------------------------------------------------------------------|
| **Vendor**    | Perusahaan/supplier pihak ketiga                                  |
| **Item**      | Layanan, barang, atau kontrak yang dimiliki oleh vendor           |
| **PO**        | Dokumen pembelian yang merinci item yang dipesan dari vendor      |
| **Approval**  | Proses persetujuan PO oleh Super Admin                            |
| **Signature** | Tanda tangan digital yang disematkan pada dokumen PO yang disetujui |

---

## 2. Hak Akses (Privilege)

Akses ke modul ini dikontrol oleh sistem privilege. Pastikan akun Anda memiliki privilege berikut sebelum menggunakan fitur-fitur di bawah ini:

| Privilege                    | Fungsi                                    |
|------------------------------|-------------------------------------------|
| `vendor.create`              | Membuat vendor baru dan item vendor baru  |
| `vendor.update`              | Mengedit data vendor dan item vendor      |
| `vendor.delete`              | Menghapus vendor dan item vendor          |
| `vendor.changeStatus`        | Mengubah status aktif/nonaktif vendor     |
| `serviceActivation.update`   | Menyetujui atau menolak PO (dari drawer)  |

> ⚠️ **Catatan**: Persetujuan PO via halaman review tautan publik (`/review-po/:id`) hanya dapat dilakukan oleh **Super Admin** yang sedang login.

---

## 3. CRUD Vendor

### 3.1 Melihat Daftar Vendor

1. Masuk ke menu **Layanan** → **Vendor Management** dari sidebar.
2. Halaman menampilkan tabel daftar vendor dengan kolom: Vendor ID, Nama, Kode, Status, dan Tanggal Dibuat.
3. Gunakan fitur pencarian dan filter di tabel untuk menyaring data.
4. Klik baris atau tombol aksi untuk membuka detail vendor.

---

### 3.2 Membuat Vendor Baru

1. Di halaman daftar vendor, klik tombol **"+ Tambah Vendor"** (pojok kanan atas tabel).
2. Drawer form akan muncul dari sisi kiri layar.
3. Isi field berikut:

   **Informasi Utama:**
   | Field          | Keterangan                                           | Wajib? |
   |----------------|------------------------------------------------------|--------|
   | **Nama**       | Nama lengkap perusahaan vendor                       | ✅ Ya  |
   | **Kode**       | Kode unik singkatan vendor (contoh: `TELKOM`, `ICON`)| ✅ Ya  |

   **Kontak Account Manager (AM):**
   | Field            | Keterangan                              | Wajib? |
   |------------------|-----------------------------------------|--------|
   | **Email AM**     | Alamat email Account Manager            | ❌ Tidak|
   | **Telepon AM**   | Nomor HP AM (pilih kode negara terlebih dahulu) | ❌ Tidak|

   **Kontak NOC:**
   | Field            | Keterangan                              | Wajib? |
   |------------------|-----------------------------------------|--------|
   | **Email NOC**    | Alamat email Network Operation Center   | ❌ Tidak|
   | **Telepon NOC**  | Nomor HP NOC                            | ❌ Tidak|

   **Informasi Lainnya:**
   | Field          | Keterangan                              | Wajib? |
   |----------------|-----------------------------------------|--------|
   | **Alamat**     | Alamat lengkap kantor vendor            | ❌ Tidak|
   | **Deskripsi**  | Catatan tambahan tentang vendor         | ❌ Tidak|

4. Klik **"Simpan"** untuk menyimpan data vendor.
5. Sistem akan menampilkan notifikasi sukses dengan tautan menuju detail vendor yang baru dibuat.

---

### 3.3 Melihat Detail Vendor

1. Dari daftar vendor, klik baris atau ikon mata (👁) untuk membuka halaman detail.
2. Halaman detail menampilkan 2 tab utama:
   - **Tab "Item/Layanan"**: Daftar item dan layanan yang dimiliki vendor ini.
   - **Tab "Purchase Order"**: Daftar semua PO yang pernah dibuat untuk vendor ini.
3. Di bagian atas terdapat 2 card info:
   - **Informasi Umum**: Kode, alamat, status, dan tanggal pembuatan.
   - **Informasi Kontak**: Email & telepon AM dan NOC.

---

### 3.4 Mengedit Vendor

1. Buka halaman detail vendor.
2. Klik tombol **"Edit"** di pojok kanan atas.
3. Drawer edit akan muncul dengan data vendor yang sudah terisi.
4. Ubah field yang diperlukan:
   - Semua field sama seperti saat membuat vendor.
   - **Kode vendor tidak dapat diubah** setelah vendor dibuat.
5. Klik **"Simpan"** untuk menyimpan perubahan.

> ℹ️ **Info**: Status vendor (aktif/nonaktif) dapat diubah langsung dari tabel daftar vendor menggunakan tombol toggle pada kolom Status.

---

### 3.5 Menghapus Vendor

1. Dari tabel daftar vendor, klik ikon hapus (🗑) pada baris vendor yang ingin dihapus.
2. Modal konfirmasi akan muncul.
3. Klik **"Ya, Hapus"** untuk melanjutkan.

> ❌ **LARANGAN**: Vendor **tidak dapat dihapus** jika:
> - Masih memiliki **item/layanan** yang terdaftar.
> - Masih memiliki **Purchase Order (PO)** yang terkait (baik yang sudah disetujui maupun belum).
>
> Hapus terlebih dahulu semua item dan PO vendor sebelum menghapus vendor.

---

## 4. CRUD Item/Layanan Vendor

Item adalah katalog layanan, barang, atau kontrak yang disediakan oleh vendor. Item ini dapat digunakan sebagai referensi saat membuat PO.

### 4.1 Melihat Daftar Item

1. Buka halaman **Detail Vendor**.
2. Klik tab **"Item/Layanan"**.
3. Tabel item akan menampilkan: Kode, Nama, Tipe, Harga, dan Status.
4. Klik ikon detail (🔍) untuk membuka drawer info lengkap item.

---

### 4.2 Menambahkan Item Baru

1. Di tab "Item/Layanan", klik tombol **"+ Tambah Item"**.
2. Modal form akan muncul. Isi field berikut:

   | Field               | Keterangan                                                  | Wajib? |
   |---------------------|-------------------------------------------------------------|--------|
   | **Nama**            | Nama item/layanan                                           | ✅ Ya  |
   | **Kode**            | Kode unik item (contoh: `FO-10G`, `COLOC-1U`)               | ✅ Ya  |
   | **Tipe**            | Pilih salah satu: Layanan / Barang / Kontraktor             | ✅ Ya  |
   | **Tipe Harga**      | `OTC` (One Time Charge) atau `MRC` (Monthly Recurring Cost) | ✅ Ya  |
   | **Harga**           | Harga satuan item (dalam angka, min 0)                      | ✅ Ya  |
   | **Satuan**          | Satuan pengukuran (contoh: `Mbps`, `Unit`, `Port`)          | ❌ Tidak|
   | **Kapasitas**       | Kapasitas atau spesifikasi (contoh: `1000`)                 | ❌ Tidak|
   | **SLA (%)**         | Service Level Agreement dalam persen (0–100)                | ❌ Tidak|
   | **Mulai Kontrak**   | Tanggal mulai kontrak berlaku                               | ❌ Tidak|
   | **Akhir Kontrak**   | Tanggal kontrak berakhir                                    | ❌ Tidak|
   | **Deskripsi**       | Catatan tambahan tentang item                               | ❌ Tidak|

3. Klik **"Simpan"** untuk menyimpan item.

---

### 4.3 Mengedit Item

1. Di tabel item, klik ikon edit (✏️) pada baris item yang ingin diubah.
2. Modal edit akan terbuka dengan data item yang sudah terisi.
3. Ubah field yang diperlukan. **Kode item tidak dapat diubah** setelah dibuat.
4. Klik **"Simpan"** untuk menyimpan perubahan.

---

### 4.4 Menghapus Item

1. Di tabel item, klik ikon hapus (🗑) pada baris item yang ingin dihapus.
2. Konfirmasi pada modal yang muncul.
3. Klik **"Ya, Hapus"** untuk melanjutkan.

---

## 5. CRUD Purchase Order (PO)

Purchase Order adalah dokumen pembelian resmi yang dikirimkan ke vendor.

### 5.1 Melihat Daftar PO

**Dari halaman Detail Vendor:**
1. Buka halaman Detail Vendor.
2. Klik tab **"Purchase Order"**.
3. Tabel menampilkan semua PO untuk vendor tersebut beserta status persetujuannya.

**Dari menu Aktivasi Layanan:**
1. Masuk ke menu **Layanan** → **Aktivasi Layanan**.
2. Tab **"Purchase Order"** menampilkan semua PO dari semua vendor.

---

### 5.2 Membuat PO Baru

1. Di halaman **Detail Vendor**, klik tombol **"+ Buat PO"**.
2. Drawer form besar akan muncul dari kiri. Isi field berikut:

   **Header PO:**
   | Field                  | Keterangan                                             | Wajib? |
   |------------------------|--------------------------------------------------------|--------|
   | **Vendor**             | Terisi otomatis dari halaman detail vendor             | ✅ Ya  |
   | **Tanggal PO**         | Tanggal dokumen PO dibuat (default: hari ini)          | ❌ Tidak|
   | **Tanggal Pengiriman** | Estimasi tanggal pengiriman/selesai                   | ❌ Tidak|
   | **No. Quotation**      | Nomor penawaran dari vendor                            | ❌ Tidak|
   | **Metode Pembayaran**  | Contoh: Transfer Bank, Tunai                           | ❌ Tidak|
   | **Mata Uang**          | `IDR` (default) atau mata uang lain                    | ❌ Tidak|

   **Item PO (Line Items) — minimal 1 item wajib:**
   
   Klik **"+ Tambah dari Katalog"** untuk memilih item dari daftar item vendor, atau klik **"+ Tambah Baris"** untuk entri manual.
   
   | Field          | Keterangan                              | Wajib? |
   |----------------|-----------------------------------------|--------|
   | **Nama Item**  | Nama barang/layanan yang dipesan        | ✅ Ya  |
   | **Qty**        | Jumlah (minimal 1)                      | ✅ Ya  |
   | **Harga Satuan** | Harga per unit                        | ✅ Ya  |
   | **Deskripsi**  | Catatan per item                        | ❌ Tidak|

   **Pajak:**
   | Opsi Pajak      | Keterangan                                          |
   |-----------------|-----------------------------------------------------|
   | **Tidak Ada**   | Tidak ada pajak (default)                           |
   | **Persentase**  | Pajak dihitung berdasarkan % dari total (contoh: PPN 11%) |
   | **Tetap**       | Pajak dalam nominal angka tetap                     |

   **Informasi Tambahan:**
   | Field            | Keterangan                                             |
   |------------------|--------------------------------------------------------|
   | **Syarat & Ketentuan** | Teks syarat yang akan muncul di dokumen PO      |
   | **Catatan**      | Catatan internal                                       |
   | **Tampilkan Kontak** | Pilih kontak vendor mana yang ditampilkan di dokumen PO (Email AM, Telepon AM, Email NOC, Telepon NOC) |

3. Nomor PO akan digenerate otomatis oleh sistem dengan format:
   ```
   PO/{KODE_VENDOR}/{NOMOR_URUT}/{BULAN}/{TAHUN}
   Contoh: PO/TELKOM/001/06/2026
   ```
4. Klik **"Simpan"** untuk membuat PO. PO langsung tersimpan dalam status **Draft**.

---

### 5.3 Mengedit PO

> ⚠️ **PENTING**: PO **hanya dapat diedit selama belum disetujui**. Setelah PO disetujui (`approval` terisi), data PO tidak dapat diubah.

1. Di daftar PO (tab "Purchase Order" di detail vendor), klik ikon edit (✏️).
2. Drawer edit terbuka dengan data PO yang sudah terisi.
3. Field yang dapat diedit:
   - Nomor PO, No. Quotation, Metode Pembayaran, Mata Uang
   - Tanggal PO, Tanggal Pengiriman
   - Syarat & Ketentuan, Catatan
   - Tipe dan jumlah pajak
   - Line items (nama, qty, harga, deskripsi)
   - Tampilan kontak
   - **Stempel (Stamp)**: Unggah gambar stempel perusahaan yang akan muncul di dokumen PO
4. Klik **"Simpan"** untuk menyimpan perubahan.

---

### 5.4 Menghapus PO

1. Di daftar PO, klik ikon hapus (🗑) pada baris PO yang ingin dihapus.
2. Konfirmasi pada modal yang muncul.
3. Klik **"Ya, Hapus"** untuk melanjutkan.

> ❌ **LARANGAN**: Tidak ada batasan status untuk menghapus PO, namun sebaiknya hanya hapus PO yang masih berstatus Draft. PO yang sudah disetujui sebaiknya tidak dihapus karena memiliki dampak pada catatan histori.

---

## 6. Alur Persetujuan PO

### 6.1 Submit PO untuk Persetujuan

Setelah PO selesai dibuat dan siap untuk disetujui:

1. Buka drawer detail PO (klik baris PO di tabel, atau klik ikon preview 👁).
2. Klik tombol **"Kirim ke Telegram"** atau **"Request Preview"** untuk mengirimkan notifikasi ke admin Super via Telegram.
3. Admin akan menerima notifikasi berisi detail PO dan tautan untuk melakukan review.

---

### 6.2 Menyetujui PO (Approve)

Ada 2 cara untuk menyetujui PO:

#### Cara A: Via Drawer (dari menu Aktivasi Layanan)

1. Masuk ke **Aktivasi Layanan** → tab **"Purchase Order"**.
2. Klik baris PO yang ingin disetujui.
3. Drawer review PO akan terbuka di sisi kanan.
4. Periksa detail PO yang tertera.
5. Klik tombol **"Setujui"**.
6. Modal konfirmasi akan muncul dengan 3 pilihan tanda tangan (lihat bagian 7).
7. Klik **"Lanjutkan"** untuk memproses persetujuan.

#### Cara B: Via Tautan Publik (dari Telegram)

1. Buka tautan review PO yang diterima via Telegram (format: `https://domain.com/review-po/{id}`).
2. Pastikan sudah login sebagai **Super Admin** di sistem.
3. Halaman akan menampilkan pratinjau dokumen PO lengkap.
4. Klik tombol **"Setujui"**.
5. Modal konfirmasi dengan opsi tanda tangan akan muncul.
6. Klik **"Lanjutkan"** untuk memproses.

> ℹ️ **Catatan**: Halaman review publik hanya bisa diakses oleh Super Admin yang sudah login. Jika belum login atau bukan Super Admin, halaman akan menampilkan pesan "Unauthorized".

---

### 6.3 Menolak PO (Reject)

1. Buka drawer review PO.
2. Klik tombol **"Tolak"**.
3. Modal konfirmasi akan muncul.
4. Klik **"Ya, Tolak"** untuk menolak PO.

> ⚠️ **PENTING**: PO yang sudah **disetujui tidak dapat ditolak**. Penolakan hanya bisa dilakukan sebelum PO disetujui.

---

## 7. Tanda Tangan Digital PO

Saat menyetujui PO, admin memiliki 3 pilihan terkait tanda tangan:

### Pilihan 1: Tanda Tangani Langsung ✅ (Default jika ada tanda tangan tersimpan)

- Menggunakan tanda tangan yang sudah tersimpan di profil admin.
- Tanda tangan langsung disematkan ke dokumen tanpa langkah tambahan.
- **Pilihan ini hanya muncul jika admin sudah pernah menyimpan tanda tangannya ke profil.**

### Pilihan 2: Tambahkan Tanda Tangan

- Membuka kanvas tanda tangan (`DrawerSign`).
- Admin dapat **menggambar tanda tangan** menggunakan mouse/touchpad, atau **mengunggah gambar** file tanda tangan (PNG/JPG).
- Terdapat checkbox **"Simpan tanda tangan ke profil"**:
  - ✅ Jika dicentang: Tanda tangan disimpan ke database/MinIO untuk digunakan di kemudian hari.
  - ❌ Jika tidak dicentang: Tanda tangan hanya digunakan untuk PO ini saja (sekali pakai).
- Klik **"Simpan"** pada DrawerSign untuk melanjutkan persetujuan.

### Pilihan 3: Abaikan Tanda Tangan

- PO disetujui **tanpa tanda tangan digital**.
- Dokumen PO tetap sah secara sistem tetapi tidak memiliki gambar tanda tangan.

---

### 7.1 Menandatangani via Halaman Publik (`PublicPODocument`)

Setelah PO disetujui, vendor/pihak terkait dapat menandatangani dokumen PO melalui tautan publik yang terpisah.

1. Admin membagikan tautan dokumen PO ke vendor (via Telegram atau email).
2. Vendor membuka tautan (format: `https://domain.com/po-document/{token}`).
3. Halaman menampilkan dokumen PO lengkap.
4. Klik tombol **"Tandatangani"** (atau **"Gunakan Tanda Tangan Tersimpan"** jika login sebagai admin dengan tanda tangan tersimpan).
5. Kanvas tanda tangan akan muncul (DrawerSign).
6. Gambar atau unggah tanda tangan.
7. Klik **"Simpan"** — dokumen PO akan bertanda `complete: true` dan `signed_at` akan tercatat.

> ℹ️ Setelah ditandatangani melalui tautan publik, dokumen PO berstatus **lengkap/complete** dan tanda tangan tidak dapat diubah lagi.

---

## 8. Aturan & Larangan Sistem

### 🔒 Aturan Umum

| # | Aturan                                                                                 |
|---|----------------------------------------------------------------------------------------|
| 1 | Nama vendor harus **unik** — tidak boleh ada 2 vendor dengan nama yang sama.           |
| 2 | Kode vendor harus **unik** dan **tidak dapat diubah** setelah vendor dibuat.           |
| 3 | Kode item vendor harus **unik** dan **tidak dapat diubah** setelah item dibuat.        |
| 4 | Nomor PO digenerate **otomatis** oleh sistem dan tidak boleh duplikat.                 |
| 5 | PO minimal harus memiliki **1 line item**.                                             |
| 6 | Qty setiap line item minimal **1**.                                                    |
| 7 | Harga satuan minimal **0** (boleh gratis, tidak boleh negatif).                        |
| 8 | SLA item vendor harus antara **0–100%**.                                               |
| 9 | Nomor telepon harus valid: dimulai dari angka 1–9, panjang 10–15 digit (tidak termasuk dial code). |
| 10| Jika nomor telepon diisi, kode negara (dial code) **wajib** dipilih.                  |

---

### 🚫 Larangan Menghapus

| Entitas           | Kondisi Larangan                                                      |
|-------------------|-----------------------------------------------------------------------|
| **Vendor**        | Masih memiliki item/layanan yang terdaftar                           |
| **Vendor**        | Masih memiliki PO yang terkait (berstatus apapun)                   |
| **Item Vendor**   | (Tidak ada batasan teknis, tetapi sebaiknya tidak hapus jika masih digunakan di PO aktif) |

---

### 🚫 Larangan Aksi pada PO

| Aksi              | Kondisi Larangan                                                      |
|-------------------|-----------------------------------------------------------------------|
| **Edit PO**       | PO sudah disetujui (`approval` sudah terisi)                        |
| **Approve PO**    | PO sudah disetujui sebelumnya (tidak bisa approve ulang)             |
| **Reject PO**     | PO sudah disetujui (tidak bisa ditolak setelah disetujui)            |
| **Submit PO**     | PO sudah disetujui atau sudah berstatus complete                     |
| **Tanda tangan publik** | PO sudah pernah ditandatangani sebelumnya (tidak bisa tanda tangan ulang via tautan publik) |

---

### 🚫 Larangan Upload Lampiran

| Kondisi                                     | Keterangan                              |
|---------------------------------------------|-----------------------------------------|
| File melebihi **10 MB**                     | Upload akan ditolak                     |
| Format file bukan yang diizinkan            | Upload akan ditolak                     |

---

### ✅ Yang Diizinkan

| Aksi                                                  | Keterangan                                              |
|-------------------------------------------------------|---------------------------------------------------------|
| Mengubah status vendor (aktif/nonaktif)               | Bisa dilakukan kapan saja, tidak ada batasan            |
| Membuat PO baru untuk vendor yang sudah nonaktif      | Secara teknis masih bisa, namun tidak disarankan        |
| Menggunakan kembali tanda tangan tersimpan            | Bisa digunakan berulang kali di banyak PO berbeda       |
| Menyimpan tanda tangan baru ke profil saat approve PO | Akan menggantikan tanda tangan lama jika sudah ada      |

---

## 9. Referensi Cepat

### Status PO

| Status         | Ikon  | Keterangan                                                              |
|----------------|-------|-------------------------------------------------------------------------|
| **Draft**      | ⏳    | PO baru dibuat, belum disetujui                                        |
| **Disetujui**  | ✅    | PO sudah mendapat persetujuan dari Super Admin                         |
| **Ditolak**    | ❌    | PO ditolak oleh Super Admin                                            |
| **Complete**   | 🎉    | PO disetujui dan sudah ditandatangani via tautan publik                |

---

### Tipe Item Vendor

| Tipe           | Keterangan                                                              |
|----------------|-------------------------------------------------------------------------|
| **Service**    | Layanan berulang (internet, co-location, dll)                          |
| **Goods**      | Barang fisik yang dibeli                                               |
| **Contractor** | Jasa kontraktor/pekerja lapangan                                       |

---

### Tipe Harga Item

| Tipe    | Keterangan                                                               |
|---------|--------------------------------------------------------------------------|
| **OTC** | One Time Charge — biaya yang dibayar sekali                             |
| **MRC** | Monthly Recurring Cost — biaya yang dibayar setiap bulan               |

---

### Format Nomor PO

```
PO / {KODE_VENDOR} / {NOMOR_URUT} / {BULAN} / {TAHUN}
```
Contoh: `PO/TELKOM/001/06/2026`

---

### Navigasi Menu

| Halaman                         | Path URL                          |
|---------------------------------|-----------------------------------|
| Daftar Vendor                   | `/services/vendor`                |
| Detail Vendor                   | `/services/vendor/view/{vendor_id}` |
| Daftar Semua PO                 | `/services/activation` (tab PO)  |
| Review PO (Super Admin)         | `/review-po/{id}`                 |
| Dokumen PO Publik (Vendor)      | `/po-document/{token}`            |
