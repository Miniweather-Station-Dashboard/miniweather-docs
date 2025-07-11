# Installation Guide

Dokumen ini menjelaskan proses instalasi ketiga komponen utama sistem Miniweather Station Dashboard, yaitu:

1. **Admin Panel**
2. **Public Dashboard**
3. **Backend API**

> ⚠️ Pastikan kamu telah menginstal `Docker` dan `Docker Compose` sebelum melanjutkan.

---

## 📁 Struktur Direktori Utama

Misalnya kamu memiliki struktur direktori sebagai berikut:

```
miniweather/
├── admin-panel/
├── public-dashboard/
├── backend-api/
├── docker-compose.yml
```

---

## 1. 🚀 Backend API

Backend bertanggung jawab atas:

- Autentikasi
- Penyimpanan data (PostgreSQL / ScyllaDB)
- Integrasi MQTT & Redis
- API untuk dashboard dan admin panel

### 📦 Langkah Instalasi

```bash
cd backend-api

# Install dependensi
npm install

# Copy konfigurasi
cp .env.example .env

# Jalankan development
npm run dev
```

> Pastikan `.env` berisi konfigurasi `DATABASE_URL`, `MQTT_BROKER_URL`, `REDIS_URL`, dll.

### 🔧 Jika Menggunakan Docker

Tambahkan ke `docker-compose.yml`:

```yaml
backend:
  build: ./backend-api
  ports:
    - "3001:3001"
  env_file:
    - ./backend-api/.env
  depends_on:
    - postgres
    - redis
    - mqtt
```

---

## 2. 🧑‍💼 Admin Panel

Admin Panel digunakan untuk mengelola perangkat, sensor, user, artikel, dan peringatan.

### 📦 Langkah Instalasi

```bash
cd admin-panel

# Install dependensi
npm install

# Copy konfigurasi
cp .env.example .env.local

# Jalankan di localhost
npm run dev
```

### 🐳 Docker Compose

```yaml
admin-panel:
  build: ./admin-panel
  ports:
    - "3000:3000"
  env_file:
    - ./admin-panel/.env.local
  depends_on:
    - backend
```

---

## 3. 🌐 Public Dashboard

Dashboard publik menampilkan data sensor secara real-time dan informasi peringatan/cuaca.

### 📦 Langkah Instalasi

```bash
cd public-dashboard

# Install dependensi
npm install

# Copy file konfigurasi
cp .env.example .env.local

# Jalankan
npm run dev
```

### 🐳 Docker Compose

```yaml
public-dashboard:
  build: ./public-dashboard
  ports:
    - "3002:3000"
  env_file:
    - ./public-dashboard/.env.local
  depends_on:
    - backend
```

---

## ⚙️ Jalankan Seluruh Sistem dengan Docker Compose

### 🐳 Contoh `docker-compose.yml`

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: miniweather
    volumes:
      - pgdata:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  mqtt:
    image: emqx/emqx:latest
    ports:
      - "1883:1883"
      - "18083:18083"

  backend:
    build: ./backend-api
    ports:
      - "3001:3001"
    env_file:
      - ./backend-api/.env
    depends_on:
      - postgres
      - redis
      - mqtt

  admin-panel:
    build: ./admin-panel
    ports:
      - "3000:3000"
    env_file:
      - ./admin-panel/.env.local
    depends_on:
      - backend

  public-dashboard:
    build: ./public-dashboard
    ports:
      - "3002:3000"
    env_file:
      - ./public-dashboard/.env.local
    depends_on:
      - backend

volumes:
  pgdata:
```

### ▶ Jalankan Semua

```bash
docker compose up --build -d
```

---

## ✅ Verifikasi

| Komponen         | URL Akses Lokal              |
| ---------------- | ---------------------------- |
| Admin Panel      | `http://localhost:3000`      |
| Public Dashboard | `http://localhost:3002`      |
| Backend API      | `http://localhost:3001/api/` |
| MQTT Dashboard   | `http://localhost:18083`     |

---

Dengan mengikuti langkah-langkah di atas, kamu dapat menjalankan sistem Miniweather Station secara lengkap di lingkungan lokal menggunakan Docker Compose.
