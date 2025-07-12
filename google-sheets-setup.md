# Setup Google Sheets Integration untuk N8N Telegram Bot

Panduan ini menjelaskan cara mengintegrasikan Google Sheets dengan N8N Telegram Bot untuk menyimpan data penjualan di cloud.

## 🔧 Persiapan Google Sheets

### 1. Buat Google Spreadsheet Baru
1. Buka [Google Sheets](https://sheets.google.com)
2. Klik "+ Blank" untuk membuat spreadsheet baru
3. Beri nama spreadsheet: "Data Penjualan Bot Telegram"
4. Rename sheet pertama menjadi "Data Penjualan"

### 2. Setup Header Kolom
Di sheet "Data Penjualan", tambahkan header di baris pertama:

| A | B | C | D | E |
|---|---|---|---|---|
| tanggal | nama_produk | harga_jual | margin | cost |

### 3. Dapatkan Google Sheet ID
1. Dari URL Google Sheets: `https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit`
2. Copy bagian `[SHEET_ID]`
3. Simpan ID ini untuk konfigurasi N8N

## 🔐 Setup Google Sheets Credentials di N8N

### 1. Buat Google Cloud Project
1. Buka [Google Cloud Console](https://console.cloud.google.com/)
2. Buat project baru atau pilih project yang ada
3. Enable Google Sheets API:
   - Pergi ke "APIs & Services" > "Library"
   - Cari "Google Sheets API"
   - Klik "Enable"

### 2. Setup OAuth2 Credentials
1. Di Google Cloud Console, pergi ke "APIs & Services" > "Credentials"
2. Klik "+ CREATE CREDENTIALS" > "OAuth client ID"
3. Pilih "Web application"
4. Tambahkan Authorized redirect URIs:
   ```
   http://localhost:5678/rest/oauth2-credential/callback
   ```
   (Sesuaikan dengan URL N8N instance Anda)
5. Download file JSON credentials

### 3. Konfigurasi di N8N
1. Buka N8N interface
2. Pergi ke "Settings" > "Credentials"
3. Klik "+ Add Credential"
4. Pilih "Google Sheets OAuth2 API"
5. Masukkan:
   - **Client ID**: Dari file JSON
   - **Client Secret**: Dari file JSON
6. Klik "Connect my account" dan ikuti proses OAuth
7. Simpan dengan nama: "Google Sheets OAuth2"

## ⚙️ Konfigurasi Workflow

### 1. Update Google Sheet ID
Di file `workflow.json`, ganti `YOUR_GOOGLE_SHEET_ID` dengan Sheet ID yang sudah didapat:

```json
"documentId": {
  "__rl": true,
  "value": "1abc123def456ghi789jkl", // Ganti dengan Sheet ID Anda
  "mode": "id"
}
```

### 2. Import Workflow ke N8N
1. Buka N8N interface
2. Klik "+ Add workflow"
3. Klik "Import from file"
4. Upload file `workflow.json`
5. Workflow akan ter-import dengan konfigurasi Google Sheets

### 3. Set Credentials
1. Buka workflow yang sudah di-import
2. Klik pada node "Save to Google Sheets"
3. Di bagian "Credential to connect with", pilih "Google Sheets OAuth2"
4. Lakukan hal yang sama untuk node "Read Google Sheets Data"
5. Save workflow

## 🧪 Testing

### 1. Test Manual
1. Aktifkan workflow di N8N
2. Kirim pesan ke bot Telegram dengan format:
   ```
   📦 Nama Produk: Test Product
   💰 Harga Jual: 100000
   📊 Margin: 20000
   💸 Cost: 80000
   ```
3. Cek Google Sheets - data harus muncul di sheet "Data Penjualan"

### 2. Test Report
1. Kirim `/laporan` ke bot
2. Bot harus mengirim summary dan file Excel
3. Data harus sesuai dengan yang ada di Google Sheets

## 🔄 Keuntungan Google Sheets Integration

### ✅ Manfaat
- **Cloud Storage**: Data tersimpan di cloud, aman dari kehilangan
- **Real-time Access**: Data bisa diakses langsung dari Google Sheets
- **Collaboration**: Tim bisa melihat dan mengedit data secara bersamaan
- **Backup Otomatis**: Google Sheets otomatis backup data
- **Mobile Access**: Bisa diakses dari smartphone via Google Sheets app
- **Advanced Analytics**: Bisa menggunakan Google Sheets functions untuk analisis

### 📊 Fitur Tambahan
- **Charts**: Buat grafik langsung di Google Sheets
- **Pivot Tables**: Analisis data dengan pivot tables
- **Conditional Formatting**: Highlight data berdasarkan kondisi
- **Data Validation**: Validasi input data
- **Sharing**: Share dengan tim atau stakeholder

## 🛠️ Troubleshooting

### Error: "The caller does not have permission"
**Solusi:**
1. Pastikan Google Sheets API sudah di-enable
2. Re-authenticate OAuth2 credentials
3. Pastikan Google account memiliki akses ke spreadsheet

### Error: "Spreadsheet not found"
**Solusi:**
1. Cek Google Sheet ID sudah benar
2. Pastikan spreadsheet tidak di-delete
3. Pastikan sheet name "Data Penjualan" ada

### Data tidak muncul di Google Sheets
**Solusi:**
1. Cek credentials sudah ter-set dengan benar
2. Pastikan workflow sudah aktif
3. Cek log N8N untuk error messages
4. Pastikan format data sesuai dengan header kolom

### Bot tidak merespon command /laporan
**Solusi:**
1. Pastikan ada data di Google Sheets
2. Cek koneksi "Read Google Sheets Data" node
3. Pastikan credentials ter-set di semua Google Sheets nodes

## 📝 Maintenance

### Regular Tasks
1. **Monitor Usage**: Cek Google Sheets API quota usage
2. **Backup**: Export data secara berkala sebagai backup tambahan
3. **Clean Data**: Hapus data lama jika diperlukan
4. **Update Permissions**: Review akses Google Sheets secara berkala

### Security Best Practices
1. **Limit Access**: Berikan akses Google Sheets hanya kepada yang memerlukan
2. **Regular Review**: Review OAuth2 permissions secara berkala
3. **Monitor Activity**: Pantau aktivitas di Google Sheets
4. **Secure Credentials**: Jangan share credentials N8N

## 🔗 Resources

- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
- [N8N Google Sheets Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets/)
- [Google Cloud Console](https://console.cloud.google.com/)
- [N8N Credentials Documentation](https://docs.n8n.io/integrations/builtin/credentials/google/)

---

**Catatan**: Pastikan untuk mengganti semua placeholder (`YOUR_GOOGLE_SHEET_ID`, dll.) dengan nilai yang sesuai dengan setup Anda.