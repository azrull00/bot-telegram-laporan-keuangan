# Bot Telegram Laporan Penjualan N8N

Bot Telegram yang dibuat dengan N8N untuk mengelola data penjualan produk dan menghasilkan laporan keuangan dalam format Excel secara otomatis.

## 🚀 Fitur Utama

- **Input Data Produk**: Terima data penjualan via Telegram dengan format terstruktur
- **Google Sheets Integration**: Simpan data ke Google Sheets untuk akses cloud dan kolaborasi
- **Laporan Excel**: Generate laporan keuangan dalam format Excel (.xlsx)
- **Laporan Harian Otomatis**: Kirim laporan secara otomatis setiap hari
- **Kalkulasi Otomatis**: Hitung total penjualan, margin, cost, dan profit
- **Interface Telegram**: Interaksi mudah melalui bot Telegram
- **Real-time Access**: Data tersimpan di cloud dan bisa diakses real-time
- **Backup Otomatis**: Google Sheets menyediakan backup otomatis

## ⚙️ Instalasi dan Konfigurasi

### 1. Setup Google Sheets
```bash
# Ikuti panduan lengkap di google-sheets-setup.md
1. Buat Google Spreadsheet
2. Setup Google Cloud Project
3. Enable Google Sheets API
4. Buat OAuth2 credentials
```

### 2. Import Workflow
```bash
# Di N8N interface
1. Klik "+ Add workflow"
2. Pilih "Import from file"
3. Upload file workflow.json
```

### 3. Setup Credentials

#### Telegram API
```
Credential Type: Telegram API
Access Token: [BOT_TOKEN_DARI_BOTFATHER]
```

#### Google Sheets OAuth2
```
Credential Type: Google Sheets OAuth2 API
Client ID: [DARI_GOOGLE_CLOUD_CONSOLE]
Client Secret: [DARI_GOOGLE_CLOUD_CONSOLE]
```

#### Konfigurasi IDs
```
# Buat file .env dari config-example.env
cp config-example.env .env

# Edit file .env dan isi nilai-nilai berikut:
TELEGRAM_BOT_TOKEN=your_bot_token_here
ADMIN_CHAT_ID=your_chat_id_here  # PENTING untuk menghindari error "chat_id is empty"
GOOGLE_SHEET_ID=your_google_sheet_id_here
```

**PENTING:** 
- Pastikan `ADMIN_CHAT_ID` diisi dengan benar untuk menghindari error "Bad Request: chat_id is empty". 
- Tambahkan `N8N_BLOCK_ENV_ACCESS_IN_NODE=false` untuk menghindari error "access to env vars denied".

Lihat panduan di TROUBLESHOOTING.md jika mengalami masalah.

### 4. Aktivasi
```
1. Set credentials di semua nodes
2. Save workflow
3. Activate workflow
4. Test dengan mengirim /start ke bot
```

## Cara Penggunaan

### Memulai Bot

Kirim perintah `/start` ke bot untuk melihat panduan penggunaan.

### Menambah Data Produk

Kirim pesan dengan format berikut:

```
📦 Nama Produk: Laptop Gaming
💰 Harga Jual: 15000000
📊 Margin: 2000000
💸 Cost: 13000000
```

**Format yang harus diikuti:**
- Gunakan emoji dan teks persis seperti contoh
- Harga dalam angka tanpa titik atau koma
- Semua field wajib diisi

### Mendapatkan Laporan

Kirim perintah `/laporan` untuk mendapatkan:
- Ringkasan keuangan dalam chat
- File Excel dengan detail lengkap

### Laporan Otomatis

Bot akan mengirim laporan otomatis setiap hari jam 9:00 pagi ke admin yang sudah dikonfigurasi.

## 📊 Struktur Data

### Google Sheets (Data Penjualan)
| tanggal | nama_produk | harga_jual | margin | cost |
|---------|-------------|------------|--------|------|
| 2024-01-15 | Laptop Gaming | 15000000 | 2000000 | 13000000 |
| 2024-01-15 | Mouse Wireless | 250000 | 50000 | 200000 |

### Input Data Produk
- **Nama Produk**: Nama/deskripsi produk
- **Harga Jual**: Harga jual ke customer (Rupiah)
- **Margin**: Keuntungan per unit (Rupiah)
- **Cost**: Biaya produksi/pembelian (Rupiah)

### Output Laporan Excel
- Detail semua transaksi dengan tanggal
- Total penjualan
- Total margin
- Total cost
- Total profit (Penjualan - Cost)
- Jumlah produk terjual

## 🔧 Troubleshooting

### Bot tidak merespon
- Cek Bot Token sudah benar
- Pastikan webhook aktif
- Cek N8N workflow sudah aktif

### Data tidak tersimpan ke Google Sheets
- Cek Google Sheets credentials sudah ter-set
- Pastikan Google Sheets API sudah di-enable
- Cek Google Sheet ID sudah benar
- Pastikan format pesan sudah sesuai
- Cek permission Google Sheets

### Error "The caller does not have permission"
- Re-authenticate Google Sheets OAuth2
- Pastikan Google account memiliki akses ke spreadsheet
- Cek Google Sheets API quota

### Laporan kosong
- Pastikan Google Sheets berisi data
- Cek sheet name "Data Penjualan" sudah benar
- Pastikan node "Read Google Sheets Data" berjalan
- Cek credentials di node pembaca data

### Error Excel generation
- Cek data format di Google Sheets
- Pastikan koneksi ke Google Sheets stabil
- Restart N8N jika perlu

## Kustomisasi

### Mengubah Jadwal Laporan Otomatis

Edit node "Daily Schedule (9 AM)" dan ubah cron expression:
- `0 9 * * *` = Setiap hari jam 9:00
- `0 17 * * *` = Setiap hari jam 17:00
- `0 9 * * 1` = Setiap Senin jam 9:00

### Menambah Field Data

1. Edit node "Parse Product Data" untuk parsing field baru
2. Update node "Save to CSV" untuk menyimpan field baru
3. Modifikasi node "Generate Report" untuk include field baru

### Mengubah Format Laporan

Edit node "Generate Report" untuk mengubah:
- Format Excel
- Perhitungan tambahan
- Layout laporan

## 📁 File dan Data yang Dihasilkan

### 1. Google Sheets: Data Penjualan
- Penyimpanan data utama di cloud
- Real-time access dan collaboration
- Backup otomatis oleh Google
- Bisa diakses via web dan mobile app

### 2. laporan_keuangan_YYYY-MM-DD.xlsx
- Laporan Excel harian
- Berisi summary dan detail transaksi
- Dikirim via Telegram
- Generated dari data Google Sheets

## 🔒 Keamanan

- Bot Token disimpan sebagai credential di N8N
- Google Sheets OAuth2 untuk akses aman
- Data tersimpan di Google Cloud (enkripsi otomatis)
- Chat ID admin untuk akses terbatas
- Validasi input data produk
- Google Sheets permissions management

⚠️ **Penting**: 
- Jangan share Bot Token ke orang lain
- Simpan Chat ID admin dengan aman
- Backup file data secara berkala
- Monitor akses ke bot

## Support

Jika mengalami masalah:
1. Cek dokumentasi N8N: https://docs.n8n.io/
2. Cek dokumentasi Telegram Bot API: https://core.telegram.org/bots/api
3. Review log error di N8N dashboard

---

**Dibuat dengan ❤️ menggunakan N8N dan Telegram Bot API**