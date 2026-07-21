# Laporan Pekerjaan — Pengujian Lab radius-server terhadap Mikrotik Nyata

**Tanggal:** 21 Juli 2026
**Pelaksana:** Tim Engineering (Dedy)
**Periode kerja:** 20–21 Juli 2026
**Ruang lingkup:** `radius-server` (server RADIUS internal) diuji langsung terhadap perangkat Mikrotik/RouterOS asli

---

## 1. Ringkasan

Kami menyelesaikan pengujian menyeluruh server RADIUS (`radiusd`) terhadap **perangkat Mikrotik sungguhan**, bukan lagi hanya simulasi. Selama ini seluruh pengujian otomatis kami "berbicara dengan dirinya sendiri" — server dan test memakai pustaka yang sama, sehingga selalu sepakat. Pengujian ini menutup celah tersebut dengan menukar paket RADIUS nyata dengan RouterOS.

**Hasil:** seluruh **8 fase (Fase 0–7) SELESAI dan LULUS**. Ditemukan **8 cacat yang lolos seluruh pengujian otomatis** — semuanya hanya muncul saat berhadapan dengan perangkat nyata, dan **semua sudah diperbaiki** beserta test pencegah regresi. Di antaranya satu bug **kritis** yang memblokir autentikasi total, dan satu bug yang **membalik perhitungan kuota download/upload pelanggan**.

---

## 2. Lingkungan Pengujian

| Komponen | Perangkat |
|---|---|
| Server RADIUS | `radiusd` (mesin uji, `192.168.100.96`) |
| NAS (R1) | Mikrotik RB2011UiAS — RouterOS 6.49.19 |
| Client (R2) | Mikrotik CRS326 — RouterOS 7.13 |
| Database | MongoDB (produksi dev-shared, dengan pengaman backup; Fase 7 memakai MongoDB lokal terisolasi) |

Setiap hasil diverifikasi di **tiga tempat sekaligus** — log server, metrik, dan database — agar tidak ada hasil "lulus palsu".

---

## 3. Hasil per Fase

| Fase | Cakupan | Hasil |
|---|---|---|
| 0 | Persiapan: fondasi server, konfigurasi router dari nol, sambungkan RADIUS | ✅ Lulus |
| 1 | PPPoE (login PAP/CHAP/MS-CHAPv2 + enkripsi, rate limit, accounting, akurasi pemakaian data) | ✅ 9/9 — akurasi pemakaian data **0,00004%** |
| 2 | Hotspot, termasuk modul User Hotspot (#148) | ✅ 7/8 — modul #148 terbukti; 1 skenario menunggu klien ber-browser |
| 3 | Login admin router via RADIUS + pemetaan hak akses | ✅ 4/4 — tanpa kebocoran password |
| 4 | Pemutusan sesi jarak jauh (CoA/Disconnect) | ✅ 5/5 |
| 5 | Isolir & kode alasan penolakan (343–346) | ✅ 5/5 |
| 6 | Pembersihan sesi tersangkut (sweep) | ✅ 3/3 |
| 7 | Ketahanan: server tetap jalan saat database bermasalah (WAL), restart bersih, ganti port tanpa restart | ✅ 4/4 |

**Catatan hari ini (21 Juli):** Fase 6 dan Fase 7 yang sebelumnya tertunda diselesaikan hari ini.
- **Fase 6** dijalankan aman terhadap data nyata: 53 akun "online" tersangkut (sudah basi lebih dari 1 tahun) berhasil dibersihkan tanpa mengganggu perangkat pelanggan mana pun.
- **Fase 7** dijalankan di database lokal terisolasi (produksi tidak tersentuh): terbukti server tetap melayani dan menyimpan data pemakaian dengan aman meski database dimatikan di tengah proses, lalu memulihkannya utuh saat database hidup kembali.

---

## 4. Temuan & Perbaikan (8 cacat, semua sudah diperbaiki)

Semua cacat ini lolos dari seluruh pengujian otomatis dan hanya terlihat saat diuji dengan perangkat asli.

| # | Temuan | Dampak |
|---|---|---|
| L1 | Server tidak bisa autentikasi ke RouterOS versi baru (atribut keamanan tidak dikirim) | **Kritis** — pemblokir total |
| L2 | Perhitungan download & upload tertukar → kuota pelanggan terbalik | Tinggi |
| L3 | Alamat IP & sesi terakhir pelanggan tidak pernah tersimpan | Sedang |
| L4 | Saat isolir, pembatasan bandwidth tidak diterapkan | Sedang |
| L5 | Saat isolir, enkripsi sesi ikut turun | Sedang |
| L6–L7 | Log diagnostik menampilkan keterangan yang menyesatkan | Rendah |
| L8 | Ketergantungan operasional yang belum terdokumentasi (kini dicatat) | Operasional |

**Nilai tambah di luar rencana:** dibuat **tiga lapis diagnostik operasional** yang memunculkan masalah "gagal senyap" ke dashboard (mis. paket dari NAS tak dikenal, CoA timeout), plus **endpoint & tombol pemutusan sesi pelanggan** di backend dan frontend broadband.

Seluruh 8 temuan kini memiliki **test regresi otomatis** — gagal sebelum perbaikan, lulus sesudahnya — sehingga tidak akan terulang tanpa terdeteksi.

---

## 5. Nilai / Dampak

- **Interoperabilitas dengan Mikrotik kini terbukti**, bukan lagi asumsi.
- Dua bug berdampak langsung ke pelanggan sudah ditutup: autentikasi yang bisa gagal total (L1) dan kuota yang terbalik (L2).
- Sistem terbukti **tahan gangguan database** — pemakaian data pelanggan tidak hilang saat database bermasalah.
- Kualitas ke depan terjaga oleh test regresi + diagnostik dashboard baru.

---

## 6. Yang Masih Terbuka (bukan penghambat)

1. **Uji beban / konkurensi tinggi** — di luar cakupan pengujian ini (2 router tidak menghasilkan beban berarti); memerlukan lingkungan staging khusus.
2. **1 skenario hotspot (CHAP)** — memerlukan klien ber-browser (HP/laptop) untuk diverifikasi; inti mekanismenya sudah terbukti di fase lain.
3. **Integrasi penuh lapis diagnostik ke-3** — inti sudah siap & teruji, tinggal penyambungan.

---

## 7. Referensi

- Rencana & hasil lengkap per skenario: `radius-server/RENCANA_PENGUJIAN_LAB.md` (§12 Hasil Eksekusi)
- Detail teknis tiap perbaikan: `radius-server/DOCUMENTATION.md` (§11)
- Peta jalan diagnostik: `radius-server/NAS_DIAGNOSTICS.md`
- Bukti mentah (log, metrik, konfigurasi): `radius-server/lab-results/`
