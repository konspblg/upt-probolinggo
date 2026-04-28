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
