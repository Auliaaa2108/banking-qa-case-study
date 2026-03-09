# Test Plan: Fitur Transfer Antar Bank (Fund Transfer)

## 1. Pendahuluan
Dokumen ini menguraikan strategi pengujian untuk fitur Transfer Antar Bank, yang dirancang untuk memastikan integritas transaksi, validasi data, dan keamanan sistem.

## 2. Ruang Lingkup (Scope)
- Pengujian Fungsional (End-to-End).
- Pengujian Batas (Boundary Value Analysis).
- Pengujian Negatif (Negative Testing).
- Integrasi API (Transfer antar bank via Payment Gateway).

## 3. Matriks Skenario Pengujian

| ID | Skenario | Langkah Pengujian | Hasil yang Diharapkan |
| :--- | :--- | :--- | :--- |
| **TC-01** | Transfer Berhasil | Masukkan nominal valid, pilih bank, masukkan PIN. | Saldo berkurang, status sukses, mutasi tercatat. |
| **TC-02** | Saldo Tidak Cukup | Transfer nominal > saldo akun. | Sistem menolak dengan pesan "Saldo tidak mencukupi". |
| **TC-03** | Rekening Tidak Valid | Masukkan nomor rekening tujuan yang tidak terdaftar. | Sistem menampilkan pesan "Rekening tujuan tidak ditemukan". |
| **TC-04** | Nominal Nol (0) | Input nominal 0. | Sistem menolak input (validasi pada UI/API). |
| **TC-05** | Double Submit | Klik tombol "Kirim" dua kali dengan cepat. | Transaksi hanya terproses satu kali (Idempotency). |

## 4. Strategi Database
Untuk memvalidasi transaksi, saya menggunakan SQL query berikut untuk memastikan tidak terjadi ketidakseimbangan data:
```sql
-- Cek saldo setelah transaksi
SELECT account_id, balance FROM accounts WHERE account_id = '12345';
-- Pastikan tidak ada transaksi ganda dengan reference_id yang sama
SELECT count(*), reference_id FROM transactions GROUP BY reference_id HAVING count(*) > 1;
