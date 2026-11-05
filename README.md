# Prediksi Harga Cryptocurrency dengan Attention-LSTM

Proyek tugas akhir untuk mengeksplorasi prediksi harga penutupan harian **Bitcoin (BTC)**, **Ethereum (ETH)**, dan **BNB**. Proyek mencakup pengambilan dan pemrosesan data historis, pelatihan model Attention-LSTM, evaluasi pada data uji, serta eksperimen pembanding menggunakan TimeGPT.

> **Catatan:** Ini adalah proyek pembelajaran dan eksperimen, bukan nasihat atau rekomendasi investasi. Pergerakan harga cryptocurrency sangat tidak pasti; hasil historis tidak menjamin hasil di masa depan.

## Ringkasan hasil

Attention-LSTM menggunakan harga penutupan 60 hari sebelumnya untuk memprediksi satu hari berikutnya. Berikut metrik pada data uji yang tersimpan di repo:

| Aset | RMSE | MAE | MAPE |
|---|---:|---:|---:|
| BTC-USD | 8,420.03 | 7,084.39 | 6.88% |
| ETH-USD | 170.69 | 125.64 | 4.10% |
| BNB-USD | 69.84 | 49.02 | 5.90% |

RMSE dan MAE mengikuti satuan harga masing-masing aset, sehingga tidak tepat untuk dibandingkan langsung antar-aset. MAPE menyatakan galat persentase absolut rata-rata. Hasil TimeGPT merupakan eksperimen terpisah dan tidak ditampilkan dalam tabel ini.

## Visualisasi

### Prediksi Attention-LSTM

| BTC-USD | ETH-USD | BNB-USD |
|---|---|---|
| ![Prediksi BTC-USD dengan Attention-LSTM](figures/BTC%20Actual%20vs%20pred.png) | ![Prediksi ETH-USD dengan Attention-LSTM](figures/ETH%20Actual%20vs%20pred.png) | ![Prediksi BNB-USD dengan Attention-LSTM](figures/BNB%20Actual%20vs%20pred.png) |

### Loss pelatihan dan validasi

| BTC-USD | ETH-USD | BNB-USD |
|---|---|---|
| ![Loss BTC-USD](figures/BTC%20Train%20and%20val%20loss.png) | ![Loss ETH-USD](figures/ETH%20Train%20and%20val%20loss.png) | ![Loss BNB-USD](figures/BNB%20Train%20and%20val%20loss.png) |

## Alur proyek

1. `notebooks/01_download_yahoo.ipynb` — mengambil data harian dari Yahoo Finance.
2. `notebooks/02_pre-processing.ipynb` — menyelaraskan dan menyiapkan data deret waktu serta window 60 hari.
3. `notebooks/03_train_attention_lstm_close_{BTC,ETH,BNB}_H1.ipynb` — melatih dan mengevaluasi Attention-LSTM untuk tiap aset.
4. `notebooks/04_timegpt_{BTC,ETH,BNB}_close_H1.ipynb` — menjalankan eksperimen TimeGPT. Perlu API key Nixtla.

Notebook dijalankan berurutan dari folder `notebooks/`. Notebook pelatihan dan preprocessing menggunakan path relatif terhadap folder tersebut.

## Struktur repository

```text
.
├── data/                 # Data mentah dan hasil preprocessing (tidak dilacak Git)
├── figures/              # Grafik hasil eksperimen
├── models/
│   └── attention_lstm/    # CSV metrik; file model tidak dilacak Git
├── notebooks/            # Notebook pengambilan data, preprocessing, dan eksperimen
├── requirements.txt      # Dependensi untuk alur utama
└── README.md
```

Dataset dan file model tidak disertakan karena ukurannya besar. Grafik dan CSV metrik yang dirujuk di README disertakan agar hasil proyek tetap dapat ditinjau tanpa menjalankan ulang training.

## Menjalankan alur utama

Disarankan menggunakan Python 3.10 atau 3.11. Dari root repository:

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

Kemudian buka notebook di `notebooks/` dan jalankan sesuai urutan pada bagian alur proyek. Notebook pengambilan data memerlukan koneksi internet. Waktu training bergantung pada perangkat yang digunakan.

### Eksperimen TimeGPT (opsional)

Eksperimen ini menggunakan layanan Nixtla dan memerlukan API key tersendiri. Pasang paket tambahannya dan simpan key sebagai environment variable; jangan menaruh key di notebook atau di Git:

```powershell
pip install nixtla
$env:NIXTLA_API_KEY = "isi_key_anda"
```

Notebook TimeGPT juga memerlukan file data panjang yang dihasilkan oleh preprocessing. Periksa nama dan lokasi file input di notebook sebelum menjalankannya.

## Batasan

- Model hanya mengevaluasi satu langkah ke depan pada aset dan rentang data yang digunakan dalam eksperimen.
- Nilai metrik dipengaruhi periode data uji, volatilitas, dan satuan harga.
- Data bersumber dari Yahoo Finance dan dapat berubah atau memiliki keterbatasan ketersediaan.
- TimeGPT bergantung pada layanan eksternal, API key, dan ketentuan penggunaan Nixtla.
- Hasil eksperimen ini tidak membuktikan kelayakan strategi trading atau performa di pasar nyata.
