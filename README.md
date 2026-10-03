# Analisis Volume Kedua Lambung Kapal Katamaran

**Nama:** Daffa Aqila Putra
**NPM:** 25083010016
**Mata kuliah:** Analisis Numerik

Repository ini berisi penyelesaian Tugas 2: menghitung volume kedua lambung kapal katamaran fiberglass (untuk wisata pancing) menggunakan integrasi numerik.

## Soal dan Ketentuan

- Satu *dash-kosong* pada grid = 5 + 5 cm.
- Lantai kapal dianggap datar (tanpa *keel*), sehingga tampak depan semuanya persegi panjang.
- Volume *reserve buoyancy* tidak dihitung.
- Hitung volume kedua lambung kapal.

## Metode

1. **Skala.** Satu garis grid sepanjang 20 kotak memuat sekitar 85 pasang dash-kosong, sehingga 1 kotak = 85/20 x 10 cm = **42,5 cm**. Lambung utama (tanpa reserve buoyancy) = 16 kotak = **6,8 m**.
2. **Stasiun.** Lambung dibagi menjadi 32 interval (33 stasiun) dengan jarak antar stasiun setengah kotak, yaitu 21,25 cm.
3. **Data ukur.** Pada tiap stasiun dibaca lebar lambung (tampak atas) dan tinggi lambung (tampak samping) dari gambar.
4. **Luas penampang.** Karena penampang depan persegi panjang, A(x) = lebar x tinggi.
5. **Volume satu lambung.** V = integral A(x) dx, dihitung dengan:
   - Metode Trapesium
   - Metode Simpson 1/3 (jumlah interval genap, n = 32)
6. **Volume kedua lambung.** Kedua lambung dianggap identik, sehingga V total = 2 x V satu lambung.

## Hasil

| Metode | Satu lambung | Kedua lambung |
|---|---|---|
| Trapesium | 6,018 m³ | 12,035 m³ |
| Simpson 1/3 | 6,024 m³ | **12,048 m³** (sekitar 12.050 liter) |

Selisih kedua metode hanya sekitar 0,1%, sehingga hasilnya konsisten.

## Catatan

- Hasil bergantung pada pembacaan lebar dan tinggi dari gambar, sehingga selisih kecil (sekitar 1 sampai 3%) masih wajar.
- Volume reserve buoyancy di ujung buritan dan haluan tidak dihitung sesuai perintah soal.
- Untuk mengubah data, edit `pasang_per_20_kotak` dan `DATA` pada notebook, lalu jalankan ulang semua sel.

## Cara Menjalankan

1. Buka file `.ipynb` di Google Colab (atau Jupyter Notebook).
2. Pilih **Runtime > Restart and run all**.
3. Library yang dipakai: `numpy`, `pandas`, `matplotlib`.
