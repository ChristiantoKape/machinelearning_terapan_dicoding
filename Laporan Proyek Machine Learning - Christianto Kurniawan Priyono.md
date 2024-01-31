# Laporan Proyek Machine Learning - Christianto Kurniawan Priyono

## Domain Proyek
Dalam era digital yang semakin berkembang, penonton memiliki akses yang tak terbatas ke ribuan film yang beragam dari berbagai platform streaming. Namun, hal ini juga menciptakan kesenjangan antara berbagai pilihan film dan keterbatasan waktu penonton. Oleh karena itu, proyek rekomendasi film menjadi semakin penting. Melalui penerapan algoritma canggih dan analisis data, rekomendasi film memiliki kemampuan untuk menyaring dan menyesuaikan rekomendasi film tertentu dengan preferensi unik setiap penonton. Dengan demikian, proyek ini memiliki potensi untuk meningkatkan kepuasan penonton, memungkinkan eksplorasi genre atau film yang belum pernah dipikirkan sebelumnya, serta memperkaya pengalaman menonton film. Fokus pada solusi ini dapat membawa pengalaman menonton yang lebih bermakna, menghemat waktu penonton dalam mencari film sesuai dengan selera mereka, dan pada akhirnya, memperkaya pengalaman sinema secara keseluruhan. Hal ini didukung oleh data Statista yang menunjukkan bahwa pendapatan segmen SVOD (subscription video on demand) di Indonesia diproyeksikan mencapai US$411 juta pada 2021 dan penetrasi penggunanya ditaksir mencapai 16,5% pada 2021 dan diharapkan naik menjadi 20,4% pada 2025. Selain itu, survei JakPat juga mengungkapkan bahwa sebanyak 74% responden mengaku berlangganan setidaknya satu platform streaming pada 2021, naik dari 59% pada 2020. Platform streaming juga menjadi peluang bagi industri film lokal untuk menjangkau penonton yang lebih luas dan mengembangkan konten-konten orisinal yang sesuai dengan selera dan kebutuhan penonton Indonesia.
[Sumber](https://bisnisindonesia.id/article/orang-indonesia-makin-hobi-nonton-industri-ott-kian-cuan)

## Business Understanding
### Problem Statements
Dalam upaya untuk memaksimalkan penjualan dan pengalaman penonton di platform streaming, akan dikembangkan sistem rekomendasi film untuk mengatasi permasalahan berikut:
- Bagaimana menciptakan sistem rekomendasi film yang dapat dipersonalisasi dengan menggunakan teknik *collaborative filtering* dan *content-based filtering* berdasarkan perferensi dan perilaku penonton?
- Bagaimana cara mengukur keberhasilan sistem rekomendasi film dalam meningkatkan interaksi dengan penonton dan pada saat yang sama meningkatkan penjualan dan konsumsi film?

### Goals
Untuk menjawab pertanyaan tersebut, dibuat sistem rekomendasi dengan tujuan atau goals sebagai berikut:
- Mengembangkan sistem rekomendasi film yang mampi menyajikan rekomendasi yang sangat personal kepada penonton berdasarkan preferensi dan perilaku mereka.
- Meningkatkan kepuasan penonton dengan memberikan rekomendasi yang akurat dan bermanfaat yang pada gilirannya dapat meningkatkan loyalitas penotnon terhadap platform streaming.
- Meningkatkan akurasi rekomendasi dengan menggunakan metode deep learning yang dapat menghasilkan skor *RMSE* yang rendah. *Root Mean Square Error (RMSE)* adalah metrik yang mengukur seberapa dekat rekomendasi dengan rating yang diberikan oleh pengguna. Semakin rendah skor *RMSE* semakin akurat rekomendasi


### Solution Approach
Berikut langkah yang bisa dilakukan untuk mencapai tujuan diatas:
- Pengumpulan Data, mengumpulkan data tentang preferensi dan perilaku penonton
- Preprocessing Data, membersihkan dan mengorganisasi serta mengolah data agar dapat digunakan dalam permodelan.
- Permodelan *Content-Based Filtering*, membangun model ini untuk memahami karakteristik film dan menghubungkannya dengan preferensi penonton berdasarkan konten, seperti genre, sutradara, aktor dan tema
- Permodelan *Collaborative Filtering*, membangun model untuk mengidentifikasi pola dan preferensi berdasarkan perilaku sejenis penonton.

Dengan mengikuti pendekatan ini, diharapkan sistem rekomendasi film dapat memberikan hasil yang optimal sesuai dengan tujuan yang telah ditetapkan. 


## Data Understanding
*Dataset Source*: https://www.kaggle.com/datasets/sunilgautam/movielens

Pada sumber dataset tersebut memiliki 4 *file*, yaitu:
- links.csv
- movie.csv
- ratings.csv
- tags.csv

Dari keempat *file* tersebut, yang akan digunakan adalah *file movies.csv* dan *ratings.csv*

### Attribute Information
**movies.csv**
| Attribut       | Tipe Data       | Deskripsi  |
|----------------|-----------------|------------|
movieId          | Int             | ID film	|
title            | Object          | Judul film |
genres           | Object          | Jenis/Tipe Film |

**ratings.csv**
| Attribut       | Tipe Data       | Deskripsi  |
|----------------|-----------------|------------|
userId           | Int             | Id user	|
movieId          | Int             | Id film |
rating           | Float           | Peringkat yang diberikan pengguna |
timestamp        | Int             | Waktu ketika pengguna memberikan rating film |

**Exploratory Data Analisis**
Langkah-langkah yang dilakukan dalam Exploratory Data Analisis adalah sebagai berikut:
- **Mengecek Data Unik**
    Dataset *movies.csv* terdiri dari 9742 data unik pada kolom *movieId*, mencakup 9737 data unik pada kolom *title*, serta 951 data unik pada kolom *genres*. Penting untuk dicatat bahwa dataset ini tidak mengandung data yang hilang (missing value).
    Sementara itu, dalam dataset *ratings.csv*, terdapat 610 data unik pada kolom *userId* dan 9724 data unik pada kolom *movieId*. Terdapat juga 10 data unik pada kolom *rating*, mencerminkan variasi penilaian yang mungkin. Namun, perhatian khusus perlu diberikan pada kolom *timestamp*, yang berisi 95043 data unik yang mungkin memerlukan pemrosesan atau konversi agar dapat digunakan secara efektif. Diperlukan penanganan khusus untuk mengatasi anomali pada kolom *timestamp* agar dataset ini dapat dimanfaatkan secara optimal.
- **Distribusi**
    - **Persebaran Tahun Rilis Film**
        ![Distribution](https://i.ibb.co/g3rpvYp/Screenshot-2023-09-25-014032.png)
        Dari hasil visualisasi, terlihat bahwa jumlah film yang paling banyak dirilis terjadi setelah tahun 2000, dan terdapat tren peningkatan dari tahun ke tahun. Temuan ini mengindikasikan bahwa industri film mengalami perkembangan pesat selama dua dekade terakhir, baik dalam hal kuantitas maupun kualitas produksi film.
    - **Persebaran Jenis Film**
        ![Distribution](https://i.ibb.co/VN7dZRg/download-11.png)
        Hasil analisis menunjukkan bahwa genre drama memiliki jumlah film terbanyak, mengindikasikan bahwa genre ini sangat populer dan kemungkinan besar memiliki basis penggemar yang besar. Selain itu, keberadaan yang signifikan dari genre komedi dan thriller juga mengungkapkan bahwa kedua genre ini menikmati popularitas yang kuat di antara penonton. Di sisi lain, adanya genre yang tidak terdefinisi dan film-noir dengan jumlah film yang terbatas menunjukkan bahwa keduanya mungkin termasuk dalam kategori genre yang lebih khusus atau niche yang mungkin tidak menarik perhatian audiens sebanyak genre utama.
    - **Persebaran Rating untuk Semua Film**
        ![Distribution](https://i.ibb.co/cYj9nn5/download.png)
        Visualisasi di atas memberikan wawasan tentang preferensi penonton terhadap film, dengan rating 4 yang paling sering diberikan. Hal ini mengindikasikan bahwa sebagian besar film dalam dataset mendapatkan penilaian positif dari penonton. Selain itu, rating 3 juga cukup umum, menunjukkan adanya variasi dalam preferensi penonton terhadap film-film tersebut.
        Namun, di sisi lain, terlihat bahwa rating 0.5 dan 1.5 jarang diberikan kepada film-film tersebut. Hal ini dapat mengindikasikan bahwa penonton mungkin lebih memilih untuk tidak memberikan penilaian daripada memberikan penilaian negatif yang sangat rendah. Hal ini menunjukkan potensi untuk meningkatkan variasi dalam penilaian film-film tersebut, mungkin dengan mengidentifikasi area yang perlu diperbaiki atau ditingkatkan dalam produksi film.

## Data Preparation
Tujuan dari *data preparation* adalah untuk memastikan bahwa data yang digunakan dalam analisis atau permodelan memiliki kualitas yang baik, meminimalisir terjadinya bias pada data, terstruktur dengan benar dan siap untuk diproses. Berikut merupakan tahapan dalam melakukan pra-pemrosesan data:
1. **Melakukan Penanganan *Missing Value***
    Pada langkah ini, penulis melakukan pengecekan *missing value* pada dataset terlebih dahulu. Karena dataset pada movies.csv dan ratings.csv sudah bersih, maka tidak dilakukan drop data.
    Tujuan dilakukan-nya penanganan *missing value* adalah sebagai berikut: 
    - Memastikan Kualitas Data, karena data yang hilang dapat menyebabkan bias dan menganggu hasil analisis yang akurat.
    - Menghindari Kesalahan Analisis, nilai yang hilang dapat mengakibatkan kesalahan dalam analisis data, seperti perhitungan yang salah atau penarikan kesimpulan yang tidak tepat.
2. **Melakukan Penanganan *Duplicate Values***
    Langkah penanganan duplicate values sama seperti missing value, yaitu penulis *check* dahulu, karena dataset movies.csv dan ratings.csv tidak memiliki data duplikat, maka tidak ada *threatment* khusus
    Tujuan dilakukan penanganan *duplicate values* adalah:
    - Memastikan Kualitas Data, duplikasi data dapat menyebabkan bias dan mengganggu hasil analisis yang akurat.
    - Menghindari Perkiraan Berlebihan, jika data ini tidak ditangani, perkiraan atau statistik yang didasarkan pada data tersebut dapat menjadi terlalu berlebihan. Ini dapat menghasilkan evaluasi yang tidak akurat dalam berbagai konteks.
3. **Menghapus kolom timestamp**
    Dalam tahap ini, kolom timestamp akan dihapus dari dataset karena tidak diperlukan dalam pengembangan model. Langkah ini dilakukan untuk mempermudah analisis data yang lebih fokus pada informasi yang relevan untuk pengembangan model, sehingga kolom timestamp yang tidak digunakan akan dihilangkan dari dataset.

## Model Development
Telah diketahui bahwa tujuan dari model adalah untuk melakukan rekomendasi film kepada penonton, maka akan dilakukan menggunakan 2 cara, yaitu *Content Based Filtering* dan *Collaborative Filtering*
- ***Content Based Filtering***
    Langkah pertama yang penulis lakukan pada model ini adalah menggunakan TF-IDF Vectorizer untuk mengubah deskripsi film menjadi representasi fitur yang mencerminkan pentingnya kata-kata dalam konteks dokumen. Fungsi yang penulis gunakan adalah 
    ```
    tfid = TfidfVectorizer()
    ```
    Langkah selanjutnya penulis melakukan fit dan transformasi ke dalam bentuk matriks, hasilnya adalah matriks berukuran (9742, 24). Nilai 9742 merupakan ukuran data dan 23 merupakan matriks genre film.
    Untuk mengukur derajat kesamaan antar film, digunakan teknik *cosine similarity* dengan menggunakan fungsi *cosine_similarity* dari library sklearn.
    Kemudian, untuk memberikan rekomendasi film, dilakukan penggunaan *argpartition* untuk mengambil sejumlah nilai k tertinggi dari *similarity* data, dan kemudian mengambil data dengan bobot tertinggi hingga terendah. Akurasi sistem rekomendasi diuji untuk memastikan bahwa rekomendasi yang diberikan mirip dengan film yang dicari.
    Referensi yang digunakan untuk menentukan 10 rekomendasi film yang memiliki kesamaan genre adalah film dengan data sebagai berikut:
    | movieId  | title                   | genres           |
    |----------|-------------------------|------------------|
    |3         | Wild China (2008)       | Documentary      |
    Kemudian, hasil rekomendasi dari model *Content Based Filtering* berdasarkan referensi diatas adalah sebagai berikut: 
    | movieId  | title                   | genres           |
    |----------|-------------------------|------------------|
    |71131     | Most Hated Family in America, The (2007)	| Documentary |
    |1361      | Paradise Lost: The Child Murders at Robin Hood Hills (1996) | Documentary |
    |60291     | Gonzo: The Life and Work of Dr. Hunter S. Thompson (2008) | Documentary |
    |6299      | Winged Migration (Peuple migrateur, Le) (2001) | Documentary |
    |5224      | Promises (2001) | Documentary |
    |60333     | Encounters at the End of the World (2008) | Documentary |
    |83827     | Marwencol (2010) | Documentary |
    |6269      | Stevie (2002) | Documentary |
    |27912     | Outfoxed: Rupert Murdoch's War on Journalism (2004) | Documentary |
    |8264      | Grey Gardens (1975) | Documentary |
    
    Berikut kelebihan dan kekurangan dari *Content Based Filtering*:
    - **Kekurangan**: 
        - **Keterbatasan Dalam Diversifikasi Rekomendasi**, memiliki kecenderungan untuk memberikan rekomendasi yang serupa dengan apa yang telah dikonsumsi oleh pengguna sebelumnya, Karena bisa mengurangi keragaman dalam pengalaman pengguna.
        - **Keterbatasan Dalam Menemukan "Sesuatu yang Baru"**, algoritma ini cenderung tidak mampu merekomendasikan item yang berbeda karena berfokus pada kesamaan karakteristik.
    - **Kelebihan**:
        - **Personalisasi**, *Content-Based Filtering* dapat memberikan rekomendasi yang sangat personal, karena berfokus pada preferensi individu pengguna.
        - **Tidak Bergantung pada Data Eksternal**, *Content-Based Filtering* tidak bergantung pada data eksternal, seperti peringkat pengguna lain atau data sosial.
    
- ***Collaborative Filtering***
    Dalam proses pemodelan ini, penulis menggunakan dataset *ratings.csv* karena dataset tersebut sudah mencakup variabel yang diperlukan. Langkah awal adalah melakukan *encoding* terhadap data *userId* dan *movieId*. Selanjutnya, dilakukan pemetaan data ke dalam format yang akan digunakan, serta konversi nilai rating menjadi tipe data float. Data kemudian dibagi menjadi dua bagian, yaitu *training set* sebanyak 80% dari total data dan *validation set* sebanyak 20%.
    Langkah berikutnya melibatkan proses *embedding* terhadap data film dan pengguna (user), di mana representasi vektor dipelajari untuk masing-masing entitas ini. Selanjutnya, dilakukan operasi perkalian *dot product* antara *embedding* film dan pengguna. Dalam proses ini, juga diterapkan bias, dan skor kecocokan (match score) diberi skala nilai antara 0 hingga 1 menggunakan fungsi aktivasi sigmoid.
    Dalam tahap *training model*, penulis menggunakan fungsi *callback* yang bertujuan untuk menghindari *overfitting* atau *underfitting*, menjaga agar model dapat belajar dengan baik dari data tanpa menyimpang secara signifikan.
    Untuk mendapatkan rekomendasi film, penulis memilih secara acak sejumlah pengguna (users) sebagai sampel dan kemudian menentukan variabel "movie_not_watched," yang merupakan daftar film yang belum pernah ditonton oleh pengguna tersebut. Dengan demikian, model dapat memberikan rekomendasi film yang mungkin sesuai dengan preferensi pengguna berdasarkan hasil pembelajaran dari data sebelumnya.

    Hasil rekomendasi dari model *Collaborative Filtering*
    ```
    Showing recommendations for users: 177
    ========================================
    movie with high ratings from user
    --------------------------------
    Toy Story (1995)
    Rain Man (1988)
    Shakespeare in Love (1998)
    Hook (1991)
    Pitch Perfect (2012)
    --------------------------------
    Top 10 movie recommendation
    --------------------------------
    Party Girl (1995)
    Three Colors: Red (Trois couleurs: Rouge) (1994)
    Sweet Hereafter, The (1997)
    Talk to Her (Hable con Ella) (2002)
    Adaptation (2002)
    Shaun of the Dead (2004)
    Elite Squad (Tropa de Elite) (2007)
    Paperman (2012)
    Day of the Doctor, The (2013)
    Three Billboards Outside Ebbing, Missouri (2017)
    ```
    Berikut kelebihan dan kekurangan dari *Collaborative Filtering*:
    - **Kekurangan:**
        - ***Cold Start***, *Collaborative Filtering* tidak dapat memberikan rekomendasi yang baik untuk pengguna atau item baru yang belum memiliki data peringkat yang cukup
        - ***Bubble Efect***, Dalam situasi di mana pengguna memberikan peringkat serupa pada sejumlah besar item, *Collaborative Filtering* dapat menghasilkan efek "bubble," di mana item yang direkomendasikan cenderung seragam dan mungkin kurang diversifikasi.
    - **Kelebihan:**
        - **Dapat Menemukan Kesamaan yang Tidak Terduga**, *Collaborative Filtering* mengungkap kesamaan dalam pola peringkat item, sehingga dapat merekomendasikan item yang berbeda dari yang pernah dilihat pengguna.
        - **Skalabilitas**, *Collaborative Filtering* skalabel karena bergantung pada perbandingan pengguna dan item, bukan jumlah mutlak pengguna atau item.

## Evaluation
### Precision Content Based Filtering
Langkah pertama yaitu mencari judul film "Grumpier Old Men (1995)"
```
dfMovies[dfMovies['title'] == 'Grumpier Old Men (1995)']
```
Hasilnya sebagai berikut: 
| movieId  | title                   | genres           |
|----------|-------------------------|------------------|
3          | Grumpier Old Men (1995) | Comedy`|`Romance |
Dapat dilihat dari data di atas bahwa film dengan judul "Grumpier Old Men (1995)" memiliki dua genre, yaitu comedy dan romance. Oleh karena itu, rekomendasi film seharusnya mencakup kedua genre tersebut.
```
MovieRecommendations("Grumpier Old Men (1995)")
```
Hasil Rekomendasi:
| movieId  | title                   | genres           |
|----------|-------------------------|------------------|
5135    | Monsoon Wedding (2001) | Comedy`|`Romance |
4018    | What Women Want (2000) | Comedy`|`Romance |
6551    | Anarchist Cookbook, The (2002) | Comedy`|`Romance |
43919    | Date Movie (2006) | Comedy`|`Romance |
2144    | Sixteen Candles (1984) | Comedy`|`Romance |
43930    | Just My Luck (2006) | Comedy`|`Romance |
111680    | At Middleton (2013) | Comedy`|`Romance |
6407    | Walk, Don't Run (1966) | Comedy`|`Romance |
44004    | Failure to Launch (2006) | Comedy`|`Romance |
6523    | Mr. Baseball (1992) | Comedy`|`Romance |

Dari hasil rekomendasi di atas, terlihat bahwa film "Grumpier Old Men (1995)" memiliki dua genre. Seluruh film yang direkomendasikan juga memiliki genre yang sama. Hal ini mengindikasikan bahwa precision sistem yang telah dibuat adalah 10/10 atau setara dengan 100%.
![Image](https://i.ibb.co/p1ZrGXZ/download-1.png)
Precision = 10/10 = 100%

### Root Mean Square Error Plot
![RMSE Plot](https://i.ibb.co/d6fjMry/download-1.png)
Perubahan *Root Mean Squared Error (RMSE)* selama proses pelatihan merupakan indikator penting dalam mengukur kualitas model. Pada awal pelatihan, terlihat RMSE yang cukup tinggi pada data pelatihan dan data validasi, mengindikasikan bahwa model belum sepenuhnya terlatih dan prediksinya kurang akurat. Namun, melalui beberapa *epoch* berikutnya, terjadi perbaikan yang signifikan, ini menunjukkan bahwa model mulai memahami pola-pola dalam data. Meskipun penurunan RMSE berlanjut namun pada epoch 11 hingga 13, *RMSE* pada data validasi tidak mengalami peningkatan yang signifikan. Hal ini mengindikasikan bahwa model mungkin telah mencapai kinerja terbaiknya untuk data validasi yang tersedia, dan pelatihan lebih lanjut tidak memberikan manfaat yang signifikan. Selain itu, ada kemungkinan model mulai overfitting pada data pelatihan, yaitu menjadi terlalu khusus untuk data tersebut dan sulit untuk menggeneraslisasi prediksi pada data yang belum pernah dilihat sebelumnya.