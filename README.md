# React Native Posts App
Haloww halowww salam kenal aku Muhammad Iqbal AL-Ayyasy
Alhamdulillah saya bisa menyelesaikan tugas di SANTRIKODING ini dengan lancar, sebagai ucapan terimakasih saya membuat file berisi fitur, cara memakai aplikasi, dan juga tutorial step by step yang mudah dengan bahasa yang gampang untuk dimengerti. Semoga Bermanfaat :D

Aplikasi mobile **CRUD Posts** (Create, Read, Update, Delete) yang dibuat dengan **React Native** dan terhubung ke backend **Express + Prisma + MySQL**. Setiap post memiliki **judul dan gambarnyahh**.

Project ini dibuat sebagai bahan belajar Mobile Programming. README ini ditulis selengkap mungkin supaya pemula bisa menjalankannya dari nol, langkah demi langkah.

> **Catatan:** Panduan ini suda diuji di **Windows** dengan **emulator Android Studio**. Kalo Versi iOS belum diuji.

---

## Daftar Isi

1. [Fitur](#1-fitur)
2. [Teknologi yang Dipakai](#2-teknologi-yang-dipakai)
3. [Cara Kerja Singkat](#3-cara-kerja-singkat)
4. [Persiapan Awal (Prasyarat)](#4-persiapan-awal-prasyarat)
5. [Langkah 1: Siapkan Android SDK dan Emulator](#5-langkah-1-siapkan-android-sdk-dan-emulator)
6. [Langkah 2: Siapkan dan Jalankan Backend](#6-langkah-2-siapkan-dan-jalankan-backend)
7. [Langkah 3: Ambil Project Aplikasi](#7-langkah-3-ambil-project-aplikasi)
8. [Langkah 4: Atur File .env](#8-langkah-4-atur-file-env)
9. [Langkah 5: Jalankan Aplikasi](#9-langkah-5-jalankan-aplikasi)
10. [Cara Memakai Aplikasi](#10-cara-memakai-aplikasi)
11. [Struktur Folder](#11-struktur-folder)
12. [Rutinitas Harian (Checklist)](#12-rutinitas-harian-checklist)
13. [Troubleshooting (Kalau Ada Error)](#13-troubleshooting-kalau-ada-error)
14. [Catatan Git dan Keamanan](#14-catatan-git-dan-keamanan)
15. [Ide Pengembangan Lanjutan](#15-ide-pengembangan-lanjutan)

---

## 1. Fitur

- Melihat daftar post (gambar dan judul)
- Menambah post baru dengan mengunggah gambar dari galeri
- Mengubah post (Edit)
- Menghapus post (Delete)

---

## 2. Teknologi yang Dipakai

| Bagian | Teknologi |
|---|---|
| Aplikasi mobile | React Native 0.77 |
| Navigasi antar layar | React Navigation (native stack) |
| Konfigurasi alamat server | `react-native-dotenv` (impor lewat `@env`) |
| Backend (server) | Node.js + Express |
| ORM (penghubung ke database) | Prisma |
| Database | MySQL (lewat XAMPP) |

---

## 3. Cara Kerja Singkat

Aplikasi tidak menyimpan data sendiri. Datanya disimpan di database MySQL, dan aplikasi mengaksesnya lewat server Express.

```
Aplikasi React Native  --HTTP-->  Express (port 3000)  --Prisma-->  MySQL (port 3306)
 (emulator Android)               (repo backend-express)             (XAMPP)
```

Artinya, **tiga hal harus hidup bersamaan** supaya aplikasi bisa menampilkan data: **MySQL**, **backend Express**, dan **aplikasi** (beserta Metro, server pengembangan React Native). Kalau salah satu mati, aplikasi akan terlihat "tidak bereaksi" atau loading terus.

---

## 4. Persiapan Awal (Prasyarat)

Pasang semua alat berikut dulu. Setelah memasang, **tutup dan buka lagi CMD** supaya perubahan terbaca.

| Alat | Fungsi | Cek apakah sudah terpasang |
|---|---|---|
| **Node.js** versi 18 atau lebih baru | Menjalankan JavaScript di luar browser, dan `npm` | `node -v` dan `npm -v` |
| **JDK 17** (misalnya Microsoft OpenJDK 17) | Dibutuhkan Gradle untuk membangun aplikasi Android | `java -version` |
| **Android Studio** | Berisi Android SDK dan pembuat emulator | Buka Android Studio |
| **XAMPP** | Menyediakan server database MySQL | Buka XAMPP Control Panel |
| **Git** | Mengunduh (clone) project dari GitHub | `git --version` |
| **Postman** atau Thunder Client (opsional) | Menguji API secara manual | |

Tips: gunakan **CMD biasa** (bukan "Run as administrator") untuk menjalankan aplikasi. Mencampur CMD admin dan CMD biasa pada project yang sama bisa menimbulkan error izin (lihat bagian Troubleshooting).

---

## 5. Langkah 1: Siapkan Android SDK dan Emulator

### 1.1 Pasang komponen SDK di Android Studio

1. Buka **Android Studio**, lalu masuk ke **More Actions → SDK Manager** (atau **Settings → Languages & Frameworks → Android SDK**).
2. Di tab **SDK Platforms**, pastikan **Android SDK Platform 35** terpasang.
3. Di tab **SDK Tools**, pastikan **Android SDK Platform-Tools**, **Android Emulator**, dan **Android SDK Build-Tools** terpasang.
4. Catat **lokasi Android SDK** yang tertulis di bagian atas jendela itu. Biasanya `C:\Users\<NamaKamu>\AppData\Local\Android\Sdk`.

Saat pertama kali membangun aplikasi, Gradle akan mengunduh komponen tambahan (NDK, CMake) secara otomatis. Itu butuh koneksi internet.

### 1.2 Atur environment variable (supaya CMD mengenali `adb` dan `emulator`)

1. Tekan `Win + R`, ketik `sysdm.cpl`, lalu Enter.
2. Buka tab **Advanced**, klik **Environment Variables**.
3. Di **User variables**, klik **New**:
   - Variable name: `ANDROID_HOME`
   - Variable value: lokasi Android SDK dari langkah 1.1, misalnya `C:\Users\<NamaKamu>\AppData\Local\Android\Sdk`
4. Pilih variabel **Path**, klik **Edit**, lalu tambahkan dua baris:
   ```
   %ANDROID_HOME%\platform-tools
   %ANDROID_HOME%\emulator
   ```
5. Klik OK di semua jendela, lalu **tutup semua CMD** dan buka CMD baru.

> Ketik nilainya secara manual dan pastikan tidak ada spasi atau baris kosong di ujungnya. Karakter tak terlihat di ujung path bisa membuat Android Studio gagal terbuka.

Pastikan berhasil:

```cmd
adb --version
emulator -list-avds
```

`adb --version` harus menampilkan versi. `emulator -list-avds` akan menampilkan daftar emulator yang sudah dibuat (kosong kalau belum ada, dan itu normal di tahap ini).

### 1.3 Buat emulator (perangkat virtual)

1. Di Android Studio, buka **Device Manager**.
2. Klik **Create Device**, pilih **Pixel 4** (atau perangkat lain), lalu **Next**.
3. Pilih **System Image** yang memiliki **Google APIs** atau **Google Play** (API 35 atau lebih baru), unduh kalau perlu, lalu **Finish**.
4. Catat **nama emulator**-nya (misalnya `Pixel_4`). Cek lagi dengan `emulator -list-avds`.

---

## 6. Langkah 2: Siapkan dan Jalankan Backend

Backend ada di repository terpisah:

**https://github.com/USERNAME/NAMA-REPO-BACKEND** (ganti dengan alamat repo backend)

Panduan lengkapnya ada di README repo tersebut. Ringkasannya:

1. **Nyalakan MySQL.** Buka **XAMPP Control Panel**, klik **Start** di baris **MySQL** sampai berwarna hijau. Bawaan XAMPP: user `root` dan password kosong.
2. **Unduh dan pasang backend:**
   ```cmd
   git clone https://github.com/USERNAME/NAMA-REPO-BACKEND.git backend-express
   cd backend-express
   npm install
   ```
3. **Atur file `.env` backend.** Salin `.env.example` menjadi `.env`, lalu isi `DATABASE_URL` sesuai database kamu. Contoh untuk XAMPP bawaan:
   ```
   DATABASE_URL="mysql://root:@localhost:3306/nama_database"
   ```
4. **Buat tabel di database** dengan migrasi Prisma:
   ```cmd
   npx prisma migrate dev
   ```
5. **Jalankan server:**
   ```cmd
   node index.js
   ```
   Kalau berhasil, terminal menampilkan `Server started on port 3000`. Terminal ini **tidak akan kembali ke prompt**, itu normal karena server sedang berjalan. Biarkan terbuka.

Catatan: backend ini dijalankan dengan `node index.js` dan tidak restart otomatis. Setelah mengubah kode backend, hentikan dengan `Ctrl + C` lalu jalankan lagi.

**Uji backend** dengan membuka `http://localhost:3000/api/posts` di browser. Hasil yang benar berupa JSON dengan data post (boleh kosong). Kalau muncul `{"success":false,"message":"Internal server error"}`, lihat terminal backend dan bagian Troubleshooting.

---

## 7. Langkah 3: Ambil Project Aplikasi

**Penting:** taruh project di folder dengan **path yang pendek**, misalnya `C:\rn\app`. Windows punya batas panjang path sekitar 260 karakter, dan path yang terlalu panjang bisa membuat proses build Android gagal dengan error aneh.

```cmd
mkdir C:\rn
cd /d C:\rn
git clone https://github.com/muhammadiqbal2406697/react-native-posts-app.git app
cd app
npm install
```

`npm install` mengunduh semua library yang dibutuhkan ke folder `node_modules`. Prosesnya bisa beberapa menit.

---

## 8. Langkah 4: Atur File .env

File `.env` berisi **alamat server backend** yang dituju aplikasi. File ini tidak ikut diunggah ke GitHub, jadi kamu harus membuatnya sendiri dari contoh.

```cmd
copy .env.example .env
```

### Cari IP laptop kamu

Di emulator atau HP, kata `localhost` berarti **perangkat itu sendiri**, bukan laptopmu. Karena itu aplikasi harus memakai **alamat IP laptop** di jaringan Wi-Fi. Cari dengan:

```cmd
ipconfig
```

Cari bagian **Wireless LAN adapter Wi-Fi** (atau **Ethernet** kalau memakai kabel), lalu lihat baris **IPv4 Address**. Kalau ada banyak adapter, pilih yang **Default Gateway**-nya terisi. Abaikan adapter virtual (VirtualBox, WSL, VPN) yang gateway-nya kosong.

### Isi `.env`

Buka file `.env` dengan editor dan isi (ganti dengan IP milikmu):

```
BACKEND_API_URL=http://192.168.1.15:3000
```

Aturan penulisan:
- Tanpa tanda kutip.
- Tanpa spasi di sekitar `=`.
- Angka `3000` harus sama dengan port backend.
- Kalau memakai **HP fisik**, HP harus tersambung ke **Wi-Fi yang sama** dengan laptop.

> IP dari Wi-Fi **bisa berubah** (pindah jaringan, laptop tidur, Wi-Fi menyambung ulang). Kalau aplikasi tiba-tiba tidak bisa mengambil data, cek `ipconfig` lagi.
>
> Setelah mengubah `.env`, hentikan Metro dengan `Ctrl + C`, lalu jalankan `npx react-native start --reset-cache` supaya nilai baru terbaca.

### Izinkan Node.js di Firewall

Saat backend pertama kali dijalankan, Windows mungkin menampilkan jendela firewall untuk **Node.js**. Pilih **Private networks**, lalu **Allow access**. Pastikan juga jaringan Wi-Fi bertipe **Private** (Settings → Network and internet → Wi-Fi → pilih jaringan → Network profile type).

---

## 9. Langkah 5: Jalankan Aplikasi

Urutan menyalakan **penting**: database dulu, lalu backend, baru aplikasi.

### 5.1 Pastikan MySQL dan backend sudah hidup

- XAMPP: tombol **MySQL** berwarna hijau.
- Terminal backend: `Server started on port 3000`.
- Browser: `http://localhost:3000/api/posts` menampilkan JSON.

### 5.2 Nyalakan emulator

Cara termudah lewat CMD (ganti `Pixel_4` dengan nama emulatormu dari `emulator -list-avds`):

```cmd
start "" emulator -avd Pixel_4
```

Tunggu sampai layar Home Android muncul, lalu cek:

```cmd
adb devices
```

Harus muncul baris seperti `emulator-5554   device`. Kalau tertulis `offline`, tunggu beberapa detik lalu cek lagi.

### 5.3 Jalankan aplikasi

Di folder project aplikasi (`C:\rn\app`), pada **CMD biasa**:

```cmd
npm run android
```

Yang terjadi:
1. Gradle membangun aplikasi Android. **Build pertama bisa 5 sampai 10 menit**, karena mengunduh komponen SDK. Build berikutnya jauh lebih cepat.
2. Jendela baru untuk **Metro** (server pengembangan React Native) akan terbuka. **Jangan ditutup** selama kamu mengembangkan aplikasi.
3. Aplikasi terpasang dan terbuka di emulator.

Kalau layar daftar post muncul, selamat, semuanya sudah berjalan.

---

## 10. Cara Memakai Aplikasi

- **Melihat daftar:** layar utama menampilkan semua post (gambar dan judul).
- **Menambah post:** tekan tombol **+** merah di kanan bawah, isi judul, tekan **Choose Image** untuk memilih gambar, lalu simpan.
- **Mengubah post:** tekan **Edit** pada post yang diinginkan.
- **Menghapus post:** tekan **Delete**.

**Emulator tidak punya foto?** Galeri emulator baru memang kosong. Cara mengisinya:
- Buka aplikasi **Camera** di emulator dan ambil satu foto. Hasilnya langsung muncul di galeri.
- Atau buka **Chrome** di emulator, tekan lama pada sebuah gambar, lalu pilih **Download image**.

---

## 11. Struktur Folder

```
react-native-posts-app/
├── android/                 Project native Android (dibuat oleh React Native)
├── ios/                     Project native iOS (belum diuji)
├── src/
│   ├── navigations/
│   │   └── index.jsx        Pengaturan perpindahan antar layar
│   ├── screens/
│   │   └── posts/
│   │       ├── Index.jsx    Layar daftar post
│   │       ├── Create.jsx   Layar tambah post
│   │       └── Edit.jsx     Layar ubah post
│   ├── services/
│   │   └── api.jsx          Pengaturan koneksi ke backend
│   ├── styles/
│   │   └── index.jsx        Gaya tampilan bersama
│   └── utils/
│       └── handleErrors.js  Penanganan pesan error
├── App.tsx                  Komponen utama aplikasi
├── index.js                 Titik masuk aplikasi
├── babel.config.js          Konfigurasi Babel (termasuk plugin react-native-dotenv)
├── package.json             Daftar library dan perintah
├── .env.example             Contoh isi .env (aman diunggah)
└── .env                     Alamat backend milikmu (TIDAK diunggah ke GitHub)
```

---

## 12. Rutinitas Harian (Checklist)

Setiap mulai belajar atau mengembangkan, nyalakan **berurutan**:

1. **MySQL**: XAMPP Control Panel → Start (hijau).
2. **Backend**: terminal di folder backend → `node index.js`.
3. **Emulator**: `start "" emulator -avd Pixel_4` lalu cek `adb devices`.
4. **Aplikasi**: terminal di folder aplikasi → `npm run android`.

Saat selesai: tekan `Ctrl + C` di terminal Metro dan backend, lalu **Stop** MySQL di XAMPP. Jangan langsung mematikan laptop saat MySQL masih jalan, karena bisa membuat database berhenti mendadak.

**Aturan emas untuk debugging:** kalau aplikasi terlihat "tidak bereaksi" atau loading terus, cek dulu apakah lapisan di bawahnya masih hidup: **MySQL, lalu backend, lalu Metro**, sebelum mencurigai kode.

---

## 13. Troubleshooting (Kalau Ada Error)

Cara membaca error: cari **pesan paling spesifik** (biasanya di baris paling atas atau yang berisi kata `error`/`Error`), jangan terpaku pada daftar panjang jejak kode (stack trace).

### `'adb' is not recognized as an internal or external command`
- **Penyebab:** folder `platform-tools` belum ada di `Path`.
- **Solusi:** ikuti bagian [1.2](#12-atur-environment-variable-supaya-cmd-mengenali-adb-dan-emulator), lalu buka CMD baru.

### `emulator -list-avds` kosong, atau `No emulators found`
- **Penyebab:** emulator tersimpan di folder lain, atau ada variabel `ANDROID_AVD_HOME` lama yang menunjuk ke folder yang salah.
- **Solusi:** periksa `echo %ANDROID_AVD_HOME%` dan `echo %ANDROID_USER_HOME%`. Hapus `ANDROID_AVD_HOME` kalau tidak diperlukan, atau arahkan ke folder `.android\avd` yang berisi file `.ini` emulatormu.

### Gradle: `Could not move temporary workspace`
- **Penyebab:** path project terlalu panjang (batas Windows sekitar 260 karakter).
- **Solusi:** pindahkan project ke path pendek (misalnya `C:\rn\app`), hapus folder `android\.gradle`, lalu `npm run android` lagi. Opsional: aktifkan Long Paths lewat PowerShell sebagai administrator:
  ```powershell
  New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
  ```
  Lalu restart Windows.

### `Access is denied` atau `EPERM` saat build atau menjalankan
- **Penyebab:** sebagian file dibuat oleh sesi **Administrator**, lalu dipakai sesi biasa (atau sebaliknya).
- **Solusi:** jalankan semua perintah project di **CMD biasa**. Untuk memperbaiki izin, buka CMD **Run as administrator** satu kali:
  ```cmd
  icacls C:\rn /grant "%USERNAME%:(OI)(CI)F" /T
  ```
  Setelah itu tutup CMD admin dan kembali ke CMD biasa.

### `EADDRINUSE: address already in use :::8081`
- **Penyebab:** Metro lama masih berjalan di port 8081.
- **Solusi:** tutup jendela Metro lama dengan `Ctrl + C`. Kalau jendelanya tidak ketemu:
  ```cmd
  netstat -ano | findstr :8081
  taskkill /PID <angka_PID> /F
  ```
  Angka PID ada di kolom paling kanan pada baris `LISTENING`.

### Layar merah: `Unable to resolve module @env`
- **Penyebab:** plugin `react-native-dotenv` belum aktif, atau cache Metro masih lama.
- **Solusi:**
  1. Pastikan library terpasang: `npm ls react-native-dotenv`.
  2. Pastikan `babel.config.js` berisi plugin `'module:react-native-dotenv'`.
  3. Hentikan Metro, lalu `npx react-native start --reset-cache`, dan muat ulang aplikasi.

### Layar merah: `Unable to resolve module ../screens/...`
- **Penyebab:** nama atau lokasi file tidak cocok dengan `import` (termasuk huruf besar dan kecil).
- **Solusi:** cek struktur folder dengan `dir /s /b src\screens` dan cocokkan huruf per huruf dengan baris `import`.

### Loading terus, atau tambah/edit/hapus tidak bereaksi
- **Penyebab paling umum:** **MySQL mati sendiri** atau backend bermasalah. Aplikasi tidak menampilkan error karena permintaan ke server menggantung.
- **Solusi:**
  1. XAMPP: pastikan **MySQL** hijau (Start kalau belum).
  2. Buka `http://IP-LAPTOP:3000/api/posts` di browser. Kalau muncul `Internal server error`, lihat terminal backend. Pesan `Can't reach database server at localhost:3306` berarti MySQL belum menyala.
  3. Muat ulang aplikasi (tekan `r` di jendela Metro).

### `Network request failed`
- **Penyebab:** IP laptop berubah, backend mati, atau firewall memblokir.
- **Solusi:**
  1. Cek IP terbaru dengan `ipconfig`, perbarui `.env`, lalu restart Metro dengan `--reset-cache`.
  2. Pastikan backend hidup dan Node.js diizinkan di firewall (jaringan **Private**).
  3. Uji dari **Chrome di emulator**: buka `http://IP-LAPTOP:3000`. Kalau tidak terbuka, emulator tidak bisa menjangkau laptop.
  4. Catatan: alamat khusus emulator `10.0.2.2` tidak selalu bisa dipakai tergantung pengaturan firewall. Pakai IP Wi-Fi laptop.

### Galeri emulator kosong saat memilih gambar
- **Solusi:** ambil foto dengan aplikasi **Camera** emulator, atau unduh gambar dari **Chrome** di emulator (lihat bagian [Cara Memakai Aplikasi](#10-cara-memakai-aplikasi)). Memindahkan file lewat drag and drop kadang tidak terdaftar di galeri.

### Menekan `R` dua kali tidak me-refresh aplikasi
- **Penyebab:** ketikan `R` terbaca sebagai teks biasa, bukan pintasan.
- **Solusi:** di jendela Metro tekan `r` satu kali. Atau di emulator tekan `Ctrl + M`, lalu pilih **Reload**. Cara lain: `adb shell input keyevent 82` untuk membuka menu developer.

### Emulator ikut mati saat Android Studio ditutup
- **Solusi:** nyalakan emulator lewat CMD (`start "" emulator -avd Pixel_4`) supaya berjalan sendiri.

### Peringatan merah `Function components cannot be given refs`
- **Penyebab:** ketidakcocokan versi antar library navigasi. Hanya muncul di mode pengembangan.
- **Solusi:** tekan **Dismiss**. Tidak mengganggu fungsi aplikasi.

### Dialog `Android App Compatibility ... 16 KB`
- **Penyebab:** emulator memakai system image dengan ukuran memory page 16 KB, sedangkan sebagian library native belum mendukungnya.
- **Solusi:** tekan **OK** atau **Don't Show Again**. Aplikasi tetap berjalan normal.

### Backend: `Missing script: "dev"`
- **Penyebab:** backend tidak punya script `dev`.
- **Solusi:** jalankan dengan `node index.js`.

---

## 14. Catatan Git dan Keamanan

- **Jangan pernah meng-commit file `.env`.** File ini sudah masuk `.gitignore`. Gunakan `.env.example` sebagai contoh yang aman.
- Sebelum `commit`, selalu jalankan `git status` dan pastikan tidak ada `.env` atau `node_modules` di daftar file yang akan masuk.
- Alur menyimpan perubahan ke GitHub:
  ```cmd
  git add .
  git status
  git commit -m "deskripsi singkat perubahan"
  git push
  ```
- Kalau muncul `Author identity unknown`, atur identitas satu kali:
  ```cmd
  git config --global user.name "Nama Kamu"
  git config --global user.email "emailkamu@contoh.com"
  ```

---

## 15. Ide Pengembangan Lanjutan

- Merapikan penanganan error (`try`, `catch`, `finally`) dan menampilkan pesan yang jelas saat server tidak bisa dihubungi.
- Memperbarui daftar post otomatis setiap layar kembali mendapat fokus (`useFocusEffect`).
- Menambah validasi form (judul wajib diisi, batas ukuran gambar).
- Menambah dialog konfirmasi sebelum menghapus post.
- Menambah fitur pencarian, pagination, atau login pengguna.
- Mengisi alamat server lewat satu tempat konfigurasi supaya mudah berganti antara emulator, HP fisik, dan server asli.

---

## Penulis

Dibuat oleh saya sendiri (Muhammad Iqbal Al-Ayyasy) A.k.A Thuatha sebagai bahan belajar Mobile Programming, Pendidikan Ilmu Komputer, Universitas Pendidikan Indonesia.
