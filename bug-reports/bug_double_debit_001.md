# Bug Report: Double Debit pada Transaksi Transfer Antar Bank

## 1. Ringkasan (Summary)
Nasabah mengalami pemotongan saldo sebanyak dua kali untuk satu kali instruksi transfer yang sama karena adanya *race condition* saat tombol "Kirim" ditekan berulang kali dalam durasi < 1 detik.

## 2. Informasi Bug
- **Severity:** Critical (High Impact on Customer Funds)
- **Status:** Open
- **Environment:** Staging - Mobile App (Android v2.4.1)

## 3. Langkah-Langkah Reproduksi (Steps to Reproduce)
1. Buka aplikasi dan login ke akun nasabah.
2. Navigasi ke menu "Transfer Antar Bank".
3. Isi kolom Rekening Tujuan dan Nominal (Contoh: Rp 1.000.000).
4. Klik tombol "Lanjutkan" untuk masuk ke halaman konfirmasi.
5. Pada halaman konfirmasi, tekan tombol "Konfirmasi Transfer" sebanyak 2-3 kali dengan sangat cepat (rapid tapping).
6. Masukkan PIN transaksi.

## 4. Hasil yang Diharapkan (Expected Result)
Sistem harus memproses hanya satu instruksi transaksi. Jika tombol diklik berulang, sistem harus mengabaikan klik kedua dan seterusnya (*Idempotency check*).

## 5. Hasil Aktual (Actual Result)
Sistem memproses dua transaksi yang identik. Saldo nasabah terpotong Rp 2.000.000 dan tercatat dua nomor referensi transaksi yang berbeda di database.

## 6. Bukti Teknis
- **Request API:** `POST /api/v1/transfer`
- **Error Log:** Terlihat dua request masuk ke server dalam rentang waktu 50ms tanpa pengecekan *idempotency key*.

---
*QA Note: Perlu segera ditambahkan mekanisme 'button debounce' pada sisi Frontend dan 'idempotency header' pada sisi Backend.*
