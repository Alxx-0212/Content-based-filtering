```markdown
# Laporan Proyek Machine Learning - Alexander Gosal
## Project Overview
Latar belakang pembuatan model rekomendasi *content-based filtering* untuk rekomendasi artikel berasal dari tantangan dalam menghadapi ledakan informasi di era digital. Dengan munculnya internet dan platform berita daring, jumlah artikel yang tersedia melebihi kapasitas manusia untuk menemukan dan menyaring informasi yang relevan. Pengguna sering kali merasa terjebak dalam gelembung informasi, hanya terpapar pada sudut pandang yang sudah dikenal, tanpa eksplorasi yang luas terhadap topik yang beragam. Model rekomendasi content-based muncul untuk mengatasi masalah ini dengan cara yang lebih cerdas dan personal. Dengan menganalisis karakteristik konten seperti topik, kata kunci, struktur, dan gaya penulisan, model ini dapat memahami esensi dari setiap artikel dan mencocokkannya dengan preferensi pengguna. Dalam konteks ini, tujuan utama adalah memberikan pengalaman membaca yang lebih bervariasi, meningkatkan pengetahuan pengguna dengan menyajikan artikel yang beragam namun tetap relevan dengan minat mereka, serta membantu pengguna mengeksplorasi sudut pandang yang berbeda, mempromosikan keragaman informasi.

Selain itu, penggunaan model rekomendasi *content-based filtering* juga melibatkan pertimbangan terhadap privasi pengguna. Model ini beroperasi berdasarkan analisis konten yang ada, sehingga tidak memerlukan informasi identitas atau perilaku pengguna lainnya. Ini menjadi penting dalam mengatasi kekhawatiran privasi yang semakin meningkat di era digital saat ini. Dengan menggunakan model *content-based*, pengguna dapat menerima rekomendasi yang berdasarkan pada apa yang telah mereka konsumsi tanpa memerlukan data pribadi atau histori interaksi yang mendalam. Keberlangsungan model ini juga terjamin karena tidak tergantung pada data interaksi pengguna yang terus-menerus. Hal ini membuat model *content-based* cocok untuk platform yang baru diluncurkan atau untuk situasi di mana data pengguna terbatas. Dengan demikian, model rekomendasi content-based filtering menjadi solusi yang cerdas dan berkelanjutan dalam menghadapi tantangan pencarian informasi yang efektif, meningkatkan pengetahuan pengguna, dan menghormati kebutuhan privasi mereka.

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
Pembobotan berdasarkan *eventType* dari pengguna adalah pendekatan yang umum digunakan dalam sistem rekomendasi berbasis konten (*content-based recommendation*) untuk meningkatkan akurasi dan relevansi rekomendasi. Kolom *eventType* dalam konteks ini merujuk pada jenis-jenis aktivitas atau interaksi yang dilakukan oleh pengguna dalam platform atau aplikasi, yang biasanya direkam dalam data log atau data interaksi. Di Deskdrop, pengguna diizinkan untuk melihat artikel berkali-kali, dan berinteraksi dengannya dengan cara yang berbeda (mis. Suka atau komentar). Oleh karena itu, untuk memodelkan minat pengguna pada artikel tertentu, dilakukan penggabungan semua interaksi yang telah dilakukan pengguna dalam item dengan penjumlahan berbobot dari jenis interaksi. Berikut adalah pembobotan berdasarkan interaksi pengguna :
|     eventType     | score |
|:-----------------:|-------|
| 'VIEW'            | 1.0   |
| 'LIKE'            | 2.0   |
| 'BOOKMARK'        | 3.0   |
| 'FOLLOW'          | 4.0   |
| 'COMMENT CREATED' | 5.0   |
#### Menggabungkan kedua dataset 
Kedua dataset kemudian digabung berdasarkan kolom *contentId* untuk memperoleh *rating* agar seluruh *rating* user terhadap suatu konten dapat dijumlahkan untuk evaluasi sistem rekomendasi.
#### Menjumlahkan seluruh rating user terhadap suatu konten untuk evaluasi
Model content-based filtering adalah jenis model rekomendasi yang didasarkan pada karakteristik atau atribut dari item itu sendiri, seperti teks, fitur, atau metadata lainnya. Dalam *content-based filtering*, tujuannya adalah mencari item-item yang memiliki kesamaan dalam atribut atau karakteristik tertentu dengan item yang disukai oleh pengguna. Ini berarti model mencoba memahami preferensi pengguna berdasarkan apa yang mereka sukai pada item sebelumnya, bukan dengan membandingkan perilaku pengguna dengan pengguna lain. Oleh karena itu, seluruh dataset digabung berdasarkan judul dan jumlah keseluruhan rating user untuk judul tersebut untuk evaluasi sistem rekomendasi.
###### Deskripsi dataset
|        | title | text |   rating  |
|:------:|:-----:|:----:|:---------:|
|  count |  2173 | 2173 |    2173   |
| unique |  2173 | 2169 |    NaN    |
|  mean  |  NaN  |  NaN | 20.936954 |
|   min  |  NaN  |  NaN |     0     |
|   max  |  NaN  |  NaN |    462    |

Rata - rata rating secara keseluruhan untuk semua artikel adalah 20.9369 atau kurang lebih sebesar 21. Oleh karena itu, rating tersebut akan digunakan sebagai threshold untuk evaluasi.

## Modeling
Model yang akan digunakan proyek kali ini yaitu menggunakan pendekatan content-based filtering menggunakan TfidfVectorizer. Model *Content-Based Filtering* adalah salah satu pendekatan dalam sistem rekomendasi yang berfokus pada karakteristik atau konten dari item yang akan direkomendasikan kepada pengguna. Dalam kasus ini, model mengukur kesamaan antara item berdasarkan fitur-fitur atau atribut-atribut yang ada pada item tersebut. Teknik yang digunakan dalam model *Content-Based Filtering* ini adalah *TF-IDF Vectorizer* dan *Cosine Similarity*.
#### TF-IDF Vectorizer
TF-IDF singkatan dari Term Frequency-Inverse Document Frequency. Ini adalah metode yang digunakan untuk mengukur pentingnya suatu kata dalam suatu dokumen atau korpus. Term Frequency (TF) mengukur seberapa sering kata tertentu muncul dalam dokumen. Ini memberikan bobot lebih besar pada kata-kata yang lebih sering muncul. Inverse Document Frequency (IDF) mengukur seberapa penting suatu kata dalam keseluruhan korpus dokumen. Kata-kata yang muncul di banyak dokumen mendapatkan nilai IDF lebih rendah. TF-IDF menggabungkan kedua konsep ini untuk memberikan representasi numerik bagi kata-kata dalam dokumen.
#### Cosine Similarity
Cosine Similarity adalah metode untuk mengukur kesamaan antara dua vektor dalam ruang berdimensi banyak, seperti dalam kasus representasi TF-IDF dari dokumen.
#### Recommendation Result
Menggunakan artikel "An operating model for company-wide agile development", berikut adalah hasil rekomendasi *content-based filtering* :
|    |                                      title                                      | rating |
|----|:-------------------------------------------------------------------------------:|--------|
| 1  | Embracing Agile                                                                 | 188.0  |
| 2  | Agile is Dead, Long Live Continuous Delivery - Gradle                           | 24.0   |
| 3  | 12 Agile principles                                                             | 1.0    |
| 4  | Scrum Community - Scrum Alliance                                                | 64.0   |
| 5  | Big IT Rising                                                                   | 50.0   |
| 6  | Organizing for digital acceleration: Making a two-speed IT operating model work | 28.0   |
| 7  | How enterprise architects can help ensure success with digital transformations  | 57.0   |
| 8  | The new tech talent you need to succeed in digital                              | 34.0   |
| 9  | Do you want Crappy Agile?                                                       | 43.0   |
| 10 | Five questions boards should ask about IT in a digital world                    | 58.0   |

## Evaluation
#### Precision@k
Precision@k adalah salah satu metrik evaluasi yang digunakan untuk mengukur performa model dalam sistem rekomendasi, termasuk model Content-Based Filtering. Dalam konteks ini, *Content-Based Filtering* adalah pendekatan dalam sistem rekomendasi yang menganalisis konten atau fitur dari item-item yang akan direkomendasikan kepada pengguna. *Precision@k* mengukur sejauh mana item-item yang direkomendasikan oleh model benar-benar relevan untuk pengguna. Metrik ini memberikan informasi tentang seberapa akurat model dalam mengidentifikasi item-item yang memang sesuai dengan preferensi atau minat pengguna.
* *Precision*: *Precision* adalah rasio antara jumlah item yang relevan yang benar-benar direkomendasikan (*true positives*) dibagi dengan total jumlah item yang direkomendasikan (*true positives* + *false positives*). *Precision* = (Jumlah item relevan yang direkomendasikan) / (Total jumlah item yang direkomendasikan)
* @k: Nilai "k" mengacu pada jumlah item teratas yang direkomendasikan oleh model.

Pengukuran nilai presisi berdasarkan rating dari suatu artikel. Jika artikel yang direkomendasikan oleh sistem memiliki rating lebih dari 21 maka konten tersebut dianggap relevan. Sebaliknya jika kurang maka artikel tidak dianggap relevan. 

Pengukuran Precision@k pada model:
Berdasarkan hasil rekomendasi artikel dengan judul "An operating model for company-wide agile development", artikel yang dianggap relevan adalah artikel yang memiliki rating total sebesar 21 atau rata - rata rating secara keseluruhan. Model Content-Based Filtering merekomendasikan 10 item kepada seorang pengguna. Dari 10 item tersebut, 9 item dianggap relevan sesuai dengan preferensi pengguna. 

Precision@k dalam kasus ini adalah Precision@10, dan nilainya adalah 9/10 = 0.9 atau 90%.

Artinya, dari 10 item yang direkomendasikan, 90% di antaranya adalah item yang sesuai dengan preferensi pengguna. Semakin tinggi nilai Precision@k, semakin baik performa model Content-Based Filtering dalam merekomendasikan item yang relevan kepada pengguna.

## Referensi
* Arie Satia Dharma, Russel Buena Basadena Ayub Hutasoit, Rade Rido Pangaribuan, "Sistem RekomendasiMenggunakanItem-based Collaborative Filtering padaKonten Artikel Berita", vol 2, No 1 (April 2021)
* Dewa Ayu Putri Diah Pramestia dan I Wayan Santiyasa, "Penerapan Metode Content-Based Filtering dalam Sistem Rekomendasi Video Game", JNATIA Volume 1, Nomor 1, (November 2022) 
--------------------------------
```