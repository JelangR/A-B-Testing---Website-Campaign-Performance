# A/B Testing Analysis: Facebook Ads Campaign Performance

Analisis eksperimen A/B pada dua campaign iklan Facebook (`Control Campaign`
vs `Test Campaign`) untuk mengevaluasi mana yang lebih efektif mendorong
purchase di website, dari sisi statistik maupun efisiensi biaya.

## Business Problem

Perusahaan menjalankan dua strategi campaign iklan secara bersamaan selama
30 hari dan ingin tahu:
1. Apakah `Test Campaign` menghasilkan purchase yang **signifikan lebih
   banyak** dibanding `Control Campaign`?
2. Kalau tidak, campaign mana yang lebih **efisien** dari sisi biaya dan
   kualitas traffic?

## Dataset

30 hari data harian teragregasi per campaign (1–30 Agustus 2019), dengan
kolom: `Spend`, `Impressions`, `Reach`, `Website Clicks`, `Searches`,
`View Content`, `Add to Cart`, `Purchase`. Sumber: [AB Testing Dataset -
Kaggle](https://www.kaggle.com/datasets/amirmotefaker/ab-testing-dataset).

## Metodologi

1. **Data cleaning** — 1 baris dengan data tidak lengkap (5 Agustus,
   `Control Campaign`) dikeluarkan dari analisis.
2. **Feature engineering** — membangun metrik funnel: CTR, Search Rate,
   View Rate, Cart Rate, Purchase Rate, Purchase Rate per Click, dan CPA
   (Cost per Acquisition).
3. **Uji statistik** — karena kedua campaign berjalan pada tanggal yang
   sama, digunakan **paired t-test** dan **Wilcoxon signed-rank test**
   (bukan independent t-test) untuk membandingkan purchase harian.
   Normalitas selisih dicek dengan **Shapiro-Wilk test** sebelum memilih
   uji yang tepat.

## Key Findings

| Metrik | Control Campaign | Test Campaign |
|---|---|---|
| Rata-rata purchase/hari | 522.8 | 512.7 |
| CTR (click-through rate) | 5.10% | 10.24% |
| Purchase rate (add to cart → purchase) | 45.7% | 61.8% |
| Cart rate (purchase → add to cart ratio) | 2.86 | 1.74 |
| CPA (cost per acquisition) | $5.05 | $6.00 |

**Signifikansi statistik:**
- Shapiro-Wilk pada selisih purchase: p = 0.380 (terdistribusi normal)
- Paired t-test: t = -0.206, **p = 0.838**
- Wilcoxon signed-rank: **p = 0.964**
- Paired t-test pada CPA: p = 0.152

➡️ **Tidak ada perbedaan signifikan secara statistik** pada jumlah purchase
harian antara kedua campaign (p >> 0.05 di kedua uji).

## Interpretasi Bisnis

Meskipun jumlah purchase nyaris identik, cara kedua campaign mencapainya
sangat berbeda:

- **Control Campaign** menjangkau audiens jauh lebih luas (reach & impressions
  lebih tinggi) tapi dengan CTR rendah — strategi "broad reach".
- **Test Campaign** menjangkau audiens yang jauh lebih kecil namun dua kali
  lebih efisien menarik klik (CTR 10.24% vs 5.10%) dan mengonversi klik
  menjadi purchase lebih baik — strategi "precision targeting".
- Dari sisi biaya, **Control Campaign sedikit lebih hemat** (CPA $5.05 vs
  $6.00), meski selisih ini juga tidak signifikan secara statistik.

**Rekomendasi**: karena tidak ada pemenang yang jelas dari sisi volume
purchase maupun biaya, keputusan strategi sebaiknya disesuaikan dengan
tujuan bisnis — pilih `Control Campaign` jika prioritasnya adalah brand
awareness/reach, atau `Test Campaign` jika prioritasnya efisiensi
targeting dengan budget lebih kecil.

## Visualizations

**Perbandingan distribusi purchase (boxplot)**
![Boxplot Purchase Comparison](images/boxplot_purchase_comparison.png)

**Distribusi purchase per campaign**
![Distribution Purchase](images/distribution_purchase.png)

**Tren purchase harian**
![Daily Trend](images/daily_trend_purchase.png)

**Perbandingan conversion rate di setiap tahap funnel**
![Funnel Comparison](images/funnel_comparison.png)

## Tools

- **Python**: pandas, numpy, scipy (statistical testing), matplotlib, seaborn
- **Tests**: Shapiro-Wilk, Paired t-test, Wilcoxon signed-rank test

## Repo Structure

```
├── A_B_Testing_Best_Campaign_Web_Purchase.ipynb   # notebook analisis lengkap
├── images/                                        # chart hasil visualisasi
├── data/                                          # control_group.csv, test_group.csv
└── README.md
```

## How to Run

```bash
pip install pandas numpy scipy matplotlib seaborn
jupyter notebook A_B_Testing_Best_Campaign_Web_Purchase.ipynb
```

---
*Dataset publik dari Kaggle, digunakan untuk tujuan pembelajaran/portofolio.*
