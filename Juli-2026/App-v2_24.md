# 📝 Daily Work Report - Dedy (2026-07-24)

---

## 📅 Laporan Harian - 24 Juli 2026

---

## 🌿 Branch: `issue-123` — Sistem Dokumen Pelanggan (Quotation, PO, SO)

### 📌 Informasi Issue

- **Nomor Issue**: #123
- **Judul Issue**: Sistem Dokumen Pelanggan — Quotation, Purchase Order, Sales Order
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`255d3e0`](../commit/255d3e0badd61c58ebf9fcb748e6012f49c2b608) - resolve #123 - 24 Juli 2026, 23:49

- **Komponen yang Berubah**:
  - [`backend/src/utils/advanceProspectStatus.js`](backend/src/utils/advanceProspectStatus.js) [NEW]
  - [`backend/src/controllers/customerQuotation.controller.js`](backend/src/controllers/customerQuotation.controller.js)
  - [`backend/src/controllers/customerPO.controller.js`](backend/src/controllers/customerPO.controller.js)
  - [`backend/src/controllers/customerSO.controller.js`](backend/src/controllers/customerSO.controller.js)
  - [`backend/src/controllers/publicQuotation.controller.js`](backend/src/controllers/publicQuotation.controller.js)
  - [`backend/src/controllers/publicCustomerPO.controller.js`](backend/src/controllers/publicCustomerPO.controller.js)
  - [`backend/src/controllers/publicCustomerSO.controller.js`](backend/src/controllers/publicCustomerSO.controller.js)
  - [`backend/src/controllers/publicPO.controller.js`](backend/src/controllers/publicPO.controller.js)
  - [`backend/src/models/customerQuotation.model.js`](backend/src/models/customerQuotation.model.js)
  - [`backend/src/models/customerSO.model.js`](backend/src/models/customerSO.model.js)
  - [`backend/src/services/customerPO.service.js`](backend/src/services/customerPO.service.js)
  - [`backend/src/services/customerQuotation.service.js`](backend/src/services/customerQuotation.service.js)
  - [`backend/src/services/customerSO.service.js`](backend/src/services/customerSO.service.js)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`frontend/src/components/shared/DocumentPreviewModal.jsx`](frontend/src/components/shared/DocumentPreviewModal.jsx)
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx)
  - [`frontend/src/components/shared/form/Combobox.jsx`](frontend/src/components/shared/form/Combobox.jsx)
  - [`frontend/src/components/shared/form/FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx)
  - [`frontend/src/components/shared/form/Listbox.jsx`](frontend/src/components/shared/form/Listbox.jsx)
  - [`frontend/src/components/shared/table/DocumentActionsMenu.jsx`](frontend/src/components/shared/table/DocumentActionsMenu.jsx)
  - [`frontend/src/app/pages/users/quotation/create.jsx`](frontend/src/app/pages/users/quotation/create.jsx)
  - [`frontend/src/app/pages/users/quotation/edit.jsx`](frontend/src/app/pages/users/quotation/edit.jsx)
  - [`frontend/src/app/pages/users/quotation/QuotationDetailDrawer.jsx`](frontend/src/app/pages/users/quotation/QuotationDetailDrawer.jsx)
  - [`frontend/src/app/pages/users/quotation/QuotationDocumentPreview.jsx`](frontend/src/app/pages/users/quotation/QuotationDocumentPreview.jsx)
  - [`frontend/src/app/pages/users/quotation/QuotationPreviewModal.jsx`](frontend/src/app/pages/users/quotation/QuotationPreviewModal.jsx)
  - [`frontend/src/app/pages/users/quotation/EditQuotationDrawerCell.jsx`](frontend/src/app/pages/users/quotation/EditQuotationDrawerCell.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/CustomerPODocumentPreview.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/CustomerPODocumentPreview.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/CustomerPOReviewDrawer.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/CustomerPOReviewDrawer.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/create.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/create.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/edit.jsx`](frontend/src/app/pages/users/customerPurchaseOrder/edit.jsx)
  - [`frontend/src/app/pages/users/customerPurchaseOrder/schema/customerPOSchema.js`](frontend/src/app/pages/users/customerPurchaseOrder/schema/customerPOSchema.js)
  - [`frontend/src/app/pages/users/customerSalesOrder/CustomerSODocumentPreview.jsx`](frontend/src/app/pages/users/customerSalesOrder/CustomerSODocumentPreview.jsx)
  - [`frontend/src/app/pages/users/customerSalesOrder/CustomerSOReviewDrawer.jsx`](frontend/src/app/pages/users/customerSalesOrder/CustomerSOReviewDrawer.jsx)
  - [`frontend/src/app/pages/users/customerSalesOrder/create.jsx`](frontend/src/app/pages/users/customerSalesOrder/create.jsx)
  - [`frontend/src/app/pages/users/customerSalesOrder/edit.jsx`](frontend/src/app/pages/users/customerSalesOrder/edit.jsx)
  - [`frontend/src/app/pages/users/business/profile.jsx`](frontend/src/app/pages/users/business/profile.jsx)
  - [`frontend/src/app/pages/users/partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx)
  - [`frontend/src/app/pages/users/customer/edit.jsx`](frontend/src/app/pages/users/customer/edit.jsx)
  - [`frontend/src/app/pages/public/PublicCustomerPODocument.jsx`](frontend/src/app/pages/public/PublicCustomerPODocument.jsx)
  - [`frontend/src/app/pages/public/PublicCustomerSODocument.jsx`](frontend/src/app/pages/public/PublicCustomerSODocument.jsx)
  - [`frontend/src/app/pages/public/PublicPODocument.jsx`](frontend/src/app/pages/public/PublicPODocument.jsx)
  - [`frontend/src/app/pages/public/PublicQuotationDocument.jsx`](frontend/src/app/pages/public/PublicQuotationDocument.jsx)
  - [`frontend/src/app/pages/public/PublicSODocument.jsx`](frontend/src/app/pages/public/PublicSODocument.jsx)
  - [`frontend/src/app/pages/public/ReviewCustomerSOPage.jsx`](frontend/src/app/pages/public/ReviewCustomerSOPage.jsx)
  - [`frontend/src/app/pages/services/hotspotUser/detail.jsx`](frontend/src/app/pages/services/hotspotUser/detail.jsx)
  - [`frontend/src/app/pages/services/purchaseOrder/PODocumentPreview.jsx`](frontend/src/app/pages/services/purchaseOrder/PODocumentPreview.jsx)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)

- **Deskripsi Perubahan & Fungsi**:

  **Backend:**
  1. **Utilitas `advanceProspectStatus` (BARU)**: Mengekstrak logika transisi status funnel prospek dari inline function di controller menjadi utilitas reusable. Mendukung input flexible (objek prospect atau ID string), dynamic import terhadap `prospect.service.js`, dan error handling yang robust agar tidak memblokir alur utama jika modul Prospect belum tersedia.

  2. **Refactor Controller Quotation & PO**: Menghapus duplikasi fungsi `advanceProspectStatus` yang sebelumnya terdefinisi inline di [`customerQuotation.controller.js`](backend/src/controllers/customerQuotation.controller.js) dan [`customerPO.controller.js`](backend/src/controllers/customerPO.controller.js), digantikan dengan import dari utilitas terpusat.

  3. **Standarisasi Response Backend**: Semua controller customer document (`customerQuotation`, `customerPO`, `customerSO`) kini mengembalikan response dengan format `{ success: true, message, data }` yang konsisten, serta menggunakan `req.t()` untuk semua pesan error guna mendukung multi-bahasa.

  4. **Penambahan Field `signer_name`**: Model [`CustomerQuotation`](backend/src/models/customerQuotation.model.js) dan [`CustomerSO`](backend/src/models/customerSO.model.js) ditambahkan field `signer_name` (String) untuk menyimpan nama penandatangan dokumen dari sisi pelanggan.

  5. **Peningkatan Penandatanganan Publik**: Endpoint [`signQuotationByToken`](backend/src/controllers/publicQuotation.controller.js) kini menerima parameter `signer_name` saat pelanggan menandatangani quotation. PO otomatis yang dibuat setelah signing menggunakan format nomor `PO/{parentCode}/{seq}/{MM}/{YYYY}` yang lebih informatif. Penanganan kegagalan auto-PO ditingkatkan — jika gagal, response menyertakan flag `poCreationFailed` dan pesan peringatan agar frontend dapat menampilkannya.

  6. **i18n Backend**: Menambahkan 11 kunci terjemahan baru di [`translation.json`](backend/src/locales/en/translation.json) (id & en) untuk pesan error terkait quotation, PO, dan SO.

  **Frontend:**
  7. **`DocumentPreviewModal` — Tipe Dokumen Baru**: Komponen [`DocumentPreviewModal`](frontend/src/components/shared/DocumentPreviewModal.jsx) ditambahkan dukungan untuk 3 tipe dokumen baru: `quotation`, `customer-po`, dan `customer-so`. Setiap tipe memiliki logika fetching data spesifik (endpoint berbeda) dan fallback ke data awal jika fetching gagal.

  8. **`QuotationApprovalStatusCell` — Ikon PO/SO Terkait**: Komponen [`QuotationApprovalStatusCell`](frontend/src/components/shared/table/rows.jsx) dirombak untuk menampilkan ikon akses cepat ke dokumen PO dan SO yang terkait dengan quotation. Admin dapat langsung mengklik ikon untuk melihat pratinjau dokumen terkait tanpa harus berpindah halaman.

  9. **Refactor Form Quotation Create**: Halaman [`quotation/create.jsx`](frontend/src/app/pages/users/quotation/create.jsx) dilakukan refactor indentasi dan perbaikan penanganan error (`err.response?.data?.message || err.message`) agar pesan error dari backend dapat ditampilkan dengan benar.

  10. **Peningkatan Dokumen Preview**: Komponen [`CustomerPODocumentPreview`](frontend/src/app/pages/users/customerPurchaseOrder/CustomerPODocumentPreview.jsx) dan [`CustomerSODocumentPreview`](frontend/src/app/pages/users/customerSalesOrder/CustomerSODocumentPreview.jsx) mengalami peningkatan signifikan dalam rendering dan integrasi data.

  11. **Perbaikan Komponen Shared**: [`Combobox`](frontend/src/components/shared/form/Combobox.jsx), [`FormInput`](frontend/src/components/shared/form/FormInput.jsx), [`Listbox`](frontend/src/components/shared/form/Listbox.jsx), dan [`DocumentActionsMenu`](frontend/src/components/shared/table/DocumentActionsMenu.jsx) menerima perbaikan minor untuk kompatibilitas dan konsistensi.

  12. **i18n Frontend**: Menambahkan 101+ kunci terjemahan baru di [`translations.json`](frontend/src/i18n/locales/id/translations.json) (id & en) untuk seluruh UI terkait quotation, PO, SO, dan dokumen publik.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                        | Dampak Utama                                                                                          |
| ----- | -------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| #123  | Sistem Dokumen Pelanggan (Quotation, PO, SO) | Peningkatan besar-besaran pada seluruh siklus dokumen pelanggan dari pembuatan hingga penandatanganan |

### Kemampuan Baru Pengguna/Admin

- Admin dapat melihat **ikon akses cepat PO/SO** langsung dari kolom status approval quotation di tabel, memungkinkan navigasi instan ke dokumen terkait
- Pelanggan kini dapat **menuliskan nama penandatangan** (`signer_name`) saat menandatangani quotation melalui halaman publik
- Admin menerima **peringatan jika auto-PO gagal** dibuat setelah quotation ditandatangani pelanggan, sehingga dapat mengambil tindakan manual

### Bug Fix / Solusi Masalah

- **Duplikasi kode `advanceProspectStatus`**: Fungsi transisi status prospek yang sebelumnya terduplikasi inline di 3 controller berbeda kini dipusatkan ke satu utilitas shared, mengurangi maintenance cost dan risiko inkonsistensi
- **Error message hardcoded**: Seluruh pesan error di backend customer document kini menggunakan i18n (`req.t()`) sehingga mendukung multi-bahasa secara konsistent
- **Response format tidak konsisten**: Beberapa endpoint sebelumnya mengembalikan data langsung tanpa wrapper; kini seluruhnya menggunakan format `{ success, message, data }` yang terstandarisasi
- **Nomor PO tidak informatif**: Format nomor PO kini menyertakan kode parent (partner/prospect/customer) sehingga lebih mudah diidentifikasi

### Menu/Fitur Baru

- **Tipe dokumen `quotation`** pada `DocumentPreviewModal` — pratinjau dokumen quotation dapat diakses dari shared modal
- **Tipe dokumen `customer-po`** dan **`customer-so`** pada `DocumentPreviewModal` — pratinjau PO dan SO pelanggan terintegrasi dalam satu modal
- **Ikon PO/SO terkait** di kolom approval quotation — akses cepat ke dokumen terkait tanpa navigasi ekstra

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Peningkatan hari ini berfokus pada **Siklus Dokumen Pelanggan** — alur kerja dari Quotation → PO → SO. Setiap tahapan memiliki alur penandatanganan publik, auto-generation dokumen berikutnya, dan pratinjau dokumen yang terintegrasi. Utilitas `advanceProspectStatus` memastikan status funnel prospek selalu maju secara aman (tidak pernah menurun) saat dokumen berpindah tahapan.

- **Langkah Penggunaan (Tutorial)**:
  1. **Membuat Quotation**: Buka menu Quotation → Create → pilih Customer atau Prospect → isi detail item → Submit
  2. **Menandatangani (Pelanggan)**: Buka link publik quotation → Review dokumen → Isi nama penandatangan → Tanda tangan → PO akan otomatis dibuat
  3. **Melihat Dokumen Terkait**: Di tabel Quotation, kolom status approval kini menampilkan ikon PO dan SO — klik ikon untuk langsung membuka pratinjau dokumen terkait
  4. **Pratinjau Dokumen**: Klik ikon dokumen di tabel atau gunakan DocumentPreviewModal untuk melihat pratinjau Quotation, PO, atau SO dalam satu modal terpadu
