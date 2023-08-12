# Laporan Proyek Machine Learning - Alexander Gosal
## Project Overview
Latar belakang pembuatan model rekomendasi *content-based filtering* untuk rekomendasi artikel berasal dari tantangan dalam menghadapi ledakan informasi di era digital. Dengan munculnya internet dan platform berita daring, jumlah artikel yang tersedia melebihi kapasitas manusia untuk menemukan dan menyaring informasi yang relevan. Pengguna sering kali merasa terjebak dalam gelembung informasi, hanya terpapar pada sudut pandang yang sudah dikenal, tanpa eksplorasi yang luas terhadap topik yang beragam. Model rekomendasi content-based muncul untuk mengatasi masalah ini dengan cara yang lebih cerdas dan personal. Dengan menganalisis karakteristik konten seperti topik, kata kunci, struktur, dan gaya penulisan, model ini dapat memahami esensi dari setiap artikel dan mencocokkannya dengan preferensi pengguna. Dalam konteks ini, tujuan utama adalah memberikan pengalaman membaca yang lebih bervariasi, meningkatkan pengetahuan pengguna dengan menyajikan artikel yang beragam namun tetap relevan dengan minat mereka, serta membantu pengguna mengeksplorasi sudut pandang yang berbeda, mempromosikan keragaman informasi.

Selain itu, penggunaan model rekomendasi *content-based filtering* juga melibatkan pertimbangan terhadap privasi pengguna. Model ini beroperasi berdasarkan analisis konten yang ada, sehingga tidak memerlukan informasi identitas atau perilaku pengguna lainnya. Ini menjadi penting dalam mengatasi kekhawatiran privasi yang semakin meningkat di era digital saat ini. Dengan menggunakan model *content-based*, pengguna dapat menerima rekomendasi yang berdasarkan pada apa yang telah mereka konsumsi tanpa memerlukan data pribadi atau histori interaksi yang mendalam. Keberlangsungan model ini juga terjamin karena tidak tergantung pada data interaksi pengguna yang terus-menerus. Hal ini membuat model content-based cocok untuk platform yang baru diluncurkan atau untuk situasi di mana data pengguna terbatas. Dengan demikian, model rekomendasi content-based filtering menjadi solusi yang cerdas dan berkelanjutan dalam menghadapi tantangan pencarian informasi yang efektif, meningkatkan pengetahuan pengguna, dan menghormati kebutuhan privasi mereka.

## Business Understanding
#### Problem Statement
Berdasarkan pada latar belakang di atas, permasalahan yang dapat diselesaikan pada proyek ini adalah bagaimana cara membangun sistem rekomendasi artikel menggunakan metode *content-based filtering* untuk meningkatkan pengalaman membaca pengguna.
#### Goal
Tujuan dari proyek ini adalah meningkatkan pengalaman pengguna dengan menyajikan rekomendasi konten yang lebih relevan, personal, dan menarik. 

## Data Understanding
Deskdrop adalah platform komunikasi internal yang dikembangkan oleh CI&T, berfokus pada perusahaan yang menggunakan Google G Suite. Di antara fitur lainnya, platform ini memungkinkan karyawan perusahaan untuk berbagi artikel yang relevan dengan rekan mereka, dan berkolaborasi di sekitar mereka. [CI&T Deskdrop Dataset](https://www.kaggle.com/datasets/gspmoreira/articles-sharing-reading-from-cit-deskdrop?select=users_interactions.csv) berisi sampel log 12 bulan (Maret 2016 - Februari 2017) dari platform Komunikasi Internal CI&T (DeskDrop) dengan 73000 interaksi pengguna yang tercatat dari 3000 artikel publik yang dibagikan di platform.
#### Variabel-variabel pada *shared_articles.csv* adalah sebagai berikut:
* timestamp : pengenal waktu unik
* eventType : 'CONTENT SHARED' (Artikel dibagikan di platform dan tersedia untuk pengguna.), 'CONTENT REMOVED' (Artikel telah dihapus dari platform dan tidak tersedia untuk rekomendasi lebih lanjut.)
* contentId : pengenal konten unik
* authorPersonId : pengenal id unik
* authorSessionId : pengenal sesi unik
* authorUserAgent : informasi yang dikirim oleh peramban web
* authorRegion : informasi mengenai asal wilayah pembuat artikel
* authorCountry : informasi mengenai asal negara pembuat artikel
* contentType : 'HTML', 'RICH', 'VIDEO'
* url : url atau link menuju ke artikel
* title : judul artikel
* text : isi artikel
* lang : bahasa 
#### Variabel-variabel pada *users_interactions.csv* adalah sebagai berikut:
* timestamp : pengenal waktu unik
* eventType : 'VIEW' (Pengguna telah membuka artikel.), 'LIKE' (Pengguna telah menyukai artikel tersebut.), 'COMMENT CREATED' (Pengguna membuat komentar di artikel.), 'FOLLOW' (Pengguna memilih untuk diberi tahu tentang komentar baru apa pun di artikel.), 'BOOKMARK' (Pengguna telah mem-bookmark artikel agar mudah dikembalikan di masa mendatang.)
* contentId : pengenal konten unik
* personId : pengenal id unik
* sessionId : pengenal sesi unik
* userAgent : informasi yang dikirim oleh peramban web
* userRegion : informasi mengenai asal wilayah user
* userCountry : informasi mengenai asal negara user

## Data Preparation
#### Memilih fitur untuk membuat sistem rekomendasi
Pemilihan fitur dalam pembuatan sistem rekomendasi *content-based filtering* berdasarkan dataset CI&T Deskdrop sangat penting untuk memastikan bahwa rekomendasi yang dihasilkan sesuai dengan preferensi dan minat pengguna. Fitur-fitur ini digunakan sebagai representasi dari konten item (dalam kasus ini, artikel) dalam dataset. Dengan memilih fitur yang tepat, sistem dapat mengidentifikasi kemiripan antara item berdasarkan karakteristik kontennya, sehingga mampu memberikan rekomendasi yang lebih relevan.
#### Mengapus data yang duplikat
Penghapusan data yang duplikat dalam dataset sangat penting karena dapat mempengaruhi kualitas analisis, model, dan rekomendasi yang dibangun berdasarkan dataset tersebut.
#### Mengubah eventType pada dataset users interactions menjadi bobot
Pembobotan berdasarkan *eventType* dari pengguna adalah pendekatan yang umum digunakan dalam sistem rekomendasi berbasis konten (*content-based recommendation*) untuk meningkatkan akurasi dan relevansi rekomendasi. Kolom *eventType* dalam konteks ini merujuk pada jenis-jenis aktivitas atau interaksi yang dilakukan oleh pengguna dalam platform atau aplikasi, yang biasanya direkam dalam data log atau data interaksi. Di Deskdrop, pengguna diizinkan untuk melihat artikel berkali-kali, dan berinteraksi dengannya dengan cara yang berbeda (mis. Suka atau komentar). Oleh karena itu, untuk memodelkan minat pengguna pada artikel tertentu, dilakukan penggabungan semua interaksi yang telah dilakukan pengguna dalam item dengan penjumlahan berbobot dari jenis interaksi.
#### Menggabungkan kedua dataset 
Kedua dataset kemudian digabung berdasarkan kolom *contentId* untuk memperoleh *rating* agar seluruh *rating* user terhadap suatu konten dapat dijumlahkan untuk evaluasi sistem rekomendasi.

## Modeling
Model yang akan digunakan proyek kali ini yaitu menggunakan pendekatan *content-based filtering* menggunakan TfidfVectorizer. Model *Content-Based Filtering* adalah salah satu pendekatan dalam sistem rekomendasi yang berfokus pada karakteristik atau konten dari item yang akan direkomendasikan kepada pengguna. Dalam model ini, kita mengukur kesamaan antara item berdasarkan fitur-fitur atau atribut-atribut yang ada pada item tersebut. Teknik yang digunakan dalam model *Content-Based Filtering* ini adalah *TF-IDF Vectorizer* dan *Cosine Similarity*.
#### TF-IDF Vectorizer
TF-IDF singkatan dari Term Frequency-Inverse Document Frequency. Ini adalah metode yang digunakan untuk mengukur pentingnya suatu kata dalam suatu dokumen atau korpus. Term Frequency (TF) mengukur seberapa sering kata tertentu muncul dalam dokumen. Ini memberikan bobot lebih besar pada kata-kata yang lebih sering muncul. Inverse Document Frequency (IDF) mengukur seberapa penting suatu kata dalam keseluruhan korpus dokumen. Kata-kata yang muncul di banyak dokumen mendapatkan nilai IDF lebih rendah. TF-IDF menggabungkan kedua konsep ini untuk memberikan representasi numerik bagi kata-kata dalam dokumen.
#### Cosine Similarity
Cosine Similarity adalah metode untuk mengukur kesamaan antara dua vektor dalam ruang berdimensi banyak, seperti dalam kasus representasi TF-IDF dari dokumen.
#### Recommendation Result
