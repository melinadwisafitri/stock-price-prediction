# Laporan Proyek Machine Learning - Melina Dwi Saitri

<details>
<summary>Daftar Isi</summary>

- [Domain Proyek](#domain-proyek)
- [Business Understanding](#business-understanding)
    - [Problem Statements](#problem-statements)
    - [Goals](#goals)
    - [Solution statements](#solution-statements)
- [Data Understanding](#data-understanding)
    
- [Content Based FIltering](#content-based-filtering)
    - [Data Preparation](#data-preparation)

</details>

## Project Overview
Pariwisata adalah kegiatan berkunjung ke suatu tempat lain dengan tujuan mencari suasana baru ([Islamiyah, M., Subekti, P. and Andini, T.D : 2019](https://jurnal.stmikasia.ac.id/index.php/jitika/article/view/70)). Pariwisata juga salah satu penghasil devisa yang tinggi untuk daerah sehingga sektor pariwisata terus ditingkatkan. Tetapi karena banyaknya lokasi wisata di daerah yang belum terekspos dan dikenal orang membuat orang kesulitan mencari lokasi wisata yang menarik untuk dikunjungi. 

Sehingga diperlukan sebuah sistem rekomendasi yang bisa membantu orang untuk menemukan tempat wisata yang menarik berdasarkan nama (menggunakan lokasi/kota sebagai penentu), history data ([G. Fenza, E. Fischetti, D. Furno, V. Loia : 2011](https://ieeexplore.ieee.org/abstract/document/6007604/)), maupun *rating* yang diberikan orang.

## Domain Proyek
Domain dalam proyek ini adalah pariwisata, disesuaikan dengan permasalahan yang ada di Indonesia, untuk mempermudah menemukan lokasi wisata menggunakan sistem rekomendasi.

## Business Understanding

Untuk menyelesaikan permasalah dalam menemukan tempat lokasi wisata yaitu dengan mencari data yang memuat lokasi wisata yang ada di Indonesia lalu membuat model yang bisa merekomendasikan berdasarkan content maupun rekomendasi dari user lain berdasarkan histori nya.

### Problem Statements
* Bagaimana cara memberikan rekomendasi user berdasarkan tempat yang pernah didatangi?
* Bagaimana cara membuat sistem rekomendasi berdasarkan rating tertinggi dari user lain?
* Bagaimana membuat model yang baik dengan tingkat error yang rendah?

### Goals

* Membuat sistem rekomendasi berdasarkan data *personalized user* sehingga bisa memberikan rekomendasi lokasi wisata kepada user berdasarkan lokasi yang pernah dikunjungi.
* Membentuk suatu model yang bisa merekomendasi kepada user berdasarkan nilai rating yang tinggi, dengan algoritma yang sesuai, dengan tingkat error yang tidak besar.

### Solution statements

* Untuk memberikan rekomendasi user berdasarkan lokasi yang pernah dikunjungi yaitu dengan menggunakan teknik *content based filtering* dengan mengambil kesamaan nilai/kata dari salah satu item yang ada.
* Untuk memberikan rekomendasi user berdasarkan rating suatu tepat dapat dilakukan dengan memanfaatkan  teknik *Colaborative filtering*, menggunakan algoritma Deep Learning dan Embedding layers.


## Data Understanding
Data yang digunakan adalah data tourism dari kaggle:

[Tourism Destination in Indonesia](https://www.kaggle.com/aprabowo/indonesia-tourism-destination)

Data ini terdiri dari 4 file yaitu :
* tourism_ with_id.csv : file ini berisi informasi lokasi wisata dari 5 kota di Indonesia 
* user.csv : file ini berisi mengenai data pengguna (data *dummy*)
* tourism_rating.csv : file ini berisi mengenai data *rating* dari beberapa tempat lokasi wisata yang diberikan oleh user 
* package_tourism.csv : file ini berisi paket lokasi wisata

### Univariate Eksplorasi Data
Setiap file akan di deklarasikan ke dalam variable dataframe.
#### Eksplorasi data Package Tourism
Data terdiri dari 100 baris yang berisi kolom :
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

Data User memiliki 300 baris dengan 3 kolom:
1. User_id : merupakan kolom yang berisi mengenai id dari user
2. Location : kolom ini berisi tentang lokasi/alamat dari user berdasarkan kota dan provinsi
3. Age : Kolom ini berisi mengenai umur dari user

Data bersifat dummy, type data User_id dan Age adalah integer sedangkan data Location adalah Object/string. Data akan di visualisasikan berdasarkan id user dan lokasi/alamat user berdasarkan provinsi. Maka data Lokasi akan dipecah, menjadi data kota dan provisi. 

````
tourism_user[['kota', 'provinsi']] = tourism_user['Location'].str.split(',', expand=True)
````

![user-data-split-location](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/user-loc.png)

* Visualisasi data user berdasarkan lokasi tempat tinggal

![user-data-plot](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/user-plot.png))


User berasal dari Jawa tengah, Jawa Barat, Sumatera Selatan, DKI Jakarta, Jawa Timur, DIY dan Banten.

#### Eksplorasi data Tourism Ratings
![Data-Ratings](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/ratings.png)

Data memiliki 1000 baris dan 3 kolom dengan type data integer semua.
* user id = untuk id user
* place id = untuk id dari tempat/lokasi wisata
* Place rating = untuk rating setiap lokasi yang diberikan user

Data akan di deskripsikan berdasarkan statistika menghasilkan min dari ratings 1.0 dan max adalah 5.0. Seharusnya data User_Id dan Place_id tidak boleh masuk ke deskripsi statistika. Tetapi karena jenis data integer maka terhitung memiliki nilai statistika.

![rating-describe](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/rating-desc.png)


#### Eksplorasi data Tourism with id
![Data-Tourisn-info](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tourism.png)

Data memiliki 437 baris dengan 13 kolom, tetapi ada 2 kolom yang tidak bernama maka kolom tersebut akan dihapus karena tidak diketahui itu jenis data apa.

![tourism-info](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tourism-info.png)

- Place_Id : id untuk tempat wisata
- Place_Name: Nama dari lokasi wisata
- Description: penjelasan singkat mengenai tempat wisata tersebut
- Category: wisata termasuk dalam kategori apa(budaya, taman hiburan dll)
- City: Lokasi tempat wisata(kota)
- Price: biaya masuk ke lokasi wisata 
- Rating : tingkat kepuasan pelanggan
- Time_Minutes: waktu perjalanan
- Coordinate: koordinat dari lokasi wisata
- Lat: Garis Lintang
- Long: Garis Bujur

Script dibawah ini digunakan untuk menghapus kolom tanpa nama: 

```
tourism_inform = tourism_info.loc[:, ~tourism_info.columns.str.contains('^Unnamed')]
```

Cek data kembali

![cek-ms](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tm-missing.png)

Data Time_minutes memiliki 205 data *missing value*, karena data yang mengalami *missing value* terlalu banyak maka kolom tersebut dihapus/drop.

![information-fix](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/tourism-finish.png)


Teknik ini digunakan untuk memberi rekomendasi berdasarkan *history* dari pengguna itu sendiri.
## Data Preparation
### Content Based Filtering
Data yang digunakan adalah data tourism_with_id/tourism_inform(place_id, place_name dan place_city). Yang akan disimpan ke dalam variable baru.


![Data-merge](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data_p_content.png)

#### TFIDF 

Data di rubah menjadi matrix dengan menggunakan ``TfidfVectorizer()`` Data yang akan digunakan sebagai matrix adalah data city. Data tersebut akan di `fit_transform` untuk mempelajari kosakata yang ada diubah menjadi data matrix. Data matrix tersebut akan dibuat sebuah dataframe untuk melihat korelasi antara data nama lokasi wisata dengan data kota. Untuk memasukkan data matrix ke dalam satu dataframe dengan manfaatkan fungsi `to_dense`.

![to-dense](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/to-dense.png)

Data diatas terlihat bahwa Gembira Loka Zoo berkorelasi dengan Yogyakarta yang mengindikasikan bahwa Gembira Loka Zoo terdapat di yogyakarta.

#### Cosine Similarity
Berfungsi untuk menghitung kesamaan kata antar nama lokasi wisata. 

![cosine](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/cosine.png)

Data akan diubah menjadi array, data yang digunakan untuk cosine_similarity adalah data yang sudah di konversi menggunakan TFIDF Vectorizer.

![Similarity](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/similarity.png)

Data hasil similarity akan disimpan di dalam variable baru di mana data ini yang akan digunakan untuk membangun model nanti.

![similariry1](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/similarity1.png)


## Collaborative Filtering
### Data understanding Collaborative Filtering
Data yang digunakan adalah data rating, yang memiliki baris 1000 dan kolom 3.

![rating](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/ratings.png)

Data Rating akan digabungkan dengan data tourism_inform ( place_id, place_Name, City, Category dan Rating). Data disesuaikan dengan place_Id.

![Merge](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/merge_col.png)

Data berjumlah 1000 baris dan data tidak memiliki missing value setelah proses *merge*. Data akan di urutkan berdasarkan place_id, digunakan untuk mempermudah proses selanjutnya.

![Merge-sort](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/merge_sort.png)

Data yang digunakan adalah data user_id dan id place yang berasal dari data rating, data tersebut akan di **encode*/konversi untuk membuat index yang runtut dengan type data integer. Data akan disimpan ke dalam kolom baru.

![encode-map](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/encode-map.png)

Data yang akan dipecah untuk data train dan validation akan diambil terlebih dahulu yaitu data [id_user, id_place] dan data Place_Ratings.

Jangan lupa untuk menghitung jumlah hasil masing-masing encode dari place dan user.

#### split data train dan val
Sebelum data dipecah data place_rating akan di normalisasi terlebih dahulu menggunakan MinMaxScaler dari sklearn. Proses ini berfungsi untuk mengurangi tingkat error.

Lalu data dipecah menggunakan train_test_split dengan kriteria data train 80% dan val 20% didapatkan jumlah train 8000 dan val adalah 2000.

## Modeling
### Content Based Filtering
Model dibangun dengan mengambil data nama tempat dan menggunakan similarity untuk memberikan rekomendasi data yang direkomendasikan diatur hanya menampilkan 10 data saja yang berisi informasi place name dan place_city saja.

Data yang diambil untuk dilihat rekomendasinya adalah nama lokasi <b>Pantai Ancol</b>

![Data-pulau](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/data-asli.png)

Data yang direkomendasikan memiliki nilai kota yang sama yaitu Jakarta.

![Data-pulau](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/rekomendasi.png)

### Modeling Collaborative FIltering

Setelah data selesai disiapkan dilanjutkan dengan membuat model dengan memanfaatkan tensorflow berdasarkan proses embedding dari user_id dan place_id, dengan menggunakan perkalian dot product [0, 1] menggunakan sigmoid sebagai *activation*. regulasi yang digunakan adalah li digunakan untuk mengetahui berdasarkan nilai median dari data. 

Model yang telah dibangun diberikan nilai berdasarkan jumlah data encode user_id dan data jumlah encode place_id. Dengan embeding size 16 (disesuaikan dengan kebutuhan model).

Sedangkan optimizer yang digunakan untuk model adalah Adam dengan learning rate 1e-4 dengan metrics MAE. Model di training menggunakan batch 5 dan epochs 50.

![plot](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/plot.png)

Diketahui nilai mae terendah adalah 0.2465 (25%) nilai ini cukup rendah tetapi nilai val menaik ketika nilai mae menurun. Hal ini bisa mengakibatkan model *overfitting*.

## Evaluation Model
### Content Based Filtering
![precision](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/eval-latex.png)

Berdasarkan hasil rekomendasi diketahui bahwa hasil rekomendasi memiliki nama kota yang sama yaitu Jakarta, untuk menghitung ketepatan dari model dengan menggunakan rumus berikut: 

```
precision = (len(recomended['place_city'] == 'Jakarta')/len(recomended))*100
print(f'precission: {precision} %')
```

Setelah dihitung ternyata memiliki hasil precision 10/10 = 1, data memiliki nilai precision yang baik. Sehingga model maupun data baik untuk digunakan dalam membuat sistem rekomendasi berdasarkan content. Model ini hanya merekomendasikan berdasarkan history lokasi/kota. Untuk meningkatkan bisa dengan melakukan rekomendasi berdasarkan kategori tempat wisata.

### Evaluation Model Collaborative FIltering
Evaluasi dilakukan untuk mengetahui bagaimana tingkat error model sehingga bisa mengetahui bagaimana performa dari model yang dibuat. Evaluasi menggunakan metrics MSE. 

![mae](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/mae_rumus.png)

Nilai mae yang didapatkan tidak terlalu tinggi sehingga model ini cocok untuk digunakan dalam sistem rekomendasi, tetapi mae dengan nilai 34% bisa mengakibatkan *overfitting *model.








### Prediction 
Untuk pengujian dari evaluasi model,maka diperlukan pengujian. Proses pengujian dapat dilakukan dengan menggunakan data tourism info yang sudah diproses sebelumnya, dan data tersebut disimpan dalam variable data berjenis dictionary. 

Rekomendasi dari user 225(kenapa 284 karena data yang diambil contoh acak berdasarkan id user pada data rating) mengatakan top 3 rating tertinggi adalah:
* Kampung Wisata Taman Sari : Taman Hiburan
* Pantai Watu Kodok : Bahari
* Curug Cimahi : Cagar Alam 
Data hanya ditampilkan top 3 saja, jumlah berapa banyak data ditampilkan dapat diatur di dalam sistem.

![hasil](https://github.com/melinadwisafitri/BCA_Stock_price_dan_tourism/raw/master/tourism/images/rekomendasi-c.png)

Kemudian sistem akan merekomendasikan 5 tempat lainnya yang memiliki nilai yang sama berdasarkan penilaian user.