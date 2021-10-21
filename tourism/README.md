# Laporan Proyek Machine Learning - Melina Dwi Saitri

<details>
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

</details>

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
* Data dapat dicari dengan berbagai cara salah satunya dengan mengambil data dari [kaggle](https://kaggle.com) setelah data di dapatkan data akan di eksplorasi untuk mengecek data tersebut memiliki missing value atau tidak.
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
#### Eksplorasi data Package Tourism
Data terdiri dari 100 row yang berisi kolom :
1. Package          : id package
2. City             : Lokasi tempat wisata
3. Place_Tourism1   : lokasi 1
4. Place_Tourism2   : lokasi 2
5. Place_Tourism3   : lokasi 3
6. Place_Tourism4   : lokasi 4
7. Place_Tourism5   : lokasi 5

![package-tourism.csv](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/package.png)

Kolom package memiliki type data integer sedangkan kolom data yang lain memiliki type data object/string.

#### Eksplorasi data User
![user-data](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/user.png)

Data User memiliki 300 row dengan 3 kolom:
1. User_id : merupakan kolom yang berisi mengenai id dari user
2. Location : kolom ini berisi tentang lokasi/alamat dari user berdasarkan kota dan provinsi
3. Age : Kolom ini berisi mengenai umur dari user

Data ini bersifat dummy. type data User_id dan Age adalah integer sedangkan data Location adalah Object/string. Data akan divisualisaskikan berdasarkan id user dan lokasi/alamat user berdasarkan provinsi. Maka data Lokasi akan dipecah menjadi kota dan provisi. 
````
tourism_user[['kota', 'provinsi']] = tourism_user['Location'].str.split(',', expand=True)
````

![user-data-split-location](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/user-loc.png)

Kemudian data akan divisualisasikan ke dalam bar plot dengan ketentuann id sebagai height, bertujuan untuk mengetahui dari mana saja user berasal.

![user-data-plot](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/user-plot.png))

User berasal dari Jawa tengah, Jawa Barat, Sumatera Selatan, DKI Jakarta, Jawa Timur, DIY dan Banten.
#### Eksplorasi data Tourism Ratings
![Data-Ratings](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/ratings.png)

Data tersebut memiliki 1000 row dan 3 kolom dengan type data integer semua 
* user id = untuk id user
* place id = untuk id dari tempat/lokasi wisata
* Place rating = untuk rating setiap lokasi yang diberikan user

Data akan di deskripsikan berdasarkan statistika menghasilakn min dari ratings 1.0 dan max adalah 5.0. Seharusnya data User_Id dan Place_id tidak boleh masuk ke descripsi statiska, masuk karena data memiliki type data integer.

![rating-describe](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/rating-desc.png)
#### Eksplorasi data Tourism with id
![Data-Tourisn-info](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tourism.png)

Data memiliki 437 row dengan 13 kolom tetapi ada 2 kolom yang tidak bernama maka kolom tersebut aka dihapus karena tidak diketahui itu jenis data apa.

![tourism-info](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tourism-info.png)

- Place-Id : id untuk tempat wisata
- Place_Name: Nama dari lokasi wisata
- Description: penjelasan singkat mengenai tempat wisata tersebut
- Category: wisata termasuk dalam kategori apa(budaya, taman-hiburan dll)
- City: Lokasi tempat wisata(kota)
- Price: biaya masuk ke lokasi wisata 
- Rating : tingkat kepuasan pelanggan
- Time_Minutes: waktu perjalanan
- Coordinate: koordinat dari lokasi wisata
- Lat: Garis Lintang
- Long: Garis Bujur

Untuk membuang kolom yang tidak diketahui itu kolom apa dengan menggunakan script

```
tourism_inform = tourism_info.loc[:, ~tourism_info.columns.str.contains('^Unnamed')]
```

Kemudian cek data tourism_info ini apakah terdapat ada missing atau tidak teryata data time_minutes memiliki 205 data nan, karena kalau data dari row yang mengalami missing value tersebut dihapus maka data akan hilang sebagian, untuk mengatasi itu makan kolom Time_minutes akan di drop.

## Content Based Filtering
Teknik ini digunakan untuk memberi rekomendasi berdasarkan history dari pengguna itu sendiri.
### Data Preparation
Data yang digunakan adalah data rating dari user,data user serta data tourism info.
Setelah di cek dalam proses ekplorasi data informasi tempat hanya berada pada file tourism_ with_id.csv. Data tersebut akan digabung dengan data rating, penggabungan dilakukan berdasarkan place ID. 

![Data-merge](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-p1.png)

Data kemudian di cek apakah data hasil *merge* terdapat missing value apa tidak. Jika ada maka data akan di drop atau impute.

![Data-cek_ms](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-p-cekms.png)

Tidak ada data yang mengalamu missing value di dalam data merge tersebut.

Data akan dimerge kembali, data tourism_inform yang akan akan digunakan hanya akan diambil id_place, place_name, city dan categorinya saja. Data itu akan di gabung degan data rating.

![Data-merge](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-p-merge.png)

Karena data yang akan digunakan untuk merekomendasikan tempat wisata diambil dari nama city maka data city di cek.

```
tourism_spot.City.unique()
```

Setelah di cek ternya ada 5 kota dalam data ini yaitu, yogyakarta, semarang, jakarta, Bandung, dan Surabaya. 

Untuk membuat data berurutan berdasarkan id place maka di lakukan `sort_values` berdasarkan place id. Data juga di cek apakah ada data yang mengalami duplicate atau tidak berdasarkan place_id yang ada.
Ternyata terdapat 9563 data yang mengalami duplikasi. Maka data tersebut akan dihapus. 

![Drop-duplicated](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/drop-duplicate.png)

Data berkurang menjadi 437 dari data awal yang berjumlah 10000. Data tersebut akan disimpan di dalam sebuah dictionary untuk digunak dalam proses pembuatan model dalam sistem rekomendasi.

![Data-dict](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-dict.png)

(Data setelah dibuat menjadi dictionary)

#### TFIDF 

Data di convert menjadi matrix dengan menggunkan ``TfidfVectorizer()`` Data yang akan digunakan sebagai matrix adalah data city. Data tersebut akan di `fit_transform` untuk mempelajari kosakata yang ada diubah menjadi data matrix. Data matrix tersebut akan dibuat sebuah dataframe untuk melihat korelasi antara data nama lokasi wisata dengan data kota. Untuk memasukkan data metrix kedalam satu dataframe dimanfaatakan fungsi `to_dense`.

![to-dense](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/to-dense.png)

Data diatas terlihat bahwa Pantai Glagah berkorelasi dengan Yogyakarta yang mengindikasikan bahwa Pantai Glagah terdapat di yogyakarta

#### Cosine Similarity
Berfungsi untuk menghitung kesamaan kata antar nama lokasi wisata. 

![cosine](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/cosine.png)

Data akan dirubah menjadi array data yang digunakan untuk cosine_similarity adalah data matrix yang sebelumnya sudah dirubah yang awalnya data biasa dirubah menjadi aaray.

![Similarity](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/similarity.png)

Data hasil similarity akan disimpan di dalam varible baru dimana data ini yang akan digunakan untuk membangun model nanti.

![similariry1](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/similarity1.png)

### Modeling
model dibangun dengan mengambil data nama tempat dan menggunakan similariry untuk memberikan reomendasi data yang direkomendasikan diatur hanya menampilkan 20 data saja yang berisi informasi place name dan place_city saja.

Data yang diambil untuk dilihat rekomendasinya adalah nama lokasi <b>Pulau Pramuka</b>

![Data-pulau](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-asli.png)

Data yang direkomendasikan memiliki nilai kota yang sama yaitu Jakarta.

![Data-pulau](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/rekomendasi.png)
### Evaluation Model
![precision](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/eval-latex.png)

Berdasarkan hasil rekomdasi diatas diketahui bahwa data yang memiliki nama kota yang sama dengan pulau pramuka adalah semuanya maka data tersebut akan dibagi dengan jumlah data yang direkomendasikan.
```
precision = (len(recomended['place_city'] == 'Jakarta')/len(recomended))*100
print(f'precission: {precision} %')
```

Setelah dihitung ternyata data 20/20 = 1 atau data memiliki nilai precision yang baik. Sehingga model maupun data baik untuk digunakan dalam membuat sistem rekomendasi berdasarkan content.
## Colaborative Filtering
Tenik ini digunakan untuk merekomendasikan user berdasarkan user_group atau bebeberapa user lain. Data yang digunakan adalah data rating yang sudah diberikan oleh pengguna/user.
### Data understanding Colaborative Filtering
Data yang digunakan adalah data rating data ini memili row 1000 dan kolom 3.

![rating](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/ratings.png)
### Data Preparation Colaborative FIltering
Data yang digunakan adalah data user_id dan id place yang terdapat pada rating, data tersebut awalnya bertype int, untuk melakukan encoder maka data tersebut dirubah typenya menjadi string/object. Kemudian data place_id dan user_ di encode untuk dirubah menjadi index.

![encode](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/encode.png)

Data hasil encode akan dihitung jumlahnya jumlah data ini nanti yang akan digunakan untuk proses modeling nanti. Data place_rating akan di minmaxScaler secara manual dengan memanfaatkan nilai min dan max dari data place_rating 


\begin{align}
(data - min)/(max- min)
\end{align}

### Modeling Colaborative FIltering

#### Ploting model Colaborative FIltering

### Evaluation Model Colaborative FIltering

### Prediction 

## Kesimpulan