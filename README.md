# Makine Öğrenmesi ile Ağ Saldırısı Tespiti (IDS) — NSL-KDD

Ağ bağlantılarının trafik özelliklerinden bir bağlantının **normal** mi **saldırı** mı olduğunu ve saldırı türünü (DoS, Probe, R2L, U2R) tahmin eden makine öğrenmesi projesi. Denetimli modeller (Logistic Regression, Random Forest, XGBoost) ile yalnızca normal trafikle eğitilen bir **autoencoder** karşılaştırılmış, ikisini birleştiren hibrit bir yapı denenmiştir.

- **Notebook:** [`notebooks/ids_nsl_kdd.ipynb`](notebooks/ids_nsl_kdd.ipynb) — veri keşfi, ön işleme, model eğitimi, değerlendirme ve hata analizi (çıktılarıyla birlikte)
- **Rapor:** [`report/IDS_Proje_Raporu.pdf`](report/IDS_Proje_Raporu.pdf)
- **Veri seti:** NSL-KDD — Tavallaee vd. (2009), Canadian Institute for Cybersecurity, https://www.unb.ca/cic/datasets/nsl.html

## Sonuçlar (KDDTest+)

**5 sınıflı sınıflandırma**

| Model | Accuracy | Macro Precision | Macro Recall | Macro F1 |
|---|---|---|---|---|
| Logistic Regression | 0.7608 | 0.6400 | 0.5607 | 0.5686 |
| Random Forest | 0.7634 | 0.7317 | 0.5020 | 0.4975 |
| XGBoost | 0.7853 | 0.8503 | 0.5384 | 0.5689 |

**İkili tespit (normal / saldırı)** — Hibrit = XGBoost veya Autoencoder saldırı derse alarm

| Model | Accuracy | Precision | Recall (DR) | F1 | FPR | ROC-AUC |
|---|---|---|---|---|---|---|
| Logistic Regression | 0.7966 | 0.9075 | 0.7156 | 0.8002 | 0.0964 | 0.8955 |
| Random Forest | 0.7932 | 0.9679 | 0.6585 | 0.7838 | 0.0288 | 0.9677 |
| XGBoost | 0.7981 | 0.9698 | 0.6661 | 0.7898 | 0.0274 | 0.9621 |
| Autoencoder | 0.9072 | 0.9236 | 0.9126 | 0.9180 | 0.0998 | 0.9701 |
| Hibrit | 0.9128 | 0.9241 | 0.9225 | 0.9233 | 0.1001 | – |

**Eğitimde hiç görülmemiş saldırı türlerinde tespit oranı:** XGBoost %34.4, Autoencoder %79.0, Hibrit %79.1.

## Çalıştırma

```bash
python -m venv .venv
.venv\Scripts\activate        # Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/ids_nsl_kdd.ipynb
```

Veri dosyaları (`KDDTrain+.txt`, `KDDTest+.txt`) `data/` klasöründe yoksa notebook ilk çalıştırmada otomatik indirir. Tüm çalıştırma, 16 çekirdekli bir bilgisayarda yaklaşık 20 dakika sürmektedir (çoğu hiperparametre araması). Python 3.14 ile test edilmiştir.

## Klasör yapısı

```
data/        NSL-KDD dosyaları (repoya eklenmez, otomatik indirilir)
notebooks/   ids_nsl_kdd.ipynb
figures/     notebook'un ürettiği grafikler
results/     metrik tabloları (CSV) ve summary.json
report/      proje raporu (PDF)
```
