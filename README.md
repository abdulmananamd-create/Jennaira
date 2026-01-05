<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kasir - Jennaira Group</title>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Arial, sans-serif;
        }
        
        body {
            background: #f5f7fa;
            color: #333;
            padding-top: 150px; /* Space for fixed header + nav */
        }
        
        /* FIXED HEADER */
        .app-header {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            padding: 20px;
            text-align: center;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        /* FIXED NAVIGATION - TIDAK IKUT SCROLL */
        .app-nav {
            background: #34495e;
            padding: 15px;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            position: fixed;
            top: 80px;
            left: 0;
            width: 100%;
            z-index: 999;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        .app-content {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        /* TAB BUTTON */
        .nav-tab {
            background: #2c3e50;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .nav-tab:hover {
            background: #3498db;
            transform: translateY(-2px);
        }
        
        .nav-tab.active {
            background: #27ae60;
            box-shadow: 0 4px 8px rgba(39, 174, 96, 0.3);
        }
        
        /* TAB CONTENT */
        .tab-content {
            display: none;
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.08);
            margin-bottom: 30px;
            animation: fadeIn 0.5s;
        }
        
        .tab-content.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* SEARCH BAR - DITAMBAHKAN DI ATAS PRODUK */
        .search-container {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 12px;
            margin-bottom: 25px;
            border: 2px solid #e0e0e0;
        }
        
        .search-box {
            display: flex;
            gap: 15px;
            align-items: center;
        }
        
        .search-input {
            flex: 1;
            padding: 14px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s;
        }
        
        .search-input:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
        }
        
        .search-btn {
            padding: 14px 35px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .search-btn:hover {
            background: #2980b9;
            transform: translateY(-2px);
        }
        
        /* PRODUCT GRID */
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 25px;
            margin-top: 25px;
        }
        
        .product-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            border: 2px solid #e0e0e0;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
            position: relative;
        }
        
        .product-card:hover {
            border-color: #3498db;
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.15);
        }
        
        .product-name {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 16px;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .product-price {
            color: #27ae60;
            font-weight: bold;
            font-size: 20px;
            margin-bottom: 10px;
        }
        
        .product-stock {
            font-size: 13px;
            color: #7f8c8d;
            margin-top: 8px;
        }
        
        .stock-badge {
            padding: 4px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: bold;
            margin-top: 10px;
            display: inline-block;
        }
        
        .stock-high { background: #e8f6f3; color: #27ae60; }
        .stock-medium { background: #fff4e6; color: #f39c12; }
        .stock-low { background: #ffeaea; color: #e74c3c; }
        
        /* STATS */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 25px;
            margin: 30px 0;
        }
        
        .stat-card {
            background: white;
            border-radius: 12px;
            padding: 25px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.08);
            border-left: 5px solid;
            transition: transform 0.3s;
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
        }
        
        .stat-card.sales { border-color: #27ae60; }
        .stat-card.transactions { border-color: #e74c3c; }
        .stat-card.products { border-color: #3498db; }
        .stat-card.profit { border-color: #f39c12; }
        
        /* KASIR SECTION */
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
            box-shadow: 0 8px 30px rgba(0,0,0,0.08);
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
            width: 35px;
            height: 35px;
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
        
        /* BUTTONS */
        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 15px;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
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
        
        .btn-warning {
            background: #f39c12;
            color: white;
        }
        
        .btn-danger {
            background: #e74c3c;
            color: white;
        }
        
        .btn-csv {
            background: #217346;
            color: white;
        }
        
        .btn-csv:hover {
            background: #1a5c38;
            transform: translateY(-2px);
        }
        
        .btn-excel {
            background: #1d6f42;
            color: white;
        }
        
        .btn-excel:hover {
            background: #165932;
        }
        
        /* EXPORT OPTIONS */
        .export-options {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
            margin-top: 30px;
        }
        
        .export-card {
            flex: 1;
            min-width: 200px;
            background: white;
            border-radius: 12px;
            padding: 25px;
            border: 2px solid #e0e0e0;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
        }
        
        .export-card:hover {
            border-color: #27ae60;
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
        }
        
        .export-icon {
            font-size: 48px;
            margin-bottom: 15px;
        }
        
        .export-label {
            font-weight: 600;
            color: #2c3e50;
            font-size: 16px;
        }
        
        .export-desc {
            color: #7f8c8d;
            font-size: 13px;
            margin-top: 8px;
        }
        
        /* TABLES */
        .data-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 25px;
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 8px 25px rgba(0,0,0,0.08);
        }
        
        .data-table th {
            background: #2c3e50;
            color: white;
            padding: 18px;
            text-align: left;
            font-weight: 600;
        }
        
        .data-table td {
            padding: 15px;
            border-bottom: 1px solid #eee;
        }
        
        .data-table tr:hover {
            background: #f8f9fa;
        }
        
        /* ALERTS */
        .alert {
            padding: 18px 25px;
            border-radius: 10px;
            margin-bottom: 25px;
            display: none;
            animation: slideIn 0.3s;
        }
        
        .alert-success {
            background: #d4edda;
            color: #155724;
            border: 2px solid #c3e6cb;
        }
        
        .alert-error {
            background: #f8d7da;
            color: #721c24;
            border: 2px solid #f5c6cb;
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* FORM CONTROLS */
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-control {
            width: 100%;
            padding: 14px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            transition: border 0.3s;
        }
        
        .form-control:focus {
            outline: none;
            border-color: #3498db;
        }
        
        /* RESPONSIVE */
        @media (max-width: 768px) {
            body {
                padding-top: 180px;
            }
            
            .app-nav {
                top: 70px;
                flex-direction: column;
                align-items: stretch;
            }
            
            .nav-tab {
                width: 100%;
                justify-content: center;
            }
            
            .kasir-container {
                grid-template-columns: 1fr;
            }
            
            .products-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            .search-box {
                flex-direction: column;
            }
            
            .export-options {
                flex-direction: column;
            }
            
            .export-card {
                min-width: 100%;
            }
        }
    </style>
</head>
<body>
    <!-- FIXED HEADER -->
    <header class="app-header">
        <h1>💰 KASIR JENNAIRA GROUP</h1>
        <p>Sistem Kasir Lengkap dengan Export Laporan</p>
    </header>
    
    <!-- FIXED NAVIGATION - TIDAK IKUT SCROLL -->
    <nav class="app-nav">
        <button class="nav-tab active" onclick="showTab('dashboard')">
            📊 Dashboard
        </button>
        <button class="nav-tab" onclick="showTab('kasir')">
            💼 Kasir
        </button>
        <button class="nav-tab" onclick="showTab('produk')">
            📦 Produk
        </button>
        <button class="nav-tab" onclick="showTab('laporan')">
            📈 Laporan
        </button>
        <button class="nav-tab" onclick="showTab('export')">
            📥 Export Data
        </button>
    </nav>
    
    <!-- MAIN CONTENT -->
    <main class="app-content">
        <!-- ALERTS -->
        <div id="alert" class="alert"></div>
        
        <!-- DASHBOARD -->
        <div id="dashboard" class="tab-content active">
            <h2>📊 Dashboard Penjualan</h2>
            
            <div class="stats-grid">
                <div class="stat-card sales">
                    <h3>Penjualan Hari Ini</h3>
                    <div style="font-size: 32px; font-weight: bold; color: #27ae60;" id="salesToday">Rp 0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 10px;">↗️ 12% dari kemarin</div>
                </div>
                
                <div class="stat-card transactions">
                    <h3>Total Transaksi</h3>
                    <div style="font-size: 32px; font-weight: bold; color: #e74c3c;" id="totalTransactions">0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 10px;">Rata-rata: 15 transaksi/hari</div>
                </div>
                
                <div class="stat-card products">
                    <h3>Total Produk</h3>
                    <div style="font-size: 32px; font-weight: bold; color: #3498db;" id="totalProducts">0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 10px;">Aktif di sistem</div>
                </div>
                
                <div class="stat-card profit">
                    <h3>Profit Bulan Ini</h3>
                    <div style="font-size: 32px; font-weight: bold; color: #f39c12;" id="monthlyProfit">Rp 0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 10px;">Estimasi keuntungan</div>
                </div>
            </div>
            
            <div style="margin-top: 40px;">
                <button class="btn btn-primary" onclick="refreshDashboard()">
                    🔄 Refresh Dashboard
                </button>
            </div>
        </div>
        
        <!-- KASIR -->
        <div id="kasir" class="tab-content">
            <h2>💼 Kasir Point of Sale</h2>
            
            <div class="kasir-container">
                <div class="cart-items">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                        <h3>🛒 Keranjang Belanja</h3>
                        <button class="btn btn-danger" onclick="clearCart()">
                            🗑️ Kosongkan
                        </button>
                    </div>
                    
                    <div id="cartContainer">
                        <div style="text-align: center; padding: 40px; color: #7f8c8d;">
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
                
                <div class="cart-summary">
                    <h3>💰 Pembayaran</h3>
                    
                    <div class="form-group">
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Jumlah Bayar</label>
                        <input type="number" id="paymentAmount" class="form-control" placeholder="Masukkan jumlah bayar">
                    </div>
                    
                    <div id="changeContainer" style="display: none;">
                        <div style="background: #e8f6f3; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                            <div style="display: flex; justify-content: space-between;">
                                <span>Kembalian:</span>
                                <span id="changeAmount" style="font-weight: bold; color: #27ae60;">Rp 0</span>
                            </div>
                        </div>
                    </div>
                    
                    <div style="display: grid; gap: 12px;">
                        <button class="btn btn-success" onclick="processPayment()" style="width: 100%;">
                            💳 Proses Pembayaran
                        </button>
                        <button class="btn btn-primary" onclick="printReceipt()" style="width: 100%;">
                            🖨️ Cetak Struk
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- SEARCH BAR DI ATAS PRODUK -->
            <div class="search-container" style="margin-top: 30px;">
                <h3>🔍 Cari Produk</h3>
                <div class="search-box">
                    <input type="text" id="productSearch" class="search-input" placeholder="Ketik nama produk, kategori, atau harga..." onkeyup="filterProductsKasir()">
                    <button class="search-btn" onclick="filterProductsKasir()">
                        🔍 Search
                    </button>
                </div>
                <div style="margin-top: 10px; color: #7f8c8d; font-size: 14px;">
                    <span id="searchCount">Menampilkan semua produk</span>
                </div>
            </div>
            
            <!-- DAFTAR PRODUK -->
            <h3 style="margin-top: 30px; margin-bottom: 20px;">📦 Daftar Produk</h3>
            <div class="products-grid" id="productGridKasir">
                <!-- Produk akan diisi oleh JavaScript -->
            </div>
        </div>
        
        <!-- PRODUK -->
        <div id="produk" class="tab-content">
            <h2>📦 Manajemen Produk</h2>
            
            <!-- SEARCH BAR UNTUK PRODUK -->
            <div class="search-container">
                <h3>🔍 Cari & Filter Produk</h3>
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px;">
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Nama Produk</label>
                        <input type="text" id="searchName" class="form-control" placeholder="Nama produk..." oninput="filterProducts()">
                    </div>
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Kategori</label>
                        <select id="searchCategory" class="form-control" onchange="filterProducts()">
                            <option value="">Semua Kategori</option>
                            <option value="Sembako">Sembako</option>
                            <option value="Minuman">Minuman</option>
                            <option value="Snack">Snack</option>
                            <option value="Kebersihan">Kebersihan</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Status Stok</label>
                        <select id="searchStock" class="form-control" onchange="filterProducts()">
                            <option value="">Semua Stok</option>
                            <option value="low">Stok Rendah</option>
                            <option value="medium">Stok Sedang</option>
                            <option value="high">Stok Tinggi</option>
                        </select>
                    </div>
                </div>
            </div>
            
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                <div>
                    <h3>Daftar Produk</h3>
                    <p style="color: #7f8c8d; font-size: 14px;">Total: <span id="productCount">0</span> produk</p>
                </div>
                <button class="btn btn-success" onclick="showAddProductModal()">
                    ➕ Tambah Produk
                </button>
            </div>
            
            <div class="products-grid" id="productGridManage">
                <!-- Produk akan diisi oleh JavaScript -->
            </div>
        </div>
        
        <!-- LAPORAN -->
        <div id="laporan" class="tab-content">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
                <h2>📈 Laporan Penjualan</h2>
                <div>
                    <button class="btn btn-csv" onclick="exportReportCSV()">
                        📥 Export CSV
                    </button>
                    <button class="btn btn-excel" onclick="exportReportExcel()">
                        📊 Export Excel
                    </button>
                </div>
            </div>
            
            <!-- FILTER LAPORAN -->
            <div class="search-container">
                <h3>🔍 Filter Laporan</h3>
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px;">
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Periode</label>
                        <select id="reportPeriod" class="form-control" onchange="toggleCustomDate()">
                            <option value="today">Hari Ini</option>
                            <option value="yesterday">Kemarin</option>
                            <option value="week">Minggu Ini</option>
                            <option value="month">Bulan Ini</option>
                            <option value="year">Tahun Ini</option>
                            <option value="custom">Custom Range</option>
                        </select>
                    </div>
                    
                    <div id="customDateRange" style="display: none; grid-column: span 2;">
                        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                            <div>
                                <label style="display: block; margin-bottom: 8px; font-weight: 600;">Dari Tanggal</label>
                                <input type="date" id="startDate" class="form-control">
                            </div>
                            <div>
                                <label style="display: block; margin-bottom: 8px; font-weight: 600;">Sampai Tanggal</label>
                                <input type="date" id="endDate" class="form-control">
                            </div>
                        </div>
                    </div>
                    
                    <div style="align-self: end;">
                        <button class="btn btn-primary" onclick="generateReport()" style="width: 100%;">
                            🔍 Generate Laporan
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- STATISTIK LAPORAN -->
            <div class="stats-grid" style="margin: 30px 0;">
                <div class="stat-card">
                    <div style="font-size: 28px; font-weight: bold; color: #27ae60;" id="reportTotalSales">Rp 0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 5px;">Total Penjualan</div>
                </div>
                <div class="stat-card">
                    <div style="font-size: 28px; font-weight: bold; color: #e74c3c;" id="reportTotalTransactions">0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 5px;">Jumlah Transaksi</div>
                </div>
                <div class="stat-card">
                    <div style="font-size: 28px; font-weight: bold; color: #3498db;" id="reportAvgTransaction">Rp 0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 5px;">Rata-rata per Transaksi</div>
                </div>
                <div class="stat-card">
                    <div style="font-size: 28px; font-weight: bold; color: #f39c12;" id="reportTotalItems">0</div>
                    <div style="color: #7f8c8d; font-size: 14px; margin-top: 5px;">Item Terjual</div>
                </div>
            </div>
            
            <!-- TABEL LAPORAN -->
            <div style="overflow-x: auto;">
                <table class="data-table">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Tanggal</th>
                            <th>ID Transaksi</th>
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
        
        <!-- EXPORT DATA -->
        <div id="export" class="tab-content">
            <h2>📥 Export Data</h2>
            <p style="color: #7f8c8d; margin-bottom: 30px;">Export data dalam format CSV atau Excel untuk backup dan analisis</p>
            
            <div class="export-options">
                <div class="export-card" onclick="exportData('penjualan')">
                    <div class="export-icon">💰</div>
                    <div class="export-label">Laporan Penjualan</div>
                    <div class="export-desc">Data penjualan harian dalam CSV</div>
                </div>
                
                <div class="export-card" onclick="exportData('produk')">
                    <div class="export-icon">📦</div>
                    <div class="export-label">Data Produk</div>
                    <div class="export-desc">Master data semua produk</div>
                </div>
                
                <div class="export-card" onclick="exportData('stok')">
                    <div class="export-icon">📊</div>
                    <div class="export-label">Laporan Stok</div>
                    <div class="export-desc">Stok barang & alert stok rendah</div>
                </div>
                
                <div class="export-card" onclick="exportData('transaksi')">
                    <div class="export-icon">🧾</div>
                    <div class="export-label">Transaksi Detail</div>
                    <div class="export-desc">Detail semua transaksi lengkap</div>
                </div>
            </div>
            
            <!-- EXPORT CUSTOM PERIODE -->
            <div style="background: #f8f9fa; padding: 25px; border-radius: 12px; margin-top: 40px;">
                <h3>📅 Export Custom Periode</h3>
                <p style="color: #7f8c8d; margin-bottom: 20px;">Export data berdasarkan rentang tanggal tertentu</p>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px;">
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Dari Tanggal</label>
                        <input type="date" id="exportStartDate" class="form-control">
                    </div>
                    <div>
                        <label style="display: block; margin-bottom: 8px; font-weight: 600;">Sampai Tanggal</label>
                        <input type="date" id="exportEndDate" class="form-control">
                    </div>
                    <div style="align-self: end;">
                        <button class="btn btn-csv" onclick="exportCustomPeriod('csv')" style="width: 100%;">
                            📥 Export CSV
                        </button>
                    </div>
                    <div style="align-self: end;">
                        <button class="btn btn-excel" onclick="exportCustomPeriod('excel')" style="width: 100%;">
                            📊 Export Excel
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- EXPORT SEMUA DATA -->
            <div style="margin-top: 40px; text-align: center;">
                <h3>📦 Export Semua Data</h3>
                <p style="color: #7f8c8d; margin-bottom: 20px;">Export semua data dalam satu file untuk backup lengkap</p>
                <button class="btn btn-success" onclick="exportAllData()" style="padding: 15px 40px; font-size: 16px;">
                    💾 Export Semua Data (CSV)
                </button>
            </div>
        </div>
    </main>

    <!-- MODAL TAMBAH PRODUK -->
    <div id="productModal" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 2000; justify-content: center; align-items: center;">
        <div style="background: white; width: 90%; max-width: 500px; border-radius: 15px; padding: 30px;">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
                <h3 id="modalTitle">Tambah Produk Baru</h3>
                <button onclick="closeModal()" style="background: none; border: none; font-size: 28px; cursor: pointer; color: #7f8c8d;">&times;</button>
            </div>
            
            <div class="form-group">
                <label style="display: block; margin-bottom: 8px; font-weight: 600;">Nama Produk</label>
                <input type="text" id="modalProductName" class="form-control" placeholder="Contoh: Beras 5kg">
            </div>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                <div class="form-group">
                    <label style="display: block; margin-bottom: 8px; font-weight: 600;">Harga Jual</label>
                    <input type="number" id="modalProductPrice" class="form-control" placeholder="0">
                </div>
                
                <div class="form-group">
                    <label style="display: block; margin-bottom: 8px; font-weight: 600;">Stok Awal</label>
                    <input type="number" id="modalProductStock" class="form-control" placeholder="0">
                </div>
            </div>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                <div class="form-group">
                    <label style="display: block; margin-bottom: 8px; font-weight: 600;">Kategori</label>
                    <select id="modalProductCategory" class="form-control">
                        <option value="Sembako">Sembako</option>
                        <option value="Minuman">Minuman</option>
                        <option value="Snack">Snack</option>
                        <option value="Kebersihan">Kebersihan</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label style="display: block; margin-bottom: 8px; font-weight: 600;">Satuan</label>
                    <select id="modalProductUnit" class="form-control">
                        <option value="pcs">pcs</option>
                        <option value="kg">kg</option>
                        <option value="liter">liter</option>
                        <option value="pack">pack</option>
                        <option value="dus">dus</option>
                    </select>
                </div>
            </div>
            
            <div style="display: flex; gap: 15px; margin-top: 30px;">
                <button class="btn btn-danger" onclick="closeModal()" style="flex: 1;">
                    Batal
                </button>
                <button class="btn btn-success" onclick="saveProduct()" style="flex: 2;">
                    💾 Simpan Produk
                </button>
            </div>
        </div>
    </div>

    <!-- SCRIPT -->
    <script>
        // ==================== INISIALISASI ====================
        console.log('Aplikasi Kasir Jennaira Group Loading...');
        
        // Data aplikasi
        let products = [];
        let cart = [];
        let transactions = [];
        let currentProductId = null;
        
        // Load data saat halaman dimuat
        document.addEventListener('DOMContentLoaded', function() {
            loadData();
            initializeApp();
            setupEventListeners();
        });
        
        // ==================== DATA MANAGEMENT ====================
        function loadData() {
            try {
                // Load dari localStorage
                products = JSON.parse(localStorage.getItem('kasir_products')) || getDefaultProducts();
                transactions = JSON.parse(localStorage.getItem('kasir_transactions')) || [];
                
                console.log('Data loaded:', {
                    products: products.length,
                    transactions: transactions.length
                });
            } catch (error) {
                console.error('Error loading data:', error);
                products = getDefaultProducts();
                transactions = [];
            }
        }
        
        function getDefaultProducts() {
            return [
                { id: 1, name: 'Beras 5kg', price: 60000, cost: 50000, stock: 45, minStock: 10, category: 'Sembako', unit: 'pack', sold: 125 },
                { id: 2, name: 'Minyak Goreng 2L', price: 35000, cost: 28000, stock: 28, minStock: 10, category: 'Sembako', unit: 'botol', sold: 89 },
                { id: 3, name: 'Gula Pasir 1kg', price: 15000, cost: 12000, stock: 12, minStock: 10, category: 'Sembako', unit: 'kg', sold: 156 },
                { id: 4, name: 'Telur 1kg', price: 28000, cost: 22000, stock: 8, minStock: 10, category: 'Sembako', unit: 'kg', sold: 67 },
                { id: 5, name: 'Sabun Mandi', price: 5000, cost: 3500, stock: 120, minStock: 20, category: 'Kebersihan', unit: 'pcs', sold: 234 },
                { id: 6, name: 'Shampoo', price: 12000, cost: 9000, stock: 35, minStock: 10, category: 'Kebersihan', unit: 'botol', sold: 78 },
                { id: 7, name: 'Air Mineral 600ml', price: 3000, cost: 2000, stock: 150, minStock: 30, category: 'Minuman', unit: 'botol', sold: 345 },
                { id: 8, name: 'Kopi Sachet', price: 2000, cost: 1500, stock: 89, minStock: 20, category: 'Minuman', unit: 'pcs', sold: 289 },
                { id: 9, name: 'Teh Celup', price: 1500, cost: 1000, stock: 65, minStock: 20, category: 'Minuman', unit: 'pcs', sold: 167 },
                { id: 10, name: 'Biskuit', price: 8000, cost: 6000, stock: 42, minStock: 15, category: 'Snack', unit: 'pack', sold: 134 }
            ];
        }
        
        function saveData() {
            try {
                localStorage.setItem('kasir_products', JSON.stringify(products));
                localStorage.setItem('kasir_transactions', JSON.stringify(transactions));
                console.log('Data saved');
            } catch (error) {
                console.error('Save error:', error);
                showAlert('error', 'Gagal menyimpan data: ' + error.message);
            }
        }
        
        // ==================== INITIALIZE APP ====================
        function initializeApp() {
            // Set default dates
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('exportStartDate').value = today;
            document.getElementById('exportEndDate').value = today;
            document.getElementById('startDate').value = today;
            document.getElementById('endDate').value = today;
            
            // Load initial data
            loadDashboard();
            loadProductsForKasir();
            loadProductsForManage();
            generateReport();
            
            console.log('Aplikasi initialized');
        }
        
        function setupEventListeners() {
            // Payment input listener
            document.getElementById('paymentAmount').addEventListener('input', updateChangeCalculation);
            
            // Search input listeners
            document.getElementById('productSearch').addEventListener('input', filterProductsKasir);
            document.getElementById('searchName').addEventListener('input', filterProducts);
            
            // Report period listener
            document.getElementById('reportPeriod').addEventListener('change', toggleCustomDate);
        }
        
        // ==================== NAVIGASI ====================
        function showTab(tabId) {
            // Update active button
            document.querySelectorAll('.nav-tab').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Show selected tab
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.getElementById(tabId).classList.add('active');
            
            // Load tab-specific data
            if (tabId === 'dashboard') {
                loadDashboard();
            } else if (tabId === 'kasir') {
                loadProductsForKasir();
                updateCartDisplay();
            } else if (tabId === 'produk') {
                loadProductsForManage();
            } else if (tabId === 'laporan') {
                generateReport();
            }
        }
        
        // ==================== DASHBOARD ====================
        function loadDashboard() {
            const today = new Date().toLocaleDateString('id-ID');
            let salesToday = 0;
            let transactionsToday = 0;
            let monthlyProfit = 0;
            
            // Calculate today's stats
            transactions.forEach(trans => {
                if (trans.date === today) {
                    salesToday += trans.total;
                    transactionsToday++;
                }
                
                // Calculate profit (simplified)
                const profit = trans.items.reduce((sum, item) => {
                    const product = products.find(p => p.name === item.name);
                    return sum + ((item.price - (product?.cost || item.price * 0.8)) * item.quantity);
                }, 0);
                monthlyProfit += profit;
            });
            
            // Update dashboard
            document.getElementById('salesToday').textContent = formatCurrency(salesToday);
            document.getElementById('totalTransactions').textContent = transactionsToday;
            document.getElementById('totalProducts').textContent = products.length;
            document.getElementById('monthlyProfit').textContent = formatCurrency(monthlyProfit);
        }
        
        function refreshDashboard() {
            loadDashboard();
            showAlert('success', 'Dashboard diperbarui');
        }
        
        // ==================== KASIR ====================
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
                    <div style="font-size: 12px; color: #7f8c8d; margin: 5px 0;">
                        ${product.category} • ${product.unit}
                    </div>
                    <div class="product-stock">Stok: ${product.stock}</div>
                    <div class="stock-badge ${stockClass}">${stockBadge}</div>
                `;
                productCard.onclick = () => addToCart(product);
                container.appendChild(productCard);
            });
        }
        
        function filterProductsKasir() {
            const searchTerm = document.getElementById('productSearch').value.toLowerCase();
            const container = document.getElementById('productGridKasir');
            const countSpan = document.getElementById('searchCount');
            
            if (!container) return;
            
            const filtered = products.filter(product => 
                product.name.toLowerCase().includes(searchTerm) ||
                product.category.toLowerCase().includes(searchTerm) ||
                product.price.toString().includes(searchTerm)
            );
            
            container.innerHTML = '';
            
            filtered.forEach(product => {
                const stockClass = getStockClass(product.stock, product.minStock);
                const stockBadge = getStockBadge(product.stock, product.minStock);
                
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                productCard.innerHTML = `
                    <div class="product-name">${product.name}</div>
                    <div class="product-price">${formatCurrency(product.price)}</div>
                    <div style="font-size: 12px; color: #7f8c8d; margin: 5px 0;">
                        ${product.category} • ${product.unit}
                    </div>
                    <div class="product-stock">Stok: ${product.stock}</div>
                    <div class="stock-badge ${stockClass}">${stockBadge}</div>
                `;
                productCard.onclick = () => addToCart(product);
                container.appendChild(productCard);
            });
            
            countSpan.textContent = `Menampilkan ${filtered.length} dari ${products.length} produk`;
        }
        
        function addToCart(product) {
            if (product.stock <= 0) {
                showAlert('error', 'Stok produk habis!');
                return;
            }
            
            const existingItem = cart.find(item => item.id === product.id);
            
            if (existingItem) {
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
            const paymentInput = document.getElementById('paymentAmount');
            
            if (cart.length === 0) {
                container.innerHTML = `
                    <div style="text-align: center; padding: 40px; color: #7f8c8d;">
                        <div style="font-size: 60px; margin-bottom: 20px;">🛒</div>
                        <h4>Keranjang Kosong</h4>
                        <p>Pilih produk dari daftar di bawah</p>
                    </div>
                `;
                summary.style.display = 'none';
                return;
            }
            
            let html = '';
            let subtotal = 0;
            
            cart.forEach((item, index) => {
                const itemTotal = item.price * item.quantity;
                subtotal += itemTotal;
                
                html += `
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
            
            const tax = subtotal * 0.1; // Pajak 10%
            const total = subtotal + tax;
            
            container.innerHTML = html;
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
            const tax = subtotal * 0.1; // Pajak 10%
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
            
            // Create transaction
            const transactionId = 'TRX-' + Date.now();
            const date = new Date().toLocaleDateString('id-ID');
            const time = new Date().toLocaleTimeString('id-ID');
            
            const transaction = {
                id: transactionId,
                date: date,
                time: time,
                items: [...cart],
                subtotal: cart.reduce((sum, item) => sum + (item.price * item.quantity), 0),
                tax: 10,
                total: total,
                payment: payment,
                change: payment - total,
                kasir: 'Admin'
            };
            
            // Update stock
            cart.forEach(cartItem => {
                const product = products.find(p => p.id === cartItem.id);
                if (product) {
                    product.stock -= cartItem.quantity;
                    product.sold = (product.sold || 0) + cartItem.quantity;
                }
            });
            
            // Save transaction
            transactions.unshift(transaction);
            saveData();
            
            // Show success
            const change = payment - total;
            showAlert('success', `
                Pembayaran berhasil!<br>
                Total: ${formatCurrency(total)}<br>
                Bayar: ${formatCurrency(payment)}<br>
                Kembali: ${formatCurrency(change)}
            `);
            
            // Reset cart
            cart = [];
            paymentInput.value = '';
            updateCartDisplay();
            
            // Refresh data
            loadDashboard();
            loadProductsForKasir();
            loadProductsForManage();
        }
        
        function printReceipt() {
            if (cart.length === 0) {
                showAlert('error', 'Tidak ada transaksi untuk dicetak');
                return;
            }
            
            const receiptContent = `
                <div style="font-family: 'Courier New', monospace; width: 80mm; padding: 10px;">
                    <div style="text-align: center;">
                        <h3>JENNAIRA GROUP</h3>
                        <p>Jl. Contoh No. 123</p>
                        <p>Telp: (021) 1234-5678</p>
                        <hr>
                        <p>${new Date().toLocaleString('id-ID')}</p>
                        <p>No: TRX-${Date.now()}</p>
                        <hr>
                    </div>
                    
                    <div style="margin: 10px 0;">
                        ${cart.map(item => `
                            <div style="display: flex; justify-content: space-between; margin: 3px 0;">
                                <div>${item.name} x${item.quantity}</div>
                                <div>Rp ${(item.price * item.quantity).toLocaleString('id-ID')}</div>
                            </div>
                        `).join('')}
                    </div>
                    
                    <hr>
                    <div style="display: flex; justify-content: space-between; margin: 5px 0;">
                        <div>Subtotal:</div>
                        <div>${formatCurrency(cart.reduce((sum, item) => sum + (item.price * item.quantity), 0))}</div>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin: 5px 0;">
                        <div>Pajak (10%):</div>
                        <div>${formatCurrency(cart.reduce((sum, item) => sum + (item.price * item.quantity), 0) * 0.1)}</div>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin: 5px 0; font-weight: bold;">
                        <div>TOTAL:</div>
                        <div>${formatCurrency(getCartTotal())}</div>
                    </div>
                    
                    <div style="text-align: center; margin-top: 20px;">
                        <p>Terima kasih atas kunjungan Anda</p>
                    </div>
                </div>
            `;
            
            const printWindow = window.open('', '_blank');
            printWindow.document.write(`
                <!DOCTYPE html>
                <html>
                <head>
                    <title>Struk</title>
                    <style>
                        @media print {
                            @page { margin: 0; size: 80mm auto; }
                            body { margin: 0; padding: 0; }
                        }
                        body { font-family: 'Courier New', monospace; }
                    </style>
                </head>
                <body>${receiptContent}</body>
                </html>
            `);
            printWindow.document.close();
            printWindow.print();
        }
        
        // ==================== PRODUK MANAGEMENT ====================
        function loadProductsForManage() {
            filterProducts();
        }
        
        function filterProducts() {
            const name = document.getElementById('searchName').value.toLowerCase();
            const category = document.getElementById('searchCategory').value;
            const stock = document.getElementById('searchStock').value;
            const container = document.getElementById('productGridManage');
            const countSpan = document.getElementById('productCount');
            
            if (!container) return;
            
            const filtered = products.filter(product => {
                // Name filter
                if (name && !product.name.toLowerCase().includes(name)) return false;
                
                // Category filter
                if (category && product.category !== category) return false;
                
                // Stock filter
                if (stock) {
                    if (stock === 'low' && product.stock >= product.minStock) return false;
                    if (stock === 'medium' && (product.stock < product.minStock || product.stock > 50)) return false;
                    if (stock === 'high' && product.stock <= 50) return false;
                }
                
                return true;
            });
            
            container.innerHTML = '';
            
            filtered.forEach(product => {
                const stockClass = getStockClass(product.stock, product.minStock);
                const stockBadge = getStockBadge(product.stock, product.minStock);
                
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                productCard.innerHTML = `
                    <div class="product-name">${product.name}</div>
                    <div class="product-price">${formatCurrency(product.price)}</div>
                    <div style="font-size: 12px; color: #7f8c8d; margin: 5px 0;">
                        ${product.category} • ${product.unit}
                    </div>
                    <div class="product-stock">Stok: ${product.stock} (Min: ${product.minStock})</div>
                    <div style="font-size: 11px; color: #7f8c8d; margin-top: 5px;">
                        Terjual: ${product.sold || 0}
                    </div>
                    <div class="stock-badge ${stockClass}">${stockBadge}</div>
                    <div style="display: flex; gap: 10px; margin-top: 15px;">
                        <button onclick="editProduct(${product.id})" style="flex: 1; padding: 8px; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer;">
                            ✏️ Edit
                        </button>
                        <button onclick="deleteProduct(${product.id})" style="flex: 1; padding: 8px; background: #e74c3c; color: white; border: none; border-radius: 5px; cursor: pointer;">
                            🗑️ Hapus
                        </button>
                    </div>
                `;
                container.appendChild(productCard);
            });
            
            countSpan.textContent = filtered.length;
        }
        
        function showAddProductModal() {
            currentProductId = null;
            document.getElementById('modalTitle').textContent = 'Tambah Produk Baru';
            document.getElementById('modalProductName').value = '';
            document.getElementById('modalProductPrice').value = '';
            document.getElementById('modalProductStock').value = '';
            document.getElementById('modalProductCategory').value = 'Sembako';
            document.getElementById('modalProductUnit').value = 'pcs';
            document.getElementById('productModal').style.display = 'flex';
        }
        
        function editProduct(id) {
            const product = products.find(p => p.id === id);
            if (!product) return;
            
            currentProductId = id;
            document.getElementById('modalTitle').textContent = 'Edit Produk';
            document.getElementById('modalProductName').value = product.name;
            document.getElementById('modalProductPrice').value = product.price;
            document.getElementById('modalProductStock').value = product.stock;
            document.getElementById('modalProductCategory').value = product.category;
            document.getElementById('modalProductUnit').value = product.unit || 'pcs';
            document.getElementById('productModal').style.display = 'flex';
        }
        
        function closeModal() {
            document.getElementById('productModal').style.display = 'none';
            currentProductId = null;
        }
        
        function saveProduct() {
            const name = document.getElementById('modalProductName').value.trim();
            const price = parseFloat(document.getElementById('modalProductPrice').value);
            const stock = parseInt(document.getElementById('modalProductStock').value);
            const category = document.getElementById('modalProductCategory').value;
            const unit = document.getElementById('modalProductUnit').value;
            
            if (!name || !price || !stock) {
                showAlert('error', 'Harap isi semua field dengan benar!');
                return;
            }
            
            if (currentProductId) {
                // Update product
                const product = products.find(p => p.id === currentProductId);
                if (product) {
                    product.name = name;
                    product.price = price;
                    product.stock = stock;
                    product.category = category;
                    product.unit = unit;
                }
                showAlert('success', 'Produk berhasil diupdate!');
            } else {
                // Add new product
                const newId = products.length > 0 ? Math.max(...products.map(p => p.id)) + 1 : 1;
                products.push({
                    id: newId,
                    name: name,
                    price: price,
                    cost: price * 0.8, // Default cost
                    stock: stock,
                    minStock: category === 'Sembako' ? 10 : category === 'Kebersihan' ? 20 : 15,
                    category: category,
                    unit: unit,
                    sold: 0
                });
                showAlert('success', 'Produk baru berhasil ditambahkan!');
            }
            
            saveData();
            closeModal();
            loadProductsForKasir();
            loadProductsForManage();
            loadDashboard();
        }
        
        function deleteProduct(id) {
            if (!confirm('Apakah Anda yakin ingin menghapus produk ini?')) return;
            
            const index = products.findIndex(p => p.id === id);
            if (index !== -1) {
                products.splice(index, 1);
                saveData();
                showAlert('success', 'Produk berhasil dihapus!');
                loadProductsForKasir();
                loadProductsForManage();
                loadDashboard();
            }
        }
        
        // ==================== LAPORAN ====================
        function toggleCustomDate() {
            const period = document.getElementById('reportPeriod').value;
            const customRange = document.getElementById('customDateRange');
            customRange.style.display = period === 'custom' ? 'grid' : 'none';
        }
        
        function generateReport() {
            const period = document.getElementById('reportPeriod').value;
            let filteredTransactions = [];
            
            const today = new Date();
            const yesterday = new Date(today);
            yesterday.setDate(yesterday.getDate() - 1);
            
            switch (period) {
                case 'today':
                    filteredTransactions = transactions.filter(t => t.date === today.toLocaleDateString('id-ID'));
                    break;
                case 'yesterday':
                    filteredTransactions = transactions.filter(t => t.date === yesterday.toLocaleDateString('id-ID'));
                    break;
                case 'week':
                    const weekAgo = new Date(today);
                    weekAgo.setDate(weekAgo.getDate() - 7);
                    filteredTransactions = transactions.filter(t => {
                        const transDate = new Date(t.date.split('/').reverse().join('-'));
                        return transDate >= weekAgo && transDate <= today;
                    });
                    break;
                case 'month':
                    const monthAgo = new Date(today);
                    monthAgo.setMonth(monthAgo.getMonth() - 1);
                    filteredTransactions = transactions.filter(t => {
                        const transDate = new Date(t.date.split('/').reverse().join('-'));
                        return transDate >= monthAgo && transDate <= today;
                    });
                    break;
                case 'year':
                    const yearAgo = new Date(today);
                    yearAgo.setFullYear(yearAgo.getFullYear() - 1);
                    filteredTransactions = transactions.filter(t => {
                        const transDate = new Date(t.date.split('/').reverse().join('-'));
                        return transDate >= yearAgo && transDate <= today;
                    });
                    break;
                case 'custom':
                    const startDate = document.getElementById('startDate').value;
                    const endDate = document.getElementById('endDate').value;
                    if (startDate && endDate) {
                        const start = new Date(startDate);
                        const end = new Date(endDate);
                        filteredTransactions = transactions.filter(t => {
                            const transDate = new Date(t.date.split('/').reverse().join('-'));
                            return transDate >= start && transDate <= end;
                        });
                    }
                    break;
                default:
                    filteredTransactions = transactions;
            }
            
            // Calculate report statistics
            let totalSales = 0;
            let totalItems = 0;
            
            filteredTransactions.forEach(trans => {
                totalSales += trans.total;
                totalItems += trans.items.reduce((sum, item) => sum + item.quantity, 0);
            });
            
            const avgTransaction = filteredTransactions.length > 0 ? totalSales / filteredTransactions.length : 0;
            
            // Update report cards
            document.getElementById('reportTotalSales').textContent = formatCurrency(totalSales);
            document.getElementById('reportTotalTransactions').textContent = filteredTransactions.length;
            document.getElementById('reportAvgTransaction').textContent = formatCurrency(avgTransaction);
            document.getElementById('reportTotalItems').textContent = totalItems;
            
            // Update report table
            const tableBody = document.getElementById('reportTable');
            tableBody.innerHTML = '';
            
            filteredTransactions.forEach((trans, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${trans.date}<br><small>${trans.time}</small></td>
                    <td>${trans.id}</td>
                    <td>${trans.items.map(item => `${item.name} (${item.quantity}x)`).join('<br>')}</td>
                    <td>${trans.items.reduce((sum, item) => sum + item.quantity, 0)}</td>
                    <td>${formatCurrency(trans.total)}</td>
                    <td>${trans.kasir || 'Admin'}</td>
                `;
                tableBody.appendChild(row);
            });
            
            if (filteredTransactions.length === 0) {
                tableBody.innerHTML = '<tr><td colspan="7" style="text-align: center; padding: 40px; color: #7f8c8d;">Tidak ada transaksi dalam periode ini</td></tr>';
            }
        }
        
        // ==================== EXPORT FUNCTIONS ====================
        function exportData(type) {
            switch(type) {
                case 'penjualan':
                    exportSalesCSV();
                    break;
                case 'produk':
                    exportProductsCSV();
                    break;
                case 'stok':
                    exportStockCSV();
                    break;
                case 'transaksi':
                    exportTransactionsCSV();
                    break;
            }
        }
        
        function exportSalesCSV() {
            const today = new Date().toLocaleDateString('id-ID');
            const todayTransactions = transactions.filter(t => t.date === today);
            
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "LAPORAN PENJUALAN HARIAN\r\n";
            csvContent += "Tanggal," + today + "\r\n";
            csvContent += "Total Transaksi," + todayTransactions.length + "\r\n\r\n";
            csvContent += "No,Tanggal,Waktu,ID Transaksi,Items,Total,Bayar,Kembali,Kasir\r\n";
            
            todayTransactions.forEach((trans, index) => {
                const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join('; ');
                csvContent += `${index + 1},${trans.date},${trans.time},${trans.id},"${items}",${trans.total},${trans.payment},${trans.change},${trans.kasir || 'Admin'}\r\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Laporan_Penjualan_${today.replace(/\//g, '-')}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Laporan penjualan berhasil diexport ke CSV');
        }
        
        function exportProductsCSV() {
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "DATA MASTER PRODUK\r\n";
            csvContent += "Tanggal Export," + new Date().toLocaleDateString('id-ID') + "\r\n";
            csvContent += "Total Produk," + products.length + "\r\n\r\n";
            csvContent += "Kode,Nama Produk,Kategori,Satuan,Harga Jual,Harga Beli,Stok,Stok Minimal,Status,Terjual\r\n";
            
            products.forEach((product, index) => {
                const code = 'PRD' + (index + 1).toString().padStart(3, '0');
                const status = product.stock < product.minStock ? 'STOK RENDAH' : 
                              product.stock < product.minStock * 2 ? 'STOK SEDANG' : 'STOK AMAN';
                csvContent += `${code},"${product.name}",${product.category},${product.unit || 'pcs'},${product.price},${product.cost || product.price * 0.8},${product.stock},${product.minStock},${status},${product.sold || 0}\r\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Data_Produk_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Data produk berhasil diexport ke CSV');
        }
        
        function exportStockCSV() {
            const lowStockProducts = products.filter(p => p.stock < p.minStock);
            
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "LAPORAN STOK BARANG\r\n";
            csvContent += "Tanggal," + new Date().toLocaleDateString('id-ID') + "\r\n";
            csvContent += "Total Produk," + products.length + "\r\n";
            csvContent += "Stok Rendah," + lowStockProducts.length + "\r\n\r\n";
            csvContent += "No,Nama Produk,Kategori,Stok Saat Ini,Stok Minimal,Selisih,Status\r\n";
            
            products.forEach((product, index) => {
                const difference = product.stock - product.minStock;
                const status = difference < 0 ? 'PERLU RESTOCK' : 
                              difference < 5 ? 'HATI-HATI' : 'AMAN';
                csvContent += `${index + 1},"${product.name}",${product.category},${product.stock},${product.minStock},${difference},${status}\r\n`;
            });
            
            if (lowStockProducts.length > 0) {
                csvContent += "\r\nPRODUK YANG PERLU RESTOCK:\r\n";
                csvContent += "Nama Produk,Kategori,Stok Saat Ini,Stok Minimal,Kekurangan\r\n";
                lowStockProducts.forEach(product => {
                    csvContent += `"${product.name}",${product.category},${product.stock},${product.minStock},${product.stock - product.minStock}\r\n`;
                });
            }
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Laporan_Stok_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Laporan stok berhasil diexport ke CSV');
        }
        
        function exportTransactionsCSV() {
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "LAPORAN TRANSAKSI DETAIL\r\n";
            csvContent += "Tanggal Export," + new Date().toLocaleDateString('id-ID') + "\r\n";
            csvContent += "Total Transaksi," + transactions.length + "\r\n\r\n";
            csvContent += "No,Tanggal,Waktu,ID Transaksi,Items,Subtotal,Pajak,Total,Bayar,Kembali,Kasir\r\n";
            
            transactions.forEach((trans, index) => {
                const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join('; ');
                csvContent += `${index + 1},${trans.date},${trans.time},${trans.id},"${items}",${trans.subtotal},${trans.tax || 10},${trans.total},${trans.payment},${trans.change},${trans.kasir || 'Admin'}\r\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Transaksi_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Data transaksi berhasil diexport ke CSV');
        }
        
        function exportReportCSV() {
            exportSalesCSV();
        }
        
        function exportReportExcel() {
            // For Excel, we'll create a more complex CSV that Excel can open
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "LAPORAN PENJUALAN EXCEL FORMAT\r\n";
            csvContent += "Dibuat," + new Date().toLocaleString('id-ID') + "\r\n";
            csvContent += "Periode," + document.getElementById('reportPeriod').value + "\r\n";
            csvContent += "Total Transaksi," + transactions.length + "\r\n";
            csvContent += "Total Penjualan,Rp " + transactions.reduce((sum, t) => sum + t.total, 0) + "\r\n\r\n";
            
            csvContent += "DETAIL TRANSAKSI\r\n";
            csvContent += "No,Tanggal,Waktu,ID Transaksi,Nama Produk,Quantity,Harga Satuan,Subtotal,Pajak,Total,Bayar,Kembali,Kasir\r\n";
            
            let rowNumber = 1;
            transactions.forEach(trans => {
                trans.items.forEach(item => {
                    const subtotal = item.price * item.quantity;
                    const tax = subtotal * 0.1;
                    const total = subtotal + tax;
                    csvContent += `${rowNumber},${trans.date},${trans.time},${trans.id},"${item.name}",${item.quantity},${item.price},${subtotal},${tax},${total},${trans.payment},${trans.change},${trans.kasir || 'Admin'}\r\n`;
                    rowNumber++;
                });
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Laporan_Excel_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Laporan berhasil diexport dalam format Excel (CSV)');
        }
        
        function exportCustomPeriod(format) {
            const startDate = document.getElementById('exportStartDate').value;
            const endDate = document.getElementById('exportEndDate').value;
            
            if (!startDate || !endDate) {
                showAlert('error', 'Pilih rentang tanggal terlebih dahulu');
                return;
            }
            
            const start = new Date(startDate);
            const end = new Date(endDate);
            
            const filteredTransactions = transactions.filter(t => {
                const transDate = new Date(t.date.split('/').reverse().join('-'));
                return transDate >= start && transDate <= end;
            });
            
            if (filteredTransactions.length === 0) {
                showAlert('error', 'Tidak ada transaksi dalam periode tersebut');
                return;
            }
            
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += `LAPORAN PERIODE ${startDate} s/d ${endDate}\r\n`;
            csvContent += "Dibuat," + new Date().toLocaleString('id-ID') + "\r\n";
            csvContent += "Jumlah Transaksi," + filteredTransactions.length + "\r\n";
            csvContent += "Total Penjualan,Rp " + filteredTransactions.reduce((sum, t) => sum + t.total, 0) + "\r\n\r\n";
            
            if (format === 'csv') {
                csvContent += "No,Tanggal,ID Transaksi,Items,Total,Bayar,Kembali,Kasir\r\n";
                filteredTransactions.forEach((trans, index) => {
                    const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join('; ');
                    csvContent += `${index + 1},${trans.date},${trans.id},"${items}",${trans.total},${trans.payment},${trans.change},${trans.kasir || 'Admin'}\r\n`;
                });
            } else {
                csvContent += "No,Tanggal,Waktu,ID Transaksi,Nama Produk,Quantity,Harga,Subtotal,Total\r\n";
                let rowNumber = 1;
                filteredTransactions.forEach(trans => {
                    trans.items.forEach(item => {
                        csvContent += `${rowNumber},${trans.date},${trans.time},${trans.id},"${item.name}",${item.quantity},${item.price},${item.price * item.quantity},${trans.total}\r\n`;
                        rowNumber++;
                    });
                });
            }
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            const filename = format === 'csv' ? 'CSV' : 'Excel';
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Laporan_${startDate}_${endDate}_${filename}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', `Laporan custom periode berhasil diexport ke ${filename}`);
        }
        
        function exportAllData() {
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "BACKUP DATA LENGKAP KASIR\r\n";
            csvContent += "Dibuat," + new Date().toLocaleString('id-ID') + "\r\n";
            csvContent += "Total Produk," + products.length + "\r\n";
            csvContent += "Total Transaksi," + transactions.length + "\r\n\r\n";
            
            // Section 1: Produk
            csvContent += "DATA PRODUK\r\n";
            csvContent += "No,Nama,Kategori,Satuan,Harga Jual,Harga Beli,Stok,Stok Minimal,Terjual\r\n";
            products.forEach((p, i) => {
                csvContent += `${i + 1},"${p.name}",${p.category},${p.unit || 'pcs'},${p.price},${p.cost || p.price * 0.8},${p.stock},${p.minStock},${p.sold || 0}\r\n`;
            });
            
            csvContent += "\r\n\r\nDATA TRANSAKSI\r\n";
            csvContent += "No,Tanggal,Waktu,ID Transaksi,Items,Total,Bayar,Kembali\r\n";
            transactions.forEach((t, i) => {
                const items = t.items.map(item => `${item.name} (${item.quantity}x)`).join('; ');
                csvContent += `${i + 1},${t.date},${t.time},${t.id},"${items}",${t.total},${t.payment},${t.change}\r\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Backup_Kasir_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('success', 'Semua data berhasil diexport ke CSV untuk backup');
        }
        
        // ==================== UTILITY FUNCTIONS ====================
        function formatCurrency(amount) {
            return 'Rp ' + parseInt(amount).toLocaleString('id-ID');
        }
        
        function getStockClass(stock, minStock) {
            if (stock < minStock) return 'stock-low';
            if (stock < minStock * 3) return 'stock-medium';
            return 'stock-high';
        }
        
        function getStockBadge(stock, minStock) {
            if (stock < minStock) return 'RENDAH';
            if (stock < minStock * 3) return 'SEDANG';
            return 'AMAN';
        }
        
        function showAlert(type, message) {
            const alertDiv = document.getElementById('alert');
            alertDiv.innerHTML = message;
            alertDiv.className = `alert alert-${type}`;
            alertDiv.style.display = 'block';
            
            setTimeout(() => {
                alertDiv.style.display = 'none';
            }, 5000);
        }
    </script>
</body>
</html>
