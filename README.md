# Domain Proyek

---

### Latar Belakang

Industry anggur merah menunjukkan peningkatan yang signifikan dalam beberapa tahun terakhir, bahkan akan diproyeksikan mencapai $278.5 miliar pada tahun 2028. Pada saat ini, para pemain di industry anggur merah menggunakan sertifikasi kualitas produk untuk memasarkan produknya, proses ini memakan waktu yang lama dan mahal karena membutuhkan penilaian dari ahli untuk penentuannya. Factor lain penentuan kualitas anggur merah salin dengan seritifikasi adalah uji fisiokimia yang dilakukan di laboraturium dengan mempertimbangkan beberapa kandungan kimia yang ada didalam anggur merah seperti keasaman, pH level, gula, dan lain-lain.

Dengan adanya penilaian kualitas dengan uji fisiokimia, tentunya hal ini bisa lebih bisa di kontrol karena parameter-parameter yang digunakan lebih jelas dan objektif dibandingkan dengan penilaian manusia yang cenderung subjektif.

Proyek ini bertujuan untuk menentukan kandungan kimia apa saja yang dapat mempengaruhi kualitas anggur merah serta memberikan wawasan dalam setiap factor di model penentuan kualitas anggur merah.
#Business Understanding

---

### Problem Statement

Berdasarkan latar belakang di atas, berikut ini batasan masalah yang dapat diselesaikan dengan proyek ini :

- Fitur apa yang paling berpengaruh terhadap kualitas anggur merah?
- Bagaiaman cara memproses data agar dapat dilatih dengan baik oleh model?
- Bagaimana menentukan kualitas anggur merah berdasarkan karakteristik tertentu?

### Goals

- Mengetahui fitur yang paling berpengaruh pada kualitas anggur merah.
- Melakukan persiapan data untuk dapat dilatih oleh model.
- Membuat model machine learning yang dapat menentukan kualitas anggur merah berdasarkan karakteristik tertentu

### Solution Statement

Solusi yang dapat dilakukan sebagai berikut :

- Menganalisis data dengan melakukan univariate analysis dan multivariate analysis. Memahami data juga dapat dilakukan dengan visualisasi. Memahami data dapat membantu untuk mengetahui kolerasi antar fitur dan mendeteksi outlier.
- Membandingkan hasil dari tiga algoritma _machine learning_, yaitu KNN, Random Forest dan Adaboost

---

# Data Understanding & Removing Outlier

![cover image](https://storage.googleapis.com/kaggle-datasets-images/4458/6836/30587db9a40233164f65a4a3f148f40d/dataset-cover.jpg?t=2017-11-12-14-28-34)

Dataset yang digunakan dalam proyek ini merupakan sample data uji fisiokimia dari Portugal utara. Dataset ini bisa didapatkan di [Kaggle: Red Wine Quality](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009).

**Berikut informasi pada dataset:**

- Dataset memiliki format CSV
- Dataset memiliki 1.599 sample dengan 12 fitur
- Dataset memiliki 1 fitur Integer dan 11 fitur decimal

**Variable – variable pada dataset**

- Alcohol: jumlah alcohol didalam wine
- Volatile acidity: adalah asam asetat tinggi dalam anggur yang menyebabkan rasa cuka yang tidak enak
- Sulphates: aditif anggur yang berkontribusi pada kadar SO2 dan bertindak sebagai antimikroba dan antioksidan
- Citric acid: bertindak sebagai pengawet untuk meningkatkan keasaman (jumlah kecil menambah kesegaran dan rasa anggur)
- Total sulfur dioxide: adalah jumlah SO2 bentuk bebas + terikat
- Density: anggur yang lebih manis memiliki kepadatan yang lebih tinggi
- Chlorides: jumlah garam dalam anggur
- Fixed Acidity: adalah asam non-volatil yang tidak mudah menguap
- pH: tingkat keasaman
- Free Sulfur Dioxide: itu mencegah pertumbuhan mikroba dan oksidasi anggur
- Residual sugar: adalah jumlah gula yang tersisa setelah fermentasi berhenti. Kuncinya adalah memiliki keseimbangan yang sempurna antara — manis dan asam (anggur > 45g/ltr manis)

### Univariate Analysis

Univariate Analysis adalah menganalisis setiap fitur secara terpisah.

Karena pada dataset tidak ada data kategorik, maka kita langsung menganalisis sebaran pada setiap fitur numerik dengan melihat histogram masing-masing fiturnya.

![univarate image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/univariete.png)

Mari kita lihat histogram diatas, khususnya variable ‘quality’ yang menjadi target pada data kita.

- Rata-rata kualitas wine berada di angka 5 dan 6
- Bisa dilihat pH level anggur merah selalu berada di angka 3-3.8

### Multivariate Analysis

Multivariate Analysis menunjukkan hubungan antara dua atau lebih fitur dalam data.

**Analisis Fitur Numerik**

![multivariate image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/multivariate.png)

Dari pola sebaran data(titik-titik) pada gambar diatas, kita bisa melihat adanya 3 pola yang terbentuk, yaitu positive, negative dan pola acak yang artinya tidak memiliki korelasi apapun.

Untuk pola positif, kita bisa melihat hubungan yang positif antara critical acid, alcohol dan sulphates. Meskipun anggur merah dengan kandungan alcohol yang lebih tinggi kurang popular, akan tetapi mereka memiliki kualitas yang bagus.

Untuk pola negative, variable volatile acidity, density, total sulfur dioxide dan cholireds memiliki pola negative dengan kualitas. Ini sangat masuk akal karena anggur merah yang kurang manis dan tingkat keasaman yang lebih rendah lebih disukai dalam pengujian kualitas.

Untuk pola acak, residual sugar, pH, dan free sulfur dioxide tidak memiliki kolerasi apapun dengan kualitas.

Agar lebih jelas, mari kita lihat skor korelasi pada gambar dibawah ini.

![korelasi image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/korelasi.png)

Karena skor residual sugar, pH, dan free sulfur dioxide sangat kecil, maka fitur tersebut kita drop.

![drop image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/drop.png)

# Data Preparation

- Train Test Split

Train test split aja proses membagi data menjadi data latih dan data uji. Data latih akan digunakan untuk membangun model, sedangkan data uji akan digunakan untuk menguji performa model. Pada proyek ini dataset setelah outliers di drop sebesar 1179 dibagi menjadi 943 untuk data latih dan 236 untuk data uji.

- Normalization

Algoritma machine learning akan memiliki performa lebih baik dan bekerja lebih cepat jika dimodelkan dengan data seragam yang memiliki skala relatif sama. Salah satu teknik normalisasi yang digunakan pada proyek ini adalah Standarisasi dengan sklearn.preprocessing.StandardScaler.

![standarisation image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/standard.png)

# Modeling

Algoritma Penelitian ini melakukan pemodelan dengan 3 algoritma, yaitu K-Nearest Neighbour, Random Forest, dan

- K-Nearest Neighbour K-Nearest Neighbour bekerja dengan membandingkan jarak satu sampel ke sampel pelatihan lain dengan memilih sejumlah k tetangga terdekat. Proyek ini menggunakan sklearn.neighbors.KNeighborsRegressor dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :

  - n_neighbors = Jumlah k tetangga tedekat.

- Random Forest Algoritma random forest adalah teknik dalam machine learning dengan metode ensemble. Teknik ini beroperasi dengan membangun banyak decision tree pada waktu pelatihan. Proyek ini menggunakan sklearn.ensemble.RandomForestRegressor dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :

  - n_estimators = Jumlah maksimum estimator di mana boosting dihentikan.
  - max_depth = Kedalaman maksimum setiap tree.
  - random_state = Mengontrol seed acak yang diberikan pada setiap base_estimator pada setiap iterasi boosting.

- Adaboost AdaBoost juga disebut Adaptive Boosting adalah teknik dalam machine learning dengan metode ensemble. Algoritma yang paling umum digunakan dengan AdaBoost adalah pohon keputusan (decision trees) satu tingkat yang berarti memiliki pohon Keputusan dengan hanya 1 split. Pohon-pohon ini juga disebut Decision Stumps. Algoritma ini bertujuan untuk meningkatkan performa atau akurasi prediksi dengan cara menggabungkan beberapa model sederhana dan dianggap lemah (weak learners) secara berurutan sehingga membentuk suatu model yang kuat (strong ensemble learner). Proyek ini menggunakan sklearn.ensemble.AdaBoostRegressor dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :

  - n_estimators = Jumlah maksimum estimator di mana boosting dihentikan.
  - learning_rate = Learning rate memperkuat kontribusi setiap regressor.
  - random_state = Mengontrol seed acak yang diberikan pada setiap base_estimator pada setiap iterasi boosting.

# Evaluation

Metrik evaluasi yang digunakan pada proyek ini adalah akurasi dan mean squared error (MSE). Akurasi menentukan tingkat kemiripan antara hasil prediksi dengan nilai yang sebenarnya (y_test). Mean squared error (MSE) mengukur error dalam model statistik dengan cara menghitung rata-rata error dari kuadrat hasil aktual dikurang hasil prediksi. Berikut formulan MSE :

![MSE image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/mse.png)

Keterangan:

N = jumlah dataset
yi = nilai sebenarnya
y_pred = nilai prediksi

Berikut ini Hasil evaluasi pada data latih dan data test setiap alogirtme.

![evaluasi image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/evaluasi.png)

Untuk memudahkan, mari kita plot metrik tersebut dengan bar chart.

![akurasi image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/akurasi.png)

Dari gambar diatas kitab isa melihat bahwa model Random Forest (RF) memiliki nilai error paling kecil dengan tingkat akurasi yang baik, disusul dengan model Boosting. Sedangkan model KNN memiliki nilai error yang tinggi dan tingkat akurasi yang kurang baik.

Untuk mengujinya, mari kita buat prediksi menggunakan beberapa harga dari data test.

![result image](https://raw.githubusercontent.com/hadiselamethariyanto/red-wine-quality-machine-learning/main/images/result.png)

Pada gambar diatas terlihat bahwa model Random Forest (RF) memberikan nilai yang sama dengan nilai aslinya, sedangkan model Boosting memberikan nilai dengan selisih sedikit, dan untuk model KNN memiliki nilai yang cukup jauh berbeda.
