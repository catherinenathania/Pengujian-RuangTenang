# Hands-On Security Audit: RuangTenang 🌿
**Mata Kuliah: Keamanan Aplikasi Berbasis Web**

Repositori ini berisi kode sumber aplikasi **RuangTenang** yang digunakan sebagai objek studi kasus untuk analisis keamanan web. RuangTenang adalah platform dukungan psikologis bagi korban kekerasan seksual yang dibangun dengan framework Laravel.

---

##  Ringkasan Analisis Keamanan (Hands-On)

Aplikasi ini telah diuji terhadap beberapa aspek keamanan utama untuk mengidentifikasi kerentanan dan melakukan perbaikan.

### 1. Fase Reconnaissance (Pengintaian)
- **Fingerprinting**: Mengidentifikasi teknologi menggunakan `Wappalyzer` dan analisis HTTP Header.
- **Directory Discovery**: Melakukan pemindaian direktori sensitif pada localhost (Analisis struktur folder `/storage`, `/vendor`, dan `.env`).
- **Security Headers Analysis**: Mengevaluasi keberadaan header penting seperti `Content-Security-Policy`, `X-Frame-Options`, dan `Strict-Transport-Security`.

### 2. Audit Ekosistem Pihak Ketiga
- **Composer Audit**: Menjalankan `composer audit` untuk mendeteksi kerentanan pada library PHP yang digunakan.
- **CVE Checking**: Melakukan verifikasi versi Laravel (v11.x) terhadap database *Common Vulnerabilities and Exposures*.

### 3. Implementasi Secure Coding (Laravel)
Aplikasi ini menerapkan standar keamanan bawaan Laravel dan peningkatan mandiri:
- **Autentikasi**: Menggunakan `Bcrypt` untuk hashing password.
- **Proteksi Injeksi**: Implementasi Eloquent ORM untuk mencegah *SQL Injection*.
- **CSRF Protection**: Penggunaan middleware `VerifyCsrfToken` pada seluruh formulir input (Diary, Mood Tracker, Laporan).
- **XSS Prevention**: Mekanisme auto-escaping pada mesin templating Blade.

---

##  Tech Stack & Lingkungan Pengujian
- **Framework**: Laravel 11 (PHP 8.2)
- **Database**: MySQL via XAMPP
- **Tools Analisis**: 
  - OWASP ZAP / Burp Suite (Web Scanning)
  - Wappalyzer (Technology Fingerprinting)
  - Browser Developer Tools (Header & Network Analysis)
  - Composer Audit (Dependency Security)

---

##  Komponen Utama yang Diaudit

- **`app/Http/Controllers/AuthController.php`**: Analisis logika autentikasi dan manajemen sesi.
- **`routes/web.php`**: Peninjauan rute aplikasi dan proteksi middleware.
- **`resources/views/`**: Pemeriksaan potensi *Stored XSS* pada fitur Buku Harian dan Artikel.
- **`.env`**: Simulasi risiko kebocoran kredensial jika `APP_DEBUG` aktif di lingkungan produksi.

---

##  Cara Menjalankan untuk Keperluan Audit

1. **Clone & Install**
   ```bash
   git clone https://github.com/catherinenathania/RuangTenang.git
   cd RuangTenang
   composer install
   ```

2. **Setup Environment**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```
   *Catatan: Pastikan `APP_DEBUG=true` hanya digunakan untuk analisis kerentanan di lingkungan lokal.*

3. **Database Migration**
   ```bash
   php artisan migrate
   ```

4. **Run Server**
   ```bash
   php artisan serve
   ```

---
*Disclaimer: Aplikasi ini adalah proyek pembelajaran. Jangan digunakan di lingkungan produksi tanpa penguatan keamanan lebih lanjut.*
