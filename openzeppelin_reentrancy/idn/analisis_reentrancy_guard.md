# Bedah Mendalam OpenZeppelin ReentrancyGuard 🔐

**Author:** M. Afif Al Falah  
**Topik:** Keamanan Smart Contract dan Optimasi Gas  

---

## Apa itu Reentrancy? (Analogi Kasir) 🏦

Reentrancy adalah teknik menipu smart contract dengan cara memanggil ulang fungsi sebelum proses sebelumnya selesai.

Bayangkan kita di kasir bank:

1. Kasir cek saldo kita *(ada duit)*.  
2. Kasir kasih uang cash.  
3. Kasir baru mau catat pengurangan saldo di buku.

**Celahnya:**  
Hacker memanfaatkan jeda di langkah ke-2. Saat uang diterima, hacker langsung minta uang lagi **sebelum** kasir sempat menulis di buku pada langkah ke-3.  

Karena catatan belum berubah, kasir mengira saldo masih ada dan memberi uang lagi.  
Begitu terus sampai brankas kosong.

---

## Solusi: Gembok `nonReentrant` 🛡️

Modifier ini bekerja seperti kunci pintu kamar mandi:

- **Masuk:** Kunci pintu *(ubah status jadi `ENTERED`)*  
- **Proses:** Lakukan transaksi di dalam  
- **Keluar:** Buka kunci *(ubah status jadi `NOT_ENTERED`)*  

Kalau hacker coba masuk lagi saat pintu terkunci, transaksi akan ditolak *(revert)*.

---

## Kenapa Pakai Angka `1` dan `2`? (Bukan `true` atau `false`) ⛽

Ini bagian paling menarik. OpenZeppelin tidak memakai `bool`, tetapi menggunakan `uint256` dengan angka `1` dan `2`.

---

### Alasan A: Masalah Kerdus Penyimpanan (Storage Slot)

- `uint256` ukurannya pas satu slot penuh *(32 byte)*.  
  Kalau mau ubah isinya, EVM tinggal mengganti seluruh slot *(write only)* → lebih hemat gas.

- `bool` ukurannya kecil dan sering berbagi slot dengan variabel lain.  
  Saat mau diubah, EVM harus:
  - Membaca slot dulu *(read)*  
  - Menjaga agar data lain tidak rusak  
  - Mengubah bit yang diperlukan  
  - Menulis ulang slot *(write)*  

Proses baca–ubah–tulis ini lebih boros gas.

**Kesimpulan:**  
`uint256` menghindari proses baca tambahan (`SLOAD`), sehingga lebih efisien untuk kasus ini.

---

### Alasan B: Masalah Biaya Renovasi (Gas Cost) 🔥

**Ubah `0` ke `1`:**

Ibarat membangun di lahan kosong.  
Biayanya sangat mahal (~20.000 gas) karena slot storage baru diinisialisasi.

**Ubah `1` ke `2`:**

Ibarat renovasi rumah yang sudah ada.  
Biayanya jauh lebih murah (~2.900–5.000 gas) karena slot sudah aktif (*warm storage*).

**Logikanya:**

OpenZeppelin sengaja menghindari angka `0`.  
Mereka memakai `1` dan `2` agar status storage selalu “sudah terisi”.

Hasilnya:
- Tidak perlu biaya mahal untuk inisialisasi awal  
- Biaya gas lebih stabil dan murah untuk pengguna

## 📚 Resources (Bahasa Indonesia)

- Smart contract sumber (OpenZeppelin ReentrancyGuard)  
  [Buka file ReentrancyGuard.sol di GitHub](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/ReentrancyGuard.sol)

- Repository resmi OpenZeppelin Contracts  
  [OpenZeppelin Contracts Repository](https://github.com/OpenZeppelin/openzeppelin-contracts)
