# Troubleshooting Guide untuk Bot Telegram N8N

## Error: "access to env vars denied"

**Penyebab:**
Error ini terjadi ketika bot mencoba mengakses environment variables tetapi akses tersebut diblokir oleh konfigurasi N8N.

**Solusi:**
1. Tambahkan variabel berikut ke file `.env`:
   ```
   N8N_BLOCK_ENV_ACCESS_IN_NODE=false
   ```
2. Restart N8N setelah mengubah environment variables
3. Pastikan file `.env` berada di direktori yang benar dan dapat diakses oleh N8N

## Error: "Bad request - please check your parameters"

### Error: "Bad Request: chat_id is empty"

**Penyebab:**
Error ini terjadi ketika bot mencoba mengirim pesan tetapi tidak memiliki nilai chat_id yang valid. Hal ini biasanya terjadi pada:
1. Laporan otomatis terjadwal
2. Saat mengirim laporan tanpa interaksi user sebelumnya
3. Ketika environment variable ADMIN_CHAT_ID tidak diatur dengan benar

**Solusi:**
1. Pastikan file `.env` sudah dibuat dari `config-example.env` dan nilai `ADMIN_CHAT_ID` sudah diisi dengan benar
2. Restart workflow setelah mengubah environment variables
3. Pastikan bot sudah diaktifkan dan Anda sudah mengirim pesan `/start` ke bot

**Cara Mendapatkan Chat ID:**
1. Kirim pesan ke bot Anda
2. Buka URL berikut di browser (ganti YOUR_BOT_TOKEN dengan token bot Anda):
   ```
   https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates
   ```
3. Cari nilai `"id"` dalam objek `"chat"` di respons JSON
4. Salin nilai tersebut ke environment variable `ADMIN_CHAT_ID`

## Error: "Unauthorized"

**Penyebab:**
Error ini terjadi ketika token bot tidak valid atau bot telah dinonaktifkan.

**Solusi:**
1. Periksa apakah token bot sudah benar di credentials
2. Pastikan bot tidak dinonaktifkan oleh BotFather
3. Coba buat bot baru jika diperlukan

## Error: "Webhook not found"

**Penyebab:**
Webhook tidak terdaftar dengan benar di Telegram API.

**Solusi:**
1. Restart workflow
2. Pastikan N8N bisa diakses dari internet (untuk webhook)
3. Coba gunakan polling mode jika webhook tidak berfungsi

## Bot tidak merespons

**Penyebab:**
Bisa disebabkan oleh berbagai masalah konfigurasi atau koneksi.

**Solusi:**
1. Pastikan workflow aktif (status "Active")
2. Periksa log error di N8N
3. Pastikan kredensial Telegram dan Google Sheets sudah benar
4. Restart N8N service

## Error Google Sheets

**Penyebab:**
Masalah dengan kredensial atau akses Google Sheets.

**Solusi:**
1. Re-authenticate Google Sheets OAuth2
2. Pastikan Google Sheets API sudah diaktifkan
3. Periksa apakah Google Sheet ID sudah benar
4. Pastikan sheet "Data Penjualan" ada di spreadsheet

## Langkah Verifikasi Konfigurasi

1. **Periksa Environment Variables**
   ```bash
   # Pastikan file .env berisi:
   TELEGRAM_BOT_TOKEN=your_bot_token_here
   ADMIN_CHAT_ID=your_chat_id_here
   GOOGLE_SHEET_ID=your_google_sheet_id_here
   ```

2. **Periksa Credentials di N8N**
   - Telegram API credentials sudah benar
   - Google Sheets OAuth2 credentials sudah benar

3. **Periksa Workflow**
   - Semua node sudah terkonfigurasi dengan benar
   - Workflow dalam status "Active"

4. **Test Bot**
   - Kirim `/start` ke bot
   - Kirim format data produk untuk test
   - Kirim `/laporan` untuk test laporan

## Kontak Support

Jika masalah masih berlanjut setelah mencoba solusi di atas, silakan:
1. Periksa dokumentasi resmi N8N: https://docs.n8n.io/
2. Cari di forum komunitas N8N
3. Buat issue di GitHub repository

---

**Catatan:** Panduan troubleshooting ini akan terus diperbarui sesuai dengan feedback dan masalah yang ditemukan.