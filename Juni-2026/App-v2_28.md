# 馃摑 Daily Work Report - Dedy Putra (2026-06-28)

---

## 馃搶 Informasi Issue

- **Nomor Issue**: #104
- **Judul Issue**: Revisi & Audit Kode Modul Vendor + Purchase Order
- **Branch**: `issue-104`
- **Base**: `origin/master`

## 馃搮 Laporan Harian - 28 Juni 2026

---

### 馃洜锔� Pekerjaan Belum Di-commit / Work in Progress (WIP)

#### 馃毃 Task 2.1.1 (CRITICAL) 鈥� Tambahkan `checkPrivilege` pada Route `selectListVendor`

- **File**: `[backend/src/routes/vendor.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/vendor.route.js)` 鈥� Baris **79**
- **File Terkait**: `[backend/src/controllers/vendor.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/vendor.controller.js)` 鈥� Baris **59鈥�72**

**Kondisi Saat Ini (BELUM DIPERBAIKI):**

```js
// vendor.route.js, baris 79
router.get("/vendor/select-list", protectedAdmin, selectListVendor);
```

**Perbaikan yang Diperlukan:**

```js
router.get(
  "/vendor/select-list",
  protectedAdmin,
  checkPrivilege("vendor.read"),
  selectListVendor,
);
```

**Detail Teknis:**

- Endpoint `GET /vendor/select-list` saat ini **hanya** dilindungi oleh middleware `protectedAdmin` (autentikasi JWT), tanpa verifikasi privilege spesifik.
- **Dampak Keamanan**: Setiap admin yang terautentikasi 鈥� termasuk yang tidak memiliki hak akses `vendor.read` 鈥� dapat melihat daftar vendor (nama, kode, `_id`). Ini adalah **kebocoran informasi** (information disclosure).
- **Pola yang Sudah Benar** (referensi dari file yang sama):
  - Baris 62鈥�66: `POST /vendor/list` 鈫� `protectedAdmin, checkPrivilege('vendor.list'), listVendor` 鉁�
  - Baris 102鈥�106: `GET /vendor/view/:vendor_id` 鈫� `protectedAdmin, checkPrivilege('vendor.read'), readVendor` 鉁�
- Middleware `checkPrivilege` sudah diimpor di baris 29

---

#### 馃煝 Task 2.2.2 (LOW) 鈥� Konversi String Hardcoded Notifikasi Telegram ke i18n

- **File**: `[backend/src/utils/telegram.js](file:///d:/Project/DEKASIMAL_V2/backend/src/utils/telegram.js)` 鈥� Baris **606鈥�657**
- **File i18n ID**: `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)`
- **File i18n EN**: `[backend/src/locales/en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`

**Kondisi Saat Ini (BELUM DIPERBAIKI):**

Fungsi `TelegramNotifPOSubmit` membangun pesan Telegram dengan string hardcoded Bahasa Indonesia. Label yang perlu dikonversi:

- `馃搵 PURCHASE ORDER - MENUNGGU PERSETUJUAN` 鈥� judul
- `馃彚 Vendor`, `馃搮 Tanggal PO`, `馃摝 Jumlah Item`, `馃挵 Total` 鈥� label
- `馃搸 Lampiran : N dokumen` 鈥� label attachment
- `馃懆馃徎鈥嶐煉� Dibuat oleh` 鈥� label pembuat
- `LIHAT DOKUMEN PO`, `鉁嶏笍 TANDA TANGANI DOKUMEN` 鈥� teks link

**Perbaikan:** Ekstrak 9 label ke i18n keys baru di namespace `vendor.po.telegram` (ID + EN). Fungsi ini sudah memiliki error handling `try-catch` 鉁�.

---

#### 馃煝 Task 2.4.4 (LOW) 鈥� Tambahkan Batasan Maksimum Lampiran per PO

- **File**: `[backend/src/controllers/vendor.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/vendor.controller.js)` 鈥� Baris **526鈥�577** (fungsi `uploadPOAttachment`)
- **File Terkait**: `[backend/src/services/vendor.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/vendor.service.js)` 鈥� Baris **488鈥�501** (fungsi `addAttachmentToPO`)

**Kondisi Saat Ini:** Fungsi `uploadPOAttachment` hanya validasi: file existence 鉁�, size 鈮� 10MB 鉁�, file type 鉁�. **Tidak ada batasan jumlah lampiran.**

**Perbaikan yang Diperlukan:** Tambahkan pengecekan sebelum upload:

```js
const MAX_ATTACHMENTS = 20;
if (po.attachments && po.attachments.length >= MAX_ATTACHMENTS) {
  res.status(400);
  throw new Error(req.t("vendor.po.attachment.tooMany"));
}
```

**i18n Key Baru:** `vendor.po.attachment.tooMany`

- ID: "Jumlah lampiran melebihi batas maksimum (20 berkas)"
- EN: "Attachment count exceeds the maximum limit (20 files)"

**Catatan:** Pengecekan di controller (sebelum upload MinIO) untuk menghemat resource.

### 馃搮 Rincian Commit

_Belum ada commit baru pada hari ini (28 Juni 2026). Pekerjaan masih dalam tahap pengerjaan (WIP) untuk menyelesaikan **3 item tersisa** dari total 27 task audit kode branch `issue-104`._

#### Commit

| Commit Hash | Pesan           | Tanggal      |
| ----------- | --------------- | ------------ |
| `b7a3f77`   | `revision #104` | 28 Juni 2026 |

**Komponen yang berubah pada commit `b7a3f77`:**

- `backend/src/controllers/vendor.controller.js`
- `backend/src/routes/vendor.route.js`
- `backend/src/services/vendor.service.js`
- `backend/src/models/vendor.model.js`
- `frontend/src/app/pages/services/purchaseOrder/create.jsx`
- `frontend/src/app/pages/services/vendorManagement/create.jsx` [NEW]
- `frontend/src/app/pages/services/vendorManagement/detail.jsx` [NEW]
- `frontend/src/app/pages/services/vendorManagement/edit.jsx` [NEW]
- `frontend/src/app/pages/services/vendor/VendorFormDrawer.jsx` [DELETED]
- `frontend/src/components/shared/MapRouteDrawer.jsx`

### 馃搳 Status Penyelesaian Audit 鈥� Detail Per Kategori

#### 馃毃 URGENT / CRITICAL (3 task)

| ID    | Deskripsi                                                    | File                      | Status     |
| ----- | ------------------------------------------------------------ | ------------------------- | ---------- |
| 2.1.1 | `checkPrivilege('vendor.read')` di `selectListVendor`        | `vendor.route.js:79`      | 鉂� BELUM   |
| 2.4.1 | Pisahkan `share_token` & `signature` dari `VENDOR_PO_FIELDS` | `vendor.service.js`       | 鉁� Selesai |
| 3.2.1 | Ganti raw `<button>` 鈫� `<Button>` di `Modal.jsx`             | `components/ui/Modal.jsx` | 鉁� Selesai |

#### 馃敶 HIGH (6 task 鈥� SEMUA SELESAI 鉁�)

| ID    | Deskripsi                                             |
| ----- | ----------------------------------------------------- |
| 2.1.2 | `checkPrivilege` di `deleteTicketDocument`            |
| 2.1.3 | `checkPrivilege` di `getWarehouseLogImage`            |
| 2.2.1 | i18n 'file not found' di `getVendorPOFile`            |
| 2.3.1 | Validasi status PO ('draft') di `submitPO`            |
| 2.4.2 | Pengecekan PO terkait di `deleteVendorById`           |
| 3.2.2 | Ganti raw `<button>` 鈫� `<Button>` di `POReviewDrawer` |

#### 馃煛 MEDIUM (11 task 鈥� SEMUA SELESAI 鉁�)

| ID    | Deskripsi                                                       |
| ----- | --------------------------------------------------------------- |
| 2.1.4 | Perbaiki privilege `deleteBusinessDocument` 鈫� `business.update` |
| 2.3.2 | Error handling `deleteVendorById` (objek error + statusCode)    |
| 2.3.3 | `try-catch` `sendTelegramMessage` di `TelegramNotifPOSubmit`    |
| 2.4.3 | Urutan variabel sebelum `cleanFormData` di `createVendor`       |
| 2.5.1 | Ekstrak `uploadAppFile` & `removeAppFile` ke utils              |
| 2.5.2 | Ekstrak `isObjectId` duplikat ke `utils/`                       |
| 3.4.1 | Cache `companyInfo` di Redux store                              |
| 3.4.2 | Batasi refetch privilege di `AuthGuard`                         |
| 3.5.1 | Dokumentasi props deprecated di `ConfirmModal`                  |
| 3.5.2 | Ekstrak `POFormShell` untuk dipakai bersama                     |
| 4.2.1 | Standarisasi prop drawer `open`/`onClose` (Hotspot)             |

#### 馃煝 LOW (7 task)

| ID    | Deskripsi                                       | File                           | Status     |
| ----- | ----------------------------------------------- | ------------------------------ | ---------- |
| 2.2.2 | Konversi notifikasi Telegram ke i18n            | `telegram.js:606-657`          | 鉂� BELUM   |
| 2.3.4 | `createVendor` return i18n `vendor.created`     | `vendor.controller.js`         | 鉁� Selesai |
| 2.4.4 | Batasi maksimum lampiran per PO (max 20)        | `vendor.controller.js:526-577` | 鉂� BELUM   |
| 2.5.3 | Konsistensi parameter `findVendorById`          | `vendor.service.js`            | 鉁� Selesai |
| 2.5.4 | Argumen `(err)` di catch block kosong           | `vendor.service.js`            | 鉁� Selesai |
| 3.3.2 | Format i18n `toast.error` di `VendorFormDrawer` | `VendorFormDrawer.jsx`         | 鉁� Selesai |
| 3.5.4 | Ubah `isOpen`/`close` 鈫� `open`/`onClose`        | Voucher drawer                 | 鉁� Selesai |

#### 馃搳 Ringkasan Progres

```
CRITICAL:  鈻堚枅鈻堚枅鈻堚枅鈻堚枅鈻戔枒  2/3  (67%)   鈫� 1 tersisa: 2.1.1
HIGH:      鈻堚枅鈻堚枅鈻堚枅鈻堚枅鈻堚枅  6/6  (100%)  鈫� SEMUA SELESAI 鉁�
MEDIUM:    鈻堚枅鈻堚枅鈻堚枅鈻堚枅鈻堚枅 11/11 (100%)  鈫� SEMUA SELESAI 鉁�
LOW:       鈻堚枅鈻堚枅鈻堚枅鈻堚枅鈻戔枒  5/7  (71%)   鈫� 2 tersisa: 2.2.2, 2.4.4
鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€
TOTAL:     鈻堚枅鈻堚枅鈻堚枅鈻堚枅鈻戔枒 24/27 (89%)   鈫� 3 task tersisa
```

## 馃摙 Dampak Perubahan & Fungsionalitas Baru

#### 馃敀 Keamanan 鈥� Task 2.1.1 (CRITICAL)

- **Sebelum**: Endpoint `GET /vendor/select-list` hanya dilindungi autentikasi JWT (`protectedAdmin`). Setiap admin yang login dapat melihat daftar semua vendor (nama, kode, ID) 鈥� termasuk admin tanpa hak akses modul Vendor.
- **Sesudah**: Endpoint memiliki **lapisan otorisasi ganda**: admin terautentikasi **DAN** memiliki privilege `vendor.read`. Mencegah kebocoran informasi vendor.
- **File**: `backend/src/routes/vendor.route.js` baris 79 鈥� tambah 1 middleware

#### 馃寪 Internasionalisasi 鈥� Task 2.2.2 (LOW)

- **Sebelum**: Notifikasi Telegram Purchase Order seluruhnya menggunakan teks hardcoded Bahasa Indonesia.
- **Sesudah**: Semua label dan judul notifikasi menggunakan i18n, mendukung Bahasa Indonesia & Inggris.
- **File**: `backend/src/utils/telegram.js` 鈥� 9 string 鈫� i18n keys; `backend/src/locales/{id,en}/translation.json` 鈥� 9 key baru

#### 馃搸 Validasi Upload 鈥� Task 2.4.4 (LOW)

- **Sebelum**: Tidak ada batasan jumlah lampiran per Purchase Order. Pengguna dapat mengunggah file tanpa batas.
- **Sesudah**: Maksimum 20 lampiran per PO. Error 400 jika terlampaui, dicek **sebelum** upload ke MinIO.
- **File**: `backend/src/controllers/vendor.controller.js` 鈥� tambah validasi; `backend/src/locales/` 鈥� 1 key baru `vendor.po.attachment.tooMany`

## 馃摉 Informasi & Tutorial Singkat

#### Latar Belakang Audit

Branch `issue-104` memperkenalkan **3 sub-modul baru**: Vendor CRUD, Vendor Service (Item/Katalog), dan Purchase Order (PO). Audit kode menyeluruh (`report.txt`) menghasilkan **15 temuan** 鈫� **27 task perbaikan** di `work-report/task.md` dengan 4 tingkat prioritas.

#### Rencana Penyelesaian 3 Task Tersisa

**Langkah 1 鈥� Prioritas Utama (2.1.1):**

1. Buka `backend/src/routes/vendor.route.js`
2. Baris 79, ubah: `router.get('/vendor/select-list', protectedAdmin, selectListVendor);`
   Menjadi: `router.get('/vendor/select-list', protectedAdmin, checkPrivilege('vendor.read'), selectListVendor);`
3. Tidak perlu impor baru 鈥� `checkPrivilege` sudah diimpor di baris 29

**Langkah 2 鈥� i18n Notifikasi (2.2.2):**

1. Tambahkan 9 key baru di `backend/src/locales/{id,en}/translation.json` di namespace `vendor.po.telegram`
2. Di `backend/src/utils/telegram.js`, impor `i18nx` dan ganti string hardcoded dengan `i18n.t('vendor.po.telegram.xxx')`

**Langkah 3 鈥� Limit Lampiran (2.4.4):**

1. Tambahkan key `vendor.po.attachment.tooMany` di kedua file i18n
2. Di `vendor.controller.js` fungsi `uploadPOAttachment`, tambahkan pengecekan: `if (po.attachments?.length >= 20) throw error`
3. Definisikan `MAX_PO_ATTACHMENTS = 20` di level modul

---

### 馃搧 File yang Perlu Diubah (Ringkasan)

| #   | File                                           | Task         | Jenis Perubahan                   |
| --- | ---------------------------------------------- | ------------ | --------------------------------- |
| 1   | `backend/src/routes/vendor.route.js`           | 2.1.1        | Tambah 1 middleware di baris 79   |
| 2   | `backend/src/utils/telegram.js`                | 2.2.2        | Ganti 9 string hardcoded 鈫� i18n   |
| 3   | `backend/src/locales/id/translation.json`      | 2.2.2, 2.4.4 | Tambah ~10 key baru               |
| 4   | `backend/src/locales/en/translation.json`      | 2.2.2, 2.4.4 | Tambah ~10 key baru               |
| 5   | `backend/src/controllers/vendor.controller.js` | 2.4.4        | Tambah validasi jumlah attachment |

---

## 馃帹 Restrukturisasi Frontend 鈥� Vendor Management (Issue #104)

### 馃搧 Perubahan Struktur Direktori

```
frontend/src/app/pages/services/
鈹溾攢鈹€ vendor/                              鈫� [DIHAPUS] Folder lama
鈹�   鈹溾攢鈹€ VendorFormDrawer.jsx             鈫� [DIHAPUS] 鈥� diganti create.jsx + edit.jsx
鈹�   鈹溾攢鈹€ CreatePODrawer.jsx               鈫� [DIHAPUS] 鈥� diganti purchaseOrder/create.jsx
鈹�   鈹溾攢鈹€ POFormShell.jsx                  鈫� [DIHAPUS] 鈥� shell form lama
鈹�   鈹斺攢鈹€ ...
鈹�
鈹斺攢鈹€ vendorManagement/                    鈫� [BARU] Folder terstruktur
    鈹溾攢鈹€ create.jsx                       鈫� [NEW] Drawer Create Vendor (81 baris)
    鈹溾攢鈹€ edit.jsx                         鈫� [NEW] Drawer Edit Vendor (full form)
    鈹溾攢鈹€ detail.jsx                       鈫� [NEW] Halaman Detail Vendor (730+ baris)
    鈹溾攢鈹€ VendorItemDetailDrawer.jsx       鈫� [NEW] Drawer Detail Item Vendor
    鈹溾攢鈹€ VendorPODetailDrawer.jsx         鈫� [NEW] Drawer Detail Purchase Order
    鈹溾攢鈹€ schema/
    鈹�   鈹溾攢鈹€ vendorSchema.js              鈫� Yup schema validasi vendor
    鈹�   鈹斺攢鈹€ vendorPOSchema.js            鈫� Yup schema validasi purchase order
    鈹斺攢鈹€ ...
```

**Alasan Restrukturisasi:**

- Folder `vendor/` lama menggunakan pola drawer untuk semua CRUD 鈥� terbatas dan tidak scalable
- Folder `vendorManagement/` baru: **halaman penuh** (`detail.jsx`) sebagai pusat navigasi + drawer untuk quick actions
- Penamaan ulang mencegah konflik semantik dengan istilah "vendor" di modul lain
- File `VendorFormDrawer.jsx` (lama) dipecah menjadi `create.jsx` + `edit.jsx` (baru) dengan spesialisasi masing-masing

### 馃枼锔� Tampilan & UI 鈥� Halaman Detail Vendor (`detail.jsx`)

Layout **dual-column grid** (`grid-cols-12`) dengan sticky sidebar:

```
鈹屸攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
鈹�  [Icon] Nama Vendor 鈹� [Breadcrumbs]     [Edit]       鈹�  鈫� Header
鈹溾攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
鈹�  KOLOM KIRI (4/12)   鈹�  KOLOM KANAN (8/12)           鈹�
鈹�  (sticky top-20)     鈹�                                鈹�
鈹�                      鈹�  鈹屸攢鈹€ Tab: Katalog 鈹� Tab: PO 鈹€鈹€鈹愨攤
鈹�  馃搵 Info Umum        鈹�  鈹�                            鈹傗攤
鈹�  Kode, Alamat        鈹�  鈹�  [+ Tambah Item]            鈹傗攤
鈹�  Status, Tgl Dibuat  鈹�  鈹�  Table: Nama鈹俆ipe鈹侹ap鈹侶arga 鈹傗攤
鈹�                      鈹�  鈹�         Status鈹俒Edit][Hapus]鈹傗攤
鈹�  馃懁 Kontak AM        鈹�  鈹�                            鈹傗攤
鈹�  Email, Telepon      鈹�  鈹�  [+ Buat PO]               鈹傗攤
鈹�                      鈹�  鈹�  Table: No PO鈹俆gl鈹侷tem鈹俆otal 鈹傗攤
鈹�  馃枼锔� Kontak NOC      鈹�  鈹�         Status鈹俒View][Edit]  鈹傗攤
鈹�  Email, Telepon      鈹�  鈹�                            鈹傗攤
鈹�                      鈹�  鈹斺攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹樷攤
鈹�  馃摑 Deskripsi         鈹�                                鈹�
鈹�  (jika ada)           鈹�                                鈹�
鈹溾攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
鈹�  [9 Sub-Drawers/Modals: ItemDetail, PODetail,         鈹�
鈹�   POReview, DocumentPreview, EditPO, CreatePO,        鈹�
鈹�   EditVendor, VendorItemModal, ConfirmModal脳2]        鈹�
鈹斺攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
```

**Fitur Layout:**

- Kolom kiri `lg:sticky lg:top-20 lg:self-start` 鈥� tetap terlihat saat scroll
- Tab Katalog Item & Purchase Order via **Headless UI `TabGroup`**
- Setiap Card menggunakan `Box` / `Card` dengan `shadow-soft` + `dark:bg-dark-700`
- Empty state dengan icon + teks untuk item & PO yang kosong
- Semua tabel pakai `Table dense` + `TBody`, `Tr`, `Td`
- Responsive: kolom kiri full-width di mobile (`col-span-12 lg:col-span-4`)

### 馃З 9 Sub-Komponen Terintegrasi di `detail.jsx`

| #   | Komponen                 | Props                                            | Fungsi                                   |
| --- | ------------------------ | ------------------------------------------------ | ---------------------------------------- |
| 1   | `VendorEditDrawer`       | `open, onClose, cellData, onSuccess`             | Edit data vendor via drawer              |
| 2   | `VendorItemDetailDrawer` | `open, onClose, cellData`                        | Detail read-only item katalog            |
| 3   | `VendorItemModal`        | `isOpen, onClose, onSuccess, vendorId, editData` | Create/edit item katalog                 |
| 4   | `VendorPODetailDrawer`   | `open, onClose, cellData, onSuccess`             | Detail PO + aksi (submit/approve)        |
| 5   | `CreatePODrawer`         | `open, onClose, vendorId, onSuccess`             | Buat PO baru (dari purchaseOrder/create) |
| 6   | `EditPODrawer`           | `open, onClose, po, onSuccess`                   | Edit PO existing                         |
| 7   | `POReviewDrawer`         | `isOpen, onClose, data, onApprove, onReject`     | Review & approve/reject PO               |
| 8   | `DocumentPreviewModal`   | `isOpen, onClose, data, type`                    | Preview dokumen PO (PDF style)           |
| 9   | `ConfirmModal` 脳2        | `isOpen, onClose, onConfirm, isLoading`          | Konfirmasi hapus item & hapus PO         |

**Semua komponen** sudah menggunakan komponen UI standar (**ISU #3** 鉁�) 鈥� tidak ada raw `<table>`, `<button>`, atau `<input>`.

### 馃彿锔� Komponen UI Standar yang Digunakan di Modul Vendor

| Komponen                        | Sumber                             | Penggunaan                                            |
| ------------------------------- | ---------------------------------- | ----------------------------------------------------- |
| `Button`                        | `components/ui`                    | Semua tombol aksi (Edit, Hapus, Tambah, View, Submit) |
| `Badge`                         | `components/ui`                    | Status item (active/non-active) + status PO           |
| `Box`, `Card`                   | `components/ui`                    | Container card informasi vendor, item, PO             |
| `Skeleton`                      | `components/ui`                    | Loading state saat fetch data                         |
| `Table`, `TBody`, `Tr`, `Td`    | `components/ui`                    | Semua tabel data                                      |
| `Spinner`                       | `components/ui`                    | Indikator loading submit form                         |
| `ActiveBadge`                   | `components/shared/Badge`          | Status vendor aktif/nonaktif                          |
| `POApprovalStatusCell`          | `components/shared/table/rows`     | Badge status approval PO                              |
| `Breadcrumbs`                   | `components/shared/Breadcrumbs`    | Navigasi breadcrumb                                   |
| `NotificationLink`              | `components/shared/Notification`   | Toast kustom create vendor                            |
| `Checkbox`, `Radio`             | `components/ui`                    | Form PO (pajak, payment method)                       |
| `InputDefault`, `InputTextarea` | `components/shared/form/FormInput` | Input teks, deskripsi                                 |
| `InputMoney`, `InputDatePicker` | `components/shared/form/FormInput` | Input harga, tanggal                                  |
| `InputPhoneNumber`              | `components/shared/form/FormInput` | Input telepon + kode negara                           |
| `InputAppend`                   | `components/shared/form/FormInput` | Input unit + kapasitas                                |
| `Combobox`                      | `components/shared/form/Combobox`  | Pilih vendor, item katalog                            |
| `TabGroup`, `Tab`, `TabPanels`  | `@headlessui/react`                | Tab Katalog & PO                                      |
| `ConfirmModal`                  | `components/shared/ConfirmModal`   | 2脳 konfirmasi hapus                                   |
| `Page`                          | `components/shared/Page`           | Wrapper halaman                                       |

### 馃寪 i18n Frontend 鈥� Key Baru Modul Vendor

| Key                            | ID                        | EN                        |
| ------------------------------ | ------------------------- | ------------------------- |
| `vendor.accountManagerContact` | Kontak Account Manager    | Account Manager Contact   |
| `vendor.nocContact`            | Kontak NOC                | NOC Contact               |
| `vendor.itemCatalog`           | Katalog Item              | Item Catalog              |
| `vendor.item.add`              | Tambah Item               | Add Item                  |
| `vendor.item.edit`             | Edit Item                 | Edit Item                 |
| `vendor.item.added`            | Item berhasil ditambahkan | Item added successfully   |
| `vendor.item.empty`            | Belum ada item            | No items yet              |
| `vendor.po.list`               | Daftar Purchase Order     | Purchase Order List       |
| `vendor.po.create`             | Buat PO                   | Create PO                 |
| `vendor.po.created`            | PO berhasil dibuat        | PO created successfully   |
| `vendor.po.empty`              | Belum ada purchase order  | No purchase orders yet    |
| `vendor.po.items`              | Jumlah Item               | Total Items               |
| `vendor.po.itemUnit`           | item                      | item                      |
| `vendor.po.number`             | Nomor PO                  | PO Number                 |
| `vendor.po.total`              | Total                     | Total                     |
| `vendor.po.submit`             | Ajukan                    | Submit                    |
| `vendor.po.submitted`          | PO berhasil diajukan      | PO submitted successfully |
| `vendor.emailAm`               | Email AM                  | AM Email                  |
| `vendor.phoneAm`               | Telepon AM                | AM Phone                  |
| `vendor.emailNoc`              | Email NOC                 | NOC Email                 |
| `vendor.phoneNoc`              | Telepon NOC               | NOC Phone                 |

### 馃攧 State Management & Data Flow

```
detail.jsx (Parent 鈥� 730+ baris)
鈹�
鈹溾攢鈹€ useState: vendor, items, poList          鈫� Data utama dari API
鈹溾攢鈹€ useState: loadingData, loadingPO          鈫� Loading states
鈹溾攢鈹€ useState: 11脳 drawer/modal visibility     鈫� Kontrol buka/tutup
鈹�
鈹溾攢鈹€ fetchVendorData() 鈫� GET /vendor/view/:id 鈫� setVendor(), setItems()
鈹溾攢鈹€ fetchPOList()     鈫� GET /vendor-po/by-vendor/:vendorId 鈫� setPoList()
鈹�
鈹溾攢鈹€ handleDeleteItem() 鈫� DELETE /vendor-item/delete/:code  鈫� fetchVendorData()
鈹溾攢鈹€ handleDeletePO()   鈫� DELETE /vendor-po/delete/:id      鈫� fetchPOList()
鈹溾攢鈹€ handlePOApprove()  鈫� PATCH /vendor-po/approve/:id      鈫� fetchPOList()
鈹溾攢鈹€ handlePOReject()   鈫� PATCH /vendor-po/reject/:id       鈫� fetchPOList()
鈹�
鈹斺攢鈹€ Sub-components receive callbacks:
    鈹溾攢鈹€ onSuccess 鈫� fetchVendorData() / fetchPOList()
    鈹斺攢鈹€ onClose   鈫� setXxxOpen(false)
```

**Pola Refresh:** Setiap mutasi data memicu re-fetch via callback 鈥� **tanpa reload halaman penuh** (SPA pattern).

### 馃摑 File Frontend dalam Peninjauan Akhir

- **`purchaseOrder/create.jsx`** 鈥� Form PO: `useFieldArray` line_items, kalkulasi pajak otomatis, upload lampiran, Combobox katalog item, `SelectCards` untuk payment method & tax type.

- **`vendorManagement/VendorItemDetailDrawer.jsx`** 鈥� Drawer read-only detail item: nama, tipe, kapasitas, harga, SLA, kontrak, vendor terkait. Semua `Table` + `Tr`/`Td`.

- **`vendorManagement/detail.jsx`** 鈥� Halaman utama (730+ baris). Integrasi 9 sub-komponen, dual-column layout, TabGroup. **100% komponen UI standar 鈥� tidak ada raw HTML.**