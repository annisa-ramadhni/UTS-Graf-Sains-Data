# Visualisasi Graf Similarity

Folder ini berisi dokumentasi visualisasi graf yang dihasilkan selama pengerjaan UTS Graf Sains Data menggunakan dataset Retailrocket Recommender System.

Visualisasi dibuat untuk membantu melihat hubungan kemiripan (*similarity*) antar pengguna dan antar item berdasarkan pola interaksi yang terdapat pada dataset.

## Daftar Gambar

### 1. Graph Before

File: `graph_before.png`

Gambar ini menampilkan visualisasi graf sebelum dilakukan penyesuaian tampilan. Visualisasi awal digunakan untuk melihat struktur hubungan antarnode yang terbentuk dari hasil analisis similarity.

### 2. Graph After

File: `graph_after.png`

Gambar ini menampilkan visualisasi graf setelah dilakukan penyesuaian agar hubungan antarnode lebih mudah diamati. Pada notebook, penyesuaian visualisasi dilakukan dengan memilih node tertentu dan menampilkan edge berdasarkan bobot similarity yang lebih tinggi.

## Tujuan Visualisasi

Visualisasi graf digunakan sebagai pendukung analisis untuk memahami pola kemiripan berdasarkan interaksi pengguna dengan item. Analisis mencakup dua jenis proyeksi, yaitu *user similarity* untuk melihat kemiripan antar pengguna dan *item similarity* untuk melihat kemiripan antar item.

Perbandingan visualisasi sebelum dan sesudah penyesuaian diharapkan dapat membantu pembaca mengamati struktur graf dengan lebih jelas. Visualisasi ini melengkapi hasil perhitungan cosine similarity yang dilakukan pada notebook utama.

## Sumber

Seluruh visualisasi berasal dari proses analisis dalam notebook `UTS_Graf_Sains_Data.ipynb` yang tersedia pada folder `notebooks` di repository ini.
