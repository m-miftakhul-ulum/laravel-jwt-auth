# Auth App

Aplikasi autentikasi berbasis Laravel dengan JWT menggunakan algoritma RSA (`RS256`).

## 1. Requirement

- PHP >= 8.2
- Composer
- Node.js dan npm
- OpenSSL
- Database SQLite (default) atau database lain yang didukung Laravel

Pastikan semua command tersedia:

```bash
php --version
composer --version
node --version
npm --version
openssl version
```

## 2. Instalasi Laravel

Masuk ke folder aplikasi:

```bash
cd auth-app
```

Install dependency PHP dan JavaScript:

```bash
composer install
npm install
```

Buat file environment dan application key:

```bash
copy .env.example .env
php artisan key:generate
```

> Pada macOS/Linux, gunakan `cp .env.example .env` sebagai pengganti `copy`.

Secara default aplikasi menggunakan SQLite. Jika file database belum ada, buat file tersebut:

```bash
type nul > database\database.sqlite
```

> Pada macOS/Linux, gunakan `touch database/database.sqlite`.

Jalankan migrasi:

```bash
php artisan migrate
```

Build asset frontend:

```bash
npm run build
```

Jalankan aplikasi:

```bash
php artisan serve
```

Aplikasi tersedia di `http://127.0.0.1:8000`.

## 3. Generate JWT Key

Buat folder penyimpanan key jika belum ada:

```bash
mkdir storage\jwt
```

Generate private key RSA 2048-bit:

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out storage/jwt/jwt-private.pem
```

Generate public key dari private key:

```bash
openssl rsa -in storage/jwt/jwt-private.pem -pubout -out storage/jwt/jwt-public.pem
```

Tambahkan konfigurasi berikut ke `.env`:

```dotenv
JWT_ALGO=RS256
JWT_PRIVATE_KEY=storage/jwt/jwt-private.pem
JWT_PUBLIC_KEY=storage/jwt/jwt-public.pem
JWT_PASSPHRASE=null
```

Bersihkan cache konfigurasi setelah mengubah `.env`:

```bash
php artisan config:clear
```

Jangan commit `jwt-private.pem` ke repository. Private key harus disimpan secara rahasia dan dibuat ulang jika pernah terekspos.

<!-- ## Testing

```bash
php artisan test
``` -->

