# Cara Update ROB

Untuk menambahkan bulan baru, cukup tambahkan **2 baris kode** berikut:

## 1. Lokasi source spreadsheet pada: 

   https://docs.google.com/spreadsheets/d/1HYlNbyLKIV-iihbtamC65Hp7FKtijPS-PGC3X9djXYA/

---

## 2. Update di bagian `URLS`
Tambahkan 1 baris setelah baris ke-5:
(cari pada sekitar baris 130)

```javascript
6: BASE + '?gid=XXXXXXXX&single=true&output=csv',
```

Ganti `XXXXXXXX` dengan **gid** dari sheet bulan Juni pada source spreadsheet.

---

## 3. Update di dropdown `monthSelect`
Tambahkan 1 baris setelah opsi bulan Mei (mengikuti penambahan ROB):
(cari pada sekitar baris 200)

```html
<option value="6">Juni 2026</option>
```

> ⚠️ Catatan:
> Dropdown bulan ini ada di **2 lokasi** (sticky area ROB), jadi pastikan kamu menambahkan baris ini di **keduanya**.

# 📋 Jadwal Vendor — UPT Probolinggo

Aplikasi monitoring jadwal pekerjaan pihak luar (vendor) berbasis web, diakses via browser tanpa perlu install apapun.

🔗 **URL Aplikasi:** https://konspblg.github.io/jadwal-vendor/

---

## 🗂️ Struktur Sistem

```
Google Sheets (sumber data)
    └── Sheet ROM_Final         ← data utama semua pekerjaan
    └── Sheet TEMP_STORE        ← penyimpanan status pekerjaan (IK, WP, dll)
    └── Sheet Hari Ini & Besok  ← tidak dipakai app (opsional, bisa dihapus)

Google Apps Script (GAS)
    └── Code.gs                 ← jembatan antara app dan Google Sheets

GitHub Pages
    └── index.html              ← tampilan aplikasi web
```

---

## 📱 Fitur Aplikasi

| Tab | Isi |
|-----|-----|
| **Hari Ini** | Pekerjaan yang berlangsung hari ini (filter otomatis) |
| **Besok** | Pekerjaan yang berlangsung besok (filter otomatis) |
| **ROM Final** | Semua pekerjaan, highlight yang berlangsung hari ini |
| **ROB** | Pekerjaan per bulan (Januari — Mei) |

**Fitur lainnya:**
- Badge ULTG (Probolinggo / Bangil / Jember) dan Penanggung Jawab (PIHAK LUAR / M. UPT / dll)
- Status pekerjaan (IK, Pembahasan, WP, Prapekerjaan) untuk PIHAK LUAR
- Tombol **Sync** — simpan status ke TEMP_STORE agar sinkron di semua device
- Tombol **Salin** — copy data Pihak Luar ke clipboard
- Tombol **?** — popup mitigasi sistem
- Filter ULTG dan tanggal di tab ROM Final dan ROB
- Pencarian (🔍) di semua tab

---

## 🔄 Alur Data

```
1. Data pekerjaan diisi di sheet ROM_Final (kolom A–N)
2. Petugas buka app → input status IK/WP/Pembahasan/Prapekerjaan
3. Tekan tombol SYNC → status tersimpan ke sheet TEMP_STORE via GAS
4. Sheet ROM_Final kolom O,P,Q,R otomatis baca dari TEMP_STORE (formula MATCH+INDEX)
5. Siapapun buka app dari device manapun → data status sudah sinkron
```

---

## 📊 Struktur Sheet ROM_Final

| Kolom | Isi |
|-------|-----|
| A | NO. |
| B | ASET |
| C | URAIAN PEKERJAAN |
| D | TEG (kV) |
| E | PERALATAN PADAM |
| F | SIFAT |
| G | PENANGGUNG JAWAB |
| H | TGL AWAL |
| I | TGL AKHIR |
| J | PUKUL |
| K | RENCANA OPERASI |
| L | MITIGASI SISTEM |
| M | NOTIF/WO ← kunci unik pencocokan data |
| N | ULTG |
| O | STATUS IK ← diisi otomatis dari TEMP_STORE |
| P | STATUS PEMBAHASAN ← diisi otomatis dari TEMP_STORE |
| Q | STATUS WP ← diisi otomatis dari TEMP_STORE |
| R | PRAPEKERJAAN ← diisi otomatis dari TEMP_STORE |

**Data mulai baris 5.** Baris 1–4 adalah header/judul.

---

## 🔧 Formula di Sheet ROM_Final

Formula berikut ada di kolom O, P, Q, R mulai baris 5 — **jangan dihapus:**

**O5 (STATUS IK):**
```
=IFERROR(INDEX(TEMP_STORE!O3:O500;MATCH(TRIM(M5);TRIM(TEMP_STORE!M3:M500);0));"")
```

**P5 (STATUS PEMBAHASAN):**
```
=IFERROR(INDEX(TEMP_STORE!P3:P500;MATCH(TRIM(M5);TRIM(TEMP_STORE!M3:M500);0));"")
```

**Q5 (STATUS WP):**
```
=IFERROR(INDEX(TEMP_STORE!Q3:Q500;MATCH(TRIM(M5);TRIM(TEMP_STORE!M3:M500);0));"")
```

**R5 (PRAPEKERJAAN):**
```
=IFERROR(INDEX(TEMP_STORE!R3:R500;MATCH(TRIM(M5);TRIM(TEMP_STORE!M3:M500);0));"")
```

Drag masing-masing formula dari baris 5 sampai baris 500.

---

## ⚙️ Google Apps Script (GAS)

GAS berfungsi sebagai jembatan saat tombol **Sync** ditekan di app.

### Cara setup ulang GAS (jika URL berubah atau perlu deploy ulang):

1. Buka Google Sheets → **Extensions → Apps Script**
2. Paste isi file `Code.gs` (ada di repo GitHub)
3. Klik **Deploy → New deployment**
4. Pilih tipe: **Web app**
5. *Execute as*: **Me**
6. *Who has access*: **Anyone**
7. Klik **Deploy** → izinkan akses → **copy URL**
8. Buka file `index.html` di GitHub, cari baris:
   ```javascript
   const GAS_URL='';
   ```
   Isi URL GAS di antara tanda kutip, lalu commit.

### Cara test GAS aktif:
Buka URL GAS di browser — harusnya muncul:
```json
{"status":"ok","message":"GAS aktif"}
```

---

## 🗃️ Spreadsheet

| Sheet | ID Spreadsheet |
|-------|---------------|
| Data UPT Probolinggo | `1HYlNbyLKIV-iihbtamC65Hp7FKtijPS-PGC3X9djXYA` |

---

## 🌐 GitHub Pages

Aplikasi di-host di GitHub Pages dari repo: `https://github.com/konspblg/jadwal-vendor`

### Cara update aplikasi:
1. Edit file `index.html` di repo
2. Commit & push ke branch `main`
3. GitHub Pages otomatis update dalam beberapa menit

---

## 🚨 Yang Perlu Diperhatikan

- **Jangan hapus sheet TEMP_STORE** — ini tempat penyimpanan status pekerjaan
- **Jangan hapus kolom O,P,Q,R di ROM_Final** — sudah ada formula otomatis
- **Jangan ubah struktur kolom ROM_Final** — app bergantung pada urutan kolom A–R
- **Jika GAS error**, cek apakah deployment masih aktif di Apps Script → Manage deployments
- **Status pekerjaan reset otomatis tiap bulan** di localStorage browser (tapi tidak berpengaruh karena sudah sync ke Sheets)

---

## 👤 Kontak

Dibuat oleh petugas UPT Probolinggo. Untuk pertanyaan teknis terkait formula Google Sheets atau kode aplikasi, silakan baca dokumentasi ini terlebih dahulu.
