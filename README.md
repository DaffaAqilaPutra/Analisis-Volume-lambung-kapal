# 🚤 Tugas 2 — Volume Dua Lambung Katamaran

Perhitungan volume kedua lambung katamaran fiberglass dari gambar **tampak atas** dan **tampak samping** (grid), tanpa menghitung bagian *reserve buoyancy*.

> Sumber gambar: *Desain dan Konstruksi Perahu Katamaran Fiberglass untuk Wisata Pancing* (ResearchGate)

---

## 📋 Ringkasan Hasil

| | Volume (cm³) | Volume (m³) |
|---|---:|---:|
| 1 lambung (trapesium) | 5.330.500 | 5,331 |
| 1 lambung (Simpson) | 5.336.000 | 5,336 |
| **2 lambung (Simpson)** | **10.672.000** | **≈ 10,67 m³ (± 10.672 liter)** |

---

## 🧭 Metode

1. **Skala** — 1 *dash-kosong* = 5 + 5 cm = 10 cm. Satu kotak grid berisi 4 *dash-kosong*, sehingga **1 kotak = 40 × 40 cm**.
2. **Bagian yang dihitung** — dari sekat *reserve buoyancy* buritan sampai sekat *reserve buoyancy* haluan (**640 cm**). Kedua ujung (*reserve buoyancy*) tidak dihitung.
3. **Stasiun** — dipasang tiap setengah kotak (**Δx = 20 cm**), stasiun 4 s.d. 36 (33 stasiun, 32 interval).
4. **Lebar** dibaca dari tampak atas (tepi luar lambung).
5. **Tinggi** dibaca dari tampak samping (garis sheer sampai dasar lambung).
6. **Asumsi** — lantai datar (tanpa *keel*), sehingga tampak depan berupa persegi panjang:

   `A(x) = lebar(x) × tinggi(x)`

7. **Integrasi numerik** sepanjang lambung:

   - Trapesium: `V ≈ Δx · [A₀/2 + ΣAᵢ + Aₙ/2]`
   - Simpson 1/3: `V ≈ (Δx/3) · [A₀ + 4ΣA_ganjil + 2ΣA_genap + Aₙ]`

8. **Dua lambung** dianggap identik, sehingga `V_total = 2 × V_satu lambung`.

---

## 📁 Struktur Berkas

```
tugas2/
├── README.md          # dokumentasi ini
├── hitung_volume.py   # program perhitungan
└── data_stasiun.csv   # tabel hasil pembacaan gambar
```

---

## ▶️ Cara Menjalankan

```bash
pip install numpy
python hitung_volume.py
```

Keluaran program:

```text
Panjang lambung (tanpa reserve buoyancy): 640 cm
Volume 1 lambung (trapesium): 5,330,500 cm3 = 5.330 m3
Volume 1 lambung (Simpson)  : 5,336,000 cm3 = 5.336 m3
Volume 2 lambung (Simpson)  : 10,672,000 cm3 = 10.672 m3 = 10,672 liter
```

---

## 💻 Kode Program (`hitung_volume.py`)

```python
"""Tugas 2 - Volume kedua lambung katamaran (tanpa reserve buoyancy).

Skala  : 1 kotak grid = 4 x (dash-kosong 5+5 cm) = 40 cm x 40 cm
Stasiun: tiap setengah kotak (dx = 20 cm), stasiun 4 s.d. 36
         st.4  = sekat reserve buoyancy buritan, st.36 = sekat reserve buoyancy haluan
Lebar  : dibaca dari TAMPAK ATAS (tepi luar lambung)
Tinggi : dibaca dari TAMPAK SAMPING (sheer/atas sampai dasar lambung)
Asumsi : dasar datar (tanpa keel) -> penampang = persegi panjang, A = lebar x tinggi
         Kedua lambung identik.
"""
import csv
import numpy as np

DX = 20.0  # cm, jarak antar stasiun

# (stasiun, lebar_cm, tinggi_cm) - hasil pembacaan gambar, dibulatkan ke 5 cm
DATA = [
    (4, 70, 75),
    (5, 75, 85),
    (6, 75, 90),
    (7, 80, 95),
    (8, 85, 100),
    (9, 85, 105),
    (10, 85, 105),
    (11, 85, 105),
    (12, 85, 105),
    (13, 85, 105),
    (14, 85, 105),
    (15, 85, 105),
    (16, 85, 105),
    (17, 85, 105),
    (18, 85, 105),
    (19, 85, 105),
    (20, 85, 105),
    (21, 85, 105),
    (22, 85, 105),
    (23, 85, 105),
    (24, 85, 105),
    (25, 85, 105),
    (26, 85, 105),
    (27, 85, 105),
    (28, 85, 105),
    (29, 85, 105),
    (30, 80, 105),
    (31, 75, 105),
    (32, 75, 105),
    (33, 70, 105),
    (34, 65, 105),
    (35, 60, 105),
    (36, 50, 105)
]

st     = np.array([d[0] for d in DATA])
lebar  = np.array([d[1] for d in DATA], dtype=float)
tinggi = np.array([d[2] for d in DATA], dtype=float)
A      = lebar * tinggi                      # luas penampang (cm^2)

def trapesium(y, dx):
    return dx * (y[0] / 2 + y[1:-1].sum() + y[-1] / 2)

def simpson(y, dx):                          # butuh jumlah interval genap
    return dx / 3 * (y[0] + 4 * y[1:-1:2].sum() + 2 * y[2:-1:2].sum() + y[-1])

V_trap = trapesium(A, DX)
V_simp = simpson(A, DX)

print(f"Panjang lambung (tanpa reserve buoyancy): {(len(st)-1)*DX:.0f} cm")
print(f"Volume 1 lambung (trapesium): {V_trap:,.0f} cm3 = {V_trap/1e6:.3f} m3")
print(f"Volume 1 lambung (Simpson)  : {V_simp:,.0f} cm3 = {V_simp/1e6:.3f} m3")
print(f"Volume 2 lambung (Simpson)  : {2*V_simp:,.0f} cm3 = {2*V_simp/1e6:.3f} m3"
      f" = {2*V_simp/1000:,.0f} liter")

with open("data_stasiun.csv", "w", newline="") as f:
    w = csv.writer(f)
    w.writerow(["stasiun", "x_cm", "lebar_cm", "tinggi_cm", "luas_cm2"])
    for s, b, h, a in zip(st, lebar, tinggi, A):
        w.writerow([s, (s - st[0]) * DX, b, h, a])
```

---

## 📊 Data Pembacaan Gambar

| Stasiun | x (cm) | Lebar (cm) | Tinggi (cm) | Luas (cm²) |
|:---:|---:|---:|---:|---:|
| 4 | 0 | 70 | 75 | 5.250 |
| 5 | 20 | 75 | 85 | 6.375 |
| 6 | 40 | 75 | 90 | 6.750 |
| 7 | 60 | 80 | 95 | 7.600 |
| 8 | 80 | 85 | 100 | 8.500 |
| 9 | 100 | 85 | 105 | 8.925 |
| 10 | 120 | 85 | 105 | 8.925 |
| 11 | 140 | 85 | 105 | 8.925 |
| 12 | 160 | 85 | 105 | 8.925 |
| 13 | 180 | 85 | 105 | 8.925 |
| 14 | 200 | 85 | 105 | 8.925 |
| 15 | 220 | 85 | 105 | 8.925 |
| 16 | 240 | 85 | 105 | 8.925 |
| 17 | 260 | 85 | 105 | 8.925 |
| 18 | 280 | 85 | 105 | 8.925 |
| 19 | 300 | 85 | 105 | 8.925 |
| 20 | 320 | 85 | 105 | 8.925 |
| 21 | 340 | 85 | 105 | 8.925 |
| 22 | 360 | 85 | 105 | 8.925 |
| 23 | 380 | 85 | 105 | 8.925 |
| 24 | 400 | 85 | 105 | 8.925 |
| 25 | 420 | 85 | 105 | 8.925 |
| 26 | 440 | 85 | 105 | 8.925 |
| 27 | 460 | 85 | 105 | 8.925 |
| 28 | 480 | 85 | 105 | 8.925 |
| 29 | 500 | 85 | 105 | 8.925 |
| 30 | 520 | 80 | 105 | 8.400 |
| 31 | 540 | 75 | 105 | 7.875 |
| 32 | 560 | 75 | 105 | 7.875 |
| 33 | 580 | 70 | 105 | 7.350 |
| 34 | 600 | 65 | 105 | 6.825 |
| 35 | 620 | 60 | 105 | 6.300 |
| 36 | 640 | 50 | 105 | 5.250 |

---
