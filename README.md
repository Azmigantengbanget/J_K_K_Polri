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


  { "en": "Kepanjangan LAN?", "id": "Local Area Network." },
  { "en": "Kepanjangan WAN?", "id": "Wide Area Network." },
  { "en": "Fungsi utama router?", "id": "Menghubungkan dua jaringan atau lebih." },
  { "en": "Fungsi utama switch?", "id": "Menghubungkan perangkat dalam satu LAN." },
  { "en": "Topologi paling umum digunakan?", "id": "Topologi Star (Bintang)." },
  { "en": "Kepanjangan IP address?", "id": "Internet Protocol address." },
  { "en": "Model referensi jaringan komputer?", "id": "OSI dan TCP/IP." },
  { "en": "Berapa lapis model OSI?", "id": "Tujuh lapis (layer)." },
  { "en": "Lapisan teratas pada OSI?", "id": "Application Layer (Lapisan Aplikasi)." },
  { "en": "Lapisan terbawah pada OSI?", "id": "Physical Layer (Lapisan Fisik)." },
  { "en": "Fungsi utama firewall?", "id": "Menyaring lalu lintas jaringan." },
  { "en": "Definisi malware?", "id": "Perangkat lunak perusak sistem." },
  { "en": "Apa itu phishing?", "id": "Upaya penipuan untuk mencuri data." },
  { "en": "Fungsi utama antivirus?", "id": "Mendeteksi dan menghapus virus." },
  { "en": "Trinitas keamanan informasi?", "id": "Confidentiality, Integrity, Availability (CIA)." },
  { "en": "Arti Confidentiality?", "id": "Menjaga kerahasiaan data atau informasi." },
  { "en": "Arti Integrity?", "id": "Menjamin data akurat tidak diubah." },
  { "en": "Arti Availability?", "id": "Menjamin sistem selalu bisa diakses." },
  { "en": "Kepanjangan TCP?", "id": "Transmission Control Protocol." },
  { "en": "Kepanjangan UDP?", "id": "User Datagram Protocol." },
  { "en": "Perbedaan utama TCP/UDP?", "id": "TCP andal, UDP sangat cepat." },
  { "en": "Kepanjangan DNS?", "id": "Domain Name System." },
  { "en": "Fungsi utama DNS?", "id": "Menerjemahkan nama domain ke IP." },
  { "en": "Kepanjangan HTTP?", "id": "Hypertext Transfer Protocol." },
  { "en": "Kepanjangan HTTPS?", "id": "Hypertext Transfer Protocol Secure." },
  { "en": "Kelebihan utama HTTPS?", "id": "Komunikasi terenkripsi, lebih aman." },
  { "en": "Kepanjangan VPN?", "id": "Virtual Private Network." },
  { "en": "Fungsi utama VPN Polri?", "id": "Membuat koneksi aman ke jaringan internal." },
  { "en": "Definisi enkripsi?", "id": "Proses mengamankan data dengan sandi." },
  { "en": "Kebalikan dari enkripsi?", "id": "Dekripsi, membuka data terenkripsi." },
  { "en": "Apa itu MAC address?", "id": "Alamat fisik unik perangkat keras." },
  { "en": "Perangkat pemancar sinyal Wi-Fi?", "id": "Access Point (AP)." },
  { "en": "Standar keamanan Wi-Fi terkini?", "id": "WPA3 (Wi-Fi Protected Access 3)." },
  { "en": "Definisi server?", "id": "Komputer penyedia layanan di jaringan." },
  { "en": "Definisi client?", "id": "Komputer pengguna layanan di jaringan." },
  { "en": "Kepanjangan FTP?", "id": "File Transfer Protocol." },
  { "en": "Fungsi utama FTP?", "id": "Untuk transfer file antar komputer." },
  { "en": "Apa itu 'packet'?", "id": "Unit data kecil di jaringan." },
  { "en": "Apa itu 'bandwidth'?", "id": "Kapasitas maksimal transfer data." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda pengiriman data." },
  { "en": "Penyebab 'IP conflict'?", "id": "Dua perangkat punya IP sama." },
  { "en": "Kepanjangan DHCP?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Fungsi utama DHCP?", "id": "Memberikan alamat IP secara otomatis." },
  { "en": "Apa itu 'gateway'?", "id": "Gerbang keluar dari jaringan lokal." },
  { "en": "Apa itu 'subnet mask'?", "id": "Membedakan network ID dan host ID." },
  { "en": "Pusat data informasi Polri?", "id": "Pusdatin Polri." },
  { "en": "Unit siber Polri?", "id": "Direktorat Tindak Pidana Siber (Dittipidsiber)." },
  { "en": "Definisi 'digital forensic'?", "id": "Forensik pada barang bukti digital." },
  { "en": "Serangan 'Denial-of-Service' (DoS)?", "id": "Membanjiri server hingga tidak bisa diakses." },
  { "en": "Apa itu 'port' jaringan?", "id": "Pintu virtual untuk layanan spesifik." },
  { "en": "Nomor port untuk HTTP?", "id": "Port 80." },
  { "en": "Nomor port untuk HTTPS?", "id": "Port 443." },
  { "en": "Apa itu 'patch' keamanan?", "id": "Perbaikan untuk celah keamanan software." },
  { "en": "Definisi 'vulnerability'?", "id": "Celah keamanan pada sistem." },
  { "en": "Definisi 'exploit'?", "id": "Kode yang memanfaatkan celah keamanan." },
  { "en": "Apa itu 'zero-day attack'?", "id": "Serangan pada celah belum diketahui." },
  { "en": "Definisi 'authentication'?", "id": "Proses verifikasi identitas pengguna." },
  { "en": "Contoh metode autentikasi?", "id": "Password, sidik jari, kartu akses." },
  { "en": "Definisi 'authorization'?", "id": "Proses pemberian hak akses pengguna." },
  { "en": "Definisi 'spyware'?", "id": "Malware untuk memata-matai aktivitas pengguna." },
  { "en": "Definisi 'adware'?", "id": "Malware yang menampilkan iklan paksa." },
  { "en": "Definisi 'ransomware'?", "id": "Malware yang menyandera data minta tebusan." },
  { "en": "Definisi 'trojan horse'?", "id": "Malware berkedok program yang sah." },
  { "en": "Definisi 'worm'?", "id": "Malware yang mereplikasi diri antar jaringan." },
  { "en": "Apa itu 'social engineering'?", "id": "Memanipulasi psikologis untuk dapat informasi." },
  { "en": "Apa itu 'brute-force attack'?", "id": "Mencoba semua kemungkinan kombinasi password." },
  { "en": "Kepanjangan IDS?", "id": "Intrusion Detection System." },
  { "en": "Fungsi utama IDS?", "id": "Mendeteksi aktivitas mencurigakan di jaringan." },
  { "en": "Kepanjangan IPS?", "id": "Intrusion Prevention System." },
  { "en": "Fungsi utama IPS?", "id": "Mendeteksi dan langsung memblokir ancaman." },
  { "en": "Apa itu 'honey pot'?", "id": "Sistem umpan untuk menjebak peretas." },
  { "en": "Definisi 'log' keamanan?", "id": "Catatan semua aktivitas dalam sistem." },
  { "en": "Pentingnya 'log' keamanan?", "id": "Untuk audit dan investigasi insiden." },
  { "en": "Definisi 'backup' data?", "id": "Salinan atau cadangan data penting." },
  { "en": "Pentingnya backup rutin?", "id": "Mencegah kehilangan data secara permanen." },
  { "en": "Definisi 'disaster recovery plan'?", "id": "Rencana pemulihan sistem pasca bencana." },
  { "en": "Definisi 'biometrics'?", "id": "Autentikasi menggunakan ciri fisik unik." },
  { "en": "Kepanjangan 2FA?", "id": "Two-Factor Authentication." },
  { "en": "Contoh implementasi 2FA?", "id": "Password dan kode OTP dari HP." },
  { "en": "Protokol internet versi 4?", "id": "IPv4." },
  { "en": "Protokol internet versi 6?", "id": "IPv6." },
  { "en": "Kelebihan utama IPv6?", "id": "Alamat lebih banyak, lebih aman." },
  { "en": "Perintah tes konektivitas jaringan?", "id": "Ping." },
  { "en": "Perintah melacak rute paket?", "id": "Traceroute atau tracert." },
  { "en": "Definisi 'proxy server'?", "id": "Server perantara antara client-internet." },
  { "en": "Fungsi caching pada proxy?", "id": "Menyimpan data, percepat akses." },
  { "en": "Kepanjangan SSID?", "id": "Service Set Identifier." },
  { "en": "Fungsi utama SSID?", "id": "Nama dari sebuah jaringan Wi-Fi." },
  { "en": "Apa itu 'hiding SSID'?", "id": "Menyembunyikan nama jaringan Wi-Fi." },
  { "en": "Definisi 'MAC filtering'?", "id": "Membatasi akses jaringan via MAC." },
  { "en": "Apa itu 'botnet'?", "id": "Jaringan komputer zombie dikendalikan peretas." },
  { "en": "Definisi 'data breach'?", "id": "Insiden kebocoran data sensitif." },
  { "en": "Media transmisi jaringan?", "id": "Kabel tembaga, fiber optik, nirkabel." },
  { "en": "Kabel UTP singkatan dari?", "id": "Unshielded Twisted Pair." },
  { "en": "Kelebihan fiber optik?", "id": "Kecepatan sangat tinggi, anti interferensi." },
  { "en": "Fungsi zona DMZ?", "id": "Area isolasi untuk server publik." },
  { "en": "Kepanjangan SIEM?", "id": "Security Information and Event Management." },
  { "en": "Tujuan utama sistem SIEM?", "id": "Analisa log keamanan secara terpusat." },
  { "en": "Apa itu enkripsi simetris?", "id": "Satu kunci untuk enkripsi-dekripsi." },
  { "en": "Apa itu enkripsi asimetris?", "id": "Dua kunci: publik dan privat." },
  { "en": "Contoh algoritma simetris?", "id": "AES (Advanced Encryption Standard)." },
  { "en": "Contoh algoritma asimetris?", "id": "RSA (Rivest-Shamir-Adleman)." },
  { "en": "Apa itu 'hashing'?", "id": "Mengubah data menjadi string unik." },
  { "en": "Fungsi utama 'hashing'?", "id": "Memverifikasi integritas data." },
  { "en": "Contoh algoritma hash?", "id": "SHA-256, MD5." },
  { "en": "Definisi 'digital signature'?", "id": "Tanda tangan digital untuk autentikasi." },
  { "en": "Kepanjangan PKI?", "id": "Public Key Infrastructure." },
  { "en": "Fungsi PKI?", "id": "Mengelola sertifikat dan kunci digital." },
  { "en": "Kepanjangan SSL/TLS?", "id": "Secure Sockets Layer / Transport Layer Security." },
  { "en": "Fungsi SSL/TLS?", "id": "Mengamankan koneksi website (HTTPS)." },
  { "en": "Kepanjangan ARP?", "id": "Address Resolution Protocol." },
  { "en": "Fungsi utama ARP?", "id": "Memetakan IP address ke MAC address." },
  { "en": "Kepanjangan ICMP?", "id": "Internet Control Message Protocol." },
  { "en": "Perintah yang menggunakan ICMP?", "id": "Ping dan traceroute." },
  { "en": "Protokol email untuk mengirim?", "id": "SMTP (Simple Mail Transfer Protocol)." },
  { "en": "Protokol email untuk menerima?", "id": "POP3 atau IMAP." },
  { "en": "Kepanjangan NTP?", "id": "Network Time Protocol." },
  { "en": "Fungsi NTP?", "id": "Sinkronisasi waktu di semua perangkat." },
  { "en": "Kepanjangan SNMP?", "id": "Simple Network Management Protocol." },
  { "en": "Fungsi SNMP?", "id": "Memantau dan mengelola perangkat jaringan." },
  { "en": "Kepanjangan VLAN?", "id": "Virtual Local Area Network." },
  { "en": "Fungsi utama VLAN?", "id": "Segmentasi logis jaringan LAN fisik." },
  { "en": "Kepanjangan QoS?", "id": "Quality of Service." },
  { "en": "Tujuan QoS?", "id": "Memberi prioritas pada trafik penting." },
  { "en": "Contoh trafik prioritas QoS?", "id": "Video conference, panggilan suara (VoIP)." },
  { "en": "Apa itu 'port security'?", "id": "Membatasi akses port switch." },
  { "en": "Kepanjangan Pusinafis?", "id": "Pusat Indonesia Automatic Fingerprint Identification System." },
  { "en": "Tugas utama Pusinafis?", "id": "Mengelola data identifikasi sidik jari." },
  { "en": "Kepanjangan ETLE?", "id": "Electronic Traffic Law Enforcement." },
  { "en": "Tantangan keamanan ETLE?", "id": "Melindungi data pelanggaran dan privasi." },
  { "en": "Apa itu 'incident response'?", "id": "Prosedur penanganan insiden keamanan siber." },
  { "en": "Tahap pertama 'incident response'?", "id": "Preparation (Persiapan)." },
  { "en": "Tahap terakhir 'incident response'?", "id": "Lessons Learned (Pembelajaran)." },
  { "en": "Apa itu serangan 'MitM'?", "id": "Man-in-the-Middle, penyadapan di tengah." },
  { "en": "Apa itu 'SQL Injection'?", "id": "Menyisipkan perintah SQL jahat." },
  { "en": "Cara cegah 'SQL Injection'?", "id": "Gunakan prepared statements atau validasi input." },
  { "en": "Apa itu 'Cross-Site Scripting'?", "id": "Menyisipkan script jahat di website." },
  { "en": "Beda DoS dan DDoS?", "id": "DoS satu sumber, DDoS banyak sumber." },
  { "en": "Kepanjangan DDoS?", "id": "Distributed Denial-of-Service." },
  { "en": "Apa itu 'penetration testing'?", "id": "Uji coba peretasan yang sah." },
  { "en": "Tujuan 'penetration testing'?", "id": "Menemukan celah keamanan sistem." },
  { "en": "Definisi 'red team'?", "id": "Tim yang berperan sebagai penyerang." },
  { "en": "Definisi 'blue team'?", "id": "Tim yang berperan sebagai bertahan." },
  { "en": "Definisi 'purple team'?", "id": "Kolaborasi antara red dan blue team." },
  { "en": "Apa itu 'data masking'?", "id": "Menyembunyikan data asli dengan data palsu." },
  { "en": "Apa itu 'tokenization'?", "id": "Mengganti data sensitif dengan token." },
  { "en": "Apa itu 'sandboxing'?", "id": "Menjalankan aplikasi di lingkungan terisolasi." },
  { "en": "Tujuan 'sandboxing'?", "id": "Mencegah program jahat merusak sistem." },
  { "en": "Apa itu 'air gapping'?", "id": "Mengisolasi komputer total dari jaringan." },
  { "en": "Apa itu 'principle of least privilege'?", "id": "Beri hak akses seminimal mungkin." },
  { "en": "Apa itu 'defense in depth'?", "id": "Strategi keamanan berlapis-lapis." },
  { "en": "Contoh 'defense in depth'?", "id": "Firewall, IPS, antivirus, enkripsi." },
  { "en": "Apa itu 'threat intelligence'?", "id": "Informasi tentang ancaman siber terkini." },
  { "en": "Penyebab 'human error'?", "id": "Kelalaian, kurangnya pelatihan keamanan." },
  { "en": "Apa itu 'tailgating'?", "id": "Masuk area aman tanpa otorisasi." },
  { "en": "Cara cegah 'tailgating'?", "id": "Gunakan access card, security guard." },
  { "en": "Apa itu 'dumpster diving'?", "id": "Mencari informasi dari tempat sampah." },
  { "en": "Definisi 'rootkit'?", "id": "Malware yang bersembunyi di level sistem." },
  { "en": "Definisi 'keylogger'?", "id": "Malware yang merekam setiap ketikan." },
  { "en": "Apa itu 'pharming'?", "id": "Mengarahkan trafik website ke situs palsu." },
  { "en": "Apa itu 'whaling'?", "id": "Phishing yang menargetkan pejabat tinggi." },
  { "en": "Apa itu 'vishing'?", "id": "Phishing yang dilakukan melalui telepon." },
  { "en": "Apa itu 'smishing'?", "id": "Phishing yang dilakukan melalui SMS." },
  { "en": "Kepanjangan WAF?", "id": "Web Application Firewall." },
  { "en": "Fungsi WAF?", "id": "Melindungi aplikasi web dari serangan." },
  { "en": "Apa itu 'port scanning'?", "id": "Memindai port terbuka pada sistem." },
  { "en": "Tools untuk 'port scanning'?", "id": "Nmap." },
  { "en": "Apa itu 'packet sniffing'?", "id": "Menangkap dan menganalisa paket data." },
  { "en": "Tools untuk 'packet sniffing'?", "id": "Wireshark." },
  { "en": "Apa itu 'session hijacking'?", "id": "Mengambil alih sesi pengguna." },
  { "en": "Apa itu 'cookie poisoning'?", "id": "Memodifikasi cookie untuk membajak sesi." },
  { "en": "Apa itu 'URL filtering'?", "id": "Memblokir akses ke situs web tertentu." },
  { "en": "Apa itu 'content filtering'?", "id": "Memblokir konten berdasarkan kata kunci." },
  { "en": "Definisi 'geofencing'?", "id": "Membatasi akses berdasarkan lokasi geografis." },
  { "en": "Apa itu 'access control list'?", "id": "Daftar aturan izin akses jaringan." },
  { "en": "Pusat komando siber Polri?", "id": "Command Center Dittipidsiber." },
  { "en": "Apa itu 'chain of custody'?", "id": "Dokumentasi riwayat penanganan barang bukti." },
  { "en": "Pentingnya 'chain of custody'?", "id": "Menjaga keaslian barang bukti digital." },
  { "en": "Apa itu 'file hashing' forensik?", "id": "Memastikan file tidak berubah." },
  { "en": "Apa itu 'write blocker'?", "id": "Mencegah penulisan data ke bukti." },
  { "en": "Apa itu 'RAM forensics'?", "id": "Analisa data dari memori RAM." },
  { "en": "Apa itu 'steganography'?", "id": "Menyembunyikan pesan dalam media lain." },
  { "en": "Definisi 'cryptojacking'?", "id": "Penggunaan sumber daya korban untuk mining." },
  { "en": "Apa itu 'deepfake'?", "id": "Video atau audio manipulasi AI." },
  { "en": "Ancaman 'deepfake' bagi Polri?", "id": "Disinformasi, pencemaran nama baik." },
  { "en": "Apa itu 'threat actor'?", "id": "Individu atau grup pelaku ancaman." },
  { "en": "Contoh 'threat actor'?", "id": "Peretas, aktivis, teroris siber." },
  { "en": "Tujuan 'ethical hacking'?", "id": "Mengidentifikasi kelemahan untuk diperbaiki." },
  { "en": "Apa itu 'bug bounty program'?", "id": "Program hadiah untuk penemu bug." },
  { "en": "Tujuan kebijakan medsos anggota?", "id": "Jaga citra institusi, cegah kebocoran." },
  { "en": "Definisi kebijakan BYOD?", "id": "Bring Your Own Device." },
  { "en": "Risiko kebijakan BYOD?", "id": "Keamanan data institusi lebih rentan." },
  { "en": "Contoh klasifikasi data Polri?", "id": "Sangat Rahasia, Rahasia, Terbatas, Biasa." },
  { "en": "Tujuan audit keamanan sistem?", "id": "Mengevaluasi efektivitas kontrol keamanan." },
  { "en": "Definisi 'risk management'?", "id": "Proses mengelola risiko keamanan informasi." },
  { "en": "Apa itu forensik jaringan?", "id": "Investigasi lalu lintas jaringan." },
  { "en": "Apa itu forensik seluler?", "id": "Proses investigasi pada perangkat mobile." },
  { "en": "Tantangan forensik seluler?", "id": "Enkripsi, beragam OS, kerusakan fisik." },
  { "en": "Apa itu forensik cloud?", "id": "Investigasi data di lingkungan cloud." },
  { "en": "Tantangan forensik cloud?", "id": "Masalah yurisdiksi dan akses data." },
  { "en": "Apa itu 'static analysis' malware?", "id": "Analisa kode tanpa menjalankan program." },
  { "en": "Apa itu 'dynamic analysis' malware?", "id": "Analisa perilaku saat program dijalankan." },
  { "en": "Tujuan 'imaging' barang bukti?", "id": "Membuat salinan bit-per-bit identik." },
  { "en": "Kenapa bekerja pada salinan/image?", "id": "Agar barang bukti asli tidak berubah." },
  { "en": "Fungsi kontrol akses biometrik?", "id": "Batasi akses ruang server via sidik jari." },
  { "en": "Sistem pemadam api server?", "id": "Gas bersih (clean agent) seperti FM-200." },
  { "en": "Kenapa tidak pakai air?", "id": "Air akan merusak semua perangkat elektronik." },
  { "en": "Fungsi UPS di pusat data?", "id": "Catu daya darurat sementara." },
  { "en": "Fungsi genset di pusat data?", "id": "Catu daya cadangan jangka panjang." },
  { "en": "Tugas utama patroli siber?", "id": "Memonitor konten negatif di internet." },
  { "en": "Tujuan 'cyber counter-intelligence'?", "id": "Melawan spionase siber dari lawan." },
  { "en": "Tugas tim 'online undercover'?", "id": "Menyusup ke dalam grup kriminal online." },
  { "en": "Apa itu 'social media analytics'?", "id": "Analisa tren dan sentimen publik." },
  { "en": "Dasar hukum utama kejahatan siber?", "id": "Undang-Undang ITE." },
  { "en": "Syarat penyadapan yang legal?", "id": "Harus ada izin dari pengadilan." },
  { "en": "Landasan hukum perlindungan data?", "id": "Undang-Undang Perlindungan Data Pribadi (PDP)." },
  { "en": "Contoh kerja sama siber internasional?", "id": "Dengan Interpol, FBI, atau kepolisian negara lain." },
  { "en": "Definisi 'acceptable use policy'?", "id": "Aturan penggunaan aset IT institusi." },
  { "en": "Konsekuensi pelanggaran AUP?", "id": "Sanksi disiplin hingga pemecatan." },
  { "en": "Apa itu 'data retention policy'?", "id": "Kebijakan berapa lama data disimpan." },
  { "en": "Apa itu 'data disposal policy'?", "id": "Prosedur pemusnahan data secara aman." },
  { "en": "Metode pemusnahan hard disk?", "id": "Degaussing atau dihancurkan secara fisik." },
  { "en": "Definisi 'need-to-know basis'?", "id": "Akses data hanya jika benar-benar perlu." },
  { "en": "Apa itu 'separation of duties'?", "id": "Pemisahan tugas untuk cegah penyalahgunaan." },
  { "en": "Apa itu 'security awareness training'?", "id": "Pelatihan kesadaran keamanan bagi personel." },
  { "en": "Topik utama 'security awareness'?", "id": "Phishing, password kuat, keamanan fisik." },
  { "en": "Definisi 'insider threat'?", "id": "Ancaman keamanan dari dalam institusi." },
  { "en": "Motif 'insider threat'?", "id": "Kecewa, sabotase, atau keuntungan finansial." },
  { "en": "Apa itu 'system hardening'?", "id": "Mengurangi permukaan serangan sistem." },
  { "en": "Contoh 'system hardening'?", "id": "Matikan port, uninstall software tidak perlu." },
  { "en": "Apa itu 'network segmentation'?", "id": "Memecah jaringan besar menjadi kecil." },
  { "en": "Tujuan 'network segmentation'?", "id": "Membatasi penyebaran serangan jika terjadi." },
  { "en": "Definisi 'zero trust architecture'?", "id": "Jangan percaya siapapun, selalu verifikasi." },
  { "en": "Prinsip utama 'zero trust'?", "id": "Never trust, always verify." },
  { "en": "Apa itu 'mobile device management'?", "id": "Manajemen keamanan untuk perangkat mobile." },
  { "en": "Fungsi MDM Polri?", "id": "Kontrol aplikasi, hapus data jarak jauh." },
  { "en": "Apa itu 'information sharing center'?", "id": "Pusat berbagi informasi ancaman siber." },
  { "en": "Definisi 'dark web'?", "id": "Bagian internet butuh software khusus." },
  { "en": "Aktivitas ilegal di 'dark web'?", "id": "Jual beli narkoba, data curian." },
  { "en": "Tujuan patroli 'dark web'?", "id": "Mengungkap jaringan kejahatan terorganisir." },
  { "en": "Apa itu 'cryptocurrency'?", "id": "Mata uang digital terdesentralisasi." },
  { "en": "Tantangan 'cryptocurrency' bagi Polri?", "id": "Sulit dilacak untuk pencucian uang." },
  { "en": "Apa itu 'cryptocurrency mixing'?", "id": "Layanan untuk mengaburkan jejak transaksi." },
  { "en": "Definisi 'OSINT'?", "id": "Open-Source Intelligence." },
  { "en": "Sumber data OSINT?", "id": "Media sosial, berita, situs publik." },
  { "en": "Apa itu 'SOC'?", "id": "Security Operations Center." },
  { "en": "Tugas utama analis SOC?", "id": "Memantau, mendeteksi, merespon ancaman." },
  { "en": "Apa itu 'threat hunting'?", "id": "Proaktif mencari ancaman dalam jaringan." },
  { "en": "Beda 'threat hunting' & SOC?", "id": "Hunting proaktif, SOC reaktif." },
  { "en": "Apa itu 'indicators of compromise'?", "id": "Tanda-tanda sistem telah disusupi." },
  { "en": "Contoh IOC?", "id": "Alamat IP jahat, hash file malware." },
  { "en": "Apa itu 'indicators of attack'?", "id": "Tanda-tanda sebuah serangan sedang berlangsung." },
  { "en": "Apa itu 'attack vector'?", "id": "Jalur atau cara serangan dilakukan." },
  { "en": "Contoh 'attack vector'?", "id": "Email phishing, celah keamanan software." },
  { "en": "Apa itu 'attack surface'?", "id": "Total area yang bisa diserang." },
  { "en": "Apa itu 'lateral movement'?", "id": "Pergerakan peretas di dalam jaringan." },
  { "en": "Tujuan 'lateral movement'?", "id": "Mencari data berharga, memperluas akses." },
  { "en": "Apa itu 'privilege escalation'?", "id": "Meningkatkan hak akses dari user biasa." },
  { "en": "Apa itu 'data exfiltration'?", "id": "Proses pencurian dan pengiriman data keluar." },
  { "en": "Apa itu 'command and control' server?", "id": "Server pengendali malware atau botnet." },
  { "en": "Apa itu 'firewall rule'?", "id": "Aturan spesifik untuk blokir/izinkan trafik." },
  { "en": "Aturan implisit terakhir firewall?", "id": "Implicit Deny (Tolak Semua)." },
  { "en": "Apa itu 'stateless firewall'?", "id": "Memeriksa paket secara individual." },
  { "en": "Apa itu 'stateful firewall'?", "id": "Memonitor status koneksi jaringan." },
  { "en": "Mana yang lebih aman?", "id": "Stateful firewall." },
  { "en": "Apa itu 'next-generation firewall'?", "id": "Firewall dengan fitur lebih canggih." },
  { "en": "Fitur NGFW?", "id": "IPS, kontrol aplikasi, inspeksi SSL." },
  { "en": "Apa itu 'virtual security appliance'?", "id": "Perangkat keamanan dalam bentuk software." },
  { "en": "Apa itu 'security baseline'?", "id": "Standar konfigurasi keamanan minimum." },
  { "en": "Tujuan 'security baseline'?", "id": "Memastikan semua sistem punya dasar aman." },
  { "en": "Apa itu 'change management'?", "id": "Prosedur formal untuk perubahan sistem." },
  { "en": "Tujuan 'change management'?", "id": "Mencegah error akibat perubahan sembarangan." },
  { "en": "Apa itu 'digital watermark'?", "id": "Tanda digital tersembunyi pada file." },
  { "en": "Fungsi 'digital watermark'?", "id": "Membuktikan kepemilikan, melacak kebocoran." },
  { "en": "Apa itu 'data loss prevention'?", "id": "Sistem pencegah kebocoran data." },
  { "en": "Cara kerja DLP?", "id": "Memantau, mendeteksi, memblokir transfer data." },
  { "en": "Peran 'forensic image' di pengadilan?", "id": "Sebagai duplikat sah barang bukti." },
  { "en": "Apa itu 'time stamping'?", "id": "Memberi cap waktu pada data/log." },
  { "en": "Pentingnya 'time stamping'?", "id": "Membuktikan waktu kejadian secara akurat." },
  { "en": "Apa itu 'RAID'?", "id": "Redundant Array of Independent Disks." },
  { "en": "Fungsi 'RAID' di server?", "id": "Meningkatkan performa dan redundansi data." },
  { "en": "Kepanjangan ERI Polri?", "id": "Electronic Registration and Identification." },
  { "en": "Fungsi utama sistem ERI?", "id": "Registrasi dan identifikasi kendaraan bermotor." },
  { "en": "Jaringan pada sistem Mambis?", "id": "Koneksi aman ke database biometrik." },
  { "en": "Keamanan aplikasi Polri Super App?", "id": "Enkripsi data, autentikasi pengguna." },
  { "en": "Beda utama hashing dan enkripsi?", "id": "Enkripsi bisa dibalik, hashing tidak." },
  { "en": "Kegunaan enkripsi simetris?", "id": "Enkripsi data dalam volume besar." },
  { "en": "Kegunaan enkripsi asimetris?", "id": "Pertukaran kunci aman, tanda tangan digital." },
  { "en": "Beda utama VPN dan Proxy?", "id": "VPN mengenkripsi semua trafik." },
  { "en": "Beda virus dan worm?", "id": "Worm menyebar sendiri tanpa host." },
  { "en": "Tujuan 'Risk Assessment'?", "id": "Mengidentifikasi dan mengevaluasi risiko keamanan." },
  { "en": "Tujuan 'Business Impact Analysis'?", "id": "Mengukur dampak gangguan pada operasi." },
  { "en": "Apa itu standar ISO 27001?", "id": "Standar manajemen keamanan informasi (ISMS)." },
  { "en": "Prinsip dasar UU PDP?", "id": "Melindungi hak privasi data pribadi." },
  { "en": "Definisi 'data controller'?", "id": "Pihak yang mengendalikan pemrosesan data." },
  { "en": "Definisi 'data processor'?", "id": "Pihak yang memproses data." },
  { "en": "Kepanjangan IaaS?", "id": "Infrastructure as a Service." },
  { "en": "Kepanjangan PaaS?", "id": "Platform as a Service." },
  { "en": "Kepanjangan SaaS?", "id": "Software as a Service." },
  { "en": "Contoh layanan SaaS?", "id": "Google Mail, Microsoft 365." },
  { "en": "Risiko keamanan utama pada cloud?", "id": "Konfigurasi salah, akses tidak sah." },
  { "en": "Keamanan pada kontainer Docker?", "id": "Image scanning, network policies." },
  { "en": "Definisi 'Virtual Desktop Infrastructure'?", "id": "Menjalankan desktop di server pusat." },
  { "en": "Kepanjangan VDI?", "id": "Virtual Desktop Infrastructure." },
  { "en": "Apa itu hypervisor?", "id": "Software untuk membuat mesin virtual." },
  { "en": "Beda hypervisor tipe 1 & 2?", "id": "Tipe 1 langsung, Tipe 2 di OS." },
  { "en": "Risiko keamanan utama IoT?", "id": "Perangkat lemah, mudah diretas." },
  { "en": "Keamanan pada 'Body Worn Camera'?", "id": "Enkripsi rekaman, kontrol akses." },
  { "en": "Definisi 'memory forensics'?", "id": "Analisa bukti digital dari RAM." },
  { "en": "Apa itu 'live forensics'?", "id": "Analisa pada sistem yang menyala." },
  { "en": "Definisi 'cyber kill chain'?", "id": "Tahapan dalam sebuah serangan siber." },
  { "en": "Tahap pertama 'kill chain'?", "id": "Reconnaissance (Pengintaian)." },
  { "en": "Tahap terakhir 'kill chain'?", "id": "Actions on Objectives (Tujuan akhir)." },
  { "en": "Definisi 'honeypot' tingkat lanjut?", "id": "Sistem umpan untuk studi metode peretas." },
  { "en": "Apa itu 'honeynet'?", "id": "Jaringan berisi beberapa sistem honeypot." },
  { "en": "Definisi 'sinkhole' DNS?", "id": "Mengarahkan trafik jahat ke server aman." },
  { "en": "Apa itu 'domain generation algorithm'?", "id": "Malware membuat nama domain acak." },
  { "en": "Tujuan DGA pada malware?", "id": "Menghindari blokir server C2." },
  { "en": "Apa itu 'fast flux' DNS?", "id": "Teknik ganti IP address cepat." },
  { "en": "Tujuan 'fast flux'?", "id": "Menyembunyikan lokasi server sebenarnya." },
  { "en": "Apa itu 'header' email?", "id": "Informasi teknis pengiriman sebuah email." },
  { "en": "Fungsi analisa 'header' email?", "id": "Melacak asal-usul email phishing." },
  { "en": "Apa itu 'SPF' record?", "id": "Mencegah pemalsuan alamat email pengirim." },
  { "en": "Kepanjangan SPF?", "id": "Sender Policy Framework." },
  { "en": "Apa itu 'DKIM'?", "id": "Menambahkan tanda tangan digital ke email." },
  { "en": "Kepanjangan DKIM?", "id": "DomainKeys Identified Mail." },
  { "en": "Apa itu 'DMARC'?", "id": "Kebijakan autentikasi email." },
  { "en": "Fungsi DMARC?", "id": "Melindungi dari spoofing dan phishing." },
  { "en": "Apa itu 'file carving'?", "id": "Memulihkan file dari data mentah." },
  { "en": "Apa itu 'slack space'?", "id": "Area kosong di akhir file." },
  { "en": "Fungsi analisa 'slack space'?", "id": "Menemukan data tersembunyi." },
  { "en": "Apa itu 'metadata' file?", "id": "Informasi tentang sebuah file." },
  { "en": "Contoh metadata?", "id": "Tanggal dibuat, tipe kamera, lokasi." },
  { "en": "Tools untuk ekstraksi metadata?", "id": "ExifTool." },
  { "en": "Apa itu 'load balancer'?", "id": "Membagi beban trafik ke beberapa server." },
  { "en": "Manfaat 'load balancer'?", "id": "Meningkatkan ketersediaan dan performa." },
  { "en": "Apa itu 'failover'?", "id": "Sistem cadangan mengambil alih otomatis." },
  { "en": "Apa itu 'high availability'?", "id": "Sistem dirancang minim downtime." },
  { "en": "Definisi 'single point of failure'?", "id": "Satu komponen gagal, sistem mati." },
  { "en": "Cara hindari 'SPOF'?", "id": "Gunakan redundansi dan failover." },
  { "en": "Apa itu 'API'?", "id": "Application Programming Interface." },
  { "en": "Risiko keamanan API?", "id": "Akses tidak sah, kebocoran data." },
  { "en": "Apa itu 'rate limiting' API?", "id": "Membatasi jumlah permintaan API." },
  { "en": "Tujuan 'rate limiting'?", "id": "Mencegah serangan Denial-of-Service." },
  { "en": "Apa itu 'containerization'?", "id": "Mengemas aplikasi dalam lingkungan terisolasi." },
  { "en": "Contoh platform 'containerization'?", "id": "Docker, Podman." },
  { "en": "Apa itu 'orchestration'?", "id": "Manajemen otomatis untuk banyak kontainer." },
  { "en": "Contoh platform 'orchestration'?", "id": "Kubernetes, Docker Swarm." },
  { "en": "Apa itu 'Infrastructure as Code'?", "id": "Mengelola infrastruktur melalui file konfigurasi." },
  { "en": "Manfaat 'IaC'?", "id": "Otomatisasi, konsistensi, pemulihan cepat." },
  { "en": "Apa itu 'DevSecOps'?", "id": "Mengintegrasikan keamanan dalam siklus DevOps." },
  { "en": "Prinsip 'DevSecOps'?", "id": "Keamanan adalah tanggung jawab semua." },
  { "en": "Apa itu 'supply chain attack'?", "id": "Menyerang target melalui software pihak ketiga." },
  { "en": "Contoh 'supply chain attack'?", "id": "Menyisipkan malware di library software." },
  { "en": "Apa itu 'disinformation'?", "id": "Penyebaran informasi palsu secara sengaja." },
  { "en": "Apa itu 'malinformation'?", "id": "Penyebaran informasi asli untuk merugikan." },
  { "en": "Tujuan 'digital attribution'?", "id": "Mengidentifikasi pelaku di balik serangan." },
  { "en": "Tantangan 'digital attribution'?", "id": "Pelaku sering menggunakan anonimitas." },
  { "en": "Apa itu 'clean desk policy'?", "id": "Kebijakan meja kerja bersih." },
  { "en": "Tujuan 'clean desk policy'?", "id": "Mencegah pencurian informasi fisik." },
  { "en": "Apa itu 'shoulder surfing'?", "id": "Mengintip layar untuk mencuri informasi." },
  { "en": "Cara cegah 'shoulder surfing'?", "id": "Gunakan privacy screen, waspada sekitar." },
  { "en": "Apa itu 'BIOS/UEFI password'?", "id": "Password untuk akses level firmware." },
  { "en": "Apa itu 'full disk encryption'?", "id": "Mengenkripsi seluruh isi hard drive." },
  { "en": "Contoh 'full disk encryption'?", "id": "BitLocker (Windows), FileVault (Mac)." },
  { "en": "Apa itu 'certificate authority'?", "id": "Entitas yang menerbitkan sertifikat digital." },
  { "en": "Fungsi 'Certificate Authority' (CA)?", "id": "Memverifikasi identitas pemilik sertifikat." },
  { "en": "Apa itu 'root certificate'?", "id": "Sertifikat digital paling terpercaya." },
  { "en": "Apa itu 'certificate pinning'?", "id": "Mengasosiasikan host dengan sertifikatnya." },
  { "en": "Tujuan 'certificate pinning'?", "id": "Mencegah serangan Man-in-the-Middle." },
  { "en": "Apa itu 'HTTP Strict Transport Security'?", "id": "Memaksa browser menggunakan HTTPS." },
  { "en": "Kepanjangan HSTS?", "id": "HTTP Strict Transport Security." },
  { "en": "Apa itu 'Content Security Policy'?", "id": "Mencegah serangan XSS dan injeksi." },
  { "en": "Kepanjangan CSP?", "id": "Content Security Policy." },
  { "en": "Lima fungsi kerangka NIST?", "id": "Identify, Protect, Detect, Respond, Recover." },
  { "en": "Tujuan kerangka NIST?", "id": "Membantu organisasi mengelola risiko siber." },
  { "en": "Apa itu MITRE ATT&CK?", "id": "Database taktik dan teknik peretas." },
  { "en": "Tujuan MITRE ATT&CK?", "id": "Memahami dan melawan metode penyerang." },
  { "en": "Definisi 'Cyber Resilience'?", "id": "Kemampuan sistem untuk pulih dari serangan." },
  { "en": "Apa itu kriptografi eliptik?", "id": "Enkripsi kuat dengan kunci lebih pendek." },
  { "en": "Kepanjangan ECC?", "id": "Elliptic Curve Cryptography." },
  { "en": "Konsep 'quantum cryptography'?", "id": "Keamanan berbasis prinsip fisika kuantum." },
  { "en": "Ancaman komputer kuantum?", "id": "Dapat memecahkan enkripsi klasik." },
  { "en": "Apa itu 'Perfect Forward Secrecy'?", "id": "Kunci sesi unik setiap koneksi." },
  { "en": "Tujuan PFS?", "id": "Melindungi sesi lampau jika kunci privat bocor." },
  { "en": "Apa itu 'salted hash'?", "id": "Menambahkan data acak sebelum hashing." },
  { "en": "Tujuan 'salting'?", "id": "Mencegah serangan 'rainbow table'." },
  { "en": "Kepanjangan SSO?", "id": "Single Sign-On." },
  { "en": "Keuntungan SSO?", "id": "Satu kali login untuk banyak aplikasi." },
  { "en": "Apa itu 'Federated Identity'?", "id": "Mempercayai identitas dari domain lain." },
  { "en": "Kepanjangan FIM?", "id": "Federated Identity Management." },
  { "en": "Definisi RBAC?", "id": "Akses berdasarkan peran atau jabatan." },
  { "en": "Definisi ABAC?", "id": "Akses berdasarkan atribut pengguna/sumber daya." },
  { "en": "Kepanjangan PAM?", "id": "Privileged Access Management." },
  { "en": "Tujuan PAM?", "id": "Mengelola dan mengawasi akun berhak istimewa." },
  { "en": "Apa itu 'reverse engineering' malware?", "id": "Membongkar kode untuk memahami malware." },
  { "en": "Apa itu 'packer' malware?", "id": "Alat untuk mengompres dan menyamarkan malware." },
  { "en": "Apa itu 'obfuscation'?", "id": "Membuat kode sulit dibaca manusia." },
  { "en": "Tujuan 'obfuscation' malware?", "id": "Menghindari deteksi oleh antivirus." },
  { "en": "Apa itu 'YARA rule'?", "id": "Pola untuk mengidentifikasi file malware." },
  { "en": "SOP keamanan Command Center?", "id": "Akses terbatas, monitor fisik, log ketat." },
  { "en": "Prosedur hapus data BWC?", "id": "Sesuai kebijakan retensi, penghapusan aman." },
  { "en": "Tantangan yurisdiksi siber?", "id": "Pelaku dan korban beda negara." },
  { "en": "Apa itu 'Mutual Legal Assistance'?", "id": "Perjanjian bantuan hukum antar negara." },
  { "en": "Risiko keamanan pada AI/ML?", "id": "Data poisoning, adversarial attacks." },
  { "en": "Apa itu 'data poisoning'?", "id": "Merusak data latih model AI." },
  { "en": "Apa itu 'adversarial attack' AI?", "id": "Input dirancang untuk menipu model AI." },
  { "en": "Keamanan pada drone Polri?", "id": "Enkripsi sinyal kendali dan video." },
  { "en": "Apa itu 'zero-day broker'?", "id": "Pihak yang menjual celah zero-day." },
  { "en": "Apa itu 'credential stuffing'?", "id": "Mencoba kredensial bocor di banyak situs." },
  { "en": "Beda 'credential stuffing' & 'brute-force'?", "id": "Stuffing coba data bocor, brute-force acak." },
  { "en": "Apa itu 'password spraying'?", "id": "Mencoba satu password ke banyak akun." },
  { "en": "Apa itu 'golden ticket attack'?", "id": "Serangan pada Microsoft Active Directory." },
  { "en": "Apa itu 'pass the hash'?", "id": "Menggunakan hash password untuk autentikasi." },
  { "en": "Apa itu 'living off the land'?", "id": "Menyerang pakai tools bawaan sistem." },
  { "en": "Tujuan 'living off the land'?", "id": "Menghindari deteksi, tampak seperti aktivitas normal." },
  { "en": "Apa itu 'fileless malware'?", "id": "Malware yang beroperasi di memori (RAM)." },
  { "en": "Tantangan 'fileless malware'?", "id": "Sangat sulit dideteksi antivirus tradisional." },
  { "en": "Apa itu 'DNS over HTTPS'?", "id": "Mengenkripsi kueri DNS via HTTPS." },
  { "en": "Kepanjangan DoH?", "id": "DNS over HTTPS." },
  { "en": "Apa itu 'DNS over TLS'?", "id": "Mengenkripsi kueri DNS via TLS." },
  { "en": "Kepanjangan DoT?", "id": "DNS over TLS." },
  { "en": "Apa itu 'DNSSEC'?", "id": "Memastikan respons DNS asli dan valid." },
  { "en": "Kepanjangan DNSSEC?", "id": "Domain Name System Security Extensions." },
  { "en": "Apa itu 'OS hardening'?", "id": "Proses mengamankan sistem operasi." },
  { "en": "Apa itu 'application whitelisting'?", "id": "Hanya izinkan aplikasi terpercaya berjalan." },
  { "en": "Apa itu 'application blacklisting'?", "id": "Melarang aplikasi berbahaya berjalan." },
  { "en": "Mana lebih aman, whitelisting/blacklisting?", "id": "Whitelisting lebih aman dan ketat." },
  { "en": "Apa itu 'sandnet'?", "id": "Jaringan terisolasi untuk analisis malware." },
  { "en": "Apa itu 'threat modeling'?", "id": "Proses identifikasi ancaman saat desain." },
  { "en": "Tujuan 'threat modeling'?", "id": "Membangun sistem aman sejak awal." },
  { "en": "Apa itu 'Fuzzing'?", "id": "Mengirim data acak untuk cari bug." },
  { "en": "Tujuan 'fuzz testing'?", "id": "Menemukan celah keamanan seperti buffer overflow." },
  { "en": "Apa itu 'buffer overflow'?", "id": "Menulis data melebihi kapasitas buffer." },
  { "en": "Apa itu 'canary value'?", "id": "Teknik deteksi serangan buffer overflow." },
  { "en": "Apa itu 'Address Space Layout Randomization'?", "id": "Mengacak lokasi memori program." },
  { "en": "Kepanjangan ASLR?", "id": "Address Space Layout Randomization." },
  { "en": "Tujuan ASLR?", "id": "Mempersulit eksploitasi celah memori." },
  { "en": "Apa itu 'Data Execution Prevention'?", "id": "Mencegah eksekusi kode dari area data." },
  { "en": "Kepanjangan DEP?", "id": "Data Execution Prevention." },
  { "en": "Apa itu 'tamper-proofing'?", "id": "Membuat sistem sulit untuk dimodifikasi." },
  { "en": "Apa itu 'security by design'?", "id": "Memasukkan keamanan sejak awal pengembangan." },
  { "en": "Apa itu 'security by default'?", "id": "Konfigurasi awal sistem paling aman." },
  { "en": "Definisi 'uptime'?", "id": "Waktu sistem beroperasi tanpa gangguan." },
  { "en": "Definisi 'downtime'?", "id": "Waktu sistem tidak dapat beroperasi." },
  { "en": "Apa itu 'Service Level Agreement'?", "id": "Perjanjian tingkat layanan dengan vendor." },
  { "en": "Kepanjangan SLA?", "id": "Service Level Agreement." },
  { "en": "Contoh isi SLA?", "id": "Jaminan uptime, waktu respons." },
  { "en": "Apa itu 'cold site'?", "id": "Situs pemulihan bencana tanpa peralatan." },
  { "en": "Apa itu 'warm site'?", "id": "Situs pemulihan dengan sebagian peralatan." },
  { "en": "Apa itu 'hot site'?", "id": "Situs pemulihan siap pakai." },
  { "en": "Mana pemulihan tercepat?", "id": "Hot site." },
  { "en": "Apa itu 'Recovery Time Objective'?", "id": "Target waktu maksimal untuk pemulihan." },
  { "en": "Kepanjangan RTO?", "id": "Recovery Time Objective." },
  { "en": "Apa itu 'Recovery Point Objective'?", "id": "Toleransi kehilangan data maksimal." },
  { "en": "Kepanjangan RPO?", "id": "Recovery Point Objective." },
  { "en": "Apa itu 'tabletop exercise'?", "id": "Latihan simulasi penanganan insiden." },
  { "en": "Tujuan 'tabletop exercise'?", "id": "Menguji kesiapan tim dan prosedur." },
  { "en": "Apa itu 'out-of-band management'?", "id": "Manajemen perangkat via jaringan terpisah." },
  { "en": "Tujuan 'out-of-band management'?", "id": "Akses perangkat saat jaringan utama mati." },
  { "en": "Apa itu 'agentless monitoring'?", "id": "Monitoring tanpa instal software di target." },
  { "en": "Apa itu 'agent-based monitoring'?", "id": "Monitoring dengan instal software di target." },
  { "en": "Apa itu 'log normalization'?", "id": "Mengubah format log menjadi standar." },
  { "en": "Apa itu 'log aggregation'?", "id": "Mengumpulkan log dari banyak sumber." },
  { "en": "Apa itu 'log correlation'?", "id": "Menghubungkan event dari log berbeda." },
  { "en": "Tujuan 'log correlation'?", "id": "Mendeteksi pola serangan yang kompleks." },
  { "en": "Beda 'chain of evidence' & 'custody'?", "id": "Evidence alur bukti, custody alur penanganan." },
  { "en": "Syarat bukti digital di pengadilan?", "id": "Asli, otentik, dan tidak berubah." },
  { "en": "Konvensi internasional kejahatan siber?", "id": "The Budapest Convention on Cybercrime." },
  { "en": "Dilema etis 'online undercover'?", "id": "Penipuan demi pengungkapan kejahatan." },
  { "en": "Apa itu 'key lifecycle management'?", "id": "Manajemen siklus hidup kunci kriptografi." },
  { "en": "Tahap 'key lifecycle'?", "id": "Pembuatan, distribusi, penyimpanan, pemusnahan." },
  { "en": "Kepanjangan HSM?", "id": "Hardware Security Module." },
  { "en": "Fungsi utama HSM?", "id": "Melindungi dan mengelola kunci digital." },
  { "en": "Apa itu 'Certificate Revocation List'?", "id": "Daftar sertifikat digital tidak valid." },
  { "en": "Kepanjangan CRL?", "id": "Certificate Revocation List." },
  { "en": "Apa itu 'OCSP'?", "id": "Protokol cek status sertifikat realtime." },
  { "en": "Beda CRL dan OCSP?", "id": "OCSP lebih cepat dan real-time." },
  { "en": "Tantangan forensik sistem SCADA?", "id": "Sistem lawas, tidak boleh ada downtime." },
  { "en": "Apa itu teknik 'anti-forensics'?", "id": "Upaya menghapus atau menyembunyikan bukti." },
  { "en": "Contoh teknik 'anti-forensics'?", "id": "Data wiping, enkripsi, steganografi." },
  { "en": "Apa itu 'timeline analysis'?", "id": "Menyusun urutan waktu kejadian digital." },
  { "en": "Kenapa 'social engineering' efektif?", "id": "Memanfaatkan kelemahan psikologis manusia." },
  { "en": "Faktor pendorong 'threat actor'?", "id": "Uang, politik, ideologi, atau ego." },
  { "en": "Apa itu 'security culture'?", "id": "Kesadaran keamanan menjadi kebiasaan." },
  { "en": "Respon ransomware dan data breach?", "id": "Isolasi sistem, investigasi, lapor pimpinan." },
  { "en": "SOP perangkat BYOD hilang?", "id": "Lapor segera, lakukan remote wipe." },
  { "en": "Beda 'state-sponsored' & 'cybercrime'?", "id": "Motivasi, sumber daya, dan targetnya." },
  { "en": "Apa itu 'write-once media'?", "id": "Media yang hanya bisa ditulis sekali." },
  { "en": "Contoh 'write-once media'?", "id": "CD-R, DVD-R." },
  { "en": "Kegunaannya dalam forensik?", "id": "Menyimpan image bukti agar tidak berubah." },
  { "en": "Apa itu 'order of volatility'?", "id": "Urutan pengumpulan bukti dari paling rentan." },
  { "en": "Contoh data paling 'volatile'?", "id": "Data di RAM dan register CPU." },
  { "en": "Apa itu 'memory dump'?", "id": "Proses menyalin seluruh isi RAM." },
  { "en": "Definisi 'active defense'?", "id": "Tindakan proaktif melawan penyerang." },
  { "en": "Contoh 'active defense'?", "id": "Honeypot, sinkhole, 'hack back'." },
  { "en": "Legalitas 'hack back'?", "id": "Umumnya ilegal, main hakim sendiri." },
  { "en": "Apa itu 'tarpitting'?", "id": "Sengaja memperlambat koneksi penyerang." },
  { "en": "Tujuan 'tarpitting'?", "id": "Menghabiskan waktu dan sumber daya penyerang." },
  { "en": "Apa itu 'pseudo-malware'?", "id": "Malware palsu untuk mengelabui analis." },
  { "en": "Apa itu 'polymorphic malware'?", "id": "Malware yang mengubah kodenya sendiri." },
  { "en": "Tujuan 'polymorphic malware'?", "id": "Menghindari deteksi berbasis signature." },
  { "en": "Apa itu 'metamorphic malware'?", "id": "Malware menulis ulang dirinya sendiri." },
  { "en": "Apa itu 'endpoint security'?", "id": "Mengamankan perangkat akhir seperti laptop." },
  { "en": "Contoh solusi 'endpoint security'?", "id": "Antivirus, HIPS, EDR." },
  { "en": "Kepanjangan EDR?", "id": "Endpoint Detection and Response." },
  { "en": "Fungsi EDR?", "id": "Deteksi dan respon ancaman canggih." },
  { "en": "Apa itu 'User Behavior Analytics'?", "id": "Analisa perilaku pengguna deteksi anomali." },
  { "en": "Kepanjangan UBA/UEBA?", "id": "User and Entity Behavior Analytics." },
  { "en": "Apa itu 'security orchestration'?", "id": "Menghubungkan alat keamanan berbeda." },
  { "en": "Kepanjangan SOAR?", "id": "Security Orchestration, Automation, and Response." },
  { "en": "Tujuan SOAR?", "id": "Otomatisasi respon insiden keamanan." },
  { "en": "Apa itu 'threat feed'?", "id": "Sumber data intelijen ancaman." },
  { "en": "Apa itu 'black-box testing'?", "id": "Pengujian tanpa pengetahuan internal sistem." },
  { "en": "Apa itu 'white-box testing'?", "id": "Pengujian dengan pengetahuan penuh sistem." },
  { "en": "Apa itu 'gray-box testing'?", "id": "Pengujian dengan pengetahuan sebagian." },
  { "en": "Apa itu 'static application security testing'?", "id": "Analisa kode sumber cari kerentanan." },
  { "en": "Kepanjangan SAST?", "id": "Static Application Security Testing." },
  { "en": "Apa itu 'dynamic application security testing'?", "id": "Pengujian aplikasi saat berjalan." },
  { "en": "Kepanjangan DAST?", "id": "Dynamic Application Security Testing." },
  { "en": "Apa itu 'interactive application security testing'?", "id": "Kombinasi SAST dan DAST." },
  { "en": "Kepanjangan IAST?", "id": "Interactive Application Security Testing." },
  { "en": "Apa itu 'secure code review'?", "id": "Tinjauan manual kode cari kelemahan." },
  { "en": "Apa itu 'mean time to detect'?", "id": "Rata-rata waktu untuk deteksi ancaman." },
  { "en": "Kepanjangan MTTD?", "id": "Mean Time To Detect." },
  { "en": "Apa itu 'mean time to respond'?", "id": "Rata-rata waktu untuk merespon insiden." },
  { "en": "Kepanjangan MTTR?", "id": "Mean Time To Respond." },
  { "en": "Pentingnya menurunkan MTTD/MTTR?", "id": "Meminimalkan dampak kerusakan akibat serangan." },
  { "en": "Apa itu 'data sovereignty'?", "id": "Data tunduk pada hukum negara lokasi." },
  { "en": "Apa itu 'data residency'?", "id": "Lokasi geografis tempat data disimpan." },
  { "en": "Tantangan 'data sovereignty' cloud?", "id": "Data bisa disimpan di negara lain." },
  { "en": "Apa itu 'secure boot'?", "id": "Memastikan hanya OS terpercaya yang berjalan." },
  { "en": "Apa itu 'trusted platform module'?", "id": "Chip perangkat keras untuk keamanan." },
  { "en": "Kepanjangan TPM?", "id": "Trusted Platform Module." },
  { "en": "Fungsi utama TPM?", "id": "Menyimpan kunci enkripsi secara aman." },
  { "en": "Apa itu 'air-gapped network'?", "id": "Jaringan yang terisolasi secara fisik." },
  { "en": "Risiko 'air-gapped network'?", "id": "Bisa ditembus via media fisik (USB)." },
  { "en": "Apa itu 'bastion host'?", "id": "Server khusus sebagai gerbang akses." },
  { "en": "Sinonim 'bastion host'?", "id": "Jump server atau jump host." },
  { "en": "Apa itu 'network access control'?", "id": "Mengontrol akses perangkat ke jaringan." },
  { "en": "Kepanjangan NAC?", "id": "Network Access Control." },
  { "en": "Cara kerja NAC?", "id": "Memeriksa postur keamanan sebelum akses." },
  { "en": "Apa itu 'posture assessment'?", "id": "Pengecekan status keamanan perangkat." },
  { "en": "Contoh 'posture assessment'?", "id": "Cek antivirus, patch, firewall aktif." },
  { "en": "Apa itu 'quarantine network'?", "id": "Jaringan isolasi untuk perangkat tak patuh." },
  { "en": "Apa itu 'deception technology'?", "id": "Teknologi untuk menipu dan mendeteksi penyerang." },
  { "en": "Contoh 'deception technology'?", "id": "Honeypot, honeytoken, honey-credentials." },
  { "en": "Apa itu 'honeytoken'?", "id": "Aset palsu untuk memancing penyerang." },
  { "en": "Apa itu 'canary trap'?", "id": "Umpan untuk deteksi kebocoran data." },
  { "en": "Apa itu 'privacy by design'?", "id": "Memasukkan privasi sejak awal pengembangan." },
  { "en": "Prinsip 'privacy by design'?", "id": "Proaktif, default privasi, transparan." },
  { "en": "Apa itu 'data minimization'?", "id": "Mengumpulkan data sesedikit mungkin." },
  { "en": "Apa itu 'purpose limitation'?", "id": "Data hanya dipakai sesuai tujuan." },
  { "en": "Apa itu 'anonymization'?", "id": "Menghapus informasi identitas pribadi." },
  { "en": "Apa itu 'pseudonymization'?", "id": "Mengganti identitas dengan nama samaran." },
  { "en": "Beda 'anonymization' & 'pseudonymization'?", "id": "Pseudonymization masih bisa dikembalikan." },
  { "en": "Apa itu 'right to be forgotten'?", "id": "Hak subjek data meminta penghapusan." },
  { "en": "Tantangan 'right to be forgotten'?", "id": "Menghapus data dari semua backup." },
  { "en": "Apa itu 'data protection officer'?", "id": "Pejabat pengawas perlindungan data." },
  { "en": "Kepanjangan DPO?", "id": "Data Protection Officer." },
  { "en": "Tujuan utama audit log?", "id": "Menelusuri jejak aktivitas dan insiden." },
  { "en": "Prinsip dasar 'secure coding'?", "id": "Menulis kode yang tahan serangan." },
  { "en": "Contoh praktik 'secure coding'?", "id": "Validasi input, manajemen error aman." },
  { "en": "Apa itu 'security misconfiguration'?", "id": "Kesalahan pengaturan yang ciptakan celah." },
  { "en": "Contoh 'security misconfiguration'?", "id": "Password default, service tidak perlu." },
  { "en": "Apa itu 'asset management' IT?", "id": "Inventarisasi dan pengelolaan semua aset IT." },
  { "en": "Pentingnya 'asset management'?", "id": "Mengetahui apa yang harus dilindungi." },
  { "en": "Apa itu 'threat landscape'?", "id": "Gambaran keseluruhan ancaman siber saat ini." },
  { "en": "Kaitan 'threat landscape' & strategi?", "id": "Strategi keamanan disesuaikan dengan ancaman." },
  { "en": "Apa itu 'configuration management'?", "id": "Menjaga konsistensi konfigurasi sistem." },
  { "en": "Apa itu 'secure baseline'?", "id": "Konfigurasi standar keamanan untuk sistem." },
  { "en": "Hubungan 'baseline' & 'hardening'?", "id": "Hardening adalah proses terapkan baseline." },
  { "en": "Apa itu 'outdated software risk'?", "id": "Risiko dari software tidak di-patch." },
  { "en": "Apa itu 'end-of-life' software?", "id": "Software tidak lagi didukung vendor." },
  { "en": "Risiko 'end-of-life' software?", "id": "Tidak ada patch keamanan baru." },
  { "en": "Apa itu 'vulnerability assessment'?", "id": "Proses identifikasi celah keamanan." },
  { "en": "Beda 'VA' & 'penetration testing'?", "id": "VA mengidentifikasi, pentest mengeksploitasi." },
  { "en": "Apa itu 'false positive'?", "id": "Peringatan palsu, aktivitas normal dideteksi." },
  { "en": "Apa itu 'false negative'?", "id": "Ancaman nyata gagal terdeteksi." },
  { "en": "Mana lebih berbahaya?", "id": "False negative." },
  { "en": "Apa itu 'event'?", "id": "Kejadian yang terpantau pada sistem." },
  { "en": "Apa itu 'alert'?", "id": "Notifikasi dari event yang mencurigakan." },
  { "en": "Apa itu 'incident'?", "id": "Event yang terbukti insiden keamanan." },
  { "en": "Tujuan 'incident classification'?", "id": "Memberi prioritas penanganan insiden." },
  { "en": "Tujuan 'incident containment'?", "id": "Membatasi dan menghentikan penyebaran kerusakan." },
  { "en": "Fase setelah 'containment'?", "id": "Eradication (Pemberantasan)." },
  { "en": "Tujuan 'eradication'?", "id": "Menghilangkan akar penyebab insiden." },
  { "en": "Fase setelah 'eradication'?", "id": "Recovery (Pemulihan)." },
  { "en": "Fase terakhir siklus insiden?", "id": "Post-Incident Activity (Lessons Learned)." },
  { "en": "Apa itu 'cyber range'?", "id": "Lingkungan virtual untuk simulasi siber." },
  { "en": "Manfaat 'cyber range'?", "id": "Melatih tim respons insiden realistis." },
  { "en": "Apa itu 'threat actor profiling'?", "id": "Mempelajari karakteristik dan motivasi penyerang." },
  { "en": "Apa itu 'TTPs'?", "id": "Tactics, Techniques, and Procedures." },
  { "en": "Fungsi analisis TTP?", "id": "Memahami cara kerja kelompok peretas." },
  { "en": "Apa itu 'cyber diplomacy'?", "id": "Upaya diplomasi terkait isu siber." },
  { "en": "Apa itu 'trusted execution environment'?", "id": "Area aman di dalam prosesor." },
  { "en": "Kepanjangan TEE?", "id": "Trusted Execution Environment." },
  { "en": "Fungsi TEE?", "id": "Melindungi data bahkan dari OS." },
  { "en": "Apa itu 'homomorphic encryption'?", "id": "Enkripsi yang bisa diolah datanya." },
  { "en": "Manfaat 'homomorphic encryption'?", "id": "Analisa data sensitif tanpa dekripsi." },
  { "en": "Apa itu 'secure multi-party computation'?", "id": "Komputasi bersama tanpa berbagi data." },
  { "en": "Apa itu 'zero-knowledge proof'?", "id": "Membuktikan sesuatu tanpa ungkap datanya." },
  { "en": "Kepanjangan ZKP?", "id": "Zero-Knowledge Proof." },
  { "en": "Contoh penggunaan ZKP?", "id": "Verifikasi usia tanpa tunjukkan tanggal lahir." },
  { "en": "Apa itu 'differential privacy'?", "id": "Teknik melindungi privasi dalam dataset." },
  { "en": "Tujuan 'differential privacy'?", "id": "Analisa data agregat tanpa identifikasi individu." },
  { "en": "Apa itu 'canary token'?", "id": "Umpan digital untuk deteksi penyusup." },
  { "en": "Contoh 'canary token'?", "id": "File palsu, link palsu, akun palsu." },
  { "en": "Apa itu 'data diode'?", "id": "Perangkat yang izinkan data satu arah." },
  { "en": "Fungsi 'data diode'?", "id": "Mencegah data bocor dari jaringan aman." },
  { "en": "Apa itu 'compliance audit'?", "id": "Audit untuk memastikan kepatuhan regulasi." },
  { "en": "Apa itu 'security control'?", "id": "Tindakan untuk mengurangi risiko keamanan." },
  { "en": "Tipe 'security control'?", "id": "Preventive, Detective, Corrective." },
  { "en": "Contoh 'preventive control'?", "id": "Firewall, password, enkripsi." },
  { "en": "Contoh 'detective control'?", "id": "IDS, audit logs, CCTV." },
  { "en": "Contoh 'corrective control'?", "id": "Backup, disaster recovery plan." },
  { "en": "Apa itu ' compensating control'?", "id": "Kontrol alternatif jika kontrol utama gagal." },
  { "en": "Apa itu 'physical control'?", "id": "Kontrol keamanan fisik (kunci, pagar)." },
  { "en": "Apa itu 'administrative control'?", "id": "Kontrol berbasis kebijakan dan prosedur." },
  { "en": "Contoh 'administrative control'?", "id": "Security awareness training, AUP." },
  { "en": "Apa itu 'logical control'?", "id": "Kontrol teknis berbasis software." },
  { "en": "Contoh 'logical control'?", "id": "Access Control List, password." },
  { "en": "Pentingnya keamanan rantai pasok?", "id": "Mencegah ancaman dari vendor/pihak ketiga." },
  { "en": "Apa itu 'third-party risk management'?", "id": "Manajemen risiko dari pihak ketiga." },
  { "en": "Apa itu 'service account'?", "id": "Akun non-manusia untuk aplikasi." },
  { "en": "Keamanan 'service account'?", "id": "Gunakan password kuat, hak akses minimal." },
  { "en": "Apa itu 'password complexity'?", "id": "Syarat kekuatan sebuah password." },
  { "en": "Contoh 'password complexity'?", "id": "Panjang, huruf besar-kecil, angka, simbol." },
  { "en": "Apa itu 'password history'?", "id": "Melarang penggunaan password lama." },
  { "en": "Apa itu 'password expiration'?", "id": "Masa berlaku sebuah password." },
  { "en": "Apa itu 'account lockout policy'?", "id": "Kebijakan kunci akun setelah gagal login." },
  { "en": "Tujuan 'account lockout'?", "id": "Mencegah serangan brute-force." },
  { "en": "Apa itu 'CAPTCHA'?", "id": "Tes untuk bedakan manusia-bot." },
  { "en": "Kepanjangan CAPTCHA?", "id": "Completely Automated Public Turing test." },
  { "en": "Apa itu 'memory leak'?", "id": "Program gagal melepas memori." },
  { "en": "Dampak 'memory leak'?", "id": "Menurunkan performa, bisa sebabkan crash." },
  { "en": "Apa itu 'race condition'?", "id": "Bug akibat urutan event tak terduga." },
  { "en": "Apa itu 'dead code'?", "id": "Kode yang ada tapi tak pernah dieksekusi." },
  { "en": "Risiko 'dead code'?", "id": "Bisa mengandung celah keamanan tersembunyi." },
  { "en": "Apa itu 'code signing'?", "id": "Memberi tanda tangan digital pada software." },
  { "en": "Tujuan 'code signing'?", "id": "Memastikan integritas dan asal software." },
  { "en": "Apa itu 'software composition analysis'?", "id": "Analisa komponen pihak ketiga software." },
  { "en": "Kepanjangan SCA?", "id": "Software Composition Analysis." },
  { "en": "Tujuan SCA?", "id": "Mencari kerentanan di library eksternal." },
  { "en": "Apa itu 'bill of materials' software?", "id": "Daftar semua komponen dalam software." },
  { "en": "Kepanjangan SBOM?", "id": "Software Bill of Materials." },
  { "en": "Pentingnya SBOM?", "id": "Transparansi dan manajemen kerentanan." },
  { "en": "Kaitan 'secure boot' dan 'chain of trust'?", "id": "Membangun rantai kepercayaan dari hardware." },
  { "en": "Fungsi 'audit trail'?", "id": "Jejak audit kronologis sebuah aktivitas." },
  { "en": "Prinsip 'non-repudiation'?", "id": "Mencegah penyangkalan sebuah aksi." },
  { "en": "Contoh 'non-repudiation'?", "id": "Tanda tangan digital pada transaksi." }


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
