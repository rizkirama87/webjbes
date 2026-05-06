# 📊 Penjelasan Sistem Data Penjualan Es Krim

## ⚠️ Penting Dipahami

Website ini adalah aplikasi **HTML/JavaScript murni** yang berjalan di **browser saja** (tanpa server). Karena itu ada beberapa hal penting tentang cara menyimpan data:

---

## 1️⃣ Bagaimana Data Disimpan?

### **Dua Tempat Penyimpanan:**

| Tempat | Data | Persisten? | Perubahan File? |
|--------|------|-----------|-----------------|
| **localStorage** | Stok berkurang, penjualan | ✅ Tetap saat reload | ❌ Tidak |
| **database.json** | Stok awal, modal, produk | ✅ Tetap | ❌ Tidak otomatis |

### **localStorage (Browser Storage)**
- Menyimpan data di browser, bukan di file
- Data tetap ada meskipun halaman di-reload
- Setiap browser memiliki localStorage terpisah
- Kapasitas: ~5-10MB tergantung browser

### **database.json**
- File statis yang tidak bisa diubah browser
- Hanya bisa dibaca oleh aplikasi
- Perlu diubah manual di text editor
- Atau via Backend/Server API

---

## 2️⃣ Mengapa Data Tidak Langsung Berubah di database.json?

### **Alasan Keamanan**
Browser tidak boleh menulis langsung ke file karena alasan keamanan:
- Melindungi komputer dari akses tidak sah
- Mencegah malware menulis file berbahaya
- Standar keamanan browser modern

### **Solusi:**

#### ✅ **Opsi 1: Export Data (Saat Ini)**
```
Klik 💾 Export Data → Download laporan_penjualan_YYYY-MM-DD.json
File akan berisi semua data penjualan, stok, dan keuntungan
```

#### ✅ **Opsi 2: Update database.json Manual**
```
Buka database.json di text editor (VSCode, Notepad++, dll)
Ubah nilai "stock" atau "modal"
Simpan file
Refresh halaman web
```

#### ✅ **Opsi 3: Backend Server (Untuk Masa Depan)**
- Setup Node.js / PHP / Python
- Buat API untuk update database
- Browser bisa kirim data ke server → server update file
- Lebih kompleks tapi otomatis

---

## 3️⃣ Alur Data Saat Reload

```
Halaman Dibuka
    ↓
Baca stok dari database.json (nilai awal)
    ↓
Cek localStorage → ada data penjualan sebelumnya?
    ↓
    ├─ ADA → Muat penjualan dari localStorage
    └─ TIDAK → Buat baru dengan 0 penjualan
    ↓
Tampilkan produk dengan stok dari database.json
Tampilkan laporan dengan penjualan dari localStorage
```

---

## 4️⃣ Contoh Alur Penjualan

### **Skenario: Beli 5 unit Sweetcorn**

```
1. Customer: Klik "Belanja Sekarang"
2. Customer: Tambah 5 Sweetcorn ke keranjang
3. Customer: Klik "Lanjut ke Pembayaran"
   ↓
4. JavaScript:
   - Kurangi stok Sweetcorn: 40 → 35
   - Simpan stok ke localStorage
   - Catat penjualan: Sweetcorn +5 unit
   - Simpan penjualan ke localStorage
   ↓
5. UI Update:
   - Tampilkan pesanan berhasil
   - Produk card refresh (stok 35)
   - Laporan keuntungan update: Sweetcorn 5 unit terjual
   ↓
6. User: Reload halaman (Ctrl+R)
   ↓
7. JavaScript:
   - Baca database.json → Stok default 40
   - Baca localStorage → Stok terakhir 35 ✅
   - Baca localStorage → Penjualan 5 unit ✅
   ↓
8. UI Tampil:
   - Stok tetap 35 (tidak kembali ke 40)
   - Penjualan tetap 5 unit
   - Keuntungan tetap terhitung ✅
```

---

## 5️⃣ Cara Backup Data

### **Metode 1: Export Otomatis**
```
1. Buka tab "Laporan"
2. Klik tombol 💾 Export Data
3. File JSON akan di-download
4. Simpan file tersebut di tempat aman
```

File export berisi:
- Tanggal export
- Data stok saat ini
- Semua penjualan yang tercatat
- Total keuntungan

### **Metode 2: Backup database.json**
```
1. Buka file: c:\PENJUALAN\database.json
2. Duplikat file → database_backup_YYYY-MM-DD.json
3. Simpan di tempat aman
```

### **Metode 3: Backup localStorage**
```
Di Console Browser (F12 → Console), ketik:
```
```javascript
let data = localStorage.getItem('sales_data');
console.log(data);
// Selanjutnya copy output dan simpan ke file txt
```

---

## 6️⃣ Cara Mengedit database.json

### **Update Stok Awal**
```json
{
  "products": [
    {
      "id": 1,
      "name": "Sweetcorn",
      "stock": 50  // Ubah dari 40 ke 50
    }
  ]
}
```
Setelah reload, stok akan 50 (atau 50 - yang sudah terjual)

### **Update Modal/Cost**
```json
"modal": 2000  // Ubah harga modal
```
Keuntungan akan otomatis dihitung ulang

---

## 7️⃣ Reset/Clear Data

### **Reset Data Penjualan**
```
1. Buka tab "Laporan"
2. Klik 🗑️ Reset Data
3. Konfirmasi
4. Penjualan kembali 0, keuntungan kembali Rp 0
```

### **Reset Stok ke Default**
```
Di Console Browser (F12):
```
```javascript
localStorage.removeItem('ice_cream_stock');
location.reload();
```

### **Clear Semua Data**
```
Di Console Browser (F12):
```
```javascript
localStorage.clear();
location.reload();
```

---

## 8️⃣ Troubleshooting

### **Q: Data hilang setelah reload?**
**A:** 
- Pastikan tidak menghapus localStorage
- Cek browser privacy settings
- Coba browser lain
- Check Console (F12) untuk error

### **Q: database.json tidak berubah?**
**A:**
- Ini normal! Browser tidak bisa menulis file
- Gunakan "Export Data" untuk backup
- Atau edit manual di text editor

### **Q: Mau data otomatis tersimpan di file?**
**A:**
- Perlu setup Backend Server
- Rekomendasi: Node.js + Express + File System
- Atau: PHP dengan file system
- Atau: Cloud Database (Firebase, MongoDB)

### **Q: Stok berkurang tapi tidak di database.json?**
**A:**
- Stok berkurang di localStorage (browser)
- database.json tetap file asli
- Edit database.json manual untuk perubahan permanent

---

## 9️⃣ Tips & Trik

### **Backup Rutin**
```
- Export data setiap hari
- Simpan di folder terpisah
- Jika perlu, bisa restore data lama
```

### **Monitor localStorage**
```
Di Browser DevTools (F12):
1. Application tab
2. Local Storage
3. Lihat file → bisa lihat semua data
```

### **Export ke Excel**
```
1. Export JSON dari website
2. Buka Excel
3. File → Open → Pilih JSON
4. Data otomatis jadi tabel
```

---

## 🔟 Kesimpulan

| Pertanyaan | Jawaban |
|-----------|---------|
| Data persisten saat reload? | ✅ Ya (di localStorage) |
| Database.json otomatis update? | ❌ Tidak (perlu backend) |
| Bisa export data? | ✅ Ya (tombol Export) |
| Data aman? | ✅ Ya (hanya di device) |
| Bisa pakai offline? | ✅ Ya |

---

## 📞 Bantuan Lanjutan

Jika ingin fitur lebih advanced:
1. **Cloud Database**: Firebase, MongoDB, Supabase
2. **Backend Server**: Node.js, Python, PHP
3. **Mobile App**: React Native, Flutter
4. **Desktop App**: Electron, PyQt

Hubungi developer untuk implementasi lebih lanjut!
