# Ringkasan Pembaruan UI Frontend (Clashmate Mail)

Dokumen ini mencatat seluruh file yang dibuat dan diubah di dalam folder `src/` untuk mengubah tampilan web mail agar menyerupai referensi antarmuka macOS desktop mail app.

---

## Daftar File yang Diubah & Lokasinya

| No | Lokasi File | Keterangan Pembaruan |
|---|---|---|
| 1 | `src/assets/logo.png` | **(File Baru)** Aset logo resmi Clashmate. |
| 2 | `src/style.css` | Penyesuaian tema global: palet warna root ke gaya macOS terang (*clean light theme*), warna aksen biru `#1d68ff`, kanvas desktop `#eaedf1`, dan font sistem antialiased. |
| 3 | `src/layout/index.vue` | Penyesuaian container aplikasi menjadi jendela macOS berlayar melengkung (*rounded window* dengan sudut `16px`), drop shadow halus, dan kanvas abu-abu. |
| 4 | `src/layout/aside/index.vue` | **Sidebar Kiri**: Kontrol titik jendela Mac (merah, kuning, hijau), logo Clashmate + nama brand, tombol pill biru cerah **"+ New Mail"**, kategori **Mail** (Inbox 349, Sent Mails 27, Drafts 3, Spam 136, Trash, Favourites 48) dan **Others** (Calendar, Plans), serta menu Manage admin. |
| 5 | `src/layout/header/index.vue` | **Top Header**: Menampilkan judul halaman (**"Inbox"**) dengan sub-teks jumlah email (**"128 Mails"**), integrasi bilah pencarian modern, dan menu avatar pengguna. |
| 6 | `src/components/email-search/index.vue` | **Bilah Pencarian**: Mengubah kotak pencarian menjadi bentuk kapsul (*pill shape*) dengan teks placeholder `"Search..."` di sebelah kiri dan ikon kaca pembesar di sebelah kanan. |
| 7 | `src/components/email-scroll/index.vue` | **Daftar Email (Floating Cards)**: Baris email diubah menjadi kartu mengambang dengan sudut melengkung `14px`, indikator titik biru unread (`•`), waktu relatif (*"3 hours ago"*), subjek tebal, cuplikan pesan 2 baris, dan status aktif biru lembut (`#e0efff`). Dilengkapi data sampel identik dengan referensi gambar (Dribbble, Cyprode, Google, Netflix, Twitter). |
| 8 | `src/views/email/index.vue` | **Desktop Split View**: Tata letak responsif 3 kolom berdampingan (*Sidebar \| Email List Cards \| Reader Pane*) pada layar desktop, serta transisi mulus pada layar ponsel/tablet. |
| 9 | `src/views/content/index.vue` | **Pembaca Email (Kolom Kanan)**: Judul subjek besar tebal (*"Welcome to Cyprode Mail App!"*), avatar pengirim bulat, informasi pengirim (*Team Cyprode <hello@cyprode.com>*), dan format isi pesan rapi dengan nomor list. Mendukung prop `isEmbedded` untuk split view. |
| 10 | `src/views/login/index.vue` | **Halaman Login**: Menampilkan logo Clashmate dan identitas brand di form autentikasi. |
| 11 | `src/utils/day.js` | **Format Waktu**: Menyesuaikan fungsi `fromNow()` agar menampilkan format jam relatif seperti *"3 hours ago"*. |

---

## Catatan Penting
* Seluruh perubahan kode **hanya dilakukan di dalam direktori `src/`**, tidak ada konfigurasi eksternal atau backend yang dimodifikasi.
* Jika ingin meng-update repositori Git yang sudah ada, cukup tambahkan folder `src/`:
  ```bash
  git add src/ update.md
  git commit -m "Update frontend UI to Clashmate modern design"
  git push
  ```

