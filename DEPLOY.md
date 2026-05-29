# Panduan Deploy ke Railway (Gratis)

## Persiapan

### 1. Install Git (kalau belum)
Download di https://git-scm.com/download/win

### 2. Buat akun Railway
Buka https://railway.app → Sign up with GitHub (gratis)

---

## Langkah Deploy

### Step 1 — Push project ke GitHub

Buka terminal di folder project (`c:\laragon\www\website-pembayaran-app`):

```bash
git init
git add .
git commit -m "first commit"
```

Buat repo baru di https://github.com/new (nama: `website-pembayaran-app`, set Private)

```bash
git remote add origin https://github.com/USERNAME/website-pembayaran-app.git
git branch -M main
git push -u origin main
```

---

### Step 2 — Buat project di Railway

1. Buka https://railway.app/new
2. Klik **"Deploy from GitHub repo"**
3. Pilih repo `website-pembayaran-app`
4. Railway akan mulai detect project

---

### Step 3 — Tambah database MySQL

1. Di dashboard Railway, klik **"+ New"** → **"Database"** → **"MySQL"**
2. MySQL akan otomatis terhubung ke project kamu

---

### Step 4 — Set Environment Variables

Di Railway → project kamu → tab **"Variables"**, tambahkan satu per satu:

| Key | Value |
|-----|-------|
| `APP_NAME` | Kas Humanika |
| `APP_ENV` | production |
| `APP_DEBUG` | false |
| `APP_KEY` | *(generate dulu, lihat di bawah)* |
| `APP_URL` | https://your-app.up.railway.app |
| `DB_CONNECTION` | mysql |
| `DB_HOST` | `${{MySQL.MYSQLHOST}}` |
| `DB_PORT` | `${{MySQL.MYSQLPORT}}` |
| `DB_DATABASE` | `${{MySQL.MYSQLDATABASE}}` |
| `DB_USERNAME` | `${{MySQL.MYSQLUSER}}` |
| `DB_PASSWORD` | `${{MySQL.MYSQLPASSWORD}}` |
| `SESSION_DRIVER` | file |
| `CACHE_STORE` | file |
| `QUEUE_CONNECTION` | sync |
| `LOG_CHANNEL` | stderr |

**Generate APP_KEY** — jalankan di terminal lokal:
```bash
php artisan key:generate --show
```
Salin hasilnya (format: `base64:xxx...`) ke variable `APP_KEY` di Railway.

---

### Step 5 — Deploy

Railway otomatis deploy setelah variable diset. Tunggu 2-3 menit.

Setelah selesai, klik **"View Logs"** untuk cek status.
URL app kamu akan muncul di tab **"Settings"** → **"Domains"**.

---

## Catatan Penting

- **Free tier Railway:** $5 credit/bulan, cukup untuk project kecil aktif ~500 jam
- **Upload foto:** File yang diupload di Railway akan hilang saat redeploy karena filesystem-nya ephemeral. Untuk production, sebaiknya pakai Cloudinary (gratis) atau S3.
- Setiap `git push` ke GitHub akan otomatis trigger redeploy di Railway.
