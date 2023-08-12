# Laporan Proyek Machine Learning - Alexander Gosal
## Project Overview
Latar belakang pembuatan model rekomendasi content-based filtering untuk rekomendasi artikel berasal dari tantangan dalam menghadapi ledakan informasi di era digital. Dengan munculnya internet dan platform berita daring, jumlah artikel yang tersedia melebihi kapasitas manusia untuk menemukan dan menyaring informasi yang relevan. Pengguna sering kali merasa terjebak dalam gelembung informasi, hanya terpapar pada sudut pandang yang sudah dikenal, tanpa eksplorasi yang luas terhadap topik yang beragam. Model rekomendasi content-based muncul untuk mengatasi masalah ini dengan cara yang lebih cerdas dan personal. Dengan menganalisis karakteristik konten seperti topik, kata kunci, struktur, dan gaya penulisan, model ini dapat memahami esensi dari setiap artikel dan mencocokkannya dengan preferensi pengguna. Dalam konteks ini, tujuan utama adalah memberikan pengalaman membaca yang lebih bervariasi, meningkatkan pengetahuan pengguna dengan menyajikan artikel yang beragam namun tetap relevan dengan minat mereka, serta membantu pengguna mengeksplorasi sudut pandang yang berbeda, mempromosikan keragaman informasi.

Selain itu, penggunaan model rekomendasi content-based filtering juga melibatkan pertimbangan terhadap privasi pengguna. Model ini beroperasi berdasarkan analisis konten yang ada, sehingga tidak memerlukan informasi identitas atau perilaku pengguna lainnya. Ini menjadi penting dalam mengatasi kekhawatiran privasi yang semakin meningkat di era digital saat ini. Dengan menggunakan model content-based, pengguna dapat menerima rekomendasi yang berdasarkan pada apa yang telah mereka konsumsi tanpa memerlukan data pribadi atau histori interaksi yang mendalam. Keberlangsungan model ini juga terjamin karena tidak tergantung pada data interaksi pengguna yang terus-menerus. Hal ini membuat model content-based cocok untuk platform yang baru diluncurkan atau untuk situasi di mana data pengguna terbatas. Dengan demikian, model rekomendasi content-based filtering menjadi solusi yang cerdas dan berkelanjutan dalam menghadapi tantangan pencarian informasi yang efektif, meningkatkan pengetahuan pengguna, dan menghormati kebutuhan privasi mereka.

## Business Understanding
#### Problem Statement
Berdasarkan pada latar belakang di atas, permasalahan yang dapat diselesaikan pada proyek ini adalah bagaimana cara membangun sistem rekomendasi artikel menggunakan metode *content-based filtering* untuk meningkatkan pengalaman membaca pengguna.
#### Goal
Tujuan dari proyek ini adalah meningkatkan pengalaman pengguna dengan menyajikan rekomendasi konten yang lebih relevan, personal, dan menarik. 

## Data Understanding
#### Context
Deskdrop adalah platform komunikasi internal yang dikembangkan oleh CI&T, berfokus pada perusahaan yang menggunakan Google G Suite. Di antara fitur lainnya, platform ini memungkinkan karyawan perusahaan untuk berbagi artikel yang relevan dengan rekan mereka, dan berkolaborasi di sekitar mereka.
#### Content
[CI&T Deskdrop Dataset](https://www.kaggle.com/datasets/gspmoreira/articles-sharing-reading-from-cit-deskdrop?select=users_interactions.csv) berisi sampel log 12 bulan (Maret 2016 - Februari 2017) dari platform Komunikasi Internal CI&T (DeskDrop) dengan 73000 interaksi pengguna yang tercatat dari 3000 artikel publik yang dibagikan di platform.



## Referensi

