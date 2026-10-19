# Dataset

Folder ini berisi dataset yang digunakan dalam pengerjaan UTS Graf Sains Data. Dataset yang digunakan adalah **Retailrocket Recommender System Dataset**, yang berisi data interaksi pengguna dengan item pada platform e-commerce.

Dataset ini digunakan untuk membangun graf interaksi antara *Visitor* dan *Item* di Neo4j. Data tersebut kemudian dimanfaatkan untuk analisis kemiripan (*similarity*) dan pembuatan rekomendasi item sesuai dengan tahapan yang diterapkan pada notebook.

## Dataset yang Digunakan

Dataset Retailrocket terdiri dari beberapa file utama:

- `events.csv` — berisi catatan aktivitas atau interaksi pengguna terhadap item.
- `category_tree.csv` — berisi informasi hubungan hierarki antar kategori.
- `item_properties_part1.csv` dan `item_properties_part2.csv` — berisi informasi properti item.

Dataset mentah dapat diunduh melalui Kaggle berikut:

[Retailrocket Recommender System Dataset](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset)

## Data Hasil Pemrosesan

Pada tugas ini, data interaksi difilter menjadi dua periode analisis, yaitu P1 dan P2. Hasil pemrosesan tersebut disimpan dalam format CSV dan dikompres menjadi ZIP agar lebih mudah diunggah ke GitHub.

File yang tersedia dalam folder ini adalah:

- `events_P1.csv.zip` — dataset interaksi hasil pemfilteran untuk periode P1.
- `events_P2.csv.zip` — dataset interaksi hasil pemfilteran untuk periode P2.

Untuk menggunakan data hasil pemrosesan, unduh file ZIP yang diperlukan, kemudian ekstrak terlebih dahulu hingga mendapatkan file CSV. File CSV tersebut dapat digunakan sesuai dengan tahapan analisis pada notebook.

## Catatan Penggunaan

File P1 dan P2 merupakan data hasil pemrosesan dari dataset Retailrocket yang digunakan dalam analisis tugas ini. Apabila ingin menjalankan ulang proses dari data mentah, unduh dataset asli melalui tautan Kaggle di atas dan ikuti tahapan pemrosesan yang dijelaskan dalam notebook.

Notebook pengerjaan dapat ditemukan pada folder `notebooks` di repository ini.
