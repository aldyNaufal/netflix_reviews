# Google Play Store Review Scraper & Sentiment Analysis: Netflix App


## 1. Domain Proyek: Sentiment Analysis Ulasan Aplikasi Netflix

Ulasan pengguna di platform seperti Google Play Store kini menjadi sumber data penting untuk memahami pengalaman pengguna terhadap aplikasi mobile (Samanmali & Rupasingha, 2024). Analisis sentimen terhadap review ini memungkinkan pengembang untuk mengidentifikasi kepuasan, keluhan, dan isyarat penting lainnya secara real-time.

Penelitian oleh Ama (2025) menunjukkan bahwa dari 13.500 ulasan Netflix yang ditelaah, sekitar **61,9 % bersifat positif**. Hasil analisis sentimen ini terbukti menjadi prediktor kuat terhadap rating bintang pengguna dan engagement seperti jumlah “thumbs-up” (Ama, 2025).

Dalam studi yang lebih umum, Samanmali dan Rupasingha (2024) menyampaikan bahwa teknik berbasis **deep learning** seperti LSTM menghasilkan akurasi antara **80–90 %** dalam klasifikasi sentimen, hal ini menunjukkan bahwa metode ini menghasilkan akurasi lebih tinggi dibanding metode klasik seperti SVM dan ANN.

Dengan dasar tersebut, proyek ini dirancang untuk membangun model **analisis sentimen ulasan aplikasi Netflix** berbahasa Inggris berdasarkan review dari Google Play Store. Pendekatan yang digunakan meliputi:

* **Lexicon-based sentiment labeling** menggunakan VADER,
* **Data balancing** melalui oversampling,
* **Ekstraksi fitur** menggunakan TF-IDF, BoW, dan Word2Vec,
* Pelatihan model klasifikasi seperti MLP, Random Forest, dan lainnya.

Tujuan utama proyek ini adalah menciptakan model sentiment analysis yang **akurat dan seimbang**, guna membantu tim produk dan customer service dalam:

* Mengevaluasi persepsi pengguna secara kuantitatif dan kualitatif,
* Mengidentifikasi fitur atau isu yang sering dikomentari oleh pengguna,
* Memberikan dasar pengambilan keputusan berbasis data untuk peningkatan aplikasi.






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

* Ama, N. (2025). *A Combined Sentiment and Statistical Analysis for Netflix User Reviews*. Preprints.org. [https://doi.org/10.20944/preprints202507.1906.v1](https://doi.org/10.20944/preprints202507.1906.v1)
* Samanmali, P. H. C., & Rupasingha, R. A. H. M. (2024). Sentiment analysis on Google Play Store app users’ reviews based on deep learning approach. *Multimedia Tools and Applications*, 83, 84425–84453. [https://doi.org/10.1007/s11042-024-19185-w](https://doi.org/10.1007/s11042-024-19185-w)

