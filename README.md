# 🦠 Analisis Sentimen Pemberitaan Penyakit Menular pada Portal Berita Online Menggunakan IndoBERT-CNN-BiLSTM
---

## Dataset & Kategori Sentimen
* **Dataset:** Dataset Epidemi berjumlah **17.444 baris data** berita online.
* **Distribusi Kelas Asli:** Negatif (**62.3%**), Positif (**22.9%**), Netral (**14.8%**).
* **Pelabelan Kelas:**
  * **`1` (Positif):** Berita mengindikasikan situasi wabah membaik atau membawa harapan.
  * **`0` (Netral):** Berita berupa informasi umum tanpa indikasi membaik/memburuk.
  * **`-1` (Negatif):** Berita melaporkan kejadian buruk yang berpotensi memperparah keadaan.

---

## Arsitektur Model Hybrid
Sistem dibangun menggabungkan kekuatan dari tiga arsitektur utama:
1. **IndoBERT:** Bertindak sebagai *Contextual Word Embedding* untuk menangani representasi bahasa Indonesia yang kompleks secara dua arah.
2. **CNN (Convolutional Neural Network):** Digunakan setelah lapisan IndoBERT untuk mereduksi dimensi ekstraksi fitur teks secara efisien.
3. **BiLSTM (Bidirectional LSTM):** Memproses urutan informasi dari arah maju dan mundur guna menangkap konteks teks secara utuh.

---

## Hasil Eksperimen & Perbandingan Evaluasi

Pengujian dilakukan menggunakan metode *Stratified K-Fold Cross Validation* dengan rasio data terbaik: **76% Training**, **4% Validation**, dan **20% Testing**.

### 1. Performa Model vs Teknik Undersampling
Eksperimen membandingkan model Hybrid asli dengan penambahan metode penyeimbangan data seperti *One-Sided Selection* (OSS), *Neighborhood Cleaning Rule* (NCR), dan *Random Undersampling* (RUS).

| Konfigurasi Metode | Akurasi | Precision | Recall | F1-Score | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Hybrid (Tanpa Undersampling)** | **86.29%** | **80.47%** | 80.36% | **80.41%** | 90.79% |
| Hybrid + NCR | 84.82% | 78.59% | **81.21%** | 79.61% | **91.62%** |
| Hybrid + OSS | 84.49% | 78.36% | 80.95% | 79.37% | 91.37% |
| Hybrid + RUS | 82.21% | 75.71% | 80.63% | 77.43% | 90.98% |
| Hybrid + OSS + RUS | 81.64% | 75.43% | 80.79% | 77.14% | 89.91% |
| Hybrid + NCR + RUS | 81.15% | 75.17% | 81.17% | 76.96% | 90.23% |

---

## Temuan Utama (Findings)

1. **Model Terbaik Tanpa Undersampling:** Arsitektur gabungan **IndoBERT–CNN–BiLSTM murni** tanpa intervensi *undersampling* terbukti memberikan hasil paling optimal dengan akurasi **86.29%** dan F1-Score **80.41%**.
2. **Efek Negatif Undersampling:** Penerapan teknik penyeimbangan data (NCR, OSS, RUS) justru secara konsisten **menurunkan kinerja akurasi global model**. Hal ini terjadi karena pemangkasan data pada kelas mayoritas (Negatif) membuang terlalu banyak informasi krusial yang dibutuhkan model *Deep Learning* untuk belajar.
3. **Ketahanan Model (*Robustness*):** Karakteristik sebaran data asli dinilai belum ekstrem (*not severe imbalance*), sehingga kapabilitas representasi kontekstual dari IndoBERT sudah cukup tangguh memisahkan batas kelas secara akurat tanpa manipulasi data.
