<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Kasir Lengkap + Export Excel</title>
    <!-- ANTI CACHE -->
    <meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
    <meta http-equiv="Pragma" content="no-cache">
    <meta http-equiv="Expires" content="0">
    
    <!-- EXCELJS CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/exceljs/4.4.0/exceljs.min.js"></script>
    <!-- FILE SAVER -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <!-- CHART JS -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        /* RESET DAN BASE STYLES */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Arial, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        /* CONTAINER UTAMA */
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            overflow: hidden;
            min-height: 90vh;
        }
        
        /* HEADER */
        .app-header {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            padding: 25px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo-icon {
            font-size: 36px;
            background: white;
            color: #3498db;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        /* NAVIGASI */
        .nav-tabs {
            display: flex;
            background: #f8f9fa;
            border-bottom: 3px solid #3498db;
            overflow-x: auto;
        }
        
        .nav-tab {
            padding: 15px 25px;
            background: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            color: #555;
            white-space: nowrap;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s;
            border-right: 1px solid #eee;
        }
        
        .nav-tab:hover {
            background: #e3f2fd;
        }
        
        .nav-tab.active {
            background: #3498db;
            color: white;
        }
        
        /* CONTENT AREA */
        .content-area {
            padding: 30px;
            min-height: 70vh;
        }
        
        .tab-content {
            display: none;
            animation: fadeIn 0.5s;
        }
        
        .tab-content.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* ACTION BAR */
        .action-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 2px solid #eee;
        }
        
        .action-buttons {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        /* DASHBOARD */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .stat-card {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            border-left: 5px solid;
            transition: transform 0.3s;
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
        }
        
        .stat-card.sales { border-color: #27ae60; }
        .stat-card.transactions { border-color: #e74c3c; }
        .stat-card.products { border-color: #3498db; }
        .stat-card.stock { border-color: #f39c12; }
        
        .stat-icon {
            font-size: 40px;
            margin-bottom: 15px;
            opacity: 0.8;
        }
        
        .stat-value {
            font-size: 32px;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 5px;
        }
        
        .stat-label {
            color: #7f8c8d;
            font-size: 14px;
        }
        
        /* PRODUK */
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .product-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            border: 1px solid #e0e0e0;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .product-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        
        .product-name {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 16px;
        }
        
        .product-price {
            color: #27ae60;
            font-weight: bold;
            font-size: 18px;
            margin-bottom: 8px;
        }
        
        .product-stock {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 10px;
        }
        
        .stock-badge {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }
        
        .stock-low { background: #ffeaea; color: #e74c3c; }
        .stock-medium { background: #fff4e6; color: #f39c12; }
        .stock-high { background: #e8f6f3; color: #27ae60; }
        
        /* KASIR */
        .kasir-container {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 30px;
        }
        
        .cart-items {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 25px;
            max-height: 500px;
            overflow-y: auto;
        }
        
        .cart-summary {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            border: 2px solid #3498db;
        }
        
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid #eee;
        }
        
        .item-actions {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .qty-btn {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: none;
            background: #3498db;
            color: white;
            cursor: pointer;
            font-size: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        /* EXPORT OPTIONS */
        .export-options {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-top: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 10px;
        }
        
        .export-option {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 15px;
            background: white;
            border-radius: 10px;
            min-width: 120px;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid transparent;
        }
        
        .export-option:hover {
            border-color: #3498db;
            transform: translateY(-3px);
        }
        
        .export-icon {
            font-size: 32px;
            margin-bottom: 10px;
        }
        
        .export-label {
            font-size: 12px;
            text-align: center;
            color: #2c3e50;
            font-weight: 600;
        }
        
        /* FORM */
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-label {
            display: block;
            margin-bottom: 8px;
            color: #2c3e50;
            font-weight: 500;
        }
        
        .form-input {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border 0.3s;
        }
        
        .form-input:focus {
            outline: none;
            border-color: #3498db;
        }
        
        /* TOMBOL */
        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 16px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s;
        }
        
        .btn-primary {
            background: #3498db;
            color: white;
        }
        
        .btn-primary:hover {
            background: #2980b9;
            transform: translateY(-2px);
        }
        
        .btn-success {
            background: #27ae60;
            color: white;
        }
        
        .btn-success:hover {
            background: #219653;
        }
        
        .btn-danger {
            background: #e74c3c;
            color: white;
        }
        
        .btn-warning {
            background: #f39c12;
            color: white;
        }
        
        .btn-excel {
            background: #217346;
            color: white;
        }
        
        .btn-excel:hover {
            background: #1a5c38;
        }
        
        .btn-pdf {
            background: #e74c3c;
            color: white;
        }
        
        .btn-lg {
            padding: 15px 30px;
            font-size: 18px;
        }
        
        .btn-block {
            width: 100%;
            justify-content: center;
        }
        
        /* TABEL */
        .data-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
        }
        
        .data-table th {
            background: #2c3e50;
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: 600;
        }
        
        .data-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #eee;
        }
        
        .data-table tr:hover {
            background: #f8f9fa;
        }
        
        /* MODAL */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .modal.active {
            display: flex;
        }
        
        .modal-content {
            background: white;
            border-radius: 15px;
            width: 90%;
            max-width: 600px;
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .modal-header {
            padding: 20px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .modal-body {
            padding: 20px;
        }
        
        .modal-footer {
            padding: 15px 20px;
            border-top: 1px solid #eee;
            text-align: right;
        }
        
        .close-btn {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #7f8c8d;
        }
        
        /* UTILITIES */
        .text-center { text-align: center; }
        .text-right { text-align: right; }
        .mb-20 { margin-bottom: 20px; }
        .mt-20 { margin-top: 20px; }
        .p-20 { padding: 20px; }
        
        /* LOADING */
        .loading {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255,255,255,0.9);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: #3498db;
        }
        
        .loading.active {
            display: flex;
        }
        
        /* ALERT */
        .alert {
            padding: 15px 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: none;
        }
        
        .alert-success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .alert-error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .alert.active {
            display: block;
            animation: slideIn 0.3s;
        }
        
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        /* RESPONSIVE */
        @media (max-width: 768px) {
            .kasir-container {
                grid-template-columns: 1fr;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .products-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            .nav-tabs {
                flex-wrap: wrap;
            }
            
            .nav-tab {
                flex: 1;
                min-width: 120px;
                justify-content: center;
            }
            
            .action-bar {
                flex-direction: column;
                gap: 15px;
                align-items: stretch;
            }
        }
        
        /* PRINT STRUK */
        @media print {
            .navbar, .no-print {
                display: none !important;
            }
            
            .section {
                display: none !important;
            }
            
            #printArea, #printArea * {
                display: block !important;
            }
        }
    </style>
</head>
<body>
    <!-- LOADING OVERLAY -->
    <div id="loading" class="loading">
        <div class="text-center">
            <div style="font-size: 48px; margin-bottom: 20px;">⏳</div>
            <div>Memuat aplikasi...</div>
        </div>
    </div>

    <!-- ALERT MESSAGES -->
    <div id="alertSuccess" class="alert alert-success"></div>
    <div id="alertError" class="alert alert-error"></div>

    <!-- MODAL EXPORT OPTIONS -->
    <div id="exportModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>📊 Export Data</h3>
                <button class="close-btn" onclick="closeExportModal()">&times;</button>
            </div>
            <div class="modal-body">
                <div class="export-options">
                    <div class="export-option" onclick="exportToExcel('penjualan')">
                        <div class="export-icon">💰</div>
                        <div class="export-label">Laporan Penjualan</div>
                    </div>
                    
                    <div class="export-option" onclick="exportToExcel('produk')">
                        <div class="export-icon">📦</div>
                        <div class="export-label">Data Produk</div>
                    </div>
                    
                    <div class="export-option" onclick="exportToExcel('stok')">
                        <div class="export-icon">📊</div>
                        <div class="export-label">Laporan Stok</div>
                    </div>
                    
                    <div class="export-option" onclick="exportToExcel('transaksi')">
                        <div class="export-icon">🧾</div>
                        <div class="export-label">Transaksi Harian</div>
                    </div>
                    
                    <div class="export-option" onclick="exportToExcel('custom')">
                        <div class="export-icon">📅</div>
                        <div class="export-label">Custom Periode</div>
                    </div>
                    
                    <div class="export-option" onclick="exportToPDF()">
                        <div class="export-icon">📄</div>
                        <div class="export-label">Export PDF</div>
                    </div>
                </div>
                
                <!-- CUSTOM PERIODE -->
                <div id="customExportOptions" style="display: none; margin-top: 20px;">
                    <h4>Custom Periode Export</h4>
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 15px;">
                        <div class="form-group">
                            <label class="form-label">Dari Tanggal</label>
                            <input type="date" id="exportStartDate" class="form-input">
                        </div>
                        <div class="form-group">
                            <label class="form-label">Sampai Tanggal</label>
                            <input type="date" id="exportEndDate" class="form-input">
                        </div>
                    </div>
                    <button class="btn btn-excel btn-block" onclick="exportCustomPeriod()">
                        📥 Export Custom Periode
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- APLIKASI KASIR -->
    <div class="app-container">
        <!-- HEADER -->
        <header class="app-header">
            <div class="logo">
                <div class="logo-icon">💰</div>
                <div>
                    <h1 style="font-size: 28px; margin-bottom: 5px;">KASIR PRO</h1>
                    <p style="opacity: 0.9; font-size: 14px;">Sistem Kasir dengan Export Excel</p>
                </div>
            </div>
            <div style="text-align: right;">
                <div id="currentDate" style="font-size: 16px; margin-bottom: 5px;"></div>
                <div id="currentTime" style="font-size: 14px; opacity: 0.9;"></div>
            </div>
        </header>

        <!-- NAVIGASI -->
        <nav class="nav-tabs">
            <button class="nav-tab active" onclick="showTab('dashboard')">
                📊 Dashboard
            </button>
            <button class="nav-tab" onclick="showTab('kasir')">
                💼 Kasir
            </button>
            <button class="nav-tab" onclick="showTab('produk')">
                📦 Produk & Stok
            </button>
            <button class="nav-tab" onclick="showTab('laporan')">
                📈 Laporan
            </button>
            <button class="nav-tab" onclick="showTab('transaksi')">
                🧾 Riwayat
            </button>
            <button class="nav-tab" onclick="showTab('export')">
                📥 Export Data
            </button>
        </nav>

        <!-- CONTENT AREA -->
        <main class="content-area">
            <!-- DASHBOARD -->
            <div id="dashboard" class="tab-content active">
                <div class="action-bar">
                    <h2>📊 Dashboard Penjualan</h2>
                    <div class="action-buttons">
                        <button class="btn btn-excel" onclick="exportDashboardExcel()">
                            📊 Export Dashboard
                        </button>
                        <button class="btn btn-primary" onclick="refreshDashboard()">
                            🔄 Refresh
                        </button>
                    </div>
                </div>
                
                <div class="stats-grid">
                    <div class="stat-card sales">
                        <div class="stat-icon">💰</div>
                        <div class="stat-value" id="totalSales">Rp 0</div>
                        <div class="stat-label">Total Penjualan Hari Ini</div>
                    </div>
                    
                    <div class="stat-card transactions">
                        <div class="stat-icon">🧾</div>
                        <div class="stat-value" id="totalTransactions">0</div>
                        <div class="stat-label">Total Transaksi</div>
                    </div>
                    
                    <div class="stat-card products">
                        <div class="stat-icon">📦</div>
                        <div class="stat-value" id="totalProducts">0</div>
                        <div class="stat-label">Total Produk</div>
                    </div>
                    
                    <div class="stat-card stock">
                        <div class="stat-icon">⚠️</div>
                        <div class="stat-value" id="lowStockProducts">0</div>
                        <div class="stat-label">Stok Hampir Habis</div>
                    </div>
                </div>
                
                <div style="display: grid; grid-template-columns: 2fr 1fr; gap: 30px; margin-top: 30px;">
                    <div style="background: white; border-radius: 15px; padding: 25px;">
                        <h3>📈 Grafik Penjualan 7 Hari Terakhir</h3>
                        <canvas id="salesChart" style="width: 100%; height: 300px;"></canvas>
                    </div>
                    
                    <div style="background: white; border-radius: 15px; padding: 25px;">
                        <h3>🏆 Top 5 Produk Terlaris</h3>
                        <div id="topProducts" style="margin-top: 15px;">
                            <!-- Top products will be filled by JS -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- KASIR -->
            <div id="kasir" class="tab-content">
                <div class="action-bar">
                    <h2>💼 Kasir Point of Sale</h2>
                    <div class="action-buttons">
                        <button class="btn btn-success" onclick="processPayment()">
                            💳 Proses Pembayaran
                        </button>
                        <button class="btn btn-danger" onclick="clearCart()">
                            🗑️ Kosongkan
                        </button>
                    </div>
                </div>
                
                <div class="kasir-container">
                    <!-- KERANJANG -->
                    <div class="cart-items">
                        <h3>🛒 Keranjang Belanja</h3>
                        <div id="cartContainer" style="margin-top: 20px;">
                            <div class="text-center p-20" style="color: #7f8c8d;">
                                <div style="font-size: 60px; margin-bottom: 20px;">🛒</div>
                                <h4>Keranjang Kosong</h4>
                                <p>Pilih produk dari daftar di bawah</p>
                            </div>
                        </div>
                        
                        <div id="cartSummary" style="margin-top: 20px; display: none;">
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                                <span>Subtotal:</span>
                                <span id="subtotal">Rp 0</span>
                            </div>
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                                <span>Pajak (10%):</span>
                                <span id="tax">Rp 0</span>
                            </div>
                            <div style="display: flex; justify-content: space-between; font-size: 20px; font-weight: bold; padding-top: 15px; border-top: 2px solid #3498db;">
                                <span>TOTAL:</span>
                                <span id="total">Rp 0</span>
                            </div>
                        </div>
                    </div>
                    
                    <!-- PROSES PEMBAYARAN -->
                    <div class="cart-summary">
                        <h3>💰 Pembayaran</h3>
                        
                        <div class="form-group">
                            <label class="form-label">Jumlah Bayar</label>
                            <input type="number" id="paymentAmount" class="form-input" placeholder="Masukkan jumlah bayar">
                        </div>
                        
                        <div id="changeContainer" style="display: none;">
                            <div style="background: #e8f6f3; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                                <div style="display: flex; justify-content: space-between;">
                                    <span>Kembalian:</span>
                                    <span id="changeAmount" style="font-weight: bold; color: #27ae60;">Rp 0</span>
                                </div>
                            </div>
                        </div>
                        
                        <div class="btn-group" style="display: grid; gap: 10px;">
                            <button class="btn btn-success btn-lg" onclick="processPayment()">
                                💳 Proses Pembayaran
                            </button>
                            <button class="btn btn-primary" onclick="printReceipt()">
                                🖨️ Cetak Struk
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- DAFTAR PRODUK -->
                <h3 class="mt-20 mb-20">📦 Daftar Produk</h3>
                <div class="products-grid" id="productGridKasir">
                    <!-- Produk akan diisi oleh JavaScript -->
                </div>
            </div>

            <!-- PRODUK & STOK -->
            <div id="produk" class="tab-content">
                <div class="action-bar">
                    <div>
                        <h2>📦 Manajemen Produk & Stok</h2>
                        <p style="color: #7f8c8d; font-size: 14px; margin-top: 5px;">Total: <span id="totalProductCount">0</span> produk</p>
                    </div>
                    <div class="action-buttons">
                        <button class="btn btn-success" onclick="showAddProductModal()">
                            ➕ Tambah Produk
                        </button>
                        <button class="btn btn-excel" onclick="exportProductExcel()">
                            📥 Export Produk
                        </button>
                    </div>
                </div>
                
                <!-- FILTER -->
                <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
                    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px;">
                        <div>
                            <label class="form-label">Cari Produk</label>
                            <input type="text" id="searchProduct" class="form-input" placeholder="Nama produk..." oninput="filterProducts()">
                        </div>
                        <div>
                            <label class="form-label">Kategori</label>
                            <select id="filterCategory" class="form-input" onchange="filterProducts()">
                                <option value="">Semua Kategori</option>
                                <option value="Sembako">Sembako</option>
                                <option value="Minuman">Minuman</option>
                                <option value="Snack">Snack</option>
                                <option value="Kebersihan">Kebersihan</option>
                            </select>
                        </div>
                        <div>
                            <label class="form-label">Status Stok</label>
                            <select id="filterStock" class="form-input" onchange="filterProducts()">
                                <option value="">Semua Stok</option>
                                <option value="low">Stok Rendah</option>
                                <option value="medium">Stok Sedang</option>
                                <option value="high">Stok Tinggi</option>
                            </select>
                        </div>
                    </div>
                </div>
                
                <!-- DAFTAR PRODUK -->
                <div class="products-grid" id="productGridManage">
                    <!-- Produk akan diisi oleh JavaScript -->
                </div>
            </div>

            <!-- LAPORAN -->
            <div id="laporan" class="tab-content">
                <div class="action-bar">
                    <h2>📈 Laporan Penjualan</h2>
                    <div class="action-buttons">
                        <button class="btn btn-excel" onclick="exportSalesExcel()">
                            📥 Export Excel
                        </button>
                        <button class="btn btn-primary" onclick="generateReport()">
                            🔍 Generate
                        </button>
                    </div>
                </div>
                
                <!-- FILTER LAPORAN -->
                <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 30px;">
                    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px;">
                        <div>
                            <label class="form-label">Periode</label>
                            <select id="reportPeriod" class="form-input" onchange="toggleCustomDate()">
                                <option value="today">Hari Ini</option>
                                <option value="yesterday">Kemarin</option>
                                <option value="week">Minggu Ini</option>
                                <option value="month">Bulan Ini</option>
                                <option value="custom">Custom Range</option>
                            </select>
                        </div>
                        
                        <div id="customDateRange" style="display: none; grid-column: span 2;">
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                                <div>
                                    <label class="form-label">Dari Tanggal</label>
                                    <input type="date" id="startDate" class="form-input">
                                </div>
                                <div>
                                    <label class="form-label">Sampai Tanggal</label>
                                    <input type="date" id="endDate" class="form-input">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- STATISTIK -->
                <div class="stats-grid" style="margin-bottom: 30px;">
                    <div class="stat-card">
                        <div class="stat-value" id="reportTotalSales">Rp 0</div>
                        <div class="stat-label">Total Penjualan</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="reportTotalTransactions">0</div>
                        <div class="stat-label">Jumlah Transaksi</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="reportAvgTransaction">Rp 0</div>
                        <div class="stat-label">Rata-rata per Transaksi</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="reportTotalItems">0</div>
                        <div class="stat-label">Item Terjual</div>
                    </div>
                </div>
                
                <!-- TABEL LAPORAN -->
                <div style="overflow-x: auto;">
                    <table class="data-table">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Tanggal</th>
                                <th>No. Transaksi</th>
                                <th>Produk</th>
                                <th>Qty</th>
                                <th>Total</th>
                                <th>Kasir</th>
                            </tr>
                        </thead>
                        <tbody id="reportTable">
                            <!-- Data laporan akan diisi JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- RIWAYAT TRANSAKSI -->
            <div id="transaksi" class="tab-content">
                <div class="action-bar">
                    <h2>🧾 Riwayat Transaksi</h2>
                    <div class="action-buttons">
                        <button class="btn btn-excel" onclick="exportTransactionsExcel()">
                            📥 Export Excel
                        </button>
                        <button class="btn btn-primary" onclick="loadTransactions()">
                            🔄 Refresh
                        </button>
                    </div>
                </div>
                
                <div style="overflow-x: auto;">
                    <table class="data-table">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Tanggal</th>
                                <th>ID Transaksi</th>
                                <th>Items</th>
                                <th>Total</th>
                                <th>Bayar</th>
                                <th>Kembali</th>
                                <th>Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="transactionTable">
                            <!-- Data transaksi akan diisi JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- EXPORT DATA -->
            <div id="export" class="tab-content">
                <div class="action-bar">
                    <h2>📥 Export Data ke Excel</h2>
                    <div class="action-buttons">
                        <button class="btn btn-excel" onclick="showExportModal()">
                            📊 Pilih Export
                        </button>
                        <button class="btn btn-primary" onclick="exportAllData()">
                            📦 Export Semua
                        </button>
                    </div>
                </div>
                
                <div style="background: #f8f9fa; border-radius: 15px; padding: 30px; margin-top: 20px;">
                    <h3>📋 Pilihan Export Data</h3>
                    <p style="color: #7f8c8d; margin-bottom: 30px;">Pilih jenis data yang ingin diexport ke Excel</p>
                    
                    <div class="export-options">
                        <div class="export-option" onclick="exportToExcel('penjualan')">
                            <div class="export-icon">💰</div>
                            <div class="export-label">Laporan Penjualan</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Data penjualan harian</small>
                        </div>
                        
                        <div class="export-option" onclick="exportToExcel('produk')">
                            <div class="export-icon">📦</div>
                            <div class="export-label">Master Produk</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Data semua produk</small>
                        </div>
                        
                        <div class="export-option" onclick="exportToExcel('stok')">
                            <div class="export-icon">📊</div>
                            <div class="export-label">Laporan Stok</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Stok barang & alert</small>
                        </div>
                        
                        <div class="export-option" onclick="exportToExcel('transaksi')">
                            <div class="export-icon">🧾</div>
                            <div class="export-label">Transaksi Detail</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Detail semua transaksi</small>
                        </div>
                        
                        <div class="export-option" onclick="exportToExcel('custom')">
                            <div class="export-icon">📅</div>
                            <div class="export-label">Custom Periode</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Export berdasarkan periode</small>
                        </div>
                        
                        <div class="export-option" onclick="exportToPDF()">
                            <div class="export-icon">📄</div>
                            <div class="export-label">Export PDF</div>
                            <small style="color: #7f8c8d; margin-top: 5px;">Laporan dalam PDF</small>
                        </div>
                    </div>
                    
                    <!-- QUICK STATS -->
                    <div style="margin-top: 40px;">
                        <h3>📈 Statistik Data Tersedia</h3>
                        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin-top: 20px;">
                            <div style="background: white; padding: 20px; border-radius: 10px;">
                                <div style="font-size: 24px; font-weight: bold; color: #3498db;" id="exportProductCount">0</div>
                                <div style="color: #7f8c8d; font-size: 14px;">Total Produk</div>
                            </div>
                            <div style="background: white; padding: 20px; border-radius: 10px;">
                                <div style="font-size: 24px; font-weight: bold; color: #27ae60;" id="exportTransactionCount">0</div>
                                <div style="color: #7f8c8d; font-size: 14px;">Total Transaksi</div>
                            </div>
                            <div style="background: white; padding: 20px; border-radius: 10px;">
                                <div style="font-size: 24px; font-weight: bold; color: #e74c3c;" id="exportLowStockCount">0</div>
                                <div style="color: #7f8c8d; font-size: 14px;">Stok Rendah</div>
                            </div>
                            <div style="background: white; padding: 20px; border-radius: 10px;">
                                <div style="font-size: 24px; font-weight: bold; color: #f39c12;" id="exportTodaySales">Rp 0</div>
                                <div style="color: #7f8c8d; font-size: 14px;">Penjualan Hari Ini</div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- EXPORT HISTORY -->
                    <div style="margin-top: 40px;">
                        <h3>📋 Riwayat Export</h3>
                        <div style="overflow-x: auto; margin-top: 20px;">
                            <table class="data-table">
                                <thead>
                                    <tr>
                                        <th>Tanggal</th>
                                        <th>Jenis Data</th>
                                        <th>Jumlah Data</th>
                                        <th>Ukuran File</th>
                                        <th>Status</th>
                                    </tr>
                                </thead>
                                <tbody id="exportHistoryTable">
                                    <!-- Export history will be filled by JS -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- MODAL TAMBAH PRODUK -->
    <div id="productModal" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 1000; justify-content: center; align-items: center;">
        <div style="background: white; width: 90%; max-width: 500px; border-radius: 15px; padding: 25px;">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                <h3 id="modalTitle">Tambah Produk Baru</h3>
                <button onclick="closeModal()" style="background: none; border: none; font-size: 24px; cursor: pointer;">&times;</button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Nama Produk</label>
                <input type="text" id="modalProductName" class="form-input" placeholder="Contoh: Beras 5kg">
            </div>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                <div class="form-group">
                    <label class="form-label">Harga Jual</label>
                    <input type="number" id="modalProductPrice" class="form-input" placeholder="0">
                </div>
                
                <div class="form-group">
                    <label class="form-label">Stok Awal</label>
                    <input type="number" id="modalProductStock" class="form-input" placeholder="0">
                </div>
            </div>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                <div class="form-group">
                    <label class="form-label">Kategori</label>
                    <select id="modalProductCategory" class="form-input">
                        <option value="Sembako">Sembako</option>
                        <option value="Minuman">Minuman</option>
                        <option value="Snack">Snack</option>
                        <option value="Kebersihan">Kebersihan</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Satuan</label>
                    <select id="modalProductUnit" class="form-input">
                        <option value="pcs">pcs</option>
                        <option value="kg">kg</option>
                        <option value="liter">liter</option>
                    </select>
                </div>
            </div>
            
            <div style="display: flex; gap: 10px; margin-top: 30px;">
                <button class="btn btn-danger" onclick="closeModal()" style="flex: 1;">
                    Batal
                </button>
                <button class="btn btn-success" onclick="saveProduct()" style="flex: 2;">
                    💾 Simpan Produk
                </button>
            </div>
        </div>
    </div>

    <!-- SCRIPT UTAMA -->
    <script>
        // ==================== INITIALIZATION ====================
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Aplikasi Kasir PRO dengan Export Excel Loading...');
            showLoading(true);
            
            // Initialize aplikasi
            initializeApp();
            
            // Setup real-time clock
            updateDateTime();
            setInterval(updateDateTime, 1000);
            
            // Load initial data
            loadDashboard();
            loadProductsForKasir();
            loadProductsForManage();
            loadTransactions();
            updateExportStats();
            
            showLoading(false);
            
            // Show welcome message
            setTimeout(() => {
                showAlert('success', 'Aplikasi Kasir PRO dengan Export Excel siap digunakan!');
            }, 1000);
        });

        // ==================== GLOBAL VARIABLES ====================
        let products = [];
        let cart = [];
        let transactions = [];
        let settings = {};
        let salesChartInstance = null;
        let currentProductId = null;
        let exportHistory = JSON.parse(localStorage.getItem('kasir_export_history')) || [];

        // ==================== APP INITIALIZATION ====================
        function initializeApp() {
            console.log('Initializing app with Excel export...');
            
            // Load data from localStorage
            products = JSON.parse(localStorage.getItem('kasir_products')) || getDefaultProducts();
            transactions = JSON.parse(localStorage.getItem('kasir_transactions')) || [];
            settings = JSON.parse(localStorage.getItem('kasir_settings')) || getDefaultSettings();
            
            // Apply settings
            applySettings();
            
            console.log('App initialized:', {
                products: products.length,
                transactions: transactions.length,
                settings: settings
            });
        }

        function getDefaultProducts() {
            return [
                {
                    id: 1,
                    code: 'BR001',
                    name: 'Beras 5kg',
                    price: 60000,
                    cost: 50000,
                    category: 'Sembako',
                    unit: 'pack',
                    stock: 45,
                    minStock: 10,
                    sold: 125,
                    barcode: '899999901001'
                },
                {
                    id: 2,
                    code: 'MG002',
                    name: 'Minyak Goreng 2L',
                    price: 35000,
                    cost: 28000,
                    category: 'Sembako',
                    unit: 'botol',
                    stock: 28,
                    minStock: 10,
                    sold: 89,
                    barcode: '899999901002'
                },
                {
                    id: 3,
                    code: 'GP003',
                    name: 'Gula Pasir 1kg',
                    price: 15000,
                    cost: 12000,
                    category: 'Sembako',
                    unit: 'kg',
                    stock: 12,
                    minStock: 10,
                    sold: 156,
                    barcode: '899999901003'
                },
                {
                    id: 4,
                    code: 'TL004',
                    name: 'Telur 1kg',
                    price: 28000,
                    cost: 22000,
                    category: 'Sembako',
                    unit: 'kg',
                    stock: 8,
                    minStock: 10,
                    sold: 67,
                    barcode: '899999901004'
                },
                {
                    id: 5,
                    code: 'SB005',
                    name: 'Sabun Mandi',
                    price: 5000,
                    cost: 3500,
                    category: 'Kebersihan',
                    unit: 'pcs',
                    stock: 120,
                    minStock: 20,
                    sold: 234,
                    barcode: '899999901005'
                },
                {
                    id: 6,
                    code: 'SP006',
                    name: 'Shampoo',
                    price: 12000,
                    cost: 9000,
                    category: 'Kebersihan',
                    unit: 'botol',
                    stock: 35,
                    minStock: 10,
                    sold: 78,
                    barcode: '899999901006'
                },
                {
                    id: 7,
                    code: 'AM007',
                    name: 'Air Mineral 600ml',
                    price: 3000,
                    cost: 2000,
                    category: 'Minuman',
                    unit: 'botol',
                    stock: 150,
                    minStock: 30,
                    sold: 345,
                    barcode: '899999901007'
                },
                {
                    id: 8,
                    code: 'KS008',
                    name: 'Kopi Sachet',
                    price: 2000,
                    cost: 1500,
                    category: 'Minuman',
                    unit: 'pcs',
                    stock: 89,
                    minStock: 20,
                    sold: 289,
                    barcode: '899999901008'
                },
                {
                    id: 9,
                    code: 'TC009',
                    name: 'Teh Celup',
                    price: 1500,
                    cost: 1000,
                    category: 'Minuman',
                    unit: 'pcs',
                    stock: 65,
                    minStock: 20,
                    sold: 167,
                    barcode: '899999901009'
                },
                {
                    id: 10,
                    code: 'BS010',
                    name: 'Biskuit',
                    price: 8000,
                    cost: 6000,
                    category: 'Snack',
                    unit: 'pack',
                    stock: 42,
                    minStock: 15,
                    sold: 134,
                    barcode: '899999901010'
                }
            ];
        }

        function getDefaultSettings() {
            return {
                storeName: 'TOKO MAKMUR SEJAHTERA',
                storeAddress: 'Jl. Contoh No. 123, Kota Contoh',
                storePhone: '(021) 1234-5678',
                taxRate: 10,
                receiptFooter: 'Terima kasih atas kunjungan Anda'
            };
        }

        function applySettings() {
            // Settings applied in individual functions
        }

        // ==================== NAVIGATION ====================
        function showTab(tabId) {
            // Update active tab button
            document.querySelectorAll('.nav-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Show selected tab content
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            document.getElementById(tabId).classList.add('active');
            
            // Refresh data based on tab
            if (tabId === 'dashboard') loadDashboard();
            if (tabId === 'produk') loadProductsForManage();
            if (tabId === 'laporan') generateReport();
            if (tabId === 'transaksi') loadTransactions();
            if (tabId === 'export') updateExportStats();
        }

        // ==================== DASHBOARD ====================
        function loadDashboard() {
            console.log('Loading dashboard...');
            
            // Calculate statistics
            const today = new Date().toLocaleDateString('id-ID');
            let totalSales = 0;
            let totalTransactionsToday = 0;
            let totalItemsSold = 0;
            let lowStockCount = 0;
            
            // Calculate today's sales
            transactions.forEach(trans => {
                if (trans.date === today) {
                    totalSales += trans.total;
                    totalTransactionsToday++;
                    totalItemsSold += trans.items.reduce((sum, item) => sum + item.quantity, 0);
                }
            });
            
            // Count low stock products
            products.forEach(product => {
                if (product.stock < product.minStock) {
                    lowStockCount++;
                }
            });
            
            // Update dashboard cards
            document.getElementById('totalSales').textContent = formatCurrency(totalSales);
            document.getElementById('totalTransactions').textContent = totalTransactionsToday;
            document.getElementById('totalProducts').textContent = products.length;
            document.getElementById('lowStockProducts').textContent = lowStockCount;
            
            // Update sales chart
            updateSalesChart();
            
            // Update top products
            updateTopProducts();
        }

        function updateSalesChart() {
            const ctx = document.getElementById('salesChart');
            if (!ctx) return;
            
            // Prepare data for last 7 days
            const labels = [];
            const salesData = [];
            
            for (let i = 6; i >= 0; i--) {
                const date = new Date();
                date.setDate(date.getDate() - i);
                const dateStr = date.toLocaleDateString('id-ID', { weekday: 'short' });
                labels.push(dateStr);
                
                // Calculate sales for this day
                const dateFull = date.toLocaleDateString('id-ID');
                const daySales = transactions
                    .filter(trans => trans.date === dateFull)
                    .reduce((sum, trans) => sum + trans.total, 0);
                
                salesData.push(daySales);
            }
            
            // Destroy existing chart
            if (salesChartInstance) {
                salesChartInstance.destroy();
            }
            
            // Create new chart
            salesChartInstance = new Chart(ctx.getContext('2d'), {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Penjualan (Rp)',
                        data: salesData,
                        borderColor: '#3498db',
                        backgroundColor: 'rgba(52, 152, 219, 0.1)',
                        borderWidth: 3,
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return 'Rp ' + context.parsed.y.toLocaleString('id-ID');
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return 'Rp ' + (value / 1000).toFixed(0) + 'K';
                                }
                            }
                        }
                    }
                }
            });
        }

        function updateTopProducts() {
            const container = document.getElementById('topProducts');
            if (!container) return;
            
            // Count sales per product
            let productSales = {};
            transactions.forEach(trans => {
                trans.items.forEach(item => {
                    if (!productSales[item.name]) {
                        productSales[item.name] = {
                            quantity: 0,
                            total: 0
                        };
                    }
                    productSales[item.name].quantity += item.quantity;
                    productSales[item.name].total += item.quantity * item.price;
                });
            });
            
            // Sort by quantity sold
            const topProducts = Object.entries(productSales)
                .sort((a, b) => b[1].quantity - a[1].quantity)
                .slice(0, 5);
            
            let html = '';
            topProducts.forEach(([name, data], index) => {
                const product = products.find(p => p.name === name);
                html += `
                    <div style="display: flex; justify-content: space-between; align-items: center; padding: 10px; background: ${index % 2 === 0 ? '#f8f9fa' : 'white'}; border-radius: 8px; margin-bottom: 10px;">
                        <div style="display: flex; align-items: center; gap: 10px;">
                            <div style="font-weight: bold; color: #2c3e50;">${index + 1}.</div>
                            <div>
                                <div style="font-weight: 600;">${name}</div>
                                <div style="font-size: 12px; color: #7f8c8d;">${product?.category || '-'}</div>
                            </div>
                        </div>
                        <div style="text-align: right;">
                            <div style="font-weight: bold; color: #27ae60;">${data.quantity} pcs</div>
                            <div style="font-size: 12px; color: #7f8c8d;">${formatCurrency(data.total)}</div>
                        </div>
                    </div>
                `;
            });
            
            container.innerHTML = html || '<p style="color: #7f8c8d; text-align: center;">Belum ada data penjualan</p>';
        }

        function refreshDashboard() {
            loadDashboard();
            showAlert('success', 'Dashboard berhasil di-refresh');
        }

        // ==================== EXPORT EXCEL FUNCTIONS ====================
        
        async function exportToExcel(type) {
            showLoading(true);
            
            try {
                const workbook = new ExcelJS.Workbook();
                workbook.creator = 'Aplikasi Kasir PRO';
                workbook.lastModifiedBy = 'Kasir System';
                workbook.created = new Date();
                workbook.modified = new Date();
                
                let fileName = '';
                let worksheet;
                
                switch(type) {
                    case 'penjualan':
                        fileName = `Laporan_Penjualan_${getCurrentDate()}.xlsx`;
                        worksheet = await createSalesReportWorksheet(workbook);
                        break;
                        
                    case 'produk':
                        fileName = `Data_Produk_${getCurrentDate()}.xlsx`;
                        worksheet = await createProductWorksheet(workbook);
                        break;
                        
                    case 'stok':
                        fileName = `Laporan_Stok_${getCurrentDate()}.xlsx`;
                        worksheet = await createStockReportWorksheet(workbook);
                        break;
                        
                    case 'transaksi':
                        fileName = `Transaksi_${getCurrentDate()}.xlsx`;
                        worksheet = await createTransactionWorksheet(workbook);
                        break;
                        
                    case 'custom':
                        showCustomExportOptions();
                        showLoading(false);
                        return;
                        
                    default:
                        showAlert('error', 'Jenis export tidak dikenali');
                        showLoading(false);
                        return;
                }
                
                // Auto-fit columns
                worksheet.columns.forEach(column => {
                    column.width = column.header.length + 5;
                });
                
                // Generate Excel file
                const buffer = await workbook.xlsx.writeBuffer();
                const blob = new Blob([buffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
                saveAs(blob, fileName);
                
                // Add to export history
                addToExportHistory(type, fileName, worksheet.rowCount - 1);
                
                showAlert('success', `Data berhasil diexport ke ${fileName}`);
                
            } catch (error) {
                console.error('Export error:', error);
                showAlert('error', 'Gagal export data: ' + error.message);
            } finally {
                showLoading(false);
            }
        }

        async function createSalesReportWorksheet(workbook) {
            const worksheet = workbook.addWorksheet('Laporan Penjualan');
            
            // Header
            worksheet.mergeCells('A1:G1');
            worksheet.getCell('A1').value = 'LAPORAN PENJUALAN';
            worksheet.getCell('A1').font = { bold: true, size: 16 };
            worksheet.getCell('A1').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A2:G2');
            worksheet.getCell('A2').value = settings.storeName;
            worksheet.getCell('A2').font = { size: 12 };
            worksheet.getCell('A2').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A3:G3');
            worksheet.getCell('A3').value = `Periode: ${new Date().toLocaleDateString('id-ID')}`;
            worksheet.getCell('A3').alignment = { horizontal: 'center' };
            
            // Column headers
            const headers = ['No', 'Tanggal', 'ID Transaksi', 'Nama Produk', 'Qty', 'Harga Satuan', 'Total'];
            worksheet.addRow(headers);
            
            // Style header row
            const headerRow = worksheet.getRow(5);
            headerRow.font = { bold: true, color: { argb: 'FFFFFFFF' } };
            headerRow.fill = {
                type: 'pattern',
                pattern: 'solid',
                fgColor: { argb: 'FF2C3E50' }
            };
            headerRow.alignment = { horizontal: 'center' };
            
            // Data rows
            let rowNumber = 1;
            let grandTotal = 0;
            
            transactions.forEach(transaction => {
                transaction.items.forEach(item => {
                    const row = worksheet.addRow([
                        rowNumber++,
                        transaction.date,
                        transaction.id,
                        item.name,
                        item.quantity,
                        item.price,
                        item.quantity * item.price
                    ]);
                    
                    // Alternate row colors
                    if (rowNumber % 2 === 0) {
                        row.fill = {
                            type: 'pattern',
                            pattern: 'solid',
                            fgColor: { argb: 'FFF8F9FA' }
                        };
                    }
                    
                    grandTotal += item.quantity * item.price;
                });
            });
            
            // Summary row
            worksheet.addRow([]);
            const summaryRow = worksheet.addRow(['', '', '', '', '', 'TOTAL:', grandTotal]);
            summaryRow.getCell(6).font = { bold: true };
            summaryRow.getCell(7).font = { bold: true };
            summaryRow.getCell(7).numFmt = '"Rp"#,##0';
            
            return worksheet;
        }

        async function createProductWorksheet(workbook) {
            const worksheet = workbook.addWorksheet('Data Produk');
            
            // Header
            worksheet.mergeCells('A1:H1');
            worksheet.getCell('A1').value = 'DATA MASTER PRODUK';
            worksheet.getCell('A1').font = { bold: true, size: 16 };
            worksheet.getCell('A1').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A2:H2');
            worksheet.getCell('A2').value = `Total Produk: ${products.length}`;
            worksheet.getCell('A2').alignment = { horizontal: 'center' };
            
            // Column headers
            const headers = ['Kode', 'Nama Produk', 'Kategori', 'Satuan', 'Harga Beli', 'Harga Jual', 'Stok', 'Status'];
            worksheet.addRow(headers);
            
            // Style header row
            const headerRow = worksheet.getRow(4);
            headerRow.font = { bold: true, color: { argb: 'FFFFFFFF' } };
            headerRow.fill = {
                type: 'pattern',
                pattern: 'solid',
                fgColor: { argb: 'FF27AE60' }
            };
            headerRow.alignment = { horizontal: 'center' };
            
            // Data rows
            products.forEach((product, index) => {
                const status = product.stock < product.minStock ? 'STOK RENDAH' : 
                              product.stock < product.minStock * 2 ? 'STOK SEDANG' : 'STOK AMAN';
                
                const row = worksheet.addRow([
                    product.code,
                    product.name,
                    product.category,
                    product.unit,
                    product.cost,
                    product.price,
                    product.stock,
                    status
                ]);
                
                // Style based on stock status
                if (product.stock < product.minStock) {
                    row.getCell(8).fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFFFCCCC' }
                    };
                }
                
                // Number formatting for currency
                row.getCell(5).numFmt = '"Rp"#,##0';
                row.getCell(6).numFmt = '"Rp"#,##0';
                
                // Alternate row colors
                if (index % 2 === 0) {
                    row.fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFF8F9FA' }
                    };
                }
            });
            
            // Auto-fit columns
            worksheet.columns = [
                { width: 15 }, // Kode
                { width: 30 }, // Nama
                { width: 15 }, // Kategori
                { width: 10 }, // Satuan
                { width: 15 }, // Harga Beli
                { width: 15 }, // Harga Jual
                { width: 10 }, // Stok
                { width: 15 }  // Status
            ];
            
            return worksheet;
        }

        async function createStockReportWorksheet(workbook) {
            const worksheet = workbook.addWorksheet('Laporan Stok');
            
            // Header
            worksheet.mergeCells('A1:G1');
            worksheet.getCell('A1').value = 'LAPORAN STOK BARANG';
            worksheet.getCell('A1').font = { bold: true, size: 16 };
            worksheet.getCell('A1').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A2:G2');
            const lowStockCount = products.filter(p => p.stock < p.minStock).length;
            worksheet.getCell('A2').value = `Total Produk: ${products.length} | Stok Rendah: ${lowStockCount}`;
            worksheet.getCell('A2').alignment = { horizontal: 'center' };
            
            // Column headers
            const headers = ['No', 'Kode Produk', 'Nama Produk', 'Stok Saat Ini', 'Stok Minimal', 'Selisih', 'Status'];
            worksheet.addRow(headers);
            
            // Style header row
            const headerRow = worksheet.getRow(4);
            headerRow.font = { bold: true, color: { argb: 'FFFFFFFF' } };
            headerRow.fill = {
                type: 'pattern',
                pattern: 'solid',
                fgColor: { argb: 'FFF39C12' }
            };
            headerRow.alignment = { horizontal: 'center' };
            
            // Data rows
            products.forEach((product, index) => {
                const difference = product.stock - product.minStock;
                const status = difference < 0 ? 'PERLU RESTOCK' : 
                              difference < 5 ? 'HATI-HATI' : 'AMAN';
                
                const row = worksheet.addRow([
                    index + 1,
                    product.code,
                    product.name,
                    product.stock,
                    product.minStock,
                    difference,
                    status
                ]);
                
                // Color coding based on status
                if (status === 'PERLU RESTOCK') {
                    row.fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFFFCCCC' }
                    };
                    row.font = { bold: true, color: { argb: 'FFE74C3C' } };
                } else if (status === 'HATI-HATI') {
                    row.fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFFFEECC' }
                    };
                }
                
                // Alternate row colors
                if (index % 2 === 1) {
                    const currentFill = row.fill || {};
                    row.fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFF8F9FA' }
                    };
                }
            });
            
            // Summary section
            worksheet.addRow([]);
            const lowStockProducts = products.filter(p => p.stock < p.minStock);
            if (lowStockProducts.length > 0) {
                worksheet.addRow(['PRODUK YANG PERLU RESTOCK:', '', '', '', '', '', '']);
                lowStockProducts.forEach((product, index) => {
                    worksheet.addRow([
                        '',
                        product.code,
                        product.name,
                        product.stock,
                        product.minStock,
                        product.stock - product.minStock,
                        'SEGERA RESTOCK!'
                    ]);
                });
            }
            
            return worksheet;
        }

        async function createTransactionWorksheet(workbook) {
            const worksheet = workbook.addWorksheet('Transaksi');
            
            // Header
            worksheet.mergeCells('A1:H1');
            worksheet.getCell('A1').value = 'LAPORAN TRANSAKSI DETAIL';
            worksheet.getCell('A1').font = { bold: true, size: 16 };
            worksheet.getCell('A1').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A2:H2');
            worksheet.getCell('A2').value = `Total Transaksi: ${transactions.length}`;
            worksheet.getCell('A2').alignment = { horizontal: 'center' };
            
            // Column headers
            const headers = ['No', 'Tanggal', 'Waktu', 'ID Transaksi', 'Total Items', 'Subtotal', 'Pajak', 'Total'];
            worksheet.addRow(headers);
            
            // Style header row
            const headerRow = worksheet.getRow(4);
            headerRow.font = { bold: true, color: { argb: 'FFFFFFFF' } };
            headerRow.fill = {
                type: 'pattern',
                pattern: 'solid',
                fgColor: { argb: 'FF3498DB' }
            };
            headerRow.alignment = { horizontal: 'center' };
            
            // Data rows
            transactions.forEach((transaction, index) => {
                const totalItems = transaction.items.reduce((sum, item) => sum + item.quantity, 0);
                const taxAmount = transaction.subtotal * (transaction.tax / 100);
                
                const row = worksheet.addRow([
                    index + 1,
                    transaction.date,
                    transaction.time,
                    transaction.id,
                    totalItems,
                    transaction.subtotal,
                    taxAmount,
                    transaction.total
                ]);
                
                // Number formatting for currency
                row.getCell(6).numFmt = '"Rp"#,##0';
                row.getCell(7).numFmt = '"Rp"#,##0';
                row.getCell(8).numFmt = '"Rp"#,##0';
                
                // Alternate row colors
                if (index % 2 === 0) {
                    row.fill = {
                        type: 'pattern',
                        pattern: 'solid',
                        fgColor: { argb: 'FFF8F9FA' }
                    };
                }
            });
            
            // Summary
            const totalSales = transactions.reduce((sum, trans) => sum + trans.total, 0);
            worksheet.addRow([]);
            worksheet.addRow(['', '', '', '', '', '', 'TOTAL PENJUALAN:', totalSales]);
            const totalRow = worksheet.lastRow;
            totalRow.getCell(7).font = { bold: true };
            totalRow.getCell(8).font = { bold: true };
            totalRow.getCell(8).numFmt = '"Rp"#,##0';
            
            return worksheet;
        }

        async function createCustomPeriodWorksheet(workbook, startDate, endDate) {
            const worksheet = workbook.addWorksheet('Laporan Custom');
            
            // Filter transactions by date range
            const filteredTransactions = transactions.filter(trans => {
                const transDate = new Date(trans.date.split('/').reverse().join('-'));
                return transDate >= startDate && transDate <= endDate;
            });
            
            // Header
            worksheet.mergeCells('A1:H1');
            worksheet.getCell('A1').value = 'LAPORAN PENJUALAN CUSTOM PERIODE';
            worksheet.getCell('A1').font = { bold: true, size: 16 };
            worksheet.getCell('A1').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A2:H2');
            worksheet.getCell('A2').value = `Periode: ${startDate.toLocaleDateString('id-ID')} s/d ${endDate.toLocaleDateString('id-ID')}`;
            worksheet.getCell('A2').alignment = { horizontal: 'center' };
            
            worksheet.mergeCells('A3:H3');
            worksheet.getCell('A3').value = `Jumlah Transaksi: ${filteredTransactions.length}`;
            worksheet.getCell('A3').alignment = { horizontal: 'center' };
            
            // Column headers
            const headers = ['Tanggal', 'ID Transaksi', 'Nama Produk', 'Qty', 'Harga', 'Subtotal', 'Pajak', 'Total'];
            worksheet.addRow(headers);
            
            // Style header row
            const headerRow = worksheet.getRow(5);
            headerRow.font = { bold: true, color: { argb: 'FFFFFFFF' } };
            headerRow.fill = {
                type: 'pattern',
                pattern: 'solid',
                fgColor: { argb: 'FF9B59B6' }
            };
            headerRow.alignment = { horizontal: 'center' };
            
            // Data rows
            let grandTotal = 0;
            filteredTransactions.forEach(transaction => {
                transaction.items.forEach(item => {
                    const subtotal = item.quantity * item.price;
                    const taxAmount = subtotal * (transaction.tax / 100);
                    const total = subtotal + taxAmount;
                    
                    const row = worksheet.addRow([
                        transaction.date,
                        transaction.id,
                        item.name,
                        item.quantity,
                        item.price,
                        subtotal,
                        taxAmount,
                        total
                    ]);
                    
                    // Number formatting
                    row.getCell(5).numFmt = '"Rp"#,##0';
                    row.getCell(6).numFmt = '"Rp"#,##0';
                    row.getCell(7).numFmt = '"Rp"#,##0';
                    row.getCell(8).numFmt = '"Rp"#,##0';
                    
                    grandTotal += total;
                });
            });
            
            // Summary
            worksheet.addRow([]);
            worksheet.addRow(['', '', '', '', '', '', 'GRAND TOTAL:', grandTotal]);
            const totalRow = worksheet.lastRow;
            totalRow.getCell(7).font = { bold: true };
            totalRow.getCell(8).font = { bold: true };
            totalRow.getCell(8).numFmt = '"Rp"#,##0';
            
            return worksheet;
        }

        // ==================== EXPORT HELPER FUNCTIONS ====================
        
        function getCurrentDate() {
            const now = new Date();
            return now.toISOString().split('T')[0].replace(/-/g, '');
        }

        function showExportModal() {
            document.getElementById('exportModal').classList.add('active');
        }

        function closeExportModal() {
            document.getElementById('exportModal').classList.remove('active');
        }

        function showCustomExportOptions() {
            document.getElementById('customExportOptions').style.display = 'block';
        }

        async function exportCustomPeriod() {
            const startDate = new Date(document.getElementById('exportStartDate').value);
            const endDate = new Date(document.getElementById('exportEndDate').value);
            
            if (!startDate || !endDate || startDate > endDate) {
                showAlert('error', 'Tanggal tidak valid');
                return;
            }
            
            showLoading(true);
            
            try {
                const workbook = new ExcelJS.Workbook();
                await createCustomPeriodWorksheet(workbook, startDate, endDate);
                
                const fileName = `Laporan_Custom_${startDate.toISOString().split('T')[0]}_${endDate.toISOString().split('T')[0]}.xlsx`;
                const buffer = await workbook.xlsx.writeBuffer();
                const blob = new Blob([buffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
                saveAs(blob, fileName);
                
                addToExportHistory('custom', fileName, workbook.getWorksheet('Laporan Custom').rowCount - 5);
                
                showAlert('success', `Laporan custom periode berhasil diexport`);
                
            } catch (error) {
                console.error('Export custom error:', error);
                showAlert('error', 'Gagal export: ' + error.message);
            } finally {
                showLoading(false);
                closeExportModal();
            }
        }

        async function exportAllData() {
            showLoading(true);
            
            try {
                const workbook = new ExcelJS.Workbook();
                
                // Create multiple worksheets
                await createProductWorksheet(workbook);
                await createSalesReportWorksheet(workbook);
                await createStockReportWorksheet(workbook);
                await createTransactionWorksheet(workbook);
                
                // Summary sheet
                const summarySheet = workbook.addWorksheet('Ringkasan');
                summarySheet.addRow(['RINGKASAN DATA KASIR']);
                summarySheet.addRow([]);
                summarySheet.addRow(['Total Produk:', products.length]);
                summarySheet.addRow(['Total Transaksi:', transactions.length]);
                summarySheet.addRow(['Tanggal Export:', new Date().toLocaleDateString('id-ID')]);
                
                const fileName = `Backup_Data_Kasir_${getCurrentDate()}.xlsx`;
                const buffer = await workbook.xlsx.writeBuffer();
                const blob = new Blob([buffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
                saveAs(blob, fileName);
                
                addToExportHistory('all', fileName, products.length + transactions.length);
                
                showAlert('success', 'Semua data berhasil diexport ke Excel');
                
            } catch (error) {
                console.error('Export all error:', error);
                showAlert('error', 'Gagal export semua data: ' + error.message);
            } finally {
                showLoading(false);
            }
        }

        function addToExportHistory(type, fileName, dataCount) {
            const history = {
                date: new Date().toLocaleString('id-ID'),
                type: type,
                fileName: fileName,
                dataCount: dataCount,
                fileSize: '~' + Math.floor(dataCount * 0.1) + ' KB'
            };
            
            exportHistory.unshift(history);
            if (exportHistory.length > 10) exportHistory.pop();
            
            localStorage.setItem('kasir_export_history', JSON.stringify(exportHistory));
            updateExportHistoryTable();
        }

        function updateExportHistoryTable() {
            const table = document.getElementById('exportHistoryTable');
            if (!table) return;
            
            table.innerHTML = '';
            
            exportHistory.forEach(history => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${history.date}</td>
                    <td>${getExportTypeLabel(history.type)}</td>
                    <td>${history.dataCount} data</td>
                    <td>${history.fileSize}</td>
                    <td><span style="color: #27ae60;">✓ Berhasil</span></td>
                `;
                table.appendChild(row);
            });
        }

        function getExportTypeLabel(type) {
            const labels = {
                'penjualan': 'Laporan Penjualan',
                'produk': 'Data Produk',
                'stok': 'Laporan Stok',
                'transaksi': 'Transaksi',
                'custom': 'Custom Periode',
                'all': 'Semua Data'
            };
            return labels[type] || type;
        }

        function updateExportStats() {
            const today = new Date().toLocaleDateString('id-ID');
            const todaySales = transactions
                .filter(t => t.date === today)
                .reduce((sum, t) => sum + t.total, 0);
            
            const lowStockCount = products.filter(p => p.stock < p.minStock).length;
            
            document.getElementById('exportProductCount').textContent = products.length;
            document.getElementById('exportTransactionCount').textContent = transactions.length;
            document.getElementById('exportLowStockCount').textContent = lowStockCount;
            document.getElementById('exportTodaySales').textContent = formatCurrency(todaySales);
            
            updateExportHistoryTable();
        }

        // ==================== QUICK EXPORT BUTTONS ====================
        
        function exportDashboardExcel() {
            exportToExcel('penjualan');
        }

        function exportProductExcel() {
            exportToExcel('produk');
        }

        function exportSalesExcel() {
            exportToExcel('penjualan');
        }

        function exportTransactionsExcel() {
            exportToExcel('transaksi');
        }

        function exportToPDF() {
            showAlert('info', 'Fitur export PDF akan datang. Gunakan Print > Save as PDF untuk saat ini.');
            // For now, trigger print dialog
            window.print();
        }

        // ==================== KASIR FUNCTIONS ====================
        // [Previous kasir functions remain the same...]
        // Note: I'm truncating here to save space, but all previous kasir functions should be included

        function loadProductsForKasir() {
            const container = document.getElementById('productGridKasir');
            if (!container) return;
            
            container.innerHTML = '';
            
            products.forEach(product => {
                const stockClass = getStockClass(product.stock, product.minStock);
                const stockBadge = getStockBadge(product.stock, product.minStock);
                
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                productCard.innerHTML = `
                    <div class="product-name">${product.name}</div>
                    <div class="product-price">${formatCurrency(product.price)}</div>
                    <div style="color: #7f8c8d; font-size: 12px; margin-bottom: 10px;">
                        ${product.category} • ${product.unit}
                    </div>
                    <div class="product-stock">
                        <span>Stok: ${product.stock}</span>
                        <span class="stock-badge ${stockClass}">${stockBadge}</span>
                    </div>
                `;
                
                productCard.onclick = () => addToCart(product);
                container.appendChild(productCard);
            });
        }

        function addToCart(product) {
            // Check if product has stock
            if (product.stock <= 0) {
                showAlert('error', 'Stok produk habis!');
                return;
            }
            
            // Find existing item in cart
            const existingItem = cart.find(item => item.id === product.id);
            
            if (existingItem) {
                // Check if enough stock
                if (existingItem.quantity >= product.stock) {
                    showAlert('error', 'Stok tidak mencukupi!');
                    return;
                }
                existingItem.quantity++;
            } else {
                cart.push({
                    id: product.id,
                    name: product.name,
                    price: product.price,
                    quantity: 1,
                    stock: product.stock
                });
            }
            
            updateCartDisplay();
            showAlert('success', `${product.name} ditambahkan ke keranjang`);
        }

        function updateCartDisplay() {
            const container = document.getElementById('cartContainer');
            const summary = document.getElementById('cartSummary');
            
            if (cart.length === 0) {
                container.innerHTML = `
                    <div class="text-center p-20" style="color: #7f8c8d;">
                        <div style="font-size: 60px; margin-bottom: 20px;">🛒</div>
                        <h4>Keranjang Kosong</h4>
                        <p>Pilih produk dari daftar di bawah</p>
                    </div>
                `;
                summary.style.display = 'none';
                return;
            }
            
            // Calculate totals
            let subtotal = 0;
            let itemsHtml = '';
            
            cart.forEach((item, index) => {
                const itemTotal = item.price * item.quantity;
                subtotal += itemTotal;
                
                itemsHtml += `
                    <div class="cart-item">
                        <div style="flex: 2;">
                            <div style="font-weight: bold;">${item.name}</div>
                            <div style="font-size: 12px; color: #7f8c8d;">${formatCurrency(item.price)}/item</div>
                        </div>
                        <div class="item-actions">
                            <button class="qty-btn" onclick="updateCartQuantity(${index}, -1)">-</button>
                            <span style="font-weight: bold; min-width: 30px; text-align: center;">${item.quantity}</span>
                            <button class="qty-btn" onclick="updateCartQuantity(${index}, 1)">+</button>
                            <button class="qty-btn" onclick="removeFromCart(${index})" style="background: #e74c3c; margin-left: 10px;">🗑️</button>
                        </div>
                        <div style="font-weight: bold; min-width: 100px; text-align: right;">
                            ${formatCurrency(itemTotal)}
                        </div>
                    </div>
                `;
            });
            
            const taxRate = settings.taxRate || 10;
            const tax = subtotal * (taxRate / 100);
            const total = subtotal + tax;
            
            container.innerHTML = itemsHtml;
            summary.style.display = 'block';
            
            document.getElementById('subtotal').textContent = formatCurrency(subtotal);
            document.getElementById('tax').textContent = formatCurrency(tax);
            document.getElementById('total').textContent = formatCurrency(total);
            
            // Update change calculation
            updateChangeCalculation();
        }

        function updateCartQuantity(index, delta) {
            const item = cart[index];
            const product = products.find(p => p.id === item.id);
            
            if (!product) return;
            
            const newQuantity = item.quantity + delta;
            
            if (newQuantity < 1) {
                removeFromCart(index);
                return;
            }
            
            if (newQuantity > product.stock) {
                showAlert('error', 'Stok tidak mencukupi!');
                return;
            }
            
            item.quantity = newQuantity;
            updateCartDisplay();
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCartDisplay();
        }

        function clearCart() {
            if (cart.length === 0) return;
            
            if (confirm('Apakah Anda yakin ingin mengosongkan keranjang?')) {
                cart = [];
                updateCartDisplay();
                showAlert('success', 'Keranjang berhasil dikosongkan');
            }
        }

        function updateChangeCalculation() {
            const paymentInput = document.getElementById('paymentAmount');
            const changeContainer = document.getElementById('changeContainer');
            const changeAmount = document.getElementById('changeAmount');
            
            if (!paymentInput) return;
            
            const total = getCartTotal();
            const payment = parseFloat(paymentInput.value) || 0;
            
            if (payment >= total) {
                const change = payment - total;
                changeContainer.style.display = 'block';
                changeAmount.textContent = formatCurrency(change);
            } else {
                changeContainer.style.display = 'none';
            }
        }

        function getCartTotal() {
            const subtotal = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            const taxRate = settings.taxRate || 10;
            const tax = subtotal * (taxRate / 100);
            return subtotal + tax;
        }

        function processPayment() {
            if (cart.length === 0) {
                showAlert('error', 'Keranjang belanja kosong!');
                return;
            }
            
            const total = getCartTotal();
            const paymentInput = document.getElementById('paymentAmount');
            const payment = parseFloat(paymentInput.value) || 0;
            
            if (payment < total) {
                showAlert('error', `Jumlah pembayaran kurang! Total: ${formatCurrency(total)}`);
                return;
            }
            
            // Generate transaction ID
            const transactionId = 'TRX-' + Date.now();
            const date = new Date().toLocaleDateString('id-ID');
            const time = new Date().toLocaleTimeString('id-ID');
            
            // Create transaction record
            const transaction = {
                id: transactionId,
                date: date,
                time: time,
                items: [...cart],
                subtotal: cart.reduce((sum, item) => sum + (item.price * item.quantity), 0),
                tax: settings.taxRate || 10,
                total: total,
                payment: payment,
                change: payment - total,
                kasir: 'Admin'
            };
            
            // Update product stock
            cart.forEach(cartItem => {
                const product = products.find(p => p.id === cartItem.id);
                if (product) {
                    product.stock -= cartItem.quantity;
                    product.sold = (product.sold || 0) + cartItem.quantity;
                }
            });
            
            // Save to transactions
            transactions.unshift(transaction);
            saveToLocalStorage();
            
            // Show success message
            const change = payment - total;
            showAlert('success', `
                Pembayaran berhasil!<br>
                Total: ${formatCurrency(total)}<br>
                Bayar: ${formatCurrency(payment)}<br>
                Kembali: ${formatCurrency(change)}
            `);
            
            // Print receipt
            printReceipt(transaction);
            
            // Reset cart
            cart = [];
            updateCartDisplay();
            paymentInput.value = '';
            
            // Refresh dashboard
            loadDashboard();
            loadProductsForKasir();
            loadProductsForManage();
            updateExportStats();
        }

        // ==================== UTILITIES ====================
        function formatCurrency(amount) {
            return 'Rp ' + parseInt(amount).toLocaleString('id-ID');
        }

        function getStockClass(stock, minStock) {
            if (stock < minStock) return 'stock-low';
            if (stock < minStock * 3) return 'stock-medium';
            return 'stock-high';
        }

        function getStockBadge(stock, minStock) {
            if (stock < minStock) return 'Rendah';
            if (stock < minStock * 3) return 'Sedang';
            return 'Aman';
        }

        function updateDateTime() {
            const now = new Date();
            document.getElementById('currentDate').textContent = 
                now.toLocaleDateString('id-ID', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });
            document.getElementById('currentTime').textContent = 
                now.toLocaleTimeString('id-ID', { 
                    hour: '2-digit', 
                    minute: '2-digit', 
                    second: '2-digit' 
                });
        }

        function showAlert(type, message) {
            const alertDiv = type === 'success' ? 
                document.getElementById('alertSuccess') : 
                document.getElementById('alertError');
            
            alertDiv.innerHTML = message;
            alertDiv.className = `alert alert-${type} active`;
            
            setTimeout(() => {
                alertDiv.classList.remove('active');
            }, 5000);
        }

        function showLoading(show) {
            document.getElementById('loading').style.display = show ? 'flex' : 'none';
        }

        function saveToLocalStorage() {
            localStorage.setItem('kasir_products', JSON.stringify(products));
            localStorage.setItem('kasir_transactions', JSON.stringify(transactions));
            localStorage.setItem('kasir_settings', JSON.stringify(settings));
        }

        function printReceipt(transaction) {
            // ... existing print receipt code ...
        }
    </script>
</body>
</html>
