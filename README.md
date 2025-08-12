# DBngin MySQL Fixer (Windows)

Script batch untuk memperbaiki masalah MySQL di **DBngin** yang:
- Statusnya selalu hijau (*running*) padahal MySQL sebenarnya mati
- Tidak bisa konek karena file `mysql.pid` nyangkut
- Error **"Port 3306 is already in use"** karena proses `mysqld.exe` lama belum mati

Dengan script ini, kamu bisa memperbaiki masalah tanpa perlu restart perangkat.

---

## ⚠️ Disclaimer

- Gunakan script ini **dengan risiko sendiri**.
- Pastikan path `dbngin-go.exe` sudah benar sebelum menjalankan.
- Script ini mematikan semua proses `mysqld.exe` — jika kamu menjalankan MySQL dari aplikasi lain (XAMPP, WAMP, Laragon, dll.), proses tersebut juga akan ikut berhenti.
- Script ini **tidak menghapus data database**, hanya menghapus file `mysql.pid` (file penanda proses MySQL) dan menghentikan proses MySQL yang nyangkut.

---

## 📜 Fitur
- Menutup DBngin (`dbngin-go.exe`)
- Mematikan semua proses `mysqld.exe` (membebaskan port 3306 & 33060)
- Menghapus semua file `mysql.pid` di folder DBngin
- Menjalankan DBngin kembali

---

## 💻 Script

Simpan script ini dengan nama `fix-dbngin.bat`:

```bat
@echo off
echo ==========================================
echo  DBngin MySQL Fixer
echo  - Kill mysqld.exe
echo  - Hapus file mysql.pid
echo  - Jalankan DBngin
echo ==========================================

REM [1/4] Matikan proses DBngin
echo.
echo [1/4] Menutup DBngin...
taskkill /IM "dbngin-go.exe" /F >nul 2>&1

REM [2/4] Matikan semua proses MySQL
echo [2/4] Mematikan semua mysqld.exe...
taskkill /IM "mysqld.exe" /F >nul 2>&1

REM [3/4] Hapus file mysql.pid
echo [3/4] Menghapus file mysql.pid...
for /r "%LocalAppData%\com.tinyapp.DBngin\Engines\mysql" %%f in (mysql.pid) do (
    if exist "%%f" (
        del /f "%%f"
        echo   Dihapus: %%f
    )
)

REM [4/4] Jalankan DBngin lagi
echo [4/4] Menjalankan DBngin...
start "" "C:\Users\<USERNAME>\AppData\Roaming\DBngin\dbngin-go.exe"

echo.
echo Selesai! DBngin sudah dijalankan ulang.
pause
````

---

## ⚙️ Penyesuaian Path DBngin

Bagian yang **perlu kamu sesuaikan** adalah path ke file `dbngin-go.exe`:

```bat
start "" "C:\Users\<USERNAME>\AppData\Roaming\DBngin\dbngin-go.exe"
```

Ganti `<USERNAME>` dengan nama user di Windows kamu, atau ganti seluruh path sesuai lokasi DBngin di komputer masing-masing.

---

## 🔍 Cara Menemukan Path DBngin

1. **Jika ada shortcut DBngin** (di desktop atau Start Menu):

   * Klik kanan → **Open file location**
   * Jika yang terbuka adalah folder shortcut, klik kanan shortcut tersebut → **Properties**
   * Lihat kolom **Target** — itu adalah path `dbngin-go.exe`

2. **Jika tidak ada shortcut**:

   * Tekan `Win + S` dan ketik `dbngin-go.exe`
   * Klik kanan → **Open file location**
   * Copy path yang muncul

3. **Jika mau manual**:

   * Cek folder:

     ```
     C:\Users\<USERNAME>\AppData\Roaming\DBngin\
     ```

     atau

     ```
     C:\Program Files\DBngin\
     ```

---

## 🚀 Cara Menggunakan Script

1. Simpan script `fix-dbngin.bat` di lokasi yang mudah diakses
2. **Double-click** untuk menjalankan
3. DBngin akan otomatis:

   * Ditutup
   * Membersihkan proses MySQL lama
   * Menghapus file PID yang nyangkut
   * Dijalankan ulang

---

## 📝 Catatan

* Script ini hanya untuk **Windows**
* Jika kamu menggunakan MySQL lain di komputer yang sama (XAMPP, WAMP, Laragon, dll.), pastikan port yang digunakan tidak bentrok
* Untuk ganti port di DBngin, buka pengaturan service MySQL dan ubah dari `3306` ke port lain (misal `3307`)

---

## 📌 Lisensi

Bebas digunakan dan dimodifikasi. Semoga membantu pengguna DBngin lain yang mengalami masalah serupa.
