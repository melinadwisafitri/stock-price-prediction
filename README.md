# Laporan Proyek Machine Learning - Melina Dwi Safitri

## Domain Proyek
Dalam bidang perbankan khususnya saham kadang harga close untuk minggu selanjutnya belum bisa untuk ditentukan, sehingga diperlukan cara untuk mempermudah permasalahan tersebut


## Business Understanding
Bagian ini menjelaskan proses klarifikasi masalah dan mengajukan minimal satu solusi untuk menyelesaikan permasalahan. Bagian laporan ini mencakup:

### Problem Statements
- Bagaimana cara menentukan prediksi price close minggu depan?
- Bagaimana membangun machine Learning yang sesuai dengan data bentuk Time Series?

### Goals
- Proyek ini bertujuan untuk memprediksi harga close untuk harga saham BCA, dengan mengimplementasikan machine learning didalamnya sehingga lebih mempermudah proses
- Membentuk model yang optimal dengan data Time Series

### Solution statements
untuk mengatasi permasalahan disini menggunakan satu algoritma yaitu Convolutional LSTM

## Data Understanding
Data yang digunakan adalah dataset stock price dari BCA yang diambil dari tahun 2010 sampai 2020 dari Yahoo Finance <br>
[Data History BCA Stock price](https://finance.yahoo.com/quote/BBCA.JK/history?p=BBCA.JK ) 

Data yang digunakan berjumlah 2731 data.

![images](images/data_awal.png)

ada beberapa variable dalam data tersebut yaitu:

1. Low : harga terendah dalam pergerakan harga setelah harga dibuka hingga close
2. High: harga tertinggi dalam pergerakan harga, dalam hal ini biasanya ketika data semakin tinggi maka akan ada orang yang membeli
3. Open : harga awal ketika transaksi pertama kali di lakukan pada hari itu. Harga diambil dari harga close hari sebelumnya
4. Close : harga terakhir, harga ini penting karena penting untuk proses anaisis
5. Volume : berapa banyak transaksi yang terjadi, dalam jumlah per lembar
6. Adj Close : haraga akhir/penutupan yang sudah disesuaikan dengan aksi  korporasi, kondisi yang terjadi seperti isu isu yang sedang terjadi 

ke-6 variable data memiliki type float semua. 

## Data Eksploration 

Eksplorasi data bertujuan untuk mengetahui apa saja jenis data yang ada di dalam dataset yang digunakan 

* Mengecek data yang mengalami mising value atau tidak. <br>
Setelah pengecekan ternyata tidak ada data yang mengalami missing value
* mengecek data yang mengalami duplicate
diketahui bahwa ada sebanyak 30 yang mengalami duplicated

[data_duplicate](images/duplicated.png)

<br> 
untuk mengatasi itu dilakukan penghapusan data yang mengalami duplikat

* cek Deskripsi statistika dengan menggunakan ```data.describe() ```

[descriptive_statistika](images/desc.png)

Diketahui data memiliki nilai minimal 

* cek korerasi antar data untuk mengetahui keterkaitan antar variable 



## Data Preparation
setelah data di Eksplorasi maka data akan di bagi menjadi data x dan y karena data time series maka diperlukan pembagian data, peroses pembagian ini mengambil 5 step langkah yang digunakan <br>
Setelah itu data di split menjadi data train, test dan validations

## Modeling
Modeling dibuat dengan menggunakan algoritma Convolutional LSTM, data input nya bernilai None, 5 dikarenakan terdapat 5 kolom yang didapatkan dari proses perpecahan data x dan y yang sudah dilakukan.


## Evaluation
Didaptkan mae yang masih besar untuk data ini sehingga diperlukan proses evaluasi lebih lanjut untuk menyelesaikan tingkat error