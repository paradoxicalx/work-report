# 📝 Daily Work Report - Dedy S.N Putra (17 Oktober 2026)

---

## 📅 Laporan Harian - 17 Oktober 2026

---

## 🌿 Branch: `issue-434` — Issue #434: Fix updateSystemSettings/updateAppSettings agar tidak menghapus pengaturan lain & Sistem Snapshot Backup/Restore Pengaturan

### 📌 Informasi Issue

- **Nomor Issue**: #434
- **Judul Issue**: Fix updateSystemSettings/updateAppSettings agar tidak menghapus pengaturan lain & Sistem Snapshot Backup/Restore Pengaturan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 17 Oktober 2026, 21:35:43 WIB via commit `b8bb57ab` dan dirilis ke `production` via commit `43883d08`)

### 📅 Rincian Commit

#### [b8bb57ab] - resolve #434 - Sabtu, 17 Oktober 2026, 21:35:43 WIB

_(Merge commit menggabungkan branch `issue-434` / commit `ee78d697` ke branch `master`)_

---

#### [ee78d697] - resolve #434 - Sabtu, 17 Oktober 2026, 19:59:47 WIB

_(Merge commit menyelaraskan perubahan branch `master` / commit `a135efa4` ke branch `issue-434`)_

---

#### [a0d85ca9] - resolve #434 - Sabtu, 17 Oktober 2026, 18:56:44 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/settingsBackup.controller.js` [NEW]
  - `backend/src/routes/settingsBackup.route.js` [NEW]
  - `backend/src/services/dbTools.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/serviceChangeRequest.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/services/settingsBackup.service.js`
  - `backend/test/integration/settingsBackup.registry.test.js`
  - `backend/test/integration/settingsBackup.routes.test.js` [NEW]
  - `backend/test/integration/settingsBackup.service.test.js`
  - `frontend/src/app/pages/settings/sections/developer/OtherTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/CreateSnapshotModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/GroupChecklist.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/ImportModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/RestoreModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/RestoreResult.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/SettingsBackupCard.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/SnapshotList.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/utils.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/settingsBackup/utils.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Controller & Route REST API Backup/Restore Pengaturan (`settingsBackup.controller.js`, `settingsBackup.route.js`)**:
    - Membangun antarmuka API lengkap untuk manajemen snapshot konfigurasi sistem:
      - `POST /api/v1/settings-backup/snapshot`: Membuat snapshot baru dengan nama, catatan, dan pilihan grup konfigurasi.
      - `GET /api/v1/settings-backup/snapshots`: Mengambil daftar snapshot tersimpan dengan metadata lengkap (ukuran, pembuat, tanggal).
      - `POST /api/v1/settings-backup/restore/:id`: Memulihkan sebagian atau seluruh grup konfigurasi dari snapshot database.
      - `GET /api/v1/settings-backup/export/:id`: Mengunduh file backup JSON terenkripsi/terverifikasi untuk arsip eksternal.
      - `POST /api/v1/settings-backup/import`: Mengunggah file backup eksternal dan memvalidasi integritas strukturnya sebelum disimpan ke database.
      - `DELETE /api/v1/settings-backup/snapshot/:id`: Menghapus snapshot tertentu yang sudah tidak diperlukan.
    - Menambahkan middleware otorisasi privilege pengembang (`settings.readSensitive` / `developer: true`) agar fitur snapshot hanya dapat diakses oleh akun berwenang.
  - **Antarmuka UI Frontend Snapshot & Backup Pengaturan (`SettingsBackupCard.jsx` dkk.)**:
    - Menambahkan kartu panel **Backup & Restore Pengaturan** pada halaman **Pengaturan → Pengembang → Tab Lainnya**.
    - `CreateSnapshotModal.jsx`: Modal pembuatan snapshot dengan deskripsi dan selektor grup modul (System, Application, Finance, AI Agent, Notifikasi, dsb.).
    - `SnapshotList.jsx`: Tabel daftar snapshot aktif dengan aksi Cepat Buat, Restore, Ekspor File, dan Hapus.
    - `RestoreModal.jsx` & `GroupChecklist.jsx`: Modal pemulihan yang memungkinkan admin memilih secara selektif grup konfigurasi mana saja yang ingin di-restore tanpa harus menimpa seluruh sistem.
    - `ImportModal.jsx`: Modal unggah file backup JSON dengan validasi skema di sisi klien dan server.
    - `RestoreResult.jsx`: Dialog laporan hasil restore yang merinci key mana saja yang berhasil dipulihkan dan efek samping layanan apa saja yang di-trigger ulang.

---

#### [5776bdb2] - save #434 - Sabtu, 17 Oktober 2026, 14:41:03 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/settingsSnapshot.model.js` [NEW]
  - `backend/src/services/appLogo.service.js`
  - `backend/src/services/dbBackupRetention.service.js`
  - `backend/src/services/dbTools.service.js`
  - `backend/src/services/dbToolsR2.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/menuLayout.service.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/src/services/settingsBackup.registry.js` [NEW]
  - `backend/src/services/settingsBackup.service.js` [NEW]
  - `backend/src/services/settingsSideEffects.service.js` [NEW]
  - `backend/src/services/syslog.service.js`
  - `backend/src/utils/googleDrive.util.js`
  - `backend/src/utils/validation-data.js`
  - `backend/test/helpers/mockExpress.js`
  - `backend/test/integration/settingsBackup.registry.test.js` [NEW]
  - `backend/test/integration/settingsBackup.service.test.js` [NEW]
  - `backend/test/integration/settingsKeepUnknownKeys.test.js` [NEW]
  - `backend/test/integration/settingsSideEffects.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayChannelsPanel.jsx`
  - `frontend/src/app/pages/finance/gateway/PaymentInstructionsModal.jsx`
  - `frontend/src/app/pages/finance/payroll/runs/SettingsModal.jsx`
  - `frontend/src/app/pages/finance/regulatory/SettingsModal.jsx`
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/OperationalSettings.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/app/pages/settings/sections/developer/CronWorkerTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/OtherTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/SpecialSettingsTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/dbTools/BackupCollectionsModal.jsx`
  - `frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx`
  - `frontend/src/components/shared/LoadFailedNotice.jsx` [NEW]
  - `frontend/src/utils/appendFormData.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pencegahan Penghapusan/Penimpaan Pengaturan Lain Secara Diam-diam (`settings.controller.js`, `option.service.js`)**:
    - Mengubah mekanisme penyimpanan `updateSystemSettings`, `updateAppSettings`, dan pengaturan `ai_agent` agar penulisan ke dokumen Mongoose dilakukan strictly per-key hanya untuk key yang diizinkan oleh registri skema.
    - Menghilangkan celah di mana form parsial atau form yang belum selesai memuat secara tidak sengaja menimpa key konfigurasi lain dengan nilai kosong atau default.
    - Pada `finance_settings`: membuang key berbahaya yang mengandung karakter titik (`.`) atau simbol dollar (`$`) yang berpotensi merusak struktur database MongoDB.
    - Pengaturan grup Telegram dan aksi hapus host syslog kini tidak lagi menulis ulang seluruh parameter dokumen opsi, melainkan melakukan pembaruan atomik pada field target.
  - **Perbaikan Serialisasi Data Form (`appendFormData.js`)**:
    - Menambahkan penanda `__json_fields` saat menyusun payload FormData, sehingga hanya field yang memang dikonfigurasi sebagai JSON yang di-parse kembali di sisi backend, sementara payload body JSON standar tidak lagi di-parse ulang yang dapat menyebabkan korupsi tipe data.
  - **Proteksi Form Pengaturan Gagal Muat (`LoadFailedNotice.jsx`)**:
    - Membangun komponen `LoadFailedNotice` pada seluruh tab formulir pengaturan frontend: jika request pengambilan data awal gagal (misalnya karena gangguan jaringan atau backend sibuk), tombol **Simpan** akan otomatis dikunci dan kotak peringatan ditampilkan, mencegah admin menimpa data server dengan state kosong.
  - **Model & Registri Grup Backup Pengaturan (`settingsSnapshot.model.js`, `settingsBackup.registry.js`)**:
    - Membuat model data `SettingsSnapshot` untuk menyimpan salinan konfigurasi sistem, metadata admin pembuat, daftar grup, serta hash checksum data.
    - Menyusun registri modul konfigurasi `SETTINGS_BACKUP_GROUPS` dengan validasi ketat untuk `app_logo`, `grpc_backend_addr`, kredensial gateway `ipay`, dan `telegram_topic`.
  - **Sentralisasi Efek Samping Penyimpanan Pengaturan (`settingsSideEffects.service.js`)**:
    - Memisahkan seluruh efek samping penyimpanan (seperti reload hot-config RADIUS server, inisialisasi ulang WhatsApp Baileys, sinkronisasi Cron Worker, broadcast Telegram bot, refresh sensor kata kasar profanity, dan penjadwalan reminder tagihan) ke dalam satu service terpusat.
    - Efek samping ini kini dapat dipanggil ulang secara aman dan konsisten baik saat admin menekan tombol Simpan di UI maupun saat proses restore snapshot dijalankan.
  - **Pengujian Integrasi Komprehensif**:
    - Mengembangkan rangkaian unit & integration test: `settingsKeepUnknownKeys.test.js`, `settingsBackup.registry.test.js`, `settingsBackup.service.test.js`, dan `settingsSideEffects.test.js`.

---

## 🌿 Branch: `issue-433` — Issue #433: Improve AI Agents (Basis Pengetahuan, Evaluasi Kualitas, Sistem Umpan Balik & Optimasi Lapisan LLM)

### 📌 Informasi Issue

- **Nomor Issue**: #433
- **Judul Issue**: Improve AI Agents
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-433`, ahead of `origin/issue-433` by 17 commits)

### 📅 Rincian Commit

#### [5a061333] - save #433: perbaikan temuan Minor review AI Agent - Sabtu, 17 Oktober 2026, 20:53:09 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/models/aiConversation.model.js`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/aiConversation.service.js`
  - `backend/src/services/aiFeedback.service.js`
  - `backend/src/services/appIndex.service.js`
  - `backend/src/services/codeInspector.service.js`
  - `backend/src/services/endpointCatalog.service.js`
  - `backend/src/services/knowledgeBase.service.js`
  - `backend/src/utils/aiVocabulary.js`
  - `backend/src/utils/codeSnippet.js`
  - `backend/src/utils/generate-ai-knowledge.js`
  - `backend/test/integration/aiAgent.service.test.js`
  - `backend/test/integration/aiConversation.service.test.js` [NEW]
  - `backend/test/integration/aiFeedback.service.test.js`
  - `backend/test/unit/aiVocabulary.test.js`
  - `backend/test/unit/codeInspector.test.js`
  - `backend/test/unit/codeSnippet.test.js`
  - `frontend/src/app/pages/settings/sections/aiAgent/AiFeedbackSection.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan seluruh temuan review minor pada modul AI Agent:
    - Memperbaiki penanganan pemotongan teks cache jawaban agar tidak terpotong di tengah entitas Markdown.
    - Menyelaraskan normalisasi salam pembuka (*greeting*) percakapan pengguna.
    - Membatasi batas memori cache inspeksi kode di `codeInspector.service.js` agar memori Node.js tetap stabil pada repositori besar.
    - Menyempurnakan pembersihan data dan sanitasi pada `AiFeedbackSection.jsx` agar data sesi audit tetap aman dari kebocoran data hidup.
    - Menambahkan pengujian integrasi baru `aiConversation.service.test.js`.

---

#### [b5895837] - save: perbaikan temuan review (cache terpotong, sapaan, tautan bertitle, batas cache kode) - Sabtu, 17 Oktober 2026, 20:10:24 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/aiFeedback.service.js`
  - `backend/src/services/codeInspector.service.js`
  - `backend/src/services/knowledgeBase.service.js`
  - `backend/src/utils/aiAnswerLinks.js`
  - `backend/test/integration/aiFeedback.service.test.js`
  - `backend/test/unit/aiAnswerLinks.test.js`
  - `backend/test/unit/codeInspector.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menangani sanitasi tautan Markdown bertitle pada `aiAnswerLinks.js` agar sintaks `[label](url "title")` divalidasi dan dinormalisasi secara presisi.
  - Memperbaiki logika deteksi cache yang terpotong pada `aiFeedback.service.js` dan `knowledgeBase.service.js`.
  - Menerapkan batas maksimal entri cache pada modul AST/inspeksi kode untuk mencegah kebocoran memori.

---

#### [616bf3e7] - save: AGENTS.md aturan basis pengetahuan & umpan balik AI Agent - Sabtu, 17 Oktober 2026, 15:10:37 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
- **Deskripsi Perubahan & Fungsi**:
  - Mendokumentasikan aturan baku arsitektur monorepo pada **AGENTS.md Bab 28 (Lapisan LLM & AI Agent)**:
    - Spesifikasi pembuatan dan sinkronisasi `features.json` via `utils/generate-ai-knowledge.js` (`npm run gknow`).
    - Aturan penulisan dokumen panduan resmi di `src/data/aiKnowledge/docs/*.md` dan generator draf `npm run ai:draft-doc`.
    - Aturan caching jawaban terverifikasi pada koleksi `knowledge_bases` hanya untuk jawaban pertama berpenilaian 👍 tanpa data hidup sensitif.
    - Larangan kebocoran data hidup pada daftar umpan balik `ai_feedbacks` (proteksi `settings.readSensitive`).
    - Standarisasi kosakata pencarian terpusat di `utils/aiVocabulary.js` dan kewajiban evaluasi kualitas via `npm run ai:eval`.

---

#### [aebcbe99] - save: format jawaban & validasi tautan AI Agent - Sabtu, 17 Oktober 2026, 15:03:37 WIB

- **Komponen yang Berubah**:
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/utils/aiAnswerLinks.js` [NEW]
  - `backend/test/integration/aiAgent.service.test.js`
  - `backend/test/unit/aiAnswerLinks.test.js` [NEW]
  - `frontend/src/components/template/RightSidebar/index.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Mengembangkan modul validasi tautan jawaban `aiAnswerLinks.js`: memindai setiap tautan rute internal aplikasi pada respons AI Agent dan mencocokkannya dengan rute valid di frontend Dekasimal.
  - Memastikan agen tidak menghasilkan tautan navigasi rusak (*dead link* / 404).

---

#### [753deeca] - save: draf dokumen panduan AI Agent - Sabtu, 17 Oktober 2026, 15:02:55 WIB

- **Komponen yang Berubah**:
  - `.gitignore`
  - `backend/package.json`
  - `backend/scripts/ai-knowledge/draftDoc.js` [NEW]
  - `backend/scripts/ai-knowledge/draftPrompt.js` [NEW]
  - `backend/src/constants/llm.constant.js`
  - `backend/test/unit/aiDraftPrompt.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun script generator draf panduan fitur otomatis `backend/scripts/ai-knowledge/draftDoc.js` (`npm run ai:draft-doc -- /path-menu`).
  - Menggunakan LLM untuk menganalisis rute, komponen frontend, skema form, dan izin akses guna menyusun draf dokumentasi Markdown terstruktur untuk ditinjau oleh tim pengembang.

---

#### [c44a4508] - save: read_function & anggaran iterasi AI Agent - Sabtu, 17 Oktober 2026, 15:02:10 WIB

- **Komponen yang Berubah**:
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/codeInspector.service.js`
  - `backend/test/integration/aiAgent.service.test.js`
  - `backend/test/unit/codeInspector.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan tool baru `read_function` bagi AI Agent: agen dapat membaca blok fungsi tertentu secara atomik dari berkas kode sumber tanpa harus memuat seluruh isi berkas.
  - Menetapkan batas anggaran iterasi pemanggilan tool (*iteration budget*) agar agen tidak masuk ke dalam siklus pemanggilan tool tanpa akhir.

---

#### [5182499e] - save: pencarian kode berperingkat dengan cuplikan fungsi - Sabtu, 17 Oktober 2026, 15:01:27 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/codeInspector.service.js`
  - `backend/src/utils/codeSnippet.js` [NEW]
  - `backend/test/unit/codeInspector.test.js`
  - `backend/test/unit/codeSnippet.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Meningkatkan kualitas pencarian kode (`codeInspector.service.js` & `codeSnippet.js`): hasil pencarian kode kini menyajikan cuplikan blok fungsi lengkap dengan penilaian relevansi berperingkat (*ranked code search*).
  - Mengembangkan pengujian unit `codeSnippet.test.js` dan `codeInspector.test.js`.

---

#### [07f4735c] - save: tool search_knowledge untuk AI Agent - Sabtu, 17 Oktober 2026, 14:59:29 WIB

- **Komponen yang Berubah**:
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/services/aiAgent.service.js`
  - `backend/test/integration/aiAgent.service.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Mendaftarkan tool `search_knowledge` pada agen AI untuk mencari informasi panduan fitur dan tata cara penggunaan modul langsung dari basis pengetahuan resmi aplikasi.
  - Membatasi hasil pencarian hanya pada modul-modul yang diizinkan oleh hak akses pengguna penanya.

---

#### [1ac4b7d7] - save: pencarian basis pengetahuan AI Agent - Sabtu, 17 Oktober 2026, 14:58:50 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/docs/_FORMAT.md` [NEW]
  - `backend/src/services/aiKnowledge.service.js` [NEW]
  - `backend/test/unit/aiKnowledge.service.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun service `aiKnowledge.service.js` yang bertindak sebagai mesin pencari semantik & leksikal di atas `features.json` dan folder dokumentasi `src/data/aiKnowledge/docs/`.
  - Menetapkan panduan format berkas dokumentasi panduan fitur di `_FORMAT.md`.

---

#### [b127ec8d] - save: generator basis pengetahuan per fitur - Sabtu, 17 Oktober 2026, 14:56:39 WIB

- **Komponen yang Berubah**:
  - `backend/package.json`
  - `backend/src/data/aiKnowledge/features.json` [NEW]
  - `backend/src/server.js`
  - `backend/src/utils/aiKnowledgeBuilder.js` [NEW]
  - `backend/src/utils/aiKnowledgeExtract.js` [NEW]
  - `backend/src/utils/generate-ai-knowledge.js` [NEW]
  - `backend/src/utils/projectPaths.js`
  - `backend/test/unit/aiKnowledgeBuilder.test.js` [NEW]
  - `backend/test/unit/aiKnowledgeExtract.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun sistem ekstraksi otomatis fitur sistem Dekasimal:
    - `aiKnowledgeExtract.js`: Mengekstrak struktur menu navigasi, endpoint controller, skema validasi Yup, hak akses privilege, dan changelog rilis.
    - `aiKnowledgeBuilder.js`: Menggabungkan hasil ekstraksi menjadi berkas basis pengetahuan statis `features.json`.
    - Script `npm run gknow` (`utils/generate-ai-knowledge.js`) yang dijalankan otomatis saat boot backend di lingkungan non-Docker atau manual oleh pengembang.

---

#### [565480d1] - save: pemindai route Express bersama - Sabtu, 17 Oktober 2026, 14:54:02 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/endpointCatalog.service.js`
  - `backend/src/utils/routeScanner.js` [NEW]
  - `backend/test/unit/routeScanner.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mengabstraksi logika inspeksi route Express ke dalam utilitas bersama `utils/routeScanner.js`.
  - Menyeragamkan cara katalog endpoint dan generator basis pengetahuan mengenali path, metode HTTP, controller handler, dan middleware otorisasi.

---

#### [6420100e] - save: kosakata pencarian AI bersama - Sabtu, 17 Oktober 2026, 14:53:20 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/appIndex.service.js`
  - `backend/src/services/endpointCatalog.service.js`
  - `backend/src/services/knowledgeBase.service.js`
  - `backend/src/services/privilegeDictionary.service.js`
  - `backend/src/utils/aiVocabulary.js` [NEW]
  - `backend/test/unit/aiVocabulary.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun modul kosakata terpusat `utils/aiVocabulary.js`: kamus pemetaan sinonim dwibahasa (Indonesia - Inggris), daftar stop words, normalisasi kata hubung, dan stemming sederhana.
  - Diintegrasikan ke seluruh modul pencarian: `appIndex`, `endpointCatalog`, `knowledgeBase`, dan `privilegeDictionary`.

---

#### [05f2b9d5] - save: set evaluasi AI Agent & runner - Sabtu, 17 Oktober 2026, 14:52:15 WIB

- **Komponen yang Berubah**:
  - `.gitignore`
  - `backend/package.json`
  - `backend/scripts/ai-eval/questions.json` [NEW]
  - `backend/scripts/ai-eval/run.js` [NEW]
  - `backend/scripts/ai-eval/score.js` [NEW]
  - `backend/test/unit/aiEvalScore.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun suite evaluasi otomatis kemampuan AI Agent:
    - `questions.json`: Kumpulan pertanyaan evaluasi uji kasus nyata (navigasi, hak akses, troubleshooting, query modul).
    - `run.js`: Runner pengujian yang menjalankan agen dengan model sungguhan dan database temporary.
    - `score.js`: Modul penilaian otomatis yang mengevaluasi kepatuhan format jawaban, ketepatan tool yang dipanggil, dan akurasi rute yang disarankan (`npm run ai:eval`).

---

#### [3af2f517] - save: tombol penilaian jawaban AI Agent & daftar tinjauan - Sabtu, 17 Oktober 2026, 14:51:26 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx`
  - `frontend/src/app/pages/settings/sections/aiAgent/AiFeedbackSection.jsx` [NEW]
  - `frontend/src/components/template/RightSidebar/AnswerFeedback.jsx` [NEW]
  - `frontend/src/components/template/RightSidebar/answerFeedback.js` [NEW]
  - `frontend/src/components/template/RightSidebar/answerFeedback.test.js` [NEW]
  - `frontend/src/components/template/RightSidebar/index.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Menghadirkan komponen antarmuka umpan balik `AnswerFeedback.jsx` dengan tombol jempol 👍 dan 👎 pada setiap gelembung jawaban agen di sidebar obrolan kanan.
  - Membangun panel daftar tinjauan umpan balik `AiFeedbackSection.jsx` pada menu **Pengaturan → AI Agent**, memudahkan admin meninjau pertanyaan-pertanyaan yang mendapat rating positif atau negatif dari pengguna.

---

#### [12f2a22b] - save: daftar umpan balik AI tidak membocorkan data hidup - Sabtu, 17 Oktober 2026, 14:49:43 WIB

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/models/aiFeedback.model.js`
  - `backend/src/routes/aiAgent.route.js`
  - `backend/src/services/aiFeedback.service.js`
  - `backend/test/integration/aiFeedback.service.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Memperketat proteksi privasi pada koleksi dan endpoint `AiFeedback`: isi teks jawaban mentah yang berpotensi memuat data pelanggan / data operasional tidak disalin ke dokumen umpan balik.
  - Menggerbangi endpoint pembacaan daftar umpan balik dengan hak akses sensitif `settings.readSensitive`.

---

#### [e67386c0] - save: umpan balik jawaban AI Agent & cache terverifikasi - Sabtu, 17 Oktober 2026, 14:48:57 WIB

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/aiConversation.model.js`
  - `backend/src/models/aiFeedback.model.js` [NEW]
  - `backend/src/models/knowledgeBase.model.js`
  - `backend/src/routes/aiAgent.route.js`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/aiConversation.service.js`
  - `backend/src/services/aiFeedback.service.js` [NEW]
  - `backend/src/services/knowledgeBase.service.js`
  - `backend/test/integration/aiAgent.service.test.js`
  - `backend/test/integration/aiFeedback.controller.test.js` [NEW]
  - `backend/test/integration/aiFeedback.service.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membangun model `AiFeedback` dan alur caching jawaban terverifikasi:
    - Saat admin memberikan 👍 pada jawaban pembuka percakapan, sistem secara otomatis memvalidasi jawaban tersebut dan menyimpannya ke koleksi `knowledge_bases` sebagai jawaban terverifikasi cepat untuk pertanyaan serupa di masa mendatang.
    - Saat respon diberi penilaian 👎, entri cache terkait langsung dihapus secara otomatis.

---

#### [8bd5637d] - save: lapisan LLM AI lebih cepat & andal (retry, timeout per fitur, job latar, model cepat, path Docker) + rencana kualitas jawaban - Sabtu, 17 Oktober 2026, 14:46:58 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/package.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/constants/llm.constant.js` [NEW]
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/controllers/log.controller.js`
  - `backend/src/controllers/privilege.controller.js`
  - `backend/src/controllers/syslog.controller.js`
  - `backend/src/data/aiNavigationRoutes.json` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/log.route.js`
  - `backend/src/routes/syslog.route.js`
  - `backend/src/server.js`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/aiConversation.service.js`
  - `backend/src/services/apiLog.service.js`
  - `backend/src/services/appIndex.service.js`
  - `backend/src/services/codeInspector.service.js`
  - `backend/src/services/endpointCatalog.service.js`
  - `backend/src/services/knowledgeBase.service.js`
  - `backend/src/services/llmAdapter.service.js`
  - `backend/src/services/llmJob.service.js` [NEW]
  - `backend/src/services/option.service.js`
  - `backend/src/services/privilegeAi.service.js`
  - `backend/src/services/privilegeDictionary.service.js`
  - `backend/src/services/syslog.service.js`
  - `backend/src/services/waTicket.service.js`
  - `backend/src/utils/generate-ai-navigation.js` [NEW]
  - `backend/src/utils/llmErrorMessage.js` [NEW]
  - `backend/src/utils/navigationParser.js` [NEW]
  - `backend/src/utils/projectPaths.js` [NEW]
  - `backend/test/integration/aiAgent.service.test.js` [NEW]
  - `backend/test/integration/llmJob.controller.test.js` [NEW]
  - `backend/test/unit/endpointCatalog.test.js` [NEW]
  - `backend/test/unit/llmAdapter.test.js` [NEW]
  - `backend/test/unit/llmErrorMessage.test.js` [NEW]
  - `backend/test/unit/llmJob.test.js` [NEW]
  - `backend/test/unit/privilegePromptContext.test.js` [NEW]
  - `backend/test/unit/projectPaths.test.js` [NEW]
  - `frontend/src/app/pages/network/syslog/components/SyslogAiAnalysisDrawer.jsx`
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx`
  - `frontend/src/app/pages/settings/sections/developer/logs/AiLogAnalysisModal.jsx`
  - `frontend/src/app/pages/settings/sections/developer/logs/components/AiAnalysisModal.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/llmJob.js` [NEW]
  - `frontend/src/utils/llmJob.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Refactoring & Penguatan Lapisan LLM Adapter (`llmAdapter.service.js`, `llm.constant.js`)**:
    - Mewajibkan seluruh pemanggilan model AI di sistem menggunakan profil fitur terdaftar di `LLM_FEATURE_PROFILES`.
    - Menangani otomatisasi retry pada kode HTTP 429 dan 5xx, penyesuaian parameter `max_tokens` ke `max_completion_tokens`, deteksi respons terpotong (*truncated response*), serta pengawas jeda stream (*stream stall watchdog*) untuk mencegah koneksi SSE menggantung tanpa batas.
    - Menambahkan dukungan model tier `fast` (`ai_fast_model_name`) untuk fitur-fitur yang membutuhkan latensi ultra-cepat.
  - **Manajemen Job AI Latar Belakang (`llmJob.service.js`, `frontend/src/utils/llmJob.js`)**:
    - Mengakomodasi operasi analisis berdurasi lama (analisis log syslog dan audit trail API): server menunggu selama 20 detik, jika belum selesai klien menerima status `202 Accepted` beserta `job_id`, dan frontend melakukan polling secara asinkron hingga analisis rampung.
  - **Resolusi Path Docker yang Aman & Navigasi Statis (`projectPaths.js`, `generate-ai-navigation.js`)**:
    - Mengatasi masalah pencarian berkas kode pada kontainer Docker yang sebelumnya menghasilkan path `/` kosong tanpa error.
    - Mengekstrak seluruh rute navigasi frontend ke `src/data/aiNavigationRoutes.json` agar AI Agent selalu dapat membaca peta rute aplikasi bahkan saat berjalan di image Docker backend yang terisolasi.
  - **Prompt Kontekstual Hak Akses Berlapis (`buildPrivilegePromptContext`)**:
    - Mengoptimalkan token prompt dengan membagi konteks hak akses: katalog ringkas modul di bagian depan (memanfaatkan prompt caching LLM) dan rincian modul hanya disuntikkan sesuai topik obrolan aktif.

---

## 🌿 Branch: `issue-435` — Issue #435: Fix radius blocked profile & RADIUS Server Health Monitoring

### 📌 Informasi Issue

- **Nomor Issue**: #435
- **Judul Issue**: Fix radius blocked profile
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 17 Oktober 2026, 18:22:34 WIB via commit `a135efa4` dan dirilis ke `production` via commit `9669be32` / `43883d08`)

### 📅 Rincian Commit

#### [a135efa4] - resolve #435 - Sabtu, 17 Oktober 2026, 18:22:34 WIB

_(Merge commit menggabungkan branch `issue-435` / commit `16f44e74` ke branch `master`)_

---

#### [16f44e74] - resolve #435 - Sabtu, 17 Oktober 2026, 16:46:51 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/radiusSession.model.js`
  - `backend/src/services/radiusStats.service.js`
  - `frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/PppoeDisconnectPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/cmd/radiusd/main.go`
  - `radius-server/cmd/radiusd/wire.go`
  - `radius-server/cmd/radiusd/wire_test.go` [NEW]
  - `radius-server/internal/domain/auth/authenticate.go`
  - `radius-server/internal/domain/auth/authenticate_test.go`
  - `radius-server/internal/domain/ports/repositories.go`
  - `radius-server/internal/repository/mongo/hotspot_session_repo.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/internal/transport/pppoe/auth_listener_test.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Perlindungan Serangan Brute-Force & Loop Reconnect Klien Tidak Terdaftar (`authenticate.go`, `DOCUMENTATION.md`)**:
    - Memperluas cakupan logika `limit_try_counter` di server RADIUS: sebelumnya mekanisme blokir sementara hanya aktif jika username terdaftar tetapi passwordnya salah.
    - Kini, jika username tidak ditemukan dalam database (misalnya pengguna yang layanannya sudah dihapus atau username keliru) dan mencoba login berulang kali melampaui `limit_try_counter`, RADIUS server akan menghentikan pengiriman paket `Access-Reject` yang memicu reconnect loop tak berujung.
    - Sebagai gantinya, RADIUS server membalas dengan **Access-Accept** yang membawa profil kurungan `mikrotik_block_profile`. Klien PPPoE masuk ke grup isolir tanpa akses internet, menghentikan banjir log kegagalan login di MikroTik dan menghemat kapasitas CPU server.
    - Keputusan autentikasi ditandai secara eksplisit sebagai `BlockedApplied` dan status sesi dicatat sebagai `blocked`.
    - Autentikasi berstatus `blocked` tidak pernah memicu `UpdateOnLogin` (tidak memperbarui `last_login` dan `last_ip`), mencegah pencatatan login palsu pada akun atau perekaman alamat penyerang.
  - **Loop Monitor Kesehatan RADIUS & Heartbeat gRPC (`wire.go`, `wire_test.go`)**:
    - Mengimplementasikan `publishHealthLoop` pada daemon RADIUS (`radiusd`) untuk mempublikasikan pesan `HealthStatus` secara periodik (setiap 30 detik serta sekali saat boot) via bidi-stream gRPC ke Backend.
    - Menghitung metrik aktifitas sesi secara akurat:
      - `ActivePPPoESessions`: dihitung dari koleksi `radius_session` (`isOnline: true`), dengan fallback ke `radius_authentication` jika NAS berjalan tanpa RADIUS accounting.
      - `ActiveHotspotSessions`: dihitung dari `hotspot_session` (`isOnline: true`) dengan fallback ke `hotspot_voucher`.
      - `Goroutines`: memantau konsumsi thread goroutine runtime Go (`runtime.NumGoroutine()`).
  - **Penyesuaian Antarmuka Dashboard RADIUS (`LiveLogPanel.jsx` dkk.)**:
    - Menambahkan filter dan visualisasi status `blocked` pada tabel log langsung autentikasi RADIUS di frontend Dekasimal.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #434  | Fix updateSystemSettings/updateAppSettings agar tidak menghapus pengaturan lain & Sistem Snapshot Backup/Restore Pengaturan | Menghilangkan risiko hilangnya konfigurasi sistem saat form pengaturan disimpan sebagian atau gagal memuat, memisahkan efek samping simpan secara terpusat, serta menyediakan fasilitas pembuatan snapshot, pencadangan (backup), pemulihan granular (restore), dan ekspor/impor pengaturan sistem. |
| #433  | Improve AI Agents (Basis Pengetahuan, Evaluasi, Umpan Balik & Optimasi LLM) | Meningkatkan kecepatan, stabilitas, dan akurasi AI Agent Dekasimal secara drastis melalui lapisan LLM tangguh (retry, timeout dinamis, job latar asinkron), ekstraksi basis pengetahuan fitur otomatis (`features.json`), tool `search_knowledge` & `read_function`, cache jawaban terverifikasi dari penilaian jempol 👍 pengguna, serta suite evaluasi kualitas otomatis (`npm run ai:eval`). |
| #435  | Fix radius blocked profile & RADIUS Server Health Monitoring | Menghentikan loop reconnect tak berujung pada klien PPPoE yang layanannya telah dihapus atau menggunakan username tidak terdaftar dengan menerapkan `mikrotik_block_profile`, serta menghadirkan monitoring heartbeat dan metrik sesi online real-time dari RADIUS Server ke Backend. |

### Kemampuan Baru Pengguna/Admin

- **Pembuatan & Pemulihan Snapshot Pengaturan Sistem**: Admin dapat mengambil snapshot pengaturan kapan saja sebelum melakukan perubahan besar, dan dapat memulihkan kembali seluruh pengaturan atau hanya grup tertentu (misal hanya modul Keuangan atau Aplikasi) dengan aman.
- **Ekspor & Impor File Cadangan Pengaturan**: Pengaturan sistem dapat diunduh dalam bentuk file JSON terenkripsi dan diimpor kembali ke lingkungan yang sama atau server cadangan.
- **Pemberian Umpan Balik pada Jawaban AI Agent**: Admin dapat memberikan penilaian jempol ke atas (👍) atau ke bawah (👎) langsung pada gelembung jawaban asisten AI di sidebar kanan antarmuka. Jawaban yang disukai otomatis tersimpan ke cache jawaban terverifikasi untuk mempercepat respons di kemudian hari.
- **Peninjauan Kualitas Jawaban AI**: Administrator dapat membuka panel tinjauan umpan balik AI di Pengaturan untuk menganalisis pertanyaan apa saja yang berhasil dijawab dengan baik atau memerlukan perbaikan dokumentasi.
- **Pencarian Panduan Fitur Resmi Melalui AI**: AI Agent kini dapat membaca dokumentasi panduan resmi fitur Dekasimal secara langsung dan menjawab pertanyaan admin dengan menyertakan tautan navigasi menu yang valid dan terverifikasi.
- **Pemantauan Sesi RADIUS Tanpa Accounting**: Dashboard RADIUS kini tetap menampilkan jumlah sesi PPPoE dan Hotspot aktif secara akurat meskipun router MikroTik/NAS dikonfigurasi tanpa RADIUS accounting.

### Bug Fix / Solusi Masalah

- **Pencegahan Terhapusnya Pengaturan Diam-diam**: Form pengaturan di frontend kini memiliki proteksi `LoadFailedNotice` yang mengunci tombol Simpan saat data belum termuat, dan backend menulis pembaruan konfigurasi per-key hanya untuk key yang diizinkan oleh registri skema.
- **Pencegahan Korupsi Serialisasi FormData**: Penggunaan metadata `__json_fields` mencegah penguraian ganda (*double-parsing*) pada payload JSON form pengaturan.
- **Penanganan Loop Reconnect & Banjir Log MikroTik**: Klien PPPoE yang menggunakan kredensial tidak terdaftar tidak lagi dihujani `Access-Reject` terus-menerus; server RADIUS mengisolasi koneksi ke profil blokir kurungan setelah batas percobaan gagal tercapai.
- **Pencegahan Perekaman Login Palsu**: Autentikasi yang dialihkan ke profil blokir (*blocked*) tidak lagi memperbarui data waktu login terakhir atau mencatat IP penyerang pada akun pelanggan.
- **Eliminasi Request Menggantung pada AI Agent**: Implementasi *stream stall watchdog* dan manajemen job asinkron latar belakang (`202 Accepted` + polling) mencegah timeout proxy pada query analitik log berukuran besar.
- **Resolusi Berkas AI di Lingkungan Docker**: Perbaikan path proyek memastikan fitur inspeksi kode dan katalog navigasi AI Agent berfungsi sempurna di dalam kontainer Docker backend.

### Menu/Fitur Baru

- **Panel Backup & Restore Pengaturan**: Tersedia pada menu **Pengaturan → Pengembang → Tab Lainnya → Kartu Backup & Restore Pengaturan**.
- **Panel Daftar Umpan Balik AI Agent**: Tersedia pada menu **Pengaturan → AI Agent → Bagian Umpan Balik Jawaban AI**.
- **Tombol Penilaian Respon AI**: Tombol jempol 👍 / 👎 pada setiap jawaban asisten AI di sidebar obrolan kanan.
- **Perintah CLI Evaluasi Kualitas AI**: Skrip CLI `npm run ai:eval` untuk menguji performa model AI terhadap kumpulan pertanyaan standar.
- **Perintah CLI Generator Basis Pengetahuan & Draf Panduan**: Skrip `npm run gknow` untuk meregenerasi katalog fitur dan `npm run ai:draft-doc` untuk membuat draf panduan fitur baru.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Snapshot, Backup & Restore Pengaturan Sistem

- **Penjelasan Fitur**: Fitur ini memungkinkan Superadmin/Developer membuat titik pemulihan (*snapshot point*) pengaturan aplikasi sebelum mengubah konfigurasi sensitif (seperti integrasi Payment Gateway, RADIUS, WhatsApp, atau Pengaturan Umum). Pemulihan dapat dilakukan secara granular per kelompok modul tanpa mengganggu konfigurasi modul lainnya.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi dan buka menu **Pengaturan** → **Pengembang** → pilih tab **Lainnya**.
  2. Cari kartu **Backup & Restore Pengaturan**.
  3. Untuk membuat cadangan baru, klik tombol **Buat Snapshot**:
     - Masukkan nama snapshot (contoh: `Sebelum Migrasi Gateway`).
     - Berikan catatan singkat opsional mengenai alasan pencadangan.
     - Pilih grup konfigurasi yang ingin dicadangkan (atau centang *Pilih Semua*).
     - Klik **Simpan Snapshot**.
  4. Snapshot yang baru dibuat akan muncul pada tabel daftar snapshot.
  5. Untuk memulihkan konfigurasi:
     - Klik tombol **Restore** pada snapshot yang diinginkan.
     - Centang grup pengaturan spesifik yang ingin dikembalikan ke kondisi snapshot.
     - Klik **Terapkan Pemulihan**. Sistem akan memulihkan data dan otomatis memicu *side effects* (reload servis terkait) secara terpusat.
  6. Untuk mencadangkan ke komputer lokal, klik tombol **Ekspor** pada snapshot untuk mengunduh berkas `.json`. Berkas ini dapat dipulihkan kapan saja menggunakan tombol **Impor Snapshot**.

### 2. Pemanfaatan Umpan Balik AI Agent & Peningkatan Jawaban Terverifikasi

- **Penjelasan Fitur**: Asisten AI Dekasimal kini dilengkapi sistem kurasi otomatis berbasis komunitas admin. Saat admin memberi rating positif pada jawaban pembuka percakapan, jawaban tersebut akan disimpan ke dalam basis pengetahuan terverifikasi sistem sehingga pertanyaan serupa dapat dijawab lebih cepat dan konsisten di masa mendatang.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka sidebar obrolan AI Dekasimal di pojok kanan antarmuka web.
  2. Tanyakan panduan pengoperasian sistem atau kendala teknis (contoh: *"Bagaimana cara mengubah profil paket pelanggan broadband?"*).
  3. Setelah asisten AI memberikan respons beserta tautan menu terkait, perhatikan tombol jempol di bawah gelembung pesan:
     - Klik ikon **Jempol ke Atas (👍)** jika jawaban akurat dan sangat membantu. Sistem akan menyimpan intisari jawaban ke basis pengetahuan terverifikasi.
     - Klik ikon **Jempol ke Bawah (👎)** jika jawaban kurang tepat atau tautan yang disarankan tidak sesuai. Jika sebelumnya jawaban tersebut tersimpan di cache, sistem akan otomatis mencabutnya.
  4. Untuk meninjau daftar pertanyaan dan umpan balik yang pernah diberikan oleh seluruh staf:
     - Buka menu **Pengaturan** → **AI Agent**.
     - Gulir ke bagian **Tinjauan Umpan Balik AI** untuk melihat daftar percakapan, rating yang diberikan, dan status verifikasi cache.

### 3. Proteksi Klien PPPoE Tidak Terdaftar di RADIUS Server

- **Penjelasan Fitur**: Mekanisme isolasi cerdas pada server RADIUS untuk mengatasi gangguan akibat router pelanggan lama (yang layanannya telah berhenti atau akunnya telah dihapus) yang terus-menerus mencoba menyambung ke jaringan sehingga memenuhi log MikroTik dan menghabiskan sumber daya server.
- **Langkah Penggunaan (Tutorial & Verifikasi)**:
  1. Pastikan profil kurungan telah dikonfigurasi pada menu **Pengaturan** → **RADIUS** (field `mikrotik_block_profile`, misalnya `pppoe-blocked-profile`).
  2. Atur ambang batas percobaan pada field `limit_try_counter` (misalnya 5 kali).
  3. Ketika sebuah router CPE pelanggan mencoba melakukan dial PPPoE dengan username yang tidak ada di sistem:
     - Percobaan ke-1 hingga ke-5 akan ditolak dengan `Access-Reject`.
     - Pada percobaan ke-6, RADIUS server secara otomatis membalas dengan `Access-Accept` membawa `mikrotik_block_profile`.
     - Router pelanggan berhenti melakukan loop dial berulang karena status PPPoE telah terhubung di sisi router klien, namun sesi klien masuk ke firewall isolir tanpa akses data.
  4. Buka menu **Dashboard** → **RADIUS** → periksa panel **Live Authentication Log**. Sesi tersebut akan tercatat dengan badge berwarna abu-abu bertuliskan status `BLOCKED` dan alasan `Username not found (loop suppressed)`.
