<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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

  { "en": "Ikonografi?", "id": "Ilmu arca." },
  { "en": "Ikonologi?", "id": "Ilmu makna simbol." },
  { "en": "Ikosahedron?", "id": "Bidang duapuluh." },
  { "en": "Ikram?", "id": "Penghormatan (Arab)." },
  { "en": "Ikrar?", "id": "Janji." },
  { "en": "Ikut?", "id": "Serta." },
  { "en": "Ilah?", "id": "Tuhan." },
  { "en": "Ilahiah?", "id": "Ketuhanan." },
  { "en": "Ilai?", "id": "Tidak (arkais)." },
  { "en": "Ilang?", "id": "Hilang (ejaan lama)." },
  { "en": "Ilegal?", "id": "Tidak sah." },
  { "en": "Ilegalitas?", "id": "Ketidaksahan." },
  { "en": "Ilham?", "id": "Inspirasi." },
  { "en": "Ili?", "id": "Penyakit (arkais)." },
  { "en": "Ilmiah?", "id": "Bersifat ilmu." },
  { "en": "Ilmu?", "id": "Pengetahuan." },
  { "en": "Ilmuwan?", "id": "Ahli ilmu." },
  { "en": "Iluminasi?", "id": "Penerangan." },
  { "en": "Ilusi?", "id": "Khayalan." },
  { "en": "Ilusionis?", "id": "Pemain sulap." },
  { "en": "Ilustrasi?", "id": "Gambaran." },
  { "en": "Ilustratif?", "id": "Menggambarkan." },
  { "en": "Ilustrator?", "id": "Penggambar." },
  { "en": "Imago?", "id": "Serangga dewasa." },
  { "en": "Imah?", "id": "Rumah (Sunda)." },
  { "en": "Imak?", "id": "Tiru (arkais)." },
  { "en": "Imam?", "id": "Pemimpin salat." },
  { "en": "Imamah?", "id": "Kepemimpinan (Islam)." },
  { "en": "Iman?", "id": "Kepercayaan." },
  { "en": "Imang?", "id": "Goyang (arkais)." },
  { "en": "Imaterial?", "id": "Tidak berwujud." },
  { "en": "Imamat?", "id": "Jabatan imam." },
  { "en": "Imbau?", "id": "Seru." },
  { "en": "Imbuh?", "id": "Tambah." },
  { "en": "Imbuhan?", "id": "Afiks." },
  { "en": "Imbun?", "id": "Embun (arkais)." },
  { "en": "Imigran?", "id": "Pendatang." },
  { "en": "Imigrasi?", "id": "Perpindahan masuk." },
  { "en": "Iming-iming?", "id": "Bujukan." },
  { "en": "Imitasi?", "id": "Tiruan." },
  { "en": "Imitatif?", "id": "Bersifat tiruan." },
  { "en": "Imitator?", "id": "Peniru." },
  { "en": "Imla?", "id": "Dikte." },
  { "en": "Imlek?", "id": "Tahun baru Tionghoa." },
  { "en": "Imobil?", "id": "Tidak bergerak." },
  { "en": "Imobilisasi?", "id": "Pelumpuhan gerak." },
  { "en": "Imoral?", "id": "Tidak bermoral." },
  { "en": "Imoralitas?", "id": "Ketidakbermoralan." },
  { "en": "Imortal?", "id": "Abadi." },
  { "en": "Imortalitas?", "id": "Keabadian." },
  { "en": "Impak?", "id": "Benturan." },
  { "en": "Impala?", "id": "Antelop Afrika." },
  { "en": "Impas?", "id": "Sama." },
  { "en": "Impeachment?", "id": "Pemakzulan." },
  { "en": "Impedans?", "id": "Hambatan listrik." },
  { "en": "Imperatif?", "id": "Perintah." },
  { "en": "Imperfek?", "id": "Tidak sempurna." },
  { "en": "Imperfeksi?", "id": "Ketidaksempurnaan." },
  { "en": "Imperial?", "id": "Kerajaan." },
  { "en": "Imperialis?", "id": "Penganut imperialisme." },
  { "en": "Imperialisme?", "id": "Paham penjajahan." },
  { "en": "Imperium?", "id": "Kekaisaran." },
  { "en": "Impersonal?", "id": "Tidak pribadi." },
  { "en": "Impersonalia?", "id": "Bentuk impersonal." },
  { "en": "Implikasi?", "id": "Keterlibatan." },
  { "en": "Implisit?", "id": "Tersirat." },
  { "en": "Implosif?", "id": "Bunyi letup ke dalam." },
  { "en": "Impor?", "id": "Pemasukan barang." },
  { "en": "Importan?", "id": "Penting." },
  { "en": "Importir?", "id": "Pengimpor." },
  { "en": "Impoten?", "id": "Tidak berdaya (seksual)." },
  { "en": "Impotensi?", "id": "Keadaan impoten." },
  { "en": "Impresariat?", "id": "Agen artis." },
  { "en": "Impresario?", "id": "Manajer artis." },
  { "en": "Impresi?", "id": "Kesan." },
  { "en": "Impresif?", "id": "Mengesankan." },
  { "en": "Impresionisme?", "id": "Aliran seni." },
  { "en": "Imprimatur?", "id": "Izin cetak." },
  { "en": "Improvisasi?", "id": "Tanpa persiapan." },
  { "en": "Impuls?", "id": "Dorongan tiba-tiba." },
  { "en": "Impulsif?", "id": "Bertindak spontan." },
  { "en": "Imsak?", "id": "Batas sahur." },
  { "en": "Imsakiah?", "id": "Jadwal imsak." },
  { "en": "Imun?", "id": "Kebal." },
  { "en": "Imunisasi?", "id": "Pengebalan." },
  { "en": "Imunitas?", "id": "Kekebalan." },
  { "en": "Imunokimia?", "id": "Kimia kekebalan." },
  { "en": "Imunologi?", "id": "Ilmu kekebalan." },
  { "en": "Imunosupresi?", "id": "Penekanan kekebalan." },
  { "en": "Imunoterapi?", "id": "Terapi kekebalan." },
  { "en": "In absensia?", "id": "Tanpa kehadiran." },
  { "en": "In vitro?", "id": "Dalam tabung (Latin)." },
  { "en": "In vivo?", "id": "Dalam tubuh hidup (Latin)." },
  { "en": "Inai?", "id": "Pacar kuku." },
  { "en": "Inang?", "id": "Tuan rumah (biologi)." },
  { "en": "Inartikulat?", "id": "Tidak jelas." },
  { "en": "Inaugurasi?", "id": "Pelantikan." },
  { "en": "Inayat?", "id": "Pertolongan (Arab)." },
  { "en": "Inca?", "id": "Suku Indian." },
  { "en": "Incar?", "id": "Bidik." },
  { "en": "Inces?", "id": "Hubungan sedarah." },
  { "en": "Inci?", "id": "Satuan panjang." },
  { "en": "Incognito?", "id": "Menyamar." },
  { "en": "Indah?", "id": "Elok." },
  { "en": "Indarus?", "id": "Lembaga (arkais)." },
  { "en": "Indayang?", "id": "Ibu (Minang)." },
  { "en": "Indebitum?", "id": "Pembayaran tak wajib." },
  { "en": "Indehoy?", "id": "Bercinta (slang)." },
  { "en": "Indekos?", "id": "Kos." },
  { "en": "Indeks?", "id": "Penunjuk." },
  { "en": "Inden?", "id": "Pesanan." },
  { "en": "Indentasi?", "id": "Lekukan awal baris." },
  { "en": "Independen?", "id": "Bebas." },
  { "en": "Indera?", "id": "Panca indra." },
  { "en": "Inderalaya?", "id": "Kahyangan (arkais)." },
  { "en": "Inderawasih?", "id": "Cenderawasih (arkais)." },
  { "en": "Indeterminisme?", "id": "Paham serba tak tentu." },
  { "en": "Indi?", "id": "Mandiri (musik/film)." },
  { "en": "India?", "id": "Negara." },
  { "en": "Indicator?", "id": "Indikator (Inggris)." },
  { "en": "Indigen?", "id": "Pribumi." },
  { "en": "Indigenisasi?", "id": "Pempribumian." },
  { "en": "Indigo?", "id": "Warna nila." },
  { "en": "Indik?", "id": "Ukur (arkais)." },
  { "en": "Indikasi?", "id": "Petunjuk." },
  { "en": "Indikatif?", "id": "Menunjukkan." },
  { "en": "Indikator?", "id": "Penanda." },
  { "en": "Inding?", "id": "Perisai (arkais)." },
  { "en": "Indisipliner?", "id": "Tidak disiplin." },
  { "en": "Individu?", "id": "Perorangan." },
  { "en": "Individual?", "id": "Bersifat perorangan." },
  { "en": "Individualis?", "id": "Mementingkan diri." },
  { "en": "Individualisme?", "id": "Paham individualis." },
  { "en": "Individualitas?", "id": "Keunikan pribadi." },
  { "en": "Indoktrinasi?", "id": "Penanaman ajaran." },
  { "en": "Indolen?", "id": "Lamban." },
  { "en": "Indolensi?", "id": "Kelambanan." },
  { "en": "Indonesia?", "id": "Negara." },
  { "en": "Indra?", "id": "Dewa (Hindu)." },
  { "en": "Indrajala?", "id": "Sulap." },
  { "en": "Indraloka?", "id": "Kahyangan Dewa Indra." },
  { "en": "Indranila?", "id": "Batu permata biru." },
  { "en": "Indria?", "id": "Indra." },
  { "en": "Indriawi?", "id": "Bersifat indra." },
  { "en": "Induk?", "id": "Ibu (hewan)." },
  { "en": "Induksi?", "id": "Imbasan." },
  { "en": "Induktans?", "id": "Sifat rangkaian listrik." },
  { "en": "Induktif?", "id": "Dari khusus ke umum." },
  { "en": "Induktor?", "id": "Komponen listrik." },
  { "en": "Indulen?", "id": "Lamban." },
  { "en": "Indulgensia?", "id": "Pengampunan dosa (Katolik)." },
  { "en": "Indung?", "id": "Ibu." },
  { "en": "Industri?", "id": "Perusahaan." },
  { "en": "Industrialis?", "id": "Pengusaha industri." },
  { "en": "Industrialisasi?", "id": "Proses menjadi industri." },
  { "en": "Inefisiensi?", "id": "Ketidakefisienan." },
  { "en": "Inersia?", "id": "Kelembaman." },
  { "en": "Infak?", "id": "Sumbangan." },
  { "en": "Infanteri?", "id": "Pasukan jalan kaki." },
  { "en": "Infark?", "id": "Kematian jaringan." },
  { "en": "Infeksi?", "id": "Masuknya kuman." },
  { "en": "Infeksius?", "id": "Menular." },
  { "en": "Inferensi?", "id": "Simpulan." },
  { "en": "Inferensial?", "id": "Bersifat simpulan." },
  { "en": "Inferior?", "id": "Rendah." },
  { "en": "Inferioritas?", "id": "Kerendahan." },
  { "en": "Infertilitas?", "id": "Kemandulan." },
  { "en": "Infiks?", "id": "Sisipan." },
  { "en": "Infiltrasi?", "id": "Penyusupan." },
  { "en": "Infiltrometer?", "id": "Pengukur resapan." },
  { "en": "Infinitif?", "id": "Bentuk dasar kata kerja." },
  { "en": "Inflamasi?", "id": "Peradangan." },
  { "en": "Inflasi?", "id": "Kenaikan harga." },
  { "en": "Inflatoir?", "id": "Menyebabkan inflasi." },
  { "en": "Infleksi?", "id": "Perubahan bentuk kata." },
  { "en": "Infloresensi?", "id": "Susunan bunga." },
  { "en": "Influensa?", "id": "Flu." },
  { "en": "Influenza?", "id": "Penyakit." },
  { "en": "Info?", "id": "Informasi (singkatan)." },
  { "en": "Informan?", "id": "Pemberi informasi." },
  { "en": "Informasi?", "id": "Keterangan." },
  { "en": "Informatif?", "id": "Memberi keterangan." },
  { "en": "Informatika?", "id": "Ilmu informasi." },
  { "en": "Infoteknologi?", "id": "Teknologi informasi." },
  { "en": "Infra?", "id": "Bawah (awalan)." },
  { "en": "Inframerah?", "id": "Sinar." },
  { "en": "Infrastruktur?", "id": "Prasarana." },
  { "en": "Infus?", "id": "Pemasukan cairan." },
  { "en": "Ingat?", "id": "Kenang." },
  { "en": "Ingatan?", "id": "Memori." },
  { "en": "Ingau?", "id": "Igau." },
  { "en": "Inggat?", "id": "Ingat (arkais)." },
  { "en": "Inggris?", "id": "Bahasa/Negara." },
  { "en": "Ingsor?", "id": "Turun (arkais)." },
  { "en": "Ingu?", "id": "Bau tak sedap." },
  { "en": "Ingus?", "id": "Lendir hidung." },
  { "en": "Inhal?", "id": "Sedot." },
  { "en": "Inhalan?", "id": "Zat hirup." },
  { "en": "Inhalasi?", "id": "Penghirupan." },
  { "en": "Inheren?", "id": "Melekat." },
  { "en": "Inhibisi?", "id": "Penghambatan." },
  { "en": "Inhibitor?", "id": "Penghambat." },
  { "en": "Ini?", "id": "Dekat." },
  { "en": "Inisial?", "id": "Huruf awal." },
  { "en": "Inisiasi?", "id": "Permulaan." },
  { "en": "Inisiatif?", "id": "Prakarsa." },
  { "en": "Injak?", "id": "Pijak." },
  { "en": "Injeksi?", "id": "Suntikan." },
  { "en": "Injil?", "id": "Kitab suci Kristen." },
  { "en": "Inkarnasi?", "id": "Penjelmaan." },
  { "en": "Inkarserasi?", "id": "Pemenjaraan." },
  { "en": "Inkas?", "id": "Lubang (arkais)." },
  { "en": "Inkaso?", "id": "Penagihan." },
  { "en": "Inklinasi?", "id": "Kecenderungan." },
  { "en": "Inklusif?", "id": "Termasuk." },
  { "en": "Inkognito?", "id": "Menyamar." },
  { "en": "Inkohabitasi?", "id": "Hidup bersama tanpa nikah." },
  { "en": "Inkoheren?", "id": "Tidak runtut." },
  { "en": "Inkompatibel?", "id": "Tidak cocok." },
  { "en": "Inkompeten?", "id": "Tidak cakap." },
  { "en": "Inkonfeso?", "id": "Tidak diakui." },
  { "en": "Inkonsisten?", "id": "Tidak taat asas." },
  { "en": "Inkonstitusional?", "id": "Bertentangan UUD." },
  { "en": "Inkonvensional?", "id": "Tidak lazim." },
  { "en": "Inkorporasi?", "id": "Penggabungan." },
  { "en": "Inkremen?", "id": "Tambahan." },
  { "en": "Inkriminasi?", "id": "Pendakwaan." },
  { "en": "Inkubasi?", "id": "Masa pengeraman." },
  { "en": "Inkubator?", "id": "Alat pengeram." },
  { "en": "Inlander?", "id": "Pribumi (Belanda, hinaan)." },
  { "en": "Innalillahi?", "id": "Ucapan duka cita." },
  { "en": "Inning?", "id": "Babak (kriket)." },
  { "en": "Inokulasi?", "id": "Penyuntikan bibit." },
  { "en": "Inovasi?", "id": "Pembaruan." },
  { "en": "Inovatif?", "id": "Bersifat baru." },
  { "en": "Inovator?", "id": "Pembaruan." },
  { "en": "Input?", "id": "Masukan." },
  { "en": "Insaf?", "id": "Sadar." },
  { "en": "Insan?", "id": "Manusia." },
  { "en": "Insanan?", "id": "Boneka (arkais)." },
  { "en": "Insang?", "id": "Alat napas ikan." },
  { "en": "Insani?", "id": "Manusiawi." },
  { "en": "Insanulkamil?", "id": "Manusia sempurna." },
  { "en": "Insek?", "id": "Serangga." },
  { "en": "Insektari?", "id": "Tempat pelihara serangga." },
  { "en": "Insektisida?", "id": "Pembasmi serangga." },
  { "en": "Insektivor?", "id": "Pemakan serangga." },
  { "en": "Insekuri?", "id": "Tidak aman." },
  { "en": "Inseminasi?", "id": "Pembuahan buatan." },
  { "en": "Insentif?", "id": "Perangsang." },
  { "en": "Inses?", "id": "Hubungan sedarah." },
  { "en": "Inset?", "id": "Peta sisipan." },
  { "en": "Insiden?", "id": "Peristiwa." },
  { "en": "Insidental?", "id": "Kadang-kadang." },
  { "en": "Insidi?", "id": "Tipu muslihat (arkais)." },
  { "en": "Insignia?", "id": "Lencana." },
  { "en": "Insinerator?", "id": "Tungku pembakar." },
  { "en": "Insinuasi?", "id": "Sindiran." },
  { "en": "Insinyur?", "id": "Sarjana teknik." },
  { "en": "Insisif?", "id": "Tajam." },
  { "en": "Insisor?", "id": "Gigi seri." },
  { "en": "Insitu?", "id": "Di tempat asli." },
  { "en": "Insolven?", "id": "Bangkrut." },
  { "en": "Insomnia?", "id": "Sulit tidur." },
  { "en": "Inspeksi?", "id": "Pemeriksaan." },
  { "en": "Inspektorat?", "id": "Kantor inspeksi." },
  { "en": "Inspektur?", "id": "Pemeriksa." },
  { "en": "Inspirasi?", "id": "Ilham." },
  { "en": "Inspiratif?", "id": "Memberi ilham." },
  { "en": "Instabilitas?", "id": "Ketidakstabilan." },
  { "en": "Instalasi?", "id": "Pemasangan." },
  { "en": "Instan?", "id": "Segera." },
  { "en": "Instansi?", "id": "Badan pemerintah." },
  { "en": "Insting?", "id": "Naluri." },
  { "en": "Instingtif?", "id": "Berdasarkan naluri." },
  { "en": "Institut?", "id": "Lembaga." },
  { "en": "Institusi?", "id": "Pranata." },
  { "en": "Institusional?", "id": "Bersifat lembaga." },
  { "en": "Instruksi?", "id": "Perintah." },
  { "en": "Instruktif?", "id": "Bersifat perintah." },
  { "en": "Instruktur?", "id": "Pelatih." },
  { "en": "Instrumen?", "id": "Alat." },
  { "en": "Instrumental?", "id": "Berperan." },
  { "en": "Instrumentasi?", "id": "Penggunaan instrumen." },
  { "en": "Insubordinasi?", "id": "Pembangkangan." },
  { "en": "Insulator?", "id": "Penyekat." },
  { "en": "Insuler?", "id": "Terpencil." },
  { "en": "Insulin?", "id": "Hormon." },
  { "en": "Insulinde?", "id": "Nusantara." },
  { "en": "Insya Allah?", "id": "Jika Allah menghendaki." },
  { "en": "Intai?", "id": "Awasi." },
  { "en": "Intan?", "id": "Berlian." },
  { "en": "Integritas?", "id": "Kejujuran." },
  { "en": "Intel?", "id": "Mata-mata." },
  { "en": "Intelek?", "id": "Akal budi." },
  { "en": "Intelektual?", "id": "Cendekiawan." },
  { "en": "Inteligen?", "id": "Cerdas." },
  { "en": "Inteligensia?", "id": "Kaum cendekiawan." },
  { "en": "Intelijen?", "id": "Dinas rahasia." },
  { "en": "Inten?", "id": "Intan (arkais)." },
  { "en": "Intens?", "id": "Hebat." },
  { "en": "Intensif?", "id": "Sungguh-sungguh." },
  { "en": "Intensifikasi?", "id": "Peningkatan." },
  { "en": "Intensi?", "id": "Maksud." },
  { "en": "Intensional?", "id": "Sengaja." },
  { "en": "Inter?", "id": "Antar (awalan)." },
  { "en": "Interaksi?", "id": "Saling tindak." },
  { "en": "Interaktif?", "id": "Saling berpengaruh." },
  { "en": "Interdepartemental?", "id": "Antardepartemen." },
  { "en": "Interdependen?", "id": "Saling bergantung." },
  { "en": "Interdiksi?", "id": "Larangan." },
  { "en": "Interdisipliner?", "id": "Antardisiplin." },
  { "en": "Interegnum?", "id": "Masa peralihan kekuasaan." },
  { "en": "Interes?", "id": "Minat." },
  { "en": "Interesan?", "id": "Menarik." },
  { "en": "Interferens?", "id": "Gangguan." },
  { "en": "Interferensi?", "id": "Campur tangan." },
  { "en": "Interferometer?", "id": "Alat ukur." },
  { "en": "Interferon?", "id": "Protein." },
  { "en": "Interglasial?", "id": "Antar zaman es." },
  { "en": "Interim?", "id": "Sementara." },
  { "en": "Interior?", "id": "Bagian dalam." },
  { "en": "Interjeksi?", "id": "Kata seru." },
  { "en": "Interkom?", "id": "Alat komunikasi." },
  { "en": "Interkoneksi?", "id": "Saling hubung." },
  { "en": "Interkonsonantal?", "id": "Antar konsonan." },
  { "en": "Interkultural?", "id": "Antarbudaya." },
  { "en": "Interlokal?", "id": "Antar kota." },
  { "en": "Interlud?", "id": "Selipan musik." },
  { "en": "Intermediasi?", "id": "Perantaraan." },
  { "en": "Intermediat?", "id": "Peralihan." },
  { "en": "Intermeso?", "id": "Selingan." },
  { "en": "Intern?", "id": "Dalam." },
  { "en": "Internal?", "id": "Bagian dalam." },
  { "en": "Internasional?", "id": "Antarbangsa." },
  { "en": "Internat?", "id": "Asrama." },
  { "en": "Internir?", "id": "Tahan." },
  { "en": "Interniran?", "id": "Tawanan." },
  { "en": "Internis?", "id": "Ahli penyakit dalam." },
  { "en": "Internuntius?", "id": "Utusan Paus." },
  { "en": "Interogasi?", "id": "Pemeriksaan." },
  { "en": "Interogatif?", "id": "Bertanya." },
  { "en": "Interogator?", "id": "Pemeriksa." },
  { "en": "Interpol?", "id": "Polisi internasional." },
  { "en": "Interpolasi?", "id": "Penyisipan." },
  { "en": "Interpretasi?", "id": "Tafsiran." },
  { "en": "Interpreter?", "id": "Penafsir." },
  { "en": "Intersepsi?", "id": "Penyadapan." },
  { "en": "Intertidal?", "id": "Daerah pasang surut." },
  { "en": "Interupsi?", "id": "Selaan." },
  { "en": "Interval?", "id": "Jarak." },
  { "en": "Intervensi?", "id": "Campur tangan." },
  { "en": "Interventrikular?", "id": "Antar bilik jantung." },
  { "en": "Interviu?", "id": "Wawancara." },
  { "en": "Interzona?", "id": "Antar zona." },
  { "en": "Inti?", "id": "Pokok." },
  { "en": "Intifadah?", "id": "Pemberontakan (Palestina)." },
  { "en": "Intikad?", "id": "Kritik (arkais)." },
  { "en": "Intima?", "id": "Lapisan dalam pembuluh." },
  { "en": "Intim?", "id": "Akrab." },
  { "en": "Intimidasi?", "id": "Ancaman." },
  { "en": "Intip?", "id": "Lihat diam-diam." },
  { "en": "Intisari?", "id": "Pati." },
  { "en": "Intoleran?", "id": "Tidak tenggang rasa." },
  { "en": "Intonasi?", "id": "Lagu kalimat." },
  { "en": "Intoksikasi?", "id": "Keracunan." },
  { "en": "Intra?", "id": "Dalam (awalan)." },
  { "en": "Intradermal?", "id": "Dalam kulit." },
  { "en": "Intrakalimat?", "id": "Dalam kalimat." },
  { "en": "Intramembran?", "id": "Dalam selaput." },
  { "en": "Intramolekuler?", "id": "Dalam molekul." },
  { "en": "Intramuskuler?", "id": "Dalam otot." },
  { "en": "Intransitif?", "id": "Tidak berobjek." },
  { "en": "Intraokuler?", "id": "Dalam mata." },
  { "en": "Intrapersonal?", "id": "Dalam diri." },
  { "en": "Intrapsikis?", "id": "Dalam jiwa." },
  { "en": "Intrauterin?", "id": "Dalam rahim." },
  { "en": "Intravaskuler?", "id": "Dalam pembuluh." },
  { "en": "Intravena?", "id": "Dalam pembuluh vena." },
  { "en": "Intrik?", "id": "Muslihat." },
  { "en": "Intrinsik?", "id": "Terkandung." },
  { "en": "Introduksi?", "id": "Pengenalan." },
  { "en": "Introjeksi?", "id": "Penyerapan sifat." },
  { "en": "Introitus?", "id": "Pembukaan." },
  { "en": "Intromisi?", "id": "Pemasukan." },
  { "en": "Introspeksi?", "id": "Mawas diri." },
  { "en": "Introversi?", "id": "Sikap tertutup." },
  { "en": "Introvert?", "id": "Tertutup." },
  { "en": "Intuisi?", "id": "Bisikan hati." },
  { "en": "Intuitif?", "id": "Berdasarkan intuisi." },
  { "en": "Intumesensi?", "id": "Pembengkakan." },
  { "en": "Invaginasi?", "id": "Pelekukan ke dalam." },
  { "en": "Invalid?", "id": "Tidak sah." },
  { "en": "Invaliditas?", "id": "Ketidaksahan." },
  { "en": "Invasi?", "id": "Serangan." },
  { "en": "Invasi?", "id": "Penyerbuan." },
  { "en": "Inveksi?", "id": "Infeksi (arkais)." },
  { "en": "Inventaris?", "id": "Daftar barang." },
  { "en": "Inventarisasi?", "id": "Pendataan." },
  { "en": "Invensi?", "id": "Penemuan." },
  { "en": "Inventor?", "id": "Penemu." },
  { "en": "Inversi?", "id": "Kebalikan." },
  { "en": "Invertebrata?", "id": "Hewan tak bertulang belakang." },
  { "en": "Investigasi?", "id": "Penyelidikan." },
  { "en": "Investigatif?", "id": "Menyelidik." },
  { "en": "Investigator?", "id": "Penyelidik." },
  { "en": "Investasi?", "id": "Penanaman modal." },
  { "en": "Investor?", "id": "Penanam modal." },
  { "en": "Invitasi?", "id": "Undangan." },
  { "en": "Involusi?", "id": "Kemunduran." },
  { "en": "Inyik?", "id": "Nenek (Minang)." },
  { "en": "Iod?", "id": "Yodium." },
  { "en": "Iodium?", "id": "Unsur kimia." },
  { "en": "Ion?", "id": "Atom bermuatan." },
  { "en": "Ionisasi?", "id": "Proses pembentukan ion." },
  { "en": "Ionosfer?", "id": "Lapisan atmosfer." },
  { "en": "Iota?", "id": "Sangat kecil." },
  { "en": "Ipa?", "id": "Ilmu Pengetahuan Alam." },
  { "en": "Ipar?", "id": "Saudara suami/istri." },
  { "en": "Ipas?", "id": "Racun." },
  { "en": "Ipis?", "id": "Tipis (arkais)." },
  { "en": "Ipok?", "id": "Asma (arkais)." },
  { "en": "Ips?", "id": "Ilmu Pengetahuan Sosial." },
  { "en": "Ipse?" , "id": "Dia sendiri (Latin)." },
  { "en": "Ipso facto?", "id": "Karena fakta itu (Latin)." },
  { "en": "Ipuh?", "id": "Racun panah." },
  { "en": "Ipuk?", "id": "Pupuk (arkais)." },
  { "en": "Iqamat?", "id": "Ikamat." },
  { "en": "Iqra?", "id": "Bacalah (Arab)." },
  { "en": "Ira?", "id": "Sayap (arkais)." },
  { "en": "Iradah?", "id": "Kehendak Tuhan." },
  { "en": "Iradat?", "id": "Iradah." },
  { "en": "Irak?", "id": "Negara." },
  { "en": "Irama?", "id": "Ritme." },
  { "en": "Iran?", "id": "Negara." },
  { "en": "Iras?", "id": "Wajah (arkais)." },
  { "en": "Irasional?", "id": "Tidak masuk akal." },
  { "en": "Iri?", "id": "Dengki." },
  { "en": "Irid?", "id": "Berkilau." },
  { "en": "Iridektomi?", "id": "Operasi mata." },
  { "en": "Iridium?", "id": "Unsur kimia." },
  { "en": "Iris?", "id": "Selaput pelangi mata." },
  { "en": "Pertanyaanirit?", "id": "Hemat." },
  { "en": "Iritabel?", "id": "Mudah marah." },
  { "en": "Iritabilitas?", "id": "Sifat mudah marah." },
  { "en": "Iritan?", "id": "Penyebab iritasi." },
  { "en": "Iritasi?", "id": "Gangguan." },
  { "en": "Ironi?", "id": "Sindiran halus." },
  { "en": "Ironis?", "id": "Bertentangan." },
  { "en": "Irregular?", "id": "Tidak teratur (Inggris)." },
  { "en": "Irrelevan?", "id": "Tidak berkaitan." },
  { "en": "Irigasi?", "id": "Pengairan." },
  { "en": "Irigator?", "id": "Alat irigasi." },
  { "en": "Is?", "id": "Seruan (Belanda)." },
  { "en": "Isa?", "id": "Nabi." },
  { "en": "Isak?", "id": "Tangis tertahan." },
  { "en": "Isap?", "id": "Sedot." },
  { "en": "Isbat?", "id": "Penetapan." },
  { "en": "Iseng?", "id": "Senggang." },
  { "en": "Isi?", "id": "Kandungan." },
  { "en": "Isih?", "id": "Bersih (Jawa)." },
  { "en": "Isim?", "id": "Kata benda (Arab)." },
  { "en": "Isis?", "id": "Dewi Mesir." },
  { "en": "Isit?", "id": "Gusi (arkais)." },
  { "en": "Islah?", "id": "Perdamaian." },
  { "en": "Islam?", "id": "Agama." },
  { "en": "Islami?", "id": "Bersifat Islam." },
  { "en": "Islamiah?", "id": "Islami." },
  { "en": "Islamisme?", "id": "Paham keislaman." },
  { "en": "Islamolog?", "id": "Ahli Islam." },
  { "en": "Islamologi?", "id": "Ilmu keislaman." },
  { "en": "Iso?", "id": "Sama (awalan)." },
  { "en": "Isobar?", "id": "Garis tekanan sama." },
  { "en": "Isodin?", "id": "Garis kekuatan sama." },
  { "en": "Isoelektronik?", "id": "Elektron sama." },
  { "en": "Isoenzim?", "id": "Enzim serupa." },
  { "en": "Isoet?", "id": "Garis curah hujan sama." },
  { "en": "Isofin?", "id": "Garis akhir fenomena." },
  { "en": "Isofoton?", "id": "Garis intensitas cahaya sama." },
  { "en": "Isogam?", "id": "Sel kelamin sama." },
  { "en": "Isogamet?", "id": "Isogam." },
  { "en": "Isogami?", "id": "Perkawinan isogam." },
  { "en": "Isoglos?", "id": "Garis batas bahasa." },
  { "en": "Isogon?", "id": "Sudut sama." },
  { "en": "Isogram?", "id": "Kata tanpa huruf ulang." },
  { "en": "Isohalin?", "id": "Garis kadar garam sama." },
  { "en": "Isohial?", "id": "Garis kedalaman laut sama." },
  { "en": "Isohips?", "id": "Garis ketinggian sama." },
  { "en": "Isokal?", "id": "Garis kualitas sama." },
  { "en": "Isokar?", "id": "Garis produksi sama." },
  { "en": "Isokemi?", "id": "Garis unsur kimia sama." },
  { "en": "Isokeraunik?", "id": "Garis badai sama." },
  { "en": "Isokor?", "id": "Garis volume sama." },
  { "en": "Isokron?", "id": "Waktu sama." },
  { "en": "Isolasi?", "id": "Pengasingan." },
  { "en": "Isolat?", "id": "Bahasa terpencil." },
  { "en": "Isolatif?", "id": "Mengasingkan." },
  { "en": "Isolator?", "id": "Penyekat." },
  { "en": "Isoleks?", "id": "Kata sama makna." },
  { "en": "Isoleusin?", "id": "Asam amino." },
  { "en": "Isomer?", "id": "Senyawa rumus sama." },
  { "en": "Isomerisme?", "id": "Gejala isomer." },
  { "en": "Isometrik?", "id": "Ukuran sama." },
  { "en": "Isomorf?", "id": "Bentuk sama." },
  { "en": "Isomorfis?", "id": "Bersifat isomorf." },
  { "en": "Isomorfisme?", "id": "Kesamaan bentuk." },
  { "en": "Isonomi?", "id": "Persamaan hukum." },
  { "en": "Isopati?", "id": "Metode pengobatan." },
  { "en": "Isopoda?", "id": "Jenis krustasea." },
  { "en": "Isoptera?", "id": "Ordo rayap." },
  { "en": "Isosianat?", "id": "Senyawa kimia." },
  { "en": "Isosilabisme?", "id": "Suku kata sama." },
  { "en": "Isospin?", "id": "Sifat partikel." },
  { "en": "Isospor?", "id": "Spora sama." },
  { "en": "Isostasi?", "id": "Keseimbangan kerak bumi." },
  { "en": "Isoterm?", "id": "Garis suhu sama." },
  { "en": "Isotonik?", "id": "Tekanan osmotik sama." },
  { "en": "Isotop?", "id": "Atom unsur sama." },
  { "en": "Isotrop?", "id": "Sifat sama semua arah." },
  { "en": "Isotropis?", "id": "Bersifat isotrop." },
  { "en": "Isra?", "id": "Perjalanan malam Nabi." },
  { "en": "Istal?", "id": "Kandang kuda." },
  { "en": "Istamben?", "id": "Benang emas." },
  { "en": "Istambul?", "id": "Kota." },
  { "en": "Istana?", "id": "Keraton." },
  { "en": "Istanggi?", "id": "Dupa." },
  { "en": "Istawa?", "id": "Khatulistiwa (arkais)." },
  { "en": "Isteri?", "id": "Istri." },
  { "en": "Istiazah?", "id": "Doa perlindungan." },
  { "en": "Istibra?", "id": "Penyucian (Islam)." },
  { "en": "Istidraj?", "id": "Kenikmatan semu." },
  { "en": "Istifham?", "id": "(Arab)." },
  { "en": "Istigfar?", "id": "Permohonan ampun." },
  { "en": "Istihadah?", "id": "Pendarahan wanita." },
  { "en": "Istikharah?", "id": "Salat minta petunjuk." },
  { "en": "Istiklal?", "id": "Kemerdekaan (Arab)." },
  { "en": "Istilah?", "id": "Kata khusus." },
  { "en": "Istimal?", "id": "Penggunaan (Arab)." },
  { "en": "Istimewa?", "id": "Khusus." },
  { "en": "Istimna?", "id": "Onani." },
  { "en": "Istinggar?", "id": "Senapan kuno." },
  { "en": "Istinja?", "id": "Bersuci." },
  { "en": "Istirahat?", "id": "Jeda." },
  { "en": "Istitaah?", "id": "Kemampuan (haji)." },
  { "en": "Istri?", "id": "Bini." },
  { "en": "Isu?", "id": "Kabar angin." },
  { "en": "Isuria?", "id": "Kencing sama banyak." },
  { "en": "Isyarat?", "id": "Tanda." },
  { "en": "Isytiak?", "id": "Rindu (Arab)." },
  { "en": "Italian?", "id": "Orang Italia." },
  { "en": "Italika?", "id": "Huruf miring." },
  { "en": "Itches?", "id": "Gatal (Inggris)." },
  { "en": "Item?", "id": "Butir." },
  { "en": "Iterasi?", "id": "Pengulangan." },
  { "en": "Iterbium?", "id": "Unsur kimia." },
  { "en": "Itifak?", "id": "Persetujuan (Arab)." },
  { "en": "Itik?", "id": "Bebek." },
  { "en": "Itikad?", "id": "Niat baik." },
  { "en": "Itil?", "id": "Kelentit." },
  { "en": "Itlak?", "id": "Mutlak (Arab)." },
  { "en": "Itu?", "id": "Jauh." },
  { "en": "Itrium?", "id": "Unsur kimia." },
  { "en": "Iud?", "id": "Alat kontrasepsi." },
  { "en": "Iur?", "id": "Sumbang." },
  { "en": "Iuran?", "id": "Sumbangan." },
  { "en": "Ius?", "id": "Hukum (Latin)." },
  { "en": "Iwad?", "id": "Pengganti (Arab)." },
  { "en": "Iwal?", "id": "Perihal (arkais)." },
  { "en": "Iwan?", "id": "Ruang terbuka." },
  { "en": "Iwil-iwil?", "id": "Kelelawar kecil." },
  { "en": "Iya?", "id": "Ya." },
  { "en": "Iyuh?", "id": "Seruan jijik." },
  { "en": "Izin?", "id": "Permisi." },
  { "en": "Izrail?", "id": "Malaikat pencabut nyawa." },
  { "en": "Ja?", "id": "Ya (arkais)." },
  { "en": "Jaba?", "id": "Luar (Jawa)." },
  { "en": "Jabal?", "id": "Gunung (Arab)." },
  { "en": "Jabang?", "id": "Bayi." },
  { "en": "Jabar?", "id": "Maha Perkasa (Allah)." },
  { "en": "Jabariah?", "id": "Paham takdir." },
  { "en": "Jabat?", "id": "Pegang." },
  { "en": "Jabatan?", "id": "Posisi." },
  { "en": "Jabel?", "id": "Ambil (arkais)." },
  { "en": "Jabi?", "id": "Jahe (arkais)." },
  { "en": "Jabil?", "id": "Saku (arkais)." },
  { "en": "Jabir?", "id": "Kain (arkais)." },
  { "en": "Jabu?", "id": "Debu (arkais)." },
  { "en": "Jabung?", "id": "Damar." },
  { "en": "Jadah?", "id": "Haram (anak)." },
  { "en": "Jadam?", "id": "Obat." },
  { "en": "Jadi?", "id": "Selesai." },
  { "en": "Jadu?", "id": "Sihir (India)." },
  { "en": "Jaduk?", "id": "Jagoan." },
  { "en": "Jadwal?", "id": "Agenda." },
  { "en": "Jaga?", "id": "Kawal." },
  { "en": "Jagabaya?", "id": "Polisi desa." },
  { "en": "Jagad?", "id": "Alam semesta." },
  { "en": "Jagal?", "id": "Penyembelih." },
  { "en": "Jagan?", "id": "Sayur (Jawa)." },
  { "en": "Jagang?", "id": "Penyangga." },
  { "en": "Jagat?", "id": "Jagad." },
  { "en": "Jago?", "id": "Ayam jantan." },
  { "en": "Jagoan?", "id": "Andalan." },
  { "en": "Jagorawi?", "id": "Jalan tol." },
  { "en": "Jagung?", "id": "Tanaman." },
  { "en": "Jagur?", "id": "Besar." },
  { "en": "Jah?", "id": "Hormat (arkais)." },
  { "en": "Jahal?", "id": "Kejam (arkais)." },
  { "en": "Jaharu?", "id": "Penjahat (arkais)." },
  { "en": "Jahat?", "id": "Buruk." },
  { "en": "Jahe?", "id": "Rempah." },
  { "en": "Jahil?", "id": "Nakal." },
  { "en": "Jahiliah?", "id": "Zaman kegelapan." },
  { "en": "Jahit?", "id": "Rangkai kain." },
  { "en": "Jail?", "id": "Nakal." },
  { "en": "Jais?", "id": "Tidak peduli (arkais)." },
  { "en": "Jaja?", "id": "Jual." },
  { "en": "Jajah?", "id": "Kuasai." },
  { "en": "Jajak?", "id": "Duga." },
  { "en": "Jajan?", "id": "Beli makanan kecil." },
  { "en": "Jajar?", "id": "Deret." },
  { "en": "Jaka?", "id": "Perjaka." },
  { "en": "Jakal?", "id": "Serigala." },
  { "en": "Jakas?", "id": "Kuat (arkais)." },
  { "en": "Jaket?", "id": "Baju luar." },
  { "en": "Jaksa?", "id": "Penuntut umum." },
  { "en": "Jaksi?", "id": "Penyakit (arkais)." },
  { "en": "Jakun?", "id": "Lekum." },
  { "en": "Jala?", "id": "Jaring ikan." },
  { "en": "Jalad?", "id": "Algojo (Arab)." },
  { "en": "Jalak?", "id": "Burung." },
  { "en": "Jalan?", "id": "Lintasan." },
  { "en": "Jalang?", "id": "Liar." },
  { "en": "Jalar?", "id": "Rambat." },
  { "en": "Jali?", "id": "Tanaman biji." },
  { "en": "Jalia?", "id": "Terang (Arab)." },
  { "en": "Jalin?", "id": "Anyam." },
  { "en": "Jalir?", "id": "Lancar (arkais)." },
  { "en": "Jalu?", "id": "Taji ayam." },
  { "en": "Jalur?", "id": "Lintasan." },
  { "en": "Jam?", "id": "Pukul." },
  { "en": "Jamaah?", "id": "Kumpulan orang." },
  { "en": "Jamadat?", "id": "Benda mati (Arab)." },
  { "en": "Jamah?", "id": "Sentuh." },
  { "en": "Jamak?", "id": "Banyak." },
  { "en": "Jamal?", "id": "Keindahan (Arab)." },
  { "en": "Jamang?", "id": "Hiasan dahi." },
  { "en": "Jamba?", "id": "Hidangan (Minang)." },
  { "en": "Jambak?", "id": "Tarik rambut." },
  { "en": "Jambal?", "id": "Ikan." },
  { "en": "Jamban?", "id": "Toilet." },
  { "en": "Jambang?", "id": "Bulu pipi." },
  { "en": "Jambar?", "id": "Pondok (arkais)." },
  { "en": "Jambat?", "id": "Jembatan (arkais)." },
  { "en": "Jambiah?", "id": "Pisau Arab." },
  { "en": "Jambo?", "id": "Pondok (Aceh)." },
  { "en": "Jambore?", "id": "Perkemahan pramuka." },
  { "en": "Jambu?", "id": "Buah." },
  { "en": "Jambuk?", "id": "Terkam (arkais)." },
  { "en": "Jambul?", "id": "Rambut depan." },
  { "en": "Jambur?", "id": "Lumbung." },
  { "en": "Jamhur?", "id": "Ahli." },
  { "en": "Jami?", "id": "Masjid besar." },
  { "en": "Jamin?", "id": "Tanggung." },
  { "en": "Jamis?", "id": "Kotor (arkais)." },
  { "en": "Jampi?", "id": "Mantra." },
  { "en": "Jampuk?", "id": "Burung hantu." },
  { "en": "Jamrah?", "id": "Tempat lempar jumrah." },
  { "en": "Jamu?", "id": "Obat tradisional." },
  { "en": "Jamur?", "id": "Fungi." },
  { "en": "Jana?", "id": "Manusia (Sanskerta)." },
  { "en": "Janabah?", "id": "Keadaan tidak suci." },
  { "en": "Janabijana?", "id": "Dunia fana (Sanskerta)." },
  { "en": "Janah?", "id": "Surga (Arab)." },
  { "en": "Janat?", "id": "Surga." },
  { "en": "Janda?", "id": "Wanita tanpa suami." },
  { "en": "Jandai?", "id": "Tari (arkais)." },
  { "en": "Jandom?", "id": "Jarum (arkais)." },
  { "en": "Jangar?", "id": "Pusing." },
  { "en": "Jangat?", "id": "Kulit." },
  { "en": "Janggal?", "id": "Aneh." },
  { "en": "Janggar?", "id": "Pial ayam." },
  { "en": "Janggi?", "id": "Orang Negro (arkais)." },
  { "en": "Janggut?", "id": "Rambut dagu." },
  { "en": "Jangka?", "id": "Alat ukur." },
  { "en": "Jangkah?", "id": "Langkah." },
  { "en": "Jangkang?", "id": "Berjalan mengangkang." },
  { "en": "Jangkar?", "id": "Sauh." },
  { "en": "Jangkau?", "id": "Capai." },
  { "en": "Jangki?", "id": "Keranjang." },
  { "en": "Jangkih?", "id": "Gesit (arkais)." },
  { "en": "Jangkrik?", "id": "Serangga." },
  { "en": "Jangkung?", "id": "Tinggi." },
  { "en": "Janik?", "id": "Kecil (arkais)." },
  { "en": "Janin?", "id": "Bayi dalam kandungan." },
  { "en": "Janipar?", "id": "Jahe." },
  { "en": "Janissary?", "id": "Tentara Turki." },
  { "en": "Janji?", "id": "Ikrar." },
  { "en": "Jantan?", "id": "Laki-laki (hewan)." },
  { "en": "Jantina?", "id": "Jenis kelamin." },
  { "en": "Jantuk?", "id": "Pukul (arkais)." },
  { "en": "Jantung?", "id": "Organ pemompa darah." },
  { "en": "Jantur?", "id": "Tukang sulap (arkais)." },
  { "en": "Januari?", "id": "Bulan pertama." },
  { "en": "Janur?", "id": "Daun kelapa muda." },
  { "en": "Jap?", "id": "Mantra (Hindu)." },
  { "en": "Japu?", "id": "Ikan." },
  { "en": "Japuk?", "id": "Tua bangka." },
  { "en": "Jara?", "id": "Pahat (arkais)." },
  { "en": "Jera?", "id": "Kapok." },
  { "en": "Jerabai?", "id": "Rumbai." },
  { "en": "Jeradik?", "id": "Serakah (arkais)." },
  { "en": "Jeragan?", "id": "Juragan." },
  { "en": "Jerah?", "id": "Banyak (arkais)." },
  { "en": "Jerahak?", "id": "Terbengkalai." },
  { "en": "Jerait?", "id": "Jahit (arkais)." },
  { "en": "Jeram?", "id": "Arus deras." },
  { "en": "Jeramah?", "id": "Terjang." },
  { "en": "Jerambah?", "id": "Jembatan." },
  { "en": "Jerambai?", "id": "Rumbai-rumbai." },
  { "en": "Jerambang?", "id": "Kunang-kunang." },
  { "en": "Jerami?", "id": "Batang padi kering." },
  { "en": "Jeran?", "id": "Kapok." },
  { "en": "Jerang?", "id": "Masak air." },
  { "en": "Jerangan?", "id": "Panci." },
  { "en": "Jerangkang?", "id": "Melintang." },
  { "en": "Jerangkong?", "id": "Kerangka." },
  { "en": "Jerapah?", "id": "Hewan leher panjang." },
  { "en": "Jerap?", "id": "Dekap." },
  { "en": "Jerat?", "id": "Perangkap." },
  { "en": "Jerau?", "id": "Merah tua." },
  { "en": "Jeraus?", "id": "Cepat (arkais)." },
  { "en": "Jerawat?", "id": "Bisul kecil." },
  { "en": "Jerba?", "id": "Lumat (arkais)." },
  { "en": "Jerepet?", "id": "Terikat." },
  { "en": "Jeres?", "id": "Gosok (arkais)." },
  { "en": "Jerewi?", "id": "Pancing (arkais)." },
  { "en": "Jeri?", "id": "Takut." },
  { "en": "Jeriah?", "id": "Susah payah." },
  { "en": "Jeriap?", "id": "Rata (arkais)." },
  { "en": "Jeriau?", "id": "Akar." },
  { "en": "Jerib?", "id": "Ukuran (Arab)." },
  { "en": "Jerigen?", "id": "Wadah." },
  { "en": "Jerih?", "id": "Lelah." },
  { "en": "Jeriji?", "id": "Kisi-kisi." },
  { "en": "Jering?", "id": "Buah jengkol." },
  { "en": "Jeringan?", "id": "Tempat jering." },
  { "en": "Jeringing?", "id": "Menyeringai (arkais)." },
  { "en": "Jerit?", "id": "Teriak." },
  { "en": "Jerjak?", "id": "Jeruji." },
  { "en": "Jerjau?", "id": "Lurus (arkais)." },
  { "en": "Jerkah?", "id": "Bentak." },
  { "en": "Jerkat?", "id": "Ikat (arkais)." },
  { "en": "Jerlau?", "id": "Cemerlang (arkais)." },
  { "en": "Jermah?", "id": "Dekap." },
  { "en": "Jermal?", "id": "Perangkap ikan." },
  { "en": "Jerman?", "id": "Negara." },
  { "en": "Jermin?", "id": "Cermin (arkais)." },
  { "en": "Jernang?", "id": "Getah merah." },
  { "en": "Jernih?", "id": "Bening." },
  { "en": "Jeroan?", "id": "Isi perut hewan." },
  { "en": "Jerohok?", "id": "Terperosok." },
  { "en": "Jerongkang?", "id": "Mengangkang." },
  { "en": "Jerongkes?", "id": "Tersungkur." },
  { "en": "Jerongkok?", "id": "Duduk jongkok." },
  { "en": "Jerubung?", "id": "Tenda perahu." },
  { "en": "Jeruji?", "id": "Kisi." },
  { "en": "Jeruju?", "id": "Tanaman." },
  { "en": "Jeruk?", "id": "Buah." },
  { "en": "Jerukun?", "id": "Rukun (arkais)." },
  { "en": "Jerum?", "id": "Menerkam." },
  { "en": "Jerumat?", "id": "Jahit." },
  { "en": "Jerumbai?", "id": "Rumbai." },
  { "en": "Jerumbun?", "id": "Tumpukan." },
  { "en": "Jerumus?", "id": "Terperosok." },
  { "en": "Jerung?", "id": "Ikan hiu." },
  { "en": "Jerungkau?", "id": "Menunduk." },
  { "en": "Jerungkung?", "id": "Tulang." },
  { "en": "Jerun?", "id": "Dalam (arkais)." },
  { "en": "Jersey?", "id": "Kaus (Inggris)." },
  { "en": "Jet?", "id": "Pesawat pancar gas." },
  { "en": "Jeti?", "id": "Dermaga." },
  { "en": "Jewawut?", "id": "Tanaman biji." },
  { "en": "Jewer?", "id": "Tarik telinga." },
  { "en": "Jiawang?", "id": "Burung." },
  { "en": "Jib?", "id": "Layar depan." },
  { "en": "Jibaku?", "id": "Serangan bunuh diri." },
  { "en": "Jibilah?", "id": "Sifat (Arab)." },
  { "en": "Jibrail?", "id": "Jibril." },
  { "en": "Jibril?", "id": "Malaikat." },
  { "en": "Jibti?", "id": "Sihir (Arab)." },
  { "en": "Jidah?", "id": "Leher (arkais)." },
  { "en": "Jidar?", "id": "Garis." },
  { "en": "Jidat?", "id": "Dahi." },
  { "en": "Jidor?", "id": "Beduk." },
  { "en": "Jig?", "id": "Alat penari." },
  { "en": "Jigsaw?", "id": "Gergaji ukir (Inggris)." },
  { "en": "Jihad?", "id": "Perjuangan (Islam)." },
  { "en": "Jihin?", "id": "Neraka (Arab)." },
  { "en": "Jijik?", "id": "Muak." },
  { "en": "Jika?", "id": "Andai." },
  { "en": "Jikalau?", "id": "Jika." },
  { "en": "Jila?", "id": "Retak (arkais)." },
  { "en": "Jilah?", "id": "Jilat (arkais)." },
  { "en": "Jilam?", "id": "Menjilat." },
  { "en": "Jilat?", "id": "Kecap dengan lidah." },
  { "en": "Jilbab?", "id": "Kerudung." },
  { "en": "Jilid?", "id": "Bendel." },
  { "en": "Jimak?", "id": "Sanggama." },
  { "en": "Jimat?", "id": "Benda sakti." },
  { "en": "Jimbit?", "id": "Jinjing (arkais)." },
  { "en": "Jimput?", "id": "Jemput." },
  { "en": "Jin?", "id": "Makhluk halus." },
  { "en": "Jina?", "id": "Zina (arkais)." },
  { "en": "Jinak?", "id": "Tidak liar." },
  { "en": "Jinayah?", "id": "Kejahatan (Islam)." },
  { "en": "Jindra?", "id": "Dewa (arkais)." },
  { "en": "Jingap?", "id": "Teriak (arkais)." },
  { "en": "Jingau?", "id": "Lihat." },
  { "en": "Jingga?", "id": "Merah kekuningan." },
  { "en": "Jinggring?", "id": "Takut (Jawa)." },
  { "en": "Jingkat?", "id": "Lompat." },
  { "en": "Jingkik?", "id": "Berjingkrak." },
  { "en": "Jingkrak?", "id": "Lompat-lompat." },
  { "en": "Jingo?", "id": "Nasionalis ekstrem." },
  { "en": "Jingu?", "id": "Kuil Shinto." },
  { "en": "Jinjang?", "id": "Panjang (leher)." },
  { "en": "Jinjat?", "id": "Sentak." },
  { "en": "Jinjing?", "id": "Bawa dengan tangan." },
  { "en": "Jinjit?", "id": "Berdiri ujung jari." },
  { "en": "Jinsom?", "id": "Ginseng." },
  { "en": "Jintan?", "id": "Rempah." },
  { "en": "Jip?", "id": "Mobil." },
  { "en": "Jiplak?", "id": "Tiru." },
  { "en": "Jipro?", "id": "Kain." },
  { "en": "Jir?", "id": "Tanda (permainan)." },
  { "en": "Jiran?", "id": "Tetangga." },
  { "en": "Jirat?", "id": "Nisan." },
  { "en": "Jirus?", "id": "Siram." },
  { "en": "Jisim?", "id": "Tubuh (Arab)." },
  { "en": "Jita?", "id": "Kain (India)." },
  { "en": "Jitak?", "id": "Pukul kepala." },
  { "en": "Jitu?", "id": "Tepat." },
  { "en": "Jiwa?", "id": "Roh." },
  { "en": "Jiwai?", "id": "Menjiwai." },
  { "en": "Jiwan?", "id": "Jiwa (arkais)." },
  { "en": "Jiwat?", "id": "Jiwa (arkais)." },
  { "en": "Jiwatman?", "id": "Jiwa tertinggi (Hindu)." },
  { "en": "Joang?", "id": "Juang (arkais)." },
  { "en": "Job?", "id": "Pekerjaan (Inggris)." },
  { "en": "Jobak?", "id": "Lubang (arkais)." },
  { "en": "Jobong?", "id": "Berlubang." },
  { "en": "Jodang?", "id": "Wadah makanan." },
  { "en": "Jodo?", "id": "Jodoh (Jawa)." },
  { "en": "Jodoh?", "id": "Pasangan." },
  { "en": "Jodong?", "id": "Tombak (arkais)." },
  { "en": "Jogan?", "id": "Lantai." },
  { "en": "Jogi?", "id": "Yogi." },
  { "en": "Joging?", "id": "Lari santai." },
  { "en": "Joglo?", "id": "Rumah adat Jawa." },
  { "en": "Jogo?", "id": "Jaga (Jawa)." },
  { "en": "Johor?", "id": "Negeri." },
  { "en": "Johari?", "id": "Permata." },
  { "en": "Jojol?", "id": "Menonjol." },
  { "en": "Jok?", "id": "Tempat duduk." },
  { "en": "Joki?", "id": "Penunggang kuda pacu." },
  { "en": "Jol?", "id": "Peti besi (Belanda)." },
  { "en": "Jolek?", "id": "Cantik (Minang)." },
  { "en": "Jolok?", "id": "Sodok." },
  { "en": "Jolong?", "id": "Maju." },
  { "en": "Jombang?", "id": "Sangat cantik." },
  { "en": "Jomlo?", "id": "Tidak punya pacar." },
  { "en": "Jompak?", "id": "Lompat (arkais)." },
  { "en": "Jompo?", "id": "Tua renta." },
  { "en": "Jon?", "id": "Panggilan (Belanda)." },
  { "en": "Jongang?", "id": "Menonjol (gigi)." },
  { "en": "Jonget?", "id": "Tari." },
  { "en": "Jongga?", "id": "Kaki belakang (kuda)." },
  { "en": "Jonggol?", "id": "Bukit." },
  { "en": "Jongjorang?", "id": "Belalang sembah." },
  { "en": "Jongkang?", "id": "Duduk menjulur kaki." },
  { "en": "Jongkar-jangkir?", "id": "Tidak karuan." },
  { "en": "Jongkat?", "id": "Sisir." },
  { "en": "Jongki?", "id": "Pelayan (arkais)." },
  { "en": "Jongko?", "id": "Toko kecil." },
  { "en": "Jongkok?", "id": "Duduk bertumpu kaki." },
  { "en": "Jongkong?", "id": "Perahu." },
  { "en": "Jongos?", "id": "Pelayan pria." },
  { "en": "Joran?", "id": "Gagang pancing." },
  { "en": "Jore?", "id": "Paling (arkais)." },
  { "en": "Jorjoran?", "id": "Berlomba-lomba (Jawa)." },
  { "en": "Jori?", "id": "Tali kekang." },
  { "en": "Joring?", "id": "Jering." },
  { "en": "Jorok?", "id": "Kotor." },
  { "en": "Jota?", "id": "Satuan (awalan)." },
  { "en": "Jotang?", "id": "Tanaman." },
  { "en": "Jotos?", "id": "Tinju." },
  { "en": "Jouken?", "id": "Syarat (Jepang)." },
  { "en": "Joule?", "id": "Satuan energi." },
  { "en": "Jrambah?", "id": "Jembatan (Jawa)." },
  { "en": "Jreng?", "id": "Bunyi gitar." },
  { "en": "Juadah?", "id": "Penganan." },
  { "en": "Juak?", "id": "Pengikut." },
  { "en": "Jual?", "id": "Menjual." },
  { "en": "Juang?", "id": "Berjuang." },
  { "en": "Juara?", "id": "Pemenang." },
  { "en": "Jub?", "id": "Alat musik (arkais)." },
  { "en": "Jubabah?", "id": "Pakaian." },
  { "en": "Jubad?", "id": "Musang." },
  { "en": "Jubel?", "id": "Sesak." },
  { "en": "Jubin?", "id": "Ubin." },
  { "en": "Jubir?", "id": "Juru bicara." },
  { "en": "Jublag?", "id": "Kolam (Sunda)." },
  { "en": "Jublek?", "id": "Kaget (Jawa)." },
  { "en": "Jubung?", "id": "Lumbung kecil." },
  { "en": "Judes?", "id": "Galak." },
  { "en": "Judi?", "id": "Taruhan." },
  { "en": "Judo?", "id": "Olahraga bela diri." },
  { "en": "Judogi?", "id": "Pakaian judo." },
  { "en": "Judoka?", "id": "Pemain judo." },
  { "en": "Judul?", "id": "Kepala karangan." },
  { "en": "Juega?", "id": "Main (Spanyol)." },
  { "en": "Juek?", "id": "Tidak tahu (Minang)." },
  { "en": "Juga?", "id": "Pun." },
  { "en": "Juhi?", "id": "Cumi-cumi kering." },
  { "en": "Juhut?", "id": "Daging (Batak)." },
  { "en": "Juih?", "id": "Cibir." },
  { "en": "Jujai?", "id": "Hujat (arkais)." },
  { "en": "Jujitsu?", "id": "Jujutsu." },
  { "en": "Jujuh?", "id": "Lurus." },
  { "en": "Jujul?", "id": "Kembalian uang." },
  { "en": "Jujur?", "id": "Lurus hati." },
  { "en": "Jujut?", "id": "Cabut." },
  { "en": "Jukstaposisi?", "id": "Pendampingan." },
  { "en": "Juku?", "id": "Rumput." },
  { "en": "Jukung?", "id": "Perahu." },
  { "en": "Julab?", "id": "Minuman manis." },
  { "en": "Julai?", "id": "Juli (arkais)." },
  { "en": "Julang?", "id": "Capai." },
  { "en": "Julat?", "id": "Jangkauan." },
  { "en": "Juleha?", "id": "Nama wanita." },
  { "en": "Juli?", "id": "Bulan ketujuh." },
  { "en": "Juling?", "id": "Mata juling." },
  { "en": "Julir?", "id": "Tombak." },
  { "en": "Julita?", "id": "Cantik (arkais)." },
  { "en": "Julu?", "id": "Atas (Batak)." },
  { "en": "Juluk?", "id": "Gelar." },
  { "en": "Julung-julung?", "id": "Ikan." },
  { "en": "Julur?", "id": "Ulur." },
  { "en": "Jum?", "id": "Cium (arkais)." },
  { "en": "Jumadilakhir?", "id": "Bulan Hijriah." },
  { "en": "Jumadilawal?", "id": "Bulan Hijriah." },
  { "en": "Jumantan?", "id": "Zamrud." },
  { "en": "Jumat?", "id": "Hari." },
  { "en": "Jumawa?", "id": "Sombong." },
  { "en": "Jumbai?", "id": "Rumbai." },
  { "en": "Jumbil?", "id": "Bibir tebal." },
  { "en": "Jumbuh?", "id": "Sesuai (Jawa)." },
  { "en": "Jumbo?", "id": "Sangat besar." },
  { "en": "Jumjum?", "id": "Tutup (mulut)." },
  { "en": "Jumlah?", "id": "Total." },
  { "en": "Jumnut?", "id": "Genggam (arkais)." },
  { "en": "Jumpa?", "id": "Temu." },
  { "en": "Jumpalit?", "id": "Salto." },
  { "en": "Jumpelang?", "id": "Lompat." },
  { "en": "Jumpul?", "id": "Kumpul (arkais)." },
  { "en": "Jumput?", "id": "Ambil dengan jari." },
  { "en": "Jumrah?", "id": "Lemparan batu (haji)." },
  { "en": "Jumud?", "id": "Statis." },
  { "en": "Jun?", "id": "Panci (Tionghoa)." },
  { "en": "Junam?", "id": "Menukik." },
  { "en": "Jundai?", "id": "Tentara (arkais)." },
  { "en": "Jung?", "id": "Kapal." },
  { "en": "Jungar?", "id": "Menonjol." },
  { "en": "Jungat?", "id": "Terangkat." },
  { "en": "Junggang?", "id": "Miring." },
  { "en": "Jungkal?", "id": "Jatuh." },
  { "en": "Jungkang?", "id": "Pantat terangkat." },
  { "en": "Jungkar?", "id": "Terbongkar (arkais)." },
  { "en": "Jungkat?", "id": "Sisir." },
  { "en": "Jungkir?", "id": "Salto." },
  { "en": "Jungkit?", "id": "Terangkat." },
  { "en": "Jungkol?", "id": "Bengkak." },
  { "en": "Jungur?", "id": "Moncong." },
  { "en": "Juni?", "id": "Bulan keenam." },
  { "en": "Junior?", "id": "Muda." },
  { "en": "Junjung?", "id": "Angkat." },
  { "en": "Juntai?", "id": "Bergantung." },
  { "en": "Juntara?", "id": "Dunia (arkais)." },
  { "en": "Jupang?", "id": "Kain (arkais)." },
  { "en": "Jupiter?", "id": "Planet." },
  { "en": "Jura?", "id": "Sumpah (arkais)." },
  { "en": "Juragan?", "id": "Tuan." },
  { "en": "Jurai?", "id": "Rumbai." },
  { "en": "Jurang?", "id": "Ngarai." },
  { "en": "Juri?", "id": "Penilai." },
  { "en": "Juriat?", "id": "Keturunan." },
  { "en": "Juridis?", "id": "Menurut hukum." },
  { "en": "Jurik?", "id": "Hantu." },
  { "en": "Juring?", "id": "Bagian lingkaran." },
  { "en": "Juris?", "id": "Ahli hukum." },
  { "en": "Jurnal?", "id": "Catatan harian." },
  { "en": "Jurnalis?", "id": "Wartawan." },
  { "en": "Jurnalisme?", "id": "Kewartawanan." },
  { "en": "Juru?", "id": "Ahli." },
  { "en": "Jurubahasa?", "id": "Penerjemah." },
  { "en": "Jurubicara?", "id": "Juru bicara." },
  { "en": "Jurudemung?", "id": "Penabuh demung." },
  { "en": "Jurumudi?", "id": "Pengemudi kapal." },
  { "en": "Jurung?", "id": "Sudut." },
  { "en": "Juruselamat?", "id": "Penyelamat." },
  { "en": "Jurus?", "id": "Arah." },
  { "en": "Jurusita?", "id": "Petugas pengadilan." },
  { "en": "Jus?", "id": "Sari buah." },
  { "en": "Jusi?", "id": "Kain serat nanas." },
  { "en": "Justifikasi?", "id": "Pembenaran." },
  { "en": "Justisi?", "id": "Peradilan." },
  { "en": "Jut?", "id": "Kain." },
  { "en": "Juta?", "id": "Satuan angka." },
  { "en": "Jutawan?", "id": "Orang kaya." },
  { "en": "Jute?", "id": "Serat." },
  { "en": "Jutek?", "id": "Tidak ramah." },
  { "en": "Ka?", "id": "Kakak (arkais)." },
  { "en": "Kaabah?", "id": "Kakbah." },
  { "en": "Kaan?", "id": "Akan (arkais)." },
  { "en": "Kaba?", "id": "Cerita Minang." },
  { "en": "Kabak?", "id": "Nama tumbuhan." },
  { "en": "Kabal?", "id": "Kebal." },
  { "en": "Kabar?", "id": "Berita." },
  { "en": "Kabaret?", "id": "Pertunjukan." },
  { "en": "Kabat?", "id": "Ikat (arkais)." },
  { "en": "Kabe?", "id": "Cabai (Jawa)." },
  { "en": "Kabihat?", "id": "Keji (Arab)." },
  { "en": "Kabilah?", "id": "Suku." },
  { "en": "Kabin?", "id": "Ruang." },
  { "en": "Kabinet?", "id": "Dewan menteri." },
  { "en": "Kabir?", "id": "Besar (Arab)." },
  { "en": "Kabisat?", "id": "Tahun lompat." },
  { "en": "Kabit?", "id": "Kait (arkais)." },
  { "en": "Kaboi?", "id": "Koboi." },
  { "en": "Kabriolet?", "id": "Mobil atap terbuka." },
  { "en": "Kabruk?", "id": "Tutup (arkais)." },
  { "en": "Kabul?", "id": "Terkabul." },
  { "en": "Kabuki?", "id": "Teater Jepang." },
  { "en": "Kabumbu?", "id": "Kumbang." },
  { "en": "Kabung?", "id": "Berkabung." },
  { "en": "Kabur?", "id": "Tidak jelas." },
  { "en": "Kabus?", "id": "Kabut." },
  { "en": "Kabut?", "id": "Asap tebal." },
  { "en": "Kaca?", "id": "Beling." },
  { "en": "Kacai?", "id": "Remeh (arkais)." },
  { "en": "Kacak?", "id": "Gagah." },
  { "en": "Kacang?", "id": "Polong-polongan." },
  { "en": "Kacapiring?", "id": "Bunga." },
  { "en": "Kacau?", "id": "Tidak teratur." },
  { "en": "Kacek?", "id": "Selisih." },
  { "en": "Kacer?", "id": "Burung." },
  { "en": "Kaci?", "id": "Kain putih." },
  { "en": "Kacici?", "id": "Burung." },
  { "en": "Kacip?", "id": "Alat pemotong." },
  { "en": "Kacipir?", "id": "Sayuran." },
  { "en": "Kaco?", "id": "Kaca (arkais)." },
  { "en": "Kacu?", "id": "Sapu tangan." },
  { "en": "Kacuk?", "id": "Campur (arkais)." },
  { "en": "Kacukan?", "id": "Campuran." },
  { "en": "Kada?", "id": "Kadar (arkais)." },
  { "en": "Kadal?", "id": "Reptil." },
  { "en": "Kadam?", "id": "Hamba." },
  { "en": "Kadang?", "id": "Adakala." },
  { "en": "Kadar?", "id": "Ukuran." },
  { "en": "Kadariah?", "id": "Paham takdir bebas." },
  { "en": "Kadas?", "id": "Penyakit kulit." },
  { "en": "Kadaster?", "id": "Daftar tanah." },
  { "en": "Kadastral?", "id": "Berkaitan tanah." },
  { "en": "Kade?", "id": "Dermaga." },
  { "en": "Kadeh?", "id": "Gelas (arkais)." },
  { "en": "Kademangan?", "id": "Wilayah demang." },
  { "en": "Kader?", "id": "Calon pemimpin." },
  { "en": "Kades?", "id": "Kepala desa." },
  { "en": "Kadet?", "id": "Taruna." },
  { "en": "Kadi?", "id": "Hakim agama." },
  { "en": "Kadim?", "id": "Keluarga." },
  { "en": "Kadipaten?", "id": "Wilayah adipati." },
  { "en": "Kadir?", "id": "Maha Kuasa (Allah)." },
  { "en": "Kadiriah?", "id": "Tarekat." },
  { "en": "Kadmium?", "id": "Unsur kimia." },
  { "en": "Kado?", "id": "Hadiah." },
  { "en": "Kadofor?", "id": "Tangkai spora." },
  { "en": "Kadru?", "id": "Cokelat (Sanskerta)." },
  { "en": "Kadung?", "id": "Terlanjur." },
  { "en": "Kadur?", "id": "Paksa (arkais)." },
  { "en": "Kadut?", "id": "Karung." },
  { "en": "Kaedah?", "id": "Kaidah." },
  { "en": "Kaf?", "id": "Huruf Arab." },
  { "en": "Kafaah?", "id": "Kesetaraan (nikah)." },
  { "en": "Kafan?", "id": "Kain pembungkus mayat." },
  { "en": "Kafar?", "id": "Orang kafir (arkais)." },
  { "en": "Kafarat?", "id": "Denda (Islam)." },
  { "en": "Kafe?", "id": "Kedai kopi." },
  { "en": "Kafeina?", "id": "Zat perangsang." },
  { "en": "Kafetaria?", "id": "Restoran." },
  { "en": "Kafi?", "id": "Cukup." },
  { "en": "Kafilah?", "id": "Rombongan." },
  { "en": "Kafir?", "id": "Tidak beriman." },
  { "en": "Kagak?", "id": "Tidak (kasar)." },
  { "en": "Kaget?", "id": "Terkejut." },
  { "en": "Kagok?", "id": "Canggung." },
  { "en": "Kagum?", "id": "Takjub." },
  { "en": "Kah?", "id": "Partikel tanya." },
  { "en": "Kahaf?", "id": "Gua (Arab)." },
  { "en": "Kahak?", "id": "Dahak." },
  { "en": "Kahan?", "id": "Pendeta Yahudi." },
  { "en": "Kahar?", "id": "Gerobak." },
  { "en": "Kahat?", "id": "Kekeringan." },
  { "en": "Kahin?", "id": "Dukun (Arab)." },
  { "en": "Kahwa?", "id": "Kopi (Arab)." },
  { "en": "Kahyangan?", "id": "Surga." },
  { "en": "Kaidah?", "id": "Aturan." },
  { "en": "Kail?", "id": "Pancing." },
  { "en": "Kaim?", "id": "Berdiri (Arab)." },
  { "en": "Kain?", "id": "Tekstil." },
  { "en": "Kincir?", "id": "Roda berputar." },
  { "en": "Kipa?", "id": "Topi Yahudi." },
  { "en": "KIP?", "id": "Kartu Identitas Penduduk." },
  { "en": "Kais?", "id": "Garuk." },
  { "en": "Kaisar?", "id": "Maharaja." },
  { "en": "Kait?", "id": "Sangkut." },
  { "en": "Kajai?", "id": "Sihir (Minang)." },
  { "en": "Kajang?", "id": "Atap perahu." },
  { "en": "Kaji?", "id": "Telaah." },
  { "en": "Kak?", "id": "Kakak." },
  { "en": "Kaka?", "id": "Kakak." },
  { "en": "Kakaban?", "id": "Tempat telur ikan." },
  { "en": "Kakagau?", "id": "Gagap (arkais)." },
  { "en": "Kakak?", "id": "Saudara tua." },
  { "en": "Kakanda?", "id": "Kakak (hormat)." },
  { "en": "Kakang?", "id": "Kakak (Jawa)." },
  { "en": "Kakao?", "id": "Cokelat." },
  { "en": "Kakap?", "id": "Ikan." },
  { "en": "Kakas?", "id": "Kikis." },
  { "en": "Kakatua?", "id": "Burung." },
  { "en": "Kakawin?", "id": "Puisi Jawa kuno." },
  { "en": "Kakek?", "id": "Datuk." },
  { "en": "Kaki?", "id": "Alat berjalan." },
  { "en": "Kaktus?", "id": "Tanaman berduri." },
  { "en": "Kaku?", "id": "Tidak lentur." },
  { "en": "Kakus?", "id": "WC." },
  { "en": "Kala?", "id": "Waktu." },
  { "en": "Kalah?", "id": "Tewas." },
  { "en": "Kalai?", "id": "Lerai (arkais)." },
  { "en": "Kalam?", "id": "Pena." },
  { "en": "Kalamba?", "id": "Peti mayat." },
  { "en": "Kalambak?", "id": "Kayu gaharu." },
  { "en": "Kalambu?", "id": "Kelambu." },
  { "en": "Kalang?", "id": "Sangga." },
  { "en": "Kalap?", "id": "Hilang akal." },
  { "en": "Kalas?", "id": "Lepas (ikatan)." },
  { "en": "Kalat?", "id": "Sengat (arkais)." },
  { "en": "Kalau?", "id": "Jika." },
  { "en": "Kalbi?", "id": "Jantung (Arab)." },
  { "en": "Kalbu?", "id": "Hati." },
  { "en": "Kaldera?", "id": "Kawah gunung." },
  { "en": "Kalebas?", "id": "Labu." },
  { "en": "Kalem?", "id": "Tenang." },
  { "en": "Kalempagi?", "id": "Padi." },
  { "en": "Kalender?", "id": "Penanggalan." },
  { "en": "Kaleng?", "id": "Wadah logam." },
  { "en": "Kali?", "id": "Sungai." },
  { "en": "Kalian?", "id": "Kamu semua." },
  { "en": "Kaliber?", "id": "Ukuran." },
  { "en": "Kalibut?", "id": "Kacau (arkais)." },
  { "en": "Kalicau?", "id": "Kicau (arkais)." },
  { "en": "Kalifah?", "id": "Khalifah." },
  { "en": "Kaligraf?", "id": "Ahli kaligrafi." },
  { "en": "Kaligrafi?", "id": "Seni menulis indah." },
  { "en": "Kalika?", "id": "Kuncup bunga (Sanskerta)." },
  { "en": "Kalimah?", "id": "Kalimat." },
  { "en": "Kalimat?", "id": "Rangkaian kata." },
  { "en": "Kalimpanang?", "id": "Pohon." },
  { "en": "Kalingan?", "id": "Terhalang (Jawa)." },
  { "en": "Kalio?", "id": "Masakan." },
  { "en": "Kalipah?", "id": "Khalifah." },
  { "en": "Kaliptra?", "id": "Tudung akar." },
  { "en": "Kalis?", "id": "Tidak lengket." },
  { "en": "Kalistenik?", "id": "Senam." },
  { "en": "Kalium?", "id": "Unsur kimia." },
  { "en": "Kalk?", "id": "Kapur." },
  { "en": "Kalkarium?", "id": "Kapur (arkais)." },
  { "en": "Kalkasar?", "id": "Jelatang." },
  { "en": "Kalkopirit?", "id": "Mineral." },
  { "en": "Kalkosian?", "id": "Mineral." },
  { "en": "Kalkulasi?", "id": "Perhitungan." },
  { "en": "Kalkulator?", "id": "Alat hitung." },
  { "en": "Kalkulus?", "id": "Matematika." },
  { "en": "Kalkun?", "id": "Ayam belanda." },
  { "en": "Kalo?", "id": "Kalau (arkais)." },
  { "en": "Kalomel?", "id": "Senyawa raksa." },
  { "en": "Kalong?", "id": "Kelelawar besar." },
  { "en": "Kalor?", "id": "Panas." },
  { "en": "Kalori?", "id": "Satuan energi." },
  { "en": "Kalorimeter?", "id": "Pengukur kalor." },
  { "en": "Kalorimetri?", "id": "Pengukuran kalor." },
  { "en": "Kalpataru?", "id": "Pohon kehidupan." },
  { "en": "Kalsedon?", "id": "Batu permata." },
  { "en": "Kalsiferol?", "id": "Vitamin D." },
  { "en": "Kalsifikasi?", "id": "Pengapuran." },
  { "en": "Kalsinasi?", "id": "Pemanasan." },
  { "en": "Kalsit?", "id": "Mineral." },
  { "en": "Kalsium?", "id": "Unsur kimia." },
  { "en": "Kalu?", "id": "Bisu (arkais)." },
  { "en": "Kaluas?", "id": "Musang." },
  { "en": "Kalui?", "id": "Ikan." },
  { "en": "Kalumet?", "id": "Pipa damai Indian." },
  { "en": "Kalung?", "id": "Perhiasan leher." },
  { "en": "Kalus?", "id": "Kapalan." },
  { "en": "Kama?", "id": "Cinta (Sanskerta)." },
  { "en": "Kamajaya?", "id": "Dewa cinta." },
  { "en": "Kamalir?", "id": "Saluran air." },
  { "en": "Kamantuhu?", "id": "Kepala desa (arkais)." },
  { "en": "Kamar?", "id": "Ruangan." },
  { "en": "Kamarban?", "id": "Ikat pinggang." },
  { "en": "Kamariah?", "id": "Berkaitan bulan." },
  { "en": "Kamas?", "id": "Asam (arkais)." },
  { "en": "Kamat?", "id": "Koma." },
  { "en": "Kamba?", "id": "Lebar (arkais)." },
  { "en": "Kamban?", "id": "Kembang (arkais)." },
  { "en": "Kambang?", "id": "Apung." },
  { "en": "Kambar?", "id": "Kembar (arkais)." },
  { "en": "Kambeh?", "id": "Kambuh (arkais)." },
  { "en": "Kambi?", "id": "Kulit kayu." },
  { "en": "Kambing?", "id": "Hewan ternak." },
  { "en": "Kambium?", "id": "Lapisan batang." },
  { "en": "Kamboja?", "id": "Bunga." },
  { "en": "Kambrium?", "id": "Zaman geologi." },
  { "en": "Kambuh?", "id": "Muncul lagi (penyakit)." },
  { "en": "Kambus?", "id": "Timbun." },
  { "en": "Kamel?", "id": "Unta." },
  { "en": "Kamelia?", "id": "Bunga." },
  { "en": "Kamera?", "id": "Alat foto." },
  { "en": "Kamerad?", "id": "Kawan." },
  { "en": "Kamerawan?", "id": "Juru kamera." },
  { "en": "Kamfana?", "id": "Kapur barus." },
  { "en": "Kamfer?", "id": "Kamfana." },
  { "en": "Kamfor?", "id": "Kamfana." },
  { "en": "Kamhar?", "id": "Kain wol." },
  { "en": "Kami?", "id": "Kita (eksklusif)." },
  { "en": "Kamikaze?", "id": "Serangan bunuh diri (Jepang)." },
  { "en": "Kamil?", "id": "Sempurna (Arab)." },
  { "en": "Kamis?", "id": "Hari." },
  { "en": "Kamisa?", "id": "Baju (Portugis)." },
  { "en": "Kamisol?", "id": "Pakaian dalam wanita." },
  { "en": "Kamka?", "id": "Kain sutra." },
  { "en": "Kamp?", "id": "Perkemahan." },
  { "en": "Kampabel?", "id": "Pakar (arkais)." },
  { "en": "Kampai?", "id": "Sorak minum (Jepang)." },
  { "en": "Kampal?", "id": "Gumpal (arkais)." },
  { "en": "Kampang?", "id": "Sialan (kasar)." },
  { "en": "Kampanye?", "id": "Promosi politik." },
  { "en": "Kampar?", "id": "Terlempar." },
  { "en": "Kampas?", "id": "Kanvas." },
  { "en": "Kampi?", "id": "Selimut (arkais)." },
  { "en": "Kampiun?", "id": "Juara." },
  { "en": "Kampos?", "id": "Padang rumput (Amerika Selatan)." },
  { "en": "Kampret?", "id": "Kelelawar." },
  { "en": "Kampsis?", "id": "Pohon." },
  { "en": "Kampung?", "id": "Desa." },
  { "en": "Kampuh?", "id": "Selimut." },
  { "en": "Kampul?", "id": "Apung (arkais)." },
  { "en": "Kampus?", "id": "Area universitas." },
  { "en": "Kamu?", "id": "Engkau." },
  { "en": "Kamus?", "id": "Buku kosakata." },
  { "en": "Kana?", "id": "Aksara Jepang." },
  { "en": "Kanabis?", "id": "Ganja." },
  { "en": "Kanal?", "id": "Saluran." },
  { "en": "Kanalisasi?", "id": "Pembuatan saluran." },
  { "en": "Kanang?", "id": "Kanan (arkais)." },
  { "en": "Kancap?", "id": "Penuh." },
  { "en": "Kancil?", "id": "Pelanduk." },
  { "en": "Kancing?", "id": "Penutup baju." },
  { "en": "Kancung?", "id": "Celana pendek (arkais)." },
  { "en": "Kancut?", "id": "Celana dalam." },
  { "en": "Kanda?", "id": "Kakak." },
  { "en": "Kandang?", "id": "Rumah hewan." },
  { "en": "Kandarah?", "id": "Perut (arkais)." },
  { "en": "Kandas?", "id": "Gagal." },
  { "en": "Kandel?", "id": "Tebal (Jawa)." },
  { "en": "Kandela?", "id": "Satuan intensitas cahaya." },
  { "en": "Kandidat?", "id": "Calon." },
  { "en": "Kandidiasis?", "id": "Infeksi jamur." },
  { "en": "Kandil?", "id": "Pelita." },
  { "en": "Kandis?", "id": "Buah." },
  { "en": "Kandul?", "id": "Karung (arkais)." },
  { "en": "Kandung?", "id": "Muat." },
  { "en": "Kandungan?", "id": "Isi." },
  { "en": "Kandut?", "id": "Bawa (arkais)." },
  { "en": "Kane?", "id": "Kain (Jepang)." },
  { "en": "Kanela?", "id": "Kayu manis." },
  { "en": "Kang?", "id": "Kakak (Jawa/Sunda)." },
  { "en": "Kangar?", "id": "Bau tak sedap." },
  { "en": "Kangen?", "id": "Rindu." },
  { "en": "Kangjeng?", "id": "Tuan (Jawa)." },
  { "en": "Kangkang?", "id": "Terbuka lebar (kaki)." },
  { "en": "Kangkung?", "id": "Sayuran." },
  { "en": "Kangmas?", "id": "Kakak laki-laki (Jawa)." },
  { "en": "Kangsa?", "id": "Logam." },
  { "en": "Kangsar?", "id": "Sombong (arkais)." },
  { "en": "Kangtau?", "id": "Perantara (Tionghoa)." },
  { "en": "Kanker?", "id": "Penyakit." },
  { "en": "Kanon?", "id": "Aturan gereja." },
  { "en": "Kanonik?", "id": "Sesuai kanon." },
  { "en": "Kanonir?", "id": "Prajurit meriam." },
  { "en": "Kanopi?", "id": "Tudung." },
  { "en": "Kanta?", "id": "Lensa." },
  { "en": "Kantan?", "id": "Bunga kecombrang." },
  { "en": "Kantang?", "id": "Kering (arkais)." },
  { "en": "Kantar?", "id": "Neraca (Arab)." },
  { "en": "Kantata?", "id": "Musik vokal." },
  { "en": "Kantih?", "id": "Pintal (arkais)." },
  { "en": "Kantil?", "id": "Bunga." },
  { "en": "Kantin?", "id": "Kedai makan." },
  { "en": "Kanto?", "id": "Nyanyian (sastra)." },
  { "en": "Kantong?", "id": "Saku." },
  { "en": "Kantor?", "id": "Tempat kerja." },
  { "en": "Kantuk?", "id": "Mengantuk." },
  { "en": "Kantung?", "id": "Wadah." },
  { "en": "Kanun?", "id": "Undang-undang." },
  { "en": "Kanvas?", "id": "Kain tebal." },
  { "en": "Kanya?", "id": "Gadis (Sanskerta)." },
  { "en": "Kaolin?", "id": "Tanah liat." },
  { "en": "Kaon?", "id": "Partikel." },
  { "en": "Kaos?", "id": "Baju." },
  { "en": "Kap?", "id": "Tudung lampu." },
  { "en": "Kapa?", "id": "Kain kulit kayu." },
  { "en": "Kapabel?", "id": "Mampu." },
  { "en": "Kapah?", "id": "Terkejut (arkais)." },
  { "en": "Kapai?", "id": "Menggapai." },
  { "en": "Kapak?", "id": "Alat potong." },
  { "en": "Kapal?", "id": "Kendaraan laut." },
  { "en": "Kapan?", "id": "Bila." },
  { "en": "Kapang?", "id": "Jamur." },
  { "en": "Kapar?", "id": "Tersebar." },
  { "en": "Kaparat?", "id": "Sialan (kasar)." },
  { "en": "Kapas?", "id": "Serat." },
  { "en": "Kapasitans?", "id": "Ukuran kapasitor." },
  { "en": "Kapasitas?", "id": "Daya tampung." },
  { "en": "Kapasitor?", "id": "Komponen listrik." },
  { "en": "Kape?", "id": "Alat kerik." },
  { "en": "Kapel?", "id": "Gereja kecil." },
  { "en": "Kaper?", "id": "Ngengat." },
  { "en": "Kapi?", "id": "Kerekan." },
  { "en": "Kapilaritas?", "id": "Daya resap." },
  { "en": "Kapiler?", "id": "Pembuluh halus." },
  { "en": "Kapiran?", "id": "Terlantar (Jawa)." },
  { "en": "Kapital?", "id": "Modal." },
  { "en": "Kapitalis?", "id": "Penganut kapitalisme." },
  { "en": "Kapitalisme?", "id": "Sistem ekonomi." },
  { "en": "Kapitasi?", "id": "Pajak per kepala." },
  { "en": "Kapiten?", "id": "Kapten." },
  { "en": "Kapitol?", "id": "Gedung parlemen AS." },
  { "en": "Kapitula?", "id": "Bunga majemuk." },
  { "en": "Kapitulas?", "id": "Penyerahan." },
  { "en": "Kaplares?", "id": "Polisi militer (Belanda)." },
  { "en": "Kapok?", "id": "Jera." },
  { "en": "Kaporal?", "id": "Pangkat militer." },
  { "en": "Kaporit?", "id": "Zat kimia." },
  { "en": "Kapsel?", "id": "Wadah obat." },
  { "en": "Kapstan?", "id": "Mesin derek." },
  { "en": "Kapsul?", "id": "Kapsel." },
  { "en": "Kapten?", "id": "Pemimpin." },
  { "en": "Kapu?", "id": "Apu (arkais)." },
  { "en": "Kapuk?", "id": "Serat pohon." },
  { "en": "Kapulaga?", "id": "Rempah." },
  { "en": "Kapung?", "id": "Terapung." },
  { "en": "Kapur?", "id": "Zat putih." },
  { "en": "Kapus?", "id": "Kapas (arkais)." },
  { "en": "Kara?", "id": "Kacang." },
  { "en": "Karabao?", "id": "Kerbau Filipina." },
  { "en": "Karabin?", "id": "Senapan." },
  { "en": "Karaf?", "id": "Botol." },
  { "en": "Karah?", "id": "Kerak periuk." },
  { "en": "Karakter?", "id": "Watak." },
  { "en": "Karakterisasi?", "id": "Penggambaran watak." },
  { "en": "Karakteristik?", "id": "Ciri khas." },
  { "en": "Karambol?", "id": "Permainan." },
  { "en": "Karang?", "id": "Batuan laut." },
  { "en": "Karangkitri?", "id": "Taman (Sanskerta)." },
  { "en": "Karantina?", "id": "Pengasingan." },
  { "en": "Karap?", "id": "Sering (arkais)." },
  { "en": "Karar?", "id": "Tetap (Arab)." },
  { "en": "Karas?", "id": "Kue." },
  { "en": "Karat?", "id": "Korosi." },
  { "en": "Karate?", "id": "Olahraga bela diri." },
  { "en": "Karategi?", "id": "Pakaian karate." },
  { "en": "Karateka?", "id": "Pemain karate." },
  { "en": "Karau?", "id": "Kacau (suara)." },
  { "en": "Karavan?", "id": "Rombongan kafilah." },
  { "en": "Karawitan?", "id": "Seni gamelan." },
  { "en": "Karbida?", "id": "Senyawa karbon." },
  { "en": "Karbohidrase?", "id": "Enzim." },
  { "en": "Karbohidrat?", "id": "Zat tepung." },
  { "en": "Karbol?", "id": "Disinfektan." },
  { "en": "Karbon?", "id": "Unsur kimia." },
  { "en": "Karbonado?", "id": "Intan hitam." },
  { "en": "Karbonat?", "id": "Garam asam karbon." },
  { "en": "Karbonil?", "id": "Gugus kimia." },
  { "en": "Karborundum?", "id": "Bahan abrasif." },
  { "en": "Karburasi?", "id": "Pencampuran bahan bakar." },
  { "en": "Karburator?", "id": "Bagian mesin." },
  { "en": "Karcis?", "id": "Tiket." },
  { "en": "Kardan?", "id": "Gardan." },
  { "en": "Kardamunggu?", "id": "Kapulaga." },
  { "en": "Kardia?", "id": "Jantung." },
  { "en": "Kardialgia?", "id": "Nyeri jantung." },
  { "en": "Kardigan?", "id": "Rompi rajut." },
  { "en": "Kardinal?", "id": "Pejabat gereja." },
  { "en": "Kardiograf?", "id": "Perekam jantung." },
  { "en": "Kardiogram?", "id": "Hasil rekam jantung." },
  { "en": "Kardiolog?", "id": "Ahli jantung." },
  { "en": "Kardiologi?", "id": "Ilmu jantung." },
  { "en": "Kardiopati?", "id": "Penyakit jantung." },
  { "en": "Kardioskop?", "id": "Alat periksa jantung." },
  { "en": "Karditis?", "id": "Radang jantung." },
  { "en": "Kare?", "id": "Masakan." },
  { "en": "Karena?", "id": "Sebab." },
  { "en": "Karet?", "id": "Bahan elastis." },
  { "en": "Kari?", "id": "Kare." },
  { "en": "Karib?", "id": "Akrab." },
  { "en": "Karier?", "id": "Pekerjaan." },
  { "en": "Karih?", "id": "Aduk (arkais)." },
  { "en": "Karikatur?", "id": "Gambar lucu." },
  { "en": "Karim?", "id": "Mulia (Arab)." },
  { "en": "Karina?", "id": "Belas kasihan (arkais)." },
  { "en": "Karisma?", "id": "Daya tarik." },
  { "en": "Karismatik?", "id": "Berwibawa." },
  { "en": "Karitatif?", "id": "Amal." },
  { "en": "Karna?", "id": "Telinga (Sanskerta)." },
  { "en": "Karnasi?", "id": "Anyelir." },
  { "en": "Karnaval?", "id": "Pesta rakyat." },
  { "en": "Karnivor?", "id": "Pemakan daging." },
  { "en": "Karo?", "id": "Suku Batak." },
  { "en": "Karosel?", "id": "Komidi putar." },
  { "en": "Karoseri?", "id": "Badan mobil." },
  { "en": "Karotena?", "id": "Pigmen." },
  { "en": "Karotenoid?", "id": "Pigmen." },
  { "en": "Karpai?", "id": "Kapas (arkais)." },
  { "en": "Karpel?", "id": "Daun buah." },
  { "en": "Karper?", "id": "Ikan." },
  { "en": "Karpet?", "id": "Permadani." },
  { "en": "Karsa?", "id": "Kehendak." },
  { "en": "Karsinogen?", "id": "Penyebab kanker." },
  { "en": "Karsinogenik?", "id": "Bersifat karsinogen." },
  { "en": "Karsinoma?", "id": "Kanker." },
  { "en": "Karta?", "id": "Selamat (Sanskerta)." },
  { "en": "Kartaraharja?", "id": "Aman sejahtera." },
  { "en": "Kartel?", "id": "Gabungan perusahaan." },
  { "en": "Karti?", "id": "Pekerjaan (Sanskerta)." },
  { "en": "Kartika?", "id": "Bintang." },
  { "en": "Kartilago?", "id": "Tulang rawan." },
  { "en": "Kartografi?", "id": "Ilmu peta." },
  { "en": "Kartogram?", "id": "Peta statistik." },
  { "en": "Karton?", "id": "Kertas tebal." },
  { "en": "Kartu?", "id": "Lembaran kecil." },
  { "en": "Kartun?", "id": "Gambar lucu." },
  { "en": "Kartunis?", "id": "Pelukis kartun." },
  { "en": "Karu?", "id": "Kacau (pikiran)." },
  { "en": "Karuh?", "id": "Leluhur (Sunda)." },
  { "en": "Karun?", "id": "Harta terpendam." },
  { "en": "Karung?", "id": "Wadah besar." },
  { "en": "Karunia?", "id": "Pemberian." },
  { "en": "Karunkel?", "id": "Daging tumbuh." },
  { "en": "Karusi?", "id": "Kursi (arkais)." },
  { "en": "Karya?", "id": "Ciptaan." },
  { "en": "Karyah?", "id": "Pekerjaan (arkais)." },
  { "en": "Karyawan?", "id": "Pegawai." },
  { "en": "Karyawati?", "id": "Pegawai wanita." },
  { "en": "Karyawisata?", "id": "Perjalanan belajar." },
  { "en": "Kas?", "id": "Tempat uang." },
  { "en": "Kasa?", "id": "Kain tipis." },
  { "en": "Kasab?", "id": "Benang emas." },
  { "en": "Kasad?", "id": "Maksud (Arab)." },
  { "en": "Kasah?", "id": "Kasar." },
  { "en": "Kasai?", "id": "Bedak." },
  { "en": "Kasak-kusuk?", "id": "Desas-desus." },
  { "en": "Kasam?", "id": "Dendam (arkais)." },
  { "en": "Kasang?", "id": "Kering (arkais)." },
  { "en": "Kasap?", "id": "Kasar." },
  { "en": "Kasar?", "id": "Tidak halus." },
  { "en": "Kasasi?", "id": "Pembatalan putusan." },
  { "en": "Kasatmata?", "id": "Terlihat." },
  { "en": "Kasau?", "id": "Rangka atap." },
  { "en": "Kasein?", "id": "Protein susu." },
  { "en": "Kasemat?", "id": "Ruang benteng." },
  { "en": "Kasep?", "id": "Tampan (Sunda)." },
  { "en": "Kaserin?", "id": "Panci." },
  { "en": "Kaserol?", "id": "Panci tahan panas." },
  { "en": "Kaset?", "id": "Pita rekaman." },
  { "en": "Kasi?", "id": "Sayang." },
  { "en": "Kasidah?", "id": "Nyanyian Arab." },
  { "en": "Kasih?", "id": "Cinta." },
  { "en": "Kasihan?", "id": "Iba." },
  { "en": "Kasiku?", "id": "Siku (arkais)." },
  { "en": "Kasim?", "id": "Pria dikebiri." },
  { "en": "Kasino?", "id": "Tempat judi." },
  { "en": "Kasintu?", "id": "Burung." },
  { "en": "Kasip?", "id": "Terlambat." },
  { "en": "Kasir?", "id": "Juru uang." },
  { "en": "Kaskade?", "id": "Air terjun kecil." },
  { "en": "Kaskaya?", "id": "Kaya (arkais)." },
  { "en": "Kasmaran?", "id": "Jatuh cinta." },
  { "en": "Kasmutik?", "id": "Kosmetik (arkais)." },
  { "en": "Kaspe?", "id": "Singkong." },
  { "en": "Kasta?", "id": "Tingkatan sosial." },
  { "en": "Kastanyet?", "id": "Alat musik." },
  { "en": "Kastel?", "id": "Istana benteng." },
  { "en": "Kasti?", "id": "Permainan bola." },
  { "en": "Kastrasi?", "id": "Pengebirian." },
  { "en": "Kastroli?", "id": "Minyak jarak." },
  { "en": "Kasturi?", "id": "Zat wangi." },
  { "en": "Kasual?", "id": "Santai." },
  { "en": "Kasualitas?", "id": "Sebab akibat." },
  { "en": "Kasuarina?", "id": "Cemara." },
  { "en": "Kasuari?", "id": "Burung." },
  { "en": "Kasuis?", "id": "Ahli kasuistik." },
  { "en": "Kasuistik?", "id": "Berdasarkan kasus." },
  { "en": "Kasur?", "id": "Alas tidur." },
  { "en": "Kasus?", "id": "Perkara." },
  { "en": "Kasut?", "id": "Sepatu." },
  { "en": "Kata?", "id": "Ucap." },
  { "en": "Katabalik?", "id": "Gerak turun." },
  { "en": "Katabolisme?", "id": "Penguraian metabolisme." },
  { "en": "Katadrom?", "id": "Migrasi ikan ke laut." },
  { "en": "Katafalk?", "id": "Usungan jenazah." },
  { "en": "Katafil?", "id": "Daun pelindung." },
  { "en": "Katafora?", "id": "Penunjukan ke depan." },
  { "en": "Katak?", "id": "Amfibi." },
  { "en": "Kataka?", "id": "Gelang (Sanskerta)." },
  { "en": "Katakana?", "id": "Aksara Jepang." },
  { "en": "Kataklisme?", "id": "Bencana besar." },
  { "en": "Katakomba?", "id": "Kuburan bawah tanah." },
  { "en": "Katalepsi?", "id": "Kaku tubuh." },
  { "en": "Katalina?", "id": "Pesawat." },
  { "en": "Katalis?", "id": "Pemercepat reaksi." },
  { "en": "Katalisasi?", "id": "Proses katalis." },
  { "en": "Katalisator?", "id": "Katalis." },
  { "en": "Katalog?", "id": "Daftar." },
  { "en": "Katapel?", "id": "Pelontar batu." },
  { "en": "Katar?", "id": "Pilek." },
  { "en": "Katarak?", "id": "Penyakit mata." },
  { "en": "Katarsis?", "id": "Pelepasan emosi." },
  { "en": "Katartik?", "id": "Pencahar." },
  { "en": "Katastrofe?", "id": "Bencana." },
  { "en": "Kate?", "id": "Kerdil." },
  { "en": "Katedral?", "id": "Gereja utama." },
  { "en": "Kategori?", "id": "Golongan." },
  { "en": "Kategoris?", "id": "Tegas." },
  { "en": "Kategorisasi?", "id": "Penggolongan." },
  { "en": "Katek?", "id": "Ketiak (arkais)." },
  { "en": "Katekis?", "id": "Guru agama Kristen." },
  { "en": "Katekisan?", "id": "Pelajaran agama." },
  { "en": "Katekismus?", "id": "Buku pelajaran agama." },
  { "en": "Katel?", "id": "Kutu busuk." },
  { "en": "Katering?", "id": "Jasa boga." },
  { "en": "Kates?", "id": "Pepaya (Jawa)." },
  { "en": "Kati?", "id": "Ukuran berat." },
  { "en": "Katib?", "id": "Penulis (Arab)." },
  { "en": "Katibin?", "id": "Para penulis." },
  { "en": "Katifah?", "id": "Permadani." },
  { "en": "Katik?", "id": "Pendek (kaki)." },
  { "en": "Katil?", "id": "Tempat tidur." },
  { "en": "Katimaha?", "id": "Pohon." },
  { "en": "Kation?", "id": "Ion positif." },
  { "en": "Katir?", "id": "Keseimbangan perahu." },
  { "en": "Katode?", "id": "Kutub negatif." },
  { "en": "Katolik?", "id": "Agama." },
  { "en": "Katrol?", "id": "Kerekan." },
  { "en": "Katuk?", "id": "Sayuran." },
  { "en": "Katun?", "id": "Kapas." },
  { "en": "Katung?", "id": "Penyu." },
  { "en": "Katup?", "id": "Penutup." },
  { "en": "Kau?", "id": "Engkau." },
  { "en": "Kaukab?", "id": "Bintang (Arab)." },
  { "en": "Kaukasoid?", "id": "Ras kulit putih." },
  { "en": "Kaukus?", "id": "Rapat politik." },
  { "en": "Kaul?", "id": "Nazar." },
  { "en": "Kaum?", "id": "Golongan." },
  { "en": "Kaupui?", "id": "Kutu anjing." },
  { "en": "Kaus?", "id": "Baju." },
  { "en": "Kausal?", "id": "Sebab akibat." },
  { "en": "Kausalitas?", "id": "Hubungan sebab akibat." },
  { "en": "Kaustik?", "id": "Membakar (kimia)." },
  { "en": "Kavaleri?", "id": "Pasukan berkuda." },
  { "en": "Kaveling?", "id": "Petak tanah." },
  { "en": "Kaver?", "id": "Sampul (Inggris)." },
  { "en": "Kawah?", "id": "Lubang gunung berapi." },
  { "en": "Kawak?", "id": "Tua." },
  { "en": "Kawal?", "id": "Jaga." },
  { "en": "Kawan?", "id": "Teman." },
  { "en": "Kawan-kawan?", "id": "Teman-teman." },
  { "en": "Kawasan?", "id": "Daerah." },
  { "en": "Kawat?", "id": "Benang logam." },
  { "en": "Kawi?", "id": "Bahasa Jawa Kuno." },
  { "en": "Kawih?", "id": "Lagu Sunda." },
  { "en": "Kawin?", "id": "Nikah." },
  { "en": "Kawista?", "id": "Buah." },
  { "en": "Kawula?", "id": "Hamba." },
  { "en": "Kaya?", "id": "Berharta." },
  { "en": "Kayai?", "id": "Sakit (arkais)." },
  { "en": "Kayak?", "id": "Seperti." },
  { "en": "Kayam?", "id": "Terapung (arkais)." },
  { "en": "Kayambang?", "id": "Tanaman air." },
  { "en": "Kayangan?", "id": "Kahyangan." },
  { "en": "Kayap?", "id": "Penyakit kulit." },
  { "en": "Kayau?", "id": "Penggal kepala." },
  { "en": "Kayu?", "id": "Bahan dari pohon." },
  { "en": "Kayuh?", "id": "Dayung." },
  { "en": "Ke?", "id": "Menuju." },
  { "en": "Keabadian?", "id": "Kekekalan." },
  { "en": "Keadaan?", "id": "Situasi." },
  { "en": "Keagenan?", "id": "Perwakilan." },
  { "en": "Keagamaan?", "id": "Religiusitas." },
  { "en": "Keagungan?", "id": "Kemuliaan." },
  { "en": "Keahlian?", "id": "Keterampilan." },
  { "en": "Keajaiban?", "id": "Keanehan." },
  { "en": "Keakraban?", "id": "Kedekatan." },
  { "en": "Keakuratan?", "id": "Ketepatan." },
  { "en": "Kealaman?", "id": "Naturalitas." },
  { "en": "Kealiman?", "id": "Kesalehan." },
  { "en": "Kealpaan?", "id": "Kelalaian." },
  { "en": "Keamanan?", "id": "Keselamatan." },
  { "en": "Keampuhan?", "id": "Kemanjuran." },
  { "en": "Keanekaragaman?", "id": "Variasi." },
  { "en": "Keanehan?", "id": "Keganjilan." },
  { "en": "Keanggunan?", "id": "Keelokan." },
  { "en": "Keangkuhan?", "id": "Kesombongan." },
  { "en": "Keangkeran?", "id": "Keseraman." },
  { "en": "Keaniayaan?", "id": "Penyiksaan." },
  { "en": "Keapesan?", "id": "Kesialan." },
  { "en": "Kearifan?", "id": "Kebijaksanaan." },
  { "en": "Keartisan?", "id": "Kesenimanan." },
  { "en": "Keasaman?", "id": "Tingkat asam." },
  { "en": "Keasingan?", "id": "Keterasingan." },
  { "en": "Keaslian?", "id": "Orisinalitas." },
  { "en": "Keasyikan?", "id": "Kesenangan." },
  { "en": "Keausan?", "id": "Susut." },
  { "en": "Kebabal?", "id": "Terlalu (Sunda)." },
  { "en": "Kebahagiaan?", "id": "Kesenangan." },
  { "en": "Kebajikan?", "id": "Kebaikan." },
  { "en": "Kebakaran?", "id": "Musibah api." },
  { "en": "Kebal?", "id": "Tahan." },
  { "en": "Kebam?", "id": "Sakit (arkais)." },
  { "en": "Kebanggaan?", "id": "Kemegahan." },
  { "en": "Kebangkitan?", "id": "Kemunculan kembali." },
  { "en": "Kebangsaan?", "id": "Nasionalitas." },
  { "en": "Kebangsawanan?", "id": "Aristokrasi." },
  { "en": "Kebangkrutan?", "id": "Kepailitan." },
  { "en": "Kebanyakan?", "id": "Mayoritas." },
  { "en": "Kebapakan?", "id": "Sifat ayah." },
  { "en": "Kebarat-baratan?", "id": "Keinggris-inggrisan." },
  { "en": "Kebas?", "id": "Mati rasa." },
  { "en": "Kebat?", "id": "Kebut." },
  { "en": "Kebaya?", "id": "Pakaian wanita." },
  { "en": "Kebayan?", "id": "Petugas desa." },
  { "en": "Kebebasan?", "id": "Kemerdekaan." },
  { "en": "Kebekuan?", "id": "Keadaan beku." },
  { "en": "Kebenaran?", "id": "Kenyataan." },
  { "en": "Kebencian?", "id": "Ketidaksukaan." },
  { "en": "Kebendaan?", "id": "Material." },
  { "en": "Kebengisan?", "id": "Kekejaman." },
  { "en": "Keberadaan?", "id": "Eksistensi." },
  { "en": "Keberaksaraan?", "id": "Melek huruf." },
  { "en": "Keberangkatan?", "id": "Embarkasi." },
  { "en": "Keberanian?", "id": "Kegagahan." },
  { "en": "Keberatan?", "id": "Protes." },
  { "en": "Keberhasilan?", "id": "Kesuksesan." },
  { "en": "Kebersamaan?", "id": "Kekompakan." },
  { "en": "Kebersihan?", "id": "Kesterilan." },
  { "en": "Kebesaran?", "id": "Keagungan." },
  { "en": "Kebetulan?", "id": "Insidental." },
  { "en": "Kébeul?", "id": "Kebal (Sunda)." },
  { "en": "Kebiadaban?", "id": "Kebengisan." },
  { "en": "Kebijakan?", "id": "Keputusan." },
  { "en": "Kebijaksanaan?", "id": "Kearifan." },
  { "en": "Kebijaksanaan?", "id": "Kearifan." },
  { "en": "Kebiri?", "id": "Kastrasi." },
  { "en": "Kebisuan?", "id": "Keadaan bisu." },
  { "en": "Keblinger?", "id": "Tersesat (Jawa)." },
  { "en": "Kebobolan?", "id": "Kemasukan." },
  { "en": "Kebocoran?", "id": "Ketirisan." },
  { "en": "Kebodohan?", "id": "Kedunguan." },
  { "en": "Kebohongan?", "id": "Kedustaan." },
  { "en": "Kebolehan?", "id": "Kemampuan." },
  { "en": "Kebun?", "id": "Taman." },
  { "en": "Kebut?", "id": "Cepat." },
  { "en": "Kebutuhan?", "id": "Keperluan." },
  { "en": "Kecabuhan?", "id": "Kekacauan." },
  { "en": "Kecak?", "id": "Tarian Bali." },
  { "en": "Kecaman?", "id": "Kritik." },
  { "en": "Kecambah?", "id": "Tauge." }



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
            }, 4000);
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
