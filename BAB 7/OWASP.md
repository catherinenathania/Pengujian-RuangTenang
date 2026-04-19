
# Laporan Analisis Risiko Keamanan Berdasarkan OWASP Top 10 — RuangTenang

Dokumentasi ini menyajikan hasil evaluasi keamanan aplikasi RuangTenang dengan menggunakan standar **OWASP Top 10:2021**. Analisis ini mengintegrasikan temuan dari audit *third-party*, keamanan perimeter, dan *secure coding*.

### 1. Tabel Pemetaan Risiko OWASP

| Kode | Kategori Risiko (OWASP 2021) | Temuan Praktik & Bukti Teknis | Status |
| :--- | :--- | :--- | :--- |
| **A01** | **Broken Access Control** | Rute dasbor telah menggunakan middleware `auth`. Namun, area database (phpMyAdmin) masih terbuka tanpa proteksi IP. | **Warning** |
| **A02** | **Cryptographic Failures** | Aplikasi beroperasi pada protokol HTTP. Data sensitif dikirimkan tanpa enkripsi SSL/TLS (Berdasarkan praktik Bab 5). | **High** |
| **A03** | **Injection** | Penggunaan Eloquent ORM pada `AuthController` memastikan semua input melalui *parameterized queries*. | **Safe** |
| **A05** | **Security Misconfiguration** | Berkas `.env` menunjukkan `APP_DEBUG=true` dan penggunaan user `root` tanpa password pada XAMPP (Berdasarkan praktik Bab 4). | **High** |
| **A06** | **Vulnerable and Outdated Components** | Versi Laravel (12.15.0) dan Guzzle (7.9.3) berada pada rilis terbaru dan stabil (Berdasarkan praktik Bab 4). | **Safe** |
| **A07** | **Identification and Authentication Failures** | Tidak ada *Rate Limiting* (Throttle) pada login dan absennya `session()->regenerate()` setelah login (Berdasarkan praktik Bab 6). | **High** |

---

### 2. Hasil Pengamatan

#### **A. Kerentanan Autentikasi (A07:2021)**
Berdasarkan analisis pada `AuthController.php`, ditemukan bahwa sesi tidak diperbarui setelah proses autentikasi berhasil.
* **Risiko:** *Session Fixation* — Penyerang dapat menggunakan ID sesi yang sama untuk mengambil alih akun korban.
* **Bukti:** Ketiadaan fungsi `$request->session()->regenerate()`.
<img width="1344" height="310" alt="Screenshot 2026-04-19 170441" src="https://github.com/user-attachments/assets/7e9f045c-fa78-4efb-9ddc-68413e6ad8c8" />


#### **B. Kegagalan Kriptografi & Jalur Komunikasi (A02:2021)**
Aplikasi tidak memaksa penggunaan koneksi aman.
* **Risiko:** *Man-in-the-Middle (MitM) Attack* — Informasi rahasia dalam buku harian dapat disadap saat transmisi.
* **Bukti:** Indikator "Not Secure" pada bilah alamat peramban (HTTP).
<img width="1919" height="960" alt="Screenshot 2026-04-19 164515" src="https://github.com/user-attachments/assets/99818e96-efed-4684-ba0b-4b91509ad48b" />


#### **C. Kesalahan Konfigurasi Keamanan (A05:2021)**
Mode *debugging* masih aktif pada lingkungan yang dapat diakses.
* **Risiko:** *Information Exposure* — Jika terjadi error, sistem akan membocorkan struktur folder, kueri database, dan versi PHP/Laravel secara detail.
* **Bukti:** Pengaturan `APP_DEBUG=true` di file `.env`.
<img width="1026" height="427" alt="Screenshot 2026-04-19 155405" src="https://github.com/user-attachments/assets/76f69f66-7ab1-4933-9a3c-8f5df6a90e53" />

---

