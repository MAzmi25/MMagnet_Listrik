<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Muatan Listrik?", "id": "Sifat Dasar Partikel Subatomik." },
  { "en": "Apa Satuan Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Itu Muatan Positif?", "id": "Muatan Yang Dimiliki Oleh Proton." },
  { "en": "Apa Itu Muatan Negatif?", "id": "Muatan Yang Dimiliki Oleh Elektron." },
  { "en": "Apa Itu Muatan Elementer?", "id": "Besar Muatan Satu Elektron Atau Proton." },
  { "en": "Apa Itu Prinsip Kekekalan Muatan?", "id": "Muatan Total Dalam Sistem Terisolasi Tetap." },
  { "en": "Apa Itu Kuantisasi Muatan?", "id": "Muatan Selalu Kelipatan Dari Muatan Elementer." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Dimana Muatan Dapat Bergerak Bebas." },
  { "en": "Apa Itu Isolator (Dielektrik)?", "id": "Bahan Dimana Muatan Tidak Dapat Bergerak Bebas." },
  { "en": "Apa Itu Semikonduktor?", "id": "Sifatnya Di Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Gaya Listrik?", "id": "Gaya Interaksi Antara Benda Bermuatan." },
  { "en": "Apa Itu Hukum Coulomb?", "id": "Menghitung Gaya Antara Dua Muatan Titik." },
  { "en": "Bagaimana Arah Gaya Coulomb?", "id": "Tolak-Menolak Untuk Muatan Sejenis, Tarik-Menarik Berlawanan." },
  { "en": "Apa Itu Medan Listrik?", "id": "Daerah Di Sekitar Muatan Yang Mempengaruhi Muatan Lain." },
  { "en": "Apa Satuan Medan Listrik?", "id": "Newton Per Coulomb (N/C)." },
  { "en": "Bagaimana Medan Listrik Direpresentasikan?", "id": "Dengan Garis-Garis Gaya Listrik." },
  { "en": "Bagaimana Arah Garis Gaya Listrik?", "id": "Keluar Dari Muatan Positif, Masuk Ke Negatif." },
  { "en": "Apa Arti Kerapatan Garis Gaya?", "id": "Menunjukkan Kekuatan Medan Listrik." },
  { "en": "Medan Listrik Dari Muatan Titik?", "id": "Menyebar Radial Ke Segala Arah." },
  { "en": "Apa Itu Muatan Uji?", "id": "Muatan Positif Kecil Untuk Mendeteksi Medan." },
  { "en": "Apa Itu Prinsip Superposisi (Medan Listrik)?", "id": "Medan Listrik Total Adalah Jumlah Vektor." },
  { "en": "Apa Itu Dipol Listrik?", "id": "Sepasang Muatan Sama Besar Berlawanan Tanda." },
  { "en": "Apa Itu Momen Dipol Listrik?", "id": "Ukuran Kekuatan Dan Orientasi Dipol." },
  { "en": "Medan Listrik Seragam?", "id": "Medan Dengan Vektor Yang Sama Di Setiap Titik." },
  { "en": "Bagaimana Membuat Medan Listrik Seragam?", "id": "Di Antara Dua Pelat Konduktor Sejajar." },
  { "en": "Perilaku Konduktor Dalam Medan Listrik?", "id": "Medan Listrik Di Dalamnya Nol." },
  { "en": "Distribusi Muatan Pada Konduktor?", "id": "Muatan Berada Di Permukaan Konduktor." },
  { "en": "Arah Medan Listrik Pada Permukaan Konduktor?", "id": "Selalu Tegak Lurus Permukaan." },
  { "en": "Apa Itu Induksi Elektrostatis?", "id": "Pemisahan Muatan Pada Konduktor Akibat Medan." },
  { "en": "Apa Itu Sangkar Faraday?", "id": "Ruang Tertutup Konduktif Yang Melindungi Dari Medan." },
  { "en": "Apa Itu Fluks Listrik?", "id": "Ukuran Jumlah Garis Medan Menembus Permukaan." },
  { "en": "Apa Satuan Fluks Listrik?", "id": "Newton Meter Kuadrat Per Coulomb." },
  { "en": "Apa Itu Hukum Gauss?", "id": "Menghubungkan Fluks Listrik Dengan Muatan Total." },
  { "en": "Apa Itu Permukaan Gaussian?", "id": "Permukaan Tertutup Imajiner Untuk Hukum Gauss." },
  { "en": "Aplikasi Hukum Gauss?", "id": "Menghitung Medan Listrik Dari Distribusi Simetris." },
  { "en": "Medan Listrik Kawat Lurus Panjang?", "id": "Berkurang Sebanding Dengan Jarak." },
  { "en": "Medan Listrik Pelat Luas Tak Terhingga?", "id": "Seragam Dan Konstan." },
  { "en": "Medan Listrik Kulit Bola Konduktor?", "id": "Nol Di Dalam, Seperti Muatan Titik Di Luar." },
  { "en": "Apa Itu Energi Potensial Listrik?", "id": "Energi Yang Dimiliki Muatan Akibat Posisinya." },
  { "en": "Apa Itu Potensial Listrik?", "id": "Energi Potensial Listrik Per Satuan Muatan." },
  { "en": "Nama Lain Potensial Listrik?", "id": "Tegangan (Voltage)." },
  { "en": "Apa Satuan Potensial Listrik?", "id": "Volt (V)." },
  { "en": "Potensial Adalah Besaran Skalar Atau Vektor?", "id": "Besaran Skalar." },
  { "en": "Apa Itu Beda Potensial?", "id": "Perbedaan Potensial Listrik Antara Dua Titik." },
  { "en": "Apa Itu Permukaan Ekuipotensial?", "id": "Permukaan Dengan Nilai Potensial Yang Sama." },
  { "en": "Hubungan Ekuipotensial Dan Medan Listrik?", "id": "Selalu Saling Tegak Lurus." },
  { "en": "Apakah Perlu Kerja Memindahkan Muatan?", "id": "Tidak, Jika Di Permukaan Ekuipotensial." },
  { "en": "Hubungan Medan Dan Potensial?", "id": "Medan Listrik Adalah Gradien Negatif Potensial." },
  { "en": "Potensial Dari Muatan Titik?", "id": "Berkurang Sebanding Dengan Jarak." },
  { "en": "Potensial Dari Banyak Muatan?", "id": "Jumlah Aljabar Dari Potensial Masing-Masing." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Untuk Menyimpan Energi Listrik." },
  { "en": "Apa Itu Kapasitansi?", "id": "Ukuran Kemampuan Kapasitor Menyimpan Muatan." },
  { "en": "Apa Satuan Kapasitansi?", "id": "Farad (F)." },
  { "en": "Kapasitansi Pelat Sejajar?", "id": "Tergantung Pada Luas Dan Jarak Antar Pelat." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Yang Disisipkan Dalam Kapasitor." },
  { "en": "Fungsi Bahan Dielektrik?", "id": "Meningkatkan Kapasitansi Dan Kekuatan Tegangan." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Faktor Peningkatan Kapasitansi Oleh Dielektrik." },
  { "en": "Apa Itu Polarisasi Dielektrik?", "id": "Pergeseran Muatan Dalam Bahan Dielektrik." },
  { "en": "Energi Yang Tersimpan Dalam Kapasitor?", "id": "Tersimpan Dalam Medan Listrik Antar Pelat." },
  { "en": "Kapasitor Seri?", "id": "Total Kapasitansi Lebih Kecil." },
  { "en": "Kapasitor Paralel?", "id": "Total Kapasitansi Adalah Jumlahnya." },
  { "en": "Apa Itu Arus Listrik?", "id": "Aliran Terarah Dari Pembawa Muatan." },
  { "en": "Apa Satuan Arus Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Itu Arah Arus Konvensional?", "id": "Arah Aliran Muatan Positif." },
  { "en": "Apa Itu Kecepatan Drift?", "id": "Kecepatan Rata-rata Pembawa Muatan." },
  { "en": "Apa Itu Rapat Arus?", "id": "Arus Listrik Per Satuan Luas Penampang." },
  { "en": "Apa Itu Resistansi?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Apa Satuan Resistansi?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Menyatakan Hubungan Linear Antara Tegangan Arus." },
  { "en": "Apa Itu Resistivitas?", "id": "Sifat Intrinsik Bahan Menghambat Arus." },
  { "en": "Apa Itu Konduktivitas?", "id": "Kebalikan Dari Resistivitas." },
  { "en": "Efek Suhu Pada Resistivitas?", "id": "Umumnya Meningkat Pada Konduktor." },
  { "en": "Apa Itu Superkonduktor?", "id": "Bahan Dengan Resistansi Listrik Nol." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Energi Yang Dihamburkan Atau Disediakan." },
  { "en": "Apa Itu Efek Joule?", "id": "Pemanasan Konduktor Akibat Aliran Arus." },
  { "en": "Apa Itu Gaya Gerak Listrik (GGL)?", "id": "Kerja Per Muatan Oleh Sumber Energi." },
  { "en": "Sumber GGL?", "id": "Baterai, Generator." },
  { "en": "Resistansi Internal Sumber?", "id": "Hambatan Di Dalam Sumber Tegangan." },
  { "en": "Apa Itu Potensiometer?", "id": "Alat Untuk Mengukur Beda Potensial." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian Untuk Mengukur Resistansi Tidak Dikenal." },
  { "en": "Apa Itu Rangkaian RC?", "id": "Rangkaian Dengan Resistor Dan Kapasitor." },
  { "en": "Proses Pengisian Kapasitor?", "id": "Muatan Dan Tegangan Meningkat Secara Eksponensial." },
  { "en": "Proses Pengosongan Kapasitor?", "id": "Muatan Dan Tegangan Menurun Secara Eksponensial." },
  { "en": "Apa Itu Konstanta Waktu RC?", "id": "Ukuran Seberapa Cepat Rangkaian RC Merespons." },
  { "en": "Apa Itu Petir?", "id": "Pelepasan Muatan Listrik Skala Besar." },
  { "en": "Apa Itu Grounding (Pentanahan)?", "id": "Menyediakan Jalur Aman Untuk Muatan Berlebih." },
  { "en": "Apa Itu Elektroskop?", "id": "Alat Sederhana Untuk Mendeteksi Muatan." },
  { "en": "Bagaimana Elektroskop Bekerja?", "id": "Berdasarkan Prinsip Tolak-Menolak Muatan Sejenis." },
  { "en": "Apa Itu Generator Van De Graaff?", "id": "Mesin Untuk Menghasilkan Potensial Listrik Tinggi." },
  { "en": "Apa Itu Efek Tribolistrik?", "id": "Pembangkitan Muatan Akibat Gesekan." },
  { "en": "Apa Itu Pelindung Elektrostatik?", "id": "Nama Lain Untuk Sangkar Faraday." },
  { "en": "Apa Itu Breakdown Dielektrik?", "id": "Isolator Menjadi Konduktor Pada Medan Sangat Kuat." },
  { "en": "Kekuatan Dielektrik?", "id": "Medan Listrik Maksimum Yang Dapat Ditahan." },
  { "en": "Apa Itu Medan Magnet?", "id": "Medan Yang Dihasilkan Oleh Muatan Bergerak." },
  { "en": "Sumber Medan Magnet?", "id": "Arus Listrik Dan Magnet Permanen." },
  { "en": "Apa Itu Magnet Permanen?", "id": "Bahan Yang Menghasilkan Medan Magnet Sendiri." },
  { "en": "Apa Itu Kutub Magnet?", "id": "Area Dimana Medan Magnet Paling Kuat." },
  { "en": "Kutub Magnet Apa Saja?", "id": "Kutub Utara Dan Kutub Selatan." },
  { "en": "Interaksi Antar Kutub?", "id": "Sejenis Tolak-Menolak, Berlawanan Tarik-Menarik." },
  { "en": "Apakah Monopol Magnet Ada?", "id": "Tidak, Kutub Selalu Datang Berpasangan." },
  { "en": "Medan Magnet Bumi?", "id": "Melindungi Bumi Dari Angin Matahari." },
  { "en": "Kutub Utara Geografis Dekat Dengan?", "id": "Kutub Selatan Magnetik." },
  { "en": "Bagaimana Medan Magnet Direpresentasikan?", "id": "Dengan Garis-Garis Gaya Magnetik." },
  { "en": "Arah Garis Gaya Magnetik?", "id": "Keluar Dari Kutub Utara, Masuk Ke Selatan." },
  { "en": "Apa Arti Kerapatan Garis Gaya?", "id": "Menunjukkan Kekuatan Medan Magnet." },
  { "en": "Apakah Garis Gaya Magnet Berpotongan?", "id": "Tidak, Garis Gaya Tidak Pernah Berpotongan." },
  { "en": "Bentuk Garis Gaya Magnet?", "id": "Selalu Membentuk Loop Tertutup." },
  { "en": "Simbol Untuk Medan Magnet?", "id": "B." },
  { "en": "Apa Satuan Medan Magnet?", "id": "Tesla (T)." },
  { "en": "Satuan Medan Magnet Lainnya?", "id": "Gauss (G)." },
  { "en": "Apa Itu Gaya Magnetik?", "id": "Gaya Yang Dialami Muatan Bergerak." },
  { "en": "Apa Itu Gaya Lorentz?", "id": "Gaya Total Pada Muatan (Listrik Magnetik)." },
  { "en": "Arah Gaya Magnetik Pada Muatan?", "id": "Tegak Lurus Terhadap Kecepatan Dan Medan." },
  { "en": "Bagaimana Menentukan Arah Gaya Magnetik?", "id": "Menggunakan Aturan Tangan Kanan." },
  { "en": "Kapan Gaya Magnetik Bernilai Nol?", "id": "Saat Muatan Diam Atau Bergerak Sejajar Medan." },
  { "en": "Lintasan Muatan Dalam Medan Seragam?", "id": "Berbentuk Lingkaran Atau Heliks." },
  { "en": "Frekuensi Siklotron?", "id": "Frekuensi Putaran Muatan Dalam Medan Magnet." },
  { "en": "Apakah Medan Magnet Melakukan Kerja?", "id": "Tidak, Karena Gayanya Selalu Tegak Lurus." },
  { "en": "Aplikasi Gerak Muatan Dalam Medan?", "id": "Spektrometer Massa, Akselerator Partikel." },
  { "en": "Apa Itu Spektrometer Massa?", "id": "Memisahkan Ion Berdasarkan Rasio Massa-Muatan." },
  { "en": "Apa Itu Efek Hall?", "id": "Timbulnya Tegangan Melintang Pada Konduktor." },
  { "en": "Aplikasi Efek Hall?", "id": "Sensor Medan Magnet, Menentukan Jenis Pembawa Muatan." },
  { "en": "Gaya Magnetik Pada Kawat Berarus?", "id": "Hasil Dari Gaya Pada Masing-Masing Muatan." },
  { "en": "Arah Gaya Pada Kawat?", "id": "Ditentukan Juga Dengan Aturan Tangan Kanan." },
  { "en": "Gaya Antara Dua Kawat Sejajar?", "id": "Tarik-Menarik Jika Arus Searah." },
  { "en": "Kapan Dua Kawat Saling Menolak?", "id": "Saat Arus Yang Mengalir Berlawanan Arah." },
  { "en": "Definisi Satuan Ampere?", "id": "Didasarkan Pada Gaya Antara Dua Kawat." },
  { "en": "Apa Itu Torsi?", "id": "Gaya Putar." },
  { "en": "Torsi Pada Loop Arus?", "id": "Loop Berarus Cenderung Sejajar Dengan Medan." },
  { "en": "Prinsip Kerja Motor Listrik DC?", "id": "Torsi Pada Loop Arus Dalam Medan Magnet." },
  { "en": "Apa Itu Momen Dipol Magnetik?", "id": "Ukuran Kekuatan Momen Putar Magnetik." },
  { "en": "Apa Itu Hukum Biot-Savart?", "id": "Menghitung Medan Magnet Dari Elemen Arus." },
  { "en": "Medan Magnet Kawat Lurus Panjang?", "id": "Membentuk Lingkaran Konsentris Di Sekitar Kawat." },
  { "en": "Arah Medan Sekitar Kawat?", "id": "Ditentukan Dengan Aturan Genggaman Tangan Kanan." },
  { "en": "Medan Magnet Di Pusat Loop Melingkar?", "id": "Paling Kuat Dan Tegak Lurus Bidang." },
  { "en": "Apa Itu Solenoida?", "id": "Kumparan Kawat Panjang Yang Rapat." },
  { "en": "Medan Magnet Di Dalam Solenoida Ideal?", "id": "Seragam Dan Sejajar Sumbu." },
  { "en": "Apa Itu Toroida?", "id": "Solenoida Yang Dibengkokkan Membentuk Donat." },
  { "en": "Medan Magnet Di Luar Toroida Ideal?", "id": "Nol." },
  { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Integral Medan Magnet Dengan Arus." },
  { "en": "Kapan Hukum Ampere Berguna?", "id": "Untuk Situasi Dengan Simetri Tinggi." },
  { "en": "Apa Itu Lintasan Amperian?", "id": "Loop Tertutup Imajiner Untuk Hukum Ampere." },
  { "en": "Apa Itu Fluks Magnetik?", "id": "Jumlah Garis Gaya Magnet Menembus Permukaan." },
  { "en": "Apa Satuan Fluks Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Hukum Gauss Untuk Magnetisme?", "id": "Fluks Magnetik Total Melalui Permukaan Tertutup Nol." },
  { "en": "Arti Hukum Gauss Untuk Magnetisme?", "id": "Tidak Ada Sumber Monopol Magnetik." },
  { "en": "Apa Itu Bahan Magnetik?", "id": "Bahan Yang Dapat Dipengaruhi Medan Magnet." },
  { "en": "Apa Itu Magnetisasi?", "id": "Kerapatan Momen Dipol Magnetik Dalam Bahan." },
  { "en": "Apa Itu Bahan Feromagnetik?", "id": "Bahan Yang Sangat Kuat Ditarik Magnet." },
  { "en": "Contoh Bahan Feromagnetik?", "id": "Besi, Nikel, Kobalt." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Daerah Kecil Dengan Momen Magnet Sejajar." },
  { "en": "Apa Itu Temperatur Curie?", "id": "Suhu Dimana Sifat Feromagnetik Hilang." },
  { "en": "Apa Itu Bahan Paramagnetik?", "id": "Bahan Yang Sedikit Ditarik Medan Magnet." },
  { "en": "Apa Itu Bahan Diamagnetik?", "id": "Bahan Yang Sedikit Ditolak Medan Magnet." },
  { "en": "Contoh Bahan Diamagnetik?", "id": "Air, Tembaga, Emas." },
  { "en": "Apa Itu Histeresis Magnetik?", "id": "Ketergantungan Magnetisasi Pada Riwayat Medan." },
  { "en": "Apa Itu Retentivitas?", "id": "Sisa Magnetisasi Setelah Medan Dihilangkan." },
  { "en": "Apa Itu Koersivitas?", "id": "Medan Balik Untuk Menghilangkan Sisa Magnetisasi." },
  { "en": "Bahan Magnet Keras?", "id": "Memiliki Retentivitas Dan Koersivitas Tinggi." },
  { "en": "Bahan Magnet Lunak?", "id": "Mudah Dimagnetisasi Dan Didemagnetisasi." },
  { "en": "Aplikasi Magnet Keras?", "id": "Magnet Permanen, Speaker." },
  { "en": "Aplikasi Magnet Lunak?", "id": "Inti Transformator, Elektromagnet." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dibuat Dengan Melilitkan Kawat." },
  { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Ukuran Kemampuan Bahan Mendukung Medan Magnet." },
  { "en": "Apa Itu Medan Magnetisasi (H)?", "id": "Medan Magnet Akibat Sumber Eksternal." },
  { "en": "Hubungan Antara B Dan H?", "id": "B = μ * H." },
  { "en": "Apa Itu Rangkaian Magnetik?", "id": "Analogi Rangkaian Listrik Untuk Medan Magnet." },
  { "en": "Apa Itu Gaya Gerak Magnet (GGM)?", "id": "Analogi GGL Listrik, Dihasilkan Oleh Arus." },
  { "en": "Apa Itu Reluktansi?", "id": "Analogi Resistansi, Hambatan Terhadap Fluks." },
  { "en": "Hukum Hopkinson?", "id": "Analogi Hukum Ohm Untuk Rangkaian Magnetik." },
  { "en": "Apa Itu Induksi Elektromagnetik?", "id": "Pembangkitan Arus Akibat Perubahan Fluks Magnetik." },
  { "en": "Siapa Penemu Induksi Elektromagnetik?", "id": "Michael Faraday." },
  { "en": "Apa Itu Hukum Faraday Tentang Induksi?", "id": "GGL Induksi Sebanding Dengan Laju Perubahan Fluks." },
  { "en": "Apa Itu Hukum Lenz?", "id": "Arah Arus Induksi Melawan Perubahan Penyebabnya." },
  { "en": "Hukum Lenz Adalah Konsekuensi Dari?", "id": "Hukum Kekekalan Energi." },
  { "en": "Apa Itu GGL Gerak (Motional EMF)?", "id": "GGL Yang Diinduksikan Pada Konduktor Bergerak." },
  { "en": "Prinsip Kerja Generator Listrik?", "id": "Berdasarkan GGL Gerak Atau Hukum Faraday." },
  { "en": "Apa Itu Arus Eddy (Arus Pusar)?", "id": "Arus Induksi Yang Berputar Dalam Konduktor." },
  { "en": "Kerugian Arus Eddy?", "id": "Menyebabkan Pemanasan Dan Rugi-Rugi Energi." },
  { "en": "Bagaimana Mengurangi Arus Eddy?", "id": "Menggunakan Inti Berlapis-lapis (Laminasi)." },
  { "en": "Apa Itu Pengereman Magnetik?", "id": "Menggunakan Arus Eddy Untuk Menghasilkan Gaya Rem." },
  { "en": "Apa Itu Medan Listrik Terinduksi?", "id": "Medan Listrik Non-Konservatif Akibat Perubahan Medan Magnet." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Rangkaian Yang Menyimpan Energi." },
  { "en": "Energi Dalam Induktor Tersimpan Dimana?", "id": "Dalam Medan Magnetnya." },
  { "en": "Apa Itu Induktansi?", "id": "Ukuran Kemampuan Menghasilkan GGL Induksi Sendiri." },
  { "en": "Apa Satuan Induktansi?", "id": "Henry (H)." },
  { "en": "Induktansi Solenoida?", "id": "Tergantung Pada Jumlah Lilitan Dan Geometrinya." },
  { "en": "Apa Itu Rangkaian RL?", "id": "Rangkaian Dengan Resistor Dan Induktor." },
  { "en": "Apa Itu Konstanta Waktu RL?", "id": "Ukuran Seberapa Cepat Arus Berubah." },
  { "en": "Apa Itu Energi Yang Tersimpan Induktor?", "id": "Sebanding Dengan Kuadrat Arus." },
  { "en": "Apa Itu Kepadatan Energi Magnetik?", "id": "Energi Magnetik Per Satuan Volume." },
  { "en": "Apa Itu Induktansi Timbal Balik (Mutual)?", "id": "Fluks Satu Kumparan Menginduksi GGL." },
  { "en": "Apa Itu Induktansi Diri (Self-Inductance)?", "id": "GGL Diinduksikan Dalam Kumparan Sendiri." },
  { "en": "Apa Itu Induktansi Timbal Balik (Mutual Inductance)?", "id": "GGL Diinduksi Di Kumparan Tetangga." },
  { "en": "Faktor Yang Mempengaruhi Mutual Inductance?", "id": "Geometri Dan Jarak Antar Kumparan." },
  { "en": "Apa Itu Koefisien Kopling?", "id": "Ukuran Seberapa Erat Dua Kumparan Terkopling." },
  { "en": "Apa Prinsip Kerja Transformator?", "id": "Berdasarkan Induktansi Timbal Balik." },
  { "en": "Apa Itu Rangkaian LC?", "id": "Rangkaian Dengan Induktor Dan Kapasitor." },
  { "en": "Apa Perilaku Rangkaian LC Ideal?", "id": "Energi Berosilasi Antara Induktor Kapasitor." },
  { "en": "Frekuensi Osilasi Natural LC?", "id": "Ditentukan Oleh Nilai L Dan C." },
  { "en": "Apa Itu Rangkaian RLC Seri?", "id": "Rangkaian Dengan Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Redaman (Damping)?", "id": "Hilangnya Energi Osilasi Akibat Resistansi." },
  { "en": "Apa Itu Osilasi Teredam?", "id": "Osilasi Dengan Amplitudo Yang Terus Berkurang." },
  { "en": "Kondisi Underdamped (RLC)?", "id": "Sistem Berosilasi Sebelum Kembali Ke Keseimbangan." },
  { "en": "Kondisi Critically Damped (RLC)?", "id": "Kembali Ke Keseimbangan Paling Cepat." },
  { "en": "Kondisi Overdamped (RLC)?", "id": "Kembali Ke Keseimbangan Secara Lambat." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Arus Yang Besarnya Dan Arahnya Berubah." },
  { "en": "Apa Itu Sumber AC?", "id": "Generator AC Yang Menghasilkan Tegangan Sinusoidal." },
  { "en": "Apa Itu Fasor?", "id": "Vektor Berputar Untuk Merepresentasikan Besaran AC." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Diagram Yang Menunjukkan Hubungan Fasa Besaran AC." },
  { "en": "Resistor Dalam Rangkaian AC?", "id": "Arus Dan Tegangan Sefasa." },
  { "en": "Kapasitor Dalam Rangkaian AC?", "id": "Arus Mendahului Tegangan Sebesar 90 Derajat." },
  { "en": "Induktor Dalam Rangkaian AC?", "id": "Tegangan Mendahului Arus Sebesar 90 Derajat." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Hambatan Kapasitor Terhadap Arus AC." },
  { "en": "Bagaimana Reaktansi Kapasitif Berubah?", "id": "Berkurang Dengan Kenaikan Frekuensi." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Hambatan Induktor Terhadap Arus AC." },
  { "en": "Bagaimana Reaktansi Induktif Berubah?", "id": "Meningkat Dengan Kenaikan Frekuensi." },
  { "en": "Apa Itu Impedansi?", "id": "Total Hambatan Rangkaian AC (Termasuk Fasa)." },
  { "en": "Hukum Ohm Untuk Rangkaian AC?", "id": "V = I * Z." },
  { "en": "Apa Itu Sudut Fasa?", "id": "Perbedaan Fasa Antara Tegangan Dan Arus Total." },
  { "en": "Apa Itu Resonansi Dalam Rangkaian RLC?", "id": "Saat Reaktansi Induktif Sama Dengan Kapasitif." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Dimana Terjadi Resonansi." },
  { "en": "Impedansi Pada Saat Resonansi Seri?", "id": "Minimum, Sama Dengan Resistansi." },
  { "en": "Arus Pada Saat Resonansi Seri?", "id": "Maksimum." },
  { "en": "Apa Itu Faktor Kualitas (Q)?", "id": "Ukuran 'Ketajaman' Kurva Resonansi." },
  { "en": "Apa Itu Daya Pada Rangkaian AC?", "id": "Daya Sesaat Berfluktuasi." },
  { "en": "Apa Itu Daya Rata-Rata?", "id": "Daya Aktual Yang Digunakan Oleh Rangkaian." },
  { "en": "Komponen Apa Yang Mendisipasikan Daya?", "id": "Hanya Resistor." },
  { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Rata-rata Terhadap Daya Semu." },
  { "en": "Nilai Maksimum Faktor Daya?", "id": "Satu, Pada Saat Resonansi." },
  { "en": "Apa Itu Transformator?", "id": "Alat Untuk Mengubah Tegangan AC." },
  { "en": "Apa Itu Transformator Step-Up?", "id": "Menaikkan Tegangan, Menurunkan Arus." },
  { "en": "Apa Itu Transformator Step-Down?", "id": "Menurunkan Tegangan, Menaikkan Arus." },
  { "en": "Rasio Tegangan Transformator Ideal?", "id": "Sama Dengan Rasio Jumlah Lilitan." },
  { "en": "Rasio Arus Transformator Ideal?", "id": "Kebalikan Dari Rasio Jumlah Lilitan." },
  { "en": "Mengapa Listrik Ditransmisikan Tegangan Tinggi?", "id": "Untuk Meminimalkan Rugi-Rugi Daya I²R." },
  { "en": "Apa Itu Persamaan Maxwell?", "id": "Empat Persamaan Fundamental Elektromagnetisme." },
  { "en": "Hukum Gauss (Maxwell)?", "id": "Sumber Medan Listrik Adalah Muatan." },
  { "en": "Hukum Gauss Untuk Magnetisme (Maxwell)?", "id": "Tidak Ada Monopol Magnetik." },
  { "en": "Hukum Faraday (Maxwell)?", "id": "Perubahan Medan Magnet Menghasilkan Medan Listrik." },
  { "en": "Hukum Ampere-Maxwell?", "id": "Arus Dan Perubahan Medan Listrik Hasilkan Magnet." },
  { "en": "Apa Kontribusi Maxwell?", "id": "Menambahkan Suku Arus Pergeseran." },
  { "en": "Apa Itu Arus Pergeseran?", "id": "Arus Fiktif Akibat Perubahan Medan Listrik." },
  { "en": "Prediksi Utama Persamaan Maxwell?", "id": "Adanya Gelombang Elektromagnetik." },
  { "en": "Apa Itu Gelombang Elektromagnetik (EM)?", "id": "Getaran Medan Listrik Dan Magnet." },
  { "en": "Bagaimana Gelombang EM Dihasilkan?", "id": "Oleh Muatan Yang Dipercepat." },
  { "en": "Sifat Gelombang EM?", "id": "Merambat Tanpa Medium, Kecepatan Cahaya." },
  { "en": "Arah Medan E Dan B?", "id": "Saling Tegak Lurus Dan Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Kecepatan Cahaya (c)?", "id": "Kecepatan Gelombang EM Di Ruang Hampa." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Jenis Gelombang EM." },
  { "en": "Urutan Spektrum EM (Frekuensi Naik)?", "id": "Radio, Mikro, Inframerah, Tampak, UV, X, Gamma." },
  { "en": "Apa Itu Vektor Poynting?", "id": "Menunjukkan Arah Dan Laju Aliran Energi EM." },
  { "en": "Energi Dalam Gelombang EM?", "id": "Dibagi Rata Antara Medan Listrik Magnet." },
  { "en": "Apa Itu Momentum Gelombang EM?", "id": "Gelombang EM Membawa Momentum." },
  { "en": "Apa Itu Tekanan Radiasi?", "id": "Tekanan Yang Diberikan Oleh Gelombang EM." },
  { "en": "Apa Itu Polarisasi Gelombang EM?", "id": "Arah Osilasi Dari Vektor Medan Listrik." },
  { "en": "Apa Itu Polarisasi Linear?", "id": "Medan Listrik Berosilasi Dalam Satu Bidang." },
  { "en": "Apa Itu Cahaya Tak Terpolarisasi?", "id": "Arah Polarisasi Acak Dan Berubah Cepat." },
  { "en": "Bagaimana Mempolarisasi Cahaya?", "id": "Menggunakan Filter Polarisasi (Polaroid)." },
  { "en": "Polarisasi Dengan Pemantulan?", "id": "Cahaya Pantul Terpolarisasi Sebagian." },
  { "en": "Apa Itu Sudut Brewster?", "id": "Sudut Pantul Dimana Cahaya Terpolarisasi Sempurna." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Yang Memandu Perambatan Gelombang EM." },
  { "en": "Contoh Pemandu Gelombang?", "id": "Kabel Koaksial, Serat Optik." },
  { "en": "Apa Itu Antena?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang EM." },
  { "en": "Antena Dipol?", "id": "Jenis Antena Paling Sederhana." },
  { "en": "Pola Radiasi Antena?", "id": "Pola Distribusi Energi Yang Dipancarkan." },
  { "en": "Penerimaan Gelombang EM?", "id": "Gelombang EM Menginduksi Arus Di Antena." },
  { "en": "Hubungan E Dan B Dalam Gelombang?", "id": "E = c * B." },
  { "en": "Apa Itu Efek Doppler (Gelombang EM)?", "id": "Perubahan Frekuensi Akibat Gerak Relatif." },
  { "en": "Apa Itu Redshift?", "id": "Pergeseran Ke Frekuensi Lebih Rendah." },
  { "en": "Apa Itu Blueshift?", "id": "Pergeseran Ke Frekuensi Lebih Tinggi." },
  { "en": "Aplikasi Efek Doppler?", "id": "Radar Kecepatan, Astronomi." },
  { "en": "Apa Itu Relativitas Khusus?", "id": "Teori Yang Mengubah Pemahaman Elektromagnetisme." },
  { "en": "Apa Itu Elektrodinamika Kuantum (QED)?", "id": "Teori Kuantum Tentang Cahaya Dan Materi." },
  { "en": "Apa Itu Foton?", "id": "Kuantum Atau Partikel Dari Medan Elektromagnetik." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dibuat Oleh Arus Listrik." },
  { "en": "Intensitas Gelombang EM?", "id": "Daya Rata-rata Per Satuan Luas." },
  { "en": "Apa Itu Pemantulan?", "id": "Gelombang Memantul Dari Permukaan." },
  { "en": "Apa Itu Pembiasan?", "id": "Gelombang Berbelok Saat Melewati Medium." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Gelombang Saat Melewati Celah." },
  { "en": "Apa Itu Interferensi?", "id": "Superposisi Dua Atau Lebih Gelombang." },
  { "en": "Interferensi Konstruktif?", "id": "Gelombang Saling Menguatkan." },
  { "en": "Interferensi Destruktif?", "id": "Gelombang Saling Melemahkan." },
  { "en": "Apa Itu Resonator Rongga (Cavity)?", "id": "Ruang Logam Yang Dapat Beresonansi Gelombang EM." },
  { "en": "Aplikasi Resonator Rongga?", "id": "Oven Microwave, Akselerator Partikel." },
  { "en": "Mode Resonansi?", "id": "Pola Gelombang Berdiri Tertentu Dalam Resonator." },
  { "en": "Apa Itu Model Elektron Bebas?", "id": "Model Sederhana Untuk Konduktivitas Logam." },
  { "en": "Apa Itu Pita Energi?", "id": "Rentang Energi Yang Boleh Dimiliki Elektron." },
  { "en": "Apa Itu Celah Energi (Energy Gap)?", "id": "Rentang Energi Terlarang Antara Pita." },
  { "en": "Apa Itu Pita Valensi?", "id": "Pita Energi Tertinggi Yang Terisi Penuh." },
  { "en": "Apa Itu Pita Konduksi?", "id": "Pita Energi Di Atas Pita Valensi." },
  { "en": "Pita Energi Pada Konduktor?", "id": "Pita Valensi Dan Konduksi Tumpang Tindih." },
  { "en": "Pita Energi Pada Isolator?", "id": "Memiliki Celah Energi Yang Sangat Lebar." },
  { "en": "Pita Energi Pada Semikonduktor?", "id": "Memiliki Celah Energi Yang Sempit." },
  { "en": "Apa Itu Doping?", "id": "Menambahkan Pengotor Untuk Mengubah Sifat Semikonduktor." },
  { "en": "Semikonduktor Tipe-N?", "id": "Didoping Dengan Donor, Pembawa Muatan Mayoritas Elektron." },
  { "en": "Semikonduktor Tipe-P?", "id": "Didoping Dengan Akseptor, Pembawa Muatan Mayoritas Hole." },
  { "en": "Apa Itu Hole?", "id": "Kekosongan Elektron, Bertindak Seperti Muatan Positif." },
  { "en": "Apa Itu Sambungan P-N?", "id": "Batas Antara Semikonduktor Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Yang Dibuat Dari Sambungan P-N." },
  { "en": "Apa Itu Daerah Deplesi?", "id": "Daerah Tanpa Pembawa Muatan Di Sekitar Sambungan." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Mengurangi Daerah Deplesi, Arus Mengalir." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Memperlebar Daerah Deplesi, Arus Sangat Kecil." },
  { "en": "Apa Itu Penyearahan?", "id": "Proses Mengubah AC Menjadi DC Menggunakan Dioda." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Untuk Amplifikasi Dan Switching." },
  { "en": "Apa Itu Momen Dipol Atom?", "id": "Momen Dipol Magnetik Dari Elektron." },
  { "en": "Apa Itu Spin Elektron?", "id": "Sifat Kuantum Intrinsik Yang Menghasilkan Magnetisme." },
  { "en": "Mengapa Kebanyakan Bahan Tidak Magnetik?", "id": "Momen Dipol Atom Saling Menghilangkan." },
  { "en": "Klasifikasi Bahan Magnetik?", "id": "Diamagnetik, Paramagnetik, Feromagnetik." },
  { "en": "Apa Itu Diamagnetisme?", "id": "Efek Lemah Yang Menolak Medan Magnet Eksternal." },
  { "en": "Asal Mula Diamagnetisme?", "id": "Perubahan Gerak Orbital Elektron." },
  { "en": "Apakah Semua Bahan Diamagnetik?", "id": "Ya, Tetapi Seringkali Tertutupi Efek Lain." },
  { "en": "Apa Itu Paramagnetisme?", "id": "Efek Lemah Yang Menarik Medan Magnet Eksternal." },
  { "en": "Asal Mula Paramagnetisme?", "id": "Penjajaran Momen Dipol Atom Yang Acak." },
  { "en": "Apa Itu Ferromagnetisme?", "id": "Efek Sangat Kuat Yang Menarik Medan Magnet." },
  { "en": "Asal Mula Ferromagnetisme?", "id": "Interaksi Kuantum Yang Menjajarkan Spin." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Daerah Penjajaran Spin Dalam Bahan Feromagnetik." },
  { "en": "Proses Magnetisasi Bahan Feromagnetik?", "id": "Pertumbuhan Dan Rotasi Domain Magnetik." },
  { "en": "Apa Itu Titik Curie?", "id": "Suhu Di Atas Dimana Feromagnet Menjadi Paramagnet." },
  { "en": "Apa Itu Histeresis?", "id": "Ketergantungan Keadaan Saat Ini Pada Riwayatnya." },
  { "en": "Kurva Histeresis Magnetik (B-H)?", "id": "Plot Yang Menunjukkan Sifat Histeresis." },
  { "en": "Bahan Magnetik Keras?", "id": "Sulit Dimagnetisasi Dan Didemagnetisasi." },
  { "en": "Aplikasi Magnet Keras?", "id": "Magnet Permanen, Pita Perekam." },
  { "en": "Bahan Magnetik Lunak?", "id": "Mudah Dimagnetisasi Dan Didemagnetisasi." },
  { "en": "Aplikasi Magnet Lunak?", "id": "Inti Transformator Dan Induktor." },
  { "en": "Apa Itu Demagnetisasi?", "id": "Proses Menghilangkan Sifat Magnet Suatu Bahan." },
  { "en": "Bagaimana Cara Demagnetisasi?", "id": "Memanaskan Di Atas Titik Curie, Medan AC." },
  { "en": "Apa Itu Permeabilitas Magnetik (μ)?", "id": "Ukuran Respon Bahan Terhadap Medan Magnet." },
  { "en": "Permeabilitas Relatif?", "id": "Rasio Permeabilitas Bahan Terhadap Ruang Hampa." },
  { "en": "Permeabilitas Feromagnetik?", "id": "Sangat Besar." },
  { "en": "Apa Itu Suseptibilitas Magnetik (χ)?", "id": "Ukuran Lain Dari Sifat Magnetik Bahan." },
  { "en": "Apa Itu Antiferromagnetisme?", "id": "Spin Tetangga Sejajar Secara Antiparalel." },
  { "en": "Apa Itu Ferrimagnetisme?", "id": "Spin Tetangga Antiparalel Tapi Tidak Sama Besar." },
  { "en": "Contoh Ferrimagnet?", "id": "Ferrit, Magnetit." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Listrik." },
  { "en": "Apa Itu Polarisasi?", "id": "Pemisahan Muatan Positif Dan Negatif." },
  { "en": "Apa Itu Momen Dipol Terinduksi?", "id": "Dipol Yang Dibuat Oleh Medan Listrik Eksternal." },
  { "en": "Apa Itu Vektor Polarisasi (P)?", "id": "Momen Dipol Per Satuan Volume." },
  { "en": "Apa Itu Permitivitas (ε)?", "id": "Ukuran Respon Dielektrik Terhadap Medan Listrik." },
  { "en": "Konstanta Dielektrik (κ)?", "id": "Rasio Kapasitansi Dengan Dan Tanpa Dielektrik." },
  { "en": "Hubungan Permitivitas Dan Konstanta Dielektrik?", "id": "ε = κ * ε₀." },
  { "en": "Mengapa Dielektrik Melemahkan Medan Listrik?", "id": "Medan Internalnya Melawan Medan Eksternal." },
  { "en": "Apa Itu Suseptibilitas Listrik (χe)?", "id": "Ukuran Seberapa Mudah Bahan Dipolarisasi." },
  { "en": "Apa Itu Vektor Pergeseran Listrik (D)?", "id": "Medan Yang Berguna Di Dalam Dielektrik." },
  { "en": "Hubungan D, E, Dan P?", "id": "D = ε₀E + P." },
  { "en": "Bahan Dielektrik Polar?", "id": "Memiliki Momen Dipol Permanen (Contoh: Air)." },
  { "en": "Bahan Dielektrik Nonpolar?", "id": "Tidak Memiliki Momen Dipol Permanen." },
  { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Medan Listrik Maksimum Sebelum Breakdown." },
  { "en": "Apa Itu Breakdown Dielektrik?", "id": "Isolator Tiba-tiba Menjadi Konduktor." },
  { "en": "Apa Itu Piezoelektrik?", "id": "Bahan Yang Menghasilkan Tegangan Saat Ditekan." },
  { "en": "Aplikasi Piezoelektrik?", "id": "Sensor Tekanan, Pemantik Api, Mikrofon." },
  { "en": "Apa Itu Piroelektrik?", "id": "Bahan Yang Menghasilkan Tegangan Akibat Perubahan Suhu." },
  { "en": "Aplikasi Piroelektrik?", "id": "Detektor Inframerah, Sensor Gerak." },
  { "en": "Apa Itu Feroelektrik?", "id": "Bahan Dengan Polarisasi Spontan Yang Dapat Dibalik." },
  { "en": "Analogi Feroelektrik?", "id": "Analogi Listrik Dari Ferromagnetisme." },
  { "en": "Apa Itu Histeresis Feroelektrik?", "id": "Kurva P-E Yang Mirip Kurva B-H." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Fenomena Yang Menghubungkan Panas Dan Listrik." },
  { "en": "Efek Seebeck?", "id": "Tegangan Dihasilkan Oleh Perbedaan Suhu." },
  { "en": "Aplikasi Efek Seebeck?", "id": "Termokopel (Sensor Suhu)." },
  { "en": "Efek Peltier?", "id": "Arus Listrik Menyebabkan Transfer Panas." },
  { "en": "Aplikasi Efek Peltier?", "id": "Pendingin Termoelektrik Ukuran Kecil." },
  { "en": "Apa Itu Superkonduktivitas?", "id": "Hilangnya Resistansi Listrik Secara Total." },
  { "en": "Pada Suhu Berapa Superkonduktivitas Terjadi?", "id": "Pada Suhu Kritis Yang Sangat Rendah." },
  { "en": "Apa Itu Suhu Kritis?", "id": "Suhu Transisi Menuju Keadaan Superkonduktor." },
  { "en": "Apa Itu Efek Meissner?", "id": "Penolakan Total Medan Magnet Oleh Superkonduktor." },
  { "en": "Superkonduktor Adalah Bahan Diamagnetik Sempurna?", "id": "Ya, Karena Efek Meissner." },
  { "en": "Teori BCS?", "id": "Teori Mikroskopis Yang Menjelaskan Superkonduktivitas." },
  { "en": "Apa Itu Pasangan Cooper?", "id": "Pasangan Elektron Yang Memungkinkan Superkonduktivitas." },
  { "en": "Aplikasi Superkonduktor?", "id": "MRI, Kereta Maglev, Akselerator Partikel." },
  { "en": "Apa Itu Kereta Maglev?", "id": "Kereta Yang Melayang Menggunakan Magnet Superkonduktor." },
  { "en": "Apa Itu SQUID?", "id": "Superconducting Quantum Interference Device." },
  { "en": "Fungsi SQUID?", "id": "Sensor Medan Magnet Paling Sensitif." },
  { "en": "Superkonduktor Suhu Tinggi?", "id": "Bahan Yang Menjadi Superkonduktor Di Atas 77K." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Rekayasa Dengan Sifat Elektromagnetik Unik." },
  { "en": "Sifat Metamaterial?", "id": "Dapat Memiliki Permitivitas Atau Permeabilitas Negatif." },
  { "en": "Aplikasi Potensial Metamaterial?", "id": "Jubah Gaib, Antena Super, Lensa Sempurna." },
  { "en": "Apa Itu Kristal Fotonik?", "id": "Material Dengan Struktur Periodik Untuk Mengontrol Cahaya." },
  { "en": "Apa Itu Celah Pita Fotonik?", "id": "Rentang Frekuensi Cahaya Yang Dilarang Merambat." },
  { "en": "Apa Itu Bahan Kiral?", "id": "Bahan Yang Berinteraksi Berbeda Dengan Polarisasi." },
  { "en": "Siapa James Clerk Maxwell?", "id": "Ilmuwan Yang Menyatukan Listrik Dan Magnet." },
  { "en": "Ada Berapa Persamaan Maxwell?", "id": "Empat Persamaan Fundamental." },
  { "en": "Hukum Pertama Maxwell?", "id": "Hukum Gauss Untuk Kelistrikan." },
  { "en": "Makna Fisik Hukum Gauss (Listrik)?", "id": "Muatan Listrik Adalah Sumber Medan Listrik." },
  { "en": "Hukum Kedua Maxwell?", "id": "Hukum Gauss Untuk Kemagnetan." },
  { "en": "Makna Fisik Hukum Gauss (Magnet)?", "id": "Tidak Ada Monopol Magnetik Yang Ada." },
  { "en": "Hukum Ketiga Maxwell?", "id": "Hukum Induksi Faraday." },
  { "en": "Makna Fisik Hukum Faraday?", "id": "Perubahan Medan Magnet Menciptakan Medan Listrik." },
  { "en": "Hukum Keempat Maxwell?", "id": "Hukum Ampere-Maxwell." },
  { "en": "Makna Fisik Hukum Ampere-Maxwell?", "id": "Arus Dan Perubahan Medan Listrik Ciptakan Medan Magnet." },
  { "en": "Apa Itu Arus Pergeseran?", "id": "Suku Yang Ditambahkan Maxwell Ke Hukum Ampere." },
  { "en": "Mengapa Arus Pergeseran Penting?", "id": "Memungkinkan Adanya Gelombang Elektromagnetik." },
  { "en": "Bentuk Diferensial vs Integral?", "id": "Dua Cara Menulis Persamaan Maxwell." },
  { "en": "Apa Itu Persamaan Gelombang?", "id": "Dapat Diturunkan Dari Persamaan Maxwell." },
  { "en": "Apa Itu Gelombang Elektromagnetik (EM)?", "id": "Osilasi Medan E Dan B Yang Merambat." },
  { "en": "Apakah Gelombang EM Membutuhkan Medium?", "id": "Tidak, Dapat Merambat Di Ruang Hampa." },
  { "en": "Berapa Kecepatan Gelombang EM?", "id": "Kecepatan Cahaya (c)." },
  { "en": "Hubungan Kecepatan Cahaya, Permitivitas, Permeabilitas?", "id": "c = 1 / sqrt(ε₀μ₀)." },
  { "en": "Apa Sifat Gelombang EM?", "id": "Transversal." },
  { "en": "Apa Artinya Gelombang Transversal?", "id": "Osilasi Tegak Lurus Arah Perambatan." },
  { "en": "Hubungan Medan E Dan B?", "id": "Saling Tegak Lurus." },
  { "en": "Hubungan E, B, Dan Arah Rambat?", "id": "Saling Tegak Lurus Satu Sama Lain." },
  { "en": "Bagaimana Menentukan Arah Rambat?", "id": "Dengan Aturan Tangan Kanan (E Cross B)." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Seluruh Rentang Frekuensi Gelombang EM." },
  { "en": "Jenis Gelombang Dengan Frekuensi Terendah?", "id": "Gelombang Radio." },
  { "en": "Jenis Gelombang Dengan Frekuensi Tertinggi?", "id": "Sinar Gamma." },
  { "en": "Apa Itu Cahaya Tampak?", "id": "Bagian Kecil Dari Spektrum EM." },
  { "en": "Urutan Warna Cahaya Tampak?", "id": "Merah, Jingga, Kuning, Hijau, Biru, Nila, Ungu." },
  { "en": "Hubungan Frekuensi Dan Panjang Gelombang?", "id": "Berbanding Terbalik (c = fλ)." },
  { "en": "Bagaimana Gelombang Radio Dihasilkan?", "id": "Oleh Osilasi Elektron Dalam Antena." },
  { "en": "Bagaimana Sinar-X Dihasilkan?", "id": "Oleh Tumbukan Elektron Berenergi Tinggi." },
  { "en": "Energi Yang Dibawa Gelombang EM?", "id": "Sebanding Dengan Kuadrat Amplitudo Medan." },
  { "en": "Energi Terbagi Antara Medan E Dan B?", "id": "Ya, Terbagi Sama Rata." },
  { "en": "Apa Itu Vektor Poynting (S)?", "id": "Menyatakan Laju Aliran Energi EM." },
  { "en": "Arah Vektor Poynting?", "id": "Sama Dengan Arah Perambatan Gelombang." },
  { "en": "Apa Itu Intensitas Gelombang?", "id": "Nilai Rata-rata Dari Magnitudo Vektor Poynting." },
  { "en": "Apa Itu Momentum Gelombang EM?", "id": "Gelombang EM Mentransfer Momentum." },
  { "en": "Apa Itu Tekanan Radiasi?", "id": "Gaya Per Satuan Luas Oleh Gelombang EM." },
  { "en": "Apa Itu Polarisasi?", "id": "Orientasi Osilasi Medan Listrik." },
  { "en": "Apa Itu Polarisasi Linear (Bidang)?", "id": "Medan E Berosilasi Pada Satu Garis." },
  { "en": "Apa Itu Polarisasi Melingkar?", "id": "Ujung Vektor Medan E Membentuk Lingkaran." },
  { "en": "Apa Itu Polarisasi Elips?", "id": "Ujung Vektor Medan E Membentuk Elips." },
  { "en": "Apa Itu Cahaya Tak Terpolarisasi?", "id": "Superposisi Banyak Gelombang Dengan Polarisasi Acak." },
  { "en": "Apa Itu Filter Polarisasi (Polaroid)?", "id": "Hanya Melewatkan Satu Komponen Polarisasi." },
  { "en": "Apa Itu Hukum Malus?", "id": "Menentukan Intensitas Cahaya Melewati Polaroid." },
  { "en": "Polarisasi Oleh Hamburan?", "id": "Cahaya Dihamburkan Udara Menjadi Terpolarisasi." },
  { "en": "Mengapa Langit Berwarna Biru?", "id": "Karena Hamburan Rayleigh Cahaya Matahari." },
  { "en": "Polarisasi Oleh Pemantulan?", "id": "Cahaya Pantul Dari Permukaan Non-Logam Terpolarisasi." },
  { "en": "Apa Itu Sudut Brewster?", "id": "Sudut Datang Dimana Pantulan Terpolarisasi Sempurna." },
  { "en": "Apa Itu Persamaan Fresnel?", "id": "Mendeskripsikan Pemantulan Dan Transmisi Cahaya." },
  { "en": "Apa Itu Kondisi Batas?", "id": "Aturan Yang Harus Dipatuhi Medan Di Batas." },
  { "en": "Komponen Medan E Yang Kontinu?", "id": "Komponen Tangensial." },
  { "en": "Komponen Medan B Yang Kontinu?", "id": "Komponen Normal." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Pipa Untuk Memandu Gelombang EM." },
  { "en": "Aplikasi Pemandu Gelombang?", "id": "Microwave, Radar, Serat Optik." },
  { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Minimum Agar Gelombang Dapat Merambat." },
  { "en": "Apa Itu Mode Transversal Elektrik (TE)?", "id": "Medan Listrik Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Mode Transversal Magnetik (TM)?", "id": "Medan Magnet Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Mode TEM?", "id": "Medan E Dan B Tegak Lurus Arah Rambat." },
  { "en": "Apakah Mode TEM Ada Di Pemandu Gelombang Berongga?", "id": "Tidak, Hanya TE Dan TM." },
  { "en": "Apa Itu Resonator Rongga?", "id": "Pemandu Gelombang Tertutup Yang Menjebak Energi." },
  { "en": "Aplikasi Resonator Rongga?", "id": "Oven Microwave, Filter Frekuensi." },
  { "en": "Apa Itu Serat Optik?", "id": "Pemandu Gelombang Untuk Cahaya." },
  { "en": "Prinsip Kerja Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Radiasi Dipol?", "id": "Pola Radiasi Dari Dipol Yang Berosilasi." },
  { "en": "Apa Itu Potensial Elektromagnetik?", "id": "Potensial Skalar Dan Vektor." },
  { "en": "Apa Itu Kalibrasi Lorentz (Lorentz Gauge)?", "id": "Kondisi Untuk Menyederhanakan Persamaan Potensial." },
  { "en": "Apa Itu Potensial Liénard-Wiechert?", "id": "Potensial Dari Muatan Titik Bergerak." },
  { "en": "Apa Itu Radiasi Sinkrotron?", "id": "Radiasi Dari Muatan Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu Antena?", "id": "Alat Untuk Memancarkan Dan Menerima Gelombang EM." },
  { "en": "Apa Itu Daerah Dekat (Near Field)?", "id": "Daerah Dekat Antena." },
  { "en": "Apa Itu Daerah Jauh (Far Field)?", "id": "Daerah Jauh Dari Antena." },
  { "en": "Apa Itu Vektor Potensial Magnetik (A)?", "id": "Medan Vektor Yang Curl-nya Adalah Medan B." },
  { "en": "Apa Itu Potensial Skalar Listrik (V)?", "id": "Medan Skalar Yang Gradiennya Adalah Medan E." },
  { "en": "Kebebasan Kalibrasi (Gauge Freedom)?", "id": "Fleksibilitas Dalam Memilih Potensial." },
  { "en": "Apa Itu Persamaan Kontinuitas Muatan?", "id": "Menyatakan Bahwa Muatan Kekal." },
  { "en": "Hubungan Persamaan Maxwell Dengan Relativitas?", "id": "Persamaan Maxwell Invarian Terhadap Transformasi Lorentz." },
  { "en": "Apa Itu Tensor Medan Elektromagnetik?", "id": "Objek Matematis Yang Menggabungkan Medan E B." },
  { "en": "Apa Itu Elektrodinamika?", "id": "Studi Tentang Medan Elektromagnetik." },
  { "en": "Apa Itu Elektrostatika?", "id": "Studi Muatan Diam." },
  { "en": "Apa Itu Magnetostatika?", "id": "Studi Arus Konstan." },
  { "en": "Apa Itu Radiasi Termal?", "id": "Gelombang EM Yang Dipancarkan Benda Akibat Suhu." },
  { "en": "Apa Itu Radiasi Benda Hitam?", "id": "Spektrum Radiasi Termal Ideal." },
  { "en": "Apa Itu Birefringence?", "id": "Sifat Beberapa Kristal Memisahkan Polarisasi." },
  { "en": "Apa Itu Aktivitas Optik?", "id": "Kemampuan Bahan Memutar Bidang Polarisasi." },
  { "en": "Apa Itu Efek Faraday?", "id": "Rotasi Polarisasi Akibat Medan Magnet." },
  { "en": "Apa Itu Efek Pockels?", "id": "Perubahan Indeks Bias Akibat Medan Listrik." },
  { "en": "Apa Itu Efek Kerr?", "id": "Perubahan Indeks Bias Sebanding Kuadrat Medan." },
  { "en": "Apa Itu Laser?", "id": "Light Amplification by Stimulated Emission of Radiation." },
  { "en": "Apa Itu Emisi Terstimulasi?", "id": "Proses Dasar Penguatan Cahaya Dalam Laser." },
  { "en": "Apa Itu Holografi?", "id": "Teknik Untuk Merekam Dan Merekonstruksi Citra 3D." },
  { "en": "Apa Itu Dispersi?", "id": "Fenomena Dimana Kecepatan Cahaya Bergantung Frekuensi." },
  { "en": "Penyebab Pelangi?", "id": "Dispersi Cahaya Matahari Oleh Tetesan Air." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Cenderung Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Ukuran Seberapa Jauh Arus Menembus." },
  { "en": "Apa Itu Motor Listrik?", "id": "Mengubah Energi Listrik Menjadi Gerak Mekanis." },
  { "en": "Prinsip Kerja Motor?", "id": "Torsi Pada Loop Arus Dalam Medan Magnet." },
  { "en": "Apa Itu Generator Listrik?", "id": "Mengubah Energi Mekanis Menjadi Listrik." },
  { "en": "Prinsip Kerja Generator?", "id": "Induksi Elektromagnetik (Hukum Faraday)." },
  { "en": "Apa Itu Transformator?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Prinsip Kerja Transformator?", "id": "Induktansi Timbal Balik." },
  { "en": "Apa Itu Speaker?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Suara." },
  { "en": "Bagaimana Speaker Bekerja?", "id": "Gaya Magnetik Pada Kumparan Yang Bergerak." },
  { "en": "Apa Itu Mikrofon?", "id": "Mengubah Gelombang Suara Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Mikrofon Dinamis?", "id": "Menggunakan Prinsip Induksi Elektromagnetik." },
  { "en": "Apa Itu Bel Listrik?", "id": "Menggunakan Elektromagnet Untuk Memukul Lonceng." },
  { "en": "Apa Itu Relai (Relay)?", "id": "Saklar Yang Dioperasikan Secara Elektromagnetik." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Menyimpan Data Menggunakan Pola Magnetik." },
  { "en": "Apa Itu Head Baca/Tulis?", "id": "Elektromagnet Kecil Untuk Membaca Menulis Data." },
  { "en": "Apa Itu Pita Magnetik?", "id": "Media Penyimpanan Data Magnetik." },
  { "en": "Apa Itu Kartu Kredit/Debit?", "id": "Menggunakan Strip Magnetik Untuk Menyimpan Informasi." },
  { "en": "Apa Itu Kereta Maglev?", "id": "Kereta Yang Melayang Di Atas Rel." },
  { "en": "Prinsip Kereta Maglev?", "id": "Gaya Tolak-Menolak Antara Magnet Superkonduktor." },
  { "en": "Apa Itu Pengereman Elektromagnetik?", "id": "Menggunakan Arus Eddy Untuk Menghasilkan Gaya Rem." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Memanaskan Benda Konduktif Dengan Arus Eddy." },
  { "en": "Aplikasi Pemanasan Induksi?", "id": "Kompor Induksi, Peleburan Logam." },
  { "en": "Apa Itu Metal Detector?", "id": "Mendeteksi Benda Logam Melalui Induksi." },
  { "en": "Apa Itu Magnetic Resonance Imaging (MRI)?", "id": "Teknik Pencitraan Medis." },
  { "en": "Prinsip MRI?", "id": "Memanfaatkan Sifat Magnetik Inti Atom Hidrogen." },
  { "en": "Apa Itu Akselerator Partikel?", "id": "Menggunakan Medan Listrik Dan Magnet Untuk Mempercepat." },
  { "en": "Apa Itu Siklotron?", "id": "Jenis Akselerator Partikel." },
  { "en": "Apa Itu Mass Spectrometer?", "id": "Memisahkan Partikel Berdasarkan Rasio Massa-Muatan." },
  { "en": "Apa Itu Magnetometer?", "id": "Alat Untuk Mengukur Kekuatan Medan Magnet." },
  { "en": "Apa Itu Kompas?", "id": "Jarum Magnetik Yang Sejajar Medan Magnet Bumi." },
  { "en": "Apa Itu Aurora?", "id": "Cahaya Di Langit Akibat Partikel Bermuatan." },
  { "en": "Hubungan Aurora Dengan Medan Magnet?", "id": "Partikel Dibelokkan Menuju Kutub Oleh Medan Bumi." },
  { "en": "Apa Itu Sabuk Van Allen?", "id": "Zona Partikel Bermuatan Terjebak Medan Magnet." },
  { "en": "Apa Itu Terapi Magnetik?", "id": "Penggunaan Medan Magnet Untuk Tujuan Medis." },
  { "en": "Apa Itu Transcranial Magnetic Stimulation (TMS)?", "id": "Menstimulasi Otak Menggunakan Medan Magnet." },
  { "en": "Apa Itu Sistem Pengapian Mobil?", "id": "Menggunakan Induksi Untuk Menciptakan Bunga Api." },
  { "en": "Apa Itu Ground Fault Circuit Interrupter (GFCI)?", "id": "Menggunakan Induksi Untuk Mendeteksi Kebocoran Arus." },
  { "en": "Apa Itu Wireless Charging?", "id": "Mentransfer Energi Melalui Medan Magnet." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Menggunakan Gelombang Radio Untuk Identifikasi." },
  { "en": "Apa Itu Near Field Communication (NFC)?", "id": "Bentuk Jarak Dekat Dari RFID." },
  { "en": "Apa Itu Radar?", "id": "Radio Detection and Ranging." },
  { "en": "Prinsip Radar?", "id": "Memancarkan Gelombang Radio Dan Menganalisis Pantulannya." },
  { "en": "Apa Itu Oven Microwave?", "id": "Memanaskan Makanan Dengan Radiasi Gelombang Mikro." },
  { "en": "Bagaimana Microwave Memanaskan Makanan?", "id": "Molekul Air Beresonansi Dengan Gelombang." },
  { "en": "Mengapa Oven Microwave Berupa Sangkar Faraday?", "id": "Untuk Mencegah Radiasi Keluar." },
  { "en": "Apa Itu Remote Control?", "id": "Menggunakan Radiasi Inframerah Untuk Mengirim Sinyal." },
  { "en": "Apa Itu Kacamata Penglihatan Malam?", "id": "Mendeteksi Radiasi Inframerah Yang Dipancarkan Benda." },
  { "en": "Apa Itu Sinar-X Dalam Medis?", "id": "Digunakan Untuk Pencitraan Tulang." },
  { "en": "Mengapa Sinar-X Bisa Menembus Jaringan?", "id": "Karena Energinya Yang Sangat Tinggi." },
  { "en": "Apa Itu Terapi Radiasi?", "id": "Menggunakan Sinar Gamma Untuk Membunuh Sel Kanker." },
  { "en": "Apa Itu Sterilisasi Dengan Radiasi?", "id": "Menggunakan Radiasi Untuk Membunuh Mikroba." },
  { "en": "Apa Itu Komunikasi Radio?", "id": "Transmisi Informasi Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Amplitudo Gelombang Pembawa Diubah." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Frekuensi Gelombang Pembawa Diubah." },
  { "en": "Apa Itu Wi-Fi?", "id": "Komunikasi Data Nirkabel Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Radio Jarak Pendek." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Menggunakan Sinyal Radio Dari Satelit." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Menggunakan Gelombang Mikro Untuk Komunikasi Jarak Jauh." },
  { "en": "Apa Itu Astronomi Radio?", "id": "Mempelajari Alam Semesta Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Serat Optik?", "id": "Mentransmisikan Data Sebagai Pulsa Cahaya." },
  { "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren Dan Monokromatik." },
  { "en": "Aplikasi Laser?", "id": "Pemutar CD/DVD, Barcode Scanner, Pembedahan." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Sumber Cahaya Semikonduktor." },
  { "en": "Apa Itu Sel Surya?", "id": "Mengubah Energi Cahaya Menjadi Energi Listrik." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Elektron Dipancarkan Dari Logam Saat Disinari." },
  { "en": "Apa Itu Penyearah?", "id": "Komponen Yang Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Cathode Ray Tube (CRT)?", "id": "Tabung Yang Menggunakan Medan Untuk Membelokkan Elektron." },
  { "en": "Aplikasi CRT?", "id": "Televisi Dan Monitor Lama." },
  { "en": "Apa Itu Magnetron?", "id": "Sumber Gelombang Mikro Di Oven Microwave." },
  { "en": "Apa Itu Giroskop?", "id": "Mengukur Atau Mempertahankan Orientasi." },
  { "en": "Apa Itu Magnetic Levitation?", "id": "Melayangkan Objek Menggunakan Medan Magnet." },
  { "en": "Apa Itu Magnetohydrodynamics (MHD)?", "id": "Studi Dinamika Fluida Konduktif." },
  { "en": "Aplikasi MHD?", "id": "Pendorong Kapal Selam, Generator Listrik." },
  { "en": "Apa Itu Electric Guitar Pickup?", "id": "Menggunakan Induksi Untuk Mengubah Getaran Senar." },
  { "en": "Apa Itu Tape Eraser?", "id": "Menggunakan Medan Magnet Kuat Untuk Menghapus Data." },
  { "en": "Apa Itu Magnetic Stirrer?", "id": "Mengaduk Cairan Menggunakan Batang Magnet Berputar." },
  { "en": "Apa Itu Magnetic Separator?", "id": "Memisahkan Bahan Magnetik Dari Non-Magnetik." },
  { "en": "Apa Itu Solenoid Valve?", "id": "Katup Yang Dioperasikan Oleh Elektromagnet." },
  { "en": "Apa Itu Magnetic Bearing?", "id": "Bantalan Yang Menggunakan Levitasi Magnetik." },
  { "en": "Apa Itu Kunci Elektromagnetik?", "id": "Mengunci Pintu Menggunakan Elektromagnet Kuat." },
  { "en": "Apa Itu Welding (Pengelasan)?", "id": "Dapat Menggunakan Busur Listrik Suhu Tinggi." },
  { "en": "Apa Itu Electroplating (Penyepuhan)?", "id": "Melapisi Objek Logam Menggunakan Listrik." },
  { "en": "Apa Itu Cathodic Protection?", "id": "Melindungi Logam Dari Korosi." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Untuk Memvisualisasikan Sinyal Tegangan." },
  { "en": "Apa Itu Penapis Debu Elektrostatis?", "id": "Menggunakan Medan Listrik Untuk Menangkap Partikel." },
  { "en": "Aplikasi Penapis Debu?", "id": "Mengurangi Polusi Udara Dari Pabrik." },
  { "en": "Apa Itu Mesin Fotokopi?", "id": "Menggunakan Prinsip Elektrostatika Untuk Menyalin." },
  { "en": "Apa Itu Printer Laser?", "id": "Menggunakan Laser Dan Elektrostatika Untuk Mencetak." },
  { "en": "Apa Itu Layar Sentuh Kapasitif?", "id": "Mendeteksi Sentuhan Melalui Perubahan Kapasitansi." },
  { "en": "Apa Itu Defibrillator?", "id": "Menggunakan Kejut Listrik Untuk Mereset Jantung." },
  { "en": "Apa Itu Electroencephalography (EEG)?", "id": "Merekam Aktivitas Listrik Otak." },
  { "en": "Apa Itu Electrocardiography (ECG/EKG)?", "id": "Merekam Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Magnetoencephalography (MEG)?", "id": "Merekam Medan Magnet Yang Dihasilkan Otak." },
  { "en": "Apa Itu Magnetic Ink Character Recognition (MICR)?", "id": "Membaca Tinta Magnetik Di Cek Bank." },
  { "en": "Apa Itu Komunikasi Serat Optik?", "id": "Menggunakan Cahaya Untuk Transmisi Data." },
  { "en": "Apa Itu Pemantulan Internal Total?", "id": "Prinsip Dasar Dari Serat Optik." },
  { "en": "Apa Itu Skalar?", "id": "Besaran Yang Hanya Memiliki Nilai." },
  { "en": "Contoh Skalar?", "id": "Suhu, Massa, Waktu, Potensial Listrik." },
  { "en": "Apa Itu Vektor?", "id": "Besaran Yang Memiliki Nilai Dan Arah." },
  { "en": "Contoh Vektor?", "id": "Kecepatan, Gaya, Medan Listrik, Medan Magnet." },
  { "en": "Bagaimana Menggambarkan Vektor?", "id": "Sebagai Anak Panah." },
  { "en": "Apa Itu Penjumlahan Vektor?", "id": "Menggabungkan Dua Vektor Atau Lebih." },
  { "en": "Metode Penjumlahan Vektor?", "id": "Metode Segitiga, Jajar Genjang, Poligon." },
  { "en": "Apa Itu Vektor Satuan?", "id": "Vektor Dengan Panjang (Magnitudo) Satu." },
  { "en": "Vektor Satuan i, j, k?", "id": "Mewakili Arah Sumbu X, Y, Z." },
  { "en": "Apa Itu Komponen Vektor?", "id": "Proyeksi Vektor Pada Sumbu Koordinat." },
  { "en": "Apa Itu Perkalian Titik (Dot Product)?", "id": "Perkalian Dua Vektor Yang Menghasilkan Skalar." },
  { "en": "Hasil Perkalian Titik?", "id": "Skalar." },
  { "en": "Kapan Perkalian Titik Nol?", "id": "Saat Dua Vektor Saling Tegak Lurus." },
  { "en": "Apa Itu Perkalian Silang (Cross Product)?", "id": "Perkalian Dua Vektor Yang Menghasilkan Vektor." },
  { "en": "Hasil Perkalian Silang?", "id": "Vektor Baru." },
  { "en": "Arah Hasil Perkalian Silang?", "id": "Tegak Lurus Terhadap Bidang Kedua Vektor." },
  { "en": "Kapan Perkalian Silang Nol?", "id": "Saat Dua Vektor Sejajar Atau Antiparalel." },
  { "en": "Apa Itu Sistem Koordinat?", "id": "Kerangka Acuan Untuk Mendefinisikan Posisi." },
  { "en": "Sistem Koordinat Kartesius?", "id": "Menggunakan Sumbu X, Y, Z." },
  { "en": "Sistem Koordinat Silinder?", "id": "Menggunakan Jari-jari (ρ), Sudut (φ), Tinggi (z)." },
  { "en": "Sistem Koordinat Bola?", "id": "Menggunakan Jari-jari (r), Sudut (θ, φ)." },
  { "en": "Apa Itu Kalkulus Vektor?", "id": "Cabang Matematika Tentang Diferensiasi Vektor." },
  { "en": "Apa Itu Medan Skalar?", "id": "Setiap Titik Dalam Ruang Diberi Nilai Skalar." },
  { "en": "Contoh Medan Skalar?", "id": "Peta Suhu, Peta Potensial Listrik." },
  { "en": "Apa Itu Medan Vektor?", "id": "Setiap Titik Dalam Ruang Diberi Vektor." },
  { "en": "Contoh Medan Vektor?", "id": "Medan Angin, Medan Listrik, Medan Gravitasi." },
  { "en": "Apa Itu Gradien?", "id": "Operasi Yang Mengubah Medan Skalar Menjadi Vektor." },
  { "en": "Apa Arti Fisik Gradien?", "id": "Menunjuk Ke Arah Kenaikan Maksimum Skalar." },
  { "en": "Simbol Operator Gradien?", "id": "Nabla (∇)." },
  { "en": "Apa Itu Divergensi?", "id": "Operasi Yang Mengubah Medan Vektor Menjadi Skalar." },
  { "en": "Apa Arti Fisik Divergensi?", "id": "Ukuran 'Sumber' Atau 'Sink' Dari Medan Vektor." },
  { "en": "Divergensi Positif Berarti?", "id": "Ada Sumber, Medan Menyebar Keluar." },
  { "en": "Divergensi Negatif Berarti?", "id": "Ada Sink, Medan Menyusut Ke Dalam." },
  { "en": "Divergensi Nol Berarti?", "id": "Tidak Ada Sumber Atau Sink (Solenoidal)." },
  { "en": "Apa Itu Curl?", "id": "Operasi Yang Mengubah Medan Vektor Menjadi Vektor." },
  { "en": "Apa Arti Fisik Curl?", "id": "Ukuran Rotasi Atau Sirkulasi Medan Vektor." },
  { "en": "Curl Nol Berarti?", "id": "Medan Tersebut Konservatif Atau Irotasional." },
  { "en": "Apa Itu Medan Konservatif?", "id": "Kerja Yang Dilakukan Tidak Bergantung Lintasan." },
  { "en": "Medan Listrik Statis Konservatif?", "id": "Ya, Karena Curl-nya Nol." },
  { "en": "Medan Magnet Konservatif?", "id": "Tidak, Karena Curl-nya Tidak Selalu Nol." },
  { "en": "Apa Itu Laplacian?", "id": "Divergensi Dari Gradien Suatu Medan Skalar." },
  { "en": "Simbol Laplacian?", "id": "Nabla Kuadrat (∇²)." },
  { "en": "Apa Itu Integral Garis?", "id": "Integral Suatu Fungsi Di Sepanjang Kurva." },
  { "en": "Aplikasi Integral Garis?", "id": "Menghitung Kerja, Sirkulasi, Beda Potensial." },
  { "en": "Apa Itu Integral Permukaan?", "id": "Integral Suatu Fungsi Di Atas Permukaan." },
  { "en": "Aplikasi Integral Permukaan?", "id": "Menghitung Fluks." },
  { "en": "Apa Itu Integral Volume?", "id": "Integral Suatu Fungsi Di Seluruh Volume." },
  { "en": "Aplikasi Integral Volume?", "id": "Menghitung Massa Total, Muatan Total." },
  { "en": "Apa Itu Teorema Gradien?", "id": "Menghubungkan Integral Garis Gradien Dengan Nilai Batas." },
  { "en": "Apa Itu Teorema Divergensi (Gauss)?", "id": "Menghubungkan Fluks Permukaan Dengan Integral Volume Divergensi." },
  { "en": "Apa Itu Teorema Curl (Stokes)?", "id": "Menghubungkan Sirkulasi Garis Dengan Fluks Curl." },
  { "en": "Apa Itu Fungsi Delta Dirac?", "id": "Fungsi Yang Bernilai Tak Hingga Di Satu Titik." },
  { "en": "Apa Itu Teorema Helmholtz?", "id": "Setiap Medan Vektor Dapat Diuraikan." },
  { "en": "Penguraian Teorema Helmholtz?", "id": "Menjadi Bagian Irotasional Dan Solenoidal." },
  { "en": "Apa Itu Vektor Posisi?", "id": "Vektor Dari Titik Asal Ke Suatu Titik." },
  { "en": "Apa Itu Elemen Garis Diferensial?", "id": "Vektor Perpindahan Yang Sangat Kecil." },
  { "en": "Apa Itu Elemen Permukaan Diferensial?", "id": "Vektor Area Yang Sangat Kecil." },
  { "en": "Apa Itu Elemen Volume Diferensial?", "id": "Volume Yang Sangat Kecil." },
  { "en": "Apa Itu Operator Del (Nabla)?", "id": "Operator Vektor Diferensial." },
  { "en": "Identitas Kalkulus Vektor?", "id": "Hubungan Matematika Antara Operator Vektor." },
  { "en": "Contoh Identitas: Curl Gradien?", "id": "Selalu Nol." },
  { "en": "Contoh Identitas: Divergensi Curl?", "id": "Selalu Nol." },
  { "en": "Apa Itu Transformasi Koordinat?", "id": "Mengubah Representasi Vektor Antar Sistem." },
  { "en": "Mengapa Kalkulus Vektor Penting?", "id": "Bahasa Matematika Untuk Elektromagnetisme." },
  { "en": "Bentuk Diferensial Hukum Gauss?", "id": "Divergensi D = ρ." },
  { "en": "Bentuk Diferensial Hukum Gauss Magnetisme?", "id": "Divergensi B = 0." },
  { "en": "Bentuk Diferensial Hukum Faraday?", "id": "Curl E = -∂B/∂t." },
  { "en": "Bentuk Diferensial Hukum Ampere-Maxwell?", "id": "Curl H = J + ∂D/∂t." },
  { "en": "Apa Itu Medan Solenoidal?", "id": "Medan Vektor Dengan Divergensi Nol." },
  { "en": "Contoh Medan Solenoidal?", "id": "Medan Magnet." },
  { "en": "Apa Itu Medan Irotasional?", "id": "Medan Vektor Dengan Curl Nol." },
  { "en": "Contoh Medan Irotasional?", "id": "Medan Listrik Elektrostatik." },
  { "en": "Hubungan Medan Irotasional Dan Potensial?", "id": "Dapat Dinyatakan Sebagai Gradien Potensial Skalar." },
  { "en": "Hubungan Medan Solenoidal Dan Potensial?", "id": "Dapat Dinyatakan Sebagai Curl Potensial Vektor." },
  { "en": "Apa Itu Persamaan Laplace?", "id": "Laplacian Dari Potensial Adalah Nol." },
  { "en": "Apa Itu Persamaan Poisson?", "id": "Laplacian Dari Potensial Sebanding Sumber." },
  { "en": "Uniqueness Theorem?", "id": "Solusi Persamaan Laplace/Poisson Adalah Unik." },
  { "en": "Apa Itu Metode Bayangan (Method of Images)?", "id": "Teknik Menyelesaikan Masalah Elektrostatik." },
  { "en": "Prinsip Metode Bayangan?", "id": "Mengganti Batas Dengan Muatan Imajiner." },
  { "en": "Apa Itu Ekspansi Multipol?", "id": "Aproksimasi Medan Jauh Dari Distribusi Muatan." },
  { "en": "Suku Pertama Ekspansi Multipol?", "id": "Monopol (Muatan Total)." },
  { "en": "Suku Kedua Ekspansi Multipol?", "id": "Dipol." },
  { "en": "Apa Itu Tensor?", "id": "Generalisasi Dari Skalar Dan Vektor." },
  { "en": "Apa Itu Perkalian Triple Skalar?", "id": "A ⋅ (B × C)." },
  { "en": "Interpretasi Geometris Triple Skalar?", "id": "Volume Jajar Genjang." },
  { "en": "Apa Itu Perkalian Triple Vektor?", "id": "A × (B × C)." },
  { "en": "Apa Itu Medan Vektor Radial?", "id": "Vektor Selalu Menunjuk Dari Atau Ke Pusat." },
  { "en": "Contoh Medan Radial?", "id": "Medan Gravitasi, Medan Listrik Muatan Titik." },
  { "en": "Apa Itu Fluks?", "id": "Laju Aliran Suatu Besaran Menembus Permukaan." },
  { "en": "Apa Itu Sirkulasi?", "id": "Integral Garis Medan Vektor Di Lintasan Tertutup." },
  { "en": "Sirkulasi Medan Konservatif?", "id": "Selalu Nol." },
  { "en": "Apa Itu Turunan Berarah?", "id": "Laju Perubahan Fungsi Dalam Arah Tertentu." },
  { "en": "Hubungan Turunan Berarah Dan Gradien?", "id": "Perkalian Titik Antara Gradien Dan Vektor Arah." },
  { "en": "Arah Turunan Berarah Maksimum?", "id": "Searah Dengan Vektor Gradien." },
  { "en": "Apa Itu Gelombang?", "id": "Getaran Yang Merambat." },
  { "en": "Apa Itu Gelombang Mekanik?", "id": "Gelombang Yang Membutuhkan Medium." },
  { "en": "Contoh Gelombang Mekanik?", "id": "Gelombang Suara, Gelombang Air." },
  { "en": "Apa Itu Gelombang Elektromagnetik (EM)?", "id": "Gelombang Yang Tidak Membutuhkan Medium." },
  { "en": "Apa Itu Gelombang Transversal?", "id": "Getaran Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Gelombang Longitudinal?", "id": "Getaran Sejajar Arah Rambat." },
  { "en": "Gelombang EM Termasuk Jenis Apa?", "id": "Gelombang Transversal." },
  { "en": "Apa Itu Amplitudo?", "id": "Simpangan Maksimum Dari Posisi Setimbang." },
  { "en": "Apa Itu Panjang Gelombang (λ)?", "id": "Jarak Antara Dua Puncak Berturutan." },
  { "en": "Apa Itu Frekuensi (f)?", "id": "Jumlah Getaran Per Detik." },
  { "en": "Apa Itu Periode (T)?", "id": "Waktu Untuk Satu Getaran Penuh." },
  { "en": "Hubungan Frekuensi Dan Periode?", "id": "f = 1/T." },
  { "en": "Apa Itu Kecepatan Gelombang?", "id": "Seberapa Cepat Gelombang Merambat." },
  { "en": "Rumus Kecepatan Gelombang?", "id": "v = f * λ." },
  { "en": "Apa Itu Prinsip Superposisi?", "id": "Gelombang Dapat Bertumpuk Satu Sama Lain." },
  { "en": "Apa Itu Interferensi?", "id": "Efek Gabungan Dari Superposisi Gelombang." },
  { "en": "Interferensi Konstruktif?", "id": "Saat Puncak Bertemu Puncak, Amplitudo Menguat." },
  { "en": "Interferensi Destruktif?", "id": "Saat Puncak Bertemu Lembah, Amplitudo Melemah." },
  { "en": "Apa Itu Koherensi?", "id": "Gelombang Dengan Hubungan Fasa Yang Konstan." },
  { "en": "Apa Itu Gelombang Berdiri?", "id": "Hasil Interferensi Gelombang Datang Dan Pantul." },
  { "en": "Apa Itu Simpul (Node)?", "id": "Titik Dengan Amplitudo Nol Pada Gelombang Berdiri." },
  { "en": "Apa Itu Perut (Antinode)?", "id": "Titik Dengan Amplitudo Maksimum." },
  { "en": "Apa Itu Optik?", "id": "Cabang Fisika Tentang Perilaku Cahaya." },
  { "en": "Apa Itu Optik Geometris?", "id": "Mempelajari Cahaya Sebagai Sinar Lurus." },
  { "en": "Apa Itu Pemantulan?", "id": "Sinar Cahaya Memantul Dari Permukaan." },
  { "en": "Apa Itu Hukum Pemantulan?", "id": "Sudut Datang Sama Dengan Sudut Pantul." },
  { "en": "Apa Itu Pemantulan Baur (Diffuse)?", "id": "Pemantulan Pada Permukaan Kasar." },
  { "en": "Apa Itu Pemantulan Teratur (Specular)?", "id": "Pemantulan Pada Permukaan Halus (Cermin)." },
  { "en": "Apa Itu Pembiasan?", "id": "Pembelokan Cahaya Saat Melewati Batas Medium." },
  { "en": "Penyebab Pembiasan?", "id": "Perubahan Kecepatan Cahaya Dalam Medium." },
  { "en": "Apa Itu Indeks Bias?", "id": "Ukuran Seberapa Lambat Cahaya Dalam Medium." },
  { "en": "Apa Itu Hukum Snellius?", "id": "Hukum Yang Mengatur Pembiasan Cahaya." },
  { "en": "Apa Itu Pemantulan Internal Total?", "id": "Cahaya Terpantul Sempurna Di Dalam Medium." },
  { "en": "Syarat Pemantulan Internal Total?", "id": "Dari Medium Rapat Ke Kurang Rapat." },
  { "en": "Apa Itu Sudut Kritis?", "id": "Sudut Datang Minimum Untuk Pemantulan Total." },
  { "en": "Apa Itu Dispersi?", "id": "Penguraian Cahaya Putih Menjadi Warna." },
  { "en": "Penyebab Dispersi?", "id": "Indeks Bias Bergantung Pada Panjang Gelombang." },
  { "en": "Apa Itu Prisma?", "id": "Benda Kaca Yang Dapat Mendispersi Cahaya." },
  { "en": "Apa Itu Lensa?", "id": "Benda Bening Untuk Memfokuskan Atau Menyebarkan Cahaya." },
  { "en": "Apa Itu Lensa Cembung (Konvergen)?", "id": "Mengumpulkan Sinar Cahaya." },
  { "en": "Apa Itu Lensa Cekung (Divergen)?", "id": "Menyebarkan Sinar Cahaya." },
  { "en": "Apa Itu Titik Fokus?", "id": "Titik Dimana Sinar Sejajar Dikumpulkan." },
  { "en": "Apa Itu Jarak Fokus?", "id": "Jarak Dari Pusat Lensa Ke Titik Fokus." },
  { "en": "Apa Itu Persamaan Pembuat Lensa?", "id": "Menghubungkan Jarak Fokus Dengan Kelengkungan." },
  { "en": "Apa Itu Bayangan Nyata?", "id": "Bayangan Yang Terbentuk Oleh Sinar Aktual." },
  { "en": "Apa Itu Bayangan Maya?", "id": "Bayangan Yang Tidak Dapat Ditangkap Layar." },
  { "en": "Apa Itu Perbesaran?", "id": "Rasio Ukuran Bayangan Terhadap Ukuran Benda." },
  { "en": "Apa Itu Cermin?", "id": "Permukaan Pemantul." },
  { "en": "Apa Itu Cermin Datar?", "id": "Menghasilkan Bayangan Maya, Sama Besar, Tegak." },
  { "en": "Apa Itu Cermin Cekung?", "id": "Cermin Konvergen." },
  { "en": "Apa Itu Cermin Cembung?", "id": "Cermin Divergen." },
  { "en": "Apa Itu Aberasi Sferis?", "id": "Cacat Lensa/Cermin Dimana Sinar Tepi Berbeda." },
  { "en": "Apa Itu Aberasi Kromatik?", "id": "Cacat Lensa Dimana Warna Berbeda." },
  { "en": "Apa Itu Optik Fisis?", "id": "Mempelajari Cahaya Sebagai Gelombang." },
  { "en": "Apa Itu Prinsip Huygens?", "id": "Setiap Titik Pada Muka Gelombang Adalah Sumber." },
  { "en": "Apa Itu Difraksi?", "id": "Pembelokan Gelombang Saat Melewati Penghalang." },
  { "en": "Apa Itu Eksperimen Celah Ganda Young?", "id": "Demonstrasi Sifat Gelombang Cahaya (Interferensi)." },
  { "en": "Apa Itu Pola Interferensi?", "id": "Pola Terang Dan Gelap Akibat Superposisi." },
  { "en": "Apa Itu Difraksi Celah Tunggal?", "id": "Pola Difraksi Dari Satu Celah Sempit." },
  { "en": "Apa Itu Kisi Difraksi?", "id": "Banyak Celah Sempit Yang Sejajar." },
  { "en": "Aplikasi Kisi Difraksi?", "id": "Spektrometer Untuk Menganalisis Spektrum Cahaya." },
  { "en": "Apa Itu Resolusi Optik?", "id": "Kemampuan Untuk Membedakan Dua Objek Dekat." },
  { "en": "Apa Itu Kriteria Rayleigh?", "id": "Batas Teoritis Untuk Resolusi." },
  { "en": "Apa Itu Lapisan Tipis (Thin Film)?", "id": "Menghasilkan Interferensi Akibat Pantulan." },
  { "en": "Contoh Interferensi Lapisan Tipis?", "id": "Warna-Warni Gelembung Sabun, Lapisan Minyak." },
  { "en": "Apa Itu Interferometer Michelson?", "id": "Alat Untuk Mengukur Panjang Gelombang." },
  { "en": "Apa Itu Holografi?", "id": "Merekam Informasi Amplitudo Dan Fasa Cahaya." },
  { "en": "Apa Itu Foton?", "id": "Partikel Kuantum Dari Cahaya." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Bukti Sifat Partikel Cahaya." },
  { "en": "Dualitas Gelombang-Partikel?", "id": "Cahaya Memiliki Sifat Gelombang Dan Partikel." },
  { "en": "Apa Itu Mata Manusia?", "id": "Sistem Optik Biologis." },
  { "en": "Bagian Mata Yang Bekerja Seperti Lensa?", "id": "Kornea Dan Lensa Mata." },
  { "en": "Bagian Mata Yang Bekerja Seperti Layar?", "id": "Retina." },
  { "en": "Apa Itu Akomodasi Mata?", "id": "Kemampuan Lensa Mata Mengubah Jarak Fokus." },
  { "en": "Apa Itu Rabun Jauh (Miopi)?", "id": "Cahaya Terfokus Di Depan Retina." },
  { "en": "Koreksi Rabun Jauh?", "id": "Menggunakan Lensa Cekung (Divergen)." },
  { "en": "Apa Itu Rabun Dekat (Hipermetropi)?", "id": "Cahaya Terfokus Di Belakang Retina." },
  { "en": "Koreksi Rabun Dekat?", "id": "Menggunakan Lensa Cembung (Konvergen)." },
  { "en": "Apa Itu Astigmatisma?", "id": "Kornea Tidak Berbentuk Bola Sempurna." },
  { "en": "Apa Itu Kamera?", "id": "Alat Optik Untuk Merekam Gambar." },
  { "en": "Apa Itu Apertur?", "id": "Bukaan Lensa Yang Mengontrol Jumlah Cahaya." },
  { "en": "Apa Itu Shutter Speed?", "id": "Lamanya Sensor Terkena Cahaya." },
  { "en": "Apa Itu Mikroskop?", "id": "Alat Untuk Melihat Objek Sangat Kecil." },
  { "en": "Apa Itu Lensa Objektif?", "id": "Lensa Dekat Dengan Objek." },
  { "en": "Apa Itu Lensa Okuler?", "id": "Lensa Dekat Dengan Mata." },
  { "en": "Apa Itu Teleskop?", "id": "Alat Untuk Melihat Objek Sangat Jauh." },
  { "en": "Apa Itu Teleskop Refraktor?", "id": "Menggunakan Lensa Untuk Mengumpulkan Cahaya." },
  { "en": "Apa Itu Teleskop Reflektor?", "id": "Menggunakan Cermin Untuk Mengumpulkan Cahaya." },
  { "en": "Apa Itu Hamburan Rayleigh?", "id": "Hamburan Cahaya Oleh Partikel Sangat Kecil." },
  { "en": "Apakah Listrik Dan Magnet Terpisah?", "id": "Tidak, Keduanya Adalah Aspek Dari Satu Fenomena." },
  { "en": "Apa Nama Fenomena Gabungan Itu?", "id": "Elektromagnetisme." },
  { "en": "Siapa Yang Pertama Menemukan Hubungannya?", "id": "Hans Christian Oersted." },
  { "en": "Apa Penemuan Oersted?", "id": "Arus Listrik Menghasilkan Medan Magnet." },
  { "en": "Arah Medan Magnet Sekitar Arus?", "id": "Mengikuti Aturan Genggaman Tangan Kanan." },
  { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Arus Dengan Sirkulasi Medan Magnet." },
  { "en": "Apa Itu Induksi Elektromagnetik?", "id": "Medan Magnet Berubah Menghasilkan Arus Listrik." },
  { "en": "Siapa Penemu Induksi Elektromagnetik?", "id": "Michael Faraday." },
  { "en": "Apa Itu Hukum Faraday?", "id": "Besar GGL Induksi Sebanding Perubahan Fluks." },
  { "en": "Apa Itu Hukum Lenz?", "id": "Menentukan Arah Arus Induksi." },
  { "en": "Bagaimana Arah Arus Induksi?", "id": "Melawan Perubahan Fluks Yang Menghasilkannya." },
  { "en": "Prinsip Dasar Generator?", "id": "Hukum Induksi Faraday." },
  { "en": "Prinsip Dasar Motor?", "id": "Gaya Lorentz Pada Kawat Berarus." },
  { "en": "Prinsip Dasar Transformator?", "id": "Induktansi Timbal Balik." },
  { "en": "Apa Itu Persamaan Maxwell?", "id": "Rangkuman Matematis Lengkap Dari Elektromagnetisme." },
  { "en": "Apa Itu Arus Pergeseran Maxwell?", "id": "Medan Listrik Berubah Bertindak Seperti Arus." },
  { "en": "Mengapa Arus Pergeseran Penting?", "id": "Menjelaskan Perambatan Gelombang Elektromagnetik." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Kombinasi Medan Listrik Dan Magnet Merambat." },
  { "en": "Apakah Medan E Dan B Sefasa?", "id": "Ya, Dalam Gelombang EM Di Ruang Hampa." },
  { "en": "Apa Itu Gaya Lorentz?", "id": "Gaya Total Pada Muatan (Listrik Magnet)." },
  { "en": "Bagian Listrik Gaya Lorentz?", "id": "F = qE." },
  { "en": "Bagian Magnetik Gaya Lorentz?", "id": "F = q(v × B)." },
  { "en": "Apa Itu Elektrodinamika?", "id": "Studi Tentang Medan Elektromagnetik Yang Berubah Waktu." },
  { "en": "Apa Itu Potensial Vektor Magnetik (A)?", "id": "Potensial Yang Menghasilkan Medan Magnet." },
  { "en": "Apa Itu Medan Listrik Terinduksi?", "id": "Medan Listrik Yang Dihasilkan Perubahan Medan B." },
  { "en": "Apakah Medan Listrik Terinduksi Konservatif?", "id": "Tidak, Curl-nya Tidak Nol." },
  { "en": "Sirkulasi Medan Listrik Terinduksi?", "id": "Tidak Nol, Sama Dengan Laju Perubahan Fluks." },
  { "en": "Apa Itu Induktansi?", "id": "Sifat Rangkaian Yang Melawan Perubahan Arus." },
  { "en": "Apa Itu Efek Hall?", "id": "Tegangan Timbul Akibat Gaya Magnet." },
  { "en": "Apa Itu Siklotron?", "id": "Menggunakan Medan Magnet Untuk Membelokkan Partikel." },
  { "en": "Energi Dalam Medan Listrik?", "id": "Tersimpan Dalam Kapasitor." },
  { "en": "Energi Dalam Medan Magnet?", "id": "Tersimpan Dalam Induktor." },
  { "en": "Apa Itu Rangkaian Osilasi LC?", "id": "Energi Berpindah Antara Medan E Dan B." },
  { "en": "Apa Itu Vektor Poynting?", "id": "Menunjukkan Aliran Energi Elektromagnetik." },
  { "en": "Kapan Medan Listrik Menghasilkan Medan Magnet?", "id": "Saat Medan Listrik Berubah Terhadap Waktu." },
  { "en": "Kapan Medan Magnet Menghasilkan Medan Listrik?", "id": "Saat Medan Magnet Berubah Terhadap Waktu." },
  { "en": "Apa Itu Saling Ketergantungan E Dan B?", "id": "Inti Dari Perambatan Gelombang Elektromagnetik." },
  { "en": "Apa Itu Relativitas Khusus?", "id": "Menunjukkan Medan E Dan B Adalah Relatif." },
  { "en": "Bagaimana Pengamat Berbeda Melihat Medan?", "id": "Satu Mungkin Melihat Medan E, Lainnya Medan B." },
  { "en": "Apa Itu Medan Elektromagnetik Tensor?", "id": "Objek Tunggal Yang Menggabungkan E Dan B." },
  { "en": "Apa Itu Efek Aharonov-Bohm?", "id": "Efek Kuantum Dari Potensial Elektromagnetik." },
  { "en": "Apakah Potensial Memiliki Realitas Fisik?", "id": "Ya, Menurut Mekanika Kuantum." },
  { "en": "Apa Itu Dualitas Listrik-Magnet?", "id": "Simetri Dalam Persamaan Maxwell." },
  { "en": "Apa Itu Muatan Magnetik (Monopol)?", "id": "Partikel Hipotetis, Sumber Medan Magnet." },
  { "en": "Apa Implikasi Jika Monopol Ada?", "id": "Akan Membuat Persamaan Maxwell Simetris." },
  { "en": "Apa Itu Elektromagnetisme Klasik?", "id": "Teori Yang Dijelaskan Oleh Persamaan Maxwell." },
  { "en": "Apa Itu Elektrodinamika Kuantum (QED)?", "id": "Teori Kuantum Tentang Interaksi Elektromagnetik." },
  { "en": "Apa Itu Foton?", "id": "Partikel Pembawa Interaksi Elektromagnetik." },
  { "en": "Apa Itu Gaya Elektromagnetik?", "id": "Salah Satu Dari Empat Gaya Fundamental Alam." },
  { "en": "Apa Itu Teori Elektrolemah?", "id": "Menyatukan Gaya Elektromagnetik Dan Gaya Lemah." },
  { "en": "Apakah Medan Magnet Dapat Melakukan Kerja?", "id": "Tidak, Karena Gayanya Selalu Tegak Lurus." },
  { "en": "Apakah Medan Listrik Dapat Melakukan Kerja?", "id": "Ya, Dapat Mempercepat Muatan." },
  { "en": "Sumber Medan Elektrostatik?", "id": "Muatan Diam." },
  { "en": "Sumber Medan Magnetostatik?", "id": "Arus Konstan." },
  { "en": "Sumber Radiasi Elektromagnetik?", "id": "Muatan Yang Dipercepat." },
  { "en": "Apa Itu Arus Konduksi?", "id": "Aliran Fisik Dari Pembawa Muatan." },
  { "en": "Apa Itu Arus Pergeseran?", "id": "Efek Dari Perubahan Medan Listrik." },
  { "en": "Di Mana Arus Pergeseran Penting?", "id": "Di Dalam Kapasitor, Di Ruang Hampa." },
  { "en": "Bagaimana Radio Bekerja?", "id": "Mengirim Dan Menerima Gelombang Elektromagnetik." },
  { "en": "Bagaimana MRI Bekerja?", "id": "Menggunakan Medan Magnet Kuat Dan Gelombang Radio." },
  { "en": "Bagaimana Kompas Bekerja?", "id": "Menyejajarkan Diri Dengan Medan Magnet Bumi." },
  { "en": "Mengapa Langit Berwarna Biru?", "id": "Hamburan Elektromagnetik Cahaya Matahari." },
  { "en": "Apa Itu Efek Zeeman?", "id": "Pemisahan Garis Spektrum Akibat Medan Magnet." },
  { "en": "Apa Itu Efek Stark?", "id": "Pemisahan Garis Spektrum Akibat Medan Listrik." },
  { "en": "Apa Itu Gaya Elektromotif (GGL)?", "id": "Sumber Tegangan Dalam Rangkaian." },
  { "en": "Apa Itu GGL Induksi?", "id": "GGL Yang Dihasilkan Oleh Perubahan Fluks." },
  { "en": "Apa Itu GGL Gerak?", "id": "GGL Pada Konduktor Bergerak Dalam Medan B." },
  { "en": "Apa Itu Back EMF?", "id": "GGL Induksi Yang Melawan Arus Pada Motor." },
  { "en": "Bagaimana Cara Menginduksi Arus?", "id": "Mengubah Fluks Magnetik Melalui Loop." },
  { "en": "Cara Mengubah Fluks Magnetik?", "id": "Mengubah Medan B, Luas A, Atau Sudut." },
  { "en": "Apa Itu Elektrolisis?", "id": "Menggunakan Arus Listrik Untuk Memicu Reaksi Kimia." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Hubungan Langsung Antara Panas Dan Listrik." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Hubungan Langsung Antara Tekanan Dan Listrik." },
  { "en": "Apa Itu Radiasi?", "id": "Emisi Energi Sebagai Gelombang Elektromagnetik." },
  { "en": "Apa Itu Potensial Retarded?", "id": "Potensial Yang Memperhitungkan Waktu Perambatan." },
  { "en": "Mengapa Potensial Retarded Penting?", "id": "Karena Interaksi Elektromagnetik Butuh Waktu." },
  { "en": "Apa Itu Radiasi Dipol Listrik?", "id": "Radiasi Yang Dihasilkan Oleh Dipol Berosilasi." },
  { "en": "Apa Itu Solenoida?", "id": "Kumparan Untuk Menghasilkan Medan Magnet Seragam." },
  { "en": "Apa Itu Kapasitor Pelat Sejajar?", "id": "Komponen Untuk Menghasilkan Medan Listrik Seragam." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Menggunakan Prinsip Listrik Pada Skala Mikro." },
  { "en": "Apa Itu Pemandu Gelombang?", "id": "Struktur Untuk Membatasi Perambatan Gelombang EM." },
  { "en": "Apa Itu Serat Optik?", "id": "Pemandu Gelombang Untuk Cahaya." },
  { "en": "Apa Itu Resonansi?", "id": "Saat Sistem Didorong Pada Frekuensi Naturalnya." },
  { "en": "Apa Itu Radiasi Benda Hitam?", "id": "Radiasi EM Yang Dipancarkan Benda Termal." },
  { "en": "Apa Beda Medan Dekat Dan Medan Jauh?", "id": "Struktur Medan Berbeda Dekat Sumber Radiasi." },
  { "en": "Apa Itu Momentum Sudut Elektromagnetik?", "id": "Medan EM Dapat Membawa Momentum Sudut." },
  { "en": "Apa Itu Teorema Poynting?", "id": "Menyatakan Kekekalan Energi Dalam Medan EM." },
  { "en": "Apa Itu Gaya Magnetik Pada Dipol?", "id": "Tergantung Pada Gradien Dari Medan Magnet." },
  { "en": "Apa Itu Levitation Diamagnetik?", "id": "Bahan Diamagnetik Dapat Melayang Di Medan Kuat." },
  { "en": "Apa Itu Kondisi Batas Unik?", "id": "Solusi Persamaan Maxwell Unik Dengan Kondisi Batas." },
  { "en": "Apa Itu Invariansi Kalibrasi (Gauge Invariance)?", "id": "Fisika Tidak Berubah Oleh Pilihan Kalibrasi." },
  { "en": "Apa Itu Persamaan Gelombang Inhomogen?", "id": "Persamaan Gelombang Dengan Sumber (Muatan Dan Arus)." },
  { "en": "Apa Itu Sirkuit Listrik?", "id": "Jalur Tertutup Bagi Aliran Arus." },
  { "en": "Apa Itu Elemen Pasif?", "id": "Elemen Yang Tidak Dapat Menghasilkan Energi." },
  { "en": "Contoh Elemen Pasif?", "id": "Resistor, Kapasitor, Induktor." },
  { "en": "Apa Itu Elemen Aktif?", "id": "Elemen Yang Dapat Menyuplai Energi." },
  { "en": "Contoh Elemen Aktif?", "id": "Baterai, Generator, Sumber Tegangan." },
  { "en": "Apa Itu Analisis Sirkuit?", "id": "Menentukan Tegangan Dan Arus Dalam Sirkuit." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Di Simpul Adalah Nol." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Di Loop Adalah Nol." },
  { "en": "Apa Itu Analisis Simpul (Nodal)?", "id": "Metode Analisis Berbasis KCL." },
  { "en": "Apa Itu Analisis Mesh (Loop)?", "id": "Metode Analisis Berbasis KVL." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Efek Total Adalah Jumlah Efek Individual." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Sirkuit Menjadi Sumber Tegangan Ekuivalen." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Sirkuit Menjadi Sumber Arus Ekuivalen." },
  { "en": "Apa Itu Transformasi Sumber?", "id": "Mengubah Antara Model Thevenin Dan Norton." },
  { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Beban Harus Sama Dengan Resistansi Sumber." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Diferensial Dengan Gain Sangat Tinggi." },
  { "en": "Apa Itu Op-Amp Ideal?", "id": "Gain Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Konfigurasi Inverting Amplifier?", "id": "Membalik Fasa Dan Menguatkan Sinyal." },
  { "en": "Konfigurasi Non-Inverting Amplifier?", "id": "Menguatkan Sinyal Tanpa Membalik Fasa." },
  { "en": "Apa Itu Filter?", "id": "Sirkuit Yang Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Yang Menggunakan Elemen Aktif (Op-Amp)." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter Yang Hanya Menggunakan R, L, C." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Bagaimana Sirkuit Merespon Terhadap Frekuensi Berbeda." },
  { "en": "Apa Itu Osilator?", "id": "Sirkuit Yang Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Sistem Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu Sistem Tiga Fasa?", "id": "Metode Efisien Untuk Transmisi Daya AC." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Impedansi Sama Di Ketiga Fasa." },
  { "en": "Apa Itu Daya Rata-rata Tiga Fasa?", "id": "Konstan, Tidak Berdenyut Seperti Satu Fasa." },
  { "en": "Apa Itu Motor Induksi?", "id": "Motor AC Paling Umum Di Industri." },
  { "en": "Apa Itu Slip Motor Induksi?", "id": "Perbedaan Kecepatan Antara Rotor Dan Medan." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Pembangkit Utama Di Pembangkit Listrik." },
  { "en": "Apa Itu Sistem Komunikasi?", "id": "Transmisi Informasi Dari Satu Titik Ke Titik Lain." },
  { "en": "Apa Itu Modulasi?", "id": "Mengubah Sinyal Informasi Agar Sesuai Kanal." },
  { "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Sinyal Informasi Kembali." },
  { "en": "Apa Itu Sistem Kontrol?", "id": "Sistem Untuk Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Output Untuk Mempengaruhi Input." },
  { "en": "Apa Itu Sistem Loop Terbuka?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor?", "id": "Mengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator?", "id": "Mengubah Sinyal Kontrol Menjadi Aksi Fisik." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Industri Untuk Otomasi." },
  { "en": "Apa Itu Logika Digital?", "id": "Sistem Berbasis Nilai Biner (0 Dan 1)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Contoh Gerbang Logika?", "id": "AND, OR, NOT." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Matematika Untuk Menganalisis Sirkuit Logika." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Dasar Dalam Sirkuit Digital." },
  { "en": "Apa Itu Mikroprosesor?", "id": "Unit Pemroses Pusat (CPU) Pada Satu Chip." },
  { "en": "Apa Itu Instrumen Pengukuran?", "id": "Alat Untuk Mengukur Besaran Listrik." },
  { "en": "Apa Itu Voltmeter?", "id": "Mengukur Tegangan." },
  { "en": "Apa Itu Amperemeter?", "id": "Mengukur Arus." },
  { "en": "Apa Itu Ohmmeter?", "id": "Mengukur Resistansi." },
  { "en": "Apa Itu Osiloskop?", "id": "Menampilkan Bentuk Gelombang Tegangan." },
  { "en": "Apa Itu Analisis Rangkaian Transien?", "id": "Analisis Perilaku Rangkaian Saat Berubah." },
  { "en": "Apa Itu Keadaan Tunak (Steady State)?", "id": "Kondisi Sirkuit Setelah Waktu Lama." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Penggunaan Perangkat Semikonduktor Untuk Kontrol Daya." },
  { "en": "Apa Itu Penyearah Terkendali?", "id": "Dapat Mengontrol Tegangan Output DC." },
  { "en": "Apa Itu Inverter PWM?", "id": "Menghasilkan AC Dengan Mengontrol Lebar Pulsa." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Khusus Dalam Perangkat." },
  { "en": "Apa Itu DSP (Digital Signal Processing)?", "id": "Pemrosesan Sinyal Menggunakan Komputer Digital." },
  { "en": "Apa Itu Pemrosesan Sinyal Analog?", "id": "Pemrosesan Sinyal Menggunakan Sirkuit Analog." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Kuantifikasi, Penyimpanan, Komunikasi Informasi." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakpastian Atau Informasi." },
  { "en": "Apa Itu Kapasitas Kanal?", "id": "Laju Informasi Maksimum Melalui Kanal." },
  { "en": "Apa Itu Pengkodean (Coding)?", "id": "Mengubah Informasi Ke Bentuk Lain." },
  { "en": "Apa Itu Kode Koreksi Error?", "id": "Menambahkan Redundansi Untuk Memperbaiki Kesalahan." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Kumpulan Komputer Yang Saling Terhubung." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena TV Arah Yang Umum." },
  { "en": "Apa Itu Antena Parabola?", "id": "Antena Gain Tinggi Untuk Komunikasi Jarak Jauh." },
  { "en": "Apa Itu Pemandu Gelombang Dielektrik?", "id": "Seperti Serat Optik." },
  { "en": "Apa Itu Radiometer Microwave?", "id": "Mengukur Radiasi Termal Gelombang Mikro." },
  { "en": "Apa Itu Sistem Navigasi Inersia?", "id": "Menggunakan Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Sistem Mekatronika?", "id": "Sinergi Mekanik, Elektronik, Dan Perangkat Lunak." },
  { "en": "Apa Itu Robotika?", "id": "Ilmu Desain Dan Penggunaan Robot." },
  { "en": "Apa Itu Sistem Kontrol Optimal?", "id": "Mendesain Kontroler Untuk Meminimalkan Biaya." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem Dengan Noise." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Komputasi Terinspirasi Otak." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Yang Berurusan Dengan 'Kebenaran Sebagian'." },
  { "en": "Apa Itu Bahan Pintar (Smart Material)?", "id": "Material Yang Sifatnya Dapat Dikontrol." },
  { "en": "Contoh Bahan Pintar?", "id": "Piezoelektrik, Shape Memory Alloy." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanik Dan Elektronik Skala Mikro." },
  { "en": "Aplikasi MEMS?", "id": "Akselerometer Di Ponsel, Sensor Tekanan." },
  { "en": "Apa Itu Fotonik?", "id": "Teknologi Yang Melibatkan Foton (Cahaya)." },
  { "en": "Apa Itu Optoelektronik?", "id": "Perangkat Yang Mengubah Listrik Menjadi Cahaya." },
  { "en": "Contoh Optoelektronik?", "id": "LED, Laser, Sel Surya." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Tanpa Interferensi." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Yang Dihasilkan Oleh Perangkat Elektronik." },
  { "en": "Apa Itu Pelindung (Shielding)?", "id": "Menggunakan Konduktor Untuk Memblokir Medan Elektromagnetik." },
  { "en": "Apa Itu Rekayasa Biomedis?", "id": "Aplikasi Teknik Di Bidang Kedokteran." },
  { "en": "Apa Itu Sel Bahan Bakar?", "id": "Menghasilkan Listrik Dari Reaksi Kimia." },
  { "en": "Apa Itu Superkapasitor?", "id": "Kapasitor Dengan Kapasitansi Sangat Tinggi." },
  { "en": "Apa Itu Sistem Hibrida?", "id": "Sistem Yang Menggabungkan Beberapa Teknologi." },
  { "en": "Apa Itu Termodinamika?", "id": "Studi Tentang Panas Dan Energi." },
  { "en": "Apa Itu Mekanika Fluida?", "id": "Studi Tentang Cairan Dan Gas." },
  { "en": "Apa Itu Teori Rangkaian?", "id": "Dasar Untuk Menganalisis Sirkuit Listrik." },
  { "en": "Apa Itu Teori Medan?", "id": "Dasar Untuk Memahami Elektromagnetisme." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel?", "id": "Cahaya Dan Materi Memiliki Sifat Keduanya." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Demonstrasi Sifat Partikel Cahaya (Foton)." },
  { "en": "Apa Itu Efek Compton?", "id": "Hamburan Foton Oleh Elektron." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Batas Fundamental Akurasi Pengukuran Kuantum." },
  { "en": "Apa Itu Mekanika Kuantum?", "id": "Teori Fisika Untuk Skala Atomik." },
  { "en": "Apa Itu Fungsi Gelombang?", "id": "Deskripsi Matematis Dari Keadaan Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger?", "id": "Mengatur Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Terowongan Kuantum (Quantum Tunneling)?", "id": "Partikel Menembus Penghalang Energi." },
  { "en": "Aplikasi Terowongan Kuantum?", "id": "Dioda Tunnel, Scanning Tunneling Microscope." },
  { "en": "Apa Itu Keterkaitan Kuantum (Entanglement)?", "id": "Koneksi Non-Lokal Antara Partikel Kuantum." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Komputasi Yang Memanfaatkan Fenomena Kuantum." },
  { "en": "Apa Itu Qubit?", "id": "Unit Informasi Kuantum." },
  { "en": "Apa Itu Superposisi Kuantum?", "id": "Qubit Dapat Berada Di Banyak Keadaan." },
  { "en": "Apa Itu Kriptografi Kuantum?", "id": "Metode Komunikasi Aman Berbasis Kuantum." },
  { "en": "Apa Itu Relativitas Umum?", "id": "Teori Gravitasi Einstein." },
  { "en": "Apa Itu Lensa Gravitasi?", "id": "Pembelokan Cahaya Oleh Gravitasi Benda Masif." },
  { "en": "Apa Itu Gelombang Gravitasi?", "id": "Riak Dalam Jalinan Ruang-Waktu." },
  { "en": "Apa Itu Radiasi Kosmik Latar Belakang?", "id": "Sisa Cahaya Dari Big Bang." },
  { "en": "Apa Itu Materi Gelap?", "id": "Materi Tak Terlihat Yang Mendominasi Alam Semesta." },
  { "en": "Apa Itu Energi Gelap?", "id": "Penyebab Percepatan Ekspansi Alam Semesta." },
  { "en": "Apa Itu Plasma?", "id": "Keadaan Materi Keempat, Gas Terionisasi." },
  { "en": "Apa Itu Fusi Nuklir?", "id": "Penggabungan Inti Atom, Sumber Energi Bintang." },
  { "en": "Apa Itu Tokamak?", "id": "Perangkat Untuk Riset Fusi Magnetik." },
  { "en": "Apa Itu Magnetohidrodinamika (MHD)?", "id": "Studi Tentang Dinamika Plasma Magnetik." },
  { "en": "Apa Itu Angin Matahari?", "id": "Aliran Plasma Dari Matahari." },
  { "en": "Apa Itu Magnetosfer?", "id": "Wilayah Yang Didominasi Medan Magnet Planet." },
  { "en": "Apa Itu Badai Geomagnetik?", "id": "Gangguan Besar Pada Magnetosfer Bumi." },
  { "en": "Apa Itu Sinar Kosmik?", "id": "Partikel Berenergi Tinggi Dari Luar Angkasa." },
  { "en": "Apa Itu Pulsar?", "id": "Bintang Neutron Berputar Yang Memancarkan Radiasi." },
  { "en": "Apa Itu Lubang Hitam?", "id": "Objek Dengan Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Cakrawala Peristiwa (Event Horizon)?", "id": "Batas Tanpa Jalan Kembali Dari Lubang Hitam." },
  { "en": "Apa Itu Radiasi Hawking?", "id": "Radiasi Termal Yang Dipancarkan Lubang Hitam." },
  { "en": "Apa Itu Spintronik?", "id": "Elektronik Yang Memanfaatkan Spin Elektron." },
  { "en": "Apa Itu Efek Kuantum Hall?", "id": "Versi Kuantum Dari Efek Hall." },
  { "en": "Apa Itu Efek Josephson?", "id": "Arus Superkonduktor Mengalir Melintasi Isolator Tipis." },
  { "en": "Aplikasi Efek Josephson?", "id": "SQUID (Sensor Magnetik)." },
  { "en": "Apa Itu Fonon?", "id": "Kuantum Dari Getaran Kisi Kristal." },
  { "en": "Apa Itu Efek Casimir?", "id": "Gaya Tarik Antara Dua Pelat Akibat Fluktuasi." },
  { "en": "Apa Itu Anomali?", "id": "Pelanggaran Simetri Klasik Dalam Teori Kuantum." },
  { "en": "Apa Itu Teori String?", "id": "Model Fisika Dimana Partikel Adalah Getaran." },
  { "en": "Apa Itu Teori Medan Terpadu?", "id": "Upaya Untuk Menyatukan Semua Gaya Fundamental." },
  { "en": "Apa Itu Kosmologi?", "id": "Studi Tentang Asal Dan Evolusi Alam Semesta." },
  { "en": "Apa Itu Inflasi Kosmik?", "id": "Ekspansi Sangat Cepat Di Awal Alam Semesta." },
  { "en": "Apa Itu Fraktal?", "id": "Pola Geometris Yang Berulang Pada Semua Skala." },
  { "en": "Apa Itu Teori Chaos?", "id": "Studi Sistem Dinamis Yang Sangat Sensitif." },
  { "en": "Apa Itu Strange Attractor?", "id": "Struktur Geometris Dalam Sistem Chaos." },
  { "en": "Apa Itu Nonlinearitas?", "id": "Sistem Dimana Output Tidak Sebanding Input." },
  { "en": "Apa Itu Soliton?", "id": "Gelombang Tunggal Yang Menjaga Bentuknya." },
  { "en": "Apa Itu Kondensat Bose-Einstein?", "id": "Keadaan Materi Pada Suhu Sangat Dingin." },
  { "en": "Apa Itu Cairan Super (Superfluid)?", "id": "Cairan Dengan Viskositas Nol." },
  { "en": "Apa Itu Fisika Statistik?", "id": "Menerapkan Statistik Pada Sistem Fisik Besar." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Atau Informasi." },
  { "en": "Hukum Kedua Termodinamika?", "id": "Entropi Total Alam Semesta Selalu Meningkat." },
  { "en": "Apa Itu Panah Waktu?", "id": "Arah Asimetris Waktu." },
  { "en": "Apa Itu Simetri Dalam Fisika?", "id": "Invariansi Sistem Terhadap Transformasi Tertentu." },
  { "en": "Apa Itu Teorema Noether?", "id": "Setiap Simetri Berhubungan Dengan Hukum Kekekalan." },
  { "en": "Contoh Teorema Noether?", "id": "Simetri Waktu → Kekekalan Energi." },
  { "en": "Apa Itu Pelanggaran Paritas?", "id": "Interaksi Lemah Tidak Simetris Terhadap Pencerminan." },
  { "en": "Apa Itu Antimateri?", "id": "Materi Dengan Muatan Yang Berlawanan." },
  { "en": "Apa Itu Anihilasi?", "id": "Pertemuan Materi Dan Antimateri Menjadi Energi." },
  { "en": "Apa Itu Model Standar Fisika Partikel?", "id": "Teori Yang Mendeskripsikan Partikel Fundamental." },
  { "en": "Apa Itu Boson Higgs?", "id": "Partikel Yang Memberikan Massa Pada Partikel Lain." },
  { "en": "Apa Itu Large Hadron Collider (LHC)?", "id": "Akselerator Partikel Terbesar Di Dunia." },
  { "en": "Apa Itu Teori Medan Kuantum?", "id": "Kerangka Teori Untuk Fisika Partikel." },
  { "en": "Apa Itu Diagram Feynman?", "id": "Representasi Grafis Dari Interaksi Partikel." },
  { "en": "Apa Itu Renormalisasi?", "id": "Teknik Mengatasi Ketakhinggaan Dalam Perhitungan." },
  { "en": "Apa Itu Teori Medan Konformal?", "id": "Teori Kuantum Dengan Simetri Skala." },
  { "en": "Apa Itu Fisika Benda Terkondensasi?", "id": "Studi Sifat Makroskopik Materi." },
  { "en": "Apa Itu Kristal Cair?", "id": "Keadaan Materi Antara Cair Dan Padat." },
  { "en": "Aplikasi Kristal Cair?", "id": "Layar LCD." },
  { "en": "Apa Itu Polimer?", "id": "Molekul Rantai Panjang." },
  { "en": "Apa Itu Fisika Atmosfer?", "id": "Studi Fisika Dari Atmosfer Bumi." },
  { "en": "Apa Itu Ionosfer?", "id": "Lapisan Atmosfer Yang Terionisasi." },
  { "en": "Pentingnya Ionosfer?", "id": "Memantulkan Gelombang Radio." },
  { "en": "Apa Itu Geofisika?", "id": "Studi Fisika Bumi." },
  { "en": "Apa Itu Seismologi?", "id": "Studi Tentang Gempa Bumi." },
  { "en": "Apa Itu Biofisika?", "id": "Aplikasi Fisika Pada Sistem Biologis." },
  { "en": "Apa Itu Bioelektromagnetisme?", "id": "Studi Medan Listrik Dan Magnet Dalam Biologi." },
  { "en": "Contoh Bioelektromagnetisme?", "id": "Sinyal Saraf, Sinyal Jantung (EKG)." },
  { "en": "Apa Itu Ekonofisika?", "id": "Aplikasi Metode Fisika Pada Masalah Ekonomi." },
  { "en": "Apa Itu Fisika Komputasi?", "id": "Menggunakan Komputer Untuk Menyelesaikan Masalah Fisika." },
  { "en": "Apa Itu Teori Kompleksitas?", "id": "Studi Tentang Sistem Kompleks." },
  { "en": "Apa Itu Jaringan Kompleks?", "id": "Studi Struktur Dan Dinamika Jaringan." },
  { "en": "Apa Itu Emergence?", "id": "Munculnya Perilaku Kompleks Dari Aturan Sederhana." },
  { "en": "Apa Itu Self-Organization?", "id": "Proses Dimana Keteraturan Muncul Secara Spontan." },
  { "en": "Apa Itu Criticality?", "id": "Titik Kritis Antara Keteraturan Dan Kekacauan." },
  { "en": "Apa Itu Invariansi Skala?", "id": "Sistem Terlihat Sama Pada Berbagai Skala." },
  { "en": "Apa Itu Grup Renormalisasi?", "id": "Teknik Matematis Untuk Memahami Invariansi Skala." },
  { "en": "Apa Itu Fase Materi?", "id": "Keadaan Berbeda Dari Materi (Padat, Cair)." },
  { "en": "Apa Itu Transisi Fase?", "id": "Perubahan Antara Fase-fase Materi." },
  { "en": "Contoh Transisi Fase?", "id": "Mencair, Membeku, Menguap." },
  { "en": "Apa Itu Fenomena Kritis?", "id": "Perilaku Universal Di Dekat Titik Transisi Fase." },
  { "en": "Apa Itu Model Ising?", "id": "Model Matematis Sederhana Untuk Ferromagnetisme." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
