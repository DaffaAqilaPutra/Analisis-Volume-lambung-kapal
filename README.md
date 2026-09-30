# Tugas 2 - Volume Dua Lambung Katamaran

Menghitung volume kedua lambung katamaran dari gambar tampak atas dan tampak samping (tanpa *reserve buoyancy*).

## Metode
- 1 kotak grid = 40 x 40 cm (1 kotak berisi 4 *dash-kosong* @ 10 cm).
- Stasiun tiap 20 cm sepanjang 640 cm (di antara sekat *reserve buoyancy*).
- Lebar dari tampak atas, tinggi dari tampak samping. Dasar datar, jadi penampang persegi panjang: `A = lebar x tinggi`.
- Volume dihitung dengan aturan trapesium dan Simpson. Kedua lambung dianggap identik.

## Hasil
| | Volume |
|---|---|
| 1 lambung (Simpson) | 5,336 m³ |
| **2 lambung** | **10,67 m³** |

## Cara menjalankan
```bash
pip install numpy
python hitung_volume.py
```

## Berkas
- `hitung_volume.py` : kode perhitungan
- `data_stasiun.csv` : data hasil pembacaan gambar
