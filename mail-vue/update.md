# Ringkasan Pembaruan UI Frontend (Clashmate.id Mail)

Dokumen ini mencatat seluruh file yang disesuaikan di dalam folder `src/` untuk mengubah antarmuka menjadi **Clashmate.id Mail** dengan desain modern, layar penuh (*fullscreen*), dan **tanpa data dummy** (100% menggunakan data asli dari server/API).

---

## Rincian Penyesuaian Terakhir

1. **Layar Penuh (100% Fullscreen & Responsive)**:
   - Menghapus batasan padding/margin dan border frame desktop luar sehingga aplikasi langsung mengisi 100% tinggi dan lebar layar peramban (*edge-to-edge*).
   - Sidebar kiri, kolom daftar email, dan panel pembaca email memanjang penuh 100vh tanpa sisa ruang kosong.

2. **Pembersihan Data Dummy (100% Data Asli)**:
   - Menghapus semua sampel data dummy (*Dribbble, Cyprode, Google, Netflix, Twitter, dll.*) dari `src/components/email-scroll/index.vue`.
   - Menghapus data email tiruan dari `src/views/content/index.vue`. Jika belum ada email yang dipilih, panel kanan menampilkan status kosong (*empty state*) yang bersih.
   - Folder **Inbox**, **Sent**, **Drafts**, dan **Starred** kini hanya menampilkan email asli dari akun pengguna.
   - Menghapus angka email tiruan (*"128 Mails"*); jumlah email kini hanya tampil dinamis sesuai data asli.

3. **Struktur Navigasi Bersih (Tanpa Fitur Tambahan yang Tidak Perlu)**:
   - Menghapus menu tambahan *"Calendar"* dan *"Plans"*.
   - Mengembalikan susunan menu asli web mail:
     - **Mail**: Inbox, Sent Mails, Drafts, Favourites (Starred), Settings.
     - **Manage** (khusus admin): Analytics, Users, All Mail, Permissions, Invite Code, System Settings.

4. **Identitas Brand Baru**:
   - Mengganti nama brand default menjadi **"Clashmate.id Mail"** pada sidebar, header, dan halaman login.
   - Menggunakan logo resmi Clashmate di `src/assets/logo.png`.

---

## Daftar File yang Diubah & Lokasinya

| No | Lokasi File | Keterangan Perubahan |
|---|---|---|
| 1 | `src/assets/logo.png` | Aset logo Clashmate. |
| 2 | `src/style.css` | Styling global tema terang modern macOS, warna aksen biru `#1d68ff`, background bersih `#ffffff`, dan font antialiased. |
| 3 | `src/layout/index.vue` | Layout 100vw x 100vh penuh (*edge-to-edge*) tanpa padding pembatas. |
| 4 | `src/layout/aside/index.vue` | Sidebar kiri full height: tombol titik jendela Mac (merah, kuning, hijau), logo Clashmate + nama "Clashmate.id Mail", tombol "+ New Mail", menu kategori asli tanpa Calendar/Plans. |
| 5 | `src/layout/header/index.vue` | Header atas modern dengan judul dinamis asli, sub-teks jumlah email asli (tanpa angka dummy), dan bilah pencarian kapsul (*pill shape*). |
| 6 | `src/components/email-search/index.vue` | Bilah pencarian kapsul modern dengan placeholder *"Search..."* dan ikon kaca pembesar. |
| 7 | `src/components/email-scroll/index.vue` | Tampilan kartu mengambang (*floating cards*) modern untuk email asli, tanpa data dummy. Menampilkan indikator belum dibaca, format waktu relatif, subjek tebal, dan cuplikan teks. |
| 8 | `src/views/email/index.vue` | Split-view desktop 3 kolom (Sidebar \| List Kartu Email Asli \| Reader Pane). |
| 9 | `src/views/content/index.vue` | Kolom pembaca email modern dengan avatar pengirim, info pengirim asli, dan format pesan yang rapi tanpa data dummy. |
| 10 | `src/views/login/index.vue` | Tampilan logo Clashmate dan nama brand "Clashmate.id Mail" pada form login. |
| 11 | `src/layout/main/index.vue` | Penyesuaian container konten agar mengisi 100% tinggi secara mulus. |
| 12 | `src/utils/day.js` | Format waktu relatif hingga 24 jam (*"3 hours ago"*). |
