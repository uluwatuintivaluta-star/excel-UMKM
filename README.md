# 📘 README – Sistem Manajemen Keuangan UMKM (Excel)

Dokumen ini menjelaskan **cara pembuatan, struktur, dan logika** sistem manajemen keuangan UMKM berbasis Excel yang sederhana, stabil, dan mudah dipakai user awam.

Sistem ini **TIDAK menggunakan ledger mutasi stok atau HPP kompleks**, melainkan pendekatan **praktis & konsisten** yang cocok untuk UMKM retail.

---

## 🎯 TUJUAN SISTEM

1. Mencatat **barang masuk (pembelian)**
2. Mencatat **penjualan**
3. Menghitung **stok akhir** secara otomatis
4. Menghitung **HPP sederhana (rata-rata beli)**
5. Menghasilkan **laporan penjualan & laba** yang masuk akal

> Fokus utama: **mudah dipakai, minim error user, dan angka bisa dipercaya**

---

## 🧠 FILOSOFI DESAIN (PENTING)

* Sistem ini **BUKAN sistem akuntansi audit-grade**
* Sistem ini **BUKAN ERP / warehouse ledger**
* Sistem ini adalah **alat bantu manajemen UMKM**

Prinsip yang dipakai:

* ✔ Stok = Barang Masuk − Penjualan
* ✔ HPP = Rata-rata harga beli dari barang masuk
* ✔ Penjualan TIDAK mengubah HPP
* ✔ HPP hanya dipakai untuk hitung laba

---

## 📂 STRUKTUR SHEET (FINAL)

### 🟢 INPUT (USER BOLEH EDIT)

1. **Master_Barang**
2. **Master_Konversi_Satuan**
3. **Barang_Masuk**
4. **Penjualan**

### 🟡 HELPER 

5. **HPP_Sederhana**

### 🔵 OUTPUT (READ ONLY)

6. **Laporan_Stok**
7. **Laporan_Penjualan**
8. **Laporan_Laba**

❌ Sheet `Stok_Mutasi` dan `HPP_Calc` lama **tidak dipakai lagi**

---

## 1️⃣ MASTER_BARANG

Berisi daftar barang utama.

| Kolom        | Keterangan                     |
| ------------ | ------------------------------ |
| Kode_Barang  | Kode unik barang               |
| Nama_Barang  | Nama barang                    |
| Kategori     | Opsional                       |
| Satuan_Dasar | Unit terkecil (PCS / KG / LTR) |
| Stok_Minimum | Untuk alert                    |
| Status       | Aktif / Nonaktif               |

📌 **Catatan:**

* `Satuan_Dasar` bersifat **konseptual**, sistem menghitung dalam *unit terkecil*

---

## 2️⃣ MASTER_KONVERSI_SATUAN

Digunakan untuk konversi satuan jual/beli ke unit terkecil.

| Kolom           | Keterangan               |
| --------------- | ------------------------ |
| Kode_Barang     | Relasi ke master         |
| Satuan          | PCS / Dus / Karung / dll |
| Konversi_ke_PCS | Faktor konversi          |
| Harga_Jual      | Harga jual per satuan    |

---

## 3️⃣ BARANG_MASUK

Input pembelian barang.

Kolom penting:

* `Kode_Barang` (dropdown)
* `Nama_Barang` (auto dari master)
* `Qty_Masuk`
* `Konversi_ke_PCS`
* `Total_PCS` = Qty × Konversi
* `Harga_Beli_per_Satuan`
* `Total_Nilai_Pembelian`

📌 **Sheet ini adalah sumber utama HPP**

---

## 4️⃣ PENJUALAN

Input transaksi penjualan.

Kolom penting:

* `Kode_Barang`
* `Nama_Barang` (auto)
* `Qty_Jual`
* `Konversi_ke_PCS`
* `Total_PCS`
* `Harga_Jual_per_Satuan`
* `Total_Penjualan`

📌 **Sheet ini hanya mempengaruhi stok & omzet**

---

## 5️⃣ LAPORAN_STOK (OPS A – SEDERHANA)

### Rumus inti:

```excel
Stok Akhir = Total Barang Masuk − Total Penjualan
```

Contoh rumus:

```excel
=SUMIFS(Barang_Masuk!Total_PCS;Barang_Masuk!Kode_Barang;A2)
 -
 SUMIFS(Penjualan!Total_PCS;Penjualan!Kode_Barang;A2)
```

📌 **Tidak menggunakan mutasi stok atau ledger**

---

## 6️⃣ HPP_SEDERHANA (HELPER)

Digunakan untuk menghitung HPP rata-rata beli.

| Kolom             | Fungsi                          |
| ----------------- | ------------------------------- |
| Total_Qty_Masuk   | Akumulasi qty beli              |
| Total_Nilai_Masuk | Akumulasi nilai beli            |
| HPP_Rata2         | Total_Nilai / Total_Qty         |
| Stok_Akhir        | Diambil dari Laporan_Stok       |
| HPP_Aktif         | HPP dipakai (0 jika stok habis) |

📌 **HPP tidak dipengaruhi penjualan**

---

## 7️⃣ LAPORAN_LABA (PER TRANSAKSI)

Satu baris = satu transaksi penjualan.

| Kolom           | Keterangan         |
| --------------- | ------------------ |
| Qty_Terjual     | Dari Penjualan     |
| Total_Penjualan | Omzet              |
| HPP_PCS         | Dari HPP_Sederhana |
| Total_HPP       | Qty × HPP          |
| Laba_Kotor      | Penjualan − HPP    |

📌 Angka desimal wajar, bisa dibulatkan untuk tampilan

---

## ⚠️ BATASAN SISTEM  

* Tidak cocok untuk audit akuntan besar
* Tidak menghitung nilai stok berjalan
* Tidak FIFO / LIFO

Namun:

* ✔ Stabil
* ✔ Mudah dipakai
* ✔ Cocok UMKM

---

## ✅ KESIMPULAN

Sistem ini dirancang agar:

> **Dipakai setiap hari tanpa bikin pusing**

Lebih baik sederhana tapi konsisten,
daripada kompleks tapi rapuh.

file Gratis di pakai dan silahkan dikembangkan kembali, 
mohon upload ulang perbaikannya agar yang lain bisa menggunakannya
open donasi jika memang dibutuhkan untuk pengembangan lanjut 

📌 README ini menjadi **dokumen acuan resmi** untuk pengembangan & pemakaian sistem Excel UMKM ini.
