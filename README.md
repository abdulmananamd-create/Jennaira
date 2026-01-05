<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Kasir Lengkap - GitHub Pages Fixed</title>
    <!-- ANTI CACHE -->
    <meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
    <meta http-equiv="Pragma" content="no-cache">
    <meta http-equiv="Expires" content="0">
    
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

    <!-- APLIKASI KASIR -->
    <div class="app-container">
        <!-- HEADER -->
        <header class="app-header">
            <div class="logo">
                <div class="logo-icon">💰</div>
                <div>
                    <h1 style="font-size: 28px; margin-bottom: 5px;">KASIR PRO</h1>
                    <p style="opacity: 0.9; font-size: 14px;">Sistem Kasir Lengkap untuk UMKM</p>
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
            <button class="nav-tab" onclick="showTab('settings')">
                ⚙️ Pengaturan
            </button>
        </nav>

        <!-- CONTENT AREA -->
        <main class="content-area">
            <!-- DASHBOARD -->
            <div id="dashboard" class="tab-content active">
                <h2 class="mb-20">📊 Dashboard Penjualan</h2>
                
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
                
                <div style="background: white; border-radius: 15px; padding: 25px; margin-top: 30px;">
                    <h3>📈 Grafik Penjualan 7 Hari Terakhir</h3>
                    <canvas id="salesChart" style="width: 100%; height: 300px;"></canvas>
                </div>
            </div>

            <!-- KASIR -->
            <div id="kasir" class="tab-content">
                <h2 class="mb-20">💼 Kasir Point of Sale</h2>
                
                <div class="kasir-container">
                    <!-- KERANJANG -->
                    <div class="cart-items">
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                            <h3>🛒 Keranjang Belanja</h3>
                            <button class="btn btn-danger" onclick="clearCart()">
                                🗑️ Kosongkan
                            </button>
                        </div>
                        
                        <div id="cartContainer">
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
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                    <h2>📦 Manajemen Produk & Stok</h2>
                    <button class="btn btn-success" onclick="showAddProductModal()">
                        ➕ Tambah Produk
                    </button>
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
                <h2 class="mb-20">📈 Laporan Penjualan</h2>
                
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
                        
                        <div style="align-self: end;">
                            <button class="btn btn-primary btn-block" onclick="generateReport()">
                                🔍 Generate Laporan
                            </button>
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
                <h2 class="mb-20">🧾 Riwayat Transaksi</h2>
                
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

            <!-- PENGATURAN -->
            <div id="settings" class="tab-content">
                <h2 class="mb-20">⚙️ Pengaturan Aplikasi</h2>
                
                <div style="background: #f8f9fa; padding: 25px; border-radius: 15px;">
                    <h3 style="margin-bottom: 20px;">Konfigurasi Sistem</h3>
                    
                    <div class="form-group">
                        <label class="form-label">Nama Toko</label>
                        <input type="text" id="storeName" class="form-input" value="TOKO MAKMUR SEJAHTERA">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Alamat Toko</label>
                        <textarea id="storeAddress" class="form-input" rows="3">Jl. Contoh No. 123, Kota Contoh</textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Telepon</label>
                        <input type="text" id="storePhone" class="form-input" value="(021) 1234-5678">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Pajak (%)</label>
                        <input type="number" id="taxRate" class="form-input" value="10" min="0" max="100">
                    </div>
                    
                    <div class="btn-group" style="display: flex; gap: 10px; margin-top: 30px;">
                        <button class="btn btn-success" onclick="saveSettings()">
                            💾 Simpan Pengaturan
                        </button>
                        <button class="btn btn-danger" onclick="resetData()">
                            🔄 Reset Data
                        </button>
                        <button class="btn btn-primary" onclick="exportData()">
                            📥 Export Data
                        </button>
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
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // ==================== INITIALIZATION ====================
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Aplikasi Kasir PRO Loading...');
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
            
            showLoading(false);
            
            // Show welcome message
            setTimeout(() => {
                showAlert('success', 'Aplikasi Kasir PRO siap digunakan!');
            }, 1000);
        });

        // ==================== GLOBAL VARIABLES ====================
        let products = [];
        let cart = [];
        let transactions = [];
        let settings = {};
        let salesChartInstance = null;
        let currentProductId = null;

        // ==================== APP INITIALIZATION ====================
        function initializeApp() {
            console.log('Initializing app...');
            
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
                    sold: 125
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
                    sold: 89
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
                    sold: 156
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
                    sold: 67
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
                    sold: 234
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
                    sold: 78
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
                    sold: 345
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
                    sold: 289
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
                    sold: 167
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
                    sold: 134
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
            document.getElementById('storeName').value = settings.storeName;
            document.getElementById('storeAddress').value = settings.storeAddress;
            document.getElementById('storePhone').value = settings.storePhone;
            document.getElementById('taxRate').value = settings.taxRate;
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
        }

        // ==================== PRODUK MANAGEMENT ====================
        function loadProductsForManage() {
            const container = document.getElementById('productGridManage');
            if (!container) return;
            
            container.innerHTML = '';
            
            // Apply filters
            const searchTerm = document.getElementById('searchProduct')?.value.toLowerCase() || '';
            const categoryFilter = document.getElementById('filterCategory')?.value || '';
            const stockFilter = document.getElementById('filterStock')?.value || '';
            
            const filteredProducts = products.filter(product => {
                // Search filter
                if (searchTerm && !product.name.toLowerCase().includes(searchTerm) && 
                    !product.code.toLowerCase().includes(searchTerm)) {
                    return false;
                }
                
                // Category filter
                if (categoryFilter && product.category !== categoryFilter) {
                    return false;
                }
                
                // Stock filter
                if (stockFilter) {
                    if (stockFilter === 'low' && product.stock >= product.minStock) return false;
                    if (stockFilter === 'medium' && (product.stock < product.minStock || product.stock > 50)) return false;
                    if (stockFilter === 'high' && product.stock <= 50) return false;
                }
                
                return true;
            });
            
            // Render products
            filteredProducts.forEach(product => {
                const stockClass = getStockClass(product.stock, product.minStock);
                const stockBadge = getStockBadge(product.stock, product.minStock);
                
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                productCard.innerHTML = `
                    <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 10px;">
                        <div class="product-name">${product.name}</div>
                        <div style="font-size: 11px; background: #e3f2fd; color: #1976d2; padding: 2px 8px; border-radius: 10px;">
                            ${product.code}
                        </div>
                    </div>
                    <div class="product-price">${formatCurrency(product.price)}</div>
                    <div style="color: #7f8c8d; font-size: 12px; margin-bottom: 10px;">
                        ${product.category} • ${product.unit}
                    </div>
                    <div class="product-stock">
                        <div>
                            <div>Stok: <strong>${product.stock}</strong></div>
                            <div style="font-size: 11px;">Terjual: ${product.sold || 0}</div>
                        </div>
                        <span class="stock-badge ${stockClass}">${stockBadge}</span>
                    </div>
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
        }

        function filterProducts() {
            loadProductsForManage();
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

        function editProduct(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;
            
            currentProductId = productId;
            document.getElementById('modalTitle').textContent = 'Edit Produk';
            document.getElementById('modalProductName').value = product.name;
            document.getElementById('modalProductPrice').value = product.price;
            document.getElementById('modalProductStock').value = product.stock;
            document.getElementById('modalProductCategory').value = product.category;
            document.getElementById('modalProductUnit').value = product.unit;
            document.getElementById('productModal').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('productModal').style.display = 'none';
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
                // Update existing product
                const productIndex = products.findIndex(p => p.id === currentProductId);
                if (productIndex !== -1) {
                    products[productIndex] = {
                        ...products[productIndex],
                        name: name,
                        price: price,
                        stock: stock,
                        category: category,
                        unit: unit
                    };
                    showAlert('success', 'Produk berhasil diupdate!');
                }
            } else {
                // Add new product
                const newId = products.length > 0 ? Math.max(...products.map(p => p.id)) + 1 : 1;
                const code = 'PRD' + newId.toString().padStart(3, '0');
                
                products.push({
                    id: newId,
                    code: code,
                    name: name,
                    price: price,
                    cost: price * 0.8, // Default cost 80% of price
                    category: category,
                    unit: unit,
                    stock: stock,
                    minStock: 10,
                    sold: 0
                });
                showAlert('success', 'Produk baru berhasil ditambahkan!');
            }
            
            saveToLocalStorage();
            closeModal();
            loadProductsForKasir();
            loadProductsForManage();
            loadDashboard();
        }

        function deleteProduct(productId) {
            if (!confirm('Apakah Anda yakin ingin menghapus produk ini?')) return;
            
            const productIndex = products.findIndex(p => p.id === productId);
            if (productIndex !== -1) {
                products.splice(productIndex, 1);
                saveToLocalStorage();
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
                case 'custom':
                    const startDate = document.getElementById('startDate').value;
                    const endDate = document.getElementById('endDate').value;
                    if (startDate && endDate) {
                        filteredTransactions = transactions.filter(t => {
                            const transDate = new Date(t.date.split('/').reverse().join('-'));
                            const start = new Date(startDate);
                            const end = new Date(endDate);
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
        }

        // ==================== TRANSAKSI ====================
        function loadTransactions() {
            const tableBody = document.getElementById('transactionTable');
            if (!tableBody) return;
            
            tableBody.innerHTML = '';
            
            transactions.slice(0, 50).forEach((trans, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${trans.date}<br><small>${trans.time}</small></td>
                    <td>${trans.id}</td>
                    <td>${trans.items.map(item => `${item.name} (${item.quantity}x)`).join('<br>')}</td>
                    <td>${formatCurrency(trans.total)}</td>
                    <td>${formatCurrency(trans.payment)}</td>
                    <td>${formatCurrency(trans.change)}</td>
                    <td>
                        <button onclick="viewTransaction('${trans.id}')" style="padding: 5px 10px; background: #3498db; color: white; border: none; border-radius: 3px; cursor: pointer; font-size: 12px;">
                            Lihat
                        </button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
        }

        function viewTransaction(transactionId) {
            const transaction = transactions.find(t => t.id === transactionId);
            if (!transaction) return;
            
            alert(`
                DETAIL TRANSAKSI
                ===================
                ID: ${transaction.id}
                Tanggal: ${transaction.date} ${transaction.time}
                Kasir: ${transaction.kasir}
                
                ITEMS:
                ${transaction.items.map(item => `- ${item.name}: ${item.quantity} x ${formatCurrency(item.price)} = ${formatCurrency(item.quantity * item.price)}`).join('\n')}
                
                Subtotal: ${formatCurrency(transaction.subtotal)}
                Pajak (${transaction.tax}%): ${formatCurrency(transaction.subtotal * (transaction.tax / 100))}
                Total: ${formatCurrency(transaction.total)}
                Bayar: ${formatCurrency(transaction.payment)}
                Kembali: ${formatCurrency(transaction.change)}
            `);
        }

        // ==================== PENGATURAN ====================
        function saveSettings() {
            settings.storeName = document.getElementById('storeName').value;
            settings.storeAddress = document.getElementById('storeAddress').value;
            settings.storePhone = document.getElementById('storePhone').value;
            settings.taxRate = parseFloat(document.getElementById('taxRate').value);
            
            localStorage.setItem('kasir_settings', JSON.stringify(settings));
            showAlert('success', 'Pengaturan berhasil disimpan!');
        }

        function resetData() {
            if (!confirm('PERINGATAN: Semua data akan direset ke default. Lanjutkan?')) return;
            
            if (confirm('Yakin reset SEMUA data?')) {
                localStorage.clear();
                initializeApp();
                showAlert('success', 'Semua data telah direset ke default');
                location.reload();
            }
        }

        function exportData() {
            const data = {
                products: products,
                transactions: transactions,
                settings: settings,
                exportDate: new Date().toISOString()
            };
            
            const dataStr = JSON.stringify(data, null, 2);
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            
            const exportFileDefaultName = `backup-kasir-${new Date().toISOString().split('T')[0]}.json`;
            
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
            
            showAlert('success', 'Data berhasil diexport!');
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
            if (!transaction) {
                showAlert('error', 'Tidak ada transaksi untuk dicetak');
                return;
            }
            
            const receiptContent = `
                <div style="font-family: 'Courier New', monospace; width: 72mm; padding: 10px;">
                    <div style="text-align: center; margin-bottom: 10px;">
                        <h2 style="margin: 0; font-size: 18px;">${settings.storeName}</h2>
                        <p style="margin: 0; font-size: 10px;">${settings.storeAddress}</p>
                        <p style="margin: 0; font-size: 10px;">Telp: ${settings.storePhone}</p>
                        <hr style="border: none; border-top: 1px dashed #000; margin: 8px 0;">
                        <p style="margin: 0; font-size: 9px;">${transaction.date} ${transaction.time}</p>
                        <p style="margin: 0; font-size: 9px;"><strong>No: ${transaction.id}</strong></p>
                        <hr style="border: none; border-top: 1px dashed #000; margin: 8px 0;">
                    </div>
                    
                    <div style="font-size: 11px; margin-bottom: 10px;">
                        ${transaction.items.map(item => `
                            <div style="display: flex; justify-content: space-between; margin-bottom: 3px;">
                                <div>${item.name}</div>
                                <div>${item.quantity} x ${formatCurrency(item.price)}</div>
                            </div>
                        `).join('')}
                    </div>
                    
                    <hr style="border: none; border-top: 1px dashed #000; margin: 10px 0;">
                    
                    <div style="font-size: 11px;">
                        <div style="display: flex; justify-content: space-between; margin-bottom: 2px;">
                            <div>Subtotal:</div>
                            <div>${formatCurrency(transaction.subtotal)}</div>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 2px;">
                            <div>Pajak (${transaction.tax}%):</div>
                            <div>${formatCurrency(transaction.subtotal * (transaction.tax / 100))}</div>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 5px; font-weight: bold;">
                            <div>TOTAL:</div>
                            <div>${formatCurrency(transaction.total)}</div>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 2px;">
                            <div>Bayar:</div>
                            <div>${formatCurrency(transaction.payment)}</div>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 10px; font-weight: bold;">
                            <div>Kembali:</div>
                            <div>${formatCurrency(transaction.change)}</div>
                        </div>
                    </div>
                    
                    <hr style="border: none; border-top: 1px dashed #000; margin: 10px 0;">
                    
                    <div style="text-align: center; font-size: 9px; margin-top: 15px;">
                        <p style="margin: 0;">${settings.receiptFooter || 'Terima kasih atas kunjungan Anda'}</p>
                        <p style="margin: 0; font-style: italic;">** Barang sudah dibeli tidak dapat ditukar **</p>
                    </div>
                </div>
            `;
            
            const printWindow = window.open('', '_blank', 'width=400,height=600');
            printWindow.document.write(`
                <!DOCTYPE html>
                <html>
                <head>
                    <title>Struk - ${transaction.id}</title>
                    <style>
                        @media print {
                            @page { margin: 0; size: 80mm auto; }
                            body { margin: 0; padding: 0; }
                        }
                        body { font-family: 'Courier New', monospace; }
                    </style>
                </head>
                <body>
                    ${receiptContent}
                    <script>
                        window.onload = function() {
                            window.print();
                            setTimeout(() => window.close(), 1000);
                        };
                    <\/script>
                </body>
                </html>
            `);
            printWindow.document.close();
        }

        // Initialize when page loads
        console.log('Aplikasi Kasir PRO v2.0 Loaded');
    </script>
</body>
</html>
