# API Testing: Fund Transfer

Folder ini berisi kumpulan collection Postman untuk pengujian API Transfer Dana.

## Cara Menggunakan:
1. Unduh file `banking_transfer_api.json` di folder ini.
2. Buka aplikasi Postman Anda.
3. Klik tombol **Import** di kiri atas.
4. Pilih file `.json` yang telah diunduh.

## Detail Pengujian:
- **Base URL:** `https://api.bank-mock.com/v1`
- **Authentication:** Menggunakan `Bearer Token` (Header).
- **Endpoint Utama:** `POST /transfer`
- **Fokus Pengujian:** - Validasi respon 200 OK untuk transaksi berhasil.
  - Validasi pesan error 400 untuk saldo tidak cukup.
  - Validasi struktur JSON respon sesuai dengan dokumentasi sistem.
