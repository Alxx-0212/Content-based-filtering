# Laporan Proyek Machine Learning - Alexander Gosal
## Project Overview
Dalam era informasi digital yang kian pesat, jumlah konten yang tersedia sangatlah besar, sehingga pengguna sering kali kesulitan dalam menemukan informasi yang paling sesuai dengan minat dan kebutuhan mereka. Model ini dirancang untuk menganalisis karakteristik dan konten dari setiap item dalam koleksi data, seperti artikel, produk, atau konten lainnya, dan membandingkannya dengan preferensi pengguna. Dengan mempertimbangkan faktor-faktor seperti topik, gaya, atau atribut lainnya, model ini dapat memberikan rekomendasi yang lebih personal dan relevan secara otomatis. Tujuan utama dari model ini adalah untuk meningkatkan pengalaman pengguna dengan meminimalkan hambatan dalam mencari konten yang berguna, serta membantu pengguna mengeksplorasi topik yang mungkin belum mereka sadari sebelumnya. Selain itu, model *content-based* juga memiliki keunggulan dalam situasi di mana data interaksi pengguna terbatas, seperti pada platform baru atau ketika privasi pengguna menjadi perhatian utama. Dengan demikian, artikel ini akan membahas konsep, metode, dan manfaat dari model sistem rekomendasi content based dalam mendukung pengalaman pengguna yang lebih baik di dunia digital yang semakin kompleks.

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

