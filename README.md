# Sistem Database Stok Es Krim

## Penjelasan Sistem

Aplikasi penjualan es krim ini menggunakan **Browser Storage (localStorage)** sebagai database untuk menyimpan stok produk. Ini memastikan bahwa stok tidak berkurang saat halaman di-reload.

## Bagaimana Cara Kerjanya?

### 1. **Load Data Saat Halaman Dibuka**
   - Saat halaman pertama kali diakses, sistem akan memeriksa apakah ada data stok yang tersimpan di localStorage
   - Jika ada, data stok akan dimuat dari localStorage
   - Jika belum ada, sistem akan membuat data default dan menyimpannya

### 2. **Simpan Data Saat Checkout**
   - Setiap kali customer melakukan checkout dan membayar
   - Stok produk akan berkurang sesuai jumlah yang dibeli
   - Data stok yang telah diperbarui akan **disimpan ke localStorage** secara otomatis

### 3. **Stok Persisten**
   - Data stok akan tetap tersimpan meskipun:
     - Halaman di-reload
     - Browser ditutup dan dibuka kembali
     - Beberapa hari kemudian
   - Selama browser tidak menghapus localStorage

## File-File yang Ada

### `index.html`
File utama aplikasi yang berisi:
- Struktur HTML
- Styling CSS
- JavaScript dengan fungsi:
  - `loadStockFromDatabase()` - Memuat stok dari localStorage
  - `saveStockToDatabase()` - Menyimpan stok ke localStorage
  - `checkout()` - Mengurangi stok saat checkout

### `database.json`
File referensi yang menunjukkan struktur dan nilai default stok produk

## Cara Menguji

1. Buka `index.html` di browser
2. Tambahkan beberapa produk ke keranjang
3. Klik "Lanjut ke Pembayaran"
4. Stok akan berkurang
5. **Reload halaman** (Ctrl+R atau F5)
6. Stok akan tetap berkurang (tidak kembali ke nilai default)

## Cara Mereset Stok ke Default

Jika ingin mereset stok ke nilai awal, buka Console browser dan jalankan:

```javascript
localStorage.removeItem('ice_cream_stock');
location.reload();
```

Atau buka Browser DevTools (F12) → Application → Local Storage → hapus entry `ice_cream_stock`

## Keunggulan Sistem Ini

✅ **Persisten** - Data tersimpan di device, tidak hilang saat reload
✅ **Tidak Perlu Server** - Bekerja dengan HTML statis
✅ **Aman** - Data hanya tersimpan di device pengguna
✅ **Cepat** - Tidak perlu koneksi internet
✅ **Mudah** - Tidak perlu setup database kompleks

## Untuk Upgrade ke Server Database

Jika di masa depan ingin menggunakan database server (seperti MySQL, MongoDB, dll), Anda dapat:
1. Membuat backend API (Node.js, PHP, Python, dll)
2. Mengganti `localStorage` dengan fetch API ke server
3. Menyimpan data di database server yang persistent
