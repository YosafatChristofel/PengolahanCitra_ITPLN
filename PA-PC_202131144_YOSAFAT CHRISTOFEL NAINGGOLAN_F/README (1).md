
# Word Segmentation

#### Apa itu Word Segmentation ? 

Word segmentation dalam pengolahan citra adalah proses membagi teks dalam gambar menjadi unit-unit kata yang terpisah. Tujuan dari word segmentation adalah untuk mengidentifikasi batas-batas antara kata-kata dalam gambar teks sehingga kata-kata tersebut dapat dikenali dan diproses secara terpisah.

Dalam konteks pengolahan citra, word segmentation sering digunakan dalam aplikasi pengenalan karakter optik (Optical Character Recognition/OCR) atau pemrosesan dokumen. Misalnya, saat mengenali teks dalam dokumen yang dihasilkan oleh pemindai (scanner), word segmentation memungkinkan untuk mengidentifikasi dan memisahkan kata-kata dalam gambar teks sehingga setiap kata dapat dikenali secara individu.

Proses word segmentation melibatkan serangkaian langkah yang kompleks. Beberapa metode umum yang digunakan dalam word segmentation antara lain:

- Segmentasi berbasis piksel: Metode ini menggunakan informasi piksel dalam gambar teks untuk mengidentifikasi dan memisahkan karakter dan kata-kata. Contohnya, metode seperti segmentasi berdasarkan ambang (thresholding), segmentasi berbasis tepi (edge-based segmentation), atau segmentasi berbasis tekstur (texture-based segmentation) dapat digunakan untuk memisahkan karakter dan kata-kata berdasarkan perbedaan intensitas, tekstur, atau fitur tepi dalam gambar.

- Segmentasi berbasis objek: Metode ini melibatkan deteksi dan pemisahan objek dalam gambar berdasarkan fitur-fitur seperti bentuk, ukuran, atau konteks. Dalam konteks word segmentation, objek yang dimaksud adalah karakter dan kata-kata. Metode seperti segmentasi berbasis region (region-based segmentation) atau segmentasi berbasis kontur (contour-based segmentation) dapat digunakan untuk mengenali dan memisahkan karakter dan kata-kata berdasarkan fitur-fitur tersebut.

- Segmentasi berbasis pemodelan: Metode ini melibatkan pemodelan statistik atau pemodelan bahasa untuk memahami struktur dan pola dalam teks. Metode seperti segmentasi berbasis pemodelan Markov tersembunyi (Hidden Markov Model/HMM) atau pemodelan n-gram dapat digunakan untuk mengenali dan memisahkan kata-kata berdasarkan kemungkinan urutan kata yang mungkin dalam teks.


### Word Segmentation menggunakan bahasa Python 

import matplotlib.pyplot as plt
import matplotlib.patches as patches
from skimage import io, color, measure

def detect_letters(image_path):
    image = io.imread(image_path)
    gray = color.rgb2gray(image)
    threshold = gray < 0.5
    clean_image = measure.label(threshold)
    
    contours = measure.find_contours(clean_image, 0.8)
    
    detected_letters = []
    for contour in contours:
        y, x = contour[:, 0], contour[:, 1]
        ymin, ymax = int(min(x)), int(max(x))
        xmin, xmax = int(min(y)), int(max(y))
        width = xmax - xmin
        height = ymax - ymin
        area = width * height
        aspect_ratio = width / height
        if area > 100 and aspect_ratio > 0.2:
            detected_letters.append((xmin, ymin, width, height))
    
    return detected_letters


image_path = 'Yosa.jpg'

#Membaca gambar awal
image = io.imread(image_path)

#Membuat figure dan axes untuk menampilkan gambar
fig, ax = plt.subplots(1, 2)

#Plot gambar awal
ax[0].imshow(image)
ax[0].axis('off')
ax[0].set_title('Gambar Awal')

#Deteksi huruf
detected_letters = detect_letters(image_path)

#Plot gambar dengan deteksi huruf
ax[1].imshow(image)
ax[1].axis('off')
ax[1].set_title('Deteksi Huruf')

#Menambahkan kotak pada huruf yang terdeteksi
for (x, y, width, height) in detected_letters:
    rect = patches.Rectangle((y, x), height, width, edgecolor='g', linewidth=2, fill=False)
    ax[1].add_patch(rect)

#Menampilkan plot

plt.show()

#### Penjelasan source code 

Pertama, kita mengimpor modul yang diperlukan untuk melakukan deteksi huruf dalam gambar. Modul yang diimpor adalah 'matplotlib.pyplot' untuk membuat plot gambar, 'skimage.io' untuk membaca gambar, 'skimage.color' untuk mengonversi gambar ke skala abu-abu, dan 'skimage.measure' untuk melakukan pengukuran pada gambar.

Selanjutnya, kita mendefinisikan sebuah fungsi bernama 'detect_letters' yang akan digunakan untuk mendeteksi huruf dalam gambar. Fungsi ini menerima parameter image_path yang merupakan path atau lokasi file gambar yang ingin dideteksi hurufnya.

Di dalam fungsi 'detect_letters', pertama-tama kita membaca gambar menggunakan 'io.imread' dari 'skimage'. Kemudian, gambar dikonversi ke skala abu-abu menggunakan 'color.rgb2gray' untuk mempermudah proses deteksi.

Selanjutnya, kita menentukan sebuah threshold dengan membandingkan nilai piksel pada gambar abu-abu dengan batas tertentu. Piksel yang bernilai lebih rendah dari batas akan dianggap sebagai huruf (foreground), sedangkan yang bernilai lebih tinggi akan dianggap sebagai latar belakang (background). Hasilnya disimpan dalam variabel 'threshold'.

Langkah selanjutnya adalah melakukan label pada gambar hasil threshold menggunakan measure.label. Proses ini akan memberikan label pada setiap region yang terhubung secara spasial dalam gambar. Hasilnya disimpan dalam variabel 'clean_image'.

Setelah gambar dilabeli, kita mencari kontur huruf menggunakan 'measure.find_contours'. Kontur adalah garis yang mengelilingi objek pada gambar. Nilai 'threshold 0.8' digunakan untuk menentukan batas ambang ketebalan kontur.

Selanjutnya, kita membuat sebuah list kosong bernama 'detected_letters' untuk menyimpan informasi lokasi dan ukuran huruf yang terdeteksi.

Melalui loop for, kita iterasi melalui setiap kontur yang ditemukan. Pada setiap iterasi, kita memperoleh koordinat x dan y dari kontur, dan kemudian menghitung batas minimal dan maksimal untuk x dan y. Dari batas tersebut, kita dapat menghitung lebar (width), tinggi (height), luas (area), dan rasio aspek (aspect_ratio) dari huruf.

Jika luas huruf lebih besar dari 100 piksel dan rasio aspek lebih besar dari 0.2, maka huruf tersebut dianggap valid dan ditambahkan ke dalam 'detected_letters' bersama dengan informasi lokasi dan ukuran huruf.

Fungsi 'detect_letters' mengembalikan detected_letters, yaitu list yang berisi informasi lokasi dan ukuran huruf yang terdeteksi.

Setelah mendefinisikan fungsi 'detect_letters', kita mendefinisikan path gambar yang akan diproses dalam variabel 'image_path'.

Gambar awal dibaca menggunakan 'io.imread' dan disimpan dalam variabel image.

Selanjutnya, kita membuat sebuah figure dan dua axes menggunakan plt.subplots(1, 2). Ini akan memberikan plot dengan dua gambar dalam satu baris.

Pada axes pertama (ax[0]), kita menampilkan gambar awal menggunakan imshow, menghilangkan sumbu dengan axis('off'), dan memberikan judul "Gambar Awal" dengan set_title.

Selanjutnya, kita memanggil fungsi detect_letters dengan menyediakan path gambar image_path. Hasil deteksi huruf disimpan dalam variabel detected_letters.

Pada axes kedua (ax[1]), kita menampilkan gambar asli menggunakan imshow dan memberikan judul "Deteksi Huruf".

Selanjutnya, dengan menggunakan loop for, kita iterasi melalui setiap huruf yang terdeteksi dalam detected_letters. Pada setiap iterasi, kita membuat sebuah kotak berdasarkan informasi lokasi dan ukuran huruf tersebut menggunakan patches.Rectangle, dan menambahkannya ke axes menggunakan ax[1].add_patch(rect).

Terakhir, kita menampilkan plot dengan memanggil plt.show().


### Web sebagai referensi
https://media.neliti.com/media/publications/323559-analisis-teknik-segmentasi-pada-pengolah-22d5da5e.pdf









