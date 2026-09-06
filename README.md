# Telco Customer Churn Predict 
-----
Proyek ini bertujuan untuk membangun **Sistem Deteksi Dini berbasis Machine Learning** untuk memprediksi apakah pelanggan telekomunikasi berisiko *churn* (berhenti berlangganan dan pindah ke kompetitor). Lebih dari sekadar melakukan prediksi, proyek ini memanfaatkan kekuatan inferensi dari algoritma **Regresi Logistik** untuk mengekstrak wawasan statistik, mengidentifikasi variabel pasti yang mendorong pelanggan pergi, serta variabel yang sukses membangun loyalitas jangka panjang.

**Dataset:** Dataset diambil dari web opensource kaggle berikut
(https://www.kaggle.com/code/farazrahman/telco-customer-churn-logisticregression)

##  Metodologi & Pemrosesan Data
1. **Pembersihan Data:** Menangani nilai kosong (*missing values*) pada variabel numerik kontinu (`TotalCharges`).
2. **Encoding Fitur:** Menerapkan *One-Hot Encoding* (`drop_first=True`, `dtype=int`) untuk mengubah variabel kategorikal teks menjadi matriks biner, sekaligus menghindari *Dummy Variable Trap* (Multikolinearitas Sempurna).
3. **Train-Test Split:** Membagi dataset dengan rasio 80:20 (`random_state=42`, `stratify=y`) untuk memastikan model diuji pada data yang merepresentasikan kondisi populasi asli.
4. **Penanganan Data Tidak Seimbang (SMOTE):** Dataset asli sangat tidak seimbang (73% pelanggan Setia vs 27% pelanggan *Churn*). Algoritma **SMOTE** (*Synthetic Minority Over-sampling Technique*) diterapkan secara eksklusif pada data latih untuk menyeimbangkan jumlah kelas (masing-masing 4.130 sampel), mencegah model menjadi bias terhadap mayoritas.

##  Performa Model (Regresi Logistik)
Model ini dioptimalkan menggunakan metode kalkulasi *Maximum Likelihood Estimation* (MLE) dan dievaluasi pada 20% data uji (*test set*) yang belum pernah dilihat oleh mesin sebelumnya.

* **Akurasi (Accuracy):** **77%** (Skor klasifikasi *baseline* yang solid).
* **Recall (Kelas 1 - Churn):** **68%** 
  * *Dampak Bisnis:* Model berhasil mendeteksi 68% dari seluruh pelanggan yang *benar-benar* akan kabur. Dalam kasus *churn*, *Recall* yang tinggi sangat penting agar perusahaan tidak kecolongan kehilangan pelanggan bernilai tinggi.
* **Precision (Kelas 1 - Churn):** **55%**

## Wawasan Bisnis Utama (Analisis Koefisien)
Dengan mengekstrak nilai koefisien Beta ($\beta$) dari persamaan Regresi Logistik, kita dapat mengidentifikasi faktor pendorong  dan penghambat *churn* yang sebenarnya berdasarkan besaran matematisnya:

### Faktor Penahan Utama (Pembangun Loyalitas)
Variabel-variabel ini memiliki angka koefisien negatif yang tinggi, yang berarti secara drastis mengurangi probabilitas matematis (*log-odds*) pelanggan untuk kabur:
1. **Layanan Telepon (Phone Service):** (Jangkar terkuat, $\beta \approx -4.0$) Pelanggan yang terintegrasi dengan infrastruktur telekomunikasi dasar menunjukkan tingkat loyalitas yang luar biasa.
2. **Internet Fiber Optic:** Kualitas infrastruktur terbukti mempertahankan pelanggan jauh lebih baik daripada layanan DSL dasar.
3. **Kontrak 2 Tahun:** Keterikatan komitmen secara legal dan finansial adalah pencegah *churn* yang masif.
4. **Tech Support & Online Security:** Layanan keamanan dan dukungan teknis membuat pelanggan merasa aman dan dilayani dengan baik.

### Faktor Pendorong Utama (Risiko Churn)
Fitur-fitur yang berlabel **"No internet service"** (seperti tidak ada *streaming TV*, tidak ada *online backup*) menunjukkan koefisien positif ($\beta \approx 0.38$). Akibat fenomena multikolinearitas, fitur-fitur yang identik ini berbagi bobot nilai yang sama. Besaran pendorong (0.38) ini relatif sangat lemah jika dibandingkan dengan faktor penahannya (-4.0). Pelanggan tanpa internet memang sedikit lebih rentan untuk pergi, tetapi *insight* utamanya adalah: **Retensi pelanggan akan jauh lebih sukses jika kita mendorong pelanggan untuk mengambil layanan penahan**, bukan sekadar memperbaiki layanan pendorongnya.

## Rekomendasi Strategis untuk Manajemen
Alih-alih hanya berfokus memperbaiki layanan skala kecil, strategi retensi Telkom harus diprioritaskan pada upaya mengunci loyalitas melalui infrastruktur dasar (Layanan Telepon & Internet Fiber Optic) serta komitmen jangka panjang (Kontrak 2 Tahun). Secara matematis, jangkar retensi ini terbukti memiliki kekuatan mencegah churn hingga 10 kali lipat lebih besar dibandingkan alasan-alasan yang mendorong pelanggan untuk pergi. Berikut hal-hal yang dapat dilakukan oleh pihak Telkom:
1. **Upselling Tertarget:** Arahkan anggaran pemasaran untuk mendorong pelanggan langganan bulanan beralih ke Kontrak 1 atau 2 Tahun dengan menawarkan diskon yang memikat.
2. **Bundling Layanan Ekstra:** Tawarkan *Tech Support* dan *Online Security* sebagai tambahan layanan standar dan gratis di bulan-bulan pertama untuk membangun rasa saling percaya dan ketergantungan.

## Library yang Digunakan
* **Python** (Pandas, NumPy)
* **Scikit-Learn** (Logistic Regression, Evaluation Metrics, Train-Test Split)
* **Imbalanced-learn** (SMOTE)
* **Visualisasi Data:** Matplotlib, Seaborn
