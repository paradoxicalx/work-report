# 📝 Daily Work Report - Dedy Putra (2026-07-14)

---

## 📅 Laporan Harian - 14 Juli 2026

---

## 🌿 Branch: `issue-115` — Halaman Settings (System & Application)

### 📌 Informasi Issue
- **Nomor Issue**: #115
- **Judul Issue**: Implementasi Halaman Settings System & Application
- **Status Branch**: `Dalam pengerjaan (Work In Progress — belum di-commit)`

### 📅 Rincian Perubahan (Belum Di-commit)

#### [Unstaged Changes] - Work In Progress - 14 Juli 2026

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/settings.controller.js` [NEW]
  - `backend/src/routes/settings.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/form/TextEditor.jsx`
  - `frontend/src/components/ui/Form/Input.jsx`
  - `frontend/src/app/pages/settings/schema/applicationSchema.js` [NEW]
  - `frontend/src/app/pages/settings/schema/systemSchema.js` [NEW]
  - `frontend/src/app/pages/settings/sections/System.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/Application.jsx` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `work-report/sample-settings/settings_detail.txt` [NEW]

- **Deskripsi Perubahan & Fungsi**:

  **Backend:**
  - **Settings Controller (Baru)**: Pembuatan controller baru `settings.controller.js` dengan fungsionalitas:
    - `readSettings`: Mengambil data pengaturan berdasarkan parameter nama (`system_settings` atau `application_settings`) dari collection Option di MongoDB.
    - `readSettingsSensitive`: Mengambil data sensitif (seperti SMTP Password, Mapbox Token, API iPaymu, dan Token Telegram) hanya jika admin memiliki privilege `settings.readSensitive`.
    - `updateSettings`: Memperbarui pengaturan menggunakan database upsert. Mendukung upload berkas multipart (company logo dan JSON Google Drive service key) ke MinIO menggunakan controller file terpusat. Mengabaikan pembaruan field sensitif jika bernilai kosong untuk menghindari penulisan ulang data rahasia secara tidak sengaja.
  - **Settings Route (Baru)**: Pembuatan router baru `settings.route.js` untuk registrasi endpoint `/settings/read`, `/settings/read/sensitive`, dan `/settings/update` yang diproteksi dengan middleware `protectedAdmin` dan otorisasi berbasis hak akses `checkPrivilege`. Route ini juga dilengkapi dengan dokumentasi Swagger JSDoc yang lengkap.
  - **Option Service Enhancement**: Modifikasi `option.service.js` untuk mendukung pengambilan data non-sensitif (proyeksi spesifik) dan sensitif secara terpisah bagi system settings dan application settings. Menyediakan fungsi `updateSystemSettings` dan `updateAppSettings` dengan logika gabungan (merge) data agar parameter yang tidak dikirimkan tetap utuh tersimpan di database.
  - **App Registration**: Mendaftarkan router settings baru di `app.js` dengan prefix `/settings`.
  - **Privilege & i18n Backend**: Penyesuaian `privilege.json` dengan menambahkan privilege `settings.read`, `settings.readSensitive`, dan `settings.update` untuk tingkat admin super. Penambahan localization key terkait operasional opsi pengaturan di file terjemahan backend (EN & ID).

  **Frontend:**
  - **System Settings Page (Baru)**: Pembuatan halaman antarmuka `System.jsx` (437 baris) dengan interface berbasis Tab yang dinamis (Radius, Hotspot, Email, dan Other):
    - **Tab Radius**: Konfigurasi port autentikasi & accounting RADIUS, profil block & drop user di Router MikroTik, batas invoice menunggak, batas retry brute-force, interval pemrosesan accounting, dan generate username/password otomatis.
    - **Tab Hotspot**: Konfigurasi port autentikasi & accounting khusus untuk pelanggan layanan Hotspot.
    - **Tab Email (SMTP)**: Pengaturan integrasi SMTP mailer (host, port, username, password).
    - **Tab Other**: Pengaturan Syslog port dan formulir unggah Google Drive Service Account JSON key untuk fitur backup otomatis.
  - **Application Settings Page (Baru)**: Pembuatan halaman antarmuka `Application.jsx` (634 baris) dengan panel pengaturan aplikasi yang komprehensif:
    - **Tab General**: Manajemen profil aplikasi, nama aplikasi, subtitle, informasi kontak instansi/perusahaan, dan sistem upload logo perusahaan terintegrasi dengan pratinjau avatar.
    - **Tab Maps**: Pengaturan zona waktu regional sistem dan akses API token Mapbox untuk modul GIS.
    - **Tab Invoice**: Kustomisasi format nama invoice otomatis, tempo hari jatuh tempo, pengingat otomatis, pesan catatan kaki invoice, dan pengiriman notifikasi via WhatsApp.
    - **Tab iPaymu**: Pengaturan detail integrasi gerbang pembayaran digital iPaymu (API Key, VA Parent, sandbox mode toggle, callback URL, dan opsi link pembayaran otomatis).
    - **Tab Telegram**: Pintu gerbang notifikasi chat bot Telegram untuk grup koordinasi internal (tiket, laporan, debug, approval, gudang, absensi).
  - **Validation Schema (Baru)**: Pembuatan file `applicationSchema.js` dan `systemSchema.js` menggunakan Yup untuk validasi isian formulir di sisi klien agar sesuai dengan tipe data dan batasan nilai di basis data.
  - **UI Form Components Update**: Modifikasi minor pada `FormInput.jsx` dan `Input.jsx` untuk kelancaran rendering state input password sensitif (toggle eye icon) dan integrasi input standar.
  - **Navigation & Routing**: Penambahan menu navigasi "System" dan "Application" di file `settings.js` navigasi, dan mendaftarkannya sebagai lazy-loaded routes di `protected.jsx` di bawah hak akses halaman pengaturan.
  - **i18n Frontend**: Penambahan masif localization key baru untuk label input, deskripsi helper tooltip, pesan validasi, dan notifikasi penyimpanan di file `translations.json` (Bahasa Inggris dan Bahasa Indonesia).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
|-------|-------|--------------|
| #115  | Halaman Settings System & Application | Membuka fungsionalitas panel kontrol administrasi utama sistem untuk mengubah parameter aplikasi (Billing, Notifikasi Bot, iPaymu Gateway, Profil Bisnis) dan sistem backend (Radius, Hotspot, Email SMTP, Syslog, Backup Google Drive). |

### Kemampuan Baru Pengguna/Admin
- Admin sekarang dapat memodifikasi seluruh informasi dasar aplikasi dan detail kontak perusahaan yang akan dicetak pada faktur invoice pelanggan secara dinamis.
- Admin dapat mengintegrasikan gateway pembayaran **iPaymu** secara mandiri dengan mengisi API Key, Virtual Account, serta mengaktifkan status sandbox dan link pembayaran otomatis.
- Admin dapat mengatur **Bot Telegram** perusahaan untuk mengirim notifikasi event operasional (seperti log debug, tiket baru, persetujuan admin, absensi, pergudangan) ke grup chat internal yang berbeda-beda secara real-time.
- Admin jaringan dapat mengatur **Radius Server** (port autentikasi, port accounting, toleransi ganda sesi, profile MikroTik) serta modul hotspot langsung dari GUI tanpa perlu menyunting file konfigurasi server manual.
- Admin dapat mengunggah file credential JSON Google Drive untuk mengaktifkan backup otomatis database berkala.

### Bug Fix / Solusi Masalah
- Menghindari risiko kebocoran data sensitif (seperti password SMTP dan API Key) ke klien biasa dengan memisahkan endpoint pembacaan data umum (`/settings/read`) dengan data sensitif (`/settings/read/sensitive`) yang dilindungi privilege super admin (`settings.readSensitive`).
- Mencegah terhapusnya password SMTP atau token Telegram lama saat admin menyimpan data tab lain melalui mekanisme penyaringan field kosong di server-side controller.
- Standardisasi penanganan form input password di frontend agar mendukung visibility toggle yang aman dan konsisten.

### Menu/Fitur Baru
- **Halaman Pengaturan Sistem**: `/settings/system` dengan Tab Radius, Hotspot, Email SMTP, dan File Backup.
- **Halaman Pengaturan Aplikasi**: `/settings/application` dengan Tab General, Maps, Invoice, iPaymu, dan Notifikasi Telegram.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur — Integrasi Notifikasi Bot Telegram**:
  Fitur ini memungkinkan backend aplikasi mendistribusikan notifikasi event penting ke grup Telegram operasional yang relevan secara otomatis. Token bot Telegram dan ID Chat grup dikonfigurasi melalui panel pengaturan aplikasi, disimpan di database MongoDB, dan digunakan oleh modul notifikasi backend saat event terpicu.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Settings > Application**, lalu pilih tab **Telegram**.
  2. Masukkan **Bot Token** Telegram resmi yang Anda dapatkan dari `@BotFather`.
  3. Masukkan ID Chat / ID Grup Telegram pada kolom yang sesuai (contoh: isi kolom **Telegram Ticket** untuk grup penanganan gangguan, **Telegram Presence** untuk grup monitoring absensi).
  4. Klik tombol **Simpan**.
  5. Sistem otomatis menggunakan bot tersebut untuk mengirim pesan real-time ke masing-masing grup sesuai kategori aktivitas yang terjadi di ekosistem Dekasimal.

- **Penjelasan Fitur — Google Drive Backup Configuration**:
  Fitur ini memfasilitasi pencadangan database MongoDB terenkripsi ke penyimpanan cloud Google Drive milik perusahaan. Integrasi memerlukan service account key berkas JSON dari Google Cloud Console.

- **Langkah Penggunaan (Tutorial)**:
  1. Siapkan Service Account Key file JSON dari Google Cloud Platform Anda.
  2. Buka menu **Settings > System**, lalu pilih tab **Other**.
  3. Pada bagian Google Drive Key, klik ikon upload atau pilih berkas JSON key Anda.
  4. Isi **Google Drive Directory ID** dengan ID folder Google Drive tempat cadangan data akan disimpan.
  5. Klik tombol **Simpan**. Cron job backup database di server backend otomatis berjalan menggunakan kredensial tersebut.
