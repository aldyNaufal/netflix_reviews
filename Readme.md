# Google Play Store Review Scraper & Sentiment Analysis: Netflix App

## 1. Domain Proyek: Sentiment Analysis Ulasan Netflix

Pada era digital saat ini, *user-generated reviews* di platform seperti Google Play Store menjadi sumber data utama dalam memahami pengalaman pengguna terhadap aplikasi mobile. Studi terbaru oleh Ama (2025) mengungkap bahwa lewat **sentiment analysis** dan pemodelan statistik terhadap 13.500 ulasan Netflix, ditemukan bahwa sekitar **61.9% ulasan bersifat positif**, dan skor sentimen terbukti menjadi prediktor kuat terhadap rating bintang pengguna ([Preprints][1], [Scilit][2]). Menariknya, hal ini mempertegas bahwa analisis sentimen dapat memberikan wawasan lebih dalam mengenai kepuasan pengguna, di luar sekedar angka rating.

Selain itu, penelitian yang membandingkan ulasan aplikasi (termasuk Netflix) menunjukkan bahwa masalah pada *algorithmic decision-making* seringkali terungkap lewat sentimen negatif yang diungkapkan pengguna di ulasan, yang berdampak pada desain antarmuka dan interaksi sistem ([arXiv][3]). Sementara itu, tinjauan umum terhadap metode sentiment analysis di Google Play Store menekankan manfaat bagi pengembang aplikasi dalam memahami opini pengguna, termasuk preferensi fitur dan pengalaman pengguna secara real-time ([SpringerLink][4], [AppTweak][5]).

Dengan dasar tersebut, proyek ini bertujuan membangun model **analisis sentimen ulasan Netflix** berbahasa Inggris dari Google Play Store menggunakan pendekatan **lexicon-based** (misalnya VADER), dilanjutkan dengan **balancing data**, ekstraksi fitur (TF-IDF, BoW, Word2Vec), serta pelatihan **berbagai model klasifikasi** — mulai dari MLP hingga Random Forest — untuk menghasilkan klasifikasi saudara **positif, netral, negatif** dengan akurasi tinggi.

Tujuan akhir dari proyek ini adalah menghasilkan model sentiment analysis yang **akurat dan seimbang**, yang dapat membantu tim produk dan customer service dalam:

* Memahami persepsi pengguna secara kuantitatif dan kualitatif.
* Menyoroti fitur atau permasalahan yang ramai dikomentari pengguna.
* Mendukung pengambilan keputusan berbasis data untuk peningkatan aplikasi.





---

## 2. Business Understanding

Ulasan pengguna di Google Play Store adalah sumber data berharga untuk memahami pengalaman pengguna Netflix. Melalui analisis sentimen, perusahaan dapat:

* Mengidentifikasi keluhan paling umum.
* Menilai tingkat kepuasan pengguna.
* Memberikan masukan bagi pengembangan fitur dan perbaikan layanan.

**Tujuan bisnis:**

* Menyediakan laporan berbasis data terkait persepsi pengguna.
* Mempercepat respon terhadap isu yang sering muncul.

---

## 3. Data Understanding

### 3.1 Dataset Overview

* **Sumber**: Google Play Store – aplikasi Netflix (`com.netflix.mediaclient`)
* **Jumlah Review Terambil**: ±10.000 ulasan (bahasa Inggris, negara US)
* **Periode**: Review terbaru berdasarkan sorting `Sort.NEWEST`

### 3.2 Fitur Dataset

| Kolom         | Deskripsi                           |
| ------------- | ----------------------------------- |
| `Review Text` | Isi ulasan pengguna                 |
| `Score`       | Rating 1–5                          |
| `Date`        | Tanggal ulasan ditulis              |
| `Sentiment`   | Label sentimen hasil analisis VADER |

### 3.3 Statistik Umum Sebelum Balancing

Berdasarkan label sentimen awal (VADER):

* **Positive**: 6026 ulasan (60.3%)
* **Negative**: 2109 ulasan (21.1%)
* **Neutral**: 1865 ulasan (18.6%)

![Distribusi Sentimen](928c9d37-1a44-4519-97c3-9fb3a4d4f387.png)

---

## 4. Data Preparation

1. **Pembersihan Teks**

   * Menghapus URL, mention, hashtag, angka, emoji, dan tanda baca.
   * Normalisasi slang → kata baku (bahasa Inggris).
   * Koreksi ejaan dengan `pyspellchecker`.
   * Tokenisasi → Stopword removal → Lemmatization.

2. **Label Sentimen**

   * Menggunakan **VADER SentimentIntensityAnalyzer** (`compound_score`)

     * ≥ 0.05 → Positive
     * ≤ -0.05 → Negative
     * Selain itu → Neutral

3. **Data Augmentation (Balancing)**

   * Oversampling kelas minoritas (Negative & Neutral) agar jumlah seimbang dengan Positive.

4. **Ekstraksi Fitur**

   * **TF-IDF Vectorizer** (`max_features=5000`)
   * **Bag of Words (CountVectorizer)** (`max_features=5000`)
   * **Word2Vec** untuk embedding sequence token (vector\_size=100).

---

## 5. Modeling and Result

Model yang dilatih:

* **Random Forest (BiLSTM + W2V)**
* **XGBoost (BiLSTM + W2V)**
* **MLP (BoW)**
* **MLP (TF-IDF)**
* **Random Forest (BoW)**
* **Random Forest (TF-IDF)**

**Hasil akurasi**:

![Perbandingan Akurasi Model](55059c7f-5231-40c1-92ca-3bd0bf44f71a.png)

| Model                        | Akurasi |
| ---------------------------- | ------- |
| Random Forest (BiLSTM + W2V) | 0.91    |
| XGBoost (BiLSTM + W2V)       | 0.91    |
| MLP (BoW)                    | 0.95    |
| MLP (TF-IDF)                 | 0.94    |
| Random Forest (BoW)          | 0.90    |
| Random Forest (TF-IDF)       | 0.95    |

**Model terbaik**:

* **MLP (BoW)** dan **Random Forest (TF-IDF)** keduanya mencapai akurasi **0.95**.

---

## 6. Evaluation

* **Confusion Matrix** digunakan untuk mengevaluasi per kelas sentimen.
* **Precision, Recall, dan F1-score** menunjukkan model terbaik konsisten memberikan prediksi positif & negatif dengan kesalahan minimal.
* **ROC-AUC** di atas 0.9 untuk model terbaik → indikasi performa klasifikasi yang sangat baik.
---


## 📖 **Referensi**

* Ama, N. (2025). *A Combined Sentiment and Statistical Analysis for Netflix User Reviews*. Preprints.org. DOI: 10.20944/preprints202507.1906.v1 ([Preprints][1], [Scilit][2])
* Eiband, M., Völkel, S. T., Buschek, D., Cook, S., & Hussmann, H. (2020). A method and analysis to elicit user-reported problems in intelligent everyday applications. *arXiv*. ([arXiv][3])
* Samanmali, P. H. C., & Rupasingha, R. A. H. M. (2024). Sentiment analysis on Google Play Store app users’ reviews based on deep learning approach. *Multimedia Tools and Applications*, 83, 84425–84453. ([SpringerLink][4])
* Shepherd, G. (2024, Desember 30). Understanding Sentiment Analysis in App Reviews. *AppTweak Blog*. ([AppTweak][5])

[1]: https://www.preprints.org/manuscript/202507.1906/v1?utm_source=chatgpt.com "A Combined Sentiment and Statistical Analysis for Netflix User Reviews"
[2]: https://www.scilit.com/publications/4724f890c3638c92ab42b58e18c149c6?utm_source=chatgpt.com "A Combined Sentiment and Statistical Analysis for Netflix User Reviews"
[3]: https://arxiv.org/abs/2002.01288?utm_source=chatgpt.com "A Method and Analysis to Elicit User-reported Problems in Intelligent Everyday Applications"
[4]: https://link.springer.com/article/10.1007/s11042-024-19185-w?utm_source=chatgpt.com "Sentiment analysis on google play store app users’ reviews based on ..."
[5]: https://www.apptweak.com/en/aso-blog/use-app-review-sentiment-analysis-to-make-product-decisions?utm_source=chatgpt.com "Use App Review Sentiment Analysis to Make Product Decisions"

