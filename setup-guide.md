# Panduan Setup Lengkap Bot Telegram N8N

## Langkah 1: Instalasi N8N

### Opsi A: Menggunakan npm (Recommended)
```bash
npm install -g n8n
```

### Opsi B: Menggunakan Docker
```bash
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n
```

### Opsi C: N8N Cloud
Daftar di https://n8n.cloud untuk menggunakan versi cloud

## Langkah 2: Menjalankan N8N

```bash
n8n start
```

Buka browser dan akses: http://localhost:5678

## Langkah 3: Setup Bot Telegram

### 3.1 Membuat Bot Baru
1. Buka Telegram dan cari `@BotFather`
2. Kirim `/newbot`
3. Berikan nama bot: `Laporan Penjualan Bot`
4. Berikan username: `laporan_penjualan_bot` (harus unik)
5. Simpan token yang diberikan

### 3.2 Mendapatkan Chat ID (PENTING)
1. Kirim pesan ke bot Anda
2. Buka URL berikut di browser (ganti <BOT_TOKEN> dengan token bot Anda):
   ```
   https://api.telegram.org/bot<BOT_TOKEN>/getUpdates
   ```
3. Cari nilai `"id"` dalam objek `"chat"` di respons JSON
4. Simpan Chat ID ini di file `.env` sebagai `ADMIN_CHAT_ID`

**PENTING:** Chat ID ini wajib diisi dengan benar untuk menghindari error "chat_id is empty". Jika tidak diisi, bot tidak akan bisa mengirim pesan laporan otomatis atau respons.

### 3.3 Mengatur Bot Commands (Opsional)
Kirim ke @BotFather:
```
/setcommands
```
Pilih bot Anda, lalu kirim:
```
start - Memulai bot dan melihat panduan
laporan - Mendapatkan laporan keuangan
```

## Langkah 4: Import dan Konfigurasi Workflow

### 4.1 Import Workflow
1. Di N8N dashboard, klik "Import from file"
2. Upload file `workflow.json`
3. Workflow akan muncul di dashboard

### 4.2 Setup Credentials
1. Klik pada node "Telegram Trigger"
2. Klik "Create New Credential"
3. Masukkan Bot Token dari BotFather
4. Simpan dengan nama "Telegram API"
5. Ulangi untuk semua node Telegram lainnya

### 4.3 Konfigurasi Chat ID Admin
1. Buka node "Set Admin Chat ID"
2. Ganti `YOUR_CHAT_ID_HERE` dengan Chat ID Anda
3. Simpan perubahan

### 4.4 Konfigurasi Environment Variables
1. Pastikan file `.env` sudah dibuat dari `config-example.env`
2. Pastikan variabel `ADMIN_CHAT_ID` sudah diisi dengan benar
3. Tambahkan variabel `N8N_BLOCK_ENV_ACCESS_IN_NODE=false` untuk mengizinkan akses ke environment variables
4. Restart N8N untuk menerapkan perubahan

### 4.5 Test Webhook
1. Klik node "Telegram Trigger"
2. Klik "Listen for Test Event"
3. Kirim pesan `/start` ke bot Anda
4. Pastikan webhook menerima data

## Langkah 5: Aktivasi dan Testing

### 5.1 Aktivasi Workflow
1. Klik toggle "Active" pada workflow
2. Pastikan status berubah menjadi "Active"

### 5.2 Testing Fitur

**Test 1: Welcome Message**
- Kirim `/start` ke bot
- Bot harus membalas dengan pesan selamat datang

**Test 2: Input Data Produk**
- Kirim format data produk:
  ```
  📦 Nama Produk: Test Product
  💰 Harga Jual: 100000
  📊 Margin: 20000
  💸 Cost: 80000
  ```
- Bot harus konfirmasi data tersimpan

**Test 3: Generate Laporan**
- Kirim `/laporan` ke bot
- Bot harus mengirim ringkasan dan file Excel

## Langkah 6: Konfigurasi Lanjutan

### 6.1 Setting Timezone
1. Buka workflow settings
2. Set timezone ke "Asia/Jakarta"
3. Simpan perubahan

### 6.2 Backup Data
Buat script backup untuk file CSV:
```bash
# backup-data.sh
#!/bin/bash
DATE=$(date +%Y%m%d)
cp data_penjualan.csv backup/data_penjualan_$DATE.csv
```

### 6.3 Monitoring
1. Setup log monitoring di N8N
2. Buat alert untuk error
3. Monitor penggunaan resource

## Troubleshooting Umum

### Error: "Webhook not found"
**Solusi:**
1. Restart workflow
2. Cek bot token
3. Regenerate webhook URL

### Error: "Unauthorized"
**Solusi:**
1. Cek bot token di credentials
2. Pastikan bot tidak di-disable
3. Cek permission bot

### Error: "File not found"
**Solusi:**
1. Pastikan direktori kerja N8N benar
2. Cek permission write file
3. Buat file CSV manual jika perlu

### Bot tidak merespons
**Solusi:**
1. Cek status workflow (harus Active)
2. Cek log error di N8N
3. Test webhook manual
4. Restart N8N service

## Optimasi Performance

### 1. Database Optimization
- Gunakan database proper (PostgreSQL/MySQL) untuk data besar
- Index pada kolom tanggal untuk query cepat

### 2. File Management
- Rotate file CSV bulanan
- Compress file lama
- Cleanup file temporary

### 3. Memory Management
- Set limit memory untuk N8N
- Monitor usage RAM
- Cleanup workflow history

## Security Best Practices

### 1. Bot Token Security
- Jangan commit token ke repository
- Gunakan environment variables
- Rotate token secara berkala

### 2. Access Control
- Whitelist Chat ID yang diizinkan
- Implement rate limiting
- Log semua aktivitas

### 3. Data Protection
- Encrypt file data sensitif
- Backup dengan encryption
- Implement data retention policy

## Maintenance Rutin

### Harian
- Cek status workflow
- Monitor error logs
- Verify laporan otomatis

### Mingguan
- Backup data
- Update N8N version
- Clean temporary files

### Bulanan
- Archive old data
- Performance review
- Security audit

---

## Kontak Support

Jika mengalami masalah yang tidak bisa diselesaikan:
1. Cek dokumentasi resmi N8N
2. Cari di community forum N8N
3. Buat issue di GitHub repository

**Happy Automating! 🚀**