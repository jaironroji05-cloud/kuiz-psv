# kuiz-psv
<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuiz Interaktif PSV Tingkatan 3 (Bab 7 - 11)</title>
    <!-- Tailwind CSS for modern responsive UI -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #f8fafc;
        }
        .option-label {
            transition: all 0.2s ease;
        }
        .option-label:hover {
            border-color: #3b82f6;
            background-color: #eff6ff;
        }
        input[type="radio"]:checked + .option-content {
            border-color: #2563eb;
            background-color: #eff6ff;
            color: #1e40af;
        }
        .correct-answer {
            border-color: #22c55e !important;
            background-color: #f0fdf4 !important;
            color: #15803d !important;
        }
        .wrong-answer {
            border-color: #ef4444 !important;
            background-color: #fef2f2 !important;
            color: #b91c1c !important;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block !important;
            }
            body {
                background: white;
                color: black;
            }
            .shadow-lg, .shadow-md, .shadow-sm {
                box-shadow: none !important;
            }
            .border {
                border-color: #cbd5e1 !important;
            }
        }
    </style>
</head>
<body class="text-slate-800 antialiased min-h-screen pb-16">

    <!-- Sticky Header -->
    <header class="sticky top-0 z-50 bg-white/90 backdrop-blur-md border-b border-slate-200 shadow-sm no-print">
        <div class="max-w-5xl mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="bg-blue-600 text-white p-2.5 rounded-xl shadow-md">
                    <i class="fa-solid fa-palette text-xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg leading-tight text-slate-900">Kuiz PSV Tingkatan 3</h1>
                    <p class="text-xs text-slate-500 font-medium">Tajuk 7 hingga 11 • Bahagian A</p>
                </div>
            </div>

            <!-- Score & Progress -->
            <div class="flex items-center gap-4">
                <div class="hidden sm:block text-right">
                    <div class="text-xs text-slate-500 font-semibold uppercase tracking-wider">Kemajuan</div>
                    <div class="text-sm font-bold text-blue-600" id="progress-text">0 / 42 Soalan</div>
                </div>
                <div class="w-24 sm:w-32 bg-slate-100 rounded-full h-2.5 overflow-hidden border border-slate-200">
                    <div id="progress-bar" class="bg-blue-600 h-2.5 rounded-full transition-all duration-300" style="width: 0%"></div>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 pt-6 space-y-8">

        <!-- Banner Intro & Filter -->
        <div class="bg-gradient-to-r from-blue-700 via-indigo-700 to-purple-800 rounded-3xl p-6 md:p-8 text-white shadow-xl relative overflow-hidden">
            <div class="relative z-10 space-y-3">
                <span class="bg-white/20 text-white text-xs px-3 py-1 rounded-full font-semibold backdrop-blur-sm inline-block">
                    Mod Latihan / Peperiksaan
                </span>
                <h2 class="text-2xl md:text-3xl font-extrabold tracking-tight">
                    Latihan Gabungan PSV (Bab 7 - 11)
                </h2>
                <p class="text-blue-100 text-sm max-w-2xl leading-relaxed">
                    Jawab semua soalan objektif di bawah. Anda boleh semak jawapan serta-merta atau hantar jawapan di akhir kuiz untuk melihat markah keseluruhan dan analisis jawapan.
                </p>
                
                <div class="flex flex-wrap gap-2 pt-2 no-print">
                    <button onclick="scrollToTopic('t7')" class="bg-white/10 hover:bg-white/20 text-white text-xs font-medium px-3 py-1.5 rounded-lg border border-white/20 transition">Tajuk 7: Reka Bentuk Industri</button>
                    <button onclick="scrollToTopic('t8')" class="bg-white/10 hover:bg-white/20 text-white text-xs font-medium px-3 py-1.5 rounded-lg border border-white/20 transition">Tajuk 8: Seni Tekat</button>
                    <button onclick="scrollToTopic('t9')" class="bg-white/10 hover:bg-white/20 text-white text-xs font-medium px-3 py-1.5 rounded-lg border border-white/20 transition">Tajuk 9: Seni Anyaman</button>
                    <button onclick="scrollToTopic('t10')" class="bg-white/10 hover:bg-white/20 text-white text-xs font-medium px-3 py-1.5 rounded-lg border border-white/20 transition">Tajuk 10: Tipografi</button>
                    <button onclick="scrollToTopic('t11')" class="bg-white/10 hover:bg-white/20 text-white text-xs font-medium px-3 py-1.5 rounded-lg border border-white/20 transition">Tajuk 11: Seni Foto</button>
                </div>
            </div>
            <!-- Decorative Circle -->
            <div class="absolute -right-10 -bottom-10 w-48 h-48 bg-white/10 rounded-full blur-2xl pointer-events-none"></div>
        </div>

        <form id="quiz-form" class="space-y-10">
            <!-- Questions dynamically generated or styled in template -->
            <div id="quiz-container" class="space-y-10"></div>

            <!-- Bottom Action Panel -->
            <div class="bg-white p-6 rounded-2xl shadow-lg border border-slate-200 flex flex-col sm:flex-row items-center justify-between gap-4 no-print">
                <button type="button" onclick="resetQuiz()" class="w-full sm:w-auto px-5 py-2.5 text-slate-600 font-semibold bg-slate-100 hover:bg-slate-200 rounded-xl transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-rotate-left"></i> Set Semula
                </button>
                <div class="flex items-center gap-3 w-full sm:w-auto">
                    <button type="button" onclick="window.print()" class="w-full sm:w-auto px-5 py-2.5 text-slate-700 font-semibold bg-slate-100 hover:bg-slate-200 rounded-xl transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-print"></i> Cetak
                    </button>
                    <button type="submit" class="w-full sm:w-auto px-8 py-2.5 text-white font-bold bg-blue-600 hover:bg-blue-700 rounded-xl shadow-lg shadow-blue-500/30 transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-paper-plane"></i> Hantar & Semak
                    </button>
                </div>
            </div>
        </form>

        <!-- Score Result Modal / Card -->
        <div id="score-card" class="hidden bg-white p-8 rounded-3xl shadow-2xl border border-slate-200 text-center space-y-6">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-full bg-blue-100 text-blue-600 mb-2">
                <i class="fa-solid fa-trophy text-3xl"></i>
            </div>
            <div>
                <h3 class="text-2xl font-bold text-slate-900">Keputusan Keputusan Latihan</h3>
                <p class="text-slate-500 text-sm mt-1">Tahniah kerana telah menyelesaikan kuiz ini!</p>
            </div>
            
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 max-w-lg mx-auto">
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <div class="text-xs text-slate-400 font-semibold uppercase">Markah</div>
                    <div id="score-percentage" class="text-2xl font-black text-blue-600">0%</div>
                </div>
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <div class="text-xs text-slate-400 font-semibold uppercase">Betul</div>
                    <div id="score-correct" class="text-2xl font-black text-green-600">0</div>
                </div>
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <div class="text-xs text-slate-400 font-semibold uppercase">Salah</div>
                    <div id="score-wrong" class="text-2xl font-black text-red-600">0</div>
                </div>
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <div class="text-xs text-slate-400 font-semibold uppercase">Jumlah</div>
                    <div id="score-total" class="text-2xl font-black text-slate-700">42</div>
                </div>
            </div>

            <div class="pt-4 border-t border-slate-100 flex justify-center gap-4">
                <button onclick="window.scrollTo({top: 0, behavior: 'smooth'})" class="px-6 py-2.5 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-xl shadow-md transition">
                    Lihat Semakan Jawapan
                </button>
            </div>
        </div>

    </main>

    <script>
        // Complete Question Bank for Tajuk 7 - 11
        const quizData = [
            // TAJUK 7
            {
                id: 't7_q1',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Gambar di bawah ialah lakaran idea satu produk. Antara berikut manakah jawapan yang bersesuaian dengan gambar yang ditunjukkan?',
                svgIllustration: `
                    <div class="w-full h-48 bg-amber-50 rounded-xl border border-amber-200 flex items-center justify-center p-4 relative overflow-hidden">
                        <svg viewBox="0 0 300 150" class="w-full h-full">
                            <rect x="10" y="10" width="280" height="130" fill="none" stroke="#d97706" stroke-width="1.5" stroke-dasharray="4,4"/>
                            <!-- Draft sketches of products -->
                            <path d="M 40 80 Q 70 30 110 80 T 180 80" fill="none" stroke="#475569" stroke-width="2"/>
                            <rect x="50" y="40" width="40" height="25" rx="5" fill="none" stroke="#1e293b" stroke-width="2"/>
                            <circle cx="150" cy="90" r="18" fill="none" stroke="#2563eb" stroke-width="2"/>
                            <polygon points="210,40 250,40 260,80 200,80" fill="none" stroke="#059669" stroke-width="2"/>
                            <path d="M20 120 L280 120" stroke="#cbd5e1" stroke-width="1"/>
                            <text x="200" y="135" font-size="10" fill="#94a3b8" font-weight="bold">LAKARAN IDEA REKA BENTUK</text>
                        </svg>
                    </div>`,
                options: [
                    'A. Memilih warna bentuk perabot',
                    'B. Berkomunikasi dengan pekedai',
                    'C. Runding bersama pelanggan',
                    'D. Menyelesaikan masalah reka bentuk'
                ],
                answer: 3,
                explanation: 'Lakaran idea ialah proses awal pereka bentuk untuk mencetuskan penyelesaian masalah reka bentuk dan merealisasikan fungsi produk.'
            },
            {
                id: 't7_q2',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Reka bentuk kontemporari memiliki ciri-ciri ____________.',
                options: [
                    'A. konsep klasik',
                    'B. gaya futuristik',
                    'C. reka bentuk yang ringkas',
                    'D. warna perabot yang gelap'
                ],
                answer: 2,
                explanation: 'Reka bentuk kontemporari menekankan garis panduan moden yang ringkas, bersih, dan mengutamakan fungsi.'
            },
            {
                id: 't7_q3',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Konsep perabot yang dipengaruhi oleh reka bentuk perabot 100 tahun lalu dikenali sebagai konsep ___________.',
                options: [
                    'A. antik',
                    'B. klasik',
                    'C. moden',
                    'D. futuristik'
                ],
                answer: 1,
                explanation: 'Konsep klasik berasaskan gaya perabot lama atau bersejarah yang pernah popular sekitar 100 tahun yang lalu.'
            },
            {
                id: 't7_q4',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Reka bentuk perabot yang menggunakan gaya futuristik dikenali sebagai konsep ____________.',
                options: [
                    'A. antik',
                    'B. klasik',
                    'C. moden',
                    'D. ultramoden'
                ],
                answer: 3,
                explanation: 'Konsep ultramoden menggunakan reka bentuk berorientasikan futuristik dan ciri-ciri canggih ke depan zaman.'
            },
            {
                id: 't7_q5',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Konsep ultramoden dipengaruhi oleh gaya ____________.',
                options: [
                    'A. futuristik',
                    'B. zaman klasik',
                    'C. ringkas dan berlandaskan perabot 1970-an',
                    'D. reka bentuk perabot yang berusia 100 tahun'
                ],
                answer: 0,
                explanation: 'Konsep ultramoden dipengaruhi secara langsung oleh gaya futuristik.'
            },
            {
                id: 't7_q6',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Apakah jenis konsep yang ditunjukkan pada rekaan perabot di bawah?',
                svgIllustration: `
                    <div class="w-full h-44 bg-slate-100 rounded-xl border border-slate-200 flex items-center justify-center p-3">
                        <svg viewBox="0 0 200 120" class="h-full">
                            <!-- Armchair traditional classic -->
                            <path d="M 50 40 Q 50 10 100 10 Q 150 10 150 40 L 150 70 Q 160 70 160 85 Q 160 95 140 95 L 60 95 Q 40 95 40 85 Q 40 70 50 70 Z" fill="#b45309" stroke="#78350f" stroke-width="2"/>
                            <rect x="60" y="65" width="80" height="25" rx="5" fill="#d97706"/>
                            <path d="M 65 95 L 60 110 M 135 95 L 140 110" stroke="#451a03" stroke-width="4" stroke-linecap="round"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Antik',
                    'B. Klasik',
                    'C. Moden',
                    'D. Ultramarin'
                ],
                answer: 1,
                explanation: 'Kerusi berspan tebal dengan rekaan tradisional berukir tergolong dalam konsep klasik.'
            },
            {
                id: 't7_q7',
                topicId: 't7',
                topicTitle: 'TAJUK 7: REKA BENTUK INDUSTRI',
                question: 'Perkembangan reka bentuk industri dalam sesebuah negara dapat meningkatkan ____________.',
                options: [
                    'A. ekonomi negara',
                    'B. kestabilan politik',
                    'C. kesedaran rakyat',
                    'D. gaya hidup mewah'
                ],
                answer: 0,
                explanation: 'Reka bentuk industri memacu sektor pembuatan dan komersial yang menyumbang kepada peningkatan ekonomi negara.'
            },

            // TAJUK 8
            {
                id: 't8_q1',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Bahan yang terdapat dalam gambar di atas merupakan bahan yang sangat penting dalam penghasilan kraf tekat. Apakah bahan tersebut?',
                svgIllustration: `
                    <div class="w-full h-36 bg-amber-100/50 rounded-xl border border-amber-300 flex items-center justify-center p-2">
                        <svg viewBox="0 0 200 100" class="h-full">
                            <!-- Gold Thread Spool -->
                            <ellipse cx="100" cy="50" rx="60" ry="25" fill="#f59e0b" stroke="#d97706" stroke-width="2"/>
                            <path d="M 40 50 L 40 70 Q 100 95 160 70 L 160 50" fill="#fbbf24" stroke="#d97706" stroke-width="2"/>
                            <line x1="45" y1="55" x2="155" y2="55" stroke="#fef08a" stroke-width="1.5"/>
                            <line x1="48" y1="62" x2="152" y2="62" stroke="#fef08a" stroke-width="1.5"/>
                            <line x1="52" y1="68" x2="148" y2="68" stroke="#fef08a" stroke-width="1.5"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Benang kait',
                    'B. Benang perak',
                    'C. Benang emas',
                    'D. Benang bulu kambing'
                ],
                answer: 2,
                explanation: 'Benang emas merupakan bahan hiasan utama dalam seni tekat timbul.'
            },
            {
                id: 't8_q2',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Berikut merupakan motif seni tekat KECUALI?',
                options: [
                    'A. Motif bunga raya',
                    'B. Motif bunga keladi',
                    'C. Motif bunga kekwa',
                    'D. Motif bunga lawang'
                ],
                answer: 1,
                explanation: 'Motif seni tekat biasanya terdiri daripada flora seperti bunga raya, kekwa, dan bunga lawang. Bunga keladi bukan motif seni tekat utama.'
            },
            {
                id: 't8_q3',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Gambar di atas merupakan bahan yang sangat penting bagi penghasilan tekat bunga timbul. Apakah bahan tersebut?',
                svgIllustration: `
                    <div class="w-full h-36 bg-orange-50 rounded-xl border border-orange-200 flex items-center justify-center p-2">
                        <svg viewBox="0 0 160 90" class="h-full">
                            <!-- Mempulur Pattern -->
                            <path d="M 80 15 C 60 30, 40 20, 30 45 C 40 70, 70 65, 80 80 C 90 65, 120 70, 130 45 C 120 20, 100 30, 80 15 Z" fill="#f97316" stroke="#c2410c" stroke-width="2"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Pulur',
                    'B. Pulut',
                    'C. Mempulur',
                    'D. Motif tekat'
                ],
                answer: 2,
                explanation: 'Mempulur ialah kadbod tebal yang dipotong mengikut corak/motif untuk menghasilkan kesan timbul.'
            },
            {
                id: 't8_q4',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Prinsip asas tekat ialah ____________.',
                options: [
                    'A. proses menjahit benang pada kain dasar',
                    'B. proses merenggangkan kain putih pada pemidang',
                    'C. proses menyusun benang emas dan benang perak',
                    'D. proses menjahit menggunakan benang emas atau perak hingga menutupi mempulur'
                ],
                answer: 3,
                explanation: 'Prinsip asas tekat ialah menjahit benang emas/perak merentasi mempulur hingga menutupinya rapat.'
            },
            {
                id: 't8_q5',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Alat X di atas digunakan untuk melilit benang emas semasa penghasilan proses tekat dijalankan. Apakah nama alat bertanda X di atas?',
                svgIllustration: `
                    <div class="w-full h-36 bg-stone-100 rounded-xl border border-stone-300 flex items-center justify-center p-2">
                        <svg viewBox="0 0 180 90" class="h-full">
                            <!-- Cuban tool -->
                            <path d="M 30 45 L 150 45 M 40 25 L 40 65 M 140 25 L 140 65" stroke="#78350f" stroke-width="6" stroke-linecap="round"/>
                            <ellipse cx="90" cy="45" rx="35" ry="12" fill="#fbbf24" stroke="#d97706" stroke-width="1.5"/>
                            <text x="83" y="49" font-size="12" font-weight="bold" fill="#78350f">X</text>
                        </svg>
                    </div>`,
                options: [
                    'A. Cuban',
                    'B. Kayu tiga segi',
                    'C. Kayu tekat benang',
                    'D. Pelilit benang emas'
                ],
                answer: 0,
                explanation: 'Alat X ialah cuban, iaitu alat bercabang tiga buatan kayu untuk melilit benang emas.'
            },
            {
                id: 't8_q6',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Kain putih digunakan dalam proses kraf tekat. Apakah fungsi kain putih ini?',
                options: [
                    'A. sebagai kain dasar',
                    'B. sebagai kain alas tekatan',
                    'C. bagi memudahkan proses tekat',
                    'D. bagi mencantikkan lagi hasil tekat'
                ],
                answer: 1,
                explanation: 'Kain putih ditempatkan di bawah kain baldu sebagai kain alas/lapik tekatan.'
            },
            {
                id: 't8_q7',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Terdapat dua jenis seni tekat iaitu?',
                options: [
                    'A. Tekat timbul dan tekat ubah',
                    'B. Tekat simpul dan tekat gubah',
                    'C. Tekat timbul dan tekat tenggelam',
                    'D. Tekat gubah dan tekat timbul benang emas'
                ],
                answer: 3,
                explanation: 'Dua jenis seni tekat tradisional di Malaysia ialah Tekat Gubah dan Tekat Timbul Benang Emas.'
            },
            {
                id: 't8_q8',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: '"Menjahit menggunakan benang putih bagi menetapkan kedudukan benang emas secara berulang-ulang sehingga menutup mempulur" - Pernyataan di atas adalah berkaitan ____________.',
                options: [
                    'A. Motif bunga raya',
                    'B. Jenis-jenis seni tekat',
                    'C. Prinsip asas seni tekat',
                    'D. Proses terakhir seni tekat'
                ],
                answer: 2,
                explanation: 'Pernyataan ini menerangkan prinsip asas seni tekat.'
            },
            {
                id: 't8_q9',
                topicId: 't8',
                topicTitle: 'TAJUK 8: SENI TEKAT',
                question: 'Manakah antara berikut ciri-ciri tekatan yang berkualiti?',
                options: [
                    'A. Jarum yang tajam',
                    'B. Kain yang tegang pada pemidang',
                    'C. Mempunyai mempulur yang bercorak bunga',
                    'D. Sulaman benang perak atau benang emas yang kemas'
                ],
                answer: 3,
                explanation: 'Tekatan berkualiti mempunyai susunan sulaman benang emas/perak yang rapat, rata, dan kemas.'
            },

            // TAJUK 9
            {
                id: 't9_q1',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Anyaman ialah satu seni warisan yang menghasilkan hasil seni yang menarik. Anyaman merupakan suatu ____________.',
                options: [
                    'A. proses menjalin secara sehala helaian daun',
                    'B. proses menyusun helaian daun mengikut motif',
                    'C. proses memotong helaian daun menjadi kembaran yang lebih kecil',
                    'D. proses menjalin secara silang menyilang helaian-helaian daun mengkuang, pandan dan sebagainya'
                ],
                answer: 3,
                explanation: 'Definisi anyaman ialah proses menjalin helaian bahan secara silang-menyilang.'
            },
            {
                id: 't9_q2',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Anyaman kelarai mempunyai keistimewaan tersendiri. Apakah maksud kelarai?',
                options: [
                    'A. Susunan anyaman',
                    'B. Corak yang ditampal',
                    'C. Corak silangan anyaman',
                    'D. Corak yang terbentuk semasa proses anyaman'
                ],
                answer: 3,
                explanation: 'Kelarai ialah motif/corak yang terbentuk secara automatik menerusi ragam silangan semasa menganyam.'
            },
            {
                id: 't9_q3',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Apakah jenis daun yang terdapat dalam gambar di atas (pokok berduri di tepi daun)?',
                svgIllustration: `
                    <div class="w-full h-36 bg-emerald-50 rounded-xl border border-emerald-200 flex items-center justify-center p-2">
                        <svg viewBox="0 0 180 90" class="h-full">
                            <!-- Mengkuang plant -->
                            <path d="M 90 85 C 70 50, 20 40, 10 20 C 30 35, 70 55, 88 80 Z" fill="#059669"/>
                            <path d="M 90 85 C 110 50, 160 40, 170 20 C 150 35, 110 55, 92 80 Z" fill="#10b981"/>
                            <path d="M 90 85 L 90 10 L 95 85 Z" fill="#047857"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Daun palas',
                    'B. Daun lalang',
                    'C. Daun pandan',
                    'D. Daun mengkuang'
                ],
                answer: 3,
                explanation: 'Pokok mengkuang mempunyai daun yang panjang, tebal, dan berduri di bahagian tepi serta tulang belakang daun.'
            },
            {
                id: 't9_q4',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Daun mengkuang/pandan dapat menghasilkan pelbagai jenis anyaman. Antara hasil anyaman adalah ____________.',
                options: [
                    'A. raga',
                    'B. lekar',
                    'C. tudung saji',
                    'D. bakul berkat'
                ],
                answer: 2,
                explanation: 'Tudung saji merupakan antara produk utama hasil anyaman daun mengkuang/pandan.'
            },
            {
                id: 't9_q5',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Anyaman Gadas ialah ____________.',
                options: [
                    'A. anyaman yang belum siap',
                    'B. anyaman yang tiada kelarai',
                    'C. anyaman yang mempunyai kelarai tapak harimau',
                    'D. anyaman yang mempunyai motif kelarai yang besar'
                ],
                answer: 1,
                explanation: 'Anyaman gadas dikenali sebagai anyaman biasa tanpa corak kelarai.'
            },
            {
                id: 't9_q6',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Proses melayur daun mengkuang di atas api perlu dilakukan bagi bertujuan ____________.',
                options: [
                    'A. Supaya daun lebih cantik',
                    'B. Agar daun senang dianyam',
                    'C. Supaya daun lebih tahan lama',
                    'D. Agar daun menjadi lebih lembut dan senang dijangka'
                ],
                answer: 3,
                explanation: 'Melayur melembutkan daun supaya helaian fleksibel dan mudah dijangka tanpa pecah/patah.'
            },
            {
                id: 't9_q7',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Apakah fungsi dan kegunaan parang dalam penghasilan seni anyaman?',
                options: [
                    'A. Untuk melurut daun',
                    'B. Untuk membelah helaian daun',
                    'C. Untuk menitik daun untuk menjadi lembut',
                    'D. Untuk memotong dan menetak daun pokok mengkuang'
                ],
                answer: 3,
                explanation: 'Parang digunakan pada peringkat awal untuk menetak/memotong daun dari pokok mengkuang.'
            },
            {
                id: 't9_q8',
                topicId: 't9',
                topicTitle: 'TAJUK 9: SENI ANYAMAN',
                question: 'Waktu manakah yang sesuai melakukan proses menganyam?',
                options: [
                    'A. Pagi',
                    'B. Tengah hari',
                    'C. Pukul 1.00 petang',
                    'D. Sesudah solat Isyak'
                ],
                answer: 0,
                explanation: 'Waktu pagi atau malam adalah waktu paling sesuai kerana keadaan udara lembap menjadikan daun lembut.'
            },

            // TAJUK 10
            {
                id: 't10_q1',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Salah satu kaedah untuk manusia berkomunikasi selain percakapan dan bahasa isyarat ialah melalui ____________.',
                options: [
                    'A. figura',
                    'B. tulisan',
                    'C. anatomi',
                    'D. metafora'
                ],
                answer: 1,
                explanation: 'Tulisan (tipografi) merupakan medium visual penting komunikasi manusia.'
            },
            {
                id: 't10_q2',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Tipografi yang terdapat pada sesebuah poster mestilah ____________.',
                options: [
                    'A. besar',
                    'B. mudah dibaca',
                    'C. menggunakan klasifikasi script',
                    'D. menggunakan warna yang gelap'
                ],
                answer: 1,
                explanation: 'Syarat paling penting tipografi ialah mudah dibaca (legibility).'
            },
            {
                id: 't10_q3',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Perkataan "typo" dalam Bahasa Greek bermaksud ____________.',
                options: [
                    'A. tanda',
                    'B. besar',
                    'C. tinggi',
                    'D. lurus'
                ],
                answer: 0,
                explanation: 'Perkataan typos bermaksud kesan, cap atau tanda.'
            },
            {
                id: 't10_q4',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Apakah istilah yang sering digunakan oleh pereka untuk memudahkan proses klasifikasi rekaan?',
                options: [
                    'A. Serif, slab serif',
                    'B. Type, typeface, font',
                    'C. San Ekspresi, desktop',
                    'D. Ornamentasi, type, serif'
                ],
                answer: 1,
                explanation: 'Istilah Type, Typeface dan Font ialah pengelasan asas tipografi.'
            },
            {
                id: 't10_q5',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Manakah antara berikut merupakan ciri utama huruf jenis Sans Serif?',
                svgIllustration: `
                    <div class="w-full h-32 bg-slate-100 rounded-xl border border-slate-200 flex items-center justify-center gap-12">
                        <div class="text-center">
                            <span class="text-5xl font-serif font-bold text-slate-800">A</span>
                            <p class="text-xs text-slate-500 mt-1">Serif (Garis Hiasan)</p>
                        </div>
                        <div class="text-center">
                            <span class="text-5xl font-sans font-bold text-blue-600">A</span>
                            <p class="text-xs text-blue-600 font-semibold mt-1">Sans Serif (Tanpa Garis)</p>
                        </div>
                    </div>`,
                options: [
                    'A. Mempunyai garisan penamat halus (serif)',
                    'B. Tidak mempunyai garisan hiasan di hujung huruf',
                    'C. Berbentuk seperti tulisan tangan bersambung',
                    'D. Bertema klasik dan antik'
                ],
                answer: 1,
                explanation: 'Sans bermaksud "tanpa". Sans Serif tidak mempunyai garisan hiasan kecil di hujung huruf.'
            },
            {
                id: 't10_q6',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Apakah yang dimaksudkan ciri-ciri tipografi ekspresi?',
                options: [
                    'A. Meniru bunyi semula jadi',
                    'B. Konotasi kasual ataupun formal',
                    'C. Mempunyai tulisan dan hiasan yang indah',
                    'D. Mempunyai imej visual yang mempunyai maksud tersurat'
                ],
                answer: 2,
                explanation: 'Tipografi ekspresi olahan gaya tulisan bergrafik indah yang menyampaikan maksud emosi/mesej.'
            },
            {
                id: 't10_q7',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Antara taip di bawah manakah taip Sans Serif?',
                options: [
                    'A. Meniru bunyi semula jadi',
                    'B. Konotasi kasual ataupun formal',
                    'C. Mempunyai tulisan dan hiasan yang indah',
                    'D. Mempunyai imej visual yang mempunyai maksud tersurat'
                ],
                answer: 1,
                explanation: 'Ciri taip sans serif bersifat moden, bersih dan ringkas sama ada berkonotasi kasual atau formal.'
            },
            {
                id: 't10_q8',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Apakah maksud Onomatopoeia dalam tipografi?',
                svgIllustration: `
                    <div class="w-full h-28 bg-yellow-100 rounded-xl border border-yellow-300 flex items-center justify-center">
                        <span class="text-3xl font-black italic tracking-widest text-red-600 uppercase transform -rotate-3">BOOM!</span>
                    </div>`,
                options: [
                    'A. Kata-kata positif',
                    'B. Saiz rekaan huruf yang sama',
                    'C. Huruf yang berbentuk kasual atau formal',
                    'D. Pembentukan kata-kata yang meniru bunyi'
                ],
                answer: 3,
                explanation: 'Onomatopoeia ialah seni tipografi pembentukan kata yang meniru bunyi persekitaran (cth: BANG!, BOOM!).'
            },
            {
                id: 't10_q9',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Figura taip bermaksud ____________.',
                options: [
                    'A. Figura manusia perempuan',
                    'B. Menghasilkan taip yang disusun membentuk figura',
                    'C. Figura binatang yang dihasilkan dengan teknik catan',
                    'D. Bentuk figura manusia yang dilukis berhampiran tipografi'
                ],
                answer: 1,
                explanation: 'Figura taip ialah gubahan huruf-huruf yang disusun membentuk rupa objek atau figura.'
            },
            {
                id: 't10_q10',
                topicId: 't10',
                topicTitle: 'TAJUK 10: SENI REKA GRAFIK (TIPOGRAFI)',
                question: 'Berdasarkan jenis tipografi gaya kasual dan ceria, apakah maksud tersurat yang sesuai disampaikan?',
                options: [
                    'A. Sedih',
                    'B. Marah',
                    'C. Meletup',
                    'D. Gembira'
                ],
                answer: 3,
                explanation: 'Gaya tulisan kasual dan berliku melambangkan emosi gembira dan ceria.'
            },

            // TAJUK 11
            {
                id: 't11_q1',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Penguasaan dan kemahiran penggunaan kamera kepada pengkarya seni sangat penting bagi menghasilkan ____________.',
                options: [
                    'A. simpanan gambar',
                    'B. untuk rekod kenangan',
                    'C. karya foto yang bermutu',
                    'D. menghasilkan salinan pendua'
                ],
                answer: 2,
                explanation: 'Kemahiran teknikal fotografi membolehkan pengkarya menghasilkan foto yang berkualiti dan bermutu tinggi.'
            },
            {
                id: 't11_q2',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Apakah teknik yang digunakan pada gambar di atas? Objek diletakkan pada garisan pembahagi grid (tidak di tengah).',
                svgIllustration: `
                    <div class="w-full h-36 bg-slate-900 rounded-xl border border-slate-700 flex items-center justify-center p-2 relative">
                        <svg viewBox="0 0 200 100" class="h-full w-full">
                            <!-- Rule of Thirds Grid -->
                            <line x1="66" y1="0" x2="66" y2="100" stroke="#38bdf8" stroke-width="1" stroke-dasharray="2,2"/>
                            <line x1="133" y1="0" x2="133" y2="100" stroke="#38bdf8" stroke-width="1" stroke-dasharray="2,2"/>
                            <line x1="0" y1="33" x2="200" y2="33" stroke="#38bdf8" stroke-width="1" stroke-dasharray="2,2"/>
                            <line x1="0" y1="66" x2="200" y2="66" stroke="#38bdf8" stroke-width="1" stroke-dasharray="2,2"/>
                            <!-- Subject on left intersection -->
                            <circle cx="66" cy="33" r="12" fill="#f43f5e"/>
                            <circle cx="66" cy="33" r="18" fill="none" stroke="#f43f5e" stroke-width="1.5"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Teknik imbangan',
                    'B. Teknik kedalaman',
                    'C. Teknik garisan persegi',
                    'D. Teknik peraturan ketiga'
                ],
                answer: 3,
                explanation: 'Teknik Peraturan Ketiga (Rule of Thirds) membahagikan foto kepada grid 3x3 dan meletakkan subjek pada garisan/titik persilangan.'
            },
            {
                id: 't11_q3',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Gambar menunjukkan rakaman pokok dari pandangan bawah ke atas. Apakah teknik yang diaplikasikan?',
                svgIllustration: `
                    <div class="w-full h-36 bg-sky-900 rounded-xl border border-sky-700 flex items-center justify-center p-2">
                        <svg viewBox="0 0 160 100" class="h-full">
                            <!-- Low angle view trees -->
                            <polygon points="80,10 50,90 110,90" fill="#0284c7" opacity="0.6"/>
                            <polygon points="80,5 30,95 130,95" fill="#0369a1" opacity="0.8"/>
                            <polygon points="80,0 10,100 150,100" fill="#075985"/>
                        </svg>
                    </div>`,
                options: [
                    'A. Teknik imbangan',
                    'B. Teknik segiempat',
                    'C. Teknik kedalaman',
                    'D. Teknik sudut pandangan'
                ],
                answer: 3,
                explanation: 'Merakam dari sudut bawah (low angle) menggunakan teknik Sudut Pandangan.'
            },
            {
                id: 't11_q4',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Tempoh masa bagi cahaya masuk ke dalam kamera dikawal oleh ____________.',
                options: [
                    'A. ISO',
                    'B. lensa',
                    'C. aperture',
                    'D. pengatup'
                ],
                answer: 3,
                explanation: 'Pengatup (shutter speed) mengawal tempoh kelajuan cahaya memasuki penderia kamera.'
            },
            {
                id: 't11_q5',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: '"Fungsinya membolehkan kita melihat imej yang ingin dirakam. Sangat penting untuk mendapat imej foto yang cantik dan memilih kawasan yang menarik" - Pernyataan di atas menerangkan tentang ____________.',
                options: [
                    'A. Kanta',
                    'B. Shutter',
                    'C. Aperture',
                    'D. Skrin paparan (view finder)'
                ],
                answer: 3,
                explanation: 'Viewfinder / Skrin paparan membolehkan jurufoto membingkai dan melihat komposit gambar.'
            },
            {
                id: 't11_q6',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Gambar pantulan bayangan objek yang seimbang kiri dan kanan menggunakan teknik komposisi kreatif ____________.',
                options: [
                    'A. Imbangan',
                    'B. Peraturan ketiga',
                    'C. Sudut pandangan',
                    'D. Garisan membawa'
                ],
                answer: 0,
                explanation: 'Komposisi imbangan simetri mewujudkan keseimbangan visual menerusi pantulan.'
            },
            {
                id: 't11_q7',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Jawapan di bawah adalah BUKAN jenis peranti pengimejan.',
                options: [
                    'A. Handycam',
                    'B. Mesin cetakan',
                    'C. Kamera kompak',
                    'D. Twin-lens Reflex'
                ],
                answer: 1,
                explanation: 'Mesin cetakan ialah peranti pencetak (output), bukan peranti pengimejan perakam foto.'
            },
            {
                id: 't11_q8',
                topicId: 't11',
                topicTitle: 'TAJUK 11: SENI FOTO (KAMERA DAN PERANTI PENGIMEJAN)',
                question: 'Kamera berfungsi sebagai ____________.',
                options: [
                    'A. alat untuk berkongsi cerita',
                    'B. merakam dan merekod gambar',
                    'C. perhiasan dan perkakasan rumah',
                    'D. alat untuk melukis dalam bentuk maya'
                ],
                answer: 1,
                explanation: 'Fungsi asas utama kamera ialah merakam dan merekod imej pegun atau video.'
            }
        ];

        function renderQuiz() {
            const container = document.getElementById('quiz-container');
            let currentTopic = '';
            let html = '';

            quizData.forEach((q, index) => {
                // Section Header when topic changes
                if (q.topicTitle !== currentTopic) {
                    currentTopic = q.topicTitle;
                    html += `
                        <div id="${q.topicId}" class="pt-4">
                            <div class="bg-slate-900 text-white px-5 py-3 rounded-2xl shadow-md flex items-center justify-between">
                                <h3 class="font-bold text-base md:text-lg tracking-wide">${currentTopic}</h3>
                                <span class="bg-blue-600 text-xs px-2.5 py-1 rounded-full font-semibold">Bahagian A</span>
                            </div>
                        </div>`;
                }

                // Question Card
                html += `
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 transition-all hover:shadow-md" id="card-${q.id}">
                        <div class="flex items-start gap-3">
                            <span class="bg-blue-100 text-blue-700 text-xs font-bold px-2.5 py-1 rounded-lg shrink-0 mt-0.5">
                                Soalan ${index + 1}
                            </span>
                            <div class="space-y-3 flex-1">
                                <p class="font-semibold text-slate-800 leading-relaxed">${q.question}</p>
                                
                                ${q.svgIllustration ? q.svgIllustration : ''}

                                <div class="grid grid-cols-1 gap-2.5 pt-2">
                                    ${q.options.map((opt, optIdx) => `
                                        <label class="option-label flex items-start p-3 rounded-xl border border-slate-200 cursor-pointer text-sm font-medium text-slate-700" id="label-${q.id}-${optIdx}">
                                            <input type="radio" name="${q.id}" value="${optIdx}" onchange="updateProgress()" class="hidden">
                                            <div class="option-content w-full flex items-center justify-between">
                                                <span>${opt}</span>
                                                <i class="fa-regular fa-circle text-slate-300 check-icon"></i>
                                            </div>
                                        </label>
                                    `).join('')}
                                </div>

                                <!-- Feedback Box (Hidden initially) -->
                                <div id="feedback-${q.id}" class="hidden p-4 rounded-xl text-xs sm:text-sm font-medium leading-relaxed mt-3 border"></div>
                            </div>
                        </div>
                    </div>`;
            });

            container.innerHTML = html;
            document.getElementById('score-total').innerText = quizData.length;
        }

        function updateProgress() {
            let answered = 0;
            quizData.forEach(q => {
                if (document.querySelector(`input[name="${q.id}"]:checked`)) {
                    answered++;
                }
            });

            const total = quizData.length;
            const percentage = Math.round((answered / total) * 100);
            document.getElementById('progress-text').innerText = `${answered} / ${total} Soalan`;
            document.getElementById('progress-bar').style.width = `${percentage}%`;
        }

        function scrollToTopic(topicId) {
            const el = document.getElementById(topicId);
            if (el) {
                el.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
        }

        document.getElementById('quiz-form').addEventListener('submit', function (e) {
            e.preventDefault();
            let score = 0;

            quizData.forEach(q => {
                const selected = document.querySelector(`input[name="${q.id}"]:checked`);
                const feedbackEl = document.getElementById(`feedback-${q.id}`);
                
                // Reset styling
                q.options.forEach((_, optIdx) => {
                    const label = document.getElementById(`label-${q.id}-${optIdx}`);
                    label.classList.remove('correct-answer', 'wrong-answer');
                });

                if (selected) {
                    const userAns = parseInt(selected.value);
                    if (userAns === q.answer) {
                        score++;
                        document.getElementById(`label-${q.id}-${userAns}`).classList.add('correct-answer');
                        feedbackEl.className = "p-4 rounded-xl text-xs sm:text-sm font-medium mt-3 border bg-emerald-50 border-emerald-200 text-emerald-800 block";
                        feedbackEl.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-600 mr-1.5"></i> <strong>Tepat Sekali!</strong> ${q.explanation}`;
                    } else {
                        document.getElementById(`label-${q.id}-${userAns}`).classList.add('wrong-answer');
                        document.getElementById(`label-${q.id}-${q.answer}`).classList.add('correct-answer');
                        feedbackEl.className = "p-4 rounded-xl text-xs sm:text-sm font-medium mt-3 border bg-rose-50 border-rose-200 text-rose-800 block";
                        feedbackEl.innerHTML = `<i class="fa-solid fa-circle-xmark text-rose-600 mr-1.5"></i> <strong>Kurang Tepat.</strong> Jawapan betul ialah <strong>${q.options[q.answer]}</strong>.<br><span class="mt-1 block text-slate-600">${q.explanation}</span>`;
                    }
                } else {
                    // Unanswered
                    document.getElementById(`label-${q.id}-${q.answer}`).classList.add('correct-answer');
                    feedbackEl.className = "p-4 rounded-xl text-xs sm:text-sm font-medium mt-3 border bg-amber-50 border-amber-200 text-amber-800 block";
                    feedbackEl.innerHTML = `<i class="fa-solid fa-triangle-exclamation text-amber-600 mr-1.5"></i> <strong>Tidak Dijawab.</strong> Jawapan betul ialah <strong>${q.options[q.answer]}</strong>.<br><span class="mt-1 block text-slate-600">${q.explanation}</span>`;
                }
            });

            // Update Summary Card
            const total = quizData.length;
            const percentage = Math.round((score / total) * 100);
            document.getElementById('score-percentage').innerText = `${percentage}%`;
            document.getElementById('score-correct').innerText = score;
            document.getElementById('score-wrong').innerText = total - score;

            const scoreCard = document.getElementById('score-card');
            scoreCard.classList.remove('hidden');
            scoreCard.scrollIntoView({ behavior: 'smooth' });
        });

        function resetQuiz() {
            document.getElementById('quiz-form').reset();
            quizData.forEach(q => {
                q.options.forEach((_, optIdx) => {
                    const label = document.getElementById(`label-${q.id}-${optIdx}`);
                    if (label) label.classList.remove('correct-answer', 'wrong-answer');
                });
                const feedbackEl = document.getElementById(`feedback-${q.id}`);
                if (feedbackEl) {
                    feedbackEl.className = "hidden";
                    feedbackEl.innerHTML = "";
                }
            });
            document.getElementById('score-card').classList.add('hidden');
            updateProgress();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Initialize quiz on load
        window.onload = function() {
            renderQuiz();
        };
    </script>
</body>
</html>
