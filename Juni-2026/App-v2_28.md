# 📝 Daily Work Report - Dedy Putra (2026-06-28)

---

## 📌 Informasi Issue

- **Nomor Issue**: #104
- **Judul Issue**: Revisi & Audit Kode Modul Vendor + Purchase Order
- **Branch**: `issue-104`
- **Base**: `origin/master`

## 📅 Laporan Harian - 28 Juni 2026

---

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

#### 🚨 Task 2.1.1 (CRITICAL) — Tambahkan `checkPrivilege` pada Route `selectListVendor`

- **File**: `[backend/src/routes/vendor.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/vendor.route.js)` — Baris **79**
- **File Terkait**: `[backend/src/controllers/vendor.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/vendor.controller.js)` — Baris **59–72**

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
- **Dampak Keamanan**: Setiap admin yang terautentikasi — termasuk yang tidak memiliki hak akses `vendor.read` — dapat melihat daftar vendor (nama, kode, `_id`). Ini adalah **kebocoran informasi** (information disclosure).
- **Pola yang Sudah Benar** (referensi dari file yang sama):
  - Baris 62–66: `POST /vendor/list` → `protectedAdmin, checkPrivilege('vendor.list'), listVendor` ✅
  - Baris 102–106: `GET /vendor/view/:vendor_id` → `protectedAdmin, checkPrivilege('vendor.read'), readVendor` ✅
- Middleware `checkPrivilege` sudah diimpor di baris 29

---

#### 🟢 Task 2.2.2 (LOW) — Konversi String Hardcoded Notifikasi Telegram ke i18n

- **File**: `[backend/src/utils/telegram.js](file:///d:/Project/DEKASIMAL_V2/backend/src/utils/telegram.js)` — Baris **606–657**
- **File i18n ID**: `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)`
- **File i18n EN**: `[backend/src/locales/en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`

**Kondisi Saat Ini (BELUM DIPERBAIKI):**

Fungsi `TelegramNotifPOSubmit` membangun pesan Telegram dengan string hardcoded Bahasa Indonesia. Label yang perlu dikonversi:

- `📋 PURCHASE ORDER - MENUNGGU PERSETUJUAN` — judul
- `🏢 Vendor`, `📅 Tanggal PO`, `📦 Jumlah Item`, `💰 Total` — label
- `📎 Lampiran : N dokumen` — label attachment
- `👨🏻‍💼 Dibuat oleh` — label pembuat
- `LIHAT DOKUMEN PO`, `✍️ TANDA TANGANI DOKUMEN` — teks link

**Perbaikan:** Ekstrak 9 label ke i18n keys baru di namespace `vendor.po.telegram` (ID + EN). Fungsi ini sudah memiliki error handling `try-catch` ✅.

---

#### 🟢 Task 2.4.4 (LOW) — Tambahkan Batasan Maksimum Lampiran per PO

- **File**: `[backend/src/controllers/vendor.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/vendor.controller.js)` — Baris **526–577** (fungsi `uploadPOAttachment`)
- **File Terkait**: `[backend/src/services/vendor.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/vendor.service.js)` — Baris **488–501** (fungsi `addAttachmentToPO`)

**Kondisi Saat Ini:** Fungsi `uploadPOAttachment` hanya validasi: file existence ✅, size ≤ 10MB ✅, file type ✅. **Tidak ada batasan jumlah lampiran.**

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

### 📅 Rincian Commit

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

### 📊 Status Penyelesaian Audit — Detail Per Kategori

#### 🚨 URGENT / CRITICAL (3 task)

| ID    | Deskripsi                                                    | File                      | Status     |
| ----- | ------------------------------------------------------------ | ------------------------- | ---------- |
| 2.1.1 | `checkPrivilege('vendor.read')` di `selectListVendor`        | `vendor.route.js:79`      | ❌ BELUM   |
| 2.4.1 | Pisahkan `share_token` & `signature` dari `VENDOR_PO_FIELDS` | `vendor.service.js`       | ✅ Selesai |
| 3.2.1 | Ganti raw `<button>` → `<Button>` di `Modal.jsx`             | `components/ui/Modal.jsx` | ✅ Selesai |

#### 🔴 HIGH (6 task — SEMUA SELESAI ✅)

| ID    | Deskripsi                                             |
| ----- | ----------------------------------------------------- |
| 2.1.2 | `checkPrivilege` di `deleteTicketDocument`            |
| 2.1.3 | `checkPrivilege` di `getWarehouseLogImage`            |
| 2.2.1 | i18n 'file not found' di `getVendorPOFile`            |
| 2.3.1 | Validasi status PO ('draft') di `submitPO`            |
| 2.4.2 | Pengecekan PO terkait di `deleteVendorById`           |
| 3.2.2 | Ganti raw `<button>` → `<Button>` di `POReviewDrawer` |

#### 🟡 MEDIUM (11 task — SEMUA SELESAI ✅)

| ID    | Deskripsi                                                       |
| ----- | --------------------------------------------------------------- |
| 2.1.4 | Perbaiki privilege `deleteBusinessDocument` → `business.update` |
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

#### 🟢 LOW (7 task)

| ID    | Deskripsi                                       | File                           | Status     |
| ----- | ----------------------------------------------- | ------------------------------ | ---------- |
| 2.2.2 | Konversi notifikasi Telegram ke i18n            | `telegram.js:606-657`          | ❌ BELUM   |
| 2.3.4 | `createVendor` return i18n `vendor.created`     | `vendor.controller.js`         | ✅ Selesai |
| 2.4.4 | Batasi maksimum lampiran per PO (max 20)        | `vendor.controller.js:526-577` | ❌ BELUM   |
| 2.5.3 | Konsistensi parameter `findVendorById`          | `vendor.service.js`            | ✅ Selesai |
| 2.5.4 | Argumen `(err)` di catch block kosong           | `vendor.service.js`            | ✅ Selesai |
| 3.3.2 | Format i18n `toast.error` di `VendorFormDrawer` | `VendorFormDrawer.jsx`         | ✅ Selesai |
| 3.5.4 | Ubah `isOpen`/`close` → `open`/`onClose`        | Voucher drawer                 | ✅ Selesai |

#### 📊 Ringkasan Progres

```
CRITICAL:  ████████░░  2/3  (67%)   ← 1 tersisa: 2.1.1
HIGH:      ██████████  6/6  (100%)  ← SEMUA SELESAI ✅
MEDIUM:    ██████████ 11/11 (100%)  ← SEMUA SELESAI ✅
LOW:       ████████░░  5/7  (71%)   ← 2 tersisa: 2.2.2, 2.4.4
────────────────────────────────────────
TOTAL:     ████████░░ 24/27 (89%)   ← 3 task tersisa
```

## 📢 Dampak Perubahan & Fungsionalitas Baru

#### 🔒 Keamanan — Task 2.1.1 (CRITICAL)

- **Sebelum**: Endpoint `GET /vendor/select-list` hanya dilindungi autentikasi JWT (`protectedAdmin`). Setiap admin yang login dapat melihat daftar semua vendor (nama, kode, ID) — termasuk admin tanpa hak akses modul Vendor.
- **Sesudah**: Endpoint memiliki **lapisan otorisasi ganda**: admin terautentikasi **DAN** memiliki privilege `vendor.read`. Mencegah kebocoran informasi vendor.
- **File**: `backend/src/routes/vendor.route.js` baris 79 — tambah 1 middleware

#### 🌐 Internasionalisasi — Task 2.2.2 (LOW)

- **Sebelum**: Notifikasi Telegram Purchase Order seluruhnya menggunakan teks hardcoded Bahasa Indonesia.
- **Sesudah**: Semua label dan judul notifikasi menggunakan i18n, mendukung Bahasa Indonesia & Inggris.
- **File**: `backend/src/utils/telegram.js` — 9 string → i18n keys; `backend/src/locales/{id,en}/translation.json` — 9 key baru

#### 📎 Validasi Upload — Task 2.4.4 (LOW)

- **Sebelum**: Tidak ada batasan jumlah lampiran per Purchase Order. Pengguna dapat mengunggah file tanpa batas.
- **Sesudah**: Maksimum 20 lampiran per PO. Error 400 jika terlampaui, dicek **sebelum** upload ke MinIO.
- **File**: `backend/src/controllers/vendor.controller.js` — tambah validasi; `backend/src/locales/` — 1 key baru `vendor.po.attachment.tooMany`

## 📖 Informasi & Tutorial Singkat

#### Latar Belakang Audit

Branch `issue-104` memperkenalkan **3 sub-modul baru**: Vendor CRUD, Vendor Service (Item/Katalog), dan Purchase Order (PO). Audit kode menyeluruh (`report.txt`) menghasilkan **15 temuan** → **27 task perbaikan** di `work-report/task.md` dengan 4 tingkat prioritas.

#### Rencana Penyelesaian 3 Task Tersisa

**Langkah 1 — Prioritas Utama (2.1.1):**

1. Buka `backend/src/routes/vendor.route.js`
2. Baris 79, ubah: `router.get('/vendor/select-list', protectedAdmin, selectListVendor);`
   Menjadi: `router.get('/vendor/select-list', protectedAdmin, checkPrivilege('vendor.read'), selectListVendor);`
3. Tidak perlu impor baru — `checkPrivilege` sudah diimpor di baris 29

**Langkah 2 — i18n Notifikasi (2.2.2):**

1. Tambahkan 9 key baru di `backend/src/locales/{id,en}/translation.json` di namespace `vendor.po.telegram`
2. Di `backend/src/utils/telegram.js`, impor `i18nx` dan ganti string hardcoded dengan `i18n.t('vendor.po.telegram.xxx')`

**Langkah 3 — Limit Lampiran (2.4.4):**

1. Tambahkan key `vendor.po.attachment.tooMany` di kedua file i18n
2. Di `vendor.controller.js` fungsi `uploadPOAttachment`, tambahkan pengecekan: `if (po.attachments?.length >= 20) throw error`
3. Definisikan `MAX_PO_ATTACHMENTS = 20` di level modul

---

### 📁 File yang Perlu Diubah (Ringkasan)

| #   | File                                           | Task         | Jenis Perubahan                   |
| --- | ---------------------------------------------- | ------------ | --------------------------------- |
| 1   | `backend/src/routes/vendor.route.js`           | 2.1.1        | Tambah 1 middleware di baris 79   |
| 2   | `backend/src/utils/telegram.js`                | 2.2.2        | Ganti 9 string hardcoded → i18n   |
| 3   | `backend/src/locales/id/translation.json`      | 2.2.2, 2.4.4 | Tambah ~10 key baru               |
| 4   | `backend/src/locales/en/translation.json`      | 2.2.2, 2.4.4 | Tambah ~10 key baru               |
| 5   | `backend/src/controllers/vendor.controller.js` | 2.4.4        | Tambah validasi jumlah attachment |

---

## 🎨 Restrukturisasi Frontend — Vendor Management (Issue #104)

### 📁 Perubahan Struktur Direktori

```
frontend/src/app/pages/services/
├── vendor/                              ← [DIHAPUS] Folder lama
│   ├── VendorFormDrawer.jsx             ← [DIHAPUS] — diganti create.jsx + edit.jsx
│   ├── CreatePODrawer.jsx               ← [DIHAPUS] — diganti purchaseOrder/create.jsx
│   ├── POFormShell.jsx                  ← [DIHAPUS] — shell form lama
│   └── ...
│
└── vendorManagement/                    ← [BARU] Folder terstruktur
    ├── create.jsx                       ← [NEW] Drawer Create Vendor (81 baris)
    ├── edit.jsx                         ← [NEW] Drawer Edit Vendor (full form)
    ├── detail.jsx                       ← [NEW] Halaman Detail Vendor (730+ baris)
    ├── VendorItemDetailDrawer.jsx       ← [NEW] Drawer Detail Item Vendor
    ├── VendorPODetailDrawer.jsx         ← [NEW] Drawer Detail Purchase Order
    ├── schema/
    │   ├── vendorSchema.js              ← Yup schema validasi vendor
    │   └── vendorPOSchema.js            ← Yup schema validasi purchase order
    └── ...
```

**Alasan Restrukturisasi:**

- Folder `vendor/` lama menggunakan pola drawer untuk semua CRUD — terbatas dan tidak scalable
- Folder `vendorManagement/` baru: **halaman penuh** (`detail.jsx`) sebagai pusat navigasi + drawer untuk quick actions
- Penamaan ulang mencegah konflik semantik dengan istilah "vendor" di modul lain
- File `VendorFormDrawer.jsx` (lama) dipecah menjadi `create.jsx` + `edit.jsx` (baru) dengan spesialisasi masing-masing

### 🖥️ Tampilan & UI — Halaman Detail Vendor (`detail.jsx`)

Layout **dual-column grid** (`grid-cols-12`) dengan sticky sidebar:

```
┌──────────────────────────────────────────────────────┐
│  [Icon] Nama Vendor │ [Breadcrumbs]     [Edit]       │  ← Header
├──────────────────────┬───────────────────────────────┤
│  KOLOM KIRI (4/12)   │  KOLOM KANAN (8/12)           │
│  (sticky top-20)     │                                │
│                      │  ┌── Tab: Katalog │ Tab: PO ──┐│
│  📋 Info Umum        │  │                            ││
│  Kode, Alamat        │  │  [+ Tambah Item]            ││
│  Status, Tgl Dibuat  │  │  Table: Nama│Tipe│Kap│Harga ││
│                      │  │         Status│[Edit][Hapus]││
│  👤 Kontak AM        │  │                            ││
│  Email, Telepon      │  │  [+ Buat PO]               ││
│                      │  │  Table: No PO│Tgl│Item│Total ││
│  🖥️ Kontak NOC      │  │         Status│[View][Edit]  ││
│  Email, Telepon      │  │                            ││
│                      │  └────────────────────────────┘│
│  📝 Deskripsi         │                                │
│  (jika ada)           │                                │
├──────────────────────┴───────────────────────────────┤
│  [9 Sub-Drawers/Modals: ItemDetail, PODetail,         │
│   POReview, DocumentPreview, EditPO, CreatePO,        │
│   EditVendor, VendorItemModal, ConfirmModal×2]        │
└──────────────────────────────────────────────────────┘
```

**Fitur Layout:**

- Kolom kiri `lg:sticky lg:top-20 lg:self-start` — tetap terlihat saat scroll
- Tab Katalog Item & Purchase Order via **Headless UI `TabGroup`**
- Setiap Card menggunakan `Box` / `Card` dengan `shadow-soft` + `dark:bg-dark-700`
- Empty state dengan icon + teks untuk item & PO yang kosong
- Semua tabel pakai `Table dense` + `TBody`, `Tr`, `Td`
- Responsive: kolom kiri full-width di mobile (`col-span-12 lg:col-span-4`)

### 🧩 9 Sub-Komponen Terintegrasi di `detail.jsx`

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
| 9   | `ConfirmModal` ×2        | `isOpen, onClose, onConfirm, isLoading`          | Konfirmasi hapus item & hapus PO         |

**Semua komponen** sudah menggunakan komponen UI standar (**ISU #3** ✅) — tidak ada raw `<table>`, `<button>`, atau `<input>`.

### 🏷️ Komponen UI Standar yang Digunakan di Modul Vendor

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
| `ConfirmModal`                  | `components/shared/ConfirmModal`   | 2× konfirmasi hapus                                   |
| `Page`                          | `components/shared/Page`           | Wrapper halaman                                       |

### 🌐 i18n Frontend — Key Baru Modul Vendor

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

### 🔄 State Management & Data Flow

```
detail.jsx (Parent — 730+ baris)
│
├── useState: vendor, items, poList          ← Data utama dari API
├── useState: loadingData, loadingPO          ← Loading states
├── useState: 11× drawer/modal visibility     ← Kontrol buka/tutup
│
├── fetchVendorData() → GET /vendor/view/:id → setVendor(), setItems()
├── fetchPOList()     → GET /vendor-po/by-vendor/:vendorId → setPoList()
│
├── handleDeleteItem() → DELETE /vendor-item/delete/:code  → fetchVendorData()
├── handleDeletePO()   → DELETE /vendor-po/delete/:id      → fetchPOList()
├── handlePOApprove()  → PATCH /vendor-po/approve/:id      → fetchPOList()
├── handlePOReject()   → PATCH /vendor-po/reject/:id       → fetchPOList()
│
└── Sub-components receive callbacks:
    ├── onSuccess → fetchVendorData() / fetchPOList()
    └── onClose   → setXxxOpen(false)
```

**Pola Refresh:** Setiap mutasi data memicu re-fetch via callback — **tanpa reload halaman penuh** (SPA pattern).

### 📝 File Frontend dalam Peninjauan Akhir

- **`purchaseOrder/create.jsx`** — Form PO: `useFieldArray` line_items, kalkulasi pajak otomatis, upload lampiran, Combobox katalog item, `SelectCards` untuk payment method & tax type.

- **`vendorManagement/VendorItemDetailDrawer.jsx`** — Drawer read-only detail item: nama, tipe, kapasitas, harga, SLA, kontrak, vendor terkait. Semua `Table` + `Tr`/`Td`.

- **`vendorManagement/detail.jsx`** — Halaman utama (730+ baris). Integrasi 9 sub-komponen, dual-column layout, TabGroup. **100% komponen UI standar — tidak ada raw HTML.**

16
