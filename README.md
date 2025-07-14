# Studi Komparatif Skenario Fine-Tuning Model Transformer untuk Analisis Sentimen Teks Berbahasa Indonesia

Repositori ini berisi kode dan notebook untuk penelitian Kerja Praktik yang bertujuan mengevaluasi berbagai strategi *fine-tuning* pada model *transformer* untuk tugas analisis sentimen berbahasa Indonesia.

## Abstrak

Penelitian ini melakukan studi komparatif untuk mengevaluasi tiga skenario *fine-tuning*—*Fine-Tuning* Standar, *Gradual Unfreezing*, dan *Differential Learning Rates*—pada tiga arsitektur model: IndoBERTbase, IndoBERTweet, dan RoBERTa. Pengujian dilakukan pada dua dataset dengan domain berbeda, yaitu ulasan aplikasi (Dataset BBM) dan komentar politik (Dataset Pemilu), dengan metrik evaluasi utama F1-Score. Hasil penelitian menunjukkan bahwa **IndoBERTweet** secara konsisten menjadi model dengan performa tertinggi, sementara strategi **Fine-Tuning Standar** dengan *learning rate* yang teroptimasi terbukti lebih unggul dibandingkan dua teknik lanjutan yang lebih kompleks.

## 🔑 Temuan Utama

1.  **Model Terbaik adalah IndoBERTweet:** Model ini secara konsisten mengungguli arsitektur lain. Pada skenario terbaiknya (*Fine-Tuning* Standar), IndoBERTweet mencapai F1-Score **0.9218** pada Dataset BBM dan **0.7431** 
2.  **Strategi Terbaik adalah *Fine-Tuning* Hyperparameter:** Skenario 1 dengan *learning rate* yang teroptimasi menghasilkan performa puncak. Sebagai contoh, IndoBERTweet pada Dataset BBM mencapai F1-Score **0.9218** dengan metode ini, melampaui hasil dari Skenario 3 (*Differential LR*, 0.9159) dan Skenario 2 (*Gradual Unfreezing*, 0.9042).
3.  ***Differential LR* Lebih Unggul dari *Gradual Unfreezing*:** Di antara dua teknik lanjutan, *Differential Learning Rates* (S3) terbukti jauh lebih robust. Pada model RoBERTa dengan Dataset BBM, Skenario 3 berhasil mencapai F1-Score **0.8530**, sementara Skenario 2 mengalami kegagalan performa dengan skor hanya **0.4890**, menunjukkan perbedaan efektivitas yang sangat besar.
4.  **Kualitas Data Sangat Berpengaruh:** Performa model secara drastis berbeda antar dataset. Skor F1-Score tertinggi pada Dataset BBM (**0.9218**) hampir **25% lebih tinggi** daripada skor tertinggi pada Dataset Pemilu (**0.7431**). Perbedaan ini juga tercermin pada *validation loss*, di mana *loss* terendah pada Dataset Pemilu (~0.64) jauh lebih tinggi daripada pada Dataset BBM (~0.27), mengindikasikan bahwa **kompleksitas domain, *noise*, dan ambiguitas data** merupakan tantangan besar dalam analisis sentimen politik.

---

## 📁 Struktur Proyek

```
kp-penelitian/
│
├── data/
│   ├── dataset_bbm.csv         # Dataset ulasan aplikasi BBM
│   └── dataset_pemilu.csv      # Dataset komentar seputar Pemilu
│
├── notebooks/
│   ├── 1_Skenario_Fine_Tuning.ipynb
│   ├── 2_Skenario_Gradual_Unfreezing.ipynb
│   └── 3_Skenario_Differential_LR.ipynb
│
├── .gitignore
└── README.md
```

---

## 🔬 Metodologi Eksperimen

Eksperimen dilakukan dengan membandingkan tiga model pada tiga skenario pelatihan:

#### Model yang Digunakan:
- **IndoBERTbase** (`indobenchmark/indobert-base-p1`)
- **IndoBERTweet** (`indobenchmark/indobertweet-base-p1`)
- **RoBERTa** (`roberta-base` atau varian Indonesia lainnya)

#### Skenario Pengujian:
1.  **Skenario 1: Fine-Tuning Standar & Optimasi LR:** Melatih seluruh model secara bersamaan dan mencari *learning rate* terbaik melalui *cross-validation*.
2.  **Skenario 2: Gradual Unfreezing:** Melatih model secara bertahap, dimulai dari *classifier head* lalu perlahan membuka lapisan-lapisan di bawahnya.
3.  **Skenario 3: Differential Learning Rates:** Melatih seluruh model secara bersamaan namun dengan *learning rate* yang berbeda untuk setiap grup lapisan.

## 🚀 Cara Menjalankan Eksperimen

Untuk mereproduksi hasil dari penelitian ini, ikuti langkah-langkah berikut:

#### 1. Prasyarat
- Python 3.8+
- `pip` dan `venv` (direkomendasikan)
- GPU dengan VRAM yang cukup (minimal 8GB direkomendasikan) untuk melatih model.

#### 2. Kloning Repositori
```bash
git clone https://github.com/rifqimaruf/kp-penelitian.git
cd kp-penelitian
```

#### 3. Setup Environment Virtual (Direkomendasikan)
```bash
# Membuat environment virtual
python -m venv venv

# Mengaktifkan environment (Windows)
.\venv\Scripts\activate

# Mengaktifkan environment (macOS/Linux)
source venv/bin/activate
```

## 📊 Hasil
Hasil detail dari setiap skenario, termasuk tabel perbandingan, F1-Score, dan kurva *loss* untuk setiap model dan dataset, dapat ditemukan dalam laporan akhir penelitian ini. Secara ringkas, kombinasi terbaik yang ditemukan adalah:
- **Model:** `IndoBERTweet`
- **Strategi:** `Fine-Tuning Standar`
- **Learning Rate Optimal:** `3e-05` (untuk kedua dataset)

## Catatan Peneliti
Menurut peneliti, terdapat tiga alasan mengapa skenario 2 dan 3 gagal melakukan optimasi performa baseline:
1. Baseline sudah diuji dengan beberapa versi learning rate dan divalidasi dengan 3-Fold sehingga hasilya menjadi tolak ukur yang tinggi.
2. Penentuan lyaer yang dibekukan pada skenario Gradual Unfreezing terlalu kaku, semestinya menyesuaikan lagi dengan arsitektur modelnya.
3. Kombinasi LR pada Differential learning rate masih belum dioptimalkan.
