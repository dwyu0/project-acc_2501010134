# Akuisisi dan Analisis Data Akomodasi Kecamatan Kintamani

Repositori ini berisi kode dan dokumentasi untuk memenuhi Tugas Praktik Mata Kuliah **BIFP-243 Akuisisi dan Manajemen Data** di **INSTIKI** (Institut Bisnis dan Teknologi Indonesia).

---

## 🎯 1. Tujuan Proyek
* **Mengakuisisi Data:** Mengambil data fasilitas akomodasi pariwisata (hotel, guest house, hostel, motel, apartment, chalet) di Kecamatan Kintamani secara otomatis dan etis dari API publik.
* **Transformasi & Dokumentasi:** Menyimpan respon mentah JSON dan mengubahnya menjadi dataset tabular terstruktur (`akomodasi_kintamani.csv`) yang dilengkapi metadata lengkap (*provenance* & *data dictionary*).
* **Evaluasi Kelengkapan:** Membandingkan hasil akuisisi OpenStreetMap dengan data resmi Badan Pusat Statistik (BPS) Kabupaten Bangli serta melakukan verifikasi sampel manual.

---

## 🌐 2. Sumber Data
* **Sumber Utama:** OpenStreetMap (OSM) via Overpass API (`admin_level=6` Kecamatan Kintamani).
* **Endpoint API:** `https://overpass-api.de/api/interpreter`
* **Sumber Pembanding:** Data Resmi BPS Kabupaten Bangli (*Kecamatan Kintamani Dalam Angka*).

---

## 📅 3. Tanggal Akses
* **Tanggal Akuisisi Data:** 10 November 2026

---

## 🛠️ 4. Metode Akuisisi & Pengolahan
* **Wilayah Administratif:** Menggunakan batas wilayah administratif Kecamatan Kintamani (`admin_level=6`).
* **Query Overpass:** Mengakses node, way, dan relation dengan tag `tourism` yang mencakup `hotel`, `guest_house`, `hostel`, `motel`, `apartment`, dan `chalet`.
* **Ketahanan API (Retry Mechanism):** Menerapkan fungsi `ambil_overpass()` dengan *exponential backoff* dan jeda waktu untuk menangani kemungkinan *HTTP 429* (Too Many Requests) atau *HTTP 504* (Gateway Timeout).
* **Flattening:** Mengubah struktur JSON bersarang menjadi format CSV tabular, mengambil koordinat titik tengah (*center*) untuk objek tipe *way/relation*.

---

## ⚖️ 5. Batasan Etika & Minimisasi Data
* **Prinsip Minimisasi Data:** Data kontak pribadi seperti nomor telepon (`phone`, `contact:phone`), email (`email`), dan media sosial sengaja **tidak diambil** dari tag OpenStreetMap untuk menjaga privasi.
* **Toleransi Beban Server:** Pengambilan data dilakukan secara wajar tanpa eksekusi berulang dalam *loop* cepat serta menyertakan *User-Agent* teridentifikasi (`INSTIKI-BIFP243-Praktikum/1.0`).
* **Kepatuhan Sumber:** Tidak melakukan *scraping* pada platform OTA (Booking.com, Agoda, Traveloka) maupun Google Maps yang melarang pengambil data otomatis.

---

## 📜 6. Atribusi Data
Data spasial yang digunakan dalam proyek ini bersumber dari **© OpenStreetMap contributors** di bawah lisensi Open Database License (ODbL).

---

## 🔄 7. Cara Menjalankan Ulang (Reproducibility)
1. Buka Google Colab dan pastikan pustaka `pandas`, `requests`, `numpy`, dan `json` sudah terpasang.
2. Buka notebook `notebooks/akuisisi_akomodasi.ipynb`.
3. Jalankan semua sel kode secara berurutan dari sel 1 hingga akhir (*Runtime > Run all*).
4. Hasil akuisisi mentah akan disimpan secara otomatis ke folder `data/raw/` dan dataset tabular ke `data/interim/akomodasi_kintamani.csv`.
