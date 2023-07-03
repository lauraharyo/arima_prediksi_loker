# Tim FaLS
* Fitra Febriansyah
* Laura Haryo
* Stanislaus Jiwandana Pinasthika

# Deskripsi
Code ini digunakan untuk "Prediksi Ketersediaan Lowongan Kerja Menggunakan ARIMA", sebagai hasil karya partisipasi Big Data Hackathon 2021.

## Metode
Metode prediksi yang akan digunakan adalah ARIMA (Autoregressive Integrated Moving Average). Langkah-langkah yang diperlukan adalah sebagai berikut.
•	Periksa apakah data stasioner.
•	Jika data belum stasioner, maka dilakukan differencing hingga data menjadi stasioner. Jika data sudah stasioner, maka tidak perlu dilakukan differencing data.
•	Identifikasi metode menggunakan autokorelasi dan autokorelasi parsial.
•	Interpretasi dan estimasi metode menggunakan data sebelumnya (past) dengan metode Cramer.
•	Testing untuk menemukan metode yang bisa diaplikasikan untuk prediksi.
•	Aplikasi metode yang telah ditemukan dari testing untuk memprediksi nilai periodik.

## Dataset
Data yang digunakan dalam penelitian ini adalah data sebaran lowongan pekerjaan dari tahun 2018 hingga 2020 yang diperoleh dari sebuah sumber publik. Data ini mencakup beberapa informasi seperti lokasi, kategori pekerjaan, jenis kelamin yang dibutuhkan, nama perusahaan, dan gaji yang ditawarkan. Untuk menjaga kerahasiaan, detail dan file dataset telah dianonimkan. Proses preprocessing dilakukan untuk mengisi data yang kosong dan mengubah fitur kategorikal (kategori, industri, dan pendidikan) menjadi tipe numerik.

## Pembahasan

### Preprocessing 
Pada proses ini dilakukan ekstraksi fitur yaitu fitur jenis kelamin dan jenis industri. Fitur jenis kelamin diambil dari kolom requirement. Pada kolom requirement tentu saja tercantum persyaratan jenis kelamin, namun, beberapa lowongan tidak mencantumkannya. Untuk kasus ini, diasumsikan lowongan tersebut tidak mempermasalahkan jenis kelamin calon pegawai, sehingga perlu ditambah jenis kelamin “both”. Data jenis kelamin dijadikan angka dengan 0 adalah berjenis kelamin laki-laki, 1 untuk perempuan, dan 2 untuk perusahaan yang tidak mensyaratkan jenis kelamin.

Jenis industri diperhitungkan dalam metode ini. Data dikelompokkan menjadi 53 jenis industri. dari 53 jenis tersebut, diambil 5 teratas dengan jumlah data terbanyak.

Ukuran perusahaan juga diambil sebagai fitur dan dikelompokkan menjadi delapan golongan. Ukuran perusahaan diukur dari banyak karyawan yang dimiliki. Perusahaan sangat kecil jika hanya memiliki 1 hingga 10 karyawan, sementara perusahaan dinilai sangat besar apabila memiliki 10.000 lebih karyawan.

### Hasil ARIMA
 Bagian ini menjelaskan hasil dari prediksi ARIMA. Untuk data secara keseluruhan memiliki nilai p-Value sebesar 0.007574, sehingga memenuhi syarat Dicker-Fuller. Hasil prediksi lowongan kerja secara keseluruhan akan menurun hingga Juli 2022 (seperti pada Gambar). Prediksi ini memiliki nilai RSS -1.6647. Selain menentukan prediksi secara keseluruhan, data dikumpulkan berdasarkan jenis industri, jenis kelamin calon pekerja yang diinginkan, dan ukuran besarnya industri.

 ![arima_full](gambar\arima_full.png)

 #### Berdasarkan Jenis Industri
 Data dikelompokkan berdasarkan jenis industri yang menyediakan lowongan tersebut. Terdapat 53 jenis industri dan satu kelompok tidak mencantumkan jenis industri. Untuk penelitian ini, diambil 5 jenis industri yang memiliki data terbanyak yaitu: perbankan / lembaga pembiayaan, konsultan legal riset, IT dan telekomunikasi, perdagangan komoditas, dan retail distribusi. Persebaran 53 jenis industri dapat dilihat pada gambar berikut.

 ![sebaran](gambar\sebaran_industri.png)

 Berdasarkan perhitungan Dicker-Fuller, hanya data pada industri konsultan legal riset dan IT telekomunikasi yang tidak stasioner. Data dianggap stasioner jika nilai p-value dibawah 0,05 atau ketika nilai critical value (5%) lebih besar daripada test statistic. Hampir semua nilai memiliki nilai RSS yang kecil dan cenderung negatif. Kategori jenis industri konsultan legal riset memiliki atribut stasioner yang ideal dan memenuhi syarat Dicker-Fuller.   

 ![pred_legal](gambar\pred_legal.png)

Jika data stasioner, maka prediksi dapat semakin akurat. Pada gambar prediksi konsultan legal riset di atas, jumlah lowongan kerja mengalami kenaikan namun menjelang awal 2022 akan menurun sedikit, namun akan menjadi stabil. 

#### Berdasarkan kriteria jenis kelamin
Data lowongan pekerjaan dikelompokkan berdasarkan kebutuhannya pada jenis kelamin. Untuk data lowongan yang tidak mencantumkan syarat jenis kelamin akan dianggap dapat diisi oleh kandidat laki-laki maupun perempuan. Nilai RSS untuk kategori laki-laki dan perempuan juga memiliki nilai RSS yang lebih kecil dibandingkan nilai RSS untuk kategori “keduanya”.

 ![pred_laki](gambar\pred_laki.png)

Berdasarkan jenis kelamin, untuk kategori “laki-laki” (gambar atas) dan “perempuan” (gambar bawah) memiliki tren penurunan, namun tidak drastis dan diprediksi stabil hingga Juli 2022. Prediksi berdasarkan kategori “perempuan” lebih dinamis daripada “laki-laki”.

 ![pred_perempuan](gambar\pred_perempuan.png)

#### Berdasarkan jumlah karyawan
Untuk kategori ini dibagi menjadi delapan kelompok. Industri terkecil memiliki 1-10 karyawan sedangkan, industri besar memiliki lebih dari 10.000 karyawan. Banyaknya kelompok membuat sebaran data menjadi kurang merata sehingga 5 dari 8 kelompok bersifat tidak stasioner. 

Hanya kategori 1-10, 11-50, 51-200, 5001-10000 yang memenuhi syarat Dicker-Fuller yaitu memiliki p-Value kurang dari 0.05. Selain itu, keempat kategori tersebut memiliki kecenderungan untuk bernilai negatif pada RSS, terutama untuk kategori 5001-10000 yang memiliki nilai RSS paling rendah dibanding yang kategori lain, yaitu -8.1001. Gambar grafik prediksi kategori-kategori tersebut berturut-turut dapat dilihat pada gambar-gambar berikut. 

 ![pred_category1](gambar\pred_category1.png)
 ![pred_category2](gambar\pred_category2.png)
 ![pred_category3](gambar\pred_category3.png)
 ![pred_category4](gambar\pred_category4.png)
 
Keempat prediksi tersebut mengalami tren yang berbeda dan memiliki kecenderungan untuk tidak mengalami fluktuasi. Penurunan lebih tajam dialami oleh perusahaan yang memiliki 5001-10000. Perusahaan besar lebih memungkinkan mengalami digitalisasi dan otomatisasi dalam operasi bisnisnya sehingga permintaan pegawai dapat berkurang.

### Kesimpulan

ARIMA dapat digunakan untuk memprediksi jumlah lowongan pekerjaan yang akan tersedia 12 bulan ke depan pada periode bulan Juli 2021 hingga bulan Juli 2022. Data pada setiap kolom dibagi menjadi beberapa kategori seperti jenis kelamin, jenis industri, dan jumlah karyawan, sehingga data banyak yang tidak memenuhi syarat stasioner. Jika data yang tidak stasioner diprediksikan, maka hasil prediksi kurang akurat dan cenderung statis.

Terdapat hasil yang tidak memenuhi standar dari penggunaan model ARIMA dikarenakan data yang tidak mencukupi sehingga prediksi hanya berhasil dilakukan tidak pada semua kolom dalam setiap kategori. Semakin banyak data yang digunakan maka hasilnya akan bisa maksimal dikarenakan apabila ditemukan data yang tidak stasioner maka dapat dilakukan pengurangan data seperti menggunakan moving average, eksponensial, ataupun yang lainnya sehingga data yang baru mendapatkan hasil yang diinginkan.
