# Laporan Proyek Machine Learning - Melina Dwi Saitri

<detail>
<summary>Daftar Isi</summary>

- [Domain Proyek](#domain-proyek)
- [Business Understanding](#business-understanding)
    - [Problem Statements](#problem-statements)
    - [Goals](#goals)
    - [Solution statements](#solution-statements)
- [Data Understanding](#data-understanding)
    - [Eksplorasi Data](#eksplorasi-data)
- [Content Based FIltering](#content-based-filtering)
    - [Data Preparation](#data-preparation)
</detail>

## Project Overview
Pariwisata adalah kegiatan berkunjung ke suatu tempat lain dengan tujuan mancari suana baru. Pariwisata juga salah satu penghasil devisi yang tinggi untuk daerah sehingga sektor pariwasata terus ditingkatkan. Tetapi karena banyaknya lokasi wisata di daerah yang belum terekspos dan dikenal orang membuat orang kesulitan mencari lokasi wisata yang menarik untuk dikunjungi. 
Referensi:

* [Pemanfaatan Metode Item Based Collaborative Filtering Untuk 
Rekomendasi Wisata Di Kabupaten Malang](https://jurnal.stmikasia.ac.id/index.php/jitika/article/view/70)
* [A hybrid context aware system for tourist guidance 
based on collaborative filtering](https://ieeexplore.ieee.org/abstract/document/6007604/)

Sehingga diperlukan sebuah sistem rekomendasi yang bisa membantu orang untuk menemukan tempat wisata yang menarik berdasarkan nama (menggunakan lokasi/kota sebagai penentu) maupun rating yang diberikan orang.

## Domain Proyek
Domain dalam projek ini adalah pariwisata, disesuaikan dengan permasalah yang ada di Indonesia, untuk mempermudah menemukan lokasi wisata menggunakan sistem rekomendasi.

## Business Understanding

Untuk menyelesaikan permasalah dalam menemukan tempat lokasi wisata yaitu dengan mencari data yang memuat lokasi wisata yang ada di Indonesia lalu membuat model yang bisa merekomendasikan berdasarkan content maupun rekomendasi dari user lain berdasarkan histori nya.

### Problem Statements
* Apakah data yang digunakan memiliki potensi error yang besar jika digunakan terus menerus?
* Bagiamana cara memberikan rekomendasi user berdasarkan nama dari tempat tersebut?
* Dengan data yang sudah ditemukan, bagaimana penyedia aplikasi dapat merekomendasikan tempat yang menarik kepada pengguna?

### Goals
* Mencari data tempat wisata di indonesia sehingga data tersebut dapat digunakan, kemudian data tersebut akan di ekplorasi sehingga data siap digunakan untuk membuat sistem rekomendasi.
* Mengahasikan beberapa rekomendasi tempat dengan teknik *Content based Filtering* dan teknik *Colaborative Filtering* berdasarkan data yang sudah di dapatkan.

### Solution statements
* Data dapat dicari dengan berbagai cara salah satunya dengan mengambil data dati [kaggel](https://kaggle.com) setelah data di dapatkan data akan di eksplorasi untuk mengecek data tersebut memiliki missing value atau tidak.
* Untuk membuat sistem rekomendasi berdasarkan nama bisa digunakan teknik *content based filtering* dengan memanfaatkan *TFIDF Vectorizer* kemudian menggunakan *cosine similarity* untuk menarcari kesamaan antar nama tempat.
* Sedangkan untuk memberikan rekomendasi user berdasarkan referensi user lain yang telah datang/menggunakan digunakan teknik *Colaborative filtering* dengan mengambil nilai rating yang telah diberikan pengguna.

## Data Understanding
Data yang digunakan adalah data tourism dari kaggle:

[Tourism Destination in Indonesia](https://www.kaggle.com/aprabowo/indonesia-tourism-destination)

Data ini terdiri dari 4 file yaitu :
* tourism_ with_id.csv : file ini berisi informasi lokasi wisata dari 5 kota di Indonesia 
* user.csv : file ini berisi mengenai data pengguna (data dummy)
* tourism_rating.csv : file ini berisi mengenai data rating dari bebrapa tempat lokasi wissata yang diberikan oleh user 
* package_tourism.csv : file ini berisi package lokasi wisata

### Univariate Eksplorasi Data
Setiap file akan di deklarasikan dalam varibale dataframe.
Data terdiri dari 100 row yang berisi kolom :
1. Package          : id package
2. City             : Lokasi tempat wisata
3. Place_Tourism1   : lokasi 1
4. Place_Tourism2   : lokasi 2
5. Place_Tourism3   : lokasi 3
6. Place_Tourism4   : lokasi 4
7. Place_Tourism5   : lokasi 5

![package-tourism.csv]()

#### Eksplorasi data Package Tourism
#### Eksplorasi data User
#### Eksplorasi data Tourism Ratings
#### Eksplorasi data Tourism with id

## Content Based Filtering

### Data Preparation

### Modeling

### Evaluation Model

## Colaborative Filtering

### Data Preparation Colaborative FIltering

### Modeling Colaborative FIltering

#### Ploting model Colaborative FIltering

### Evaluation Model Colaborative FIltering

### Prediction 

## Kesimpulan