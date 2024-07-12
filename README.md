# PA-PC_202231086_202231086_Tarissa-Nurhapsari-Laksono_A

## Teori yang mendukung
Project yang saya buat yaitu Deteksi Daun. Ini mengimplementasikan segmentasi warna menggunakan OpenCV dan Matplotlib untuk mengidentifikasi buah dan daun dalam gambar. Teknik ini memanfaatkan ruang warna HSV untuk memisahkan warna dengan lebih baik daripada RGB, serta masking untuk membuang bagian gambar yang tidak relevan. Hasil segmentasi dievaluasi melalui visualisasi menggunakan Matplotlib, yang berguna untuk aplikasi seperti deteksi objek otomatis, analisis tanaman, dan ekstraksi fitur dalam pengolahan citra.

## Tahapan Menyelesaikan Projek 
### Import Library dan Load Gambar
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

image_path = 'buahh.jpg'
image = cv2.imread(image_path)
```
PENJELASAN : Dalam project ini, saya menggunakan beberapa library yaitu : `cv2` dari OpenCV untuk pengolahan citra, `numpy` (disingkat `np`) untuk operasi matriks dan array, serta `matplotlib.pyplot` (disingkat `plt`) untuk visualisasi seperti plot dan gambar. Pertama, mengimpor modul-modul ini dan memuat gambar dari file `buahh.jpg` menggunakan `cv2.imread()`.

### konversi Warna dan Pengolahan Citra
```python
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
```
PENJELASAN : cv2.cvtColor() digunakan untuk mengubah ruang warna gambar. cv2.COLOR_BGR2RGB mengubah gambar dari format BGR (yang digunakan oleh OpenCV) ke RGB (yang digunakan oleh Matplotlib). Dan cv2.COLOR_BGR2HSV mengubah gambar dari ruang warna BGR ke HSV (Hue, Saturation, Value), yang sering digunakan dalam pengolahan citra untuk memisahkan warna dengan baik.

### Segmentasi Warna untuk Buah dan Daun
```python
lower_brown = np.array([5, 10, 10])
upper_brown = np.array([20, 255, 255])

mask_fruit = cv2.inRange(hsv, lower_brown, upper_brown)

lower_green = np.array([35, 20, 50])
upper_green = np.array([85, 255, 255])

mask_leaf = cv2.inRange(hsv, lower_green, upper_green)
```
PENJELASAN : Nilai `lower_brown` dan `upper_brown` adalah ambang bawah dan atas dalam format HSV yang menentukan rentang warna coklat untuk buah. Fungsi `cv2.inRange()` digunakan untuk membuat mask (masking) di mana nilai piksel dalam rentang yang ditentukan (dari `lower_brown` hingga `upper_brown`) akan diterima, sementara nilai lainnya akan dibuang. Sama seperti buah, kita mendefinisikan rentang warna hijau untuk daun dengan `lower_green` dan `upper_green`, kemudian membuat mask dengan `cv2.inRange()`.

### Segmentasi Citra
```python
# Membuat gambar segmentasi buah
fruit_segment = np.zeros_like(image_rgb)
fruit_segment[mask_fruit != 0] = image_rgb[mask_fruit != 0]

# Membuat gambar segmentasi daun
leaf_segment = np.zeros_like(image_rgb)
leaf_segment[mask_leaf != 0] = image_rgb[mask_leaf != 0]
```
PENJELASAN : `fruit_segment` dan `leaf_segment` adalah gambar hasil segmentasi yang akan menunjukkan hanya bagian buah dan daun dari gambar asli. Fungsi `np.zeros_like(image_rgb)` digunakan untuk membuat array nol dengan dimensi yang sama seperti `image_rgb`. Kemudian, nilai piksel dari `image_rgb` yang sesuai dengan mask `mask_fruit` atau `mask_leaf` yang tidak nol akan disalin ke `fruit_segment` atau `leaf_segment`, sesuai dengan mask yang relevan.

### Visualisasi dengan Matplotlib
```python
plt.figure(figsize=(20, 20))

plt.subplot(2, 2, 1)
plt.imshow(image_rgb)
plt.title("Original Image")
plt.axis('off')

plt.subplot(2, 2, 2)
plt.imshow(mask_fruit, cmap='gray')
plt.title("Mask of Fruits")
plt.axis('off')

plt.subplot(2, 2, 3)
plt.imshow(segmented_fruit)
plt.title("Segmented Fruits")
plt.axis('off')

plt.subplot(2, 2, 4)
plt.imshow(segmented_leaf)
plt.title("Segmented Leaves")
plt.axis('off')

plt.show()
```
PENJELASAN : `plt.figure(figsize=(20, 20))` membuat sebuah figure dengan ukuran 20x20 inci, menyediakan kanvas besar untuk visualisasi. `plt.subplot(2, 2, 1)` menentukan bahwa figure tersebut memiliki grid 2x2 subplot, dan kita sedang berada di subplot pertama. Fungsi `plt.imshow()` digunakan untuk menampilkan gambar dalam subplot tersebut, sementara `plt.title()` digunakan untuk memberikan judul pada subplot agar lebih informatif. Terakhir, `plt.axis('off')` digunakan untuk menyembunyikan sumbu x dan y pada gambar, sehingga tampilan gambar menjadi lebih bersih dan fokus pada kontennya. 
