# Introduction

Miniweather Station Dashboard adalah sistem pemantauan cuaca berbasis IoT yang memungkinkan pengguna untuk melihat data sensor secara real-time dari berbagai lokasi. Sistem ini dirancang untuk mendukung pemantauan lingkungan secara efisien dan terstruktur, serta dapat diskalakan sesuai kebutuhan.

---

## 🎯 Tujuan

Dokumentasi ini bertujuan untuk memberikan panduan lengkap bagi administrator dan pengguna teknis dalam:

- Mengelola perangkat IoT (onboarding, konfigurasi)
- Menambahkan dan mengelola tipe sensor
- Mengelola data dan visualisasi
- Menjalankan proses monitoring dan validasi data
- Menggunakan fitur-fitur admin panel secara optimal

---

## ⚙️ Komponen Utama Sistem

1. **Admin Panel**  
   Antarmuka untuk mengelola perangkat, sensor, artikel, dan pengguna.

2. **Public Dashboard**  
   Menampilkan data cuaca secara real-time ke publik.

3. **Backend API**  
   Menangani autentikasi, penyimpanan data, integrasi MQTT, dan pengolahan data.

4. **MQTT Broker**  
   Menjadi jalur komunikasi antara device IoT dan backend.

5. **Redis & Hyperbase**  
   Redis digunakan untuk caching data jika Hyperbase offline, sementara Hyperbase menyimpan data secara permanen.

---

## 🧭 Struktur Dokumentasi

Dokumentasi ini terbagi ke dalam beberapa bagian penting:

- **Installation**: Panduan instalasi dan setup awal
- **Onboarding Device**: Proses menambahkan perangkat ke sistem
- **Manage Alerts, Articles & Users**: Manajemen konten dashboard dan admin
- **API Reference**: Detail teknis endpoint

---

Dengan mengikuti dokumentasi ini, kamu akan dapat mengoperasikan dan mengembangkan sistem Miniweather Station dengan lebih efektif dan efisien.
