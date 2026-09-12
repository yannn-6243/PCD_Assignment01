**PCD_Assignment01**

**LAPORAN ANALISIS**
Tugas 1 Pengolahan Citra Digital (Down Sampling & Up Sampling)

Nama: Hadyan Althaf Baswara
NIM: 25/559377/PA/23527

**1. Pendahuluan**

Laporan ini membahas implementasi program down sampling dan up sampling pada citra digital yang dikembangkan secara manual, tanpa memanfaatkan fungsi resize bawaan pustaka (library). Proses down sampling diuji menggunakan tiga metode, yaitu Max Pooling, Average Pooling, dan Median Pooling. Adapun proses up sampling diuji menggunakan tiga metode lain, yaitu Nearest Neighbor (NN), Bilinear, dan Bicubic. Seluruh logika perhitungan inti disusun menggunakan struktur perulangan (for loop), sementara pustaka cv2 dan numpy hanya digunakan untuk membaca dan menulis berkas citra serta menyimpan data dalam bentuk array, bukan untuk melakukan proses perhitungan itu sendiri.

Pengujian dilakukan terhadap tiga citra dengan karakteristik yang berbeda-beda:

- **Sawah**: menampilkan pemandangan sawah dan gunung dengan tekstur alami yang cukup banyak, seperti daun padi dan pohon kelapa.
- **Sunset**: menampilkan matahari terbenam di pantai dengan gradasi warna yang halus pada langit, disertai satu titik yang sangat terang, yaitu matahari.
- **Pionir**: menggambarkan suasana Taman Pionir Gadjah Mada pada pagi hari, dengan cahaya matahari yang menembus sela-sela pepohonan (backlit) sehingga menghasilkan kontras yang tinggi.

Ketiga citra tersebut terlebih dahulu diubah ukurannya menjadi 400x300 piksel sebagai baseline, agar perbandingan pada tahap selanjutnya dapat dilakukan secara setara.

**2. Metode**

Proses down sampling dan up sampling pada penelitian ini dilaksanakan secara terpisah, bukan secara berurutan. Rinciannya sebagai berikut:

Down sampling dilakukan langsung dari citra baseline (400x300), diperkecil sebanyak empat kali menggunakan metode Max Pooling, Average Pooling, dan Median Pooling, sehingga menghasilkan citra berukuran 100x75.
Up sampling juga dilakukan langsung dari citra baseline yang sama (400x300), diperbesar sebanyak empat kali menggunakan metode NN, Bilinear, dan Bicubic, sehingga menghasilkan citra berukuran 1600x1200.

Kedua proses tersebut dilaksanakan secara independen dengan tujuan untuk menguji performa masing-masing metode, bukan untuk menguji efek citra yang diperkecil kemudian diperbesar kembali.

**3. Hasil Down Sampling**

Perbedaan antarmetode paling terlihat pada citra Pionir. Oleh karena citra ini mengandung banyak cahaya matahari yang menembus sela-sela daun, hasil Max Pooling menampilkan noise putih yang cukup mengganggu, terutama pada area pepohonan dan langit. Hal tersebut terjadi karena Max Pooling senantiasa mengambil nilai piksel paling terang pada setiap blok, sehingga titik cahaya kecil yang semula hanya muncul di sela-sela daun menjadi melebar dan tampak berlebihan. Sebaliknya, Average Pooling dan Median Pooling menghasilkan citra yang lebih bersih dan tidak menampilkan noise sebagaimana pada Max Pooling.

Pada citra Sawah, ketiga metode menghasilkan citra yang secara umum cukup serupa. Namun demikian, apabila diamati lebih saksama pada bagian dedaunan kelapa, hasil Max Pooling tampak sedikit lebih terang dan kurang halus dibandingkan dengan Average Pooling dan Median Pooling.

Pada citra Sunset, bentuk matahari pada hasil Max Pooling tampak sedikit melebar dan menyerupai bentuk kotak dibandingkan dengan hasil Average Pooling dan Median Pooling. Hal ini disebabkan oleh seluruh piksel terang di sekitar matahari yang turut terambil ketika nilai maksimal dihitung.

Berdasarkan uraian tersebut, dapat disimpulkan bahwa Max Pooling memiliki risiko paling tinggi apabila diterapkan pada citra dengan kontras cahaya tinggi, seperti pada citra Pionir. Sementara itu, Average Pooling dan Median Pooling terbukti lebih stabil untuk diterapkan pada berbagai jenis citra. Median Pooling merupakan metode yang paling seimbang, karena tetap menghasilkan citra yang halus tanpa merusak objek terang seekstrem Max Pooling.

**4. Hasil Up Sampling**

Oleh karena proses up sampling dilakukan langsung dari citra baseline dan bukan dari hasil down sampling, hasil yang diperoleh pada ketiga citra tampak jauh lebih halus dan hampir menyerupai citra aslinya apabila diamati secara sekilas. Hal ini menunjukkan bahwa kualitas hasil up sampling sangat bergantung pada resolusi citra sumber. Apabila citra sumber masih memiliki detail yang utuh, ketiga metode (NN, Bilinear, dan Bicubic) akan menghasilkan citra yang relatif serupa satu sama lain.

Meskipun demikian, apabila diamati lebih saksama pada bagian tepi-tepi kecil, seperti ranting pohon dan garis pagar, metode Nearest Neighbor tetap menghasilkan tepi yang paling kasar, Bilinear menghasilkan transisi yang lebih halus, dan Bicubic menghasilkan hasil paling halus di antara ketiganya. Meskipun demikian, oleh karena citra sumber yang digunakan beresolusi tinggi, perbedaan antarmetode tersebut tidak tampak semencolok apabila dibandingkan dengan up sampling yang dikerjakan dari citra yang telah diperkecil terlebih dahulu.

**5. Perbandingan Down Sampling dan Up Sampling**

Berdasarkan hasil pengujian yang dilakukan secara terpisah ini, dapat dinyatakan bahwa down sampling merupakan proses yang menghilangkan informasi secara permanen (irreversible), sedangkan up sampling hanya melakukan pendugaan atau interpolasi terhadap nilai piksel baru berdasarkan piksel yang telah ada. Apabila proses up sampling dilakukan terhadap citra yang resolusinya masih utuh, hasil yang diperoleh akan tetap halus dan detailnya tetap terjaga. Namun demikian, apabila up sampling dilakukan terhadap citra yang telah mengalami down sampling terlebih dahulu, hasilnya akan tetap tampak pecah atau buram, sekalipun menggunakan metode yang paling canggih seperti Bicubic, hal ini karena detail yang telah hilang tidak dapat dikembalikan kembali.

**6. Kesimpulan**

Max Pooling memiliki risiko paling tinggi apabila diterapkan pada citra dengan kontras cahaya tinggi, seperti citra Pionir yang bersifat backlit, karena dapat menimbulkan artefak berupa bercak putih. Adapun Average Pooling dan Median Pooling terbukti lebih stabil dan konsisten untuk diterapkan pada berbagai jenis citra.

Metode Nearest Neighbor, Bilinear, dan Bicubic sama-sama mampu memperbesar ukuran citra, tetapi kualitas hasilnya sangat bergantung pada resolusi citra sumber. Apabila citra sumber beresolusi tinggi, ketiga metode menghasilkan citra yang relatif serupa. Perbedaan yang jelas baru tampak apabila citra sumber beresolusi rendah. Di antara ketiganya, Bicubic menghasilkan citra paling halus, sedangkan Nearest Neighbor menghasilkan citra paling kasar.

Down sampling dan up sampling merupakan dua proses dengan tujuan yang berbeda. Down sampling bertujuan untuk mengurangi ukuran atau resolusi citra dan senantiasa disertai dengan kehilangan informasi, sedangkan up sampling bertujuan untuk memperbesar ukuran citra, tetapi tidak dapat menambahkan informasi baru yang benar-benar valid, melainkan hanya melakukan pendugaan atau interpolasi.

Karakteristik pencahayaan pada citra terbukti sangat berpengaruh terhadap hasil pengolahan, terutama pada proses down sampling. Citra dengan kontras ekstrem, seperti Pionir, menunjukkan perbedaan paling jelas antarmetode, sedangkan citra dengan tekstur yang lebih merata, seperti Sawah dan Sunset, menunjukkan perbedaan yang lebih halus.
