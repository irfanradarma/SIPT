<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SIPT - Pengawasan Terintegrasi</title>
    
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Lucide Icons for UI icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <style>
        /* Custom styles */
        body {
            font-family: 'Inter', sans-serif;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }
        /* Custom scrollbar for better aesthetics */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #94a3b8; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #64748b; }
        
        /* Main Nav Active State */
        .main-nav-link.active { background-color: #1e293b; }
        /* Sub Nav Active State */
        .sub-nav-link.active { background-color: #334155; color: white; font-weight: 600; }
        
        .table-group-header { background-color: #e2e8f0; font-weight: 600; color: #1e293b; cursor: pointer; }
        /* Modal styles */
        .modal-backdrop { transition: opacity 0.3s ease; }
        .modal-content { transition: transform 0.3s ease, opacity 0.3s ease; }
        /* Accordion icon transition */
        .toggle-icon { transition: transform 0.2s ease-in-out; }
        .sortable:hover { cursor: pointer; background-color: #f1f5f9; }

        /* Collapsible Sidebar styles */
        #sidebar { transition: width 0.3s ease; }
        #main-content { transition: margin-left 0.3s ease; }
        .sidebar-collapsed #sidebar { width: 5rem; /* 80px */ }
        .sidebar-collapsed #main-content { margin-left: 5rem; }
        .sidebar-collapsed .sidebar-text { display: none; }
        .sidebar-collapsed .sidebar-logo-full { display: none; }
        .sidebar-collapsed .sidebar-logo-mini { display: block; }
        .sidebar-collapsed #user-profile-details { display: none; }
        .sidebar-logo-mini { display: none; }
        .sidebar-collapsed .submenu { display: none !important; }
        .sidebar-collapsed .main-nav-link .toggle-icon { display: none; }


        /* Searchable dropdown */
        .searchable-dropdown { position: relative; }
        .searchable-dropdown-options {
            position: absolute;
            background-color: white;
            border: 1px solid #d1d5db;
            border-radius: 0.5rem;
            width: 100%;
            max-height: 150px;
            overflow-y: auto;
            z-index: 10;
            margin-top: 2px;
        }
        .searchable-dropdown-option {
            padding: 8px 12px;
            cursor: pointer;
        }
        .searchable-dropdown-option:hover {
            background-color: #f3f4f6;
        }
        
        /* Chatbot styles */
        .chat-bubble-user {
            background-color: #3b82f6;
            color: white;
            align-self: flex-end;
        }
        .chat-bubble-bot {
            background-color: #e5e7eb;
            color: #1f2937;
            align-self: flex-start;
        }
        
        /* AIS Dashboard Tab styles */
        .tab-button.active {
            border-color: #3b82f6;
            color: #3b82f6;
            font-weight: 600;
        }
    </style>
</head>
<body class="bg-slate-100">

    <!-- Main SIPT Application Wrapper -->
    <div id="app-container" class="flex h-screen">
        <!-- Sidebar Navigation -->
        <aside id="sidebar" class="w-72 bg-slate-800 text-slate-300 flex flex-col flex-shrink-0">
            <div class="p-4 border-b border-slate-700 flex items-center justify-between">
                <div class="sidebar-logo-full">
                    <h1 class="text-xl font-bold text-white">SIPT</h1>
                    <p class="text-xs text-gray-400">Pengawasan Terintegrasi</p>
                </div>
                <div class="sidebar-logo-mini">
                     <h1 class="text-2xl font-bold text-white text-center">S</h1>
                </div>
                <button onclick="toggleSidebar()" class="text-slate-400 hover:text-white">
                    <i data-lucide="menu" class="h-6 w-6"></i>
                </button>
            </div>
            <nav id="main-nav" class="flex-1 px-2 py-4 space-y-1">
                <!-- Main Modules -->
                <div>
                    <a href="#" onclick="toggleSubmenu('ais-submenu')" class="main-nav-link group flex items-center w-full px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                        <i data-lucide="shield-check" class="mr-3 h-5 w-5 flex-shrink-0"></i> 
                        <span class="sidebar-text flex-1 text-left">AIS</span>
                        <i data-lucide="chevron-down" class="ml-auto h-5 w-5 flex-shrink-0 toggle-icon sidebar-text"></i>
                    </a>
                    <div id="ais-submenu" class="pt-1 pl-6 space-y-1 hidden submenu">
                        <a href="#" onclick="showPage('ais_assistant')" class="sub-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                            <i data-lucide="bot" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">AI Assistant</span>
                        </a>
                        <a href="#" onclick="showPage('ais_dashboard')" class="sub-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                            <i data-lucide="layout-dashboard" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">Dashboard</span>
                        </a>
                    </div>
                </div>

                <a href="#" onclick="showPage('pro')" class="main-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                    <i data-lucide="bar-chart-big" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">PRO</span>
                </a>
                
                <!-- CMCA Module with Accordion -->
                <div>
                    <a href="#" onclick="toggleSubmenu('cmca-submenu')" class="main-nav-link group flex items-center w-full px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                        <i data-lucide="siren" class="mr-3 h-5 w-5 flex-shrink-0"></i> 
                        <span class="sidebar-text flex-1 text-left">CMCA</span>
                        <i data-lucide="chevron-down" class="ml-auto h-5 w-5 flex-shrink-0 toggle-icon sidebar-text"></i>
                    </a>
                    <div id="cmca-submenu" class="pt-1 pl-6 space-y-1 hidden submenu">
                        <a href="#" onclick="showPage('anomali_respon')" class="sub-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                            <i data-lucide="search-check" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">Anomali & Respon</span>
                        </a>
                        <a href="#" onclick="showPage('lab_scripts')" class="sub-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                            <i data-lucide="flask-conical" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">CMCA Lab</span>
                        </a>
                    </div>
                </div>

                <a href="#" onclick="showPage('ams')" class="main-nav-link group flex items-center px-3 py-2 text-sm font-medium rounded-md hover:bg-slate-700 hover:text-white">
                    <i data-lucide="folder-kanban" class="mr-3 h-5 w-5 flex-shrink-0"></i> <span class="sidebar-text">AMS</span>
                </a>
            </nav>
            <div class="p-4 border-t border-slate-700">
                <div class="flex items-center mb-3">
                    <img class="h-10 w-10 rounded-full flex-shrink-0" src="https://placehold.co/100x100/E2E8F0/475569?text=U" alt="User Avatar">
                    <div id="user-profile-details" class="ml-3 sidebar-text">
                        <p id="user-name" class="text-sm font-medium text-white">Budi Santoso</p>
                        <p id="user-role-text" class="text-xs text-slate-400">Manajer Lini 1</p>
                    </div>
                </div>
                <div class="sidebar-text">
                    <label for="role-switcher" class="text-xs text-slate-400">Ganti Peran Pengguna:</label>
                    <select id="role-switcher" onchange="changeUserRole(this.value)" class="mt-1 bg-slate-700 border border-slate-600 text-white text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2">
                        <option value="manager" selected>Manajer Lini 1</option>
                        <option value="operator">Operator Lini 1</option>
                        <option value="observer">Observer Lini 2</option>
                        <option value="auditor">Auditor Lini 3</option>
                        <option value="admin_probis">Admin Probis</option>
                        <option value="pengembang_lini_2">Pengembang Lini 2</option>
                        <option value="admin_platform">Admin Platform</option>
                    </select>
                </div>
            </div>
        </aside>

        <!-- Main Content -->
        <main id="main-content" class="flex-1 p-6 overflow-y-auto">
            
            <!-- All Pages -->
            <div id="page-ais_assistant" class="content-page hidden"></div>
            <div id="page-ais_dashboard" class="content-page hidden"></div>
            <div id="page-pro" class="content-page hidden"></div>
            <div id="page-ams" class="content-page hidden"></div>
            <div id="page-anomali_respon" class="content-page hidden"></div>
            <div id="page-lab_scripts" class="content-page hidden"></div>
            <div id="page-add_script" class="content-page hidden"></div>

        </main>
    </div>

    <!-- Anomaly Detail Modal -->
    <div id="anomaly-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4 hidden opacity-0 modal-backdrop">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-7xl max-h-[90vh] flex flex-col modal-content scale-95 opacity-0">
            <!-- Modal Header -->
            <div class="flex justify-between items-center p-4 border-b"><h3 class="text-xl font-bold text-slate-800">Detail Anomali: RISK-001 - Pembayaran Ganda</h3><button onclick="closeAnomalyModal()" class="p-2 rounded-full hover:bg-slate-200"><i data-lucide="x" class="h-6 w-6 text-slate-600"></i></button></div>
            <!-- Modal Body -->
            <div class="p-6 flex-grow overflow-y-auto">
                <!-- Filters -->
                <div class="flex items-center space-x-4 mb-4">
                    <h4 class="font-semibold text-slate-700">Daftar Anomali</h4>
                    <select id="anomaly-filter-status" onchange="filterAndRenderAnomalies()" class="bg-gray-50 border border-gray-300 text-gray-900 text-xs rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2">
                        <option value="all">Semua Status</option>
                        <option value="Diterima">Diterima</option>
                        <option value="Ditolak">Ditolak</option>
                        <option value="Belum Direspon">Belum Direspon</option>
                    </select>
                     <select id="anomaly-filter-type" onchange="filterAndRenderAnomalies()" class="bg-gray-50 border border-gray-300 text-gray-900 text-xs rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2">
                        <option value="all">Semua Tipe</option>
                        <option value="Loss Event">Loss Event</option>
                        <option value="Warning">Warning</option>
                    </select>
                </div>
                <!-- Anomalies Table -->
                <div class="overflow-x-auto border rounded-lg">
                    <table class="w-full text-sm text-left text-slate-500">
                        <thead id="anomaly-table-header" class="text-xs text-slate-700 uppercase bg-slate-50"></thead>
                        <tbody id="anomaly-table-body" class="divide-y divide-slate-200"></tbody>
                    </table>
                </div>
            </div>
             <!-- Modal Footer -->
            <div id="anomaly-modal-footer" class="flex justify-end items-center p-4 border-t bg-slate-50 rounded-b-lg">
                <!-- Footer buttons are rendered dynamically by JS -->
            </div>
        </div>
    </div>
    
    <!-- Script Detail Modal -->
    <div id="script-detail-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4 hidden opacity-0 modal-backdrop">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-4xl max-h-[90vh] flex flex-col modal-content scale-95 opacity-0">
            <!-- Modal Header -->
            <div class="flex justify-between items-center p-4 border-b"><h3 id="script-modal-title" class="text-xl font-bold text-slate-800">Detail Script</h3><button onclick="closeScriptDetailModal()" class="p-2 rounded-full hover:bg-slate-200"><i data-lucide="x" class="h-6 w-6 text-slate-600"></i></button></div>
            <!-- Modal Body -->
            <div id="script-modal-body" class="p-6 flex-grow overflow-y-auto space-y-4"></div>
             <!-- Modal Footer -->
            <div id="script-modal-footer" class="flex justify-between items-center p-4 border-t bg-slate-50 rounded-b-lg"></div>
        </div>
    </div>

    <script>
        // --- PROFILES & MOCK DATA ---
        const userProfiles = {
            manager: { name: 'Budi Santoso', roleText: 'Manajer Lini 1', defaultPage: 'anomali_respon' },
            operator: { name: 'Citra Lestari', roleText: 'Operator Lini 1', defaultPage: 'anomali_respon' },
            observer: { name: 'Rina Hartono', roleText: 'Observer Lini 2', defaultPage: 'anomali_respon' },
            auditor: { name: 'Agus Salim', roleText: 'Auditor Lini 3', defaultPage: 'anomali_respon' },
            admin_probis: { name: 'Probis Admin', roleText: 'Admin Proses Bisnis', defaultPage: 'lab_scripts' },
            pengembang_lini_2: { name: 'Pengembang Lini 2', roleText: 'Pengembang Lini 2', defaultPage: 'lab_scripts' },
            admin_platform: { name: 'Platform Admin', roleText: 'Admin Platform', defaultPage: 'lab_scripts' },
        };
        
        const mockOperators = ['Citra Lestari', 'Adi Nugroho', 'Dewi Anggraini', 'Eko Prasetyo', 'Fitri Handayani'];

        const mockAnomalies = [
            { id: 'LE-00123', detail: 'Invoice #INV123, Vendor ABC, Rp 5.000.000', type: 'Loss Event', status: 'Ditolak', operatorNotes: 'Salah deteksi, transaksi berbeda.', evidence: 'bukti_salah.pdf', l2_eval: 'Setuju', l2_notes: 'Penolakan oleh Lini 1 valid, bukti terlampir.' },
            { id: 'WRN-00456', detail: 'Invoice #INV456, Vendor XYZ, Rp 2.550.000', type: 'Warning', status: 'Diterima', operatorNotes: 'Benar, sudah dilakukan perbaikan.', evidence: 'bukti_perbaikan.pdf', l2_eval: null, l2_notes: '' },
            { id: 'LE-00124', detail: 'Invoice #INV124, Vendor JKL, Rp 1.200.000', type: 'Loss Event', status: 'Belum Direspon', operatorNotes: '', evidence: '', l2_eval: null, l2_notes: '' },
            { id: 'LE-00125', detail: 'Invoice #INV125, Vendor MNO, Rp 7.800.000', type: 'Loss Event', status: 'Ditolak', operatorNotes: 'Transaksi duplikat, namun sudah di-reverse.', evidence: 'bukti_reverse.pdf', l2_eval: null, l2_notes: '' },
        ];
        
        const mockDetectionData = [
            {
                processId: 'PB-01',
                processName: 'Procure to Pay',
                risks: [
                    { id: 'RISK-001', name: 'Pembayaran Ganda', lossEventNew: 3, lossEventDone: 12, warning: 8, rejected: 2, mandatory: true, operator: 'Citra Lestari' },
                    { id: 'RISK-002', name: 'Pembelian Tanpa PO', lossEventNew: 5, lossEventDone: 25, warning: 15, rejected: 4, mandatory: true, operator: 'Adi Nugroho' },
                    { id: 'RISK-003', name: 'Perubahan Data Master Vendor', lossEventNew: 0, lossEventDone: 8, warning: 2, rejected: 0, mandatory: false, operator: 'Dewi Anggraini' },
                ]
            },
            {
                processId: 'PB-02',
                processName: 'Order to Cash',
                risks: [
                    { id: 'RISK-004', name: 'Batas Kredit Pelanggan Dilanggar', lossEventNew: 1, lossEventDone: 30, warning: 5, rejected: 1, mandatory: true, operator: 'Eko Prasetyo' },
                    { id: 'RISK-005', name: 'Pengiriman Barang Tanpa SO', lossEventNew: 0, lossEventDone: 15, warning: 1, rejected: 0, mandatory: false, operator: 'Fitri Handayani' },
                ]
            },
            {
                processId: 'ITGC-01',
                processName: 'IT General Controls',
                risks: [
                    { id: 'RISK-008', name: 'Penyalahgunaan Hak Akses', lossEventNew: 1, lossEventDone: 10, warning: 4, rejected: 0, mandatory: true, operator: 'Citra Lestari' },
                ]
            }
        ];

        const mockScripts = [
            {
                process: 'PB-01: Procure to Pay',
                risks: [
                    {
                        riskName: 'RISK-001: Pembayaran Ganda',
                        scripts: [
                            { id: 'SCR-001', name: 'Cek Duplikasi Invoice v2.1', status: 'Running', lastRun: '29/07/2025 14:30', accuracy: '98.5%', author: 'Admin Platform', criteriaTest: 'Mencari No. Invoice & Nilai yang sama persis.', criteriaWarn: 'No. Invoice sama, nilai beda < 5%.', criteriaLoss: 'No. Invoice & Nilai sama persis.', sql: "SELECT\n  invoice_id, vendor_id, amount, COUNT(*)\nFROM\n  payments\nGROUP BY\n  invoice_id, vendor_id, amount\nHAVING\n  COUNT(*) > 1;" },
                            { id: 'SCR-002', name: 'Cek Kesamaan Vendor & Karyawan', status: 'Stop', lastRun: '20/06/2025 10:00', accuracy: '95.0%', author: 'Admin Probis', criteriaTest: '...', criteriaWarn: '...', criteriaLoss: '...', sql: 'SELECT ...' },
                            { id: 'SCR-005', name: 'Analisa Pola Pembayaran Vendor Baru', status: 'Requested to Run', lastRun: 'N/A', accuracy: 'N/A', author: 'Auditor Lini 3', criteriaTest: '...', criteriaWarn: '...', criteriaLoss: '...', sql: 'SELECT ...' },
                        ]
                    },
                    {
                        riskName: 'RISK-002: Pembelian Tanpa PO',
                        scripts: [
                            { id: 'SCR-003', name: 'Validasi Pembayaran vs PO', status: 'Running', lastRun: '29/07/2025 14:00', accuracy: '99.2%', author: 'Admin Platform', criteriaTest: '...', criteriaWarn: '...', criteriaLoss: '...', sql: 'SELECT ...' },
                            { id: 'SCR-006', name: 'Validasi Pembayaran vs PO v2', status: 'Requested to Stop', lastRun: '29/07/2025 13:00', accuracy: '99.5%', author: 'Admin Probis', criteriaTest: '...', criteriaWarn: '...', criteriaLoss: '...', sql: 'SELECT ...' },
                        ]
                    },
                ]
            },
            {
                process: 'ITGC-01: IT General Controls',
                risks: [
                    {
                        riskName: 'RISK-008: Penyalahgunaan Hak Akses',
                        scripts: [
                            { id: 'SCR-004', name: 'Monitoring Akses Super User', status: 'Requested to Change', lastRun: '29/07/2025 08:00', accuracy: '100%', author: 'Admin Platform', criteriaTest: '...', criteriaWarn: '...', criteriaLoss: '...', sql: 'SELECT ...' },
                        ]
                    }
                ]
            }
        ];
        
        let currentSort = { key: 'id', order: 'asc' };

        // --- PAGE RENDERING ---
        function renderAllPages() {
            renderAisAssistantPage();
            renderAisDashboardPage();
            renderAnomaliResponPage();
            renderLabScriptsPage();
            renderAddScriptPage();
            // Render placeholder pages
            document.getElementById('page-pro').innerHTML = `<h2 class="text-3xl font-bold text-slate-800">Modul PRO</h2><p class="text-slate-600">Konten untuk modul PRO akan ditampilkan di sini.</p>`;
            document.getElementById('page-ams').innerHTML = `<h2 class="text-3xl font-bold text-slate-800">Modul AMS</h2><p class="text-slate-600">Konten untuk modul Audit Management System akan ditampilkan di sini.</p>`;
        }

        function renderAisAssistantPage() {
            const container = document.getElementById('page-ais_assistant');
            container.innerHTML = `
                <div class="h-full flex flex-col bg-white rounded-lg shadow-sm">
                    <div class="p-4 border-b">
                        <h2 class="text-xl font-bold text-slate-800">Assurance Intelligence System</h2>
                    </div>
                    <div id="chat-messages" class="flex-1 p-6 space-y-4 overflow-y-auto flex flex-col">
                        <!-- Chat messages will be appended here -->
                        <div class="p-3 rounded-lg max-w-lg chat-bubble-bot">
                            Selamat datang! Ada yang bisa saya bantu terkait pengawasan?
                        </div>
                    </div>
                    <div class="p-4 border-t bg-slate-50">
                        <div class="relative">
                            <input type="text" id="chat-input" class="w-full p-3 pr-12 text-sm border rounded-lg" placeholder="Ketik pesan Anda di sini..." onkeydown="handleChatInput(event)">
                            <button onclick="sendChatMessage()" class="absolute right-2 top-1/2 -translate-y-1/2 p-2 rounded-full bg-blue-600 text-white hover:bg-blue-700">
                                <i data-lucide="send" class="h-5 w-5"></i>
                            </button>
                        </div>
                    </div>
                </div>`;
        }
        
        function renderAisDashboardPage() {
            const container = document.getElementById('page-ais_dashboard');
            container.innerHTML = `
                <h2 class="text-3xl font-bold text-slate-800 mb-6">Dashboard AIS</h2>
                <div class="border-b border-gray-200">
                    <nav id="ais-tabs" class="-mb-px flex space-x-6" aria-label="Tabs">
                        <button onclick="switchAisTab('topic')" class="tab-button active whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm">Topic Tracking</button>
                        <button onclick="switchAisTab('analisis')" class="tab-button whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 hover:border-gray-300">Analisis Hasil Pengawasan</button>
                        <button onclick="switchAisTab('lainnya')" class="tab-button whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 hover:border-gray-300">Dashboard Lainnya</button>
                    </nav>
                </div>
                <div class="mt-6">
					<!-- Topic Tracking Tab -->
					<div id="tab-content-topic" class="tab-content space-y-6">
                        <!-- Search & Filter Section -->
                        <div class="bg-white p-6 rounded-lg shadow-sm">
                            <h3 class="text-lg font-semibold text-slate-800 mb-4">Pencarian & Tracking Topik Strategis</h3>
                            <div class="flex items-center space-x-4 mb-4">
                                <div class="flex-1">
                                    <input type="text" class="w-full p-3 border rounded-lg" placeholder="Masukkan topik atau keyword untuk tracking (contoh: digitalisasi pajak, APBN 2025)...">
                                </div>
                                <select class="p-3 border rounded-lg">
                                    <option>Semua Sumber</option>
                                    <option>Rapat Pimpinan</option>
                                    <option>Media Massa</option>
                                    <option>Media Sosial</option>
                                    <option>Hasil Pengawasan</option>
                                    <option>Laporan Satker</option>
                                    <option>DPR/MPR</option>
                                </select>
                                <button class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 flex items-center">
                                    <i data-lucide="search" class="mr-2 h-5 w-5"></i>Track
                                </button>
                            </div>
                            <p class="text-sm text-slate-600">
                                <i data-lucide="info" class="inline h-4 w-4 mr-1"></i>
                                Topik yang ditrack akan menjadi dasar pertimbangan dalam perencanaan pengawasan berbasis risiko
                            </p>
                        </div>

                        <!-- Trending Topics -->
                        <div class="bg-white p-6 rounded-lg shadow-sm">
                            <h3 class="text-lg font-semibold text-slate-800 mb-4">Topik Strategis Kemenkeu (30 Hari Terakhir)</h3>
                            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Transformasi Digital Perpajakan</h4>
                                        <span class="text-sm bg-red-100 text-red-800 px-2 py-1 rounded">Critical</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">127 mentions • 8 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-blue-100 text-blue-800 px-2 py-1 rounded mr-1">Rapat Pimpinan</span>
                                        <span class="inline-block bg-green-100 text-green-800 px-2 py-1 rounded">Media</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="trending-up" class="mr-1 h-3 w-3 text-green-600"></i>
                                        +25% dari periode sebelumnya
                                    </div>
                                </div>
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Optimalisasi Penerimaan Negara</h4>
                                        <span class="text-sm bg-yellow-100 text-yellow-800 px-2 py-1 rounded">High</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">89 mentions • 6 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-purple-100 text-purple-800 px-2 py-1 rounded mr-1">DPR</span>
                                        <span class="inline-block bg-orange-100 text-orange-800 px-2 py-1 rounded">Satker</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="trending-up" class="mr-1 h-3 w-3 text-green-600"></i>
                                        +18% dari periode sebelumnya
                                    </div>
                                </div>
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Efisiensi Belanja Negara</h4>
                                        <span class="text-sm bg-orange-100 text-orange-800 px-2 py-1 rounded">Medium</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">54 mentions • 5 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-red-100 text-red-800 px-2 py-1 rounded mr-1">Pengawasan</span>
                                        <span class="inline-block bg-cyan-100 text-cyan-800 px-2 py-1 rounded">Sosmed</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="trending-down" class="mr-1 h-3 w-3 text-red-600"></i>
                                        -5% dari periode sebelumnya
                                    </div>
                                </div>
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Reformasi Birokrasi</h4>
                                        <span class="text-sm bg-green-100 text-green-800 px-2 py-1 rounded">Medium</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">41 mentions • 4 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-indigo-100 text-indigo-800 px-2 py-1 rounded mr-1">Rapat</span>
                                        <span class="inline-block bg-teal-100 text-teal-800 px-2 py-1 rounded">Media</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="minus" class="mr-1 h-3 w-3 text-slate-400"></i>
                                        Stabil dari periode sebelumnya
                                    </div>
                                </div>
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Pengelolaan Aset Negara</h4>
                                        <span class="text-sm bg-blue-100 text-blue-800 px-2 py-1 rounded">Low</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">23 mentions • 3 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-gray-100 text-gray-800 px-2 py-1 rounded mr-1">Satker</span>
                                        <span class="inline-block bg-pink-100 text-pink-800 px-2 py-1 rounded">Pengawasan</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="trending-up" class="mr-1 h-3 w-3 text-green-600"></i>
                                        +12% dari periode sebelumnya
                                    </div>
                                </div>
                                <div class="p-4 border rounded-lg hover:bg-slate-50 cursor-pointer">
                                    <div class="flex items-center justify-between mb-2">
                                        <h4 class="font-medium text-slate-700">Kepatuhan Regulasi Keuangan</h4>
                                        <span class="text-sm bg-yellow-100 text-yellow-800 px-2 py-1 rounded">High</span>
                                    </div>
                                    <p class="text-sm text-slate-600 mb-2">67 mentions • 7 sumber</p>
                                    <div class="text-xs text-slate-500 mb-2">
                                        <span class="inline-block bg-violet-100 text-violet-800 px-2 py-1 rounded mr-1">DPR</span>
                                        <span class="inline-block bg-amber-100 text-amber-800 px-2 py-1 rounded">Pengawasan</span>
                                    </div>
                                    <div class="flex items-center text-xs text-slate-500">
                                        <i data-lucide="trending-up" class="mr-1 h-3 w-3 text-green-600"></i>
                                        +22% dari periode sebelumnya
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Active Tracking -->
                        <div class="bg-white p-6 rounded-lg shadow-sm">
                            <div class="flex justify-between items-center mb-4">
                                <h3 class="text-lg font-semibold text-slate-800">Topik dalam Pengawasan Aktif</h3>
                                <button class="text-sm text-blue-600 hover:text-blue-800 flex items-center">
                                    <i data-lucide="settings" class="mr-1 h-4 w-4"></i>Kelola Alert
                                </button>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="w-full text-sm">
                                    <thead class="text-xs text-slate-700 uppercase bg-slate-50">
                                        <tr>
                                            <th class="px-4 py-3">Topik Strategis</th>
                                            <th class="px-4 py-3">Keywords</th>
                                            <th class="px-4 py-3">Proses Bisnis Terkait</th>
                                            <th class="px-4 py-3">Priority Level</th>
                                            <th class="px-4 py-3">Last Update</th>
                                            <th class="px-4 py-3">Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Digitalisasi Sistem Perpajakan</td>
                                            <td class="px-4 py-3 text-xs">digital tax, e-filing, modernisasi pajak</td>
                                            <td class="px-4 py-3 text-xs">Tax Collection, Taxpayer Services</td>
                                            <td class="px-4 py-3"><span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded">Critical</span></td>
                                            <td class="px-4 py-3">2 jam lalu</td>
                                            <td class="px-4 py-3">
                                                <button class="text-blue-600 hover:text-blue-800 mr-2 text-xs">Detail</button>
                                                <button class="text-red-600 hover:text-red-800 text-xs">Stop</button>
                                            </td>
                                        </tr>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Optimalisasi PNBP</td>
                                            <td class="px-4 py-3 text-xs">PNBP, non-tax revenue, retribusi</td>
                                            <td class="px-4 py-3 text-xs">Revenue Collection, Asset Management</td>
                                            <td class="px-4 py-3"><span class="bg-yellow-100 text-yellow-800 text-xs px-2 py-1 rounded">High</span></td>
                                            <td class="px-4 py-3">6 jam lalu</td>
                                            <td class="px-4 py-3">
                                                <button class="text-blue-600 hover:text-blue-800 mr-2 text-xs">Detail</button>
                                                <button class="text-red-600 hover:text-red-800 text-xs">Stop</button>
                                            </td>
                                        </tr>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Pengelolaan BMN</td>
                                            <td class="px-4 py-3 text-xs">BMN, aset negara, inventarisasi</td>
                                            <td class="px-4 py-3 text-xs">Asset Management, Procurement</td>
                                            <td class="px-4 py-3"><span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded">Medium</span></td>
                                            <td class="px-4 py-3">1 hari lalu</td>
                                            <td class="px-4 py-3">
                                                <button class="text-blue-600 hover:text-blue-800 mr-2 text-xs">Detail</button>
                                                <button class="text-red-600 hover:text-red-800 text-xs">Stop</button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>

                        <!-- Planning Integration -->
                        <div class="bg-blue-50 p-6 rounded-lg border border-blue-200">
                            <div class="flex items-center mb-3">
                                <i data-lucide="lightbulb" class="h-6 w-6 text-blue-600 mr-3"></i>
                                <h4 class="font-semibold text-blue-900">Rekomendasi Perencanaan Pengawasan</h4>
                            </div>
                            <p class="text-sm text-blue-800 mb-3">Berdasarkan analisis topik strategis, berikut area yang direkomendasikan untuk pengawasan mendalam:</p>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div class="bg-white p-3 rounded-lg border">
                                    <h5 class="font-medium text-slate-700 mb-1">Prioritas Tinggi</h5>
                                    <p class="text-xs text-slate-600">Transformasi Digital Perpajakan, Optimalisasi Penerimaan</p>
                                </div>
                                <div class="bg-white p-3 rounded-lg border">
                                    <h5 class="font-medium text-slate-700 mb-1">Prioritas Sedang</h5>
                                    <p class="text-xs text-slate-600">Efisiensi Belanja, Kepatuhan Regulasi</p>
                                </div>
                                <div class="bg-white p-3 rounded-lg border">
                                    <h5 class="font-medium text-slate-700 mb-1">Monitoring</h5>
                                    <p class="text-xs text-slate-600">Pengelolaan Aset, Reformasi Birokrasi</p>
                                </div>
                            </div>
                        </div>
                    </div>


                    <!-- Analisis Hasil Pengawasan Tab -->
                    <div id="tab-content-analisis" class="tab-content hidden space-y-6">
                        <!-- Summary Cards -->
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <div class="flex items-center">
                                    <div class="p-3 bg-blue-100 rounded-full">
                                        <i data-lucide="activity" class="h-6 w-6 text-blue-600"></i>
                                    </div>
                                    <div class="ml-4">
                                        <p class="text-sm text-slate-500">Total Pengawasan</p>
                                        <p class="text-2xl font-bold text-slate-800">1,247</p>
                                        <p class="text-xs text-green-600">+12% bulan ini</p>
                                    </div>
                                </div>
                            </div>
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <div class="flex items-center">
                                    <div class="p-3 bg-green-100 rounded-full">
                                        <i data-lucide="check-circle" class="h-6 w-6 text-green-600"></i>
                                    </div>
                                    <div class="ml-4">
                                        <p class="text-sm text-slate-500">Selesai</p>
                                        <p class="text-2xl font-bold text-slate-800">1,089</p>
                                        <p class="text-xs text-slate-500">87.3% completion</p>
                                    </div>
                                </div>
                            </div>
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <div class="flex items-center">
                                    <div class="p-3 bg-red-100 rounded-full">
                                        <i data-lucide="alert-triangle" class="h-6 w-6 text-red-600"></i>
                                    </div>
                                    <div class="ml-4">
                                        <p class="text-sm text-slate-500">Temuan Kritis</p>
                                        <p class="text-2xl font-bold text-slate-800">23</p>
                                        <p class="text-xs text-red-600">Perlu tindak lanjut</p>
                                    </div>
                                </div>
                            </div>
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <div class="flex items-center">
                                    <div class="p-3 bg-purple-100 rounded-full">
                                        <i data-lucide="clock" class="h-6 w-6 text-purple-600"></i>
                                    </div>
                                    <div class="ml-4">
                                        <p class="text-sm text-slate-500">Rata-rata Waktu</p>
                                        <p class="text-2xl font-bold text-slate-800">4.2</p>
                                        <p class="text-xs text-slate-500">hari per kasus</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Charts Section -->
                        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Tren Temuan Pengawasan</h3>
                                <div class="h-64 bg-slate-50 rounded-lg flex items-center justify-center">
                                    <div class="text-center text-slate-500">
                                        <i data-lucide="bar-chart-3" class="h-12 w-12 mx-auto mb-2"></i>
                                        <p>Chart: Tren temuan 6 bulan terakhir</p>
                                        <p class="text-sm">(Line chart menunjukkan peningkatan/penurunan)</p>
                                    </div>
                                </div>
                            </div>
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Distribusi Risiko</h3>
                                <div class="h-64 bg-slate-50 rounded-lg flex items-center justify-center">
                                    <div class="text-center text-slate-500">
                                        <i data-lucide="pie-chart" class="h-12 w-12 mx-auto mb-2"></i>
                                        <p>Chart: Distribusi risiko berdasarkan kategori</p>
                                        <p class="text-sm">(Pie chart: Operasional, Kredit, Pasar, dll)</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Top Findings Table -->
                        <div class="bg-white p-6 rounded-lg shadow-sm">
                            <div class="flex justify-between items-center mb-4">
                                <h3 class="text-lg font-semibold text-slate-800">Top Findings</h3>
                                <select class="text-sm border rounded-lg px-3 py-2">
                                    <option>Bulan ini</option>
                                    <option>3 bulan terakhir</option>
                                    <option>6 bulan terakhir</option>
                                </select>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="w-full text-sm">
                                    <thead class="text-xs text-slate-700 uppercase bg-slate-50">
                                        <tr>
                                            <th class="px-4 py-3">Risiko</th>
                                            <th class="px-4 py-3">Proses Bisnis</th>
                                            <th class="px-4 py-3">Severity</th>
                                            <th class="px-4 py-3">Frekuensi</th>
                                            <th class="px-4 py-3">Status</th>
                                            <th class="px-4 py-3">Tindak Lanjut</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Pembayaran Ganda</td>
                                            <td class="px-4 py-3">Procure to Pay</td>
                                            <td class="px-4 py-3"><span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded">High</span></td>
                                            <td class="px-4 py-3">15 kasus</td>
                                            <td class="px-4 py-3"><span class="bg-yellow-100 text-yellow-800 text-xs px-2 py-1 rounded">In Progress</span></td>
                                            <td class="px-4 py-3">87%</td>
                                        </tr>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Batas Kredit Dilanggar</td>
                                            <td class="px-4 py-3">Order to Cash</td>
                                            <td class="px-4 py-3"><span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded">Medium</span></td>
                                            <td class="px-4 py-3">8 kasus</td>
                                            <td class="px-4 py-3"><span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded">Resolved</span></td>
                                            <td class="px-4 py-3">100%</td>
                                        </tr>
                                        <tr class="border-b">
                                            <td class="px-4 py-3 font-medium">Penyalahgunaan Akses</td>
                                            <td class="px-4 py-3">IT General Controls</td>
                                            <td class="px-4 py-3"><span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded">High</span></td>
                                            <td class="px-4 py-3">3 kasus</td>
                                            <td class="px-4 py-3"><span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded">Open</span></td>
                                            <td class="px-4 py-3">33%</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- Dashboard Lainnya Tab -->
                    <div id="tab-content-lainnya" class="tab-content hidden space-y-6">
                        <!-- Quick Stats Grid -->
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Performance Metrics</h3>
                                <div class="space-y-3">
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">System Uptime</span>
                                        <span class="font-semibold text-green-600">99.97%</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Response Time</span>
                                        <span class="font-semibold">0.8s</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Active Users</span>
                                        <span class="font-semibold">127</span>
                                    </div>
                                </div>
                            </div>

                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Data Quality</h3>
                                <div class="space-y-3">
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Completeness</span>
                                        <span class="font-semibold text-green-600">94.5%</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Accuracy</span>
                                        <span class="font-semibold text-green-600">97.2%</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Freshness</span>
                                        <span class="font-semibold text-yellow-600">89.1%</span>
                                    </div>
                                </div>
                            </div>

                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Script Execution</h3>
                                <div class="space-y-3">
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Running Scripts</span>
                                        <span class="font-semibold text-blue-600">12</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Success Rate</span>
                                        <span class="font-semibold text-green-600">98.3%</span>
                                    </div>
                                    <div class="flex justify-between items-center">
                                        <span class="text-sm text-slate-600">Avg Runtime</span>
                                        <span class="font-semibold">2.4min</span>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- System Health & Alerts -->
                        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">System Health</h3>
                                <div class="space-y-4">
                                    <div>
                                        <div class="flex justify-between items-center mb-2">
                                            <span class="text-sm text-slate-600">CPU Usage</span>
                                            <span class="text-sm font-semibold">23%</span>
                                        </div>
                                        <div class="w-full bg-slate-200 rounded-full h-2">
                                            <div class="bg-green-600 h-2 rounded-full" style="width: 23%"></div>
                                        </div>
                                    </div>
                                    <div>
                                        <div class="flex justify-between items-center mb-2">
                                            <span class="text-sm text-slate-600">Memory Usage</span>
                                            <span class="text-sm font-semibold">67%</span>
                                        </div>
                                        <div class="w-full bg-slate-200 rounded-full h-2">
                                            <div class="bg-yellow-500 h-2 rounded-full" style="width: 67%"></div>
                                        </div>
                                    </div>
                                    <div>
                                        <div class="flex justify-between items-center mb-2">
                                            <span class="text-sm text-slate-600">Storage Usage</span>
                                            <span class="text-sm font-semibold">45%</span>
                                        </div>
                                        <div class="w-full bg-slate-200 rounded-full h-2">
                                            <div class="bg-blue-600 h-2 rounded-full" style="width: 45%"></div>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="bg-white p-6 rounded-lg shadow-sm">
                                <h3 class="text-lg font-semibold text-slate-800 mb-4">Recent Alerts</h3>
                                <div class="space-y-3">
                                    <div class="flex items-start space-x-3 p-3 bg-red-50 rounded-lg">
                                        <i data-lucide="alert-circle" class="h-5 w-5 text-red-600 mt-0.5"></i>
                                        <div class="flex-1">
                                            <p class="text-sm font-medium text-red-800">Script SCR-001 Failed</p>
                                            <p class="text-xs text-red-600">Connection timeout - 5 min ago</p>
                                        </div>
                                    </div>
                                    <div class="flex items-start space-x-3 p-3 bg-yellow-50 rounded-lg">
                                        <i data-lucide="alert-triangle" class="h-5 w-5 text-yellow-600 mt-0.5"></i>
                                        <div class="flex-1">
                                            <p class="text-sm font-medium text-yellow-800">High Memory Usage</p>
                                            <p class="text-xs text-yellow-600">Server load above 80% - 12 min ago</p>
                                        </div>
                                    </div>
                                    <div class="flex items-start space-x-3 p-3 bg-blue-50 rounded-lg">
                                        <i data-lucide="info" class="h-5 w-5 text-blue-600 mt-0.5"></i>
                                        <div class="flex-1">
                                            <p class="text-sm font-medium text-blue-800">Data Sync Complete</p>
                                            <p class="text-xs text-blue-600">Daily sync finished - 1 hour ago</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- User Activity & Recent Actions -->
                        <div class="bg-white p-6 rounded-lg shadow-sm">
                            <h3 class="text-lg font-semibold text-slate-800 mb-4">User Activity Overview</h3>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                <div>
                                    <h4 class="font-medium text-slate-700 mb-3">Most Active Users (Today)</h4>
                                    <div class="space-y-2">
                                        <div class="flex justify-between items-center">
                                            <span class="text-sm">Citra Lestari</span>
                                            <span class="text-sm font-semibold">47 actions</span>
                                        </div>
                                        <div class="flex justify-between items-center">
                                            <span class="text-sm">Adi Nugroho</span>
                                            <span class="text-sm font-semibold">34 actions</span>
                                        </div>
                                        <div class="flex justify-between items-center">
                                            <span class="text-sm">Dewi Anggraini</span>
                                            <span class="text-sm font-semibold">28 actions</span>
                                        </div>
                                    </div>
                                </div>
                                <div>
                                    <h4 class="font-medium text-slate-700 mb-3">Recent Actions</h4>
                                    <div class="space-y-2 text-sm">
                                        <div class="flex justify-between">
                                            <span>Script approved</span>
                                            <span class="text-slate-500">2 min ago</span>
                                        </div>
                                        <div class="flex justify-between">
                                            <span>Anomaly resolved</span>
                                            <span class="text-slate-500">15 min ago</span>
                                        </div>
                                        <div class="flex justify-between">
                                            <span>New risk detected</span>
                                            <span class="text-slate-500">32 min ago</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>`;
        }
        
        function renderAnomaliResponPage() {
            const container = document.getElementById('page-anomali_respon');
            const currentRole = document.getElementById('role-switcher').value;
            const isManager = currentRole === 'manager';
            
            let managerCols = '';
            if (isManager) {
                managerCols = `<th class="px-6 py-3 text-center">Mandatory</th><th class="px-6 py-3">Assign Operator</th>`;
            }

            let content = `<h2 class="text-3xl font-bold text-slate-800 mb-2">Anomali & Respon</h2>
                <p class="text-slate-600 mb-6">Tinjau dan respon anomali yang terdeteksi pada proses bisnis.</p>
                <!-- Stats Cards -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
                    <div class="bg-white p-6 rounded-lg shadow-sm flex items-center">
                        <div class="p-3 bg-yellow-100 rounded-full"><i data-lucide="alert-circle" class="h-6 w-6 text-yellow-600"></i></div>
                        <div class="ml-4"><p class="text-sm text-slate-500">Anomali Warning</p><p class="text-2xl font-bold text-slate-800">23</p></div>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-sm flex items-center">
                        <div class="p-3 bg-red-100 rounded-full"><i data-lucide="alert-triangle" class="h-6 w-6 text-red-600"></i></div>
                        <div class="ml-4"><p class="text-sm text-slate-500">Loss Event (Belum Respon)</p><p class="text-2xl font-bold text-slate-800">9</p></div>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-sm flex items-center">
                        <div class="p-3 bg-green-100 rounded-full"><i data-lucide="check-circle-2" class="h-6 w-6 text-green-600"></i></div>
                        <div class="ml-4"><p class="text-sm text-slate-500">Loss Event (Sudah Respon)</p><p class="text-2xl font-bold text-slate-800">37</p></div>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-sm flex items-center">
                        <div class="p-3 bg-purple-100 rounded-full"><i data-lucide="shield-x" class="h-6 w-6 text-purple-600"></i></div>
                        <div class="ml-4"><p class="text-sm text-slate-500">Dibatalkan Lini 2</p><p class="text-2xl font-bold text-slate-800">1</p></div>
                    </div>
                </div>
                <!-- Filters and Search -->
                <div class="mb-4 flex items-center space-x-3">
                    <div class="relative flex-grow">
                        <input type="text" id="detection-search" class="w-full p-2 pl-10 text-sm border rounded-lg" placeholder="Cari proses atau risiko...">
                        <i data-lucide="search" class="absolute left-3 top-1/2 -translate-y-1/2 h-5 w-5 text-slate-400"></i>
                    </div>
                     <select id="detection-filter-process" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg p-2">
                        <option value="all">Semua Proses Bisnis</option>
                        ${mockDetectionData.map(p => `<option value="${p.processId}">${p.processName}</option>`).join('')}
                    </select>
                </div>

                <div class="bg-white rounded-lg shadow-sm">
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left text-slate-500">
                            <thead class="text-xs text-slate-700 uppercase bg-slate-50">
                                <tr>
                                    <th class="px-4 py-3 w-20 text-center">
                                        <div class="flex items-center justify-center space-x-1">
                                            <button onclick="toggleAllAccordion('detection-table-body', true)" title="Expand All" class="p-1 rounded hover:bg-slate-200"><i data-lucide="plus-square" class="h-4 w-4"></i></button>
                                            <button onclick="toggleAllAccordion('detection-table-body', false)" title="Collapse All" class="p-1 rounded hover:bg-slate-200"><i data-lucide="minus-square" class="h-4 w-4"></i></button>
                                        </div>
                                    </th>
                                    <th class="px-6 py-3">Risiko</th>
                                    <th class="px-6 py-3 text-center">Loss Event (Belum Respon)</th>
                                    <th class="px-6 py-3 text-center">Loss Event (Sudah Respon)</th>
                                    <th class="px-6 py-3 text-center">Anomali Warning</th>
                                    <th class="px-6 py-3 text-center">Respon Ditolak</th>
                                    ${managerCols}
                                    <th class="px-6 py-3 text-center">Detail</th>
                                </tr>
                            </thead>
                            <tbody id="detection-table-body">
                                <!-- Content generated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>`;
            container.innerHTML = content;
            renderDetectionTableBody(mockDetectionData);
            lucide.createIcons();
        }

        function renderDetectionTableBody(data) {
            const tableBody = document.getElementById('detection-table-body');
            const currentRole = document.getElementById('role-switcher').value;
            const isManager = currentRole === 'manager';
            let content = '';

            data.forEach(process => {
                // Calculate totals for the process
                const totals = process.risks.reduce((acc, risk) => {
                    acc.lossEventNew += risk.lossEventNew;
                    acc.lossEventDone += risk.lossEventDone;
                    acc.warning += risk.warning;
                    acc.rejected += risk.rejected;
                    if (risk.mandatory) acc.mandatoryCount++;
                    return acc;
                }, { lossEventNew: 0, lossEventDone: 0, warning: 0, rejected: 0, mandatoryCount: 0 });

                content += `<tr class="table-group-header" onclick="toggleProcessRisks(this)">
                    <td class="px-6 py-3"><i data-lucide="chevron-down" class="h-5 w-5 toggle-icon rotate-180"></i></td>
                    <td class="py-3 font-bold">${process.processId}: ${process.processName}</td>
                    <td class="py-3 font-bold text-center text-red-600">${totals.lossEventNew}</td>
                    <td class="py-3 font-bold text-center">${totals.lossEventDone}</td>
                    <td class="py-3 font-bold text-center">${totals.warning}</td>
                    <td class="py-3 font-bold text-center">${totals.rejected}</td>
                    ${isManager ? `<td class="py-3 font-bold text-center">${totals.mandatoryCount}/${process.risks.length}</td><td colspan="2"></td>` : '<td></td>'}
                </tr>`;
                process.risks.forEach((risk, index) => {
                    content += `<tr class="risk-row bg-white border-b">
                        <td></td>
                        <td class="px-6 py-4 font-medium text-slate-900">${risk.id}: ${risk.name}</td>
                        <td class="px-6 py-4 text-center font-bold text-red-600">${risk.lossEventNew}</td>
                        <td class="px-6 py-4 text-center">${risk.lossEventDone}</td>
                        <td class="px-6 py-4 text-center">${risk.warning}</td>
                        <td class="px-6 py-4 text-center">${risk.rejected}</td>`;
                    if (isManager) {
                        content += `<td class="px-6 py-4 text-center"><input type="checkbox" class="h-5 w-5 rounded text-blue-600 focus:ring-blue-500" ${risk.mandatory ? 'checked' : ''}></td>
                        <td class="px-6 py-4">
                            <div class="searchable-dropdown" id="dropdown-${risk.id}">
                                <input type="text" value="${risk.operator}" onfocus="showOperatorOptions(this)" onblur="setTimeout(() => hideOperatorOptions(this), 200)" onkeyup="filterOperators(this)" class="w-full p-1.5 text-sm border rounded-md" placeholder="Cari operator...">
                                <div class="searchable-dropdown-options hidden">
                                    ${mockOperators.map(op => `<div class="searchable-dropdown-option" onclick="selectOperator(this, '${risk.id}')">${op}</div>`).join('')}
                                </div>
                            </div>
                        </td>`;
                    }
                    content += `<td class="px-6 py-4 text-center"><button onclick="openAnomalyModal()" class="p-2 rounded-full hover:bg-slate-200"><i data-lucide="search" class="h-5 w-5 text-slate-600"></i></button></td>
                    </tr>`;
                });
            });
            tableBody.innerHTML = content;
            lucide.createIcons();
        }

        function renderLabScriptsPage() {
            const container = document.getElementById('page-lab_scripts');
            const currentRole = document.getElementById('role-switcher').value;
            const canManageScripts = ['admin_probis', 'admin_platform', 'auditor', 'pengembang_lini_2'].includes(currentRole);

            let content = `<div class="flex justify-between items-center mb-6"><div><h2 class="text-3xl font-bold text-slate-800">CMCA Lab</h2><p class="text-slate-600">Browse dan kelola script pengujian.</p></div>`;
            if (canManageScripts) {
                content += `<div><button onclick="showPage('add_script')" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 flex items-center"><i data-lucide="plus" class="mr-2 h-5 w-5"></i> Tambah Script Baru</button></div>`;
            }
            content += `</div>
            <!-- Filters and Search for Lab -->
            <div class="mb-4 flex items-center space-x-3">
                <div class="relative flex-grow">
                    <input type="text" id="lab-search" onkeyup="filterAndRenderScripts()" class="w-full p-2 pl-10 text-sm border rounded-lg" placeholder="Cari proses, risiko, atau script...">
                    <i data-lucide="search" class="absolute left-3 top-1/2 -translate-y-1/2 h-5 w-5 text-slate-400"></i>
                </div>
                <select id="lab-filter-process" onchange="filterAndRenderScripts()" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg p-2">
                    <option value="all">Semua Proses Bisnis</option>
                    ${[...new Set(mockScripts.map(p => p.process))].map(p => `<option value="${p}">${p}</option>`).join('')}
                </select>
                <select id="lab-filter-status" onchange="filterAndRenderScripts()" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg p-2">
                    <option value="all">Semua Status</option>
                    <option value="Running">Running</option>
                    <option value="Stop">Stop</option>
                    <option value="Requested to Run">Request Run</option>
                    <option value="Requested to Stop">Request Stop</option>
                    <option value="Requested to Change">Request Change</option>
                </select>
            </div>
            <div id="lab-scripts-container"></div>
            `;
            container.innerHTML = content;
            filterAndRenderScripts();
            lucide.createIcons();
        }
        
        function filterAndRenderScripts() {
            const container = document.getElementById('lab-scripts-container');
            const currentRole = document.getElementById('role-switcher').value;
            const canManageScripts = ['admin_probis', 'admin_platform', 'auditor', 'pengembang_lini_2'].includes(currentRole);

            const searchTerm = document.getElementById('lab-search').value.toLowerCase();
            const processFilter = document.getElementById('lab-filter-process').value;
            const statusFilter = document.getElementById('lab-filter-status').value;

            let content = '';
            let filteredData = JSON.parse(JSON.stringify(mockScripts));

            if (processFilter !== 'all') {
                filteredData = filteredData.filter(p => p.process === processFilter);
            }

            filteredData = filteredData.map(process => {
                let risks = process.risks.map(risk => {
                    let scripts = risk.scripts.filter(script => {
                        const searchMatch = searchTerm === '' || 
                            process.process.toLowerCase().includes(searchTerm) ||
                            risk.riskName.toLowerCase().includes(searchTerm) ||
                            script.name.toLowerCase().includes(searchTerm);
                        
                        const statusMatch = statusFilter === 'all' || script.status === statusFilter;

                        return searchMatch && statusMatch;
                    });
                    return { ...risk, scripts };
                }).filter(risk => risk.scripts.length > 0);
                return { ...process, risks };
            }).filter(process => process.risks.length > 0);


            filteredData.forEach(process => {
                content += `<div class="mb-6 process-group"><h3 class="text-xl font-bold text-slate-700 p-3 bg-slate-200 rounded-t-lg cursor-pointer flex items-center" onclick="toggleAccordionGroup(this)"><i data-lucide="chevron-down" class="h-5 w-5 mr-2 toggle-icon rotate-180"></i>${process.process}</h3><div class="bg-white rounded-b-lg shadow-sm accordion-content">`;
                process.risks.forEach(risk => {
                    content += `<div class="border-t"><h4 class="text-md font-semibold text-slate-600 p-3 bg-slate-100">${risk.riskName}</h4><div class="overflow-x-auto">`;
                    content += `<table class="w-full text-sm text-left text-slate-500">
                        <thead class="text-xs text-slate-700 uppercase bg-slate-50"><tr>
                            <th class="px-6 py-3">Nama Script</th><th class="px-6 py-3">Status</th><th class="px-6 py-3">Last Run</th><th class="px-6 py-3">Akurasi</th>
                            ${canManageScripts ? '<th class="px-6 py-3 text-center">Detail</th>' : ''}
                        </tr></thead><tbody>`;
                    risk.scripts.forEach(script => {
                        let statusColor, statusText;
                        switch(script.status) {
                            case 'Running': statusColor = 'green'; statusText = 'Running'; break;
                            case 'Stop': statusColor = 'slate'; statusText = 'Stop'; break;
                            case 'Requested to Run': statusColor = 'blue'; statusText = 'Request Run'; break;
                            case 'Requested to Stop': statusColor = 'orange'; statusText = 'Request Stop'; break;
                            case 'Requested to Change': statusColor = 'purple'; statusText = 'Request Change'; break;
                            default: statusColor = 'gray'; statusText = 'Unknown';
                        }
                        content += `<tr class="border-t">
                            <td class="px-6 py-4 font-medium text-slate-900">${script.name}</td>
                            <td class="px-6 py-4"><span class="bg-${statusColor}-100 text-${statusColor}-800 text-xs font-medium px-2.5 py-0.5 rounded-full">${statusText}</span></td>
                            <td class="px-6 py-4">${script.lastRun}</td>
                            <td class="px-6 py-4 font-semibold">${script.accuracy}</td>
                            ${canManageScripts ? `<td class="px-6 py-4 text-center"><button onclick="openScriptDetailModal('${script.id}')" class="p-2 rounded-full hover:bg-slate-200"><i data-lucide="search" class="h-5 w-5 text-slate-600"></i></button></td>` : ''}
                        </tr>`;
                    });
                    content += `</tbody></table></div></div>`;
                });
                content += `</div></div>`;
            });
            container.innerHTML = content;
            lucide.createIcons();
        }

        function renderAddScriptPage() {
            const container = document.getElementById('page-add_script');
            const processOptions = mockDetectionData.map(p => `<option value="${p.processId}">${p.processName}</option>`).join('');
            const riskOptions = mockDetectionData.flatMap(p => p.risks).map(r => `<option value="${r.id}">${r.name}</option>`).join('');
            const authorOptions = ['Admin Platform', 'Admin Probis', 'Auditor Lini 3'].map(a => `<option value="${a}">${a}</option>`).join('');

            container.innerHTML = `
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold text-slate-800">Tambah Script Baru</h2>
                    <div>
                        <button onclick="showPage('lab_scripts')" class="bg-slate-500 text-white px-4 py-2 rounded-lg hover:bg-slate-600">Batal</button>
                        <button class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 ml-2">Simpan Script</button>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <form class="space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label for="script-name" class="block text-sm font-medium text-gray-700">Nama Script</label>
                                <input type="text" id="script-name" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" placeholder="Contoh: Cek Duplikasi Invoice v3.0">
                            </div>
                            <div>
                                <label for="script-author" class="block text-sm font-medium text-gray-700">Penyusun</label>
                                <select id="script-author" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">${authorOptions}</select>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label for="script-process" class="block text-sm font-medium text-gray-700">Proses Bisnis</label>
                                <select id="script-process" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">${processOptions}</select>
                            </div>
                            <div>
                                <label for="script-risk" class="block text-sm font-medium text-gray-700">Risiko Terkait</label>
                                <select id="script-risk" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">${riskOptions}</select>
                            </div>
                        </div>
                        <div>
                            <label for="script-criteria-test" class="block text-sm font-medium text-gray-700">Kriteria Pengujian</label>
                            <textarea id="script-criteria-test" rows="3" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" placeholder="Jelaskan logika umum dari pengujian ini..."></textarea>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label for="script-criteria-warning" class="block text-sm font-medium text-gray-700">Kriteria Warning</label>
                                <textarea id="script-criteria-warning" rows="2" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" placeholder="Kondisi yang memicu status 'Warning'..."></textarea>
                            </div>
                            <div>
                                <label for="script-criteria-loss" class="block text-sm font-medium text-gray-700">Kriteria Loss Event</label>
                                <textarea id="script-criteria-loss" rows="2" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" placeholder="Kondisi yang memicu status 'Loss Event'..."></textarea>
                            </div>
                        </div>
                        <div>
                            <label for="script-sql" class="block text-sm font-medium text-gray-700">SQL Script</label>
                            <textarea id="script-sql" rows="10" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm font-mono" placeholder="SELECT * FROM ..."></textarea>
                        </div>
                    </form>
                </div>
            `;
        }

        // --- CORE LOGIC ---
        function showPage(pageId) {
            document.querySelectorAll('.content-page').forEach(page => page.classList.add('hidden'));
            
            // Handle main module clicks
            const mainModules = ['ais', 'pro', 'ams'];
            if (mainModules.includes(pageId)) {
                // This is a placeholder and will show the first sub-page of the module
                if (pageId === 'ais') {
                   showPage('ais_assistant');
                } else {
                   document.getElementById('page-' + pageId).classList.remove('hidden');
                   setActiveMainMenu(pageId);
                }
                return;
            }

            // Handle sub-menu clicks
            const selectedPage = document.getElementById('page-' + pageId);
            if (selectedPage) selectedPage.classList.remove('hidden');
            
            if (pageId.startsWith('ais_')) {
                setActiveMainMenu('ais');
            } else {
                setActiveMainMenu('cmca');
            }

            document.querySelectorAll('.sub-nav-link').forEach(link => {
                link.classList.remove('active');
                if (link.getAttribute('onclick').includes(`'${pageId}'`)) {
                    link.classList.add('active');
                }
            });
        }
        
        function setActiveMainMenu(module) {
            document.querySelectorAll('.main-nav-link').forEach(link => {
                link.classList.remove('active');
            });
            const cmcaMenu = document.querySelector('a[onclick*="cmca-submenu"]');
            const aisMenu = document.querySelector('a[onclick*="ais-submenu"]');
            
            if (module === 'cmca' || ['anomali_dashboard', 'anomali_respon', 'lab_scripts', 'add_script'].includes(module)) {
                cmcaMenu.classList.add('active');
            } else if (module === 'ais' || pageId.startsWith('ais_')) {
                aisMenu.classList.add('active');
            } else {
                 const activeLink = document.querySelector(`a[onclick*="'${module}'"]`);
                 if(activeLink) activeLink.classList.add('active');
            }
        }
        
        function toggleSubmenu(submenuId) {
            const submenu = document.getElementById(submenuId);
            const icon = submenu.previousElementSibling.querySelector('.toggle-icon');
            submenu.classList.toggle('hidden');
            icon.classList.toggle('rotate-180');
        }

        function changeUserRole(selectedRole) {
            const profile = userProfiles[selectedRole];
            document.getElementById('user-name').textContent = profile.name;
            document.getElementById('user-role-text').textContent = profile.roleText;
            renderAnomaliResponPage();
            renderLabScriptsPage(); 
            showPage(profile.defaultPage);
        }
        
        function toggleSidebar() {
            document.getElementById('app-container').classList.toggle('sidebar-collapsed');
        }
        
        function toggleProcessRisks(rowElement) {
            let nextRow = rowElement.nextElementSibling;
            const icon = rowElement.querySelector('.toggle-icon');
            icon.classList.toggle('rotate-180');
            while(nextRow && nextRow.classList.contains('risk-row')) {
                nextRow.classList.toggle('hidden');
                nextRow = nextRow.nextElementSibling;
            }
        }
        
        function toggleAccordionGroup(headerElement) {
            const content = headerElement.nextElementSibling;
            const icon = headerElement.querySelector('.toggle-icon');
            content.classList.toggle('hidden');
            icon.classList.toggle('rotate-180');
        }
        
        function toggleAllAccordion(containerId, expand) {
            const container = document.getElementById(containerId);
            if (!container) return;

            if (containerId === 'detection-table-body') {
                const headers = container.querySelectorAll('.table-group-header');
                headers.forEach(header => {
                    const icon = header.querySelector('.toggle-icon');
                    let nextRow = header.nextElementSibling;
                    while(nextRow && nextRow.classList.contains('risk-row')) {
                        if (expand) {
                            nextRow.classList.remove('hidden');
                            icon.classList.add('rotate-180');
                        } else {
                            nextRow.classList.add('hidden');
                            icon.classList.remove('rotate-180');
                        }
                        nextRow = nextRow.nextElementSibling;
                    }
                });
            } else if (containerId === 'lab-scripts-container') {
                const groups = container.querySelectorAll('.process-group');
                groups.forEach(group => {
                    const content = group.querySelector('.accordion-content');
                    const icon = group.querySelector('.toggle-icon');
                    if (expand) {
                        content.classList.remove('hidden');
                        icon.classList.add('rotate-180');
                    } else {
                        content.classList.add('hidden');
                        icon.classList.remove('rotate-180');
                    }
                });
            }
        }


        // --- SEARCHABLE DROPDOWN LOGIC ---
        function showOperatorOptions(inputElement) {
            const dropdownOptions = inputElement.nextElementSibling;
            dropdownOptions.classList.remove('hidden');
        }

        function hideOperatorOptions(inputElement) {
            const dropdownOptions = inputElement.nextElementSibling;
            dropdownOptions.classList.add('hidden');
        }

        function filterOperators(inputElement) {
            const filter = inputElement.value.toUpperCase();
            const optionsContainer = inputElement.nextElementSibling;
            const options = optionsContainer.getElementsByClassName('searchable-dropdown-option');
            for (let i = 0; i < options.length; i++) {
                const txtValue = options[i].textContent || options[i].innerText;
                if (txtValue.toUpperCase().indexOf(filter) > -1) {
                    options[i].style.display = "";
                } else {
                    options[i].style.display = "none";
                }
            }
        }

        function selectOperator(optionElement, riskId) {
            const dropdownContainer = optionElement.closest('.searchable-dropdown');
            const inputElement = dropdownContainer.querySelector('input');
            inputElement.value = optionElement.textContent;
            // In a real app, you would save this change.
            console.log(`Risk ${riskId} assigned to ${inputElement.value}`);
        }

        // --- ANOMALY MODAL LOGIC ---
        const anomalyModal = document.getElementById('anomaly-modal');
        const anomalyModalContent = anomalyModal.querySelector('.modal-content');

        function openAnomalyModal() {
            const currentRole = document.getElementById('role-switcher').value;
            filterAndRenderAnomalies();
            renderAnomalyModalFooter(currentRole);
            anomalyModal.classList.remove('hidden');
            requestAnimationFrame(() => {
                anomalyModal.classList.remove('opacity-0');
                anomalyModalContent.classList.remove('opacity-0', 'scale-95');
            });
        }
        
        function renderAnomalyModalFooter(role) {
            const footer = document.getElementById('anomaly-modal-footer');
            let footerHtml = '';

            if (role === 'operator' || role === 'observer') {
                footerHtml = `<button class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700">Simpan Perubahan</button>
                             <button onclick="closeAnomalyModal()" class="bg-slate-500 text-slate-800 px-6 py-2 rounded-lg hover:bg-slate-200 ml-3">Batal</button>`;
            } else {
                footerHtml = `<button onclick="closeAnomalyModal()" class="bg-slate-600 text-white px-6 py-2 rounded-lg hover:bg-slate-700">Tutup</button>`;
            }
            footer.innerHTML = footerHtml;
        }

        function closeAnomalyModal() {
            anomalyModal.classList.add('opacity-0');
            anomalyModalContent.classList.add('opacity-0', 'scale-95');
            setTimeout(() => anomalyModal.classList.add('hidden'), 300);
        }

        function toggleAnomalyDetail(rowElement) {
            const detailRow = rowElement.nextElementSibling;
            const icon = rowElement.querySelector('.toggle-icon');
            if (detailRow) {
                detailRow.classList.toggle('hidden');
                icon.classList.toggle('rotate-180');
            }
        }
        
        function sortAnomalies(key) {
            if (currentSort.key === key) {
                currentSort.order = currentSort.order === 'asc' ? 'desc' : 'asc';
            } else {
                currentSort.key = key;
                currentSort.order = 'asc';
            }
            filterAndRenderAnomalies();
        }

        function filterAndRenderAnomalies() {
            const statusFilter = document.getElementById('anomaly-filter-status').value;
            const typeFilter = document.getElementById('anomaly-filter-type').value;
            
            let filteredData = mockAnomalies.filter(anomaly => {
                const statusMatch = statusFilter === 'all' || anomaly.status === statusFilter;
                const typeMatch = typeFilter === 'all' || anomaly.type === typeFilter;
                return statusMatch && typeMatch;
            });
            
            // Sorting
            filteredData.sort((a, b) => {
                const valA = a[currentSort.key] ? a[currentSort.key].toUpperCase() : '';
                const valB = b[currentSort.key] ? b[currentSort.key].toUpperCase() : '';
                if (valA < valB) return currentSort.order === 'asc' ? -1 : 1;
                if (valA > valB) return currentSort.order === 'asc' ? 1 : -1;
                return 0;
            });

            renderAnomalyTable(document.getElementById('role-switcher').value, filteredData);
        }

        function renderAnomalyTable(role, data) {
            const tableHeader = document.getElementById('anomaly-table-header');
            const tableBody = document.getElementById('anomaly-table-body');
            
            let headers = [
                { key: 'id', label: 'ID Anomali' }, { key: 'detail', label: 'Detail Transaksi' }, { key: 'type', label: 'Tipe' }
            ];
            if (role === 'operator') {
                headers.push({ key: 'konfirmasi', label: 'Konfirmasi', sortable: false }, { key: 'operatorNotes', label: 'Keterangan', sortable: false }, { key: 'evidence', label: 'Bukti', sortable: false });
            } else {
                headers.push({ key: 'status', label: 'Status Respon' }, { key: 'operatorNotes', label: 'Keterangan Operator' }, { key: 'evidence', label: 'Bukti', sortable: false });
            }
            if (role === 'observer' || role === 'auditor') {
                headers.push({ key: 'l2_eval', label: 'Evaluasi Lini 2' });
            }
             if (role === 'observer') {
                headers.push({ key: 'l2_notes', label: 'Keterangan Evaluasi', sortable: false });
            }
             if (role === 'auditor') {
                headers.push({ key: 'l2_notes', label: 'Keterangan Evaluasi' });
            }

            tableHeader.innerHTML = `<tr><th class="px-4 py-3 w-10"></th>${headers.map(h => `<th class="px-4 py-3 ${h.sortable !== false ? 'sortable' : ''}" onclick="sortAnomalies('${h.key}')">${h.label} <i data-lucide="arrow-up-down" class="inline-block h-3 w-3"></i></th>`).join('')}</tr>`;

            let bodyHtml = '';
            data.forEach(anomaly => {
                let rowHtml = `<tr class="bg-white hover:bg-slate-50 cursor-pointer" onclick="toggleAnomalyDetail(this)">
                    <td class="px-4 py-2 text-center"><i data-lucide="chevron-down" class="h-5 w-5 text-slate-400 toggle-icon"></i></td>
                    <td class="px-4 py-2 font-medium">${anomaly.id}</td><td class="px-4 py-2">${anomaly.detail}</td>
                    <td class="px-4 py-2"><span class="bg-${anomaly.type === 'Loss Event' ? 'red' : 'yellow'}-100 text-${anomaly.type === 'Loss Event' ? 'red' : 'yellow'}-800 text-xs font-medium px-2.5 py-0.5 rounded-full">${anomaly.type}</span></td>`;
                
                if (role === 'operator') {
                    rowHtml += `<td class="px-4 py-2"><div class="flex space-x-2"><button class="flex-1 bg-green-100 text-green-800 text-xs py-1 px-2 rounded hover:bg-green-200">Terima</button><button class="flex-1 bg-red-100 text-red-800 text-xs py-1 px-2 rounded hover:bg-red-200">Tolak</button></div></td>
                        <td class="px-4 py-2"><textarea class="w-full border rounded-md p-1 text-xs" rows="1" placeholder="Isi keterangan..."></textarea></td>
                        <td class="px-4 py-2"><input type="file" class="text-xs file:mr-2 file:py-1 file:px-2 file:rounded-full file:border-0 file:text-xs file:bg-slate-100 file:text-slate-700 hover:file:bg-slate-200"></td>`;
                } else {
                    let statusColor = 'slate'; if (anomaly.status === 'Diterima') statusColor = 'green'; if (anomaly.status === 'Ditolak') statusColor = 'red';
                    rowHtml += `<td class="px-4 py-2"><span class="bg-${statusColor}-100 text-${statusColor}-800 text-xs font-medium px-2.5 py-0.5 rounded-full">${anomaly.status}</span></td>
                        <td class="px-4 py-2 text-xs italic text-slate-600">${anomaly.operatorNotes || '-'}</td>
                        <td class="px-4 py-2 text-xs">${anomaly.evidence ? `<a href="#" class="text-blue-600 hover:underline">${anomaly.evidence}</a>` : '-'}</td>`;
                }

                if (role === 'observer' || role === 'auditor') {
                    const isRejected = anomaly.status === 'Ditolak';
                    const disabledAttr = (role === 'observer' && isRejected) ? '' : 'disabled';
                    let evalColor = 'slate'; if (anomaly.l2_eval === 'Setuju') evalColor = 'green'; if (anomaly.l2_eval === 'Dibatalkan') evalColor = 'red';
                    
                    if (role === 'observer') {
                         rowHtml += `<td class="px-4 py-2"><div class="flex space-x-2"><button class="flex-1 bg-green-100 text-green-800 text-xs py-1 px-2 rounded hover:bg-green-200 disabled:opacity-50 disabled:cursor-not-allowed" ${disabledAttr}>Setuju</button><button class="flex-1 bg-red-100 text-red-800 text-xs py-1 px-2 rounded hover:bg-red-200 disabled:opacity-50 disabled:cursor-not-allowed" ${disabledAttr}>Batalkan</button></div></td>
                            <td class="px-4 py-2"><textarea class="w-full border rounded-md p-1 text-xs disabled:opacity-50 disabled:bg-slate-100" rows="1" placeholder="Isi keterangan evaluasi..." ${disabledAttr}></textarea></td>`;
                    } else { // Auditor view is read-only
                        rowHtml += `<td class="px-4 py-2"><span class="bg-${evalColor}-100 text-${evalColor}-800 text-xs font-medium px-2.5 py-0.5 rounded-full">${anomaly.l2_eval || 'Belum Dievaluasi'}</span></td>
                                    <td class="px-4 py-2 text-xs italic text-slate-600">${anomaly.l2_notes || '-'}</td>`;
                    }
                }
                rowHtml += `</tr><tr class="hidden"><td colspan="${headers.length + 1}" class="p-4 bg-slate-50 text-xs"><h5 class="font-bold mb-2">Detail Tambahan untuk ${anomaly.id}:</h5><p>Data detail akan ditampilkan di sini...</p></td></tr>`;
                bodyHtml += rowHtml;
            });
            tableBody.innerHTML = bodyHtml;
            lucide.createIcons();
        }
        
        // --- SCRIPT MODAL LOGIC ---
        const scriptModal = document.getElementById('script-detail-modal');
        const scriptModalContent = scriptModal.querySelector('.modal-content');

        function openScriptDetailModal(scriptId) {
            const scriptData = mockScripts.flatMap(p => p.risks).flatMap(r => r.scripts).find(s => s.id === scriptId);
            if (!scriptData) return;

            document.getElementById('script-modal-title').textContent = `Detail Script: ${scriptData.name}`;
            const body = document.getElementById('script-modal-body');
            body.innerHTML = `
                <div><h4 class="font-semibold text-slate-700">Penyusun</h4><p class="text-sm text-slate-600 bg-slate-50 p-2 rounded-md mt-1">${scriptData.author}</p></div>
                <div><h4 class="font-semibold text-slate-700">Kriteria Pengujian</h4><p class="text-sm text-slate-600 bg-slate-50 p-2 rounded-md mt-1">${scriptData.criteriaTest}</p></div>
                <div><h4 class="font-semibold text-slate-700">Kriteria Warning</h4><p class="text-sm text-slate-600 bg-slate-50 p-2 rounded-md mt-1">${scriptData.criteriaWarn}</p></div>
                <div><h4 class="font-semibold text-slate-700">Kriteria Loss Event</h4><p class="text-sm text-slate-600 bg-slate-50 p-2 rounded-md mt-1">${scriptData.criteriaLoss}</p></div>
                <div><h4 class="font-semibold text-slate-700">SQL Script</h4><pre class="bg-slate-900 text-white p-4 rounded-md text-xs overflow-x-auto"><code>${scriptData.sql}</code></pre></div>
            `;

            const footer = document.getElementById('script-modal-footer');
            const currentRole = document.getElementById('role-switcher').value;
            let leftButtons = '';
            let rightButtons = `<button onclick="closeScriptDetailModal()" class="bg-slate-600 text-white px-6 py-2 rounded-lg hover:bg-slate-700">Tutup</button>`;

            if (currentRole === 'admin_platform') {
                leftButtons += `<button class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">Update Script</button>`;
                if (scriptData.status === 'Requested to Run') {
                    leftButtons += `<button class="ml-2 bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700">Approve Run</button>`;
                }
                if (scriptData.status === 'Requested to Stop') {
                    leftButtons += `<button class="ml-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700">Approve Stop</button>`;
                }
                 if (scriptData.status === 'Requested to Change') {
                    leftButtons += `<button class="ml-2 bg-purple-600 text-white px-4 py-2 rounded-lg hover:bg-purple-700">Review Change</button>`;
                }
            } else if (['admin_probis', 'auditor', 'pengembang_lini_2'].includes(currentRole)) {
                leftButtons += `<button class="bg-orange-500 text-white px-4 py-2 rounded-lg hover:bg-orange-600">Request to Change</button>`;
                if (scriptData.status === 'Running') {
                    leftButtons += `<button class="ml-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700">Request to Stop</button>`;
                } else if (scriptData.status === 'Stop') {
                    leftButtons += `<button class="ml-2 bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700">Request to Run</button>`;
                }
            }
            
            footer.innerHTML = `<div>${leftButtons}</div><div>${rightButtons}</div>`;

            scriptModal.classList.remove('hidden');
            requestAnimationFrame(() => {
                scriptModal.classList.remove('opacity-0');
                scriptModalContent.classList.remove('opacity-0', 'scale-95');
            });
        }

        function closeScriptDetailModal() {
            scriptModal.classList.add('opacity-0');
            scriptModalContent.classList.add('opacity-0', 'scale-95');
            setTimeout(() => scriptModal.classList.add('hidden'), 300);
        }
        
        // --- AIS LOGIC ---
        function handleChatInput(event) {
            if (event.key === 'Enter') {
                sendChatMessage();
            }
        }

        function sendChatMessage() {
            const input = document.getElementById('chat-input');
            const messagesContainer = document.getElementById('chat-messages');
            const messageText = input.value.trim();

            if (messageText === '') return;

            // Append user message
            const userBubble = document.createElement('div');
            userBubble.className = 'p-3 rounded-lg max-w-lg chat-bubble-user';
            userBubble.textContent = messageText;
            messagesContainer.appendChild(userBubble);

            input.value = '';
            messagesContainer.scrollTop = messagesContainer.scrollHeight;

            // Simulate bot response
            setTimeout(() => {
                const botBubble = document.createElement('div');
                botBubble.className = 'p-3 rounded-lg max-w-lg chat-bubble-bot';
                botBubble.textContent = 'terima kasih, ini adalah bot coba-coba';
                messagesContainer.appendChild(botBubble);
                messagesContainer.scrollTop = messagesContainer.scrollHeight;
            }, 1000);
        }
        
        function switchAisTab(tabName) {
            // Hide all content
            document.querySelectorAll('#page-ais_dashboard .tab-content').forEach(el => el.classList.add('hidden'));
            // Deactivate all buttons
            document.querySelectorAll('#ais-tabs .tab-button').forEach(el => el.classList.remove('active'));

            // Show selected content and activate button
            document.getElementById(`tab-content-${tabName}`).classList.remove('hidden');
            document.querySelector(`button[onclick*="'${tabName}'"]`).classList.add('active');
        }


        // --- INITIALIZATION ---
        document.addEventListener('DOMContentLoaded', () => {
            renderAllPages();
            changeUserRole('manager');
            lucide.createIcons();
        });
    </script>

</body>
</html>
