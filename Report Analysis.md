**PCD_Assignment01**

**LAPORAN ANALISIS**
Tugas 1 Pengolahan Citra Digital (Down Sampling & Up Sampling)

Nama: Hadyan Althaf Baswara
NIM: 25/559377/PA/23527

**1. Pendahuluan**

Tugas ini menjelaskan implementasi program down sampling dan up sampling citra digital secara manual, tanpa menggunakan fungsi resize bawaan library. Down sampling dikerjakan menggunakan 3 metode: Max Pooling, Average Pooling, dan Median Pooling. Up sampling menggunakan 3 metode: Nearest Neighbor (NN), Bilinear, dan Bicubic. Seluruh logika inti menggunakan perulangan (for loop), sedangkan library cv2 dan numpy hanya dipakai untuk membaca dan menulis file citra serta menyimpan data dalam bentuk array, bukan untuk proses perhitungannya.

Percobaan dilakukan pada 3 foto dengan karakteristik berbeda:

Sawah : pemandangan sawah dan gunung, banyak tekstur alami (daun padi, pohon kelapa).
Sunset : matahari terbenam di pantai, gradasi warna halus di langit plus 1 titik yang sangat terang (matahari).
Pionir : suasana taman Pionir Gadjah Mada pada pagi hari, cahaya matahari menembus sela-sela pohon (backlit), sehingga kontrasnya sangat tinggi.

Ketiga foto terlebih dahulu diresize ukurannya menjadi 400x300 piksel sebagai baseline, agar dapat dibandingkan secara adil pada tahap selanjutnya.

**2. Metode**

Down sampling dan up sampling pada percobaan ini dilakukan secara terpisah, bukan berurutan. Artinya:

Down sampling dilakukan langsung dari citra baseline (400x300), diperkecil 4 kali menggunakan Max/Average/Median Pooling, sehingga menghasilkan ukuran 100x75.
Up sampling juga dilakukan langsung dari citra baseline yang sama (400x300), diperbesar 4 kali menggunakan NN/Bilinear/Bicubic, sehingga menghasilkan ukuran 1600x1200.

Kedua proses ini murni digunakan untuk menguji performa masing-masing metode secara independen, bukan untuk menguji efek citra yang diperkecil kemudian diperbesar kembali.

**3. Hasil Down Sampling**

Pada foto Pionir, perbedaan antar metode terlihat paling jelas. Karena foto ini banyak mengandung cahaya matahari yang menembus sela-sela daun, hasil Max Pooling menjadi penuh noise putih yang cukup mengganggu di area pepohonan dan langit. Hal ini terjadi karena Max Pooling selalu mengambil nilai piksel paling terang di setiap blok, sehingga titik cahaya kecil yang sebelumnya hanya terselip di sela-sela daun justru melebar dan terlihat berlebihan. Average Pooling dan Median Pooling menghasilkan citra yang lebih bersih, tanpa noise seperti pada Max Pooling.

Pada foto Sawah, ketiga metode menghasilkan citra yang cukup mirip secara umum, tetapi jika diperhatikan lebih detail pada bagian dedaunan kelapa, Max Pooling membuat area tersebut terlihat sedikit lebih terang dan kurang halus dibandingkan Average dan Median Pooling.

Pada foto Sunset, bentuk matahari pada hasil Max Pooling terlihat sedikit melebar dan menyerupai kotak dibandingkan Average dan Median Pooling, karena seluruh piksel terang di sekitar matahari ikut terambil ketika diambil nilai maksimalnya.

Dapat disimpulkan bahwa Max Pooling paling berisiko digunakan pada foto dengan kontras cahaya tinggi (seperti Pionir), sedangkan Average dan Median Pooling lebih stabil digunakan pada berbagai jenis foto. Median Pooling menjadi pilihan paling seimbang karena tetap menghasilkan citra yang halus tanpa merusak objek terang seekstrem Max Pooling.

**4. Hasil Up Sampling**

Karena up sampling dikerjakan langsung dari citra baseline (bukan dari hasil down sampling), hasilnya pada ketiga foto jauh lebih halus dan hampir identik dengan citra aslinya secara sekilas. Hal ini menunjukkan bahwa kualitas hasil up sampling sangat bergantung pada resolusi citra sumbernya; apabila sumbernya masih memiliki detail yang utuh, ketiga metode (NN, Bilinear, Bicubic) akan menghasilkan citra yang terlihat mirip satu sama lain.

Jika diperhatikan lebih detail pada bagian tepi-tepi kecil (ranting pohon, garis pagar), Nearest Neighbor tetap menghasilkan tepi yang paling kasar, Bilinear menghasilkan transisi yang lebih halus, dan Bicubic paling halus di antara ketiganya. Namun, karena sumbernya menggunakan resolusi tinggi, perbedaan ketiga metode ini tidak semencolok apabila up sampling dikerjakan dari citra yang sudah diperkecil terlebih dahulu.

**5. Perbandingan Down Sampling vs Up Sampling**

Dari percobaan yang dipisahkan ini, terlihat jelas bahwa down sampling merupakan proses yang menghilangkan informasi secara permanen (irreversible), sedangkan up sampling hanya menebak atau menginterpolasi nilai piksel baru berdasarkan piksel yang sudah ada. Apabila up sampling dikerjakan dari citra yang resolusinya masih utuh, hasilnya akan halus dan detailnya tetap terjaga. Namun apabila up sampling dikerjakan dari citra yang sudah di-downsampling terlebih dahulu, hasilnya akan tetap terlihat pecah atau buram walaupun menggunakan metode paling canggih sekalipun (Bicubic), karena detail yang sudah hilang memang tidak dapat dikembalikan lagi.

**6. Kesimpulan**

Max Pooling paling berisiko digunakan pada foto dengan kontras cahaya tinggi (seperti foto Pionir yang backlit), karena dapat menimbulkan artefak berupa bercak putih. Average dan Median Pooling lebih stabil dan konsisten untuk berbagai jenis foto.

Nearest Neighbor, Bilinear, dan Bicubic sama-sama dapat memperbesar citra, tetapi kualitas hasilnya sangat bergantung pada resolusi citra sumbernya. Apabila sumbernya beresolusi tinggi, ketiga metode menghasilkan citra yang sudah cukup mirip. Apabila sumbernya beresolusi rendah, barulah terlihat perbedaan yang jelas. Bicubic paling halus, NN paling kasar.

Down sampling dan up sampling merupakan dua proses dengan tujuan yang berbeda: down sampling bertujuan mengurangi ukuran/resolusi citra (dan pasti kehilangan informasi), sedangkan up sampling bertujuan memperbesar ukuran citra (tetapi tidak dapat menambahkan informasi baru yang benar-benar valid, hanya menebak atau menginterpolasi saja).

Karakteristik pencahayaan pada foto sangat berpengaruh terhadap hasil pengolahan, terutama untuk down sampling. Foto dengan kontras ekstrem (Pionir) menunjukkan perbedaan paling jelas antar metode, sedangkan foto dengan tekstur lebih merata (Sawah, Sunset) perbedaannya lebih halus.
