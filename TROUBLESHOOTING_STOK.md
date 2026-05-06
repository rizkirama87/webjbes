# 🐛 Troubleshooting: Stok Tidak Berkurang

## ❌ Masalah: "Kenapa stok ga ngurang?"

Ini bisa disebabkan oleh beberapa hal. Mari kita diagnosa dengan step-by-step.

---

## 🔍 Cara Mendiagnosa

### **Step 1: Buka Debug Panel**

1. Buka website penjualan
2. Klik tab **"Laporan"**
3. Klik tombol **🐛 Debug Panel** (di bawah tombol Export)
4. Debug Panel akan muncul

### **Step 2: Cek localStorage**

1. Di Debug Panel, klik tombol **"Cek localStorage"**
2. Lihat output yang tampil
3. Catat apakah ada data di `ice_cream_stock` dan `sales_data`

**Kemungkinan output:**

#### ✅ **Baik (stok tersimpan):**
```json
ice_cream_stock: {
  "1": 30,
  "2": 40,
  ...
}
```
→ Stok sudah berkurang dari 40 menjadi 30

#### ❌ **Masalah (stok tidak tersimpan):**
```
ice_cream_stock: KOSONG!
```
→ Stok tidak pernah disimpan

### **Step 3: Cek Products Array**

1. Di Debug Panel, klik **"Cek Products Array"**
2. Lihat output `stock` value
3. Bandingkan dengan `ice_cream_stock`

**Kemungkinan output:**

#### ✅ **Baik:**
```
Sweetcorn: stock = 30
Choco Malt: stock = 40
```

#### ❌ **Masalah:**
```
Sweetcorn: stock = 40 (seharusnya 30)
```

### **Step 4: Buka Console Browser (F12)**

1. Tekan **F12** atau **Ctrl+Shift+I**
2. Buka tab **"Console"**
3. Cari pesan debug dengan warna berbeda:
   - 🟢 `✅` = Success (hijau)
   - 🔴 `❌` = Error (merah)
   - 🟠 `⚠️` = Warning (orange)

---

## 🎯 Solusi Berdasarkan Masalah

### **Masalah #1: localStorage Kosong (ice_cream_stock: KOSONG)**

**Penyebab:**
- Checkout tidak menyimpan ke localStorage
- Browser cache ter-clear
- Private browsing mode

**Solusi:**

#### Cepat:
```javascript
// Buka Console (F12) dan jalankan:
localStorage.setItem('ice_cream_stock', JSON.stringify({1: 30, 2: 40, 3: 30, 4: 20, 5: 20}));
location.reload();
```

#### Permanen:
1. Buka website
2. Beli 1 produk minimal
3. Klik "Lanjut ke Pembayaran"
4. Cek Console, catat error yang muncul
5. Buka issue dengan screenshot error

---

### **Masalah #2: Products Array Stok Tidak Sesuai dengan localStorage**

**Penyebab:**
- database.json tidak ter-load (fetch error)
- Fallback data di-gunakan
- Halaman refresh tapi localStorage tidak di-load

**Solusi:**

#### Opsi A: Verifikasi database.json
```
1. Buka file: c:\PENJUALAN\database.json
2. Pastikan file ada dan tidak rusak
3. Buka di browser langsung: file:///c:/PENJUALAN/database.json
4. Jika 404, database.json tidak ditemukan
```

#### Opsi B: Reload dan perhatikan Console
```
1. Buka website
2. Buka Console (F12)
3. Reload halaman (Ctrl+R)
4. Lihat pesan loading database
5. Catat pesan "✅ Stok berhasil dimuat" atau "❌ Error loading"
```

#### Opsi C: Ganti ke fallback data
```
Jika database.json bermasalah, edit fallback di file index.html
Tapi lebih baik fix database.json terlebih dahulu
```

---

### **Masalah #3: Stok Berkurang Tapi Tidak Persisten (Kembali ke 40 saat Reload)**

**Penyebab:**
- localStorage tidak ter-load saat page load
- loadStockFromJSON() bermasalah

**Solusi:**

1. Buka Console (F12)
2. Reload halaman
3. Lihat pesan di console:
   - Jika `✅ Stok diperbarui dari localStorage` → Baik
   - Jika `⚠️ localStorage kosong` → Masalah

**Jika masalah:**
```javascript
// Di Console, ketik:
localStorage.getItem('ice_cream_stock');
// Jika output null atau undefined, localStorage kosong

// Cek ulang dengan:
location.reload();
// Lihat Console lagi
```

---

### **Masalah #4: Export Data Menampilkan Stok Lama**

**Penyebab:**
- Export mengambil data dari products array yang belum ter-sync dengan localStorage

**Solusi:**
- Sudah diperbaiki di versi terbaru
- Export sekarang menampilkan `stockDariLocalStorage` yang akurat
- Gunakan field itu untuk melihat stok yang benar

---

## 📋 Checklist Debugging

Gunakan checklist ini untuk verifikasi:

- [ ] **localStorage ada?** Cek dengan Debug Panel → "Cek localStorage"
- [ ] **Stok berkurang?** Lihat Console saat checkout
- [ ] **Products Array match?** Cek dengan Debug Panel → "Cek Products Array"
- [ ] **database.json valid?** Buka di browser atau JSON validator
- [ ] **Reload persisten?** Reload halaman, cek stok tetap sama
- [ ] **Sales terrekam?** Lihat Laporan → sales table
- [ ] **Export akurat?** Export → buka file JSON → lihat `stockDariLocalStorage`

---

## 🔧 Command Cepat di Console (F12)

Copy-paste di Console untuk testing:

### Cek semua localStorage
```javascript
console.log('ice_cream_stock:', localStorage.getItem('ice_cream_stock'));
console.log('sales_data:', localStorage.getItem('sales_data'));
```

### Clear localStorage
```javascript
localStorage.clear();
location.reload();
```

### Set stok manual
```javascript
localStorage.setItem('ice_cream_stock', JSON.stringify({1: 30, 2: 40, 3: 30, 4: 20, 5: 20}));
location.reload();
```

### Lihat products array
```javascript
console.table(products);
```

### Lihat sales data
```javascript
console.log(JSON.parse(localStorage.getItem('sales_data')));
```

---

## 📸 Saat Melaporkan Bug

Jika ingin bantuan lebih lanjut, sediakan:

1. **Screenshot Console (F12)** saat checkout
2. **Screenshot Debug Panel** output
3. **Describe:** Apa yang Anda kerjakan (beli produk apa, berapa unit)
4. **Describe:** Apa yang terjadi (stok berkurang? persisten? export akurat?)
5. **Browser:** Chrome, Firefox, Safari, Edge?
6. **OS:** Windows, Mac, Linux?

---

## 🆘 Jika Masih Bermasalah

Buka issue dengan info:
- Console output (screenshot)
- Debug Panel output
- Steps to reproduce
- Expected vs Actual result

---

## ✅ Konfirmasi Semua Berfungsi

Setelah perbaikan, test dengan scenario:

```
1. Halaman baru/fresh
2. Beli 5 unit Sweetcorn
3. Klik "Lanjut ke Pembayaran"
4. Cek: Stok Sweetcorn = 35 ✓
5. Reload halaman (Ctrl+R)
6. Cek: Stok Sweetcorn tetap 35 ✓ (bukan kembali 40)
7. Buka Laporan: Sweetcorn 5 unit terjual ✓
8. Export Data: stockDariLocalStorage = 35 ✓
```

**Jika semua ✓ = BERHASIL! 🎉**

---

## 📞 Kontak Support

Jika masalah persisten dan sudah coba semua langkah:
- Buka Debug Panel
- Export data
- Buka Console output
- Hubungi developer dengan detail lengkap
