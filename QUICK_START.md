# 🚀 Quick Start - Laporan Penjualan & Data

## ⚡ Jawaban Cepat Pertanyaan Anda

### ❌ "Kenapa data kereset saat di reload?"

**SALAH!** Data TIDAK seharusnya kereset. Saya sudah fix:

✅ Penjualan tetap tersimpan di `localStorage`
✅ Stok berkurang tetap terrekam
✅ Saat reload, semua data kembali

**Cara memverifikasi:**
1. Beli produk → klik "Lanjut ke Pembayaran"
2. Buka tab "Laporan" → lihat data penjualan
3. Reload halaman (Ctrl+R)
4. Data penjualan tetap ada ✓

---

### 💾 "Kenapa database.json tidak langsung berubah?"

**NORMAL!** Ini bukan bug, tapi **limitasi keamanan browser**:

❌ Browser **TIDAK BISA** menulis file (demi keamanan)
✅ Browser **BISA** membaca file JSON
✅ Browser **BISA** simpan data ke localStorage

**Solusi:**

#### **Opsi 1: Export Data (Recommended)**
```
Laporan → 💾 Export Data → Download JSON → Simpan backup
```
File akan berisi semua data yang bisa di-import kembali

#### **Opsi 2: Edit Manual database.json**
```
1. Buka file: c:\PENJUALAN\database.json
2. Edit di text editor (VSCode, Notepad++)
3. Ubah "stock" atau "modal"
4. Simpan (Ctrl+S)
5. Reload website (Ctrl+R)
```

---

## 📊 Cara Kerja Data Sekarang

```
BROWSER
├─ localStorage (Tempat Utama)
│  ├─ ice_cream_stock → Stok berkurang
│  └─ sales_data → Penjualan tercatat
│
└─ database.json (File Referensi)
   ├─ Stok awal
   └─ Modal/cost
```

**Alurnya:**
1. Halaman dibuka → Baca database.json (stok awal)
2. Customer beli → Kurangi stok & catat penjualan di localStorage
3. Reload → Baca localStorage (stok tetap berkurang!)

---

## 🎯 Yang Bisa Dilakukan Sekarang

| Aksi | Cara | Hasil |
|------|------|-------|
| Lihat laporan | Klik tab "Laporan" | ✅ Semua penjualan tampil |
| Export data | 💾 Export Data button | ✅ Download JSON |
| Refresh laporan | 🔄 Refresh Laporan button | ✅ Data ter-update |
| Reset penjualan | 🗑️ Reset Data button | ✅ Penjualan jadi 0 |
| Backup database | Export + Simpan file | ✅ Aman |
| Edit stok awal | Edit database.json | ✅ Stok reset ke nilai baru saat reload |
| Ubah harga modal | Edit database.json + reload | ✅ Keuntungan otomatis dihitung ulang |

---

## 🔄 Contoh Kasus: Beli & Reload

### **Skenario:**
1. Awal: database.json stok Sweetcorn = 40
2. Customer beli 5 unit Sweetcorn
3. Reload halaman
4. **Hasil: Stok = 35** ✅ (bukan 40!)

### **Kenapa?**
- Stok berkurang disimpan di localStorage
- Saat reload, sistem prioritas:
  - Database.json (40) → Baca
  - localStorage (35) → Override ✅
  - Hasil akhir: 35

---

## 💡 Tips Penting

### ✅ DO's
- Export data secara rutin
- Backup database.json sebelum edit
- Reload halaman setelah edit JSON
- Cek Console (F12) jika ada masalah

### ❌ DON'Ts
- Jangan hapus database.json
- Jangan asal edit JSON (perhatikan comma, bracket)
- Jangan clear browser cache/cookie tanpa backup

---

## 📁 File-File Penting

```
c:\PENJUALAN\
├─ index.html              → Website utama
├─ database.json           → Data stok & modal (edit manual di sini)
├─ README.md              → Dokumentasi awal
├─ PENJELASAN_DATA.md     → Penjelasan lengkap (baca ini!)
└─ laporan_penjualan_*.json → Hasil export (auto-generated)
```

---

## 🔧 Jika Ada Bug

### **Data penjualan tidak muncul:**
```
1. Buka Console (F12 → Console)
2. Ketik: localStorage.getItem('sales_data')
3. Lihat hasilnya
```

### **Stok tidak berkurang:**
```
1. Cek localStorage:
   F12 → Application → Local Storage → lihat ice_cream_stock
2. Cek database.json syntax (gunakan JSON validator)
```

### **Export tidak berfungsi:**
```
1. Cek Console untuk error
2. Pastikan folder writable
3. Coba browser lain (Chrome, Firefox, Edge)
```

---

## 🚀 Next Steps (Jika Mau lebih Canggih)

Beri tahu jika ingin:
1. **Backend Server** → Data otomatis tersimpan ke file
2. **Database Sejati** → MySQL, MongoDB, PostgreSQL
3. **Cloud Storage** → Firebase, AWS
4. **Mobile App** → Bisa akses dari smartphone
5. **Auto Backup** → Backup otomatis harian

---

## 📌 Ringkas

| Pertanyaan | Jawaban Singkat |
|-----------|-----------------|
| Data kereset? | ❌ Tidak (sudah fixed). Lihat tab Laporan |
| File JSON berubah? | ❌ Normal, browser ga bisa tulis file. Gunakan Export |
| Stok berkurang persisten? | ✅ Ya, simpan di localStorage |
| Bisa offline? | ✅ Ya, semua simpan di browser |
| Butuh server? | ❌ Untuk sekarang tidak, tapi recommended untuk future |

---

**Udah clear? Coba sekarang! Beli produk → cek Laporan → Reload → Data persisten ✓**
