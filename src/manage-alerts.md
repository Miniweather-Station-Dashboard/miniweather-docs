# Manage Warnings

Halaman ini menjelaskan cara menambahkan dan mengelola peringatan dini (warning) pada sistem Miniweather Station Dashboard. Warning yang ditambahkan oleh admin akan ditampilkan di halaman publik untuk memberikan informasi penting kepada pengguna.

---

## ✅ 1. Akses Halaman Warning Management

Setelah login sebagai admin, klik menu **Warning Management** di sidebar.

![Halaman Warning Management](images/warning/halaman_warning_mangement.png)

---

## ➕ 2. Tambahkan Warning Baru

Klik tombol **Create New Warning** untuk membuka form input warning.

![Form Tambah Warning](images/warning/tamabahkan_data_warning_pada_form.png)

---

### 📝 3. Isi Form Warning

Isi formulir dengan informasi berikut:

- **Message**: Isi pesan peringatan (misal: *Hati-hati akan potensi badai di sekitar Pantai Wohkudu*)
- **Type**: Pilih tipe peringatan dari daftar berikut:
  - General
  - Weather
  - Tsunami
  - Earthquake
  - Volcano
  - Flood
- **Status**: Centang “Active” untuk langsung menampilkan warning

![Contoh Isi Form Warning](images/warning/contoh_cara_mengisi_data_warning.png)

![Tipe-Tipe Warning](images/warning/tipe_tipe_warning.png)

---

## 💾 4. Simpan dan Verifikasi

Klik tombol **Create** untuk menyimpan peringatan. Setelah berhasil, warning akan muncul di tabel daftar seperti berikut:

![Warning Berhasil Ditambahkan](images/warning/warning_berhasil_ditambah.png)

---

## 🌐 5. Tampil di Dashboard Publik

Peringatan yang aktif akan langsung ditampilkan di bagian “Peringatan Dini” pada halaman dashboard publik.

![Tampilan di Dashboard Publik](images/warning/tampilan_warning_di_dashboard_publik.png)

---

## 🛠️ Catatan Teknis

- Warning disimpan di database melalui endpoint:
  
```
POST /v1/warnings
Authorization: Bearer <admin-token>
Content-Type: application/json

```


- Tipe warning digunakan untuk styling dan filtering pada frontend
- Hanya warning yang berstatus **active** yang akan ditampilkan di dashboard publik

---

Dengan fitur ini, admin dapat memberikan peringatan penting seperti cuaca ekstrem, gempa, atau banjir kepada pengguna secara cepat dan real-time.


