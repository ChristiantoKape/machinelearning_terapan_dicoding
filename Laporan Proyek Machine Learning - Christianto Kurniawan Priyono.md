# Laporan Proyek Machine Learning - Christianto Kurniawan Priyono

## Domain Proyek
Fenomena *churn* pelanggan dalam industri *e-commerce*, yang mencerminkan kehilangan pelanggan setelah transaksi atau pengalaman negatif, telah menjadi permasalahan serius yang mempengaruhi perusahaan di seluruh dunia. Dampaknya terlihat melalui penurunan pendapatan yang signifikan, dan penelitian menunjukkan bahwa peningkatan retensi pelanggan sebesar 5% dapat menghasilkan kenaikan keuntungan yang substansial. Persaingan yang sengit di pasar global *e-commerce* memperkuat pentingnya menjaga pelanggan dengan fokus pada peningkatan pengalaman dan retensi. Untuk mengatasi tantangan ini, perusahaan *e-commerce* semakin mengandalkan teknologi seperti machine learning untuk membangun model prediksi yang mengidentifikasi pelanggan beresiko *churn*, memungkinkan mereka mengembangkan strategi retensi yang efektif berdasarkan data pelanggan historis, transaksi, dan umpan balik *(feedback)*. Pemahaman yang mendalam tentang *churn* dan respons proaktif menjadi kunci bagi perusahaan *e-commerce* untuk menjaga keberlanjutan dan kompetitivitas di pasar yang terus berkembang.

## Business Understanding
### Problem Statements
Tingkat *churn* yang tinggi dalam perusahaan *E-Commerce* adalah tanda kegagalan dalam menjaga pelanggan, karena investasi untuk mendapatkan pelanggan baru lebih besar dibandingkan dengan menjaga pelanggan yang sudah ada. Contohnya, jika perusahaan menghabiskan $100 per bulan untuk mendapatkan pelanggan baru dengan biaya per pelanggan $50 per bulan, perusahaan perlu memastikan minimal 2 pelanggan tetap bertahan setiap bulan untuk menutupi biaya tersebut. 
Berdasarkan project background diatas, Bagaimana perusahaan dapat secara efektif mengidentifikasi pelanggan yang beresiko churn dengan menggunakan data historis, transaksi dan umpan balik *(feedback)* pelanggan?

### Goals
Dalam rangka mengatasi permasalahan *churn* pelanggan. Perusahaan memiliki tujuan yaitu mengurangi tingkat *churn* pelanggan dalam bisnis *e-commerce* menjadi sekecil mungkin, dengan fokus pada menciptakan strategi retensi yang efektif.

### Solution statements

Untuk mengatasi permasalahan *churn* pelanggan dalam bisnis *e-commerce* dan mencapai tujuan yang telah ditetapkan. Berikut langkah-langkah dalam *solution statement*:
- Pengumpulan data yang relevan
    Mengumpulkan dan mengintegrasikan data yang relevan dan berkualitas tinggi untuk digunakan sebagai dasar untuk analisis dan prediksi churn
- Pengembangan model *machine learning*.
    Menggunakan algoritma *machine learning* seperti *KNeighborsClassifier*, *Random Forest* dll untuk mengembangkan model prediksi *churn* pada pelanggan masa depan.

## Data Understanding

*Dataset Source*: https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction  

### Variabel-variabel pada E-Customer Churn Dataset sebagai berikut:
| Attribut       | Tipe Data       | Deskripsi  |
|------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
CustomerID              | Int | ID Pelanggan	|
Churn                   | Int | Berhenti menjadi pelanggan. 0 : Tidak Churn, 1 : Churn |
Tenure                  | Float |  Lama waktu di mana seorang pelanggan telah bertransaksi |
PreferredLoginDevice    | Text | Perangkat login pelanggan |
CityTier                | Int | Tingkatan Kota |
WarehouseToHome         | Float | Jarak antara penyimpanan dan rumah pelanggan	 |
PreferredPaymentMode    | Text | Metode pembayaran pilihan pelanggan |
Gender                  | Text    | Jenis Kelamin Pelanggan |
HourSpendOnApp          | Float | Jumlah jam yang dihabiskan di aplikasi mobile atau web |
NumberOfDeviceRegistered | Int | Jumlah perangkat yang terdaftar  |
PreferedOrderCat   | Object | Kategori pesanan pilihan pelanggan pada bulan lalu |
SatisfactionScore   | Int | Tingkat kepuasan pelanggan dalam pelayanan  |
MaritalStatus   | Object | Status Pernikahan pelanggan |
NumberOfAddress   | Int | Jumlah alamat pelanggan |
Complain   | Int | 	Banyaknya keluhan yang diajukan |
OrderAmountHikeFromlastYear  | Float | Kenaikan Jumlah Pesanan Dari Tahun Lalu |
CouponUsed   | Float | Total kupon yang telah terpakai pada bulan lalu |
OrderCount   | Float | Jumlah pesanan pada bulan lalu |
DaySinceLastOrder | Float | Hari sejak pesanan terakhir oleh pelanggan |
CashbackAmount | Float | Rata-rata cashback tiap bulan |

**Exploratory Data Analysis**:
- Mengecek Data Unik
    Pada proses ini, ditemukan persamaan values pada beberapa kolom yaitu:
    **PreferredOrderCat** yaitu values *Mobile* dan *Mobile phone*
    **PreferredPaymenntMode** yaitu values CC dan *Credit Card* serta COD dan *Cash on Delivery*
    **PreferredLoginDevice** yaitu values *Mobile Phone* dan *Phone*.
- Mengecek Korelasi
    ![Correlation](https://i.ibb.co/FK8Gf4p/Screenshot-2023-09-08-163148.png)
    Ditemukan *positive correlation* yang kuat antara kolom *OrderCount* dengan *CouponUsed* serta *negative correlation* yang kuat antara kolom *Tenure* dan *Churn*. Itu berarti semakin lama seorang menjadi pelanggan semakin rendah juga mereka melakukan *churn*
- Menangani Kolom
    Pada proses ini, dilakukan menghapus kolom *Customer_ID*
- Data duplikat
    Ditemukan *duplicate value* sebanyak 557 data atau 9.89% dari total dataset
- Data Kosong
    Terdapat *missing value* pada beberapa kolom yaitu ***Tenure***, ***WarehouseToHome***, ***HourSpendOnApp***, ***OrderAmountHikeFromlastYear***, ***CouponUsed***, ***OrderCount***, dan ***DaySinceLastOrder***
    perlakuannya dilakukan penghapusan *missing value* tersebut karena hanya sebesar 4.5$ - 5.4% dari keseluruhan dataset
- *Outliers*
    Metode yang digunakan dalam *handling outlier* adalah *IQR*, *IQR* adalah ukuran statistik yang mengukur sebaran data di antara kuartil pertama (Q1) dan kuartil ketiga (Q3) dalam distribusi data.
- *Unvariate Analysis*
    - *Categorical Feature*
        - Fitur PreferredLoginDevice
          ![PrefferedLoginDevice](https://i.ibb.co/HPQvhpp/download.png)
          Pada fitur ini, ditemukan bahwa pelanggan mayoritas menggunakan ponsel sebagai perangkat utama mereka login daripada menggunakan komputer
        - Fitur PreferredPaymentMode
          ![PrefferedPaymentMode](https://i.ibb.co/gM2t3nN/download-1.png)
          Metode pembayaran yang paling dominan adalah *"Debit Card"* sekitar 41.1% dari total pelanggan memilih metode ini. *"Credit Card"* juga cukup populer, digunakan oleh sekitar 30.1% pelanggan. Sementara itu, metode pembayaran lain seperti *"E-Wallet"*, *"Cash on Delivery"*, dan *"UPI"* digunakan oleh persentase pelanggan yang lebih kecil, masing-masing sekitar 12.1%, 9.0%, 7.7%
        - Fitur Gender
          ![Gender](https://i.ibb.co/zS6T8kq/download-2.png)
            Terlihat bahwa mayoritas pelanggan, sekitar 60.7%, adalah pelanggan dengan jenis kelamin "Male" (pria), sementara sekitar 39.3% lainnya adalah pelanggan dengan jenis kelamin "Female" (wanita).
        - Fitur PreferredOrderCat
          ![PrefferedOrderCat](https://i.ibb.co/yFbkqDk/download-3.png)
            Terlihat bahwa mayoritas pelanggan, sekitar 50.3%, lebih memilih untuk berbelanja dalam kategori *"Laptop & Accessory"* (laptop dan aksesorinya). Sementara itu, sekitar 41.1% pelanggan lebih suka berbelanja dalam kategori *"Mobile Phone"* (telepon seluler), dan sekitar 8.7% lainnya memilih kategori *"Fashion"* (mode).
        - Fitur MaritalStatus
            ![MaritalStatus](https://i.ibb.co/WDd7xtz/download-4.png)
            Terlihat bahwa mayoritas pelanggan, sekitar 49.9%, adalah pelanggan yang berstatus *"Married"* (menikah). Kemudian sekitar 34.0% pelanggan adalah *"Single"* (single/tidak menikah), dan sekitar 16.1% pelanggan merupakan pelanggan yang *"Divorced"* (bercerai).
    - *Numerical Feature*
    ![Numerical Feature](https://i.ibb.co/BcXFSHc/download-5.png)
    Dari visualisasi diatas, dapat dilihat bahwa rata-rata distribusi pada kolom numerical yaitu *right-skewed* (miring ke kanan)
- Checking Data Proportion
  Tujuan dari langkah ini adalah untuk mengetahui total data
  ![Data Proportion](https://i.ibb.co/ZHqHsX0/download-6.png)
  Dari visualisasi diatas, 83% didominasi No dan 17% didominasi Yes
  Berikut perbandingan *Churn* dengan kolom-kolom kategorikal lainnya:
  ![Data Proportion-2](https://i.ibb.co/K9rSFMb/download-7.png)
  Dapat diketahui bahwa:
    * Pada kolom `PrefferedLoginDevice` Pelanggan yang menggunakan *Mobile Phone* lebih banyak melakukan *churn* daripada menggunakan komputer
    * Pada kolom `PreferredPaymentMode` Pelanggan yang menggunakan *Debit Card* atau *Credit Card* lebih banyak melakukan *churn*
    * Pada kolom `Gender` dapat dilihat bahwa pelanggan laki-laki cenderung melakukan *churn* daripada perempuan
    * Pada kolom `PreferedOrderCat` dapat dilihat bahwa kategori *Fashion* memiliki jumlah pelanggan yang lebih sedikit, namun memiliki tingkat churn yang relatif lebih tinggi, dengan 39 pelanggan yang melakukan churn dari total 165 pelanggan
    * Pada kolom `MaritalStatus` dapat dilihat bahwa pelanggan dengan status pernikahan *"Single"* cenderung lebih mungkin untuk melakukan *churn* dibandingkan dengan pelanggan dalam status pernikahan lainnya, 

## Data Preparation
- Melakukan **Splitting**:
    - Tujuannya adalah memisahankan dataset menjadi data pelatihan *(training)* dan data pengujian *(testing)* adalah langkah penting dalam pengembangan model prediktif. Ini membantu dalam mengukur seberapa baik model yang dihasilkan akan berkinerja pada data yang belum pernah dilihat sebelumnya.
    - Pembagian data menjadi 80% untuk pelatihan dan 20% untuk pengujian adalah pilihan yang umum dan dapat membantu memastikan bahwa model yang dikembangkan memiliki pelatihan yang memadai dan juga diuji dengan baik pada data yang independen
    - *Random State* ini digunakan untuk memastikan pembagian data menjadi pelatihan dan pengujian tetap konsisten setiap kode dijalankan.
    - *Stratify* ini juga memastikan bahwa pembagian data tetap menjaga proporsi kelas target untuk mencegah bias dalam pengujian
- Melakukan **Encoding**:
    - Tujuan *encoding* adalah (mengubah categorical menjadi numeric) untuk fitur kategori, salah satu teknik yang umum dilakukan adalah teknik one-hot-encoding
    Variabel Kategori:
        - PreferredLoginDevice,
        - PreferredPaymentMode,
        - Gender,
        - PreferedOrderCat,
        - MaritalStatus
        
        *One Hot Encoding* mengubah setiap kategori menjadi variabel biner (0 atau 1), yang memungkinkan model untuk menganggap setiap kategori secara terpisah tanpa memberikan urutan atau hierarki yang salah.

## Modeling
Telah diketahui bahwa tujuan dari model adalah untuk memprediksi *customer churn*, maka model yang digunakan adalah klasifikasi. 
Model yang dicoba:
- *K-Nearest Neighbors (KNN)*
    Mengukur jarak antara *instance* baru dan *instance* tetangganya untuk menentukan kelasnya.
    - Kekurangan:
        - Sensitif terhadap pemilihan parameter k (jumlah tetangga yang akan digunakan).
        - Mahal secara komputasi saat digunakan pada dataset besar.
        - Tidak memahami hubungan antar fitur atau pentingnya fitur tertentu.
    - Kelebihan:
        - Sederhana dan mudah dimengerti.
        - Cocok untuk dataset dengan pola kelas yang tidak teratur.
    ```
    knn = KNeighborsClassifier(n_neighbors=10)
    ```
    ***n_neighbors*** merupakan parameter yang mengacu pada jumlah tetangga terdekat yang akan digunakan oleh model untuk membuat prediksi atau melakukan klasifikasi pada data baru.
    
- *Random Forest*
    *Random Forest Classifier* Menciptakan "hutan" pohon keputusan, dimana setiap pohon secara independen memprediksi kelas instance, Prediksi akhir dibuat dengan voting mayoritas.
    - Kekurangan:
        - Sulit untuk diinterpretasi dibandingkan dengan pohon keputusan tunggal.
        - Memerlukan lebih banyak sumber daya komputasi daripada pohon keputusan tunggal.
    - Kelebihan:
        - Mengurangi overfitting dengan menggabungkan hasil dari banyak pohon keputusan.
        - Mampu menangani dataset dengan banyak fitur dan kelas.
        - Cocok untuk masalah klasifikasi dan regresi.
    ```
    rf = RandomForestClassifier(n_estimators=50, max_depth=16, random_state=55, n_jobs=-1)
    ```
    ***n_estimators*** merupakan jumlah pohon keputusan yang akan digunakan dalam *ensemble Random Forest.*
    ***max_depth***  mengontrol kedalaman maksimum setiap pohon keputusan dalam ensemble.
    ***random_state*** merupakan seed untuk generator angka acak. Pengaturan random_state memastikan bahwa pengacakan data yang digunakan dalam pembuatan setiap pohon tetap konsisten
    ***n_jobs*** untuk mengontrol berapa banyak pekerjaan paralel yang akan dijalankan saat melatih model.
    
- *AdaBoost*
    Secara iteratif melatih pengklasifikasi yang lemah pada subset data yang berbeda, memberi bobot lebih pada instance yang salah klasifikasi di setiap iterasi. Prediksi akhir dibuat dengan menggabungkan prediksi dari semua pengklarifikasi dengan bobot telah diberikan untuk kinerja yang lebih kuat
    - Kekurangan:
        - Rentan terhadap noise dan outlier dalam data pelatihan.
        - Kinerja dapat terpengaruh jika pengklasifikasi lemah tidak baik atau terlalu kompleks.
        - Memerlukan penyetelan parameter yang hati-hati.
    - Kelebihan:
        - Meningkatkan kinerja model dengan menggabungkan pengklasifikasi lemah.
        - Dapat digunakan dengan berbagai algoritma pengklasifikasi lemah.
        - Cenderung mengurangi overfitting.
    ```
    boosting = AdaBoostClassifier(learning_rate=0.05, random_state=55)
    ```
    ***learning_rate*** merupakan *hyperparameter* yang mengontrol seberapa besar langkah yang diambil dalam mengoreksi kesalahan model pada setiap iterasi atau langkah dalam algoritma boosting. Semakin kecil *learning_rate* semakin lambat model akan belajar, tapi ini jug abisa membantu mencegah *overfitting*. Begitu sebaliknya.
    ***random_state*** merupakan seed untuk generator angka acak. Pengaturan random_state memastikan bahwa pengacakan data yang digunakan dalam pembuatan setiap pohon tetap konsisten.

## Evaluation
|        | KNN       | RandomForest  | Boosting
|------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ----- |
train_f1_score       | 0.481876 | 1.0	    | 0.239782 |
test_f1_score        | 0.386555 | 0.870748  | 0.177778 |

![Evaluation](https://i.ibb.co/W22spCR/download-10.png)
Dari tabel tersebut, dapat menarik beberapa insight yaitu:
- pada model KNN menunjukkan kinerja yang lebih baik pada data pelatihan daripada pada data uji, menunjukkan adanya overfitting.
- pada model Random Forest memiliki kinerja yang hampir sempurna pada data pelatihan, yang mungkin menunjukkan overfitting, tetapi kinerja yang sangat baik pada data uji, menunjukkan kemampuan generalisasi yang baik.
- pada model Boosting menunjukkan kinerja yang rendah pada kedua data pelatihan dan data uji, menunjukkan kesulitan dalam menyesuaikan diri dengan data pelatihan dan kurangnya kemampuan generalisasi.

Random Forest mungkin merupakan pilihan terbaik karena memiliki keseimbangan antara kinerja pada data pelatihan dan data uji, meskipun penting untuk melanjutkan evaluasi dan tuning untuk memastikan hasil yang optimal.

F1 Score adalah metrik evaluasi yang mengukur kinerja model klasifikasi dengan menemukan keseimbangan antara Recall dan Presisi. Metrik ini berguna saat ingin mengurangi false positive dan false negative secara seimbang. Semakin tinggi F1 Score, semakin baik performa model dalam klasifikasi, dan nilai maksimumnya adalah 1, yang menandakan performa sempurna.
