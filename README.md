# UTS Graf Sains Data — Analisis Interaksi E-Commerce Retailrocket

Repository ini berisi pengerjaan Ujian Tengah Semester (UTS) mata kuliah Graf Sains Data. Pada tugas ini, saya menggunakan **Retailrocket Recommender System Dataset** untuk mempelajari hubungan antara pengguna (*Visitor*) dan item (*Item*), menganalisis kemiripan berdasarkan pola interaksi, serta menerapkan metode K-Nearest Neighbors (KNN) untuk menghasilkan rekomendasi item.

Pengerjaan dilakukan menggunakan Python dan Neo4j. Selain membangun representasi graf, saya juga melakukan analisis *Vector Space Model* (VSM), perhitungan *cosine similarity*, visualisasi graf, dan evaluasi rekomendasi menggunakan Hit@5 serta NDCG@5.

## Tujuan

Tujuan dari pengerjaan ini adalah:

- Merepresentasikan interaksi antara pengguna dan item dalam bentuk graf menggunakan Neo4j.
- Membentuk Vector Space Model (VSM) untuk analisis User dan Item.
- Mengamati karakteristik serta proyeksi graf berdasarkan pola interaksi.
- Menghitung kemiripan antar pengguna dan antar item menggunakan cosine similarity.
- Menelusuri hubungan similarity menggunakan query Cypher.
- Menerapkan KNN untuk menghasilkan rekomendasi Top-5.
- Mengevaluasi hasil rekomendasi menggunakan Hit@5 dan NDCG@5.

## Dataset

Dataset yang digunakan adalah [Retailrocket Recommender System Dataset di Kaggle](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset).

Dataset ini berisi catatan aktivitas pengguna pada platform e-commerce. Beberapa file utama yang terdapat dalam dataset asli adalah:

| File | Keterangan |
|---|---|
| `events.csv` | Catatan interaksi pengguna terhadap item. |
| `category_tree.csv` | Informasi hierarki kategori. |
| `item_properties_part1.csv` | Bagian pertama data properti item. |
| `item_properties_part2.csv` | Bagian kedua data properti item. |

Dalam pengerjaan ini, data interaksi dibagi menjadi dua periode analisis, yaitu P1 dan P2. Periode P1 digunakan sebagai data dasar untuk membentuk matriks interaksi, sedangkan P2 digunakan untuk menguji hasil rekomendasi.

Hasil pemrosesan tersedia pada folder `data` dalam bentuk file ZIP yang berisi CSV:

- `events_P1.csv.zip`
- `events_P2.csv.zip`

Unduh file yang diperlukan, lalu ekstrak sebelum digunakan. Dataset asli dapat diunduh dari tautan Kaggle di atas.

## Metode dan Tahapan Pengerjaan

### 1. Representasi Graf di Neo4j

Data interaksi direpresentasikan menggunakan node `Visitor` dan `Item`. Hubungan antara keduanya dibentuk berdasarkan jenis aktivitas pengguna, yaitu:

- `VIEWED` — pengguna melihat item.
- `ADDED_TO_CART` — pengguna menambahkan item ke keranjang.
- `BOUGHT` — pengguna melakukan transaksi pembelian item.

Representasi ini digunakan untuk menyimpan dan menelusuri hubungan interaksi menggunakan Cypher pada Neo4j.

### 2. Vector Space Model (VSM)

Interaksi pengguna dan item diubah menjadi matriks Visitor × Item. Setiap jenis interaksi diberikan bobot untuk merepresentasikan tingkat aktivitas pengguna:

| Jenis interaksi | Bobot |
|---|---:|
| `VIEWED` | 1 |
| `ADDED_TO_CART` | 3 |
| `BOUGHT` | 5 |

Bobot dari interaksi kemudian digabungkan pada setiap pasangan pengguna dan item. Matriks dibentuk menggunakan sparse matrix dari SciPy untuk mengurangi kebutuhan memori karena sebagian besar pasangan pengguna dan item tidak memiliki interaksi.

### 3. User dan Item Similarity

Analisis similarity dilakukan dengan cosine similarity berdasarkan matriks interaksi.

- **User projection:** menganalisis kemiripan antar pengguna berdasarkan item yang mereka interaksikan.
- **Item projection:** menganalisis kemiripan antar item berdasarkan pola interaksi pengguna.

Untuk visualisasi, notebook memilih 100 pengguna dan 100 item dengan jumlah interaksi unik terbanyak. Hubungan similarity kemudian dibentuk sebagai graf menggunakan NetworkX.

Notebook juga memuat visualisasi dengan edge similarity terkuat dan tampilan graf yang lebih ringkas agar struktur hubungan dapat diamati dengan lebih jelas.

### 4. Pemeriksaan Similarity melalui Neo4j

Query Cypher digunakan untuk mengambil lima hubungan pengguna dengan nilai similarity tertinggi terhadap pengguna referensi yang dipilih.

Hasil query ini digunakan untuk memeriksa hubungan similarity yang tersimpan pada Neo4j, termasuk nilai similarity dan informasi metode serta periode jika tersedia pada relasinya.

### 5. Rekomendasi Menggunakan KNN

Metode K-Nearest Neighbors digunakan untuk mencari pengguna dengan pola interaksi yang mirip. Pencarian tetangga dilakukan berdasarkan cosine distance pada matriks interaksi periode P1.

Item yang telah berinteraksi dengan pengguna pada data dasar dikecualikan dari kandidat rekomendasi. Item dari pengguna tetangga kemudian diberi skor berdasarkan nilai kemiripan dan bobot interaksi, lalu diurutkan untuk menghasilkan rekomendasi Top-5.

Pengujian menggunakan data periode P2 dan dibatasi pada pengguna serta item yang memenuhi syarat evaluasi berdasarkan data yang tersedia.

### 6. Evaluasi Rekomendasi

Kualitas rekomendasi dievaluasi menggunakan dua metrik:

- **Hit@5:** mengukur apakah terdapat setidaknya satu item relevan di antara lima rekomendasi teratas.
- **NDCG@5:** mengukur kualitas urutan rekomendasi dengan mempertimbangkan posisi item relevan.

Evaluasi dilakukan pada sampel hingga 100 pengguna dengan proses pemilihan sampel yang dapat direproduksi menggunakan random seed.

## Hasil Evaluasi

Berdasarkan hasil yang tercatat pada notebook, evaluasi rekomendasi menghasilkan nilai berikut:

| Metrik | Hasil |
|---|---:|
| Jumlah pengguna yang dievaluasi | 100 |
| Rata-rata Hit@5 | 0,0100 |
| Rata-rata NDCG@5 | 0,0021 |

Nilai evaluasi menunjukkan bahwa performa rekomendasi pada pengujian ini masih rendah. Hasil tersebut menjadi bahan evaluasi bahwa kemiripan pola interaksi saja belum tentu menghasilkan rekomendasi yang relevan pada periode berikutnya.

Salah satu pengembangan yang dapat dilakukan adalah mengevaluasi kembali pemilihan tetangga, strategi pemberian skor item, dan pendekatan pembentukan rekomendasi.

## Struktur Repository

```text
UTS-Graf-Sains-Data/
├── README.md
├── data/
│   ├── README.md
│   ├── events_P1.csv.zip
│   └── events_P2.csv.zip
├── notebooks/
│   ├── README.md
│   └── UTS_Graf_Sains_Data.ipynb
└── screenshots/
    ├── README.md
    ├── graph_before.png
    └── graph_after.png
```

Keterangan:

- `data/` berisi dokumentasi dataset dan data hasil pemrosesan periode P1 dan P2.
- `notebooks/` berisi notebook utama beserta penjelasan tahapan analisis.
- `screenshots/` berisi visualisasi graf sebelum dan sesudah penyesuaian tampilan.

## Tools dan Library

Pengerjaan ini menggunakan:

- Python
- Pandas dan NumPy untuk pengolahan data.
- SciPy untuk sparse matrix.
- Scikit-learn untuk cosine similarity dan KNN.
- NetworkX dan Matplotlib untuk pembentukan serta visualisasi graf.
- Neo4j dan Cypher untuk penyimpanan serta penelusuran hubungan graf.

## Cara Menjalankan Notebook

1. Clone atau unduh repository ini ke komputer.
2. Unduh dataset Retailrocket dari [Kaggle](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset) jika diperlukan untuk pemrosesan ulang.
3. Ekstrak `events_P1.csv.zip` dan `events_P2.csv.zip` pada folder `data`.
4. Siapkan lingkungan Python dan instal library yang digunakan pada notebook.
5. Jalankan Neo4j dan pastikan database dapat diakses.
6. Sesuaikan konfigurasi `PROJECT_DIR` dan koneksi Neo4j dengan lingkungan komputer masing-masing.
7. Jalankan sel notebook secara berurutan. Masukkan password Neo4j saat diminta.

**Catatan:** notebook menggunakan konfigurasi direktori lokal dan koneksi Neo4j. Karena itu, beberapa pengaturan perlu disesuaikan sebelum notebook dijalankan di komputer lain. Password database tidak perlu dituliskan langsung di dalam notebook.

## Kesimpulan

Melalui pengerjaan UTS ini, saya mempelajari bagaimana data interaksi pengguna dan item dapat direpresentasikan sebagai graf serta diolah menjadi matriks interaksi untuk analisis similarity. Saya juga menerapkan KNN untuk menghasilkan rekomendasi item dan mengevaluasi hasilnya menggunakan Hit@5 serta NDCG@5.

Hasil evaluasi menunjukkan bahwa rekomendasi yang dihasilkan masih perlu dikembangkan. Dari proses ini, saya memahami bahwa pembentukan graf dan perhitungan similarity merupakan bagian dari proses analisis, sedangkan evaluasi tetap diperlukan untuk mengetahui apakah rekomendasi yang diberikan sudah relevan dengan kebutuhan pengguna.

Repository ini disusun sebagai dokumentasi proses pengerjaan, mulai dari data yang digunakan, tahapan analisis, visualisasi graf, hingga evaluasi sistem rekomendasi.

## Referensi

- Retailrocket Recommender System Dataset — https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset
