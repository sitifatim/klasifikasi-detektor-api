## **Domain Proyek**
![smoke-detector-500x500](https://user-images.githubusercontent.com/99231159/197402111-8e712774-1fde-4906-83a5-0799fa320f45.jpg)

Pendeteksi asap adalah sebuah alat yang dapat mendeteksi atau merasakan apabila adanya asap di sekitar. Alat ini biasa digunakan untuk pendeteksi kebakaran atau Fire Alarm Detection. Penggunaan alarm api telah membantu banyak kasus kebakaran yang telah terjadi di dunia [1]. Dengan perkembangan teknologi yang meningkat, hingga saat ini telah banyak inovasi tentang perkembangan alarm kebakaran yaitu dengan menggabungkan kecerdasan buatan pada klasifikasi data-data yang ditangkap oleh sensor asap. Sensor asap mendeteksi beberapa parameter untuk pengindikasian terdapatnya asap atau api di sekitar seperti *humidity, temperature, raw H2,* dan lainnya. Data-data yang dikumpulkan seringkali ratusan hingga puluhan ribu yang mana tidak efektif jika dilakukan pengamatan manual. Oleh karena itu diperlukannya sebuah klasifikasi untuk mengklasifikasikan parameter-parameter tersebut agar dapat membantu pendeteksian kebakaran. Kecerdasan buatan memiliki keuntungan yaitu dapat menangkap pola pada data dengan jumlah besar [2]. Oleh karena klasifikasi parameter kebakaran perlu dibuat dengan kecerdasan buatan.

## **Business Understanding**
### Problem Statements
* Bagaimana cara mengolah ribuan data parameter yang dihasilkan sensor?
* Metode apa yang akan digunakan untuk pengolahan parameter tersebut?

### Goals
* Pengolah data parameter dapat dilakukan dengan pembuatan klasifikasi parameter untuk menghasilkan kelas terjadinya kebakaran *(Fire)* atau tidak terjadinya kebakaran *(No Fire)*
* Klasifikasi akan dibuat dengan metode dari kecerdasan buatan, yaitu *machine learning* dengan algoritma yang akan digunakan ialah *K-Nearest Neighbor, Support Vector Machine,* dan *Random Forest*.

## **Data Understanding**
Dataset yang akan digunakan ialah dataset "Smoke Detection Dataset" yang dapat diakses melalui situs Kaggle pada link [ini](https://www.kaggle.com/datasets/deepcontractor/smoke-detection-dataset?datasetId=2424784) yang mana dibuat oleh Stefan Blattman pada proyeknya berjudul [Real-time Smoke Detection with AI-based Sensor Fusion](https://www.hackster.io/stefanblattmann/real-time-smoke-detection-with-ai-based-sensor-fusion-1086e6). Dataset tersebut merupakan data-data yang dikumpulkan oleh sensor detektor asap. Detektor asap adalah perangkat yang mendeteksi asap, biasanya sebagai indikator kebakaran dan berbentuk seperti piringan berdiameter sekitar 150 milimeter (6 inci) dan tebal 25 milimeter (1 inci), tetapi bentuk dan ukurannya bervariasi. Jumlah data yang terkumpul kurang lebih 60.000 data per 1 Hz *sample rate* dengan dilengkapi pula waktu tera UTC agar memudahkan *tracking* data. Beberapa parameter atau variabel yang terkumpul diantaranya adalah sebagai berikut:
* Fire Alarm adalah fitur target, dengan kode 1 berarti menandakan ada api
* Temperature adalah suhu dari hasil pengukuran dalam satuan C
* Humidity adalah kelembaban dalam satuan %
* TVOC adalah Total Volatile Organic Compounds dalam satuan per billions 
* eCO2 adalah konsentrasi CO2 dengan memiliki perhitungan nilai yang berbeda dari TVCO
* Raw H2 adalah hidrogen mentah yang tidak terkompensasi dari bias atau temperatur
* Raw Ethanol adalah gas ethanol mentah
* Pressure adalah tekanan udara (Air Pressure)
* PM 1.0 adalah ukuran partikulat dengan range nilai ukur < 1.0 µm 
* PM 2.5 adalah ukuran partikulat dengan range ukur 1.0 µm < 2.5 µm
* CNT adalah sampel penghitungan
* UTC adalah timestamp untuk detik UTC
* NC0.5/NC1.0 and NC2.5 adalah besaran konsentrasi partikulat yang berbeda dari PM karena NC memberikan nilai aktual partikel udara. NC memiliki range ukuran < 0.5 µm (NC0.5); 0.5 µm < 1.0 µm (NC1.0); dan 1.0 µm < 2.5 µm (NC2.5);


## **Data Preparation**
Preparasi data yang dilakukan mencakupi tiga tahap yaitu:
#### Data Cleaning
Data Cleaning dimaksudkan untuk mengecek apakah dataset yang dipakai memiliki nilai kosong (null value) atau tidak. Pada dataset Fire Alarm Detection ini tidak terindikasi adanya nilai Null yang dapat dilihat pada gambar 1. 
   
   ![2022-10-23-21-25-colab research google com](https://user-images.githubusercontent.com/99231159/197401789-24caaa2b-d9d6-4910-ad09-190f160a1215.png)
   
   Berdasarkan gambar 1, maka data tidak perlu dilakukan pembersihan karena tidak ada data yang bernilai null.
#### Feature Selection
Pada bagian ini dilakukan seleksi fitur dimana terdapat fitur inputan dan fitur target. Fitur inputan ialah terdiri dari kolom 2 hingga 15 adalah data numerik dari parameter kebakaran. Kolom 16 adalah fitur target yaitu kolom label Fire Alarm, yang mana disimbolkan 1 dan 0. 1 berartikan ada api *(Fire)* dan 0 berartikan tidak ada api *(No Fire)*
#### Normalization
Data parameter kebakaran akan dinormalisasikan terlebih dahulu. Hal ini dilakukan agar data semua ada pada range nilai yang sama. Metode normalisasi yang digunakan yaitu Standar Scaler dimana metode ini bekerja menormalisasikan data sehingga data akan memiliki nilai rata-rata 0 dan variansi 1. 

## **Modeling**
Tahap ini melakukan modeling pada data. Algoritma yang digunakan yaitu K-Nearest Neighbor, Support Vector Machine, Random Forest. Tiap algoritma memiliki parameter-parameter tersendiri yang dapat diatur sendiri nilainya. Pengaturan parameter tersebut beserta keterangannya dapat dilihat pada tabel 1. 

Tabel 1. Parameter Algoritma
| Nama Algoritma        | Parameter       | Nilai | Keterangan          |
|:---------------------:|:---------------:|:-----:|:--------------------|
| K-Nearest Neighbor    | K               | 43    | Berfungsi untuk menentukan nilai tetangga data baru sehingga mempermudah pengklasifikasian |
| Support Vector Machine| C               | 1      | Berfungsi untuk pengoptimalan untuk menghindari misklasifikasi di setiap sampel dalam dataset training |
|                       | Kernel          | RBF   | Berfungsi untuk mengatur data non-linier menjadi linier |
| Random Forest         | n_estimators    | 100   | Berfungsi untuk menentukan jumlah trees atau pohon yang digunakan untuk memnentukan keputusan|
|                       | n_jobs          | -1    |Berfungsi untuk menetukan berapa proses atau pekerjaan yang dapat dilakukan pada saat trees bekerja |
|                       | radom_state     | 30    | Berfungsi untuk menentukan seberapa acak data training ketika proses training model |


Setelah dilakukan training model, hasil akurasi terbaik adalah diperoleh oleh Random Forest dengan tingkat akurasi 100%.

## **Evaluation**
Tahap evaluasi yang dilakukan yaitu ada dua tahap sebgai berikut:
#### Perbandingan Akurasi
Perbandingan akurasi K-Nearest Neighbor, Support Vector Machine, Random Forest didapatkan bahwa Random Forest memiliki akurasi yang tertinggi sebesar 100%. Apapun hasil perbandingan ketiga akurasi model tersebut dapat dilihat pada gambar 2 berikut.
   
   ![2022-10-23-22-09-colab research google com](https://user-images.githubusercontent.com/99231159/197401825-d925a5b5-b754-4f6c-b8f8-31746a97a8ed.png)
   
   Gambar 2. Perbandingan Akurasi Model
   
#### Perbandingan MAE (Mean Absoluter Error)
MAE merupakan teknik evaluasi yang dapat digunakan untuk melihat seberapa baik model bekerja. Semakin kecil nilai MAE, maka akan semakin kecil pula kemungkina model melakukan kesalahan saat melakukan prediksi. Hasil perbandingan algoritma K-Nearest Neighbor, Support Vector Machine, Random Forest dapat dilihat pada gambar 3. Dari gambar 3, dapat diihat bahwasanya Random Forest memiliki MAE 0 yang mana artinya tidak memiliki kesalahan prediksi dibandingkan kedua algoritma yang lain. 
   
   ![2022-10-23-22-22-colab research google com](https://user-images.githubusercontent.com/99231159/197401850-c1598bb9-3956-4077-a2e0-90ab6d02258e.png)
   
   Gambar 3. Perbandingan MAE Model

Dari perbandingan yang sudah dilakukan dapat dilihat bahwa algoritma Random Forest merupakan algoritma terbaik untuk mengklasifikasikan dataset Fire Alarm karena memiliki akurasi 100% dibandingkan Support Vector Machine dan K-Nearest Neighbor. Algoritma terbaik kedua yaitu Support Vector Machine karena meskipun memiliki nilai akurasi yang sama seperti K-Nearest Neighbor, akan tetapi ketika dilihat dari nilai MAE kedua algoritma tersebut, dapat dilihat bahwa nilai MAE SVM lebih kecil dibandingkan KNN sehingga SVM bisa dikatakan lebih baik dari KNN. Kesimpulan dari perbandingan algoritma ini yaitu tingkat akurasi terbaik ialah:
* Random Forest
* Support Vector Machine
* K-Nearest Neighbor

## **References**
[1] Stanley Gilbert, "Estimating Smoke Detector Effectiveness and Utilization in Homes", National Institute of Standards and Technology, 2018.
[2] Dr. Suryanto, "Data Mining untuk Klasifikasi dan Klasterisasi Data", Informatika Bandung, 2017. 
