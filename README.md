# Tugas 6 - MLlib model to PMML dan Spark Compiled Model Predictor menggunakan KNIME
Tugas 6 adalah mencoba materi yang telah diberikan untuk menjalankan MLlib model to PMML dan Spark Compiled Model Predictor pada KNIME

## 1. MLlib model to PMML
Berikut adalah workflow yang akan dijalankan:

![](/screenshoot/1/1.PNG)


### 1.1 Business Understanding
Data test yang digunakan adalah data bunga Iris, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan klasifikasi terhadap bunga Iris berdasarkan ciri-cirinya.


### 1.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi spesifikasi dari sebuah bunga, baik dari besar kelopak, atau bunganya, serta class dari bunga tersebut.

Terdapat 5 kolom data dengan :
- sepal length, yaitu angka yang menunjukkan panjang sepal
- sepal width, yaitu angka yang menunjukkan lebar sepal
- petal length, yaitu angka yang menunjukkan panjang petal
- petal width, yaitu angka yang menunjukkan lebar petal
- class, merupakan kelas dari bunga tersebut


### 1.3 Data Preparation

![](/tugas_6_knime_spark/screenshoot/1/2.PNG)

Pada data preparation saya menyiapkan data yang dibutuhkan, yaitu data bunga Iris. Data ini akan kita konversi ke table knime, dan kita simpan di local Spark. Tabelnya sebagai berikut:

![](/tugas_6_knime_spark/screenshoot/1/3.PNG)

### 1.4 Modeling
![](/tugas_6_knime_spark/screenshoot/1/4.PNG)

Selanjutnya adalah modelling, di tahap ini adalah melakukan training pada data tadi, dengan menggunakan spark k-means. Berikut konfigurasinya:

![](/tugas_6_knime_spark/screenshoot/1/5.PNG)

Dapat dilihat bahwa, 4 kolom akan di train disana, yaitu sepal length, sepal width, petal length, petal width. Hasil dari train tersebut nantinya akan di rubah dalam bentuk PMML yang kemudian dilakukan convert dalam bentuk Java agar data bisa digunakan di proses selanjutnya.


### 1.5 Evaluation
![](/tugas_6_knime_spark/screenshoot/1/6.PNG)

Pada tahap ini adalah evaluasi, yaitu melakukan prediksi dari data tadi yang diconvert ke java(data hasil train) dengan data test. 

![](/tugas_6_knime_spark/screenshoot/1/7.PNG)

Tabel hasil prediksi adalah:

![](/tugas_6_knime_spark/screenshoot/1/8.PNG)

Dihasilkan score:

![](/tugas_6_knime_spark/screenshoot/1/9.PNG)


### 1.6 Deployment
![](/tugas_6_knime_spark/screenshoot/1/10.PNG)

Dari hasil prediksi di tahap sebelumnya, maka didapatkan sebuah data untuk menebak suatu bunga masuk klasifikasi mana.

Lalu, deploy dan berikan inputan berbentuk json, yang nantinya akan diketahui klasifikasi bunga tersebut berdasarkan data tadi. Inputan tersebut dikonversi ke tabel dan diprediksi. Lalu hasil dikembalikan dalam json lagi.

![](/tugas_6_knime_spark/screenshoot/1/11.PNG)

![](/tugas_6_knime_spark/screenshoot/1/12.PNG)

![](/tugas_6_knime_spark/screenshoot/1/13.PNG)

![](/tugas_6_knime_spark/screenshoot/1/14.PNG)

Didapatkan dari ciri bunga Iris yang diinput tadi, bahwa bunga tersebut merupakan klasifikasi bunga Iris cluster_0


## 2. Spark Compiled Model Predictor
Workflow kedua yang akan saya jalankan dan jelaskan adalah:

![](/tugas_6_knime_spark/screenshoot/2/1.PNG)


### 2.1 Business Understanding
Data test yang digunakan adalah data bunga Iris, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan klasifikasi terhadap bunga Iris berdasarkan ciri-cirinya.


### 2.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi spesifikasi dari sebuah bunga, baik dari besar kelopak, atau bunganya, serta class dari bunga tersebut.

Terdapat 5 kolom data dengan :
- sepal length, yaitu angka yang menunjukkan panjang sepal
- sepal width, yaitu angka yang menunjukkan lebar sepal
- petal length, yaitu angka yang menunjukkan panjang petal
- petal width, yaitu angka yang menunjukkan lebar petal
- class, merupakan kelas dari bunga tersebut


### 2.3 Data Preparation
Kita siapkan data yang akan diproses kedalam PMML Cell, workflow yang akan dijalankan adalah

![](/tugas_6_knime_spark/screenshoot/2/2.PNG)

Melakukan decision tree learner dengan konfigurasi:

![](/tugas_6_knime_spark/screenshoot/2/3.PNG)

Berikut hasilnya:

![](/tugas_6_knime_spark/screenshoot/2/4.PNG)

Kemudian dilakuakn MLP learner dengan class sebagai class column:

![](/tugas_6_knime_spark/screenshoot/2/5.PNG)

Hasil errorprop nya adalah sebagai berikut:

![](/tugas_6_knime_spark/screenshoot/2/6.PNG)


### 2.4 Modeling
![](/tugas_6_knime_spark/screenshoot/2/7.PNG)

Tahap selanjutnya adalah pembuatan model. Pada tahap ini dilakukan penggabungan(append) dari 2 table cell sebelumnya, yang selanjutnya dari hasil penggabungan tersebut dirubah dalam bentuk PMML Ensemble untuk dilakukan compiler.


### 2.5 Evaluation
![](/tugas_6_knime_spark/screenshoot/2/8.PNG)

Tahap ini adalah melakukan prediksi dari data sebelumnya dengan sebuah test data tambahan. Berikut data test tambahannya:

![](/tugas_6_knime_spark/screenshoot/2/9.PNG)

Setelah itu dilakukan prediksi secara massal didalam hadoop dan memiliki hasil:

![](/tugas_6_knime_spark/screenshoot/2/10.PNG)

Data yang diprediksi persis seperti klasifikasi yang sudah ada, yang artinya prediksi cukup akurat


### 2.6 Deployment
![](/tugas_6_knime_spark/screenshoot/2/11.PNG)

Setelah mendapatkan hasil yang cukup akurat, kita hitung score akurasinya. Kita rubah data pada spark menjadi dalam bentuk table yang kemudian dilakukan scorer yang menghasilkan:

![](/tugas_6_knime_spark/screenshoot/2/12.PNG)

Didapatkan 25 iris dan versi-color tanpa adanya klasifikasi yang salah, sedangkan pada virginica terdapat kesalahan prediksi sebesar 4, yaitu dimana 4 data dibaca sebagai versi-color. Akurasi dari hasil prediksi adalah sebesar 94,667% dengan error 5.333%.