# Laporan Proyek Machine Learning - Melina Dwi Safitri

<details>
<summary>Daftar isi</summary>

- [Domain Proyek](#domain-proyek)
- [Business Understanding](#business-understanding)
    - [Problem Statements](#problem-statements)
    - [Goals](#goals)
    - [Solution statements](#solution-statements)
- [Data Understanding](#data-nderstanding)
- [Data Eksplotatio](#data-eksploration)
- [Data Preparation](#data-preparation)
- [Modeling](#modeling)
- [Evaluation Model](#evaluation-model)
- [Prediction](#prediction-next-day)

</details>

## Domain Proyek
Dalam bidang perekonomian suatu negara dipengaruhi oleh pasar saham/modal yang didalamnya terdapat proses jual beli daham sesuai dengan harga yang ada dipasar.<br>

Melihat kondisi pasar saham dalam negeri khusus nya BCA setiap harinya mengalami naik turun tidak menentu maka perlu untuk mempelajari data masa lalu dari perbankan, untuk strategi investasi kedepannya. Sehingga Investor dapat melakukan analisis terhadap perusahaan yang akan diinvestasikan. <br> 
sumber = [ANALISIS PREDIKSI HARGA SAHAM SEKTOR PERBANKAN 
MENGGUNAKAN ALGORITMA LONG-SHORT TERMS MEMORY
(LSTM)](http://www.jurnal.upnyk.ac.id/index.php/semnasif/article/view/4135)<br>
Maka perlu dilakukanlah proses pengolahan dan pelatihan data menggunakan suatu algoritma sehingga data tersebut dapat digunakan untuk memprediksi harga saham untuk *next day*. 

## Business Understanding
Untuk menyelesaikan permasalahan itu akan dimulai dengan analisis isi dari data tersebut yang kemudian akan dibuat model dengan menggunakan deep learning LSTM.
### Problem Statements
- Apakah algoritma yang bisa digunakan untuk kasus ini?
- Apakah data yang digunakan memiliki tingkat error yang besar?
- Bagaimana cara menentukan prediksi price close untuk next day menggunakan *time series*?

### Goals
- Proyek ini bertujuan untuk memprediksi harga *close* untuk harga saham BCA, dengan mengimplementasikan *machine learning* didalamnya sehingga lebih mempermudah proses.
- Membentuk model yang optimal dengan data *Time Series*

### Solution statements
Untuk menentukan prediksi *price close next day* algoritma yang digunakan adalah *Deep Learning (convolutional LSTM)*, LSTM digunakan karena dapat mengolah data yang besar dengan urutan yang kompleks (*time series*). LSTM bekerja dengan mempelajari urutan data menggunakan blok memori yang terhubung dengan lapisan neuron.
Sedangkan untuk mengetahui data yang digunakan sesuai dan tidak banyak terdapat error dapat dilakukan dengan exploratory data analysis, dan ketika model sudah selesai training, model akan di evaluasi dengan menggunakan MAE (*Mean Absolute Error*).


## Data Understanding
Data yang digunakan adalah dataset *stock price* dari BCA yang diambil dari tahun 2010 sampai 2020 dari Yahoo Finance <br>
[Data History BCA Stock price](https://finance.yahoo.com/quote/BBCA.JK/history?p=BBCA.JK ) 

Data yang digunakan berjumlah 2731 data.

![images](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/data_awal.png)

Ada beberapa variable dalam data tersebut yaitu:

1. Low : harga terendah dalam pergerakan harga setelah harga dibuka hingga close
2. High: harga tertinggi dalam pergerakan harga, dalam hal ini biasanya ketika data semakin tinggi maka akan ada orang yang membeli
3. Open : harga awal ketika transaksi pertama kali di lakukan pada hari itu. Harga diambil dari harga close hari sebelumnya
4. Close : harga terakhir, harga ini penting karena penting untuk proses anaisys
5. Volume : berapa banyak transaksi yang terjadi, dalam jumlah per-lembar
6. Adj Close : harga akhir/penutupan yang sudah disesuaikan dengan aksi  korporasi, kserti isu ekonomi, politik dll.

Keenam variable data memiliki type float. 

## Data Eksploration 

Eksplorasi data bertujuan untuk mengetahui apa saja jenis data yang ada di dalam dataset yang digunakan 

* Mengecek data yang mengalami mising value atau tidak. <br>
Setelah pengecekan ternyata tidak ada data yang mengalami missing value
* mengecek data yang mengalami duplicate
diketahui bahwa ada sebanyak 30 yang mengalami duplicated

![data_duplicate](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/duplicate.png)

<br> 
Untuk mengatasi itu dilakukan penghapusan data yang mengalami duplikat

* cek Deskripsi statistika dengan menggunakan ```data.describe() ```

![descriptive_statistika](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/desc.png)

Diketahui data Volume memiliki nilai minimal 0 maka cek lokasi tersebut dan penyebab data tersebut bernilai nol

 !['loc_vol_0](https://github.com/melinadwisafitri/BCA_Stock_price/blob/master/images/vol0.png)


Data Volume bernilai 0 dikarenakan pada hari itu tidak terjadi proses transaksi

* Visualisasi data variabel 
    * Visualisasi semua variable dengan histogram<br>![var](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/semua.png)

    * Visualisasi data Close berdasarkan index(rentang taun 2010-2020)<br>![close](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/close.png)

    * Visualisasi data open-close berdasarkan week/minggu dalam data ini ternyata proses dalam satu minggu hanya terjadi 5 kali transaksi saja menggunakan barplot
        * close Open Price 
        Data diambil dengan menentukan nilai gap dari close dan open dengan rumus:

            opencloseweek=(open−close)/close<br>![close_open](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/week.png)

        * High Low Price<br>![low_high](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/high_low.png)

* Cek korelasi antar data untuk mengetahui keterkaitan antar variable 

![corelation_data](images/heatmap.png)
Berdasarkan data korelasi diketahui bahwa data memiliki range antara 0 - 1, variable/features yang mengalami korelasi data paling kecil adalah Volume

* Mengecek apakah data Close terdapat oulier atau tidak

![outlier_close](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/close_box.png)

Setelah dilakukan pengecekan ternyata tidak ada data yang mengalami outlier, maka tidak perlu dilakukan IQR.

## Data Preparation
Setelah data di Eksplorasi data akan disiapkan supaya bisa digunakan untuk membuat model.
* Melakukan split data x dan y yang bertujuan y sebagi feature
split, dengan membuat step = 5 karena disini data yang akan digunakan untuk prediksi diambil per minggu. Kenapa data sibagi hanya 5 step? Karena proses beli dan jual saham dalam seminggu hanya berlangsung dalam 5 hari.
* Melakukan split data train, test, validation 
split data harus dilakukan sebelum pembuatan model karena harus tetap mempertahankan beberapa data yang digunakan untuk proses pengujian.<br>
data yang digunakan adalah data *Close* saja karena ingin memprediksi *close next day*, harga penutupan dari saham BCA. Dengan persentase :
    * Split data menjadi:
    * train = 70% 
    * test =  20%
    * validation =10%

Split data dilakukan tanpa melakukan random dan shuffle data untuk mengurangi perubahan urutan data
Setelah proses split data diketahui jumlah dari masing masing data train 1940, test 540 dan val 216
* Melakukan MinMaxScaler
Tujuan dari penggunaan MinMaxScaler ini untuk menyesuaikan rentang data 0-1 sehingga bisa mengurangi tingkat error, data yang di scaler adalah data train .

Proses dilakukan dengan menggunakan windowed dataset<br>
Menggunakan window Size 10 dan batch size=10(batch_size digunakan menjalankan berapa data yang akan di training untuk setiap epochs misalkan terdapat 1936 data dengan batch size 10 maka 1936/10 = 193 kali untuk setiap epochs)


## Modeling
Model yang digunakan adalah model Convolutional LSTM, model ini digunakan karena dapat menangani data yang banyak dan lebih sederhana dalam implementasiannya, disini menggunakan Conv1D dengan pengaturan stride 2, yang memiliki arti perubahan pergerakan per dua data untuk perhitungannya, dengan 2 layer LSTM.<br>

Atur compile data sebelum proses training, dengan ketentuan loss=mean_squared error, dan optimizer yang digunakan `Adam`, karena `Adam` memiliki tingkat optimizer yang lebih tinggi dan cenderung stabil.

![model](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/model.png)

Kemudian setelah model terbentuk lakukan training data untuk prosesnya dijalankan sebanyak 10 epochs

![plot_training](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/plot_train.png)

Berdasarkan plot diatas data training memiliki nilai mae terendah yaitu 0.015 (1.5%)

## Evaluation Model
* Evaluasi model dilakukan dengan metrik evaluasi, metrik ini berfungsi untuk mengetahui untuk mengukur bagiamana kualitas model yang telah kita buat.

MAE digunakan untuk mengetahui melihat training data gagal/rusak karena outlier. 
![mae](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/mae_rumus.png)

<p align='center'>Didapatkan nilai dari model yaitu : </p>
![model_mae](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/mae.png)

Berdasarkan nilai mae yang didapatkan 1e-12 maka nilai error yang didapatkan kecil. Sehingga model baik digunakan untuk proses prediksi

## Prediction *Next Day*

Data seharusnya
![real_data](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/real_data.png)

Prediction

![prediksi](https://github.com/melinadwisafitri/BCA_Stock_price/raw/master/images/predict.png)

Berdasarkan hasil diatas diketahui prediksi untuk tanggal 4-10-21 adalah 56457.074, padahal seharusnya 34800, model harus diperbaiki lagi. Dengan meningkatkan nilai dari window size atau mengatur unit dari LSTM maupun Conv1D.


