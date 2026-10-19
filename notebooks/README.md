# Notebook Pengerjaan UTS Graf Sains Data

Folder ini berisi notebook Python yang digunakan untuk mengerjakan UTS Graf Sains Data dengan dataset Retailrocket Recommender System. Pengerjaan ini berfokus pada analisis interaksi pengguna dengan item, pembentukan graf kemiripan, serta penerapan metode K-Nearest Neighbors (KNN) untuk menghasilkan rekomendasi item.

## File Notebook

- `UTS_Graf_Sains_Data.ipynb` — notebook utama yang berisi proses pengolahan data, analisis, visualisasi, penerapan model rekomendasi, dan evaluasi hasil.

## Tujuan Pengerjaan

Tujuan dari pengerjaan ini adalah memahami bagaimana data interaksi pada platform e-commerce dapat diolah menjadi representasi yang mendukung analisis graf dan sistem rekomendasi. Hasil analisis digunakan untuk melihat kemiripan antar pengguna maupun antar item berdasarkan pola interaksi yang tercatat.

## Tahapan Pengerjaan

### 1. Persiapan Library dan Dataset

Notebook menggunakan beberapa library Python, seperti Pandas dan NumPy untuk pengolahan data, SciPy untuk membentuk matriks sparse, Matplotlib dan NetworkX untuk visualisasi graf, serta Scikit-learn untuk perhitungan cosine similarity dan penerapan KNN.

Dataset hasil pemrosesan dibagi menjadi dua periode, yaitu P1 dan P2. Periode P1 digunakan sebagai data dasar untuk membentuk matriks interaksi, sedangkan periode P2 digunakan sebagai data pengujian rekomendasi.

### 2. Koneksi dan Pengambilan Data dari Neo4j

Notebook terhubung ke Neo4j untuk mengambil data interaksi antara node `Visitor` dan `Item`. Data yang digunakan mencakup jenis interaksi `VIEWED`, `ADDED_TO_CART`, dan `BOUGHT`.

Koneksi dilakukan melalui driver Neo4j, sedangkan pengambilan data menggunakan query Cypher sesuai periode yang dianalisis.

### 3. Pembentukan Vector Space Model (VSM)

Interaksi pengguna dan item direpresentasikan dalam bentuk matriks Visitor × Item. Setiap jenis interaksi diberikan bobot sebagai berikut:

| Jenis interaksi | Bobot |
|---|---:|
| `VIEWED` | 1 |
| `ADDED_TO_CART` | 3 |
| `BOUGHT` | 5 |

Bobot digunakan untuk merepresentasikan perbedaan tingkat interaksi pengguna terhadap item. Matriks dibentuk menggunakan sparse matrix agar penggunaan memori lebih efisien, mengingat tidak semua pengguna berinteraksi dengan seluruh item.

### 4. Analisis User dan Item Similarity

Kemiripan dihitung menggunakan cosine similarity berdasarkan matriks interaksi. Analisis dilakukan melalui dua proyeksi:

- **User projection:** melihat kemiripan antar pengguna berdasarkan pola interaksi mereka terhadap item.
- **Item projection:** melihat kemiripan antar item berdasarkan pola interaksi pengguna terhadap item tersebut.

Untuk visualisasi, notebook memilih 100 pengguna dan 100 item dengan jumlah interaksi unik terbanyak. Hubungan kemiripan kemudian direpresentasikan sebagai graf menggunakan NetworkX.

Visualisasi juga dibuat dalam beberapa bentuk, termasuk graf dengan edge terkuat dan tampilan yang lebih ringkas agar hubungan antarnode lebih mudah diamati.

### 5. Pengujian Menggunakan Data Periode P2

Data periode P2 dipersiapkan sebagai data pengujian. Pada tahap ini, notebook memeriksa pengguna dan item yang memenuhi syarat evaluasi berdasarkan data yang tersedia pada periode P1 dan P2.

Pengujian dibatasi pada item yang telah dikenal dalam data training agar sesuai dengan representasi item pada matriks interaksi yang digunakan oleh model.

### 6. Penerapan K-Nearest Neighbors (KNN)

Metode KNN digunakan untuk mencari pengguna lain yang memiliki pola interaksi serupa. Pencarian tetangga dilakukan menggunakan cosine distance.

Item yang telah berinteraksi dengan pengguna pada periode P1 tidak dimasukkan kembali sebagai rekomendasi. Kandidat item dari pengguna lain diberi skor berdasarkan kemiripan pengguna dan bobot interaksinya, kemudian diurutkan untuk menghasilkan rekomendasi Top-5.

### 7. Evaluasi Sistem Rekomendasi

Evaluasi dilakukan pada sampel hingga 100 pengguna menggunakan data periode P2. Dua metrik yang digunakan adalah:

- **Hit@5:** mengukur apakah terdapat setidaknya satu item relevan di antara lima rekomendasi teratas.
- **NDCG@5:** mengukur kualitas urutan rekomendasi dengan mempertimbangkan posisi item relevan.

Hasil evaluasi digunakan untuk melihat sejauh mana rekomendasi yang dihasilkan sesuai dengan interaksi pengguna pada periode pengujian.

### 8. Penyimpanan dan Pemeriksaan Relasi di Neo4j

Notebook juga melakukan pemeriksaan terhadap relasi `SIMILAR_TO` di Neo4j. Query Cypher digunakan untuk mengambil lima pengguna dengan nilai similarity tertinggi terhadap pengguna referensi yang dipilih.

Tahap ini menghubungkan hasil analisis kemiripan di Python dengan penyimpanan dan penelusuran hubungan pada graph database.

## Hasil Pengerjaan

Berdasarkan hasil yang tercatat pada notebook, pengerjaan mencakup pembentukan matriks interaksi berbobot, analisis kemiripan pengguna dan item, visualisasi graf, serta rekomendasi Top-5 menggunakan KNN.

Evaluasi pada 100 pengguna menghasilkan rata-rata Hit@5 sebesar **0,0100** dan NDCG@5 sebesar **0,0021**. Hasil ini menunjukkan bahwa performa rekomendasi pada pengujian masih rendah dan masih dapat dikembangkan lebih lanjut.

Notebook juga memuat pemeriksaan relasi `SIMILAR_TO` di Neo4j untuk menelusuri pengguna yang memiliki kemiripan tertinggi dengan pengguna referensi.

## Cara Menjalankan Notebook

1. Unduh atau clone repository ini ke komputer.
2. Unduh dataset Retailrocket dari [Kaggle](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset) apabila diperlukan untuk pemrosesan ulang.
3. Ekstrak file `events_P1.csv.zip` dan `events_P2.csv.zip` yang tersedia pada folder `data` di repository utama.
4. Pastikan file CSV berada pada lokasi folder yang sesuai dengan konfigurasi `PROJECT_DIR` di notebook.
5. Instal library Python yang digunakan apabila belum tersedia di lingkungan kerja.
6. Jalankan Neo4j dan pastikan database dapat diakses melalui URI yang dikonfigurasi pada notebook.
7. Jalankan setiap sel notebook secara berurutan dan masukkan password Neo4j ketika diminta.

**Catatan:** konfigurasi lokasi folder dan koneksi Neo4j pada notebook perlu disesuaikan dengan lingkungan komputer masing-masing. Password Neo4j dimasukkan saat notebook berjalan dan tidak perlu ditulis langsung di dalam notebook.

## Sumber Dataset

Retailrocket Recommender System Dataset: https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset
