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


{ "en": "Apa Itu Komputer?", "id": "Alat Elektronik Pengolah Data." },
{ "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Utama Dari Komputer." },
{ "en": "Apa Fungsi RAM (Random Access Memory)?", "id": "Menyimpan Data Sementara Saat Aktif." },
{ "en": "Apa Itu ROM (Read-Only Memory)?", "id": "Menyimpan Instruksi Startup Permanen." },
{ "en": "Apa Itu Perangkat Keras?", "id": "Komponen Fisik Dari Komputer." },
{ "en": "Apa Itu Perangkat Lunak?", "id": "Kumpulan Instruksi Dan Data." },
{ "en": "Sebutkan Contoh Perangkat Keras?", "id": "Mouse, Keyboard, Dan Monitor." },
{ "en": "Sebutkan Contoh Perangkat Lunak?", "id": "Sistem Operasi Dan Aplikasi." },
{ "en": "Apa Itu Sistem Operasi?", "id": "Perangkat Lunak Pengelola Sumber Daya." },
{ "en": "Sebutkan Contoh Sistem Operasi?", "id": "Windows, MacOS, Dan Linux." },
{ "en": "Apa Itu Motherboard?", "id": "Papan Sirkuit Utama Komputer." },
{ "en": "Apa Fungsi Hard Drive?", "id": "Menyimpan Data Jangka Panjang." },
{ "en": "Apa Itu SSD (Solid State Drive)?", "id": "Penyimpanan Data Tanpa Bagian Bergerak." },
{ "en": "Apa Itu Input Device?", "id": "Perangkat Untuk Memasukkan Data." },
{ "en": "Apa Itu Output Device?", "id": "Perangkat Untuk Menampilkan Hasil." },
{ "en": "Apa Fungsi Keyboard?", "id": "Memasukkan Teks Dan Perintah." },
{ "en": "Apa Fungsi Mouse?", "id": "Menggerakkan Kursor Pada Layar." },
{ "en": "Apa Itu Monitor?", "id": "Perangkat Output Penampil Visual." },
{ "en": "Apa Fungsi Printer?", "id": "Mencetak Dokumen Ke Kertas." },
{ "en": "Apa Itu Internet?", "id": "Jaringan Komputer Global Terhubung." },
{ "en": "Apa Itu WWW (World Wide Web)?", "id": "Sistem Halaman Web Terhubung." },
{ "en": "Apa Itu Browser Web?", "id": "Perangkat Lunak Akses World Wide Web." },
{ "en": "Apa Itu URL (Uniform Resource Locator)?", "id": "Alamat Unik Sumber Daya Web." },
{ "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Komunikasi Data Web." },
{ "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman Dari Protokol HTTP." },
{ "en": "Apa Itu Email?", "id": "Surat Elektronik Dikirim Via Internet." },
{ "en": "Apa Itu ISP (Internet Service Provider)?", "id": "Perusahaan Penyedia Layanan Internet." },
{ "en": "Apa Itu Jaringan Komputer?", "id": "Dua Atau Lebih Komputer Terhubung." },
{ "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Komputer Area Lokal." },
{ "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Komputer Area Luas." },
{ "en": "Apa Itu Router?", "id": "Perangkat Penghubung Antar Jaringan." },
{ "en": "Apa Itu Modem?", "id": "Mengubah Sinyal Digital Ke Analog." },
{ "en": "Apa Itu Wi-Fi?", "id": "Teknologi Jaringan Nirkabel Lokal." },
{ "en": "Apa Itu Ethernet?", "id": "Teknologi Jaringan Kabel Standar." },
{ "en": "Apa Itu IP Address (Internet Protocol Address)?", "id": "Alamat Unik Perangkat Di Jaringan." },
{ "en": "Apa Itu DNS (Domain Name System)?", "id": "Mengubah Nama Domain Menjadi IP Address." },
{ "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Penyaring Lalu Lintas." },
{ "en": "Apa Itu Virus Komputer?", "id": "Program Jahat Pengganda Diri." },
{ "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya Umum." },
{ "en": "Apa Itu Antivirus?", "id": "Perangkat Lunak Pendeteksi Virus." },
{ "en": "Apa Itu Data?", "id": "Fakta Mentah Belum Diolah." },
{ "en": "Apa Itu Informasi?", "id": "Data Yang Sudah Diolah." },
{ "en": "Apa Itu Database?", "id": "Kumpulan Data Terstruktur Rapi." },
{ "en": "Apa Itu DBMS (Database Management System)?", "id": "Sistem Pengelola Database Terpusat." },
{ "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Untuk Database." },
{ "en": "Apa Itu Bit?", "id": "Unit Data Terkecil Komputer." },
{ "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit Data." },
{ "en": "Berapa Bit Dalam Satu Byte?", "id": "Delapan Bit." },
{ "en": "Apa Itu Kilobyte (KB)?", "id": "Sekitar Seribu Byte Data." },
{ "en": "Apa Itu Megabyte (MB)?", "id": "Sekitar Satu Juta Byte." },
{ "en": "Apa Itu Gigabyte (GB)?", "id": "Sekitar Satu Miliar Byte." },
{ "en": "Apa Itu Terabyte (TB)?", "id": "Sekitar Satu Triliun Byte." },
{ "en": "Apa Itu Algoritma?", "id": "Langkah-Langkah Logis Selesaikan Masalah." },
{ "en": "Apa Itu Pemrograman?", "id": "Proses Menulis Instruksi Komputer." },
{ "en": "Apa Itu Bahasa Pemrograman?", "id": "Bahasa Komunikasi Dengan Komputer." },
{ "en": "Sebutkan Contoh Bahasa Pemrograman?", "id": "Python, Java, Dan C++." },
{ "en": "Apa Itu Source Code?", "id": "Kode Program Ditulis Manusia." },
{ "en": "Apa Itu Compiler?", "id": "Penerjemah Source Code Ke Mesin." },
{ "en": "Apa Itu Debugging?", "id": "Proses Mencari Dan Memperbaiki Error." },
{ "en": "Apa Itu Variabel?", "id": "Tempat Penyimpanan Nilai Data." },
{ "en": "Apa Itu Tipe Data?", "id": "Jenis Nilai Yang Disimpan." },
{ "en": "Sebutkan Contoh Tipe Data?", "id": "Integer, String, Dan Boolean." },
{ "en": "Apa Itu Fungsi (Function)?", "id": "Blok Kode Yang Dapat Digunakan Ulang." },
{ "en": "Apa Itu GUI (Graphical User Interface)?", "id": "Tampilan Grafis Antarmuka Pengguna." },
{ "en": "Apa Itu CLI (Command-Line Interface)?", "id": "Antarmuka Pengguna Berbasis Teks." },
{ "en": "Apa Itu Aplikasi?", "id": "Program Komputer Tugas Tertentu." },
{ "en": "Apa Itu Cloud Computing?", "id": "Layanan Komputasi Melalui Internet." },
{ "en": "Apa Itu Backup?", "id": "Salinan Cadangan Data Penting." },
{ "en": "Apa Itu Restore?", "id": "Proses Mengembalikan Data Backup." },
{ "en": "Apa Itu Download?", "id": "Mengunduh Data Dari Internet." },
{ "en": "Apa Itu Upload?", "id": "Mengunggah Data Ke Internet." },
{ "en": "Apa Itu Software Open Source?", "id": "Perangkat Lunak Kode Terbuka." },
{ "en": "Apa Itu Freeware?", "id": "Perangkat Lunak Gratis Digunakan." },
{ "en": "Apa Itu Shareware?", "id": "Perangkat Lunak Uji Coba Gratis." },
{ "en": "Apa Itu Pixel?", "id": "Titik Terkecil Pada Layar." },
{ "en": "Apa Itu Resolusi Layar?", "id": "Jumlah Total Pixel Layar." },
{ "en": "Apa Itu AI (Artificial Intelligence)?", "id": "Kecerdasan Buatan Pada Mesin." },
{ "en": "Apa Itu Machine Learning?", "id": "Cabang AI, Mesin Belajar." },
{ "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar." },
{ "en": "Apa Itu IoT (Internet of Things)?", "id": "Jaringan Perangkat Fisik Terhubung." },
{ "en": "Apa Itu Folder Atau Direktori?", "id": "Wadah Penyimpanan File Digital." },
{ "en": "Apa Itu File?", "id": "Kumpulan Data Disimpan Komputer." },
{ "en": "Apa Itu Ekstensi File?", "id": "Akhiran Nama File Tentukan Tipe." },
{ "en": "Contoh Ekstensi File Dokumen?", "id": ".Docx, .Pdf, Dan .Txt." },
{ "en": "Contoh Ekstensi File Gambar?", "id": ".Jpg, .Png, Dan .Gif." },
{ "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Standar Koneksi Perangkat Eksternal." },
{ "en": "Apa Itu Port USB?", "id": "Tempat Menghubungkan Kabel USB." },
{ "en": "Apa Itu VGA (Video Graphics Array)?", "id": "Standar Konektor Tampilan Analog." },
{ "en": "Apa Itu HDMI (High-Definition Multimedia Interface)?", "id": "Standar Konektor Tampilan Digital." },
{ "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel Jarak Pendek." },
{ "en": "Apa Itu QR Code?", "id": "Kode Batang Dua Dimensi." },
{ "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual Aman." },
{ "en": "Apa Itu Cookie Web?", "id": "Data Kecil Disimpan Oleh Browser." },
{ "en": "Apa Itu Cache?", "id": "Penyimpanan Sementara Data Akses." },
{ "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Inisialisasi Perangkat Keras." },
{ "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Perangkat Keras." },
{ "en": "Apa Itu Driver Perangkat?", "id": "Perangkat Lunak Penghubung Hardware OS." },
{ "en": "Apa Itu Booting?", "id": "Proses Menghidupkan Komputer Awal." },
{ "en": "Apa Itu Restart?", "id": "Proses Memulai Ulang Komputer." },
{ "en": "Apa Itu Shutdown?", "id": "Proses Mematikan Komputer Sepenuhnya." },
{ "en": "Apa Itu Sleep Mode?", "id": "Mode Hemat Daya, Aktivitas Cepat." },
{ "en": "Apa Itu Hibernate Mode?", "id": "Menyimpan Sesi Ke Disk, Mati." },
{ "en": "Apa Itu Server?", "id": "Komputer Penyedia Layanan Jaringan." },
{ "en": "Apa Itu Klien (Client)?", "id": "Komputer Penerima Layanan Jaringan." },
{ "en": "Apa Itu Jaringan Client-Server?", "id": "Model Jaringan Server Dan Klien." },
{ "en": "Apa Itu Jaringan Peer-to-Peer?", "id": "Setiap Komputer Setara, Berbagi." },
{ "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Fisik Atau Logis Jaringan." },
{ "en": "Apa Itu Topologi Bintang (Star)?", "id": "Semua Terhubung Ke Hub Sentral." },
{ "en": "Apa Itu Topologi Bus?", "id": "Terhubung Ke Satu Kabel Utama." },
{ "en": "Apa Itu Topologi Cincin (Ring)?", "id": "Terhubung Membentuk Lingkaran Tertutup." },
{ "en": "Apa Itu Topologi Mesh?", "id": "Setiap Perangkat Terhubung Satu Sama Lain." },
{ "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Umum Jaringan Ethernet." },
{ "en": "Apa Itu Kabel Fiber Optik?", "id": "Kabel Transmisi Data Cahaya." },
{ "en": "Apa Itu Bandwidth?", "id": "Kapasitas Maksimal Transfer Data." },
{ "en": "Apa Itu Latency?", "id": "Waktu Tunda Pengiriman Data." },
{ "en": "Apa Itu Download Speed?", "id": "Kecepatan Menerima Data Internet." },
{ "en": "Apa Itu Upload Speed?", "id": "Kecepatan Mengirim Data Internet." },
{ "en": "Apa Itu Domain Name?", "id": "Nama Unik Identifikasi Situs Web." },
{ "en": "Apa Itu Top-Level Domain (TLD)?", "id": "Bagian Akhir Nama Domain." },
{ "en": "Contoh Top-Level Domain?", "id": ".Com, .Org, Dan .Net." },
{ "en": "Apa Itu Web Hosting?", "id": "Layanan Penyimpanan Data Situs Web." },
{ "en": "Apa Itu HTML (Hypertext Markup Language)?", "id": "Bahasa Markup Standar Halaman Web." },
{ "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa Mengatur Tampilan Halaman Web." },
{ "en": "Apa Itu JavaScript?", "id": "Bahasa Pemrograman Interaktivitas Web." },
{ "en": "Apa Itu Website Statis?", "id": "Konten Tetap, Tidak Berubah Otomatis." },
{ "en": "Apa Itu Website Dinamis?", "id": "Konten Berubah, Dihasilkan Server." },
{ "en": "Apa Itu CMS (Content Management System)?", "id": "Sistem Pengelola Konten Situs Web." },
{ "en": "Contoh CMS Populer?", "id": "WordPress, Joomla, Dan Drupal." },
{ "en": "Apa Itu E-commerce?", "id": "Perdagangan Elektronik Melalui Internet." },
{ "en": "Apa Itu Enkripsi?", "id": "Proses Mengamankan Data Menjadi Kode." },
{ "en": "Apa Itu Dekripsi?", "id": "Proses Mengembalikan Data Terenkripsi." },
{ "en": "Apa Itu Kunci Enkripsi?", "id": "Informasi Rahasia Untuk Enkripsi/Dekripsi." },
{ "en": "Apa Itu Enkripsi Simetris?", "id": "Satu Kunci Untuk Enkripsi/Dekripsi." },
{ "en": "Apa Itu Enkripsi Asimetris?", "id": "Dua Kunci, Publik Dan Privat." },
{ "en": "Apa Itu Kunci Publik?", "id": "Kunci Dibagikan Untuk Enkripsi." },
{ "en": "Apa Itu Kunci Privat?", "id": "Kunci Rahasia Untuk Dekripsi." },
{ "en": "Apa Itu SSL (Secure Sockets Layer)?", "id": "Protokol Keamanan Koneksi Internet." },
{ "en": "Apa Itu TLS (Transport Layer Security)?", "id": "Penerus SSL, Protokol Keamanan." },
{ "en": "Apa Itu Phishing?", "id": "Penipuan Mencuri Informasi Sensitif." },
{ "en": "Apa Itu Ransomware?", "id": "Malware Mengenkripsi Data, Minta Tebusan." },
{ "en": "Apa Itu Spyware?", "id": "Malware Memata-matai Aktivitas Pengguna." },
{ "en": "Apa Itu Adware?", "id": "Perangkat Lunak Penampil Iklan." },
{ "en": "Apa Itu Trojan Horse?", "id": "Malware Menyamar Program Legal." },
{ "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Metode Keamanan Verifikasi Ganda." },
{ "en": "Apa Itu Backup Cloud?", "id": "Menyimpan Cadangan Data Di Internet." },
{ "en": "Apa Itu Sinkronisasi Data?", "id": "Menjaga Konsistensi Data Antar Perangkat." },
{ "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Aplikasi." },
{ "en": "Apa Itu SDK (Software Development Kit)?", "id": "Kumpulan Alat Pengembangan Perangkat Lunak." },
{ "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Lingkungan Terpadu Pengembangan Kode." },
{ "en": "Apa Itu Plugin?", "id": "Program Tambahan Menambah Fitur." },
{ "en": "Apa Itu Ekstensi Browser?", "id": "Program Kecil Kustomisasi Browser." },
{ "en": "Apa Itu Beta Software?", "id": "Versi Uji Coba Perangkat Lunak." },
{ "en": "Apa Itu Update Perangkat Lunak?", "id": "Pembaruan Perbaikan Atau Fitur Baru." },
{ "en": "Apa Itu Patch?", "id": "Perbaikan Cepat Untuk Bug Spesifik." },
{ "en": "Apa Itu Bug?", "id": "Kesalahan Atau Cacat Program." },
{ "en": "Apa Itu Error?", "id": "Kesalahan Dalam Eksekusi Program." },
{ "en": "Apa Itu Sistem Bilangan Biner?", "id": "Sistem Angka Berbasis Dua (0, 1)." },
{ "en": "Apa Itu Sistem Bilangan Desimal?", "id": "Sistem Angka Berbasis Sepuluh." },
{ "en": "Apa Itu Sistem Bilangan Heksadesimal?", "id": "Sistem Angka Berbasis Enam Belas." },
{ "en": "Apa Itu ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter Teks." },
{ "en": "Apa Itu Unicode?", "id": "Standar Pengkodean Karakter Universal." },
{ "en": "Apa Itu Kompresi Data?", "id": "Proses Mengecilkan Ukuran Data." },
{ "en": "Apa Itu Kompresi Lossy?", "id": "Kompresi, Sebagian Data Hilang." },
{ "en": "Apa Itu Kompresi Lossless?", "id": "Kompresi, Data Asli Utuh." },
{ "en": "Contoh Format File Lossy?", "id": "JPEG Dan MP3." },
{ "en": "Contoh Format File Lossless?", "id": "PNG Dan ZIP." },
{ "en": "Apa Itu ZIP File?", "id": "Arsip Terkompresi Menggunakan Kompresi Lossless." },
{ "en": "Apa Itu Ekstrak File?", "id": "Mengembalikan File Dari Arsip Kompresi." },
{ "en": "Apa Itu Motherboard Form Factor?", "id": "Standar Ukuran Dan Bentuk Motherboard." },
{ "en": "Contoh Form Factor Motherboard?", "id": "ATX, Micro-ATX, Mini-ITX." },
{ "en": "Apa Itu Power Supply Unit (PSU)?", "id": "Unit Catu Daya Listrik Komputer." },
{ "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Unit Pemroses Grafis Dan Visual." },
{ "en": "Apa Itu Kartu Grafis (Video Card)?", "id": "Komponen Penghasil Output Gambar." },
{ "en": "Apa Itu Kartu Suara (Sound Card)?", "id": "Komponen Pengolah Input/Output Audio." },
{ "en": "Apa Itu Heatsink?", "id": "Komponen Pendingin Pasif Logam." },
{ "en": "Apa Itu Kipas Komputer?", "id": "Komponen Pendingin Aktif Udara." },
{ "en": "Apa Itu Pendingin Cair (Liquid Cooling)?", "id": "Sistem Pendingin Menggunakan Cairan." },
{ "en": "Apa Itu Overclocking?", "id": "Menjalankan Komponen Lebih Cepat Standar." },
{ "en": "Apa Itu Benchmark?", "id": "Pengujian Kinerja Komponen Komputer." },
{ "en": "Apa Itu Virtualisasi?", "id": "Membuat Versi Virtual Sumber Daya." },
{ "en": "Apa Itu Virtual Machine (VM)?", "id": "Mesin Virtual Simulasi Komputer." },
{ "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Pembuat VM." },
{ "en": "Apa Itu Emulator?", "id": "Mensimulasikan Perangkat Keras/Lunak Lain." },
{ "en": "Apa Itu Open Source Initiative (OSI)?", "id": "Organisasi Promosi Perangkat Lunak Open Source." },
{ "en": "Apa Itu Lisensi Perangkat Lunak?", "id": "Izin Legal Penggunaan Perangkat Lunak." },
{ "en": "Apa Itu EULA (End-User License Agreement)?", "id": "Perjanjian Lisensi Pengguna Akhir." },
{ "en": "Apa Itu SDLC (Software Development Life Cycle)?", "id": "Proses Tahapan Pengembangan Perangkat Lunak." },
{ "en": "Apa Itu Model Waterfall?", "id": "Model SDLC Sekuensial Linier." },
{ "en": "Apa Itu Model Agile?", "id": "Model SDLC Iteratif Dan Fleksibel." },
{ "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Agile Management Proyek." },
{ "en": "Apa Itu Sprint Dalam Scrum?", "id": "Siklus Kerja Singkat Pengembangan." },
{ "en": "Apa Itu OOP (Object-Oriented Programming)?", "id": "Paradigma Pemrograman Berbasis Objek." },
{ "en": "Apa Itu Kelas (Class) Dalam OOP?", "id": "Cetakan Biru Untuk Membuat Objek." },
{ "en": "Apa Itu Objek (Object) Dalam OOP?", "id": "Instansi Nyata Dari Sebuah Kelas." },
{ "en": "Apa Itu Inheritance (Pewarisan) OOP?", "id": "Kelas Mewarisi Sifat Kelas Lain." },
{ "en": "Apa Itu Polymorphism (Polimorfisme) OOP?", "id": "Objek Berbeda, Respon Sama." },
{ "en": "Apa Itu Encapsulation (Enkapsulasi) OOP?", "id": "Menyembunyikan Detail Implementasi Internal." },
{ "en": "Apa Itu Abstraction (Abstraksi) OOP?", "id": "Menampilkan Fitur Penting, Sembunyikan Kompleksitas." },
{ "en": "Apa Itu Struktur Data?", "id": "Cara Mengatur Data Di Komputer." },
{ "en": "Apa Itu Array?", "id": "Kumpulan Elemen Tipe Data Sama." },
{ "en": "Apa Itu Linked List?", "id": "Kumpulan Elemen Terhubung Via Pointer." },
{ "en": "Apa Itu Stack (Tumpukan)?", "id": "Struktur Data LIFO (Last-In First-Out)." },
{ "en": "Apa Itu Queue (Antrean)?", "id": "Struktur Data FIFO (First-In First-Out)." },
{ "en": "Apa Itu Tree (Pohon)?", "id": "Struktur Data Hirarkis Non-Linier." },
{ "en": "Apa Itu Graph (Graf)?", "id": "Kumpulan Node Dan Edge." },
{ "en": "Apa Itu Algoritma Pencarian (Search)?", "id": "Algoritma Menemukan Data Spesifik." },
{ "en": "Apa Itu Algoritma Pengurutan (Sort)?", "id": "Algoritma Mengatur Data Urutan Tertentu." },
{ "en": "Apa Itu Bubble Sort?", "id": "Algoritma Pengurutan Sederhana Berulang." },
{ "en": "Apa Itu Binary Search?", "id": "Algoritma Pencarian Data Terurut." },
{ "en": "Apa Itu TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Kumpulan Protokol Komunikasi Inti Internet." },
{ "en": "Apa Itu TCP (Transmission Control Protocol)?", "id": "Protokol Komunikasi Handal, Berurutan." },
{ "en": "Apa Itu UDP (User Datagram Protocol)?", "id": "Protokol Komunikasi Cepat, Tidak Handal." },
{ "en": "Apa Itu MAC Address (Media Access Control Address)?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
{ "en": "Apa Itu Port Jaringan?", "id": "Titik Akhir Komunikasi Logis." },
{ "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol Memberi IP Address Otomatis." },
{ "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Standar Transfer File Jaringan." },
{ "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Standar Pengiriman Email." },
{ "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Pengambilan Email Dari Server." },
{ "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Akses Email Di Server." },
{ "en": "Apa Itu SSH (Secure Shell)?", "id": "Protokol Koneksi Jarak Jauh Aman." },
{ "en": "Apa Itu Telnet?", "id": "Protokol Koneksi Jarak Jauh Tidak Aman." },
{ "en": "Apa Itu Autentikasi?", "id": "Proses Verifikasi Identitas Pengguna." },
{ "en": "Apa Itu Otorisasi?", "id": "Proses Pemberian Hak Akses." },
{ "en": "Apa Itu Kata Sandi (Password)?", "id": "Kunci Rahasia Untuk Autentikasi." },
{ "en": "Apa Itu Biometrik?", "id": "Autentikasi Menggunakan Karakteristik Fisik." },
{ "en": "Apa Itu Digital Signature?", "id": "Tanda Tangan Elektronik Terenkripsi." },
{ "en": "Apa Itu Sertifikat Digital?", "id": "File Digital Memvalidasi Identitas Online." },
{ "en": "Apa Itu CA (Certificate Authority)?", "id": "Entitas Penerbit Sertifikat Digital." },
{ "en": "Apa Itu Malware Analysis?", "id": "Studi Perilaku Perangkat Lunak Berbahaya." },
{ "en": "Apa Itu Penetration Testing?", "id": "Simulasi Serangan Siber Sah." },
{ "en": "Apa Itu Social Engineering?", "id": "Manipulasi Psikologis Dapatkan Informasi." },
{ "en": "Apa Itu Brute Force Attack?", "id": "Serangan Mencoba Semua Kombinasi Password." },
{ "en": "Apa Itu DDoS (Distributed Denial-of-Service)?", "id": "Serangan Membanjiri Server Dengan Trafik." },
{ "en": "Apa Itu Man-in-the-Middle (MITM) Attack?", "id": "Serangan Mencegat Komunikasi Dua Pihak." },
{ "en": "Apa Itu SQL Injection?", "id": "Serangan Memasukkan Kueri SQL Jahat." },
{ "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Serangan Memasukkan Skrip Jahat Website." },
{ "en": "Apa Itu Version Control System (VCS)?", "id": "Sistem Perekam Perubahan Kode." },
{ "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
{ "en": "Apa Itu Repository?", "id": "Tempat Penyimpanan Kode Proyek." },
{ "en": "Apa Itu Commit?", "id": "Menyimpan Perubahan Kode Ke Repository." },
{ "en": "Apa Itu Branch (Cabang)?", "id": "Versi Paralel Dari Kode Utama." },
{ "en": "Apa Itu Merge (Menggabungkan)?", "id": "Menggabungkan Perubahan Antar Branch." },
{ "en": "Apa Itu Frontend Development?", "id": "Pengembangan Sisi Klien (Tampilan)." },
{ "en": "Apa Itu Backend Development?", "id": "Pengembangan Sisi Server (Logika)." },
{ "en": "Apa Itu Fullstack Development?", "id": "Pengembangan Frontend Dan Backend." },
{ "en": "Apa Itu Framework?", "id": "Kerangka Kerja Pengembangan Aplikasi." },
{ "en": "Contoh Framework Frontend?", "id": "React, Angular, Dan Vue." },
{ "en": "Contoh Framework Backend?", "id": "Django, Laravel, Dan Express.js." },
{ "en": "Apa Itu Programmer?", "id": "Orang Yang Menulis Kode Komputer." },
{ "en": "Apa Itu System Analyst?", "id": "Menganalisis Kebutuhan Dan Desain Sistem." },
{ "en": "Apa Itu Database Administrator (DBA)?", "id": "Orang Yang Mengelola Database." },
{ "en": "Apa Itu Network Administrator?", "id": "Orang Yang Mengelola Jaringan Komputer." },
{ "en": "Apa Itu IT Support?", "id": "Memberikan Bantuan Teknis Pengguna." },
{ "en": "Apa Itu UI (User Interface) Designer?", "id": "Perancang Tampilan Antarmuka Pengguna." },
{ "en": "Apa Itu UX (User Experience) Designer?", "id": "Perancang Pengalaman Pengguna Produk." },
{ "en": "Apa Itu Software Tester?", "id": "Orang Yang Menguji Perangkat Lunak." },
{ "en": "Apa Itu Project Manager?", "id": "Orang Yang Mengelola Proyek IT." },
{ "en": "Apa Itu Data Scientist?", "id": "Menganalisis Data Kompleks Jumlah Besar." },
{ "en": "Apa Itu DevOps Engineer?", "id": "Menggabungkan Development Dan Operations." },
{ "en": "Apa Itu Cybersecurity Analyst?", "id": "Analis Keamanan Siber Perusahaan." },
{ "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Layanan Cloud Menyediakan Infrastruktur Virtual." },
{ "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Layanan Cloud Platform Pengembangan Aplikasi." },
{ "en": "Apa Itu SaaS (Software as a Service)?", "id": "Layanan Cloud Perangkat Lunak Jadi." },
{ "en": "Contoh Layanan IaaS?", "id": "Amazon Web Services (AWS) EC2." },
{ "en": "Contoh Layanan PaaS?", "id": "Heroku Dan Google App Engine." },
{ "en": "Contoh Layanan SaaS?", "id": "Gmail Dan Microsoft 365." },
{ "en": "Apa Itu Public Cloud?", "id": "Layanan Cloud Tersedia Untuk Umum." },
{ "en": "Apa Itu Private Cloud?", "id": "Layanan Cloud Untuk Satu Organisasi." },
{ "en": "Apa Itu Hybrid Cloud?", "id": "Gabungan Public Dan Private Cloud." },
{ "en": "Apa Itu File System?", "id": "Struktur Penyimpanan File Di Disk." },
{ "en": "Apa Itu NTFS (New Technology File System)?", "id": "File System Standar Windows Modern." },
{ "en": "Apa Itu FAT32 (File Allocation Table 32)?", "id": "File System Lama, Kompatibilitas Luas." },
{ "en": "Apa Itu ext4 (Fourth Extended Filesystem)?", "id": "File System Umum Digunakan Linux." },
{ "en": "Apa Itu Partisi Disk?", "id": "Pembagian Logis Dari Hard Drive." },
{ "en": "Apa Itu Formatting Disk?", "id": "Mempersiapkan Disk Dengan File System." },
{ "en": "Apa Itu Defragmentasi?", "id": "Menyusun Ulang File Terfragmentasi Disk." },
{ "en": "Apa Itu Kernel?", "id": "Inti Utama Dari Sistem Operasi." },
{ "en": "Apa Itu Shell?", "id": "Antarmuka Pengguna Ke Kernel OS." },
{ "en": "Apa Itu Desktop Environment?", "id": "Kumpulan GUI Pada Sistem Operasi." },
{ "en": "Apa Itu Command Prompt (CMD)?", "id": "CLI Bawaan Sistem Operasi Windows." },
{ "en": "Apa Itu Terminal?", "id": "Antarmuka Baris Perintah (CLI)." },
{ "en": "Apa Itu Pseudocode?", "id": "Deskripsi Informal Algoritma, Bukan Kode." },
{ "en": "Apa Itu Flowchart?", "id": "Diagram Alir Representasi Visual Algoritma." },
{ "en": "Apa Itu Scanner?", "id": "Perangkat Input Pemindai Dokumen." },
{ "en": "Apa Itu Webcam?", "id": "Kamera Video Input Komputer." },
{ "en": "Apa Itu Mikrofon?", "id": "Perangkat Input Perekam Suara." },
{ "en": "Apa Itu Speaker?", "id": "Perangkat Output Penghasil Suara." },
{ "en": "Apa Itu Headphone?", "id": "Perangkat Output Suara Personal." },
{ "en": "Apa Itu Joystick?", "id": "Perangkat Input Pengontrol Game." },
{ "en": "Apa Itu Touchpad?", "id": "Perangkat Input Kursor Laptop." },
{ "en": "Apa Itu Touchscreen?", "id": "Layar Input Sensitif Sentuhan." },
{ "en": "Apa Itu NIC (Network Interface Card)?", "id": "Kartu Penghubung Komputer Ke Jaringan." },
{ "en": "Apa Itu Network Switch?", "id": "Perangkat Penghubung Perangkat Jaringan LAN." },
{ "en": "Apa Itu Network Hub?", "id": "Perangkat Penghubung Jaringan Versi Lama." },
{ "en": "Apa Itu Access Point (AP)?", "id": "Perangkat Pemancar Sinyal Jaringan Wi-Fi." },
{ "en": "Apa Itu Port RJ45?", "id": "Konektor Standar Kabel Jaringan Ethernet." },
{ "en": "Apa Itu Port Audio Jack?", "id": "Konektor Standar Input Output Audio." },
{ "en": "Apa Itu DisplayPort?", "id": "Konektor Digital Audio Video." },
{ "en": "Apa Itu Slot PCIe (Peripheral Component Interconnect Express)?", "id": "Slot Ekspansi Cepat Motherboard." },
{ "en": "Apa Itu Slot RAM?", "id": "Tempat Memasang Modul RAM." },
{ "en": "Apa Itu Konektor SATA (Serial ATA)?", "id": "Konektor Standar Hard Drive." },
{ "en": "Apa Itu NVMe (Non-Volatile Memory Express)?", "id": "Protokol Penyimpanan SSD Super Cepat." },
{ "en": "Apa Itu Sistem Software?", "id": "Perangkat Lunak Pengelola Perangkat Keras." },
{ "en": "Apa Itu Application Software?", "id": "Perangkat Lunak Tugas Spesifik Pengguna." },
{ "en": "Apa Itu Utility Software?", "id": "Perangkat Lunak Perawatan Sistem." },
{ "en": "Apa Itu Embedded Software?", "id": "Perangkat Lunak Tertanam Sistem Khusus." },
{ "en": "Contoh Utility Software?", "id": "Antivirus, Disk Defragmenter." },
{ "en": "Apa Itu File Manager?", "id": "Utilitas Mengelola File Dan Folder." },
{ "en": "Apa Itu Task Manager?", "id": "Utilitas Memantau Proses Berjalan." },
{ "en": "Apa Itu Disk Cleanup?", "id": "Utilitas Menghapus File Tidak Perlu." },
{ "en": "Apa Itu Metadata?", "id": "Data Tentang Data Lain." },
{ "en": "Apa Itu Tabel Database?", "id": "Kumpulan Record Data Terstruktur." },
{ "en": "Apa Itu Record (Baris)?", "id": "Satu Entri Data Lengkap Tabel." },
{ "en": "Apa Itu Field (Kolom)?", "id": "Satu Jenis Data Record." },
{ "en": "Apa Itu Primary Key?", "id": "Kunci Unik Identifikasi Record." },
{ "en": "Apa Itu Foreign Key?", "id": "Kunci Penghubung Antar Tabel." },
{ "en": "Apa Itu Sintaks (Syntax) Pemrograman?", "id": "Aturan Penulisan Kode Bahasa." },
{ "en": "Apa Itu Semantik (Semantic) Pemrograman?", "id": "Arti Logis Dari Kode Program." },
{ "en": "Apa Itu Library (Pustaka)?", "id": "Kumpulan Kode Jadi Siap Pakai." },
{ "en": "Apa Itu Loop (Perulangan)?", "id": "Instruksi Mengulang Blok Kode." },
{ "en": "Jenis Loop Umum?", "id": "For Loop Dan While Loop." },
{ "en": "Apa Itu Conditional Statement?", "id": "Instruksi Berdasarkan Kondisi Benar/Salah." },
{ "en": "Contoh Conditional Statement?", "id": "If, Else, Dan Switch." },
{ "en": "Apa Itu IPV4 (Internet Protocol Version 4)?", "id": "Standar Alamat IP Versi 4." },
{ "en": "Apa Itu IPV6 (Internet Protocol Version 6)?", "id": "Standar Alamat IP Versi Baru." },
{ "en": "Apa Itu Subnet Mask?", "id": "Angka Pemisah Network ID Host ID." },
{ "en": "Apa Itu Gateway?", "id": "Gerbang Keluar Jaringan Lokal." },
{ "en": "Apa Itu Perintah Ping?", "id": "Perintah Tes Konektivitas Jaringan." },
{ "en": "Apa Itu Perintah Tracert/Traceroute?", "id": "Perintah Melacak Rute Paket Jaringan." },
{ "en": "Apa Itu DHCP Server?", "id": "Server Penyedia IP Address Otomatis." },
{ "en": "Apa Itu Password Manager?", "id": "Aplikasi Penyimpan Mengelola Kata Sandi." },
{ "en": "Apa Itu Akun Pengguna (User Account)?", "id": "Profil Pengguna Akses Sistem." },
{ "en": "Apa Itu Akun Administrator?", "id": "Akun Kontrol Penuh Sistem." },
{ "en": "Apa Itu Akun Tamu (Guest)?", "id": "Akun Akses Terbatas Sementara." },
{ "en": "Apa Itu File Permissions?", "id": "Izin Akses File (Baca, Tulis)." },
{ "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Sirkuit Digital Elektronik." },
{ "en": "Apa Itu Gerbang AND?", "id": "Logika Output 1 Jika Semua Input 1." },
{ "en": "Apa Itu Gerbang OR?", "id": "Logika Output 1 Jika Salah Satu Input 1." },
{ "en": "Apa Itu Gerbang NOT?", "id": "Logika Pembalik Nilai Input." },
{ "en": "Apa Itu Memori Volatile?", "id": "Memori Hilang Saat Listrik Mati." },
{ "en": "Contoh Memori Volatile?", "id": "RAM (Random Access Memory)." },
{ "en": "Apa Itu Memori Non-Volatile?", "id": "Memori Tetap Saat Listrik Mati." },
{ "en": "Contoh Memori Non-Volatile?", "id": "ROM, HDD, Dan SSD." },
{ "en": "Apa Itu Multitasking?", "id": "Menjalankan Banyak Tugas Sekaligus." },
{ "en": "Apa Itu Multithreading?", "id": "Menjalankan Banyak Thread Satu Proses." },
{ "en": "Apa Itu Proses (Process)?", "id": "Program Yang Sedang Dieksekusi." },
{ "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil Proses." },
{ "en": "Apa Itu HTTP Status Code?", "id": "Kode Respon Server Permintaan Web." },
{ "en": "Apa Itu Error 404?", "id": "Kode Status Halaman Tidak Ditemukan." },
{ "en": "Apa Itu Status 200 OK?", "id": "Kode Status Permintaan Berhasil." },
{ "en": "Apa Itu Container (Virtualisasi)?", "id": "Paket Aplikasi Dan Dependensinya." },
{ "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Container." },
{ "en": "Apa Itu Sistem Operasi Android?", "id": "Sistem Operasi Mobile Google." },
{ "en": "Apa Itu Sistem Operasi iOS?", "id": "Sistem Operasi Mobile Apple." },
{ "en": "Apa Itu Piracy (Pembajakan)?", "id": "Penggunaan Perangkat Lunak Ilegal." },
{ "en": "Apa Itu Copyright (Hak Cipta)?", "id": "Hak Legal Eksklusif Pencipta." },
{ "en": "Apa Itu Plagiarisme?", "id": "Menjiplak Karya Orang Lain." },
{ "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi." },
{ "en": "Apa Itu Mata Uang Kripto?", "id": "Mata Uang Digital Terenkripsi." },
{ "en": "Contoh Mata Uang Kripto?", "id": "Bitcoin Dan Ethereum." },
{ "en": "Apa Itu Quantum Computing?", "id": "Komputasi Menggunakan Prinsip Kuantum." },
{ "en": "Apa Itu Safe Mode?", "id": "Mode Diagnostik Sistem Operasi." },
{ "en": "Apa Itu Factory Reset?", "id": "Mengembalikan Perangkat Ke Pengaturan Pabrik." },
{ "en": "Apa Itu Troubleshooting?", "id": "Proses Mendiagnosis Menyelesaikan Masalah." },
{ "en": "Apa Itu Bandwidth Throttling?", "id": "Pembatasan Kecepatan Internet Sengaja." },
{ "en": "Apa Itu Data Breach?", "id": "Insiden Kebocoran Data Sensitif." },
{ "en": "Apa Itu Dark Web?", "id": "Bagian Internet Tersembunyi, Terenkripsi." },
{ "en": "Apa Itu Deep Web?", "id": "Bagian Web Tidak Terindeks." },
{ "en": "Apa Itu Surface Web?", "id": "Bagian Web Terindeks Google." },
{ "en": "Apa Itu End-to-End Encryption?", "id": "Enkripsi, Hanya Pengirim Penerima Baca." },
{ "en": "Apa Itu Hotspot?", "id": "Titik Akses Wi-Fi Publik." },
{ "en": "Apa Itu Tethering?", "id": "Berbagi Koneksi Internet Perangkat." },
{ "en": "Apa Itu Geolocation?", "id": "Identifikasi Lokasi Geografis Perangkat." },
{ "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Satelit Global." },
{ "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Teknologi Identifikasi Gelombang Radio." },
{ "en": "Apa Itu NFC (Near Field Communication)?", "id": "Teknologi Komunikasi Nirkabel Jarak Dekat." },
{ "en": "Apa Itu Barcode?", "id": "Kode Batang Visual Terbaca Mesin." },
{ "en": "Apa Itu Augmented Reality (AR)?", "id": "Menggabungkan Dunia Nyata Virtual." },
{ "en": "Apa Itu Virtual Reality (VR)?", "id": "Simulasi Imersif Lingkungan Virtual." },
{ "en": "Apa Itu Integer?", "id": "Tipe Data Bilangan Bulat." },
{ "en": "Apa Itu Float Atau Double?", "id": "Tipe Data Bilangan Desimal." },
{ "en": "Apa Itu Boolean?", "id": "Tipe Data Nilai Benar (True) Salah (False)." },
{ "en": "Apa Itu String?", "id": "Tipe Data Kumpulan Karakter." },
{ "en": "Apa Itu Karakter (Char)?", "id": "Tipe Data Satu Huruf." },
{ "en": "Apa Itu Operator Aritmatika?", "id": "Operator Matematika (Tambah, Kurang)." },
{ "en": "Apa Itu Operator Relasional?", "id": "Operator Perbandingan (Lebih Besar)." },
{ "en": "Apa Itu Operator Logika?", "id": "Operator (AND, OR, NOT)." },
{ "en": "Apa Itu Komentar Kode?", "id": "Catatan Penjelasan Dalam Kode." },
{ "en": "Apa Itu Pseudocode?", "id": "Deskripsi Algoritma Mirip Kode." },
{ "en": "Apa Itu Flowchart?", "id": "Diagram Alir Langkah Algoritma." },
{ "en": "Apa Itu Word Processor?", "id": "Aplikasi Pengolah Kata Dokumen." },
{ "en": "Contoh Word Processor?", "id": "Microsoft Word, Google Docs." },
{ "en": "Apa Itu Spreadsheet?", "id": "Aplikasi Pengolah Angka (Sheet)." },
{ "en": "Contoh Spreadsheet?", "id": "Microsoft Excel, Google Sheets." },
{ "en": "Apa Itu Presentation Software?", "id": "Aplikasi Membuat Slide Presentasi." },
{ "en": "Contoh Presentation Software?", "id": "Microsoft PowerPoint, Google Slides." },
{ "en": "Apa Itu Email Client?", "id": "Aplikasi Mengelola Email Pengguna." },
{ "en": "Apa Itu Web Browser?", "id": "Aplikasi Menjelajahi World Wide Web." },
{ "en": "Contoh Web Browser?", "id": "Chrome, Firefox, Dan Safari." },
{ "en": "Apa Itu Search Engine?", "id": "Mesin Pencari Informasi Internet." },
{ "en": "Contoh Search Engine?", "id": "Google, Bing, Dan Yahoo." },
{ "en": "Apa Itu Media Sosial?", "id": "Platform Interaksi Sosial Online." },
{ "en": "Apa Itu Blog?", "id": "Situs Web Jurnal Online." },
{ "en": "Apa Itu Forum Online?", "id": "Tempat Diskusi Online Publik." },
{ "en": "Apa Itu Usability?", "id": "Tingkat Kemudahan Penggunaan Produk." },
{ "en": "Apa Itu Accessibility?", "id": "Kemudahan Akses Produk Disabilitas." },
{ "en": "Apa Itu User-Friendly?", "id": "Sifat Mudah Digunakan Pengguna." },
{ "en": "Apa Itu Desain Intuitif?", "id": "Desain Mudah Dipahami Langsung." },
{ "en": "Apa Itu Wireframe?", "id": "Kerangka Dasar Tampilan Antarmuka." },
{ "en": "Apa Itu Prototype?", "id": "Model Awal Produk Uji Coba." },
{ "en": "Apa Itu Mockup?", "id": "Desain Visual Detail Statis." },
{ "en": "Apa Itu Ergonomi Komputer?", "id": "Studi Kenyamanan Kerja Komputer." },
{ "en": "Apa Itu Repetitive Strain Injury (RSI)?", "id": "Cedera Akibat Gerakan Berulang." },
{ "en": "Apa Itu Backlink?", "id": "Tautan Masuk Ke Situs Web." },
{ "en": "Apa Itu SEO (Search Engine Optimization)?", "id": "Optimasi Peringkat Mesin Pencari." },
{ "en": "Apa Itu Keyword?", "id": "Kata Kunci Pencarian Informasi." },
{ "en": "Apa Itu Control Panel?", "id": "Pusat Pengaturan Sistem Windows." },
{ "en": "Apa Itu System Preferences?", "id": "Pusat Pengaturan Sistem MacOS." },
{ "en": "Apa Itu Command 'dir'?", "id": "Perintah CLI Menampilkan Isi Direktori (Windows)." },
{ "en": "Apa Itu Command 'ls'?", "id": "Perintah CLI Menampilkan Isi Direktori (Linux)." },
{ "en": "Apa Itu Command 'cd'?", "id": "Perintah CLI Berpindah Direktori." },
{ "en": "Apa Itu Command 'mkdir'?", "id": "Perintah CLI Membuat Direktori Baru." },
{ "en": "Apa Itu Command 'copy'?", "id": "Perintah CLI Menyalin File." },
{ "en": "Apa Itu Command 'move'?", "id": "Perintah CLI Memindahkan File." },
{ "en": "Apa Itu Command 'del' Atau 'rm'?", "id": "Perintah CLI Menghapus File." },
{ "en": "Apa Itu Root Directory?", "id": "Direktori Paling Atas Sistem." },
{ "en": "Apa Itu Subdirectory?", "id": "Direktori Di Dalam Direktori Lain." },
{ "en": "Apa Itu File Path?", "id": "Lokasi Lengkap File Sistem." },
{ "en": "Apa Itu Absolute Path?", "id": "Jalur Lengkap Dari Root." },
{ "en": "Apa Itu Relative Path?", "id": "Jalur Relatif Direktori Saat Ini." },
{ "en": "Apa Itu Perintah 'ipconfig'?", "id": "Menampilkan Konfigurasi IP Windows." },
{ "en": "Apa Itu Perintah 'ifconfig'?", "id": "Menampilkan Konfigurasi IP Linux." },
{ "en": "Apa Itu Localhost?", "id": "Merujuk Ke Komputer Sendiri (127.0.0.1)." },
{ "en": "Apa Itu Clipboard?", "id": "Tempat Penyimpanan Sementara Copy/Paste." },
{ "en": "Apa Itu Copy?", "id": "Menyalin Data Ke Clipboard." },
{ "en": "Apa Itu Paste?", "id": "Menempel Data Dari Clipboard." },
{ "en": "Apa Itu Cut?", "id": "Memindah Data Ke Clipboard." },
{ "en": "Apa Itu Undo?", "id": "Membatalkan Tindakan Terakhir." },
{ "en": "Apa Itu Redo?", "id": "Mengulang Tindakan Yang Dibatalkan." },
{ "en": "Apa Itu Shortcut Keyboard?", "id": "Kombinasi Tombol Pintas Perintah." },
{ "en": "Contoh Shortcut 'Ctrl+C'?", "id": "Shortcut Untuk Perintah Copy." },
{ "en": "Contoh Shortcut 'Ctrl+V'?", "id": "Shortcut Untuk Perintah Paste." },
{ "en": "Contoh Shortcut 'Ctrl+Z'?", "id": "Shortcut Untuk Perintah Undo." },
{ "en": "Apa Itu Instalasi Perangkat Lunak?", "id": "Proses Memasang Program Komputer." },
{ "en": "Apa Itu Uninstalasi Perangkat Lunak?", "id": "Proses Menghapus Program Komputer." },
{ "en": "Apa Itu Lisensi Proprietary?", "id": "Lisensi Perangkat Lunak Berbayar." },
{ "en": "Apa Itu Lisensi GNU GPL (General Public License)?", "id": "Lisensi Populer Perangkat Lunak Bebas." },
{ "en": "Apa Itu Digital Footprint?", "id": "Jejak Data Aktivitas Online." },
{ "en": "Apa Itu Netiquette (Nettik)?", "id": "Etika Berkomunikasi Di Internet." },
{ "en": "Apa Itu Cyberbullying?", "id": "Penindasan Menggunakan Teknologi Digital." },
{ "en": "Apa Itu Hoax?", "id": "Informasi Bohong Atau Palsu." },
{ "en": "Apa Itu Spam?", "id": "Email Atau Pesan Sampah." },
{ "en": "Apa Itu Clickbait?", "id": "Judul Konten Pancingan Klik." },
{ "en": "Apa Itu Internet Censorship?", "id": "Sensor Konten Di Internet." },
{ "en": "Apa Itu Data Center?", "id": "Fasilitas Penyimpanan Server Data." },
{ "en": "Apa Itu Redundancy?", "id": "Duplikasi Komponen Sistem Cadangan." },
{ "en": "Apa Itu Disaster Recovery Plan?", "id": "Rencana Pemulihan Bencana Sistem." },
{ "en": "Apa Itu Uptime?", "id": "Waktu Sistem Beroperasi Normal." },
{ "en": "Apa Itu Downtime?", "id": "Waktu Sistem Tidak Beroperasi." },
{ "en": "Apa Itu Load Balancing?", "id": "Distribusi Beban Trafik Jaringan." },
{ "en": "Apa Itu Skalabilitas (Scalability)?", "id": "Kemampuan Sistem Tangani Peningkatan Beban." },
{ "en": "Apa Itu Boolean Algebra?", "id": "Aljabar Logika Menggunakan True/False." },
{ "en": "Apa Itu Truth Table?", "id": "Tabel Menampilkan Output Logika." },
{ "en": "Apa Itu Hash Table?", "id": "Struktur Data Akses Cepat." },
{ "en": "Apa Itu Hashing?", "id": "Transformasi Data Menjadi Nilai Tetap." },
{ "en": "Apa Itu Collision (Hashing)?", "id": "Dua Input Menghasilkan Hash Sama." },
{ "en": "Apa Itu Konversi Biner Ke Desimal?", "id": "Mengubah Angka Basis 2 Ke 10." },
{ "en": "Apa Itu Konversi Desimal Ke Biner?", "id": "Mengubah Angka Basis 10 Ke 2." },
{ "en": "Apa Itu Interpreter?", "id": "Menerjemahkan Kode Baris Per Baris." },
{ "en": "Perbedaan Compiler Dan Interpreter?", "id": "Compiler Menerjemahkan Sekaligus, Interpreter Per Baris." },
{ "en": "Apa Itu Bytecode?", "id": "Kode Menengah Hasil Kompilasi." },
{ "en": "Apa Itu Java Virtual Machine (JVM)?", "id": "Mesin Virtual Menjalankan Bytecode Java." },
{ "en": "Apa Itu IT Help Desk?", "id": "Layanan Dukungan Teknis Pengguna." },
{ "en": "Apa Itu Trouble Ticket?", "id": "Catatan Laporan Masalah Teknis." },
{ "en": "Apa Itu Remote Desktop?", "id": "Mengakses Komputer Dari Jarak Jauh." },
{ "en": "Apa Itu Screen Sharing?", "id": "Berbagi Tampilan Layar Komputer." },
{ "en": "Apa Itu CPU Socket?", "id": "Tempat Dudukan CPU Di Motherboard." },
{ "en": "Apa Fungsi Thermal Paste?", "id": "Penghantar Panas CPU Ke Heatsink." },
{ "en": "Apa Itu Chipset Motherboard?", "id": "Pengontrol Komunikasi Komponen Papan Induk." },
{ "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Chip Penyimpan Pengaturan BIOS." },
{ "en": "Apa Fungsi Baterai CMOS?", "id": "Memberi Daya Chip CMOS." },
{ "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS Modern Grafis." },
{ "en": "Apa Itu Port DVI (Digital Visual Interface)?", "id": "Konektor Video Digital, Pengganti VGA." },
{ "en": "Apa Itu Port PS/2?", "id": "Konektor Lama Keyboard Dan Mouse." },
{ "en": "Apa Itu Serial Port?", "id": "Port Komunikasi Serial Data Lama." },
{ "en": "Apa Itu Parallel Port?", "id": "Port Komunikasi Paralel, Biasa Printer." },
{ "en": "Apa Itu Case Fan?", "id": "Kipas Pendingin Sirkulasi Udara Casing." },
{ "en": "Apa Itu Software Kolaborasi?", "id": "Aplikasi Bekerja Bersama Jarak Jauh." },
{ "en": "Apa Itu ERP (Enterprise Resource Planning)?", "id": "Sistem Perencanaan Sumber Daya Perusahaan." },
{ "en": "Apa Itu CRM (Customer Relationship Management)?", "id": "Sistem Pengelola Hubungan Pelanggan." },
{ "en": "Apa Itu Software Grafis?", "id": "Aplikasi Membuat Mengedit Gambar." },
{ "en": "Contoh Software Grafis?", "id": "Adobe Photoshop, CorelDRAW." },
{ "en": "Apa Itu CAD (Computer-Aided Design)?", "id": "Software Desain Bantuan Komputer." },
{ "en": "Apa Itu Ruang Lingkup Variabel (Scope)?", "id": "Area Kode Variabel Dapat Diakses." },
{ "en": "Apa Itu Konstanta (Constant)?", "id": "Variabel Nilai Tetap Tidak Berubah." },
{ "en": "Apa Itu Parameter Fungsi?", "id": "Input Yang Diterima Fungsi." },
{ "en": "Apa Itu Argumen Fungsi?", "id": "Nilai Diberikan Ke Parameter Fungsi." },
{ "en": "Apa Itu Return Value?", "id": "Nilai Hasil Keluaran Fungsi." },
{ "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Memanggil Dirinya Sendiri." },
{ "en": "Perbedaan Library Dan Framework?", "id": "Library Dipanggil, Framework Mengatur Alur." },
{ "en": "Apa Itu Subnetting?", "id": "Proses Membagi Jaringan IP Besar." },
{ "en": "Apa Itu Private IP Address?", "id": "Alamat IP Internal Jaringan Lokal." },
{ "en": "Apa Itu Public IP Address?", "id": "Alamat IP Unik Di Internet." },
{ "en": "Apa Itu NAT (Network Address Translation)?", "id": "Mengubah IP Privat Ke Publik." },
{ "en": "Apa Itu Port Forwarding?", "id": "Meneruskan Trafik Port Jaringan." },
{ "en": "Apa Itu Integritas Data?", "id": "Keakuratan Konsistensi Data Tersimpan." },
{ "en": "Apa Itu Redundansi Data?", "id": "Duplikasi Data Yang Tidak Perlu." },
{ "en": "Apa Itu Konsistensi Data?", "id": "Keseragaman Data Di Berbagai Tempat." },
{ "en": "Apa Itu Normalisasi Database?", "id": "Proses Mengurangi Redundansi Data." },
{ "en": "Apa Itu Data Warehouse?", "id": "Tempat Penyimpanan Data Besar Analisis." },
{ "en": "Apa Itu Data Mining?", "id": "Proses Menemukan Pola Data Besar." },
{ "en": "Apa Itu Web Server?", "id": "Server Penyedia Konten Halaman Web." },
{ "en": "Contoh Web Server?", "id": "Apache, Nginx." },
{ "en": "Apa Itu Application Server?", "id": "Server Menjalankan Logika Aplikasi Bisnis." },
{ "en": "Apa Itu Static IP Address?", "id": "Alamat IP Tetap Tidak Berubah." },
{ "en": "Apa Itu Dynamic IP Address?", "id": "Alamat IP Berubah, Diberi DHCP." },
{ "en": "Apa Itu Domain Registrar?", "id": "Perusahaan Tempat Mendaftar Nama Domain." },
{ "en": "Apa Itu Webmail?", "id": "Layanan Email Diakses Melalui Browser." },
{ "en": "Apa Itu Keylogger?", "id": "Malware Perekam Setiap Tombol Ditekan." },
{ "en": "Apa Itu Rootkit?", "id": "Malware Bersembunyi Akses Root." },
{ "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Terinfeksi (Zombie)." },
{ "en": "Apa Itu Zombie Computer?", "id": "Komputer Terinfeksi Dikendalikan Jarak Jauh." },
{ "en": "Apa Itu Honeypot?", "id": "Sistem Jebakan Memancing Peretas." },
{ "en": "Apa Itu IDS (Intrusion Detection System)?", "id": "Sistem Pendeteksi Aktivitas Serangan Jaringan." },
{ "en": "Apa Itu IPS (Intrusion Prevention System)?", "id": "Sistem Pencegah Aktivitas Serangan Jaringan." },
{ "en": "Apa Itu Legacy System?", "id": "Sistem Teknologi Lama Masih Digunakan." },
{ "en": "Apa Itu Alpha Testing?", "id": "Pengujian Internal Pertama Produk." },
{ "en": "Apa Itu Beta Testing?", "id": "Pengujian Eksternal Oleh Pengguna." },
{ "en": "Apa Itu End User?", "id": "Pengguna Akhir Produk Aplikasi." },
{ "en": "Apa Itu User Manual?", "id": "Buku Panduan Penggunaan Produk." },
{ "en": "Apa Itu Knowledge Base?", "id": "Kumpulan Informasi Bantuan Mandiri." },
{ "en": "Apa Itu File Attribute?", "id": "Properti Metadata File (Read-Only)." },
{ "en": "Apa Itu Hidden File?", "id": "File Yang Disembunyikan Tampilan Default." },
{ "en": "Apa Itu System File?", "id": "File Penting Sistem Operasi." },
{ "en": "Atribut Read-Only Artinya?", "id": "File Hanya Bisa Dibaca." },
{ "en": "Apa Itu File Archive?", "id": "Kumpulan File Disatukan Kompresi." },
{ "en": "Contoh Format File Archive?", "id": ".ZIP, .RAR, .7z." },
{ "en": "Apa Itu Pixel Density (PPI)?", "id": "Kepadatan Pixel Per Inci Layar." },
{ "en": "Apa Itu Aspect Ratio?", "id": "Rasio Perbandingan Lebar Tinggi Layar." },
{ "en": "Contoh Aspect Ratio?", "id": "16:9 Atau 4:3." },
{ "en": "Apa Itu Refresh Rate?", "id": "Kecepatan Layar Memperbarui Gambar." },
{ "en": "Apa Satuan Refresh Rate?", "id": "Hertz (Hz)." },
{ "en": "Apa Itu Response Time?", "id": "Waktu Pixel Berubah Warna." },
{ "en": "Apa Satuan Response Time?", "id": "Milidetik (Ms)." },
{ "en": "Apa Itu Color Depth?", "id": "Jumlah Bit Penyimpan Warna Pixel." },
{ "en": "Apa Itu Grayscale?", "id": "Gambar Skala Abu-Abu, Hitam Putih." },
{ "en": "Apa Itu GUI Testing?", "id": "Pengujian Antarmuka Grafis Pengguna." },
{ "en": "Apa Itu Unit Testing?", "id": "Pengujian Unit Terkecil Kode." },
{ "en": "Apa Itu Integration Testing?", "id": "Pengujian Penggabungan Modul Kode." },
{ "en": "Apa Itu System Testing?", "id": "Pengujian Sistem Lengkap Terintegrasi." },
{ "en": "Apa Itu Acceptance Testing?", "id": "Pengujian Penerimaan Oleh Pengguna." },
{ "en": "Apa Itu Regression Testing?", "id": "Pengujian Ulang Pastikan Update Berhasil." },
{ "en": "Apa Itu White Box Testing?", "id": "Pengujian Struktur Internal Kode." },
{ "en": "Apa Itu Black Box Testing?", "id": "Pengujian Fungsionalitas Tanpa Lihat Kode." },
{ "en": "Apa Itu Zero-Day Exploit?", "id": "Serangan Celah Keamanan Belum Diketahui." },
{ "en": "Apa Itu Data Center Tier?", "id": "Standar Klasifikasi Keandalan Data Center." },
{ "en": "Apa Itu Hot Swappable?", "id": "Komponen Bisa Diganti Tanpa Matikan." },
{ "en": "Apa Itu Cold Swappable?", "id": "Komponen Harus Diganti Saat Mati." },
{ "en": "Apa Itu Server Blade?", "id": "Server Ringkas Tipis Modular." },
{ "en": "Apa Itu Rack Server?", "id": "Server Dirancang Muat Rak Logam." },
{ "en": "Apa Itu Tower Server?", "id": "Server Bentuk Casing Komputer Biasa." },
{ "en": "Apa Itu Load Tester?", "id": "Alat Penguji Beban Sistem." },
{ "en": "Apa Itu Log File?", "id": "File Catatan Aktivitas Sistem." },
{ "en": "Apa Itu Event Viewer?", "id": "Alat Windows Melihat Log Sistem." },
{ "en": "Apa Itu Registry Editor?", "id": "Database Pengaturan Konfigurasi Windows." },
{ "en": "Apa Itu Disk Imaging?", "id": "Membuat Salinan Persis Hard Drive." },
{ "en": "Apa Itu Cloning Disk?", "id": "Menyalin Isi Satu Disk Ke Disk Lain." },
{ "en": "Apa Itu Boot Sector?", "id": "Bagian Disk Berisi Kode Boot." },
{ "en": "Apa Itu MBR (Master Boot Record)?", "id": "Sektor Boot Utama Disk Lama." },
{ "en": "Apa Itu GPT (GUID Partition Table)?", "id": "Standar Partisi Disk Pengganti MBR." },
{ "en": "Apa Itu Kernel Panic?", "id": "Error Fatal Sistem Operasi Unix." },
{ "en": "Apa Itu Blue Screen of Death (BSOD)?", "id": "Layar Error Fatal Sistem Windows." },
{ "en": "Apa Itu Dual Boot?", "id": "Memasang Dua Sistem Operasi." },
{ "en": "Apa Itu Mobile Application (App)?", "id": "Program Perangkat Lunak Ponsel." },
{ "en": "Apa Itu App Store?", "id": "Toko Aplikasi Digital Resmi." },
{ "en": "Apa Itu Google Play Store?", "id": "Toko Aplikasi Resmi Android." },
{ "en": "Apa Itu Apple App Store?", "id": "Toko Aplikasi Resmi iOS." },
{ "en": "Apa Itu APK (Android Package Kit)?", "id": "Format File Instalasi Aplikasi Android." },
{ "en": "Apa Itu Sideloading?", "id": "Instalasi Aplikasi Sumber Tidak Resmi." },
{ "en": "Apa Itu Update OTA (Over-The-Air)?", "id": "Pembaruan Perangkat Lunak Nirkabel." },
{ "en": "Apa Itu SIM (Subscriber Identity Module) Card?", "id": "Kartu Identitas Pelanggan Jaringan Seluler." },
{ "en": "Apa Itu eSIM (Embedded SIM)?", "id": "SIM Digital Tertanam Perangkat." },
{ "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor Identitas Unik Perangkat Seluler." },
{ "en": "Apa Itu Hyperlink?", "id": "Tautan Penghubung Antar Halaman Web." },
{ "en": "Apa Itu Anchor Text?", "id": "Teks Klik-able Pada Hyperlink." },
{ "en": "Apa Itu Breadcrumbs (Navigasi)?", "id": "Jejak Navigasi Tunjukkan Lokasi Halaman." },
{ "en": "Apa Itu Sitemap?", "id": "Peta Struktur Situs Web." },
{ "en": "Apa Itu Favicon?", "id": "Ikon Kecil Tab Browser." },
{ "en": "Apa Itu Error 500 (Internal Server Error)?", "id": "Error Internal Pada Server." },
{ "en": "Apa Itu Error 403 (Forbidden)?", "id": "Akses Ditolak Server." },
{ "en": "Apa Itu Error 401 (Unauthorized)?", "id": "Autentikasi Diperlukan Gagal." },
{ "en": "Apa Itu Caching Sisi Klien?", "id": "Penyimpanan Cache Di Browser Pengguna." },
{ "en": "Apa Itu Caching Sisi Server?", "id": "Penyimpanan Cache Di Server Web." },
{ "en": "Apa Itu Data Validation?", "id": "Proses Memastikan Data Valid Benar." },
{ "en": "Apa Itu Data Visualization?", "id": "Representasi Visual Data (Grafik)." },
{ "en": "Apa Itu Array Index?", "id": "Posisi Elemen Dalam Array." },
{ "en": "Apa Itu String Concatenation?", "id": "Menggabungkan Dua String Teks." },
{ "en": "Apa Itu Nilai Null?", "id": "Nilai Kosong, Tidak Ada Nilai." },
{ "en": "Apa Itu Nilai Undefined?", "id": "Variabel Dideklarasi, Belum Diisi." },
{ "en": "Apa Itu VLAN (Virtual Local Area Network)?", "id": "LAN Virtual Logis Terisolasi." },
{ "en": "Apa Itu Port Scanning?", "id": "Pemindaian Port Terbuka Server." },
{ "en": "Apa Itu MAC Spoofing?", "id": "Pemalsuan Alamat MAC Perangkat." },
{ "en": "Apa Itu Packet Sniffing?", "id": "Mencegat Menganalisis Lalu Lintas Jaringan." },
{ "en": "Apa Itu Northbridge Chipset?", "id": "Chipset Penghubung CPU, RAM, GPU." },
{ "en": "Apa Itu Southbridge Chipset?", "id": "Chipset Pengatur Perangkat I/O Lambat." },
{ "en": "Apa Itu Jumper Motherboard?", "id": "Penghubung Pin Mengatur Konfigurasi." },
{ "en": "Apa Itu DIP Switch?", "id": "Saklar Kecil Mengatur Konfigurasi Hardware." },
{ "en": "Apa Itu Journaling File System?", "id": "File System Pencatat Perubahan (Jurnal)." },
{ "en": "Apa Itu Root User?", "id": "Pengguna Super Administrator Linux." },
{ "en": "Apa Itu UAC (User Account Control)?", "id": "Fitur Keamanan Konfirmasi Aksi Windows." },
{ "en": "Apa Itu QA (Quality Assurance)?", "id": "Proses Penjaminan Kualitas Perangkat Lunak." },
{ "en": "Apa Itu Data Entry?", "id": "Proses Memasukkan Data Ke Sistem." },
{ "en": "Apa Itu Webmaster?", "id": "Orang Pengelola Situs Web." },
{ "en": "Apa Itu IT Consultant?", "id": "Konsultan Profesional Bidang IT." },
{ "en": "Apa Itu Vulnerability (Kelemahan)?", "id": "Celah Keamanan Sistem Perangkat Lunak." },
{ "en": "Apa Itu Exploit?", "id": "Kode Memanfaatkan Vulnerability Sistem." },
{ "en": "Apa Itu Patch Management?", "id": "Manajemen Instalasi Pembaruan Keamanan." },
{ "en": "Apa Itu Security Audit?", "id": "Audit Evaluasi Keamanan Sistem." },
{ "en": "Apa Itu Principle of Least Privilege?", "id": "Prinsip Hak Akses Minimal." },
{ "en": "Apa Itu System Restore Point?", "id": "Titik Pemulihan Konfigurasi Sistem." },
{ "en": "Apa Itu Digital Divide?", "id": "Kesenjangan Akses Teknologi Digital." },
{ "en": "Apa Itu E-Waste (Limbah Elektronik)?", "id": "Sampah Perangkat Elektronik Bekas." },
{ "en": "Apa Itu CRUD (Create, Read, Update, Delete)?", "id": "Empat Operasi Dasar Database." },
{ "en": "Apa Itu NoSQL Database?", "id": "Database Non-Relasional, Fleksibel." },
{ "en": "Perbedaan SQL Dan NoSQL?", "id": "SQL Relasional, NoSQL Non-Relasional." },
{ "en": "Apa Itu Data Mart?", "id": "Subset Data Warehouse Spesifik." },
{ "en": "Apa Itu Primary Key Composite?", "id": "Primary Key Gabungan Dua Kolom." },
{ "en": "Apa Itu Database Index?", "id": "Struktur Mempercepat Pencarian Data." },
{ "en": "Apa Itu Query Database?", "id": "Perintah Permintaan Data Database." },
{ "en": "Apa Itu VMware?", "id": "Perusahaan Software Virtualisasi Populer." },
{ "en": "Apa Itu Hyper-V?", "id": "Teknologi Hypervisor Milik Microsoft." },
{ "en": "Apa Itu Container Orchestration?", "id": "Manajemen Otomatisasi Container Skala Besar." },
{ "en": "Apa Itu Kubernetes (K8s)?", "id": "Platform Orchestration Container Open Source." },
{ "en": "Apa Itu Vector Graphics?", "id": "Gambar Berbasis Rumus Matematika." },
{ "en": "Apa Itu Raster Graphics?", "id": "Gambar Berbasis Kumpulan Pixel." },
{ "en": "Perbedaan Vector Dan Raster?", "id": "Vector Skalabel, Raster Pecah." },
{ "en": "Apa Itu Format SVG (Scalable Vector Graphics)?", "id": "Format File Gambar Vector XML." },
{ "en": "Apa Itu Codec?", "id": "Kompresor Dekompresor Data Media." },
{ "en": "Apa Itu Bitrate?", "id": "Jumlah Bit Diproses Per Detik." },
{ "en": "Apa Itu Resolusi Video?", "id": "Jumlah Pixel Lebar Kali Tinggi." },
{ "en": "Apa Itu Frame Rate (FPS)?", "id": "Jumlah Frame Ditampilkan Per Detik." },
{ "en": "Apa Itu File Plain Text?", "id": "File Teks Murni Tanpa Format." },
{ "en": "Apa Itu Rich Text Format (RTF)?", "id": "File Teks Dengan Format Styling." },
{ "en": "Apa Itu Readme File?", "id": "File Berisi Informasi Dasar Proyek." },
{ "en": "Apa Itu Changelog?", "id": "Catatan Daftar Perubahan Versi." },
{ "en": "Apa Itu Help File?", "id": "File Dokumentasi Bantuan Pengguna." },
{ "en": "Apa Itu Syntax Highlighting?", "id": "Pewarnaan Kode Sesuai Sintaks." },
{ "en": "Apa Itu Code Editor?", "id": "Editor Teks Khusus Pemrograman." },
{ "en": "Apa Itu WYSIWYG (What You See Is What You Get)?", "id": "Editor Tampilan Visual Sesuai Hasil." },
{ "en": "Apa Itu Software Portable?", "id": "Aplikasi Berjalan Tanpa Instalasi." },
{ "en": "Apa Itu Executable File (.exe)?", "id": "File Program Dapat Dieksekusi Windows." },
{ "en": "Apa Itu Administrator Privileges?", "id": "Hak Akses Tertinggi Sistem." },
{ "en": "Apa Itu Standard User?", "id": "Pengguna Biasa Hak Akses Terbatas." },
{ "en": "Apa Itu File Association?", "id": "Asosiasi Tipe File Aplikasi Default." },
{ "en": "Apa Itu Default Program?", "id": "Program Otomatis Buka Tipe File." },
{ "en": "Apa Itu Service (Windows)?", "id": "Program Berjalan Latar Belakang." },
{ "en": "Apa Itu Daemon (Linux)?", "id": "Program Latar Belakang Linux." },
{ "en": "Apa Itu Task Scheduler?", "id": "Penjadwal Tugas Otomatis Komputer." },
{ "en": "Apa Itu Cron (Linux)?", "id": "Penjadwal Tugas Otomatis Linux." },
{ "en": "Apa Itu Virtual Memory?", "id": "Memori Virtual Menggunakan Hard Drive." },
{ "en": "Apa Itu Page File (Swap File)?", "id": "File Disk Untuk Virtual Memory." },
{ "en": "Apa Itu Memory Leak?", "id": "Program Gagal Melepas Memori." },
{ "en": "Apa Itu Garbage Collection?", "id": "Proses Otomatis Membersihkan Memori." },
{ "en": "Apa Itu Command 'sudo' (Linux)?", "id": "Menjalankan Perintah Hak Root." },
{ "en": "Apa Itu Command 'apt' (Linux)?", "id": "Manajemen Paket Debian/Ubuntu." },
{ "en": "Apa Itu Command 'yum' (Linux)?", "id": "Manajemen Paket Red Hat/CentOS." },
{ "en": "Apa Itu Package Manager?", "id": "Alat Instalasi Pembaruan Software." },
{ "en": "Apa Itu Linux Distribution (Distro)?", "id": "Varian Sistem Operasi Linux." },
{ "en": "Contoh Linux Distro?", "id": "Ubuntu, Fedora, Dan Debian." },
{ "en": "Apa Itu GUI Shell?", "id": "Antarmuka Grafis Pengganti CLI." },
{ "en": "Apa Itu Remote Administration?", "id": "Administrasi Sistem Jarak Jauh." },
{ "en": "Apa Itu RDP (Remote Desktop Protocol)?", "id": "Protokol Remote Desktop Windows." },
{ "en": "Apa Itu VNC (Virtual Network Computing)?", "id": "Sistem Remote Desktop Grafis." },
{ "en": "Apa Itu Firewall Perangkat Keras?", "id": "Perangkat Fisik Penyaring Jaringan." },
{ "en": "Apa Itu Firewall Perangkat Lunak?", "id": "Program Penyaring Jaringan Komputer." },
{ "en": "Apa Itu DMZ (Demilitarized Zone)?", "id": "Zona Jaringan Terisolasi Keamanan." },
{ "en": "Apa Itu Proxy Server?", "id": "Server Perantara Antara Klien Internet." },
{ "en": "Apa Itu Reverse Proxy?", "id": "Proxy Server Pelindung Server Internal." },
{ "en": "Apa Itu Kriptografi?", "id": "Ilmu Teknik Enkripsi Data." },
{ "en": "Apa Itu Steganografi?", "id": "Ilmu Menyembunyikan Pesan Data." },
{ "en": "Apa Itu Information Security?", "id": "Keamanan Informasi Melindungi Data." },
{ "en": "Apa Itu CIA (Confidentiality, Integrity, Availability)?", "id": "Tiga Prinsip Utama Keamanan Informasi." },
{ "en": "Apa Itu Confidentiality (Kerahasiaan)?", "id": "Menjaga Data Rahasia Tidak Terbuka." },
{ "en": "Apa Itu Integrity (Integritas)?", "id": "Menjaga Data Tetap Akurat Utuh." },
{ "en": "Apa Itu Availability (Ketersediaan)?", "id": "Memastikan Data Sistem Dapat Diakses." },
{ "en": "Apa Itu Ethical Hacking?", "id": "Peretasan Sah Uji Keamanan." },
{ "en": "Apa Itu White Hat Hacker?", "id": "Peretas Etis Menguji Keamanan." },
{ "en": "Apa Itu Black Hat Hacker?", "id": "Peretas Jahat Aktivitas Ilegal." },
{ "en": "Apa Itu Gray Hat Hacker?", "id": "Peretas Antara Etis Ilegal." },
{ "en": "Apa Itu Script Kiddie?", "id": "Peretas Pemula Menggunakan Skrip Jadi." },
{ "en": "Apa Itu Hacktivist?", "id": "Peretas Motivasi Politik Sosial." },
{ "en": "Apa Itu Software Development?", "id": "Proses Pembuatan Perangkat Lunak." },
{ "en": "Apa Itu Requirements Gathering?", "id": "Pengumpulan Kebutuhan Awal Proyek." },
{ "en": "Apa Itu Software Design?", "id": "Tahap Perancangan Arsitektur Software." },
{ "en": "Apa Itu Software Implementation?", "id": "Tahap Penulisan Kode Program." },
{ "en": "Apa Itu Software Testing?", "id": "Tahap Pengujian Mencari Bug." },
{ "en": "Apa Itu Software Deployment?", "id": "Tahap Peluncuran Perangkat Lunak." },
{ "en": "Apa Itu Software Maintenance?", "id": "Tahap Perawatan Pembaruan Software." },
{ "en": "Apa Itu IT Infrastructure?", "id": "Infrastruktur Teknologi Informasi Perusahaan." },
{ "en": "Apa Itu Network Topology Star?", "id": "Topologi Jaringan Terhubung Ke Pusat." },
{ "en": "Apa Itu Bus Topology?", "id": "Topologi Jaringan Terhubung Kabel Tunggal." },
{ "en": "Apa Itu Ring Topology?", "id": "Topologi Jaringan Terhubung Melingkar." },
{ "en": "Apa Itu Mesh Topology?", "id": "Topologi Jaringan Saling Terhubung Penuh." },
{ "en": "Apa Itu Tree Topology?", "id": "Topologi Jaringan Gabungan Star Bus." },
{ "en": "Apa Itu Hybrid Topology?", "id": "Topologi Gabungan Dua Jenis Berbeda." },
{ "en": "Apa Itu OSI (Open Systems Interconnection) Model?", "id": "Model Konseptual Tujuh Lapis Jaringan." },
{ "en": "Sebutkan Lapis Terbawah OSI?", "id": "Lapis Fisik (Physical Layer)." },
{ "en": "Sebutkan Lapis Teratas OSI?", "id": "Lapis Aplikasi (Application Layer)." },
{ "en": "Apa Fungsi Physical Layer OSI?", "id": "Transmisi Bit Mentah Fisik." },
{ "en": "Apa Fungsi Data Link Layer OSI?", "id": "Pengalamatan Fisik (MAC) Error Control." },
{ "en": "Apa Fungsi Network Layer OSI?", "id": "Pengalamatan Logis (IP) Routing." },
{ "en": "Apa Fungsi Transport Layer OSI?", "id": "Koneksi Antar Host (TCP/UDP)." },
{ "en": "Apa Fungsi Session Layer OSI?", "id": "Mengelola Sesi Komunikasi Antar Aplikasi." },
{ "en": "Apa Fungsi Presentation Layer OSI?", "id": "Translasi Data, Enkripsi, Kompresi." },
{ "en": "Apa Fungsi Application Layer OSI?", "id": "Antarmuka Pengguna Ke Jaringan." },
{ "en": "Apa Itu Router?", "id": "Perangkat Jaringan Lapis Tiga (Network)." },
{ "en": "Apa Itu Switch?", "id": "Perangkat Jaringan Lapis Dua (Data Link)." },
{ "en": "Apa Itu Hub?", "id": "Perangkat Jaringan Lapis Satu (Physical)." },
{ "en": "Apa Itu Bridge?", "id": "Penghubung Dua Segmen Jaringan LAN." },
{ "en": "Apa Itu Repeater?", "id": "Penguat Sinyal Jaringan Fisik." },
{ "en": "Apa Itu Gateway Jaringan?", "id": "Gerbang Penghubung Jaringan Berbeda Protokol." },
{ "en": "Apa Itu Packet?", "id": "Unit Data Lapis Jaringan (Network)." },
{ "en": "Apa Itu Frame?", "id": "Unit Data Lapis Data Link." },
{ "en": "Apa Itu Segment?", "id": "Unit Data Lapis Transport." },
{ "en": "Apa Itu Bit (Jaringan)?", "id": "Unit Data Lapis Fisik." },
{ "en": "Apa Itu Model TCP/IP?", "id": "Model Standar Protokol Internet Praktis." },
{ "en": "Berapa Lapis Model TCP/IP?", "id": "Empat Lapis (Umumnya Dikenal)." },
{ "en": "Apa Itu Port 80?", "id": "Port Standar Protokol HTTP." },
{ "en": "Apa Itu Port 443?", "id": "Port Standar Protokol HTTPS." },
{ "en": "Apa Itu Port 21?", "id": "Port Standar Protokol FTP (Control)." },
{ "en": "Apa Itu Port 22?", "id": "Port Standar Protokol SSH." },
{ "en": "Apa Itu Port 25?", "id": "Port Standar Protokol SMTP." },
{ "en": "Apa Itu Port 53?", "id": "Port Standar Layanan DNS." },
{ "en": "Apa Itu Port 110?", "id": "Port Standar Protokol POP3." },
{ "en": "Apa Itu Port 143?", "id": "Port Standar Protokol IMAP." },
{ "en": "Apa Itu Flow Control (Jaringan)?", "id": "Mengontrol Kecepatan Pengiriman Data." },
{ "en": "Apa Itu Error Control (Jaringan)?", "id": "Mendeteksi Memperbaiki Kesalahan Data." },
{ "en": "Apa Itu Congestion Control?", "id": "Mengatasi Kemacetan Lalu Lintas Jaringan." },
{ "en": "Apa Itu Circuit Switching?", "id": "Metode Koneksi Jaringan Jalur Khusus." },
{ "en": "Apa Itu Packet Switching?", "id": "Metode Koneksi Jaringan Berbasis Paket." },
{ "en": "Apa Itu Denial of Service (DoS)?", "id": "Serangan Menghabiskan Sumber Daya Server." },
{ "en": "Apa Itu Command 'netstat'?", "id": "Menampilkan Statistik Koneksi Jaringan." },
{ "en": "Apa Itu Command 'nslookup'?", "id": "Mencari Informasi DNS Server." },
{ "en": "Apa Itu DNS Spoofing?", "id": "Pemalsuan Data DNS Server." },
{ "en": "Apa Itu ARP (Address Resolution Protocol)?", "id": "Protokol Pemetaan IP Ke MAC Address." },
{ "en": "Apa Itu ARP Spoofing?", "id": "Pemalsuan Data Protokol ARP." },
{ "en": "Apa Itu DHCP Snooping?", "id": "Keamanan Switch Mencegah DHCP Palsu." },
{ "en": "Apa Itu WEP (Wired Equivalent Privacy)?", "id": "Standar Keamanan Wi-Fi Lama." },
{ "en": "Apa Itu WPA (Wi-Fi Protected Access)?", "id": "Standar Keamanan Wi-Fi Pengganti WEP." },
{ "en": "Apa Itu WPA2 (Wi-Fi Protected Access 2)?", "id": "Versi WPA Lebih Aman." },
{ "en": "Apa Itu WPA3 (Wi-Fi Protected Access 3)?", "id": "Standar Keamanan Wi-Fi Terbaru." },
{ "en": "Apa Itu WPS (Wi-Fi Protected Setup)?", "id": "Fitur Koneksi Wi-Fi Mudah." },
{ "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Wi-Fi Publik." },
{ "en": "Menyembunyikan SSID Apakah Aman?", "id": "Tidak, Hanya Menambah Kesulitan Sedikit." },
{ "en": "Apa Itu MAC Filtering?", "id": "Membatasi Akses Jaringan Berdasarkan MAC." },
{ "en": "Apa Itu Evil Twin Attack?", "id": "Serangan Wi-Fi Palsu Menyerupai Asli." },
{ "en": "Apa Itu Ping of Death?", "id": "Serangan DoS Menggunakan Paket Ping." },
{ "en": "Apa Itu Smurf Attack?", "id": "Serangan DDoS Menggunakan Trafik Broadcast." },
{ "en": "Apa Itu Data Type Primitive?", "id": "Tipe Data Dasar Bawaan Bahasa." },
{ "en": "Apa Itu Data Type Composite?", "id": "Tipe Data Bentukan (Array, Object)." },
{ "en": "Apa Itu Assignment Operator?", "id": "Operator Pemberi Nilai (Contoh: =)." },
{ "en": "Apa Itu Comparison Operator?", "id": "Operator Pembanding Nilai (Contoh: ==)." },
{ "en": "Apa Itu Logical Operator?", "id": "Operator Logika (AND, OR, NOT)." },
{ "en": "Apa Itu Bitwise Operator?", "id": "Operator Operasi Level Bit." },
{ "en": "Apa Itu Increment Operator?", "id": "Operator Menambah Nilai Satu (Contoh: ++)." },
{ "en": "Apa Itu Decrement Operator?", "id": "Operator Mengurangi Nilai Satu (Contoh: --)." },
{ "en": "Apa Itu Break Statement?", "id": "Perintah Menghentikan Perulangan (Loop)." },
{ "en": "Apa Itu Continue Statement?", "id": "Perintah Melompati Iterasi Loop Saat Ini." },
{ "en": "Apa Itu Exception Handling?", "id": "Mekanisme Menangani Error Saat Runtime." },
{ "en": "Apa Itu Try-Catch Block?", "id": "Blok Kode Menangani Exception." },
{ "en": "Apa Itu Final (Pemrograman)?", "id": "Keyword Nilai Variabel Tetap." },
{ "en": "Apa Itu Static (Pemrograman)?", "id": "Keyword Milik Kelas, Bukan Objek." },
{ "en": "Apa Itu Clock Speed CPU?", "id": "Kecepatan CPU Menjalankan Instruksi." },
{ "en": "Apa Satuan Clock Speed?", "id": "Hertz (Hz), Gigahertz (GHz)." },
{ "en": "Apa Itu CPU Core?", "id": "Unit Pemroses Independen Dalam CPU." },
{ "en": "Apa Itu Multi-Core Processor?", "id": "CPU Memiliki Dua Core Lebih." },
{ "en": "Apa Itu CPU Cache?", "id": "Memori Super Cepat Didalam CPU." },
{ "en": "Apa Itu L1 Cache?", "id": "Cache Tercepat, Terkecil, Didalam Core." },
{ "en": "Apa Itu L2 Cache?", "id": "Cache Level Dua, Lebih Besar." },
{ "en": "Apa Itu L3 Cache?", "id": "Cache Level Tiga, Dibagi Antar Core." },
{ "en": "Apa Itu Bus Sistem?", "id": "Jalur Komunikasi Antar Komponen." },
{ "en": "Apa Itu Bus Data?", "id": "Jalur Bus Membawa Data." },
{ "en": "Apa Itu Bus Alamat?", "id": "Jalur Bus Membawa Alamat Memori." },
{ "en": "Apa Itu Bus Kontrol?", "id": "Jalur Bus Membawa Sinyal Kontrol." },
{ "en": "Apa Itu Throughput?", "id": "Tingkat Transfer Data Aktual." },
{ "en": "Apa Itu RAM DDR (Double Data Rate)?", "id": "Jenis RAM Transfer Data Dua Kali." },
{ "en": "Apa Itu RAM SODIMM (Small Outline Dual In-line Memory Module)?", "id": "Modul RAM Kecil Untuk Laptop." },
{ "en": "Apa Itu RAM DIMM (Dual In-line Memory Module)?", "id": "Modul RAM Standar Desktop." },
{ "en": "Apa Itu Virtualisasi Server?", "id": "Membagi Satu Server Fisik Virtual." },
{ "en": "Apa Itu Server Migration?", "id": "Proses Memindahkan Data Server." },
{ "en": "Apa Itu Server Provisioning?", "id": "Proses Menyiapkan Server Baru." },
{ "en": "Apa Itu Uptime Server?", "id": "Waktu Server Menyala Beroperasi." },
{ "en": "Apa Itu SLA (Service Level Agreement)?", "id": "Perjanjian Kontrak Layanan IT." },
{ "en": "Apa Itu KPI (Key Performance Indicator)?", "id": "Indikator Kunci Pengukur Kinerja." },
{ "en": "Apa Itu Technical Debt?", "id": "Hutang Teknis Akibat Solusi Cepat." },
{ "en": "Apa Itu Refactoring Kode?", "id": "Memperbaiki Struktur Kode Tanpa Ubah Fungsi." },
{ "en": "Apa Itu Code Smell?", "id": "Indikasi Masalah Struktur Kode." },
{ "en": "Apa Itu Prinsip DRY (Don't Repeat Yourself)?", "id": "Prinsip Hindari Duplikasi Kode." },
{ "en": "Apa Itu Prinsip KISS (Keep It Simple, Stupid)?", "id": "Prinsip Menjaga Desain Sederhana." },
{ "en": "Apa Itu Prinsip YAGNI (You Ain't Gonna Need It)?", "id": "Prinsip Jangan Tambah Fitur Sia-sia." },
{ "en": "Apa Itu Big O Notation?", "id": "Notasi Mengukur Kompleksitas Algoritma." },
{ "en": "Apa Itu Kompleksitas Waktu?", "id": "Waktu Eksekusi Algoritma Tumbuh." },
{ "en": "Apa Itu Kompleksitas Ruang?", "id": "Memori Dibutuhkan Algoritma Tumbuh." },
{ "en": "Apa Itu O(1)?", "id": "Kompleksitas Konstan, Waktu Tetap." },
{ "en": "Apa Itu O(n)?", "id": "Kompleksitas Linier, Waktu Sebanding Input." },
{ "en": "Apa Itu O(log n)?", "id": "Kompleksitas Logaritmik, Sangat Efisien." },
{ "en": "Apa Itu Heap (Struktur Data)?", "id": "Struktur Data Pohon Tertentu." },
{ "en": "Apa Itu Hash Map?", "id": "Struktur Data Pasangan Kunci-Nilai." },
{ "en": "Apa Itu Collision (Hash Map)?", "id": "Dua Kunci Menghasilkan Hash Sama." },
{ "en": "Apa Itu Algoritma Greedy?", "id": "Algoritma Ambil Pilihan Terbaik Lokal." },
{ "en": "Apa Itu Dynamic Programming?", "id": "Algoritma Pecah Masalah, Simpan Solusi." },
{ "en": "Apa Itu Brute Force Algorithm?", "id": "Algoritma Mencoba Semua Kemungkinan Solusi." },
{ "en": "Apa Itu Cyber Law?", "id": "Hukum Terkait Penggunaan Internet." },
{ "en": "Apa Itu Privasi Data?", "id": "Hak Individu Kontrol Data Pribadi." },
{ "en": "Apa Itu PII (Personally Identifiable Information)?", "id": "Informasi Identifikasi Personal Individu." },
{ "en": "Apa Itu GDPR (General Data Protection Regulation)?", "id": "Regulasi Privasi Data Uni Eropa." },
{ "en": "Apa Itu Lisensi MIT?", "id": "Lisensi Open Source Sangat Bebas." },
{ "en": "Apa Itu Lisensi Apache?", "id": "Lisensi Open Source Fleksibel." },
{ "en": "Apa Itu Public Domain?", "id": "Karya Kreatif Tanpa Hak Cipta." },
{ "en": "Apa Itu Fair Use?", "id": "Penggunaan Wajar Materi Berhak Cipta." },
{ "en": "Apa Itu Watermark Digital?", "id": "Tanda Digital Tersembunyi Identifikasi." },
{ "en": "Apa Itu Etika Komputer?", "id": "Prinsip Moral Penggunaan Komputer." },
{ "en": "Apa Itu Whistleblower (IT)?", "id": "Pelapor Pelanggaran Internal Perusahaan IT." },
{ "en": "Apa Itu HTTP Request?", "id": "Permintaan Klien Ke Server Web." },
{ "en": "Apa Itu HTTP Response?", "id": "Jawaban Server Ke Klien Web." },
{ "en": "Metode HTTP GET Gunanya?", "id": "Meminta Data Dari Server." },
{ "en": "Metode HTTP POST Gunanya?", "id": "Mengirim Data Baru Ke Server." },
{ "en": "Metode HTTP PUT Gunanya?", "id": "Memperbarui Data Di Server." },
{ "en": "Metode HTTP DELETE Gunanya?", "id": "Menghapus Data Dari Server." },
{ "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan." },
{ "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Markup Pertukaran Data." },
{ "en": "Perbedaan JSON Dan XML?", "id": "JSON Lebih Ringan, XML Lebih Verbose." },
{ "en": "Apa Itu Web Service?", "id": "Layanan Komunikasi Antar Aplikasi Web." },
{ "en": "Apa Itu REST (Representational State Transfer)?", "id": "Arsitektur Populer Web Service." },
{ "en": "Apa Itu SOAP (Simple Object Access Protocol)?", "id": "Protokol Web Service Berbasis XML." },
{ "en": "Apa Itu API Key?", "id": "Kunci Unik Otentikasi Akses API." },
{ "en": "Apa Itu API Endpoint?", "id": "URL Spesifik Akses Fungsi API." },
{ "en": "Apa Itu Sistem Monitoring?", "id": "Pemantauan Kinerja Kesehatan Sistem." },
{ "en": "Apa Itu Logging?", "id": "Pencatatan Aktivitas Kejadian Sistem." },
{ "en": "Apa Itu Alerting?", "id": "Notifikasi Otomatis Masalah Sistem." },
{ "en": "Apa Itu Dashboard Monitoring?", "id": "Tampilan Visual Data Monitoring." },
{ "en": "Apa Itu QA Engineer?", "id": "Insinyur Penjamin Kualitas Perangkat Lunak." },
{ "en": "Apa Itu DevOps?", "id": "Kolaborasi Antara Development Operations." },
{ "en": "Apa Itu CI/CD (Continuous Integration/Continuous Delivery)?", "id": "Praktik Otomatisasi Rilis Software." },
{ "en": "Apa Itu Continuous Integration?", "id": "Integrasi Kode Otomatis Sering." },
{ "en": "Apa Itu Continuous Delivery?", "id": "Pengiriman Kode Otomatis Ke Produksi." },
{ "en": "Apa Itu Scrum Master?", "id": "Fasilitator Kerangka Kerja Scrum." },
{ "en": "Apa Itu Product Owner?", "id": "Pemilik Visi Produk Proyek." },
{ "en": "Apa Itu Development Team?", "id": "Tim Pengembang Perangkat Lunak." },
{ "en": "Apa Itu Daily Stand-up?", "id": "Rapat Harian Singkat Tim Agile." },
{ "en": "Apa Itu Sprint Retrospective?", "id": "Evaluasi Tim Akhir Sprint." },
{ "en": "Apa Itu User Story?", "id": "Deskripsi Fitur Dari Sisi Pengguna." },
{ "en": "Apa Itu Acceptance Criteria?", "id": "Syarat Fitur Diterima Selesai." },
{ "en": "Apa Itu Burndown Chart?", "id": "Grafik Visual Sisa Pekerjaan." },
{ "en": "Apa Itu Backlog Produk?", "id": "Daftar Semua Fitur Kebutuhan Produk." },
{ "en": "Apa Itu Sprint Backlog?", "id": "Daftar Tugas Sprint Saat Ini." },
{ "en": "Apa Itu Agile Manifesto?", "id": "Deklarasi Nilai Prinsip Agile." },
{ "en": "Apa Itu Pair Programming?", "id": "Dua Programmer Bekerja Satu Komputer." },
{ "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Pengembangan Berbasis Tes Terlebih Dahulu." },
{ "en": "Apa Itu Behavior-Driven Development (BDD)?", "id": "Pengembangan Berbasis Perilaku Pengguna." },
{ "en": "Apa Itu User Persona?", "id": "Representasi Fiksi Target Pengguna." },
{ "en": "Apa Itu A/B Testing?", "id": "Pengujian Dua Versi Halaman Web." },
{ "en": "Apa Itu Heatmap (UX)?", "id": "Visualisasi Area Interaksi Pengguna." },
{ "en": "Apa Itu Card Sorting?", "id": "Metode Riset UX Struktur Informasi." },
{ "en": "Apa Itu Information Architecture (IA)?", "id": "Struktur Organisasi Konten Informasi." },
{ "en": "Apa Itu Low-Fidelity Prototype?", "id": "Prototipe Awal Sederhana (Sketsa)." },
{ "en": "Apa Itu High-Fidelity Prototype?", "id": "Prototipe Detail Mirip Produk Akhir." },
{ "en": "Apa Itu Mobile-First Design?", "id": "Desain Mulai Dari Tampilan Mobile." },
{ "en": "Apa Itu Responsive Design?", "id": "Desain Web Adaptif Ukuran Layar." },
{ "en": "Apa Itu Progressive Web App (PWA)?", "id": "Aplikasi Web Mirip Aplikasi Native." },
{ "en": "Apa Itu Native Application?", "id": "Aplikasi Dibangun Khusus Platform OS." },
{ "en": "Apa Itu Hybrid Application?", "id": "Aplikasi Gabungan Teknologi Web Native." },
{ "en": "Apa Itu Cross-Platform Development?", "id": "Pengembangan Aplikasi Multi Platform." },
{ "en": "Apa Itu Bahasa Pemrograman Swift?", "id": "Bahasa Pemrograman Apple (iOS, MacOS)." },
{ "en": "Apa Itu Bahasa Pemrograman Kotlin?", "id": "Bahasa Pemrograman Modern Android." },
{ "en": "Apa Itu .NET Framework?", "id": "Framework Pengembangan Perangkat Lunak Microsoft." },
{ "en": "Apa Itu Node.js?", "id": "Lingkungan Runtime JavaScript Sisi Server." },
{ "en": "Apa Itu NPM (Node Package Manager)?", "id": "Manajer Paket Standar Node.js." },
{ "en": "Apa Itu Ruby on Rails?", "id": "Framework Aplikasi Web Bahasa Ruby." },
{ "en": "Apa Itu Django?", "id": "Framework Web Python Level Tinggi." },
{ "en": "Apa Itu Flask?", "id": "Micro-Framework Web Bahasa Python." },
{ "en": "Apa Itu Laravel?", "id": "Framework Aplikasi Web Bahasa PHP." },
{ "en": "Apa Itu Spring Framework?", "id": "Framework Aplikasi Populer Bahasa Java." },
{ "en": "Apa Itu Angular?", "id": "Framework Frontend Berbasis TypeScript." },
{ "en": "Apa Itu React?", "id": "Library JavaScript UI Dari Facebook." },
{ "en": "Apa Itu Vue.js?", "id": "Framework JavaScript Progresif Frontend." },
{ "en": "Apa Itu TypeScript?", "id": "Superset JavaScript Dengan Tipe Statis." },
{ "en": "Apa Itu AJAX (Asynchronous JavaScript and XML)?", "id": "Teknik Update Halaman Web Asinkron." },
{ "en": "Apa Itu DOM (Document Object Model)?", "id": "Representasi Struktur Halaman Web." },
{ "en": "Apa Itu Virtual DOM?", "id": "Representasi DOM Virtual Dalam Memori." },
{ "en": "Apa Itu Git Repository Lokal?", "id": "Repository Git Di Komputer Pengguna." },
{ "en": "Apa Itu Git Repository Remote?", "id": "Repository Git Disimpan Server (GitHub)." },
{ "en": "Apa Itu GitHub?", "id": "Platform Hosting Git Populer." },
{ "en": "Apa Itu GitLab?", "id": "Alternatif Platform Hosting Git." },
{ "en": "Apa Itu Bitbucket?", "id": "Platform Hosting Git Milik Atlassian." },
{ "en": "Apa Itu Perintah 'git clone'?", "id": "Menyalin Repository Remote Ke Lokal." },
{ "en": "Apa Itu Perintah 'git push'?", "id": "Mengirim Commit Lokal Ke Remote." },
{ "en": "Apa Itu Perintah 'git pull'?", "id": "Mengambil Perubahan Remote Ke Lokal." },
{ "en": "Apa Itu Perintah 'git status'?", "id": "Menampilkan Status Perubahan Repository." },
{ "en": "Apa Itu Perintah 'git add'?", "id": "Menambahkan Perubahan Ke Staging Area." },
{ "en": "Apa Itu Staging Area Git?", "id": "Tempat Persiapan File Sebelum Commit." },
{ "en": "Apa Itu Branch 'master'/'main'?", "id": "Cabang Utama Pengembangan Kode." },
{ "en": "Apa Itu Merge Conflict?", "id": "Konflik Saat Menggabungkan Branch." },
{ "en": "Apa Itu Pull Request?", "id": "Permintaan Menggabungkan Kode Ke Branch." },
{ "en": "Apa Itu Fork (Git)?", "id": "Salinan Pribadi Repository Orang Lain." },
{ "en": "Apa Itu IT Governance?", "id": "Tata Kelola Penggunaan TI Organisasi." },
{ "en": "Apa Itu COBIT (Control Objectives for Information and Related Technology)?", "id": "Kerangka Kerja Tata Kelola TI." },
{ "en": "Apa Itu ITIL (Information Technology Infrastructure Library)?", "id": "Kerangka Kerja Manajemen Layanan TI." },
{ "en": "Apa Itu Incident Management?", "id": "Manajemen Penanganan Insiden Gangguan." },
{ "en": "Apa Itu Problem Management?", "id": "Manajemen Pencarian Akar Masalah." },
{ "en": "Apa Itu Change Management?", "id": "Manajemen Kontrol Perubahan Sistem TI." },
{ "en": "Apa Itu Service Desk?", "id": "Titik Kontak Layanan Dukungan TI." },
{ "en": "Apa Itu Business Continuity Plan (BCP)?", "id": "Rencana Keberlangsungan Bisnis Saat Bencana." },
{ "en": "Apa Itu Disaster Recovery Plan (DRP)?", "id": "Rencana Pemulihan Sistem Pasca Bencana." },
{ "en": "Apa Itu RPO (Recovery Point Objective)?", "id": "Batas Toleransi Kehilangan Data." },
{ "en": "Apa Itu RTO (Recovery Time Objective)?", "id": "Batas Waktu Pemulihan Sistem." },
{ "en": "Apa Itu High Availability (HA)?", "id": "Sistem Ketersediaan Tinggi, Minim Downtime." },
{ "en": "Apa Itu Failover?", "id": "Pengalihan Otomatis Ke Sistem Cadangan." },
{ "en": "Apa Itu Business Logic?", "id": "Logika Aturan Bisnis Aplikasi." },
{ "en": "Apa Itu Presentation Layer?", "id": "Lapis Tampilan Antarmuka Pengguna." },
{ "en": "Apa Itu Data Access Layer?", "id": "Lapis Pengelolaan Akses Data." },
{ "en": "Apa Itu Arsitektur Three-Tier?", "id": "Arsitektur (Presentation, Logic, Data)." },
{ "en": "Apa Itu Arsitektur Microservices?", "id": "Arsitektur Aplikasi Pecahan Layanan Kecil." },
{ "en": "Apa Itu Arsitektur Monolithic?", "id": "Arsitektur Aplikasi Tunggal Terintegrasi." },
{ "en": "Apa Itu Serverless Computing?", "id": "Model Eksekusi Kode Tanpa Server." },
{ "en": "Apa Itu Function as a Service (FaaS)?", "id": "Layanan Cloud Menjalankan Fungsi (Serverless)." },
{ "en": "Apa Itu Edge Computing?", "id": "Komputasi Dekat Sumber Data." },
{ "en": "Apa Itu Laten Jaringan?", "id": "Waktu Tunda Perjalanan Data." },
{ "en": "Apa Itu Jitter Jaringan?", "id": "Variasi Waktu Tunda Paket." },
{ "en": "Apa Itu Packet Loss?", "id": "Paket Data Hilang Transmisi." },
{ "en": "Apa Itu Quality of Service (QoS)?", "id": "Manajemen Kualitas Kinerja Jaringan." },
{ "en": "Apa Itu Digital Literacy?", "id": "Kemampuan Menggunakan Teknologi Digital." },
{ "en": "Apa Itu Software Bloat?", "id": "Aplikasi Terlalu Besar Banyak Fitur." },
{ "en": "Apa Itu Green Computing?", "id": "Praktik TI Ramah Lingkungan." },
{ "en": "Apa Itu Power User?", "id": "Pengguna Mahir Fitur Lanjut." },
{ "en": "Apa Itu Beta Tester?", "id": "Penguji Eksternal Versi Beta." },
{ "en": "Apa Itu Early Adopter?", "id": "Pengguna Awal Teknologi Baru." },
{ "en": "Apa Itu Single Sign-On (SSO)?", "id": "Satu Kali Login Akses Aplikasi." },
{ "en": "Apa Itu Identity and Access Management (IAM)?", "id": "Manajemen Identitas Hak Akses." },
{ "en": "Apa Itu Multi-Factor Authentication (MFA)?", "id": "Otentikasi Multi Lapis Keamanan." },
{ "en": "Apa Itu Security Token?", "id": "Perangkat Keras Kunci Keamanan." },
{ "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Kontrol Izin Akses." },
{ "en": "Apa Itu Role-Based Access Control (RBAC)?", "id": "Kontrol Akses Berbasis Peran." },
{ "en": "Apa Itu Audit Trail?", "id": "Jejak Rekaman Aktivitas Sistem." },
{ "en": "Apa Itu Software Escrow?", "id": "Penyimpanan Source Code Pihak Ketiga." },
{ "en": "Apa Itu Service-Oriented Architecture (SOA)?", "id": "Arsitektur Berbasis Layanan Terdistribusi." },
{ "en": "Apa Itu Enterprise Application Integration (EAI)?", "id": "Integrasi Antar Aplikasi Perusahaan." },
{ "en": "Apa Itu Middleware?", "id": "Perangkat Lunak Perantara Aplikasi." },
{ "en": "Apa Itu Message Queue?", "id": "Antrean Pesan Komunikasi Asinkron." },
{ "en": "Apa Itu Publish-Subscribe Pattern?", "id": "Pola Komunikasi (Publisher, Subscriber)." },
{ "en": "Apa Itu Command 'chmod' (Linux)?", "id": "Perintah Mengubah Izin File." },
{ "en": "Apa Itu Command 'chown' (Linux)?", "id": "Perintah Mengubah Pemilik File." },
{ "en": "Apa Itu Command 'grep' (Linux)?", "id": "Perintah Mencari Teks File." },
{ "en": "Apa Itu Command 'find' (Linux)?", "id": "Perintah Mencari File Direktori." },
{ "en": "Apa Itu Command 'ps' (Linux)?", "id": "Perintah Menampilkan Proses Berjalan." },
{ "en": "Apa Itu Command 'kill' (Linux)?", "id": "Perintah Menghentikan Proses Paksa." },
{ "en": "Apa Itu Command 'tar' (Linux)?", "id": "Perintah Membuat Mengestrak Arsip." },
{ "en": "Apa Itu Command 'ssh' (Linux)?", "id": "Perintah Koneksi Remote Secure Shell." },
{ "en": "Apa Itu Paging (Memori)?", "id": "Mekanisme Manajemen Memori Virtual." },
{ "en": "Apa Itu Segmentation (Memori)?", "id": "Pembagian Memori Berbasis Segmen." },
{ "en": "Apa Itu Context Switching?", "id": "Proses Pengalihan CPU Antar Proses." },
{ "en": "Apa Itu Deadlock?", "id": "Kondisi Dua Proses Saling Tunggu." },
{ "en": "Apa Itu Race Condition?", "id": "Kondisi Hasil Tergantung Urutan Eksekusi." },
{ "en": "Apa Itu Semaphore?", "id": "Variabel Kontrol Akses Sumber Daya." },
{ "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Pencegahan Akses Bersamaan." },
{ "en": "Apa Itu Scheduler (OS)?", "id": "Komponen OS Penjadwal Proses." },
{ "en": "Apa Itu Time-Sharing System?", "id": "Sistem Operasi Berbagi Waktu CPU." },
{ "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Respon Waktu Nyata." },
{ "en": "Apa Itu Batch Processing?", "id": "Eksekusi Tugas Terjadwal Tanpa Intervensi." },
{ "en": "Apa Itu Interactive Processing?", "id": "Pemrosesan Membutuhkan Interaksi Pengguna." },
{ "en": "Apa Itu Real-Time Processing?", "id": "Pemrosesan Data Instan Saat Diterima." },
{ "en": "Apa Itu Distributed System?", "id": "Sistem Komponen Tersebar Jaringan." },
{ "en": "Apa Itu Centralized System?", "id": "Sistem Kontrol Terpusat Satu Lokasi." },
{ "en": "Apa Itu Decentralized System?", "id": "Sistem Tanpa Otoritas Kontrol Pusat." },
{ "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Tetap Operasi Saat Gagal." },
{ "en": "Apa Itu Redundansi Hardware?", "id": "Duplikasi Komponen Perangkat Keras Cadangan." },
{ "en": "Apa Itu Redundansi Software?", "id": "Duplikasi Logika Perangkat Lunak Cadangan." },
{ "en": "Apa Itu Scalability Vertikal (Scale Up)?", "id": "Menambah Kapasitas Satu Server." },
{ "en": "Apa Itu Scalability Horizontal (Scale Out)?", "id": "Menambah Jumlah Server Sistem." },
{ "en": "Apa Itu Elasticity (Cloud)?", "id": "Kemampuan Skala Otomatis Sesuai Kebutuhan." },
{ "en": "Apa Itu Vendor Lock-In?", "id": "Ketergantungan Tinggi Pada Satu Vendor." },
{ "en": "Apa Itu Open Standard?", "id": "Standar Teknologi Terbuka Publik." },
{ "en": "Apa Itu Proprietary Standard?", "id": "Standar Teknologi Tertutup Milik Perusahaan." },
{ "en": "Apa Itu BYOD (Bring Your Own Device)?", "id": "Bawa Perangkat Pribadi Kerja." },
{ "en": "Apa Itu Mobile Device Management (MDM)?", "id": "Manajemen Keamanan Perangkat Mobile Perusahaan." },
{ "en": "Apa Itu Data Sanitization?", "id": "Proses Menghapus Data Aman Permanen." },
{ "en": "Apa Itu Data Erasure?", "id": "Penghapusan Data Tidak Bisa Dipulihkan." },
{ "en": "Apa Itu Data Degaussing?", "id": "Menghapus Data Media Magnetik." },
{ "en": "Apa Itu Physical Destruction?", "id": "Penghancuran Fisik Media Penyimpanan." },
{ "en": "Apa Itu Software License Key?", "id": "Kode Aktivasi Perangkat Lunak." },
{ "en": "Apa Itu Volume License?", "id": "Lisensi Perangkat Lunak Jumlah Besar." },
{ "en": "Apa Itu Site License?", "id": "Lisensi Perangkat Lunak Satu Lokasi." },
{ "en": "Apa Itu Concurrent License?", "id": "Lisensi Berbasis Jumlah Pengguna Bersamaan." },
{ "en": "Apa Itu Per-User License?", "id": "Lisensi Dihitung Per Pengguna Individu." },
{ "en": "Apa Itu Software Audit?", "id": "Audit Kepatuhan Lisensi Perangkat Lunak." },
{ "en": "Apa Itu Digital Rights Management (DRM)?", "id": "Manajemen Hak Digital Konten." },
{ "en": "Apa Itu Watermark (Digital)?", "id": "Penanda Digital Kepemilikan Konten." },
{ "en": "Apa Itu Deep Learning?", "id": "Cabang Machine Learning Jaringan Saraf." },
{ "en": "Apa Itu Neural Network?", "id": "Jaringan Saraf Tiruan Model Otak." },
{ "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Pemrosesan Bahasa Alami Manusia." },
{ "en": "Apa Itu Computer Vision?", "id": "Visi Komputer Menganalisis Gambar." },
{ "en": "Apa Itu Chatbot?", "id": "Program Simulasi Percakapan Manusia." },
{ "en": "Apa Itu Virtual Assistant?", "id": "Asisten Virtual (Siri, Alexa)." },
{ "en": "Apa Itu Robotics?", "id": "Bidang Ilmu Teknologi Robot." },
{ "en": "Apa Itu Automation?", "id": "Otomatisasi Proses Tugas Manusia." },
{ "en": "Apa Itu Business Process Automation (BPA)?", "id": "Otomatisasi Proses Bisnis Rutin." },
{ "en": "Apa Itu Robotic Process Automation (RPA)?", "id": "Otomatisasi Tugas Menggunakan Bot Software." },
{ "en": "Apa Itu Command 'pwd' (Linux)?", "id": "Menampilkan Direktori Kerja Saat Ini." },
{ "en": "Apa Itu Command 'mv' (Linux)?", "id": "Memindahkan Atau Mengganti Nama File." },
{ "en": "Apa Itu Command 'cp' (Linux)?", "id": "Menyalin File Atau Direktori." },
{ "en": "Apa Itu Command 'rm' (Linux)?", "id": "Menghapus File Atau Direktori." },
{ "en": "Apa Itu Command 'man' (Linux)?", "id": "Menampilkan Manual Bantuan Perintah." },
{ "en": "Apa Itu Command 'clear' (CLI)?", "id": "Membersihkan Tampilan Layar Terminal." },
{ "en": "Apa Itu Wildcard (CLI)?", "id": "Karakter Khusus (Asterisk) Pencarian." },
{ "en": "Apa Itu I/O (Input/Output)?", "id": "Operasi Masukan Dan Keluaran." },
{ "en": "Apa Itu I/O Redirection?", "id": "Pengalihan Aliran Input Output." },
{ "en": "Apa Itu Pipe (CLI)?", "id": "Menyalurkan Output Satu Perintah Input." },
{ "en": "Apa Itu Shell Script?", "id": "Skrip Berisi Kumpulan Perintah Shell." },
{ "en": "Apa Itu Environment Variable?", "id": "Variabel Dinamis Lingkungan Sistem Operasi." },
{ "en": "Apa Itu $PATH Variable?", "id": "Variabel Lingkungan Lokasi Perintah." },
{ "en": "Apa Itu Root (Superuser)?", "id": "Akun Administrator Tertinggi Linux." },
{ "en": "Apa Itu Sudo (Superuser Do)?", "id": "Perintah Eksekusi Hak Superuser." },
{ "en": "Apa Itu File Permission 777?", "id": "Izin Penuh Baca Tulis Eksekusi." },
{ "en": "Apa Itu File Permission 644?", "id": "Izin Baca Tulis Pemilik, Baca Publik." },
{ "en": "Apa Itu Text Editor?", "id": "Program Mengedit File Teks Murni." },
{ "en": "Contoh Text Editor CLI?", "id": "Vim, Nano." },
{ "en": "Contoh Text Editor GUI?", "id": "Notepad++, VS Code." },
{ "en": "Apa Itu Source Control?", "id": "Sistem Manajemen Perubahan Kode." },
{ "en": "Apa Itu Centralized Version Control?", "id": "Kontrol Versi Terpusat Satu Server." },
{ "en": "Apa Itu Distributed Version Control?", "id": "Kontrol Versi Terdistribusi (Git)." },
{ "en": "Apa Itu LAMP Stack?", "id": "Linux, Apache, MySQL, PHP." },
{ "en": "Apa Itu MEAN Stack?", "id": "MongoDB, Express.js, Angular, Node.js." },
{ "en": "Apa Itu MERN Stack?", "id": "MongoDB, Express.js, React, Node.js." },
{ "en": "Apa Itu Full Stack Developer?", "id": "Pengembang Menguasai Frontend Backend." },
{ "en": "Apa Itu T-Shaped Skill?", "id": "Ahli Satu Bidang, Paham Bidang Lain." },
{ "en": "Apa Itu Imposter Syndrome?", "id": "Sindrom Merasa Tidak Kompeten." },
{ "en": "Apa Itu Rubber Duck Debugging?", "id": "Menjelaskan Kode Ke Objek Mati." },
{ "en": "Apa Itu Code Review?", "id": "Tinjauan Kode Oleh Rekan Setim." },
{ "en": "Apa Itu Static Code Analysis?", "id": "Analisis Kode Otomatis Tanpa Eksekusi." },
{ "en": "Apa Itu Linter?", "id": "Alat Pengecek Gaya Kode." },
{ "en": "Apa Itu Formatter?", "id": "Alat Perapi Format Kode Otomatis." },
{ "en": "Apa Itu Dependency (Software)?", "id": "Ketergantungan Proyek Pada Pustaka Lain." },
{ "en": "Apa Itu Dependency Management?", "id": "Manajemen Pustaka Eksternal Proyek." },
{ "en": "Apa Itu Dependency Hell?", "id": "Masalah Konflik Ketergantungan Pustaka." },
{ "en": "Apa Itu Build Automation?", "id": "Otomatisasi Proses Kompilasi Kode." },
{ "en": "Apa Itu Build Tool?", "id": "Alat Otomatisasi Build (Maven, Gradle)." },
{ "en": "Apa Itu Task Runner?", "id": "Alat Otomatisasi Tugas (Gulp, Webpack)." },
{ "en": "Apa Itu Web Application?", "id": "Aplikasi Diakses Melalui Web Browser." },
{ "en": "Apa Itu Desktop Application?", "id": "Aplikasi Berjalan Di Sistem Operasi." },
{ "en": "Apa Itu Client-Side Rendering (CSR)?", "id": "Rendering Halaman Di Sisi Browser." },
{ "en": "Apa Itu Server-Side Rendering (SSR)?", "id": "Rendering Halaman Di Sisi Server." },
{ "en": "Apa Itu Single Page Application (SPA)?", "id": "Aplikasi Web Halaman Tunggal Dinamis." },
{ "en": "Apa Itu Multi-Page Application (MPA)?", "id": "Aplikasi Web Tradisional Multi Halaman." },
{ "en": "Apa Itu Progressive Enhancement?", "id": "Desain Web Fokus Konten Dasar." },
{ "en": "Apa Itu Graceful Degradation?", "id": "Desain Web Fokus Fitur Penuh." },
{ "en": "Apa Itu Pixel Perfect Design?", "id": "Implementasi Desain Presisi Tinggi." },
{ "en": "Apa Itu Browser Compatibility?", "id": "Kompatibilitas Tampilan Web Antar Browser." },
{ "en": "Apa Itu Transpiler?", "id": "Penerjemah Kode Ke Versi Lain." },
{ "en": "Apa Itu Minification?", "id": "Proses Mengecilkan Ukuran Kode." },
{ "en": "Apa Itu Bundler?", "id": "Penggabung File Kode (Webpack)." },
{ "en": "Apa Itu Content Delivery Network (CDN)?", "id": "Jaringan Distribusi Konten Cache Global." },
{ "en": "Apa Itu Hot Module Replacement (HMR)?", "id": "Memperbarui Modul Tanpa Refresh Halaman." },
{ "en": "Apa Itu Tree Shaking?", "id": "Proses Menghapus Kode Tidak Terpakai." },
{ "en": "Apa Itu Lazy Loading?", "id": "Memuat Sumber Daya Saat Dibutuhkan." },
{ "en": "Apa Itu Preloading?", "id": "Memuat Aset Prioritas Tinggi Dahulu." },
{ "en": "Apa Itu Prefetching?", "id": "Memuat Aset Kemungkinan Dibutuhkan Nanti." },
{ "en": "Apa Itu ACID (Atomicity, Consistency, Isolation, Durability)?", "id": "Properti Transaksi Database Handal." },
{ "en": "Apa Itu Atomicity (ACID)?", "id": "Transaksi Berhasil Penuh Atau Gagal." },
{ "en": "Apa Itu Consistency (ACID)?", "id": "Data Tetap Valid Setelah Transaksi." },
{ "en": "Apa Itu Isolation (ACID)?", "id": "Transaksi Berjalan Independen Terisolasi." },
{ "en": "Apa Itu Durability (ACID)?", "id": "Perubahan Data Tersimpan Permanen." },
{ "en": "Apa Itu Transaksi Database?", "id": "Satu Unit Pekerjaan Logis Database." },
{ "en": "Apa Itu 1NF (First Normal Form)?", "id": "Bentuk Normal Pertama Database." },
{ "en": "Apa Itu 2NF (Second Normal Form)?", "id": "Bentuk Normal Kedua Database." },
{ "en": "Apa Itu 3NF (Third Normal Form)?", "id": "Bentuk Normal Ketiga Database." },
{ "en": "Apa Itu BCNF (Boyce-Codd Normal Form)?", "id": "Bentuk Normal Lebih Ketat 3NF." },
{ "en": "Apa Itu Denormalisasi?", "id": "Proses Menambah Redundansi Sengaja." },
{ "en": "Apa Itu Data Anomaly?", "id": "Inkonsistensi Data Akibat Redundansi." },
{ "en": "Apa Itu Hashing Algorithm?", "id": "Fungsi Mengubah Data Nilai Hash." },
{ "en": "Contoh Hashing Algorithm?", "id": "MD5, SHA-1, SHA-256." },
{ "en": "Apa Itu MD5 (Message Digest 5)?", "id": "Algoritma Hash 128-bit Lama." },
{ "en": "Apa Itu SHA (Secure Hash Algorithm)?", "id": "Keluarga Algoritma Hash Kriptografi." },
{ "en": "Apa Itu Salt (Kriptografi)?", "id": "Data Acak Tambahan Hash Password." },
{ "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Infrastruktur Pengelola Kunci Publik." },
{ "en": "Apa Itu Enkripsi Simetris?", "id": "Enkripsi Menggunakan Satu Kunci Sama." },
{ "en": "Apa Itu Enkripsi Asimetris?", "id": "Enkripsi Menggunakan Kunci Publik Privat." },
{ "en": "Apa Itu Model Spiral (SDLC)?", "id": "Model SDLC Berbasis Risiko Iteratif." },
{ "en": "Apa Itu Model V (SDLC)?", "id": "Model SDLC Verifikasi Validasi." },
{ "en": "Apa Itu Prototyping Model (SDLC)?", "id": "Model SDLC Membuat Prototipe Awal." },
{ "en": "Apa Itu RAD (Rapid Application Development)?", "id": "Model Pengembangan Aplikasi Cepat." },
{ "en": "Apa Itu Kode Etik Profesional IT?", "id": "Panduan Standar Perilaku Profesional IT." },
{ "en": "Apa Itu ACM (Association for Computing Machinery)?", "id": "Organisasi Profesional Bidang Komputasi." },
{ "en": "Apa Itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Teknik Global." },
{ "en": "Apa Itu Plagiarisme Kode?", "id": "Menjiplak Kode Program Orang Lain." },
{ "en": "Apa Itu Intellectual Property (IP)?", "id": "Hak Kekayaan Intelektual Karya Cipta." },
{ "en": "Apa Itu Copyright Perangkat Lunak?", "id": "Hak Cipta Legal Perangkat Lunak." },
{ "en": "Apa Itu Paten Perangkat Lunak?", "id": "Paten Inovasi Fungsi Perangkat Lunak." },
{ "en": "Apa Itu Trade Secret?", "id": "Informasi Rahasia Bisnis Perusahaan." },
{ "en": "Apa Itu Lisensi Permissive?", "id": "Lisensi Open Source Longgar (MIT)." },
{ "en": "Apa Itu Lisensi Copyleft?", "id": "Lisensi Open Source Ketat (GPL)." },
{ "en": "Apa Itu HFS+ (Hierarchical File System Plus)?", "id": "File System MacOS Versi Lama." },
{ "en": "Apa Itu APFS (Apple File System)?", "id": "File System Apple Modern (SSD)." },
{ "en": "Apa Itu Journaling (File System)?", "id": "Fitur Pencatatan Perubahan File System." },
{ "en": "Apa Itu Binary Tree?", "id": "Struktur Data Pohon Maksimal Dua Anak." },
{ "en": "Apa Itu Binary Search Tree (BST)?", "id": "Pohon Biner Pencarian Terurut Efisien." },
{ "en": "Apa Itu Root Node (Pohon)?", "id": "Node Paling Atas Struktur Pohon." },
{ "en": "Apa Itu Leaf Node (Pohon)?", "id": "Node Paling Bawah, Tanpa Anak." },
{ "en": "Apa Itu Parent Node (Pohon)?", "id": "Node Induk Dari Node Lain." },
{ "en": "Apa Itu Child Node (Pohon)?", "id": "Node Anak Dari Node Lain." },
{ "en": "Apa Itu Graph (Struktur Data)?", "id": "Kumpulan Node (Vertices) Dihubungkan Edge." },
{ "en": "Apa Itu Directed Graph?", "id": "Graf Dengan Edge Berarah." },
{ "en": "Apa Itu Undirected Graph?", "id": "Graf Dengan Edge Tidak Berarah." },
{ "en": "Apa Itu Weighted Graph?", "id": "Graf Edge Memiliki Nilai Bobot." },
{ "en": "Apa Itu Multi-Cloud?", "id": "Strategi Menggunakan Beberapa Penyedia Cloud." },
{ "en": "Apa Itu Community Cloud?", "id": "Cloud Dibagi Antar Organisasi Komunitas." },
{ "en": "Apa Itu Cloud Bursting?", "id": "Migrasi Beban Ke Public Cloud." },
{ "en": "Apa Itu Cloud Orchestration?", "id": "Otomatisasi Manajemen Layanan Cloud Kompleks." },
{ "en": "Apa Itu Service Discovery?", "id": "Penemuan Lokasi Layanan Jaringan Otomatis." },
{ "en": "Apa Itu BGP (Border Gateway Protocol)?", "id": "Protokol Routing Eksterior Inti Internet." },
{ "en": "Apa Itu OSPF (Open Shortest Path First)?", "id": "Protokol Routing Interior Link-State." },
{ "en": "Apa Itu RIP (Routing Information Protocol)?", "id": "Protokol Routing Interior Distance-Vector Lama." },
{ "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Protokol Routing Cisco Propietary." },
{ "en": "Apa Itu Autonomous System (AS)?", "id": "Kumpulan Jaringan IP Satu Administrasi." },
{ "en": "Apa Itu Port 20?", "id": "Port Standar Protokol FTP (Data)." },
{ "en": "Apa Itu Port 23?", "id": "Port Standar Protokol Telnet (Tidak Aman)." },
{ "en": "Apa Itu Port 8080?", "id": "Port Alternatif Umum HTTP Proxy." },
{ "en": "Apa Itu Port 3306?", "id": "Port Standar Default Database MySQL." },
{ "en": "Apa Itu Port 5432?", "id": "Port Standar Default Database PostgreSQL." },
{ "en": "Apa Itu Port 1433?", "id": "Port Standar Default Database SQL Server." },
{ "en": "Apa Itu USB Type-A?", "id": "Konektor USB Standar Bentuk Persegi Panjang." },
{ "en": "Apa Itu USB Type-B?", "id": "Konektor USB (Printer, Perangkat Besar)." },
{ "en": "Apa Itu USB Type-C?", "id": "Konektor USB Oval Reversibel Modern." },
{ "en": "Apa Itu Micro USB?", "id": "Konektor USB Kecil Ponsel Lama." },
{ "en": "Apa Itu Mini USB?", "id": "Konektor USB Lebih Kecil Type-B." },
{ "en": "Apa Itu Thunderbolt Port?", "id": "Port Transfer Data Cepat Intel/Apple." },
{ "en": "Apa Itu Port VGA (Video Graphics Array)?", "id": "Port Video Analog 15-Pin Biru." },
{ "en": "Apa Itu Port DVI (Digital Visual Interface)?", "id": "Port Video Digital/Analog Putih." },
{ "en": "Apa Itu Port HDMI (High-Definition Multimedia Interface)?", "id": "Port Video Audio Digital Umum." },
{ "en": "Apa Itu DisplayPort?", "id": "Port Video Audio Digital PC Monitor." },
{ "en": "Apa Itu Port Ethernet (RJ45)?", "id": "Port Koneksi Jaringan Kabel LAN." },
{ "en": "Apa Itu Audio Jack 3.5mm?", "id": "Port Standar Headphone Mikrofon." },
{ "en": "Apa Itu Port SATA (Serial ATA)?", "id": "Port Koneksi Internal Hard Drive." },
{ "en": "Apa Itu eSATA (External SATA)?", "id": "Port SATA Eksternal Perangkat Penyimpanan." },
{ "en": "Apa Itu Port IDE (Integrated Drive Electronics)?", "id": "Konektor Drive Paralel Lebar Lama." },
{ "en": "Apa Itu Data Bus?", "id": "Jalur Fisik Transfer Data Antar Komponen." },
{ "en": "Apa Itu Address Bus?", "id": "Jalur Fisik Membawa Alamat Memori." },
{ "en": "Apa Itu Control Bus?", "id": "Jalur Fisik Membawa Sinyal Kontrol." },
{ "en": "Apa Itu Bus Width?", "id": "Lebar Jalur Data Bus (Bit)." },
{ "en": "Apa Itu Bus Speed?", "id": "Kecepatan Transfer Data Bus (MHz)." },
{ "en": "Apa Itu Bus PCI (Peripheral Component Interconnect)?", "id": "Bus Standar Koneksi Periferal Lama." },
{ "en": "Apa Itu Bus PCIe (PCI Express)?", "id": "Bus Koneksi Serial Cepat Modern." },
{ "en": "Apa Itu AGP (Accelerated Graphics Port)?", "id": "Port Bus Grafis Khusus Lama." },
{ "en": "Apa Itu Hot Plug?", "id": "Memasang Mencabut Perangkat Saat Menyala." },
{ "en": "Apa Itu Plug and Play (PnP)?", "id": "Deteksi Konfigurasi Perangkat Keras Otomatis." },
{ "en": "Apa Itu Pembaruan Firmware?", "id": "Memperbarui Perangkat Lunak Tertanam Hardware." },
{ "en": "Apa Itu Software Patch?", "id": "Perbaikan Kode Perangkat Lunak Spesifik." },
{ "en": "Apa Itu Hotfix?", "id": "Patch Cepat Perbaikan Isu Kritis." },
{ "en": "Apa Itu Service Pack?", "id": "Kumpulan Patch Update Perangkat Lunak." },
{ "en": "Apa Itu Rollback?", "id": "Mengembalikan Sistem Ke Versi Stabil." },
{ "en": "Apa Itu Software Bloat?", "id": "Aplikasi Terlalu Besar Boros Sumber Daya." },
{ "en": "Apa Itu Feature Creep?", "id": "Penambahan Fitur Terus Menerus." },
{ "en": "Apa Itu Data Cleansing?", "id": "Bersihkan Data Kotor Tidak Konsisten." },
{ "en": "Apa Itu Data Transformation?", "id": "Mengubah Format Struktur Data." },
{ "en": "Apa Itu Data Analysis?", "id": "Proses Menganalisis Data Informasi." },
{ "en": "Apa Itu Descriptive Analysis?", "id": "Menganalisis Data Masa Lalu." },
{ "en": "Apa Itu Predictive Analysis?", "id": "Memprediksi Tren Masa Depan." },
{ "en": "Apa Itu Prescriptive Analysis?", "id": "Memberi Rekomendasi Tindakan Terbaik." },
{ "en": "Apa Itu Stakeholder Proyek?", "id": "Pihak Berkepentingan Dalam Proyek." },
{ "en": "Apa Itu Gantt Chart?", "id": "Bagan Visualisasi Jadwal Proyek." },
{ "en": "Apa Itu Project Scope?", "id": "Batasan Pekerjaan Dalam Proyek." },
{ "en": "Apa Itu Milestone Proyek?", "id": "Tanda Pencapaian Penting Proyek." },
{ "en": "Apa Itu Project Baseline?", "id": "Rencana Awal Tolok Ukur Proyek." },
{ "en": "Apa Itu Work Breakdown Structure (WBS)?", "id": "Pemecahan Proyek Jadi Tugas Kecil." },
{ "en": "Apa Itu Risk Assessment?", "id": "Proses Identifikasi Penilaian Risiko." },
{ "en": "Apa Itu Risk Management?", "id": "Proses Mengelola Risiko Proyek." },
{ "en": "Apa Itu Business Impact Analysis (BIA)?", "id": "Analisis Dampak Gangguan Bisnis." },
{ "en": "Apa Itu Single Point of Failure (SPOF)?", "id": "Satu Titik Kegagalan Sistem." },
{ "en": "Apa Itu Redundansi Sistem?", "id": "Duplikasi Komponen Jaga Ketersediaan." },
{ "en": "Apa Itu Backup Full?", "id": "Salinan Penuh Semua Data." },
{ "en": "Apa Itu Backup Incremental?", "id": "Salinan Data Baru Sejak Backup Terakhir." },
{ "en": "Apa Itu Backup Differential?", "id": "Salinan Data Baru Sejak Backup Full." },
{ "en": "Apa Itu 3-2-1 Backup Rule?", "id": "Tiga Salinan, Dua Media, Satu Offsite." },
{ "en": "Apa Itu Hot Site (DRP)?", "id": "Situs Pemulihan Bencana Siap Pakai." },
{ "en": "Apa Itu Cold Site (DRP)?", "id": "Situs Pemulihan Bencana Infrastruktur Dasar." },
{ "en": "Apa Itu Warm Site (DRP)?", "id": "Situs Pemulihan Bencana Setengah Siap." },
{ "en": "Apa Itu Session (Web)?", "id": "Informasi Status Pengguna Di Server." },
{ "en": "Apa Itu Cookie (Web)?", "id": "Data Kecil Disimpan Di Browser Klien." },
{ "en": "Apa Itu Local Storage (Web)?", "id": "Penyimpanan Data Sisi Klien Persisten." },
{ "en": "Apa Itu Session Storage (Web)?", "id": "Penyimpanan Data Sisi Klien Per Sesi." },
{ "en": "Perbedaan Cookie Dan Session?", "id": "Cookie Di Klien, Session Server." },
{ "en": "Apa Itu JSON Web Token (JWT)?", "id": "Token Standar Otorisasi Aman." },
{ "en": "Apa Itu Authentication Token?", "id": "Token Digital Bukti Identitas Pengguna." },
{ "en": "Apa Itu Authorization Header?", "id": "Header HTTP Membawa Token Otorisasi." },
{ "en": "Apa Itu Stateless (Arsitektur)?", "id": "Server Tidak Simpan Status Klien." },
{ "en": "Apa Itu Stateful (Arsitektur)?", "id": "Server Menyimpan Status Sesi Klien." },
{ "en": "Apa Itu Supervised Learning (ML)?", "id": "Belajar Dari Data Berlabel." },
{ "en": "Apa Itu Unsupervised Learning (ML)?", "id": "Belajar Dari Data Tidak Berlabel." },
{ "en": "Apa Itu Reinforcement Learning (ML)?", "id": "Belajar Melalui Umpan Balik (Reward)." },
{ "en": "Apa Itu Model Training (ML)?", "id": "Proses Melatih Model Data." },
{ "en": "Apa Itu Dataset (ML)?", "id": "Kumpulan Data Latih Uji Model." },
{ "en": "Apa Itu Training Data?", "id": "Data Untuk Melatih Model." },
{ "en": "Apa Itu Testing Data?", "id": "Data Untuk Menguji Kinerja Model." },
{ "en": "Apa Itu Validation Data?", "id": "Data Untuk Validasi Tuning Model." },
{ "en": "Apa Itu Overfitting (ML)?", "id": "Model Hafal Data Latih." },
{ "en": "Apa Itu Underfitting (ML)?", "id": "Model Gagal Pahami Pola Data." },
{ "en": "Apa Itu Feature (ML)?", "id": "Input Variabel Untuk Model ML." },
{ "en": "Apa Itu Label (ML)?", "id": "Output Target Prediksi Model ML." },
{ "en": "Apa Itu Klasifikasi (ML)?", "id": "Model Memprediksi Kategori Diskrit." },
{ "en": "Apa Itu Regresi (ML)?", "id": "Model Memprediksi Nilai Kontinu." },
{ "en": "Apa Itu Clustering (ML)?", "id": "Model Mengelompokkan Data Mirip." },
{ "en": "Apa Itu Deep Neural Network (DNN)?", "id": "Jaringan Saraf Tiruan Banyak Lapis." },
{ "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Saraf Pengolahan Gambar." },
{ "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jaringan Saraf Pengolahan Data Sekuensial." },
{ "en": "Apa Itu IT Audit?", "id": "Audit Sistem Infrastruktur Teknologi Informasi." },
{ "en": "Apa Itu Internal Audit (IT)?", "id": "Audit Dilakukan Pihak Internal Perusahaan." },
{ "en": "Apa Itu External Audit (IT)?", "id": "Audit Dilakukan Pihak Eksternal Independen." },
{ "en": "Apa Itu Compliance Audit?", "id": "Audit Kepatuhan Regulasi Standar." },
{ "en": "Apa Itu Security Policy?", "id": "Kebijakan Aturan Keamanan Informasi." },
{ "en": "Apa Itu Acceptable Use Policy (AUP)?", "id": "Kebijakan Penggunaan Aset IT." },
{ "en": "Apa Itu Password Policy?", "id": "Kebijakan Aturan Pembuatan Kata Sandi." },
{ "en": "Apa Itu Data Retention Policy?", "id": "Kebijakan Penyimpanan Penghapusan Data." },
{ "en": "Apa Itu Change Control Board (CCB)?", "id": "Dewan Pengkaji Menyetujui Perubahan." },
{ "en": "Apa Itu Request for Change (RFC)?", "id": "Dokumen Permintaan Perubahan Sistem." },
{ "en": "Apa Itu Root Cause Analysis (RCA)?", "id": "Analisis Pencarian Akar Penyebab Masalah." },
{ "en": "Apa Itu 'Five Whys' (RCA)?", "id": "Teknik RCA Tanya 'Kenapa' Lima Kali." },
{ "en": "Apa Itu Fishbone Diagram (RCA)?", "id": "Diagram Sebab Akibat Analisis RCA." },
{ "en": "Apa Itu Software Requirement Specification (SRS)?", "id": "Dokumen Spesifikasi Kebutuhan Perangkat Lunak." },
{ "en": "Apa Itu Functional Requirement?", "id": "Kebutuhan Fitur Fungsionalitas Sistem." },
{ "en": "Apa Itu Non-Functional Requirement?", "id": "Kebutuhan Kualitas Sistem (Performa)." },
{ "en": "Apa Itu Use Case?", "id": "Deskripsi Interaksi Pengguna Sistem." },
{ "en": "Apa Itu Use Case Diagram?", "id": "Diagram Visualisasi Interaksi Aktor Sistem." },
{ "en": "Apa Itu Actor (Use Case)?", "id": "Pengguna Atau Sistem Eksternal." },
{ "en": "Apa Itu Sequence Diagram?", "id": "Diagram Urutan Interaksi Objek." },
{ "en": "Apa Itu Class Diagram?", "id": "Diagram Struktur Kelas Sistem." },
{ "en": "Apa Itu Activity Diagram?", "id": "Diagram Alur Kerja Sistem." },
{ "en": "Apa Itu UML (Unified Modeling Language)?", "id": "Bahasa Pemodelan Visual Desain Sistem." },
{ "en": "Apa Itu Object-Relational Mapping (ORM)?", "id": "Teknik Pemetaan Objek Ke Database." },
{ "en": "Contoh ORM Populer?", "id": "Hibernate (Java), SQLAlchemy (Python)." },
{ "en": "Apa Itu Stored Procedure (SQL)?", "id": "Kumpulan Kueri SQL Disimpan Database." },
{ "en": "Apa Itu Trigger (Database)?", "id": "Prosedur Otomatis Eksekusi Event Database." },
{ "en": "Apa Itu View (Database)?", "id": "Tabel Virtual Hasil Kueri SQL." },
{ "en": "Apa Itu Database Sharding?", "id": "Partisi Data Database Horizontal." },
{ "en": "Apa Itu Database Replication?", "id": "Proses Menyalin Data Antar Database." },
{ "en": "Apa Itu Master-Slave Replication?", "id": "Replikasi Satu Master Banyak Slave." },
{ "en": "Apa Itu Master-Master Replication?", "id": "Replikasi Dua Master Saling Menyalin." },
{ "en": "Apa Itu CAP Theorem?", "id": "Teorema (Consistency, Availability, Partition Tolerance)." },
{ "en": "Apa Itu Eventual Consistency?", "id": "Data Konsisten Pada Akhirnya." },
{ "en": "Apa Itu Strong Consistency?", "id": "Data Selalu Konsisten Setiap Saat." },
{ "en": "Apa Itu Process State (OS)?", "id": "Kondisi Eksekusi Sebuah Proses." },
{ "en": "Apa Itu Process State 'Running'?", "id": "Proses Sedang Dieksekusi CPU." },
{ "en": "Apa Itu Process State 'Ready'?", "id": "Proses Siap Eksekusi Menunggu CPU." },
{ "en": "Apa Itu Process State 'Waiting'?", "id": "Proses Menunggu Event (Contoh: I/O)." },
{ "en": "Apa Itu Process State 'Terminated'?", "id": "Proses Selesai Dieksekusi." },
{ "en": "Apa Itu Process Control Block (PCB)?", "id": "Struktur Data Penyimpan Info Proses." },
{ "en": "Apa Itu Thread (Pemrograman)?", "id": "Unit Eksekusi Terkecil Proses." },
{ "en": "Apa Itu Multiprocessing?", "id": "Menggunakan Banyak CPU Sekaligus." },
{ "en": "Apa Itu Multithreading?", "id": "Menjalankan Banyak Thread Satu Proses." },
{ "en": "Apa Itu User-Level Thread?", "id": "Thread Dikelola Pustaka User Space." },
{ "en": "Apa Itu Kernel-Level Thread?", "id": "Thread Dikelola Langsung Oleh Kernel." },
{ "en": "Apa Itu Concurrency (Pemrograman)?", "id": "Menjalankan Tugas Bergantian (Tumpang Tindih)." },
{ "en": "Apa Itu Parallelism (Pemrograman)?", "id": "Menjalankan Tugas Bersamaan (Paralel)." },
{ "en": "Apa Itu FCFS (First-Come, First-Served)?", "id": "Penjadwalan Proses Antrean Pertama." },
{ "en": "Apa Itu SJF (Shortest Job First)?", "id": "Penjadwalan Proses Pekerjaan Terpendek." },
{ "en": "Apa Itu Round Robin (Penjadwalan)?", "id": "Penjadwalan Proses Bergiliran Waktu." },
{ "en": "Apa Itu Priority Scheduling?", "id": "Penjadwalan Proses Berbasis Prioritas." },
{ "en": "Apa Itu Swapping (Memori)?", "id": "Memindah Proses Antara RAM Disk." },
{ "en": "Apa Itu Paging (Memori)?", "id": "Membagi Memori Menjadi Halaman (Page)." },
{ "en": "Apa Itu Page Fault?", "id": "Error Halaman Data Tidak Ada RAM." },
{ "en": "Apa Itu Segmentation (Memori)?", "id": "Membagi Memori Menjadi Segmen Logis." },
{ "en": "Apa Itu Fragmentasi Internal?", "id": "Sisa Ruang Dalam Blok Memori." },
{ "en": "Apa Itu Fragmentasi Eksternal?", "id": "Sisa Ruang Antar Blok Memori." },
{ "en": "Apa Itu Compaction (Memori)?", "id": "Menyatukan Blok Memori Kosong." },
{ "en": "Apa Itu Inode (File System)?", "id": "Struktur Data Metadata File Linux." },
{ "en": "Apa Itu Superblock (File System)?", "id": "Berisi Informasi Vital File System." },
{ "en": "Apa Itu MFT (Master File Table)?", "id": "Database Metadata File Sistem NTFS." },
{ "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Teknologi Redundansi Gabungan Disk." },
{ "en": "Apa Itu RAID 0 (Striping)?", "id": "Menggabungkan Disk, Tanpa Redundansi." },
{ "en": "Apa Itu RAID 1 (Mirroring)?", "id": "Menduplikasi Data Antar Disk Persis." },
{ "en": "Apa Itu RAID 5 (Striping with Parity)?", "id": "Striping Data Dengan Blok Paritas." },
{ "en": "Apa Itu RAID 10 (1+0)?", "id": "Gabungan Mirroring Dan Striping." },
{ "en": "Apa Itu Hot Spare (RAID)?", "id": "Disk Cadangan Otomatis Gantikan Gagal." },
{ "en": "Apa Itu ICMP (Internet Control Message Protocol)?", "id": "Protokol Pesan Error Jaringan (Ping)." },
{ "en": "Apa Itu RARP (Reverse Address Resolution Protocol)?", "id": "Protokol Balikan ARP (MAC Ke IP)." },
{ "en": "Apa Itu SNMP (Simple Network Management Protocol)?", "id": "Protokol Standar Manajemen Perangkat Jaringan." },
{ "en": "Apa Itu NTP (Network Time Protocol)?", "id": "Protokol Sinkronisasi Waktu Jaringan." },
{ "en": "Apa Itu Port 123?", "id": "Port Standar Protokol NTP." },
{ "en": "Apa Itu Port 161?", "id": "Port Standar Protokol SNMP." },
{ "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Standar Pengiriman Email." },
{ "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Mengunduh Email Ke Klien." },
{ "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Akses Email Di Server." },
{ "en": "Apa Itu MTA (Mail Transfer Agent)?", "id": "Server Perantara Pengiriman Email." },
{ "en": "Apa Itu MUA (Mail User Agent)?", "id": "Aplikasi Klien Email (Outlook)." },
{ "en": "Apa Itu Webhook?", "id": "Callback HTTP Otomatis Antar Aplikasi." },
{ "en": "Apa Itu Polling (API)?", "id": "Klien Bertanya Data Server Berulang." },
{ "en": "Apa Itu Long Polling?", "id": "Polling, Server Menahan Respon Data." },
{ "en": "Apa Itu WebSocket?", "id": "Koneksi Komunikasi Dua Arah Real-time." },
{ "en": "Apa Itu CSRF (Cross-Site Request Forgery)?", "id": "Serangan Memaksa Pengguna Aksi Sah." },
{ "en": "Apa Itu Anti-CSRF Token?", "id": "Token Unik Pencegah Serangan CSRF." },
{ "en": "Apa Itu Clickjacking?", "id": "Serangan Menipu Pengguna Klik Tersembunyi." },
{ "en": "Apa Itu X-Frame-Options?", "id": "Header HTTP Pencegah Serangan Clickjacking." },
{ "en": "Apa Itu CSP (Content Security Policy)?", "id": "Kebijakan Keamanan Konten Web." },
{ "en": "Apa Itu HSTS (HTTP Strict Transport Security)?", "id": "Memaksa Koneksi HTTPS Browser." },
{ "en": "Apa Itu CORS (Cross-Origin Resource Sharing)?", "id": "Mekanisme Izin Akses Sumber Daya Web." },
{ "en": "Apa Itu Insecure Deserialization?", "id": "Celah Keamanan Proses Deserialisasi Data." },
{ "en": "Apa Itu Directory Traversal Attack?", "id": "Serangan Akses File Diluar Direktori." },
{ "en": "Apa Itu Rate Limiting?", "id": "Pembatasan Jumlah Permintaan (Request)." },
{ "en": "Apa Itu Model Tanggung Jawab Bersama (Cloud)?", "id": "Pembagian Tanggung Jawab Keamanan Cloud." },
{ "en": "Apa Itu CASB (Cloud Access Security Broker)?", "id": "Perantara Keamanan Antara Pengguna Cloud." },
{ "en": "Apa Itu Block Cipher?", "id": "Algoritma Enkripsi Data Per Blok." },
{ "en": "Apa Itu Stream Cipher?", "id": "Algoritma Enkripsi Data Per Bit." },
{ "en": "Apa Itu DES (Data Encryption Standard)?", "id": "Standar Enkripsi Simetris Lama." },
{ "en": "Apa Itu AES (Advanced Encryption Standard)?", "id": "Standar Enkripsi Simetris Modern." },
{ "en": "Apa Itu RSA (Rivest–Shamir–Adleman)?", "id": "Algoritma Enkripsi Asimetris Populer." },
{ "en": "Apa Itu Symmetric Key?", "id": "Kunci Sama Enkripsi Dekripsi." },
{ "en": "Apa Itu Asymmetric Key?", "id": "Kunci Berbeda (Publik Privat)." },
{ "en": "Apa Itu Pola Desain (Software)?", "id": "Solusi Umum Masalah Desain Software." },
{ "en": "Apa Itu Pola Singleton?", "id": "Pola Desain Satu Instansi Objek." },
{ "en": "Apa Itu Pola Factory?", "id": "Pola Desain Membuat Objek Terpusat." },
{ "en": "Apa Itu Pola Observer?", "id": "Pola Desain Notifikasi Perubahan Objek." },
{ "en": "Apa Itu MVC (Model-View-Controller)?", "id": "Pola Arsitektur (Data, Tampilan, Logika)." },
{ "en": "Apa Itu Model (MVC)?", "id": "Bagian Pengelola Data Logika Bisnis." },
{ "en": "Apa Itu View (MVC)?", "id": "Bagian Representasi Tampilan Antarmuka." },
{ "en": "Apa Itu Controller (MVC)?", "id": "Bagian Penerima Input Pengontrol Alur." },
{ "en": "Apa Itu SRE (Site Reliability Engineering)?", "id": "Praktik Menjaga Keandalan Sistem." },
{ "en": "Apa Itu Data Engineer?", "id": "Insinyur Membangun Pipeline Data." },
{ "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Integrasi Data (Ekstrak, Ubah, Muat)." },
{ "en": "Apa Itu ELT (Extract, Load, Transform)?", "id": "Proses (Ekstrak, Muat, Ubah)." },
{ "en": "Apa Itu Data Pipeline?", "id": "Alur Otomatisasi Pemrosesan Data." },
{ "en": "Apa Itu Data Lake?", "id": "Penyimpanan Data Mentah Skala Besar." },
{ "en": "Apa Itu Data Warehouse?", "id": "Penyimpanan Data Terstruktur Analisis." },
{ "en": "Perbedaan Data Lake Dan Warehouse?", "id": "Lake Mentah, Warehouse Terstruktur." },
{ "en": "Apa Itu YAML (YAML Ain't Markup Language)?", "id": "Format Serialisasi Data Mudah Dibaca." },
{ "en": "Apa Itu CSV (Comma-Separated Values)?", "id": "Format Data Teks Nilai Dipisah Koma." },
{ "en": "Apa Itu Perintah 'wget' (Linux)?", "id": "Perintah Mengunduh File Web." },
{ "en": "Apa Itu Perintah 'curl' (Linux)?", "id": "Perintah Transfer Data Jaringan." },
{ "en": "Apa Itu Perintah 'top' (Linux)?", "id": "Menampilkan Proses Sistem Real-time." },
{ "en": "Apa Itu Perintah 'df' (Linux)?", "id": "Menampilkan Penggunaan Ruang Disk." },
{ "en": "Apa Itu Perintah 'du' (Linux)?", "id": "Menampilkan Penggunaan Ruang File." },
{ "en": "Apa Itu Perintah 'history' (CLI)?", "id": "Menampilkan Riwayat Perintah Pengguna." },
{ "en": "Apa Itu Perintah 'alias' (CLI)?", "id": "Membuat Nama Pintas Perintah." },
{ "en": "Apa Itu Environment Variable?", "id": "Variabel Konfigurasi Sistem Operasi." },
{ "en": "Apa Itu File '.bashrc'?", "id": "File Konfigurasi Shell Bash." },
{ "en": "Apa Itu File '/etc/hosts'?", "id": "File Pemetaan IP Ke Hostname Lokal." },
{ "en": "Apa Itu Virtual Host?", "id": "Konfigurasi Multi Domain Server Web." },
{ "en": "Apa Itu Server Block (Nginx)?", "id": "Konfigurasi Blok Server Nginx." },
{ "en": "Apa Itu Thread Pool?", "id": "Kumpulan Thread Siap Pakai." },
{ "en": "Apa Itu Koneksi Database Pool?", "id": "Kumpulan Koneksi Database Siap Pakai." },
{ "en": "Apa Itu LIFO (Last-In, First-Out)?", "id": "Prinsip Struktur Data Tumpukan (Stack)." },
{ "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Prinsip Struktur Data Antrean (Queue)." },
{ "en": "Apa Itu Priority Queue?", "id": "Antrean Elemen Berbasis Prioritas." },
{ "en": "Apa Itu Deque (Double-Ended Queue)?", "id": "Antrean Tambah Hapus Dua Sisi." },
{ "en": "Apa Itu Pointer (Pemrograman)?", "id": "Variabel Penyimpan Alamat Memori." },
{ "en": "Apa Itu Null Pointer?", "id": "Pointer Tidak Menunjuk Alamat." },
{ "en": "Apa Itu Memory Allocation?", "id": "Proses Alokasi Ruang Memori." },
{ "en": "Apa Itu Static Allocation?", "id": "Alokasi Memori Saat Kompilasi." },
{ "en": "Apa Itu Dynamic Allocation?", "id": "Alokasi Memori Saat Runtime." },
{ "en": "Apa Itu Stack (Memori)?", "id": "Area Memori Alokasi Statis." },
{ "en": "Apa Itu Heap (Memori)?", "id": "Area Memori Alokasi Dinamis." },
{ "en": "Apa Itu Stack Overflow?", "id": "Error Memori Stack Penuh." },
{ "en": "Apa Itu Memory Leak?", "id": "Gagal Melepas Memori Tidak Terpakai." },
{ "en": "Apa Itu UI (User Interface) Designer?", "id": "Perancang Tampilan Antarmuka Pengguna." },
{ "en": "Apa Itu UX (User Experience) Designer?", "id": "Perancang Pengalaman Pengguna Produk." },
{ "en": "Apa Itu Product Manager (PM)?", "id": "Pengelola Visi Strategi Produk." },
{ "en": "Apa Itu Scrum Master?", "id": "Fasilitator Kerangka Kerja Agile Scrum." },
{ "en": "Apa Itu Product Owner (PO)?", "id": "Pemilik Visi Kebutuhan Produk." },
{ "en": "Apa Itu QA (Quality Assurance) Engineer?", "id": "Insinyur Penjamin Kualitas Perangkat Lunak." },
{ "en": "Apa Itu Manual Testing?", "id": "Pengujian Perangkat Lunak Manual." },
{ "en": "Apa Itu Automation Testing?", "id": "Pengujian Perangkat Lunak Otomatis." },
{ "en": "Apa Itu Test Case?", "id": "Skenario Langkah Pengujian Spesifik." },
{ "en": "Apa Itu Test Plan?", "id": "Dokumen Strategi Rencana Pengujian." },
{ "en": "Apa Itu Bug Report?", "id": "Laporan Detail Cacat Perangkat Lunak." },
{ "en": "Apa Itu Usability Testing?", "id": "Pengujian Kemudahan Penggunaan Produk." },
{ "en": "Apa Itu Performance Testing?", "id": "Pengujian Kinerja Kecepatan Sistem." },
{ "en": "Apa Itu Load Testing?", "id": "Pengujian Sistem Dibawah Beban Puncak." },
{ "en": "Apa Itu Stress Testing?", "id": "Pengujian Sistem Diluar Batas Beban." },
{ "en": "Apa Itu Security Testing?", "id": "Pengujian Keamanan Sistem Aplikasi." },
{ "en": "Apa Itu Penetration Testing (Pentest)?", "id": "Simulasi Serangan Siber Sah." },
{ "en": "Apa Itu White Box Testing?", "id": "Pengujian Struktur Internal Kode." },
{ "en": "Apa Itu Black Box Testing?", "id": "Pengujian Fungsionalitas Tanpa Lihat Kode." },
{ "en": "Apa Itu Gray Box Testing?", "id": "Pengujian Kombinasi White Black Box." },
{ "en": "Apa Itu Smoke Testing?", "id": "Pengujian Awal Fitur Utama Berfungsi." },
{ "en": "Apa Itu Sanity Testing?", "id": "Pengujian Cepat Fitur Spesifik." },
{ "en": "Apa Itu Regression Testing?", "id": "Pengujian Ulang Pasca Perubahan Kode." },
{ "en": "Apa Itu User Acceptance Testing (UAT)?", "id": "Pengujian Penerimaan Oleh Pengguna Akhir." },
{ "en": "Apa Itu Alpha Testing?", "id": "Pengujian Internal Tahap Awal." },
{ "en": "Apa Itu Beta Testing?", "id": "Pengujian Eksternal Rilis Terbatas." },
{ "en": "Apa Itu Wireframe (Desain)?", "id": "Kerangka Visual Dasar Tata Letak." },
{ "en": "Apa Itu Mockup (Desain)?", "id": "Desain Visual Detail Statis Produk." },
{ "en": "Apa Itu Prototype (Desain)?", "id": "Model Interaktif Simulasi Produk." },
{ "en": "Apa Itu User Flow?", "id": "Diagram Alur Langkah Pengguna." },
{ "en": "Apa Itu User Persona?", "id": "Representasi Fiksi Target Pengguna." },
{ "en": "Apa Itu User Journey Map?", "id": "Peta Visual Pengalaman Pengguna." },
{ "en": "Apa Itu Information Architecture (IA)?", "id": "Struktur Organisasi Konten Informasi." },
{ "en": "Apa Itu Card Sorting?", "id": "Metode Riset UX Organisasi Konten." },
{ "en": "Apa Itu A/B Testing?", "id": "Pengujian Dua Versi Desain." },
{ "en": "Apa Itu Heatmap (UX)?", "id": "Visualisasi Area Interaksi Pengguna." },
{ "en": "Apa Itu Responsive Design?", "id": "Desain Web Adaptif Ukuran Layar." },
{ "en": "Apa Itu Mobile-First Design?", "id": "Desain Mulai Dari Tampilan Mobile." },
{ "en": "Apa Itu Style Guide (Desain)?", "id": "Panduan Konsistensi Elemen Desain." },
{ "en": "Apa Itu Design System?", "id": "Kumpulan Komponen Panduan Desain." },
{ "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Aplikasi." },
{ "en": "Apa Itu RESTful API?", "id": "API Sesuai Prinsip REST." },
{ "en": "Apa Itu Endpoint (API)?", "id": "URL Spesifik Titik Akses API." },
{ "en": "Apa Itu API Key?", "id": "Kunci Unik Otentikasi Akses API." },
{ "en": "Apa Itu API Documentation?", "id": "Dokumen Panduan Penggunaan API." },
{ "en": "Apa Itu Software Architect?", "id": "Perancang Arsitektur Sistem Perangkat Lunak." },
{ "en": "Apa Itu System Design?", "id": "Proses Perancangan Arsitektur Sistem." },
{ "en": "Apa Itu High-Level Design (HLD)?", "id": "Desain Arsitektur Sistem Gambaran Besar." },
{ "en": "Apa Itu Low-Level Design (LLD)?", "id": "Desain Detail Komponen Modul Sistem." },
{ "en": "Apa Itu Scalability?", "id": "Kemampuan Sistem Tangani Peningkatan Beban." },
{ "en": "Apa Itu Reliability?", "id": "Keandalan Sistem Beroperasi Konsisten." },
{ "en": "Apa Itu Availability?", "id": "Ketersediaan Sistem Dapat Diakses." },
{ "en": "Apa Itu Maintainability?", "id": "Kemudahan Perawatan Perbaikan Sistem." },
{ "en": "Apa Itu Portability (Software)?", "id": "Kemudahan Pemindahan Software Antar Lingkungan." },
{ "en": "Apa Itu Interoperability?", "id": "Kemampuan Sistem Berkomunikasi Bekerja Sama." },
{ "en": "Apa Itu Modularity (Software)?", "id": "Desain Sistem Berbasis Modul Independen." },
{ "en": "Apa Itu Coupling (Software)?", "id": "Ketergantungan Antar Modul Perangkat Lunak." },
{ "en": "Apa Itu Cohesion (Software)?", "id": "Keterkaitan Fungsional Dalam Satu Modul." },
{ "en": "Apa Itu Loose Coupling?", "id": "Ketergantungan Antar Modul Rendah." },
{ "en": "Apa Itu High Cohesion?", "id": "Fokus Fungsional Modul Tinggi." },
{ "en": "Apa Itu Gantt Chart (Proyek)?", "id": "Bagan Visual Penjadwalan Tugas Proyek." },
{ "en": "Apa Itu Kanban?", "id": "Metode Manajemen Proyek Visual Agile." },
{ "en": "Apa Itu Kanban Board?", "id": "Papan Visual Alur Kerja Kanban." },
{ "en": "Apa Itu Project Milestone?", "id": "Titik Pencapaian Penting Proyek." },
{ "en": "Apa Itu Critical Path Method (CPM)?", "id": "Metode Analisis Jalur Kritis Proyek." },
{ "en": "Apa Itu Project Charter?", "id": "Dokumen Resmi Otorisasi Proyek." },
{ "en": "Apa Itu Stakeholder?", "id": "Pihak Berkepentingan Dalam Proyek." },
{ "en": "Apa Itu Project Scope Creep?", "id": "Penambahan Fitur Diluar Ruang Lingkup." },
{ "en": "Apa Itu Risk Register?", "id": "Dokumen Pencatatan Risiko Proyek." },
{ "en": "Apa Itu Lessons Learned (Proyek)?", "id": "Dokumentasi Pembelajaran Pasca Proyek." },
{ "en": "Apa Itu Command 'whoami' (CLI)?", "id": "Menampilkan Nama Pengguna Saat Ini." },
{ "en": "Apa Itu Command 'passwd' (Linux)?", "id": "Perintah Mengganti Kata Sandi Pengguna." },
{ "en": "Apa Itu Command 'uname' (Linux)?", "id": "Menampilkan Informasi Dasar Sistem Operasi." },
{ "en": "Apa Itu Command 'free' (Linux)?", "id": "Menampilkan Penggunaan Memori (RAM)." },
{ "en": "Apa Itu Command 'lsof' (Linux)?", "id": "Menampilkan File Yang Sedang Terbuka." },
{ "en": "Apa Itu Command 'systemctl' (Linux)?", "id": "Perintah Mengelola Layanan Sistemd." },
{ "en": "Apa Itu Command 'journalctl' (Linux)?", "id": "Perintah Melihat Log Sistemd." },
{ "en": "Apa Itu Command 'reboot' (CLI)?", "id": "Perintah Memulai Ulang Sistem." },
{ "en": "Apa Itu Command 'shutdown' (CLI)?", "id": "Perintah Mematikan Sistem Komputer." },
{ "en": "Apa Itu Script (Pemrograman)?", "id": "Kumpulan Perintah Otomatisasi Tugas." },
{ "en": "Apa Itu Bash Scripting?", "id": "Pembuatan Skrip Menggunakan Bash Shell." },
{ "en": "Apa Itu PowerShell?", "id": "Shell Baris Perintah Milik Microsoft." },
{ "en": "Apa Itu Data Structure?", "id": "Cara Mengatur Data Di Komputer." },
{ "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Prinsip Antrean (Queue)." },
{ "en": "Apa Itu LIFO (Last-In, First-Out)?", "id": "Prinsip Tumpukan (Stack)." },
{ "en": "Apa Itu Stack (Struktur Data)?", "id": "Struktur Data LIFO, Tumpukan." },
{ "en": "Apa Itu Queue (Struktur Data)?", "id": "Struktur Data FIFO, Antrean." },
{ "en": "Apa Itu Linked List?", "id": "Kumpulan Elemen Terhubung Pointer." },
{ "en": "Apa Itu Doubly Linked List?", "id": "Linked List Pointer Ganda." },
{ "en": "Apa Itu Tree (Struktur Data)?", "id": "Struktur Data Hirarkis Non-Linier." },
{ "en": "Apa Itu Binary Tree?", "id": "Pohon, Setiap Node Maksimal Dua Anak." },
{ "en": "Apa Itu Binary Search Tree (BST)?", "id": "Pohon Biner Pencarian Terurut." },
{ "en": "Apa Itu Graph (Struktur Data)?", "id": "Kumpulan Node (Vertices) Edge." },
{ "en": "Apa Itu Hash Table?", "id": "Struktur Data Pasangan Kunci-Nilai." },
{ "en": "Apa Itu Hashing Function?", "id": "Fungsi Mengubah Kunci Menjadi Indeks." },
{ "en": "Apa Itu Time Complexity?", "id": "Ukuran Waktu Eksekusi Algoritma." },
{ "en": "Apa Itu Space Complexity?", "id": "Ukuran Memori Dibutuhkan Algoritma." },
{ "en": "Apa Itu Algoritma Sorting?", "id": "Algoritma Mengurutkan Kumpulan Data." },
{ "en": "Apa Itu Bubble Sort?", "id": "Algoritma Pengurutan Sederhana Perbandingan." },
{ "en": "Apa Itu Merge Sort?", "id": "Algoritma Pengurutan Divide And Conquer." },
{ "en": "Apa Itu Quick Sort?", "id": "Algoritma Pengurutan Pivot Cepat." },
{ "en": "Apa Itu Insertion Sort?", "id": "Algoritma Pengurutan Sisip Sederhana." },
{ "en": "Apa Itu Selection Sort?", "id": "Algoritma Pengurutan Pilih Nilai Terkecil." },
{ "en": "Apa Itu Algoritma Searching?", "id": "Algoritma Mencari Data Tertentu." },
{ "en": "Apa Itu Linear Search?", "id": "Pencarian Urut Satu Per Satu." },
{ "en": "Apa Itu Binary Search?", "id": "Pencarian Data Terurut Bagi Dua." },
{ "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Memanggil Dirinya Sendiri." },
{ "en": "Apa Itu Base Case (Rekursi)?", "id": "Kondisi Berhenti Proses Rekursi." },
{ "en": "Apa Itu Recursive Step?", "id": "Langkah Pemanggilan Ulang Fungsi." },
{ "en": "Apa Itu Stack Overflow (Rekursi)?", "id": "Error Rekursi Terlalu Dalam." },
{ "en": "Apa Itu Algoritma Greedy?", "id": "Algoritma Mengambil Pilihan Optimal Lokal." },
{ "en": "Apa Itu Dynamic Programming?", "id": "Pemecahan Masalah Sub-Problem Optimal." },
{ "en": "Apa Itu Memoization?", "id": "Teknik Menyimpan Hasil Sub-Problem." },
{ "en": "Apa Itu Divide and Conquer?", "id": "Strategi Pecah Masalah, Selesaikan, Gabungkan." },
{ "en": "Apa Itu Backtracking?", "id": "Algoritma Coba Solusi, Mundur Gagal." },
{ "en": "Apa Itu Brute Force Algorithm?", "id": "Algoritma Mencoba Semua Kemungkinan." },
{ "en": "Apa Itu Heuristic Algorithm?", "id": "Algoritma Pendekatan Solusi Cepat." },
{ "en": "Apa Itu Big O Notation?", "id": "Notasi Kompleksitas Performa Algoritma." },
{ "en": "Apa Itu O(1) (Konstan)?", "id": "Kompleksitas Waktu Tetap, Sangat Cepat." },
{ "en": "Apa Itu O(n) (Linier)?", "id": "Waktu Sebanding Ukuran Input." },
{ "en": "Apa Itu O(n^2) (Kuadratik)?", "id": "Waktu Tumbuh Kuadratik, Cukup Lambat." },
{ "en": "Apa Itu O(log n) (Logaritmik)?", "id": "Waktu Tumbuh Lambat, Sangat Cepat." },
{ "en": "Apa Itu O(n log n)?", "id": "Kompleksitas Umum Algoritma Sort Efisien." },
{ "en": "Apa Itu Best Case Scenario?", "id": "Skenario Performa Algoritma Terbaik." },
{ "en": "Apa Itu Worst Case Scenario?", "id": "Skenario Performa Algoritma Terburuk." },
{ "en": "Apa Itu Average Case Scenario?", "id": "Skenario Performa Algoritma Rata-Rata." },
{ "en": "Apa Itu Software Engineering?", "id": "Disiplin Ilmu Rekayasa Perangkat Lunak." },
{ "en": "Apa Itu SDLC (Software Development Life Cycle)?", "id": "Siklus Hidup Pengembangan Perangkat Lunak." },
{ "en": "Tahap Requirement (SDLC)?", "id": "Tahap Pengumpulan Kebutuhan Pengguna." },
{ "en": "Tahap Design (SDLC)?", "id": "Tahap Perancangan Arsitektur Sistem." },
{ "en": "Tahap Implementation (SDLC)?", "id": "Tahap Penulisan Kode Program." },
{ "en": "Tahap Testing (SDLC)?", "id": "Tahap Pengujian Mencari Cacat." },
{ "en": "Tahap Deployment (SDLC)?", "id": "Tahap Peluncuran Rilis Perangkat Lunak." },
{ "en": "Tahap Maintenance (SDLC)?", "id": "Tahap Perawatan Pembaruan Perangkat Lunak." },
{ "en": "Apa Itu Model Waterfall?", "id": "Model SDLC Sekuensial Kaku." },
{ "en": "Apa Itu Model Agile?", "id": "Model SDLC Iteratif Fleksibel." },
{ "en": "Apa Itu Model Spiral?", "id": "Model SDLC Berbasis Risiko." },
{ "en": "Apa Itu Model V-Model?", "id": "Model SDLC Penekanan Validasi Verifikasi." },
{ "en": "Apa Itu Prototyping Model?", "id": "Model SDLC Pembuatan Prototipe Awal." },
{ "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
{ "en": "Apa Itu Sprint (Scrum)?", "id": "Siklus Kerja Waktu Tetap (Iterasi)." },
{ "en": "Apa Itu Product Backlog?", "id": "Daftar Prioritas Fitur Produk." },
{ "en": "Apa Itu Sprint Backlog?", "id": "Daftar Tugas Dikerjakan Sprint." },
{ "en": "Apa Itu Daily Scrum?", "id": "Rapat Harian Singkat Koordinasi Tim." },
{ "en": "Apa Itu Sprint Review?", "id": "Demo Hasil Kerja Sprint." },
{ "en": "Apa Itu Sprint Retrospective?", "id": "Evaluasi Peningkatan Proses Kerja Tim." },
{ "en": "Apa Itu User Story?", "id": "Deskripsi Kebutuhan Fitur Pengguna." },
{ "en": "Apa Itu Acceptance Criteria?", "id": "Kriteria Penerimaan User Story." },
{ "en": "Apa Itu Definition of Done (DoD)?", "id": "Kriteria Pekerjaan Selesai Sepenuhnya." },
{ "en": "Apa Itu Kanban?", "id": "Metode Visual Manajemen Alur Kerja." },
{ "en": "Apa Itu Work in Progress (WIP) Limit?", "id": "Batasan Jumlah Tugas Dikerjakan Bersamaan." },
{ "en": "Apa Itu Lean (Metodologi)?", "id": "Metodologi Fokus Nilai, Eliminasi Pemborosan." },
{ "en": "Apa Itu DevOps?", "id": "Kolaborasi Budaya Development Operations." },
{ "en": "Apa Itu CI (Continuous Integration)?", "id": "Integrasi Kode Otomatis Berkelanjutan." },
{ "en": "Apa Itu CD (Continuous Delivery/Deployment)?", "id": "Pengiriman Rilis Otomatis Berkelanjutan." },
{ "en": "Apa Itu CI/CD Pipeline?", "id": "Alur Otomatisasi Build, Test, Deploy." },
{ "en": "Apa Itu Build Automation?", "id": "Otomatisasi Proses Kompilasi Kode." },
{ "en": "Apa Itu Jenkins?", "id": "Alat Otomatisasi Server CI/CD Populer." },
{ "en": "Apa Itu GitLab CI?", "id": "Fitur CI/CD Terintegrasi GitLab." },
{ "en": "Apa Itu GitHub Actions?", "id": "Platform Otomatisasi Alur Kerja GitHub." },
{ "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Manajemen Infrastruktur Melalui Kode." },
{ "en": "Apa Itu Terraform?", "id": "Alat Populer Infrastructure as Code." },
{ "en": "Apa Itu Ansible?", "id": "Alat Manajemen Konfigurasi Otomatisasi." },
{ "en": "Apa Itu Containerization?", "id": "Teknologi Pengepakan Aplikasi Dependensi." },
{ "en": "Apa Itu Docker?", "id": "Platform Containerization Populer." },
{ "en": "Apa Itu Docker Image?", "id": "Template Read-Only Membuat Container." },
{ "en": "Apa Itu Docker Container?", "id": "Instansi Berjalan Dari Docker Image." },
{ "en": "Apa Itu Dockerfile?", "id": "Skrip Teks Instruksi Membuat Image." },
{ "en": "Apa Itu Docker Hub?", "id": "Registry Publik Penyimpanan Docker Image." },
{ "en": "Apa Itu Container Orchestration?", "id": "Manajemen Otomatisasi Container Skala Besar." },
{ "en": "Apa Itu Kubernetes (K8s)?", "id": "Platform Orkestrasi Container Open Source." },
{ "en": "Apa Itu Pod (Kubernetes)?", "id": "Unit Terkecil Kubernetes, Grup Container." },
{ "en": "Apa Itu Node (Kubernetes)?", "id": "Mesin Pekerja (VM/Fisik) Cluster." },
{ "en": "Apa Itu Cluster (Kubernetes)?", "id": "Kumpulan Node Dikelola Kubernetes." },
{ "en": "Apa Itu Service (Kubernetes)?", "id": "Abstraksi Jaringan Ekspos Sekelompok Pod." },
{ "en": "Apa Itu Deployment (Kubernetes)?", "id": "Mengelola Replikasi Pembaruan Pod." },
{ "en": "Apa Itu Kubectl?", "id": "Alat Baris Perintah Interaksi Kubernetes." },
{ "en": "Apa Itu Microservices Architecture?", "id": "Arsitektur Aplikasi Kumpulan Layanan Kecil." },
{ "en": "Apa Itu Monolithic Architecture?", "id": "Arsitektur Aplikasi Tunggal Terintegrasi." },
{ "en": "Kelebihan Microservices?", "id": "Skalabilitas, Fleksibilitas, Independen." },
{ "en": "Kekurangan Microservices?", "id": "Kompleksitas Jaringan, Manajemen, Monitoring." },
{ "en": "Kelebihan Monolithic?", "id": "Sederhana, Mudah Dikelola Awalnya." },
{ "en": "Kekurangan Monolithic?", "id": "Sulit Skala, Kaku, Deployment Lambat." },
{ "en": "Apa Itu API Gateway?", "id": "Gerbang Tunggal Permintaan API Microservices." },
{ "en": "Apa Itu Service Discovery?", "id": "Mekanisme Layanan Saling Menemukan Otomatis." },
{ "en": "Apa Itu Circuit Breaker Pattern?", "id": "Pola Pencegah Kegagalan Layanan Berantai." },
{ "en": "Apa Itu Serverless Computing?", "id": "Model Komputasi Tanpa Manajemen Server." },
{ "en": "Apa Itu FaaS (Function as a Service)?", "id": "Layanan Serverless Menjalankan Fungsi Kode." },
{ "en": "Contoh Layanan FaaS?", "id": "AWS Lambda, Google Cloud Functions." },
{ "en": "Kelebihan Serverless?", "id": "Skala Otomatis, Bayar Sesuai Pakai." },
{ "en": "Kekurangan Serverless?", "id": "Vendor Lock-In, Cold Start." },
{ "en": "Apa Itu Cold Start (Serverless)?", "id": "Waktu Tunda Awal Eksekusi Fungsi." },
{ "en": "Apa Itu Version Control System (VCS)?", "id": "Sistem Perekam Riwayat Perubahan Kode." },
{ "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
{ "en": "Apa Itu Centralized VCS?", "id": "Kontrol Versi Server Terpusat (SVN)." },
{ "en": "Apa Itu Distributed VCS?", "id": "Kontrol Versi Terdistribusi (Git)." },
{ "en": "Apa Itu Repository (Git)?", "id": "Tempat Penyimpanan Proyek Kode." },
{ "en": "Apa Itu Commit (Git)?", "id": "Simpanan Perubahan Kode Repository." },
{ "en": "Apa Itu Branch (Git)?", "id": "Cabang Pengembangan Paralel Kode." },
{ "en": "Apa Itu Merge (Git)?", "id": "Menggabungkan Perubahan Antar Branch." },
{ "en": "Apa Itu HTTP Request Method?", "id": "Metode Aksi Klien Ke Server." },
{ "en": "Apa Metode HTTP GET?", "id": "Meminta Representasi Sumber Daya." },
{ "en": "Apa Metode HTTP POST?", "id": "Mengirim Entitas Ke Sumber Daya." },
{ "en": "Apa Metode HTTP PUT?", "id": "Mengganti Sumber Daya Target." },
{ "en": "Apa Metode HTTP DELETE?", "id": "Menghapus Sumber Daya Spesifik." },
{ "en": "Apa Metode HTTP PATCH?", "id": "Menerapkan Modifikasi Parsial Sumber Daya." },
{ "en": "Apa Itu HTTP Header?", "id": "Informasi Tambahan Request Response." },
{ "en": "Apa Itu HTTP Status Code?", "id": "Kode Respon Status Server." },
{ "en": "Apa Arti Status 1xx?", "id": "Respon Informasional, Proses Lanjut." },
{ "en": "Apa Arti Status 2xx?", "id": "Respon Sukses, Permintaan Berhasil." },
{ "en": "Apa Arti Status 3xx?", "id": "Respon Redireksi, Perlu Aksi Lanjut." },
{ "en": "Apa Arti Status 4xx?", "id": "Respon Error Sisi Klien." },
{ "en": "Apa Arti Status 5xx?", "id": "Respon Error Sisi Server." },
{ "en": "Apa Itu Status 200 OK?", "id": "Permintaan Berhasil Sukses." },
{ "en": "Apa Itu Status 201 Created?", "id": "Sumber Daya Baru Berhasil Dibuat." },
{ "en": "Apa Itu Status 301 Moved Permanently?", "id": "Sumber Daya Pindah Permanen." },
{ "en": "Apa Itu Status 302 Found?", "id": "Sumber Daya Pindah Sementara." },
{ "en": "Apa Itu Status 400 Bad Request?", "id": "Permintaan Klien Tidak Valid." },
{ "en": "Apa Itu Status 401 Unauthorized?", "id": "Klien Membutuhkan Otentikasi." },
{ "en": "Apa Itu Status 403 Forbidden?", "id": "Klien Tidak Punya Hak Akses." },
{ "en": "Apa Itu Status 404 Not Found?", "id": "Sumber Daya Tidak Ditemukan." },
{ "en": "Apa Itu Status 500 Internal Server Error?", "id": "Kesalahan Internal Pada Server." },
{ "en": "Apa Itu Status 503 Service Unavailable?", "id": "Layanan Server Tidak Tersedia." },
{ "en": "Apa Itu Idempotent (HTTP)?", "id": "Request Sama, Hasil Tetap Sama." },
{ "en": "Metode HTTP Apa Idempotent?", "id": "GET, PUT, DELETE." },
{ "en": "Apa Itu Web Socket?", "id": "Koneksi Komunikasi Dua Arah Penuh." },
{ "en": "Apa Itu Server-Sent Events (SSE)?", "id": "Koneksi Server Ke Klien Satu Arah." },
{ "en": "Apa Itu World Wide Web Consortium (W3C)?", "id": "Organisasi Standar Utama Web." },
{ "en": "Apa Itu Web Accessibility Initiative (WAI)?", "id": "Inisiatif Aksesibilitas Web W3C." },
{ "en": "Apa Itu WCAG (Web Content Accessibility Guidelines)?", "id": "Panduan Aksesibilitas Konten Web." },
{ "en": "Apa Itu DNS Record?", "id": "Catatan Pemetaan DNS Server." },
{ "en": "Apa Itu 'A' Record (DNS)?", "id": "Memetakan Hostname Ke Alamat IPV4." },
{ "en": "Apa Itu 'AAAA' Record (DNS)?", "id": "Memetakan Hostname Ke Alamat IPV6." },
{ "en": "Apa Itu 'CNAME' Record (DNS)?", "id": "Memetakan Alias Hostname Ke Hostname." },
{ "en": "Apa Itu 'MX' Record (DNS)?", "id": "Menentukan Mail Server Domain." },
{ "en": "Apa Itu 'TXT' Record (DNS)?", "id": "Menyimpan Data Teks Informasi Domain." },
{ "en": "Apa Itu DNS Propagation?", "id": "Proses Penyebaran Update DNS." },
{ "en": "Apa Itu Time To Live (TTL) DNS?", "id": "Waktu Cache Record DNS." },
{ "en": "Apa Itu Subdomain?", "id": "Domain Bagian Dari Domain Utama." },
{ "en": "Apa Itu Top-Level Domain (TLD)?", "id": "Bagian Akhir Nama Domain (.com)." },
{ "en": "Apa Itu Country Code TLD (ccTLD)?", "id": "TLD Kode Negara (.id, .sg)." },
{ "en": "Apa Itu Generic TLD (gTLD)?", "id": "TLD Generik (.com, .org, .net)." },
{ "en": "Apa Itu PDU (Protocol Data Unit)?", "id": "Unit Data Spesifik Lapis OSI." },
{ "en": "Apa PDU Lapis Transport?", "id": "Segment (TCP) Atau Datagram (UDP)." },
{ "en": "Apa PDU Lapis Network?", "id": "Packet (Paket)." },
{ "en": "Apa PDU Lapis Data Link?", "id": "Frame." },
{ "en": "Apa PDU Lapis Physical?", "id": "Bit." },
{ "en": "Apa Itu Encapsulation (Jaringan)?", "id": "Proses Menambah Header Data." },
{ "en": "Apa Itu Decapsulation (Jaringan)?", "id": "Proses Membuka Header Data." },
{ "en": "Apa Itu Collision Domain?", "id": "Area Jaringan Potensi Tabrakan Data." },
{ "en": "Apa Itu Broadcast Domain?", "id": "Area Jaringan Menerima Broadcast." },
{ "en": "Perangkat Apa Pemisah Collision Domain?", "id": "Switch, Bridge, Dan Router." },
{ "en": "Perangkat Apa Pemisah Broadcast Domain?", "id": "Router (Secara Default)." },
{ "en": "Apa Itu CSMA/CD (Carrier Sense Multiple Access/Collision Detection)?", "id": "Metode Deteksi Tabrakan Jaringan Ethernet." },
{ "en": "Apa Itu CSMA/CA (Carrier Sense Multiple Access/Collision Avoidance)?", "id": "Metode Pencegahan Tabrakan Jaringan Wi-Fi." },
{ "en": "Apa Itu Unicast?", "id": "Pengiriman Data Satu Ke Satu." },
{ "en": "Apa Itu Multicast?", "id": "Pengiriman Data Satu Ke Grup." },
{ "en": "Apa Itu Broadcast?", "id": "Pengiriman Data Satu Ke Semua." },
{ "en": "Apa Itu Simplex Communication?", "id": "Komunikasi Satu Arah Saja." },
{ "en": "Apa Itu Half-Duplex Communication?", "id": "Komunikasi Dua Arah Bergantian." },
{ "en": "Apa Itu Full-Duplex Communication?", "id": "Komunikasi Dua Arah Bersamaan." },
{ "en": "Contoh Komunikasi Simplex?", "id": "Siaran Radio Atau Televisi." },
{ "en": "Contoh Komunikasi Half-Duplex?", "id": "Walkie-Talkie." },
{ "en": "Contoh Komunikasi Full-Duplex?", "id": "Percakapan Telepon." },
{ "en": "Apa Itu Inner Join (SQL)?", "id": "Mengambil Data Cocok Kedua Tabel." },
{ "en": "Apa Itu Left Join (SQL)?", "id": "Data Kiri, Data Cocok Kanan." },
{ "en": "Apa Itu Right Join (SQL)?", "id": "Data Kanan, Data Cocok Kiri." },
{ "en": "Apa Itu Full Outer Join (SQL)?", "id": "Semua Data Kiri Kanan." },
{ "en": "Apa Itu Cross Join (SQL)?", "id": "Produk Cartesian Semua Baris Tabel." },
{ "en": "Apa Itu Self Join (SQL)?", "id": "Menggabungkan Tabel Dengan Dirinya Sendiri." },
{ "en": "Apa Itu Klausa WHERE (SQL)?", "id": "Memfilter Baris Data Hasil Kueri." },
{ "en": "Apa Itu Klausa GROUP BY (SQL)?", "id": "Mengelompokkan Baris Hasil Kueri." },
{ "en": "Apa Itu Klausa HAVING (SQL)?", "id": "Memfilter Grup Data (Pasca GROUP BY)." },
{ "en": "Perbedaan WHERE Dan HAVING?", "id": "WHERE Filter Baris, HAVING Filter Grup." },
{ "en": "Apa Itu Klausa ORDER BY (SQL)?", "id": "Mengurutkan Hasil Kueri Data." },
{ "en": "Apa Itu Perintah ASC (SQL)?", "id": "Mengurutkan Data Menaik (Ascending)." },
{ "en": "Apa Itu Perintah DESC (SQL)?", "id": "Mengurutkan Data Menurun (Descending)." },
{ "en": "Apa Itu Aggregate Function (SQL)?", "id": "Fungsi Kalkulasi Grup Baris." },
{ "en": "Contoh Aggregate Function (SQL)?", "id": "COUNT, SUM, AVG, MAX, MIN." },
{ "en": "Apa Itu Fungsi COUNT()?", "id": "Menghitung Jumlah Baris Hasil." },
{ "en": "Apa Itu Fungsi SUM()?", "id": "Menjumlahkan Total Nilai Kolom." },
{ "en": "Apa Itu Fungsi AVG()?", "id": "Menghitung Nilai Rata-Rata Kolom." },
{ "en": "Apa Itu Fungsi MAX()?", "id": "Mencari Nilai Maksimal Kolom." },
{ "en": "Apa Itu Fungsi MIN()?", "id": "Mencari Nilai Minimal Kolom." },
{ "en": "Apa Itu Subquery (SQL)?", "id": "Kueri Didalam Kueri SQL Lain." },
{ "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Standar Database Relasional." },
{ "en": "Apa Itu DDL (Data Definition Language)?", "id": "Perintah SQL Definisi Struktur Data." },
{ "en": "Contoh Perintah DDL?", "id": "CREATE, ALTER, DROP, TRUNCATE." },
{ "en": "Apa Itu DML (Data Manipulation Language)?", "id": "Perintah SQL Manipulasi Data." },
{ "en": "Contoh Perintah DML?", "id": "SELECT, INSERT, UPDATE, DELETE." },
{ "en": "Apa Itu DCL (Data Control Language)?", "id": "Perintah SQL Kontrol Hak Akses." },
{ "en": "Contoh Perintah DCL?", "id": "GRANT Dan REVOKE." },
{ "en": "Apa Itu TCL (Transaction Control Language)?", "id": "Perintah SQL Manajemen Transaksi." },
{ "en": "Contoh Perintah TCL?", "id": "COMMIT, ROLLBACK, SAVEPOINT." },
{ "en": "Apa Itu Perintah COMMIT?", "id": "Menyimpan Transaksi Database Permanen." },
{ "en": "Apa Itu Perintah ROLLBACK?", "id": "Membatalkan Transaksi Database Terakhir." },
{ "en": "Apa Itu Perintah TRUNCATE?", "id": "Menghapus Semua Data Tabel Cepat." },
{ "en": "Perbedaan DELETE Dan TRUNCATE?", "id": "DELETE (DML) Lambat, TRUNCATE (DDL) Cepat." },
{ "en": "Apa Itu Database View?", "id": "Tabel Virtual Berdasarkan Kueri SQL." },
{ "en": "Apa Itu Stored Procedure?", "id": "Kumpulan Perintah SQL Disimpan Server." },
{ "en": "Apa Itu Konstruktor (OOP)?", "id": "Metode Khusus Inisialisasi Objek." },
{ "en": "Apa Itu Destruktor (OOP)?", "id": "Metode Khusus Membersihkan Objek." },
{ "en": "Apa Itu Interface (OOP)?", "id": "Kontrak Kumpulan Metode Abstrak." },
{ "en": "Apa Itu Kelas Abstrak (OOP)?", "id": "Kelas Tidak Bisa Diinstansiasi." },
{ "en": "Apa Itu Metode Abstrak (OOP)?", "id": "Metode Tanpa Implementasi Isi." },
{ "en": "Apa Itu Superclass (OOP)?", "id": "Kelas Induk Dalam Pewarisan." },
{ "en": "Apa Itu Subclass (OOP)?", "id": "Kelas Anak Mewarisi Superclass." },
{ "en": "Apa Itu Method Overriding (OOP)?", "id": "Subclass Mendefinisi Ulang Metode Induk." },
{ "en": "Apa Itu Method Overloading (OOP)?", "id": "Metode Sama, Parameter Berbeda." },
{ "en": "Apa Itu Static Method (OOP)?", "id": "Metode Milik Kelas, Bukan Objek." },
{ "en": "Apa Itu Static Variable (OOP)?", "id": "Variabel Milik Kelas, Dibagi Objek." },
{ "en": "Apa Itu Final Variable (OOP)?", "id": "Variabel Nilai Tidak Bisa Diubah." },
{ "en": "Apa Itu Final Class (OOP)?", "id": "Kelas Tidak Bisa Diwariskan." },
{ "en": "Apa Itu Final Method (OOP)?", "id": "Metode Tidak Bisa Di-Override." },
{ "en": "Apa Itu Access Modifier (OOP)?", "id": "Pengontrol Visibilitas (Public, Private)." },
{ "en": "Apa Itu Public (Access Modifier)?", "id": "Dapat Diakses Dari Mana Saja." },
{ "en": "Apa Itu Private (Access Modifier)?", "id": "Hanya Diakses Dalam Kelas Sendiri." },
{ "en": "Apa Itu Protected (Access Modifier)?", "id": "Akses Kelas, Package, Subclass." },
{ "en": "Apa Itu Package (Pemrograman)?", "id": "Kumpulan Kelas Terkait Logis." },
{ "en": "Apa Itu BFS (Breadth-First Search)?", "id": "Algoritma Pencarian Graf Level Per Level." },
{ "en": "Apa Itu DFS (Depth-First Search)?", "id": "Algoritma Pencarian Graf Telusur Mendalam." },
{ "en": "Apa Itu Adjacency Matrix (Graph)?", "id": "Representasi Graf Menggunakan Matriks." },
{ "en": "Apa Itu Adjacency List (Graph)?", "id": "Representasi Graf Menggunakan Daftar." },
{ "en": "Apa Itu Vertex (Graph)?", "id": "Titik Simpul Dalam Struktur Graf." },
{ "en": "Apa Itu Edge (Graph)?", "id": "Garis Penghubung Antar Vertex Graf." },
{ "en": "Apa Itu Sibling (Tree)?", "id": "Node Anak Dari Induk Sama." },
{ "en": "Apa Itu Height (Tree)?", "id": "Panjang Jalur Terpanjang Ke Daun." },
{ "en": "Apa Itu Depth (Node)?", "id": "Jarak Node Dari Root Pohon." },
{ "en": "Apa Itu Balanced Tree?", "id": "Pohon Ketinggian Sub-Pohon Seimbang." },
{ "en": "Apa Itu AVL Tree?", "id": "Jenis Pohon Biner Pencarian Seimbang." },
{ "en": "Apa Itu Red-Black Tree?", "id": "Jenis Lain Pohon Biner Seimbang." },
{ "en": "Apa Itu B-Tree?", "id": "Pohon Pencarian Data Blok Disk." },
{ "en": "Apa Itu Tipe Data Abstrak (ADT)?", "id": "Model Konseptual Tipe Data." },
{ "en": "Apa Itu Min-Heap?", "id": "Heap, Node Induk Lebih Kecil." },
{ "en": "Apa Itu Max-Heap?", "id": "Heap, Node Induk Lebih Besar." },
{ "en": "Apa Itu Set (Struktur Data)?", "id": "Kumpulan Elemen Unik Tidak Terurut." },
{ "en": "Apa Itu Map (Struktur Data)?", "id": "Kumpulan Pasangan Kunci-Nilai Unik." },
{ "en": "Apa Itu Dictionary (Struktur Data)?", "id": "Sama Seperti Map Atau Hash Table." },
{ "en": "Fungsi Lapis Fisik (OSI)?", "id": "Mengirim Bit Data Melalui Media." },
{ "en": "Alamat Lapis Data Link?", "id": "MAC Address (Media Access Control)." },
{ "en": "Alamat Lapis Network?", "id": "IP Address (Internet Protocol)." },
{ "en": "Protokol Lapis Transport?", "id": "TCP (Transmission Control Protocol) UDP (User Datagram Protocol)." },
{ "en": "Tugas Lapis Sesi (OSI)?", "id": "Manajemen Dialog Antar Aplikasi." },
{ "en": "Tugas Lapis Presentasi (OSI)?", "id": "Memformat Data Agar Dapat Dibaca." },
{ "en": "Contoh Protokol Lapis Aplikasi?", "id": "HTTP, FTP, SMTP, DNS." },
{ "en": "Perbedaan TCP Dan UDP?", "id": "TCP Handal, UDP Cepat Ringan." },
{ "en": "Kapan Menggunakan TCP?", "id": "Saat Keutuhan Data Penting (Web)." },
{ "en": "Kapan Menggunakan UDP?", "id": "Saat Kecepatan Penting (Streaming, Game)." },
{ "en": "Apa Itu Three-Way Handshake (TCP)?", "id": "Proses Tiga Langkah Memulai Koneksi TCP." },
{ "en": "Apa Itu Well-Known Ports?", "id": "Port 0-1023 Layanan Standar." },
{ "en": "Apa Itu Registered Ports?", "id": "Port 1024-49151 Aplikasi Terdaftar." },
{ "en": "Apa Itu Dynamic/Private Ports?", "id": "Port 49152-65535 Sisi Klien." },
{ "en": "Apa Itu Socket (Jaringan)?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
{ "en": "Apa Itu Data Analyst?", "id": "Menganalisis Data, Menemukan Wawasan." },
{ "en": "Tugas Data Scientist?", "id": "Analisis Data Kompleks, Machine Learning." },
{ "en": "Tugas Data Engineer?", "id": "Membangun Sistem Pengumpulan Data." },
{ "en": "Apa Itu Business Intelligence (BI) Analyst?", "id": "Menganalisis Data Tren Bisnis." },
{ "en": "Apa Itu Machine Learning Engineer?", "id": "Membangun Menerapkan Model ML." },
{ "en": "Tugas Database Administrator?", "id": "Backup, Restore, Optimasi Database." },
{ "en": "Tugas Network Administrator?", "id": "Konfigurasi Router, Switch, Firewall." },
{ "en": "Apa Itu System Administrator (SysAdmin)?", "id": "Mengelola, Merawat Server, Sistem Operasi." },
{ "en": "Apa Itu Cloud Engineer?", "id": "Merancang, Mengelola Infrastruktur Cloud." },
{ "en": "Tugas Cybersecurity Analyst?", "id": "Memantau Ancaman, Menganalisis Celah." },
{ "en": "Apa Itu Penetration Tester (Pentester)?", "id": "Penguji Keamanan, Simulasi Serangan." },
{ "en": "Tugas DevOps Engineer?", "id": "Mengelola CI/CD, Infrastruktur Kode." },
{ "en": "Fokus UI Designer?", "id": "Tipografi, Warna, Tata Letak Visual." },
{ "en": "Fokus UX Designer?", "id": "Riset Pengguna, Usability, Arsitektur." },
{ "en": "Tugas Product Manager?", "id": "Menentukan Fitur, Prioritas, Roadmap." },
{ "en": "Tugas Project Manager?", "id": "Mengelola Jadwal, Anggaran, Sumber Daya." },
{ "en": "Apa Itu Software Developer?", "id": "Menulis, Menguji, Merawat Kode." },
{ "en": "Teknologi Frontend Developer?", "id": "HTML, CSS, JavaScript, React." },
{ "en": "Teknologi Backend Developer?", "id": "Java, Python, Node.js, Database." },
{ "en": "Kemampuan Full Stack Developer?", "id": "Mengelola Seluruh Tumpukan Teknologi." },
{ "en": "Apa Itu Mobile Developer?", "id": "Pengembang Aplikasi Perangkat Mobile." },
{ "en": "Apa Itu Native Mobile Development?", "id": "Pengembangan Spesifik Platform (iOS/Android)." },
{ "en": "Apa Itu Cross-Platform Mobile Development?", "id": "Pengembangan Multi Platform (Flutter, React Native)." },
{ "en": "Apa Itu Game Developer?", "id": "Pengembang Perangkat Lunak Permainan (Game)." },
{ "en": "Apa Itu Embedded Systems Engineer?", "id": "Pengembang Perangkat Lunak Tertanam Hardware." },
{ "en": "Apa Itu IT Support Specialist?", "id": "Memberi Bantuan Teknis Pengguna." },
{ "en": "Apa Itu Technical Writer?", "id": "Penulis Dokumentasi Teknis Produk." },
{ "en": "Tugas Scrum Master?", "id": "Menghilangkan Hambatan, Memastikan Proses." },
{ "en": "Tugas Product Owner?", "id": "Memaksimalkan Nilai Produk, Mengelola Backlog." },
{ "en": "Apa Itu Quality Assurance (QA) Tester?", "id": "Penguji Manual Perangkat Lunak." },
{ "en": "Apa Itu QA Automation Engineer?", "id": "Penguji Otomatis Perangkat Lunak." },
{ "en": "Apa Itu Command 'git branch'?", "id": "Melihat, Membuat, Menghapus Cabang (Branch)." },
{ "en": "Apa Itu Command 'git checkout'?", "id": "Berpindah Antar Cabang (Branch)." },
{ "en": "Apa Itu Command 'git merge'?", "id": "Menggabungkan Riwayat Dua Cabang." },
{ "en": "Apa Itu Command 'git rebase'?", "id": "Menggabungkan Riwayat, Menulis Ulang Commit." },
{ "en": "Apa Itu Command 'git remote'?", "id": "Mengelola Koneksi Repository Remote." },
{ "en": "Apa Itu Command 'git fetch'?", "id": "Mengunduh Perubahan Remote Tanpa Menggabung." },
{ "en": "Apa Itu 'origin' (Git)?", "id": "Nama Default Repository Remote." },
{ "en": "Apa Itu 'HEAD' (Git)?", "id": "Pointer Ke Commit Terkini Cabang." },
{ "en": "Apa Itu Command 'git log'?", "id": "Menampilkan Riwayat Commit Proyek." },
{ "en": "Apa Itu Command 'git diff'?", "id": "Menampilkan Perbedaan Antar Commit File." },
{ "en": "Apa Itu File '.gitignore'?", "id": "File Teks Daftar Abaikan Git." },
{ "en": "Apa Itu Virtual Environment (Python)?", "id": "Lingkungan Terisolasi Dependensi Proyek." },
{ "en": "Apa Itu 'pip' (Python)?", "id": "Manajer Paket Standar Python." },
{ "en": "Struktur Data JSON?", "id": "Pasangan Kunci-Nilai Dan Array." },
{ "en": "Struktur Data XML?", "id": "Berbasis Tag Mirip HTML." },
{ "en": "Penggunaan Umum YAML?", "id": "File Konfigurasi (Docker, Kubernetes)." },
{ "en": "Apa Itu Manufaktur Komputer?", "id": "Proses Pembuatan Komponen Komputer." },
{ "en": "Apa Itu Perakitan Komputer?", "id": "Proses Merakit Komponen Menjadi PC." },
{ "en": "Apa Itu Casing Komputer?", "id": "Kotak Pelindung Komponen Perangkat Keras." },
{ "en": "Apa Itu Form Factor Casing?", "id": "Standar Ukuran Casing Komputer." },
{ "en": "Contoh Form Factor Casing?", "id": "Full Tower, Mid Tower, Mini ITX." },
{ "en": "Apa Itu Kipas Casing?", "id": "Kipas Pendingin Sirkulasi Udara Casing." },
{ "en": "Apa Itu Tombol Power?", "id": "Tombol Menghidupkan Mematikan Komputer." },
{ "en": "Apa Itu Tombol Reset?", "id": "Tombol Memulai Ulang Paksa Komputer." },
{ "en": "Apa Itu Lampu LED Indikator?", "id": "Lampu Indikator Status Komputer." },
{ "en": "Apa Itu Front Panel?", "id": "Bagian Depan Casing Port Tombol." },
{ "en": "Apa Itu Back Panel?", "id": "Bagian Belakang Casing Port Motherboard." },
{ "en": "Apa Itu Expansion Slot?", "id": "Slot Menambah Kartu Ekspansi (GPU)." },
{ "en": "Apa Itu Drive Bay?", "id": "Tempat Memasang Hard Drive SSD." },
{ "en": "Apa Itu Cable Management?", "id": "Manajemen Penataan Kabel Dalam Casing." },
{ "en": "Apa Itu PSU Modular?", "id": "PSU Kabel Dapat Dilepas Pasang." },
{ "en": "Apa Itu PSU Non-Modular?", "id": "PSU Kabel Terpasang Permanen." },
{ "en": "Apa Itu Peringkat Efisiensi PSU?", "id": "Standar Efisiensi (80 Plus Bronze)." },
{ "en": "Apa Itu VGA Terintegrasi (Onboard)?", "id": "GPU Terintegrasi Dalam CPU Motherboard." },
{ "en": "Apa Itu VGA Diskrit (Dedicated)?", "id": "Kartu Grafis Terpisah Slot PCIe." },
{ "en": "Apa Itu VRAM (Video RAM)?", "id": "Memori Khusus Kartu Grafis (GPU)." },
{ "en": "Apa Itu Sistem Pendingin Udara?", "id": "Pendingin Menggunakan Heatsink Kipas." },
{ "en": "Apa Itu Sistem Pendingin Cair?", "id": "Pendingin Menggunakan Cairan Radiator." },
{ "en": "Apa Itu AIO (All-In-One) Liquid Cooler?", "id": "Pendingin Cair Paket Lengkap." },
{ "en": "Apa Itu Custom Liquid Cooling?", "id": "Pendingin Cair Rakitan Kustom." },
{ "en": "Apa Itu RAM Single Channel?", "id": "Konfigurasi Satu Jalur Memori." },
{ "en": "Apa Itu RAM Dual Channel?", "id": "Konfigurasi Dua Jalur Memori Cepat." },
{ "en": "Apa Itu RAM Quad Channel?", "id": "Konfigurasi Empat Jalur Memori." },
{ "en": "Apa Itu XMP (Extreme Memory Profile)?", "id": "Profil Overclocking RAM Intel." },
{ "en": "Apa Itu L3 Cache CPU?", "id": "Cache Terbesar CPU, Dibagi Core." },
{ "en": "Apa Itu Hyper-Threading (Intel)?", "id": "Teknologi Core Fisik Jadi Virtual." },
{ "en": "Apa Itu SMT (Simultaneous Multithreading) (AMD)?", "id": "Teknologi Serupa Hyper-Threading AMD." },
{ "en": "Apa Itu Arsitektur CPU?", "id": "Desain Dasar Set Instruksi CPU." },
{ "en": "Apa Itu x86 (Arsitektur)?", "id": "Arsitektur 32-bit Umum PC." },
{ "en": "Apa Itu x86-64 (x64)?", "id": "Arsitektur 64-bit Pengganti x86." },
{ "en": "Apa Itu ARM (Arsitektur)?", "id": "Arsitektur Umum Perangkat Mobile." },
{ "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Arsitektur CPU Instruksi Kompleks." },
{ "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Arsitektur CPU Instruksi Sederhana." },
{ "en": "Apa Itu Set Instruksi?", "id": "Kumpulan Perintah Dipahami CPU." },
{ "en": "Apa Itu Register CPU?", "id": "Memori Penyimpanan Internal CPU Tercepat." },
{ "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Unit CPU Operasi Aritmatika Logika." },
{ "en": "Apa Itu Control Unit (CU)?", "id": "Unit CPU Pengontrol Alur Instruksi." },
{ "en": "Apa Itu Fetch-Decode-Execute Cycle?", "id": "Siklus Dasar Eksekusi Instruksi CPU." },
{ "en": "Apa Itu Pipelining (CPU)?", "id": "Teknik Eksekusi Instruksi Tumpang Tindih." },
{ "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Inisialisasi Hardware Saat Boot." },
{ "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS Modern Antarmuka Grafis." },
{ "en": "Apa Itu POST (Power-On Self-Test)?", "id": "Tes Mandiri Hardware Saat Boot." },
{ "en": "Apa Itu Bootloader?", "id": "Program Pemuat Sistem Operasi." },
{ "en": "Apa Itu MBR (Master Boot Record)?", "id": "Sektor Boot Partisi Gaya Lama." },
{ "en": "Apa Itu GPT (GUID Partition Table)?", "id": "Standar Partisi Modern Pengganti MBR." },
{ "en": "Apa Itu Logical Partition?", "id": "Partisi Logis Didalam Partisi Extended." },
{ "en": "Apa Itu Primary Partition?", "id": "Partisi Utama Disk (Maksimal 4 MBR)." },
{ "en": "Apa Itu File System Journaling?", "id": "Fitur File System Catat Perubahan." },
{ "en": "Apa Itu Disk Formatting?", "id": "Proses Inisialisasi Disk File System." },
{ "en": "Apa Itu Low-Level Formatting?", "id": "Format Fisik Sektor Disk Pabrikan." },
{ "en": "Apa Itu High-Level Formatting?", "id": "Format Membuat Struktur File System." },
{ "en": "Apa Itu Disk Defragmentation?", "id": "Menyusun Ulang Data Disk Terfragmentasi." },
{ "en": "Apa Itu TRIM (SSD)?", "id": "Perintah Optimasi Kinerja SSD." },
{ "en": "Apa Itu S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology)?", "id": "Teknologi Monitor Kesehatan Hard Drive." },
{ "en": "Apa Itu Bad Sector?", "id": "Sektor Disk Rusak Tidak Terbaca." },
{ "en": "Apa Itu Head Crash (HDD)?", "id": "Head Pembaca Menyentuh Piringan HDD." },
{ "en": "Apa Itu RPM (Revolutions Per Minute) HDD?", "id": "Kecepatan Putaran Piringan Hard Drive." },
{ "en": "Apa Itu Access Time (HDD)?", "id": "Waktu Akses Data Hard Drive." },
{ "en": "Apa Itu Seek Time (HDD)?", "id": "Waktu Head Bergerak Ke Track." },
{ "en": "Apa Itu Rotational Latency (HDD)?", "id": "Waktu Piringan Berputar Ke Sektor." },
{ "en": "Apa Itu SSD (Solid-State Drive)?", "id": "Media Penyimpanan Berbasis Flash." },
{ "en": "Apa Itu SSHD (Solid-State Hybrid Drive)?", "id": "Hard Drive Hibrida HDD SSD." },
{ "en": "Apa Itu NAND Flash Memory?", "id": "Tipe Memori Flash Digunakan SSD." },
{ "en": "Apa Itu Wear Leveling (SSD)?", "id": "Teknik Distribusi Penulisan Data SSD." },
{ "en": "Apa Itu Optical Drive?", "id": "Drive Pembaca Penulis Media Optik." },
{ "en": "Contoh Media Optik?", "id": "CD (Compact Disc), DVD, Blu-Ray." },
{ "en": "Apa Itu CD-ROM?", "id": "CD Read-Only Memory." },
{ "en": "Apa Itu CD-R?", "id": "CD Recordable (Tulis Sekali)." },
{ "en": "Apa Itu CD-RW?", "id": "CD ReWritable (Tulis Ulang)." },
{ "en": "Apa Itu Konektor Molex?", "id": "Konektor Daya 4-Pin Lama." },
{ "en": "Apa Itu Konektor Floppy?", "id": "Konektor Daya Kecil Floppy Drive." },
{ "en": "Apa Itu Antarmuka PATA (Parallel ATA)?", "id": "Antarmuka Drive Paralel Lama (IDE)." },
{ "en": "Apa Itu Antarmuka SATA (Serial ATA)?", "id": "Antarmuka Drive Serial Modern." },
{ "en": "Apa Itu Hot Swapping?", "id": "Mencabut Memasang Perangkat Saat Menyala." },
{ "en": "Apa Itu Port PS/2?", "id": "Port Konektor Lama Keyboard Mouse." },
{ "en": "Apa Itu Port Serial (COM)?", "id": "Port Komunikasi Serial 9-Pin." },
{ "en": "Apa Itu Port Paralel (LPT)?", "id": "Port Komunikasi Paralel 25-Pin (Printer)." },
{ "en": "Apa Itu Port FireWire (IEEE 1394)?", "id": "Port Transfer Data Cepat (Lama)." },
{ "en": "Apa Itu Port eSATA?", "id": "Port SATA Eksternal Perangkat Penyimpanan." },
{ "en": "Apa Itu Kensington Lock Slot?", "id": "Lubang Kunci Pengaman Fisik Laptop." },
{ "en": "Apa Itu Color Depth?", "id": "Jumlah Bit Warna Per Pixel." },
{ "en": "Apa Itu True Color?", "id": "Kedalaman Warna 24-Bit." },
{ "en": "Apa Itu HDR (High Dynamic Range)?", "id": "Rentang Kontras Kecerahan Tinggi." },
{ "en": "Apa Itu Panel Layar TN (Twisted Nematic)?", "id": "Panel Murah, Respon Cepat." },
{ "en": "Apa Itu Panel Layar IPS (In-Plane Switching)?", "id": "Panel Warna Akurat, Sudut Pandang Luas." },
{ "en": "Apa Itu Panel Layar VA (Vertical Alignment)?", "id": "Panel Kontras Tinggi, Antara TN IPS." },
{ "en": "Apa Itu OLED (Organic Light Emitting Diode)?", "id": "Panel Layar Kontras Sempurna." },
{ "en": "Apa Itu Screen Tearing?", "id": "Tampilan Gambar Robek Layar." },
{ "en": "Apa Itu VSync (Vertical Sync)?", "id": "Sinkronisasi FPS GPU Refresh Rate." },
{ "en": "Apa Itu G-Sync (NVIDIA)?", "id": "Teknologi VSync Adaptif NVIDIA." },
{ "en": "Apa Itu FreeSync (AMD)?", "id": "Teknologi VSync Adaptif AMD." },
{ "en": "Apa Itu Dot Pitch?", "id": "Jarak Antar Pixel Layar Monitor." },
{ "en": "Apa Itu Anti-Aliasing?", "id": "Teknik Menghaluskan Tepi Bergerigi Grafis." },
{ "en": "Apa Itu Driver Grafis?", "id": "Software Penghubung OS Kartu Grafis." }



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
