<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Kasir Lengkap + Laporan & Stok</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
        }
        
        /* NAVIGASI */
        .navbar {
            background-color: #2c3e50;
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .nav-links {
            display: flex;
            gap: 20px;
        }
        
        .nav-link {
            color: white;
            text-decoration: none;
            padding: 8px 15px;
            border-radius: 5px;
            transition: background 0.3s;
        }
        
        .nav-link:hover, .nav-link.active {
            background-color: #3498db;
        }
        
        /* KONTEN UTAMA */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .section {
            display: none;
            background: white;
            border-radius: 10px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 3px 15px rgba(0,0,0,0.08);
        }
        
        .section.active {
            display: block;
            animation: fadeIn 0.5s;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .section-title {
            color: #2c3e50;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #3498db;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        /* DASHBOARD */
        .dashboard-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            border-left: 5px solid #3498db;
        }
        
        .card.total-penjualan { border-left-color: #27ae60; }
        .card.total-transaksi { border-left-color: #e74c3c; }
        .card.stok-habis { border-left-color: #f39c12; }
        .card.produk-terjual { border-left-color: #9b59b6; }
        
        .card h3 {
            font-size: 14px;
            color: #7f8c8d;
            margin-bottom: 10px;
        }
        
        .card .value {
            font-size: 28px;
            font-weight: bold;
            color: #2c3e50;
        }
        
        /* PRODUK & STOK */
        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .product-card {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            border: 1px solid #eaeaea;
            transition: transform 0.2s, box-shadow 0.2s;
            cursor: pointer;
        }
        
        .product-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .product-name {
            font-weight: bold;
            margin-bottom: 5px;
            color: #2c3e50;
        }
        
        .product-price {
            color: #27ae60;
            font-weight: bold;
            font-size: 16px;
            margin-bottom: 5px;
        }
        
        .product-stock {
            font-size: 12px;
            color: #7f8c8d;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .stock-badge {
            padding: 2px 8px;
            border-radius: 10px;
            font-size: 11px;
            font-weight: bold;
        }
        
        .stock-low { background: #fdeaea; color: #e74c3c; }
        .stock-medium { background: #fff4e6; color: #f39c12; }
        .stock-high { background: #e8f6f3; color: #27ae60; }
        
        /* FORM TAMBAH/EDIT PRODUK */
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            color: #2c3e50;
            font-weight: 500;
        }
        
        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
        }
        
        .form-row {
            display: flex;
            gap: 15px;
        }
        
        .form-row .form-group {
            flex: 1;
        }
        
        /* KERANJANG */
        .cart-container {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }
        
        .cart-items {
            flex: 2;
            min-width: 300px;
            max-height: 400px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 8px;
            padding: 15px;
        }
        
        .cart-summary {
            flex: 1;
            min-width: 300px;
            background: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
        }
        
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        
        .item-info {
            flex: 2;
        }
        
        .item-qty {
            flex: 1;
            display: flex;
            align-items: center;
            gap: 10px;
            justify-content: center;
        }
        
        .item-total {
            flex: 1;
            text-align: right;
            font-weight: bold;
            color: #2c3e50;
        }
        
        .qty-btn {
            width: 30px;
            height: 30px;
            border: none;
            border-radius: 50%;
            background: #3498db;
            color: white;
            cursor: pointer;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .qty-btn:hover {
            background: #2980b9;
        }
        
        /* LAPORAN */
        .report-filters {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
        }
        
        .filter-group {
            flex: 1;
            min-width: 200px;
        }
        
        .report-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        .report-table th {
            background: #2c3e50;
            color: white;
            padding: 12px;
            text-align: left;
        }
        
        .report-table td {
            padding: 10px;
            border-bottom: 1px solid #eee;
        }
        
        .report-table tr:hover {
            background: #f8f9fa;
        }
        
        /* TOMBOL */
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s;
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
        
        .btn-danger:hover {
            background: #c0392b;
        }
        
        .btn-warning {
            background: #f39c12;
            color: white;
        }
        
        .btn-warning:hover {
            background: #d68910;
        }
        
        .btn-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-top: 20px;
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
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
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
        .badge {
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
        }
        
        .badge-success { background: #d5f4e6; color: #27ae60; }
        .badge-danger { background: #fdeaea; color: #e74c3c; }
        .badge-warning { background: #fff4e6; color: #f39c12; }
        
        .text-right { text-align: right; }
        .text-center { text-align: center; }
        .text-muted { color: #7f8c8d; }
        
        /* RESPONSIVE */
        @media (max-width: 768px) {
            .navbar {
                flex-direction: column;
                gap: 15px;
            }
            
            .nav-links {
                width: 100%;
                justify-content: space-around;
            }
            
            .cart-container {
                flex-direction: column;
            }
            
            .form-row {
                flex-direction: column;
                gap: 0;
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
    <!-- NAVIGASI -->
    <nav class="navbar">
        <h1>🏪 Aplikasi Kasir Lengkap</h1>
        <div class="nav-links">
            <a href="#" class="nav-link active" onclick="showSection('dashboard')">📊 Dashboard</a>
            <a href="#" class="nav-link" onclick="showSection('kasir')">💼 Kasir</a>
            <a href="#" class="nav-link" onclick="showSection('produk')">📦 Produk & Stok</a>
            <a href="#" class="nav-link" onclick="showSection('laporan')">📈 Laporan</a>
            <a href="#" class="nav-link" onclick="showSection('transaksi')">🧾 Transaksi</a>
        </div>
    </nav>

    <!-- KONTEN UTAMA -->
    <div class="container">
        <!-- DASHBOARD -->
        <section id="dashboard" class="section active">
            <h2 class="section-title">Dashboard</h2>
            
            <div class="dashboard-cards">
                <div class="card total-penjualan">
                    <h3>Total Penjualan Hari Ini</h3>
                    <div class="value" id="totalHarian">Rp 0</div>
                    <div class="text-muted" style="font-size: 12px; margin-top: 5px;">↗️ 12% dari kemarin</div>
                </div>
                
                <div class="card total-transaksi">
                    <h3>Total Transaksi Hari Ini</h3>
                    <div class="value" id="totalTransaksi">0</div>
                    <div class="text-muted" style="font-size: 12px; margin-top: 5px;">Rata-rata: 15 transaksi/hari</div>
                </div>
                
                <div class="card stok-habis">
                    <h3>Stok Hampir Habis</h3>
                    <div class="value" id="stokHampirHabis">0 produk</div>
                    <div class="text-muted" style="font-size: 12px; margin-top: 5px;">Perlu restock segera</div>
                </div>
                
                <div class="card produk-terjual">
                    <h3>Produk Terjual Hari Ini</h3>
                    <div class="value" id="produkTerjual">0 item</div>
                    <div class="text-muted" style="font-size: 12px; margin-top: 5px;">↘️ 5% dari kemarin</div>
                </div>
            </div>
            
            <div style="display: grid; grid-template-columns: 2fr 1fr; gap: 20px; margin-top: 30px;">
                <div>
                    <h3 style="margin-bottom: 15px;">📈 Grafik Penjualan 7 Hari Terakhir</h3>
                    <canvas id="salesChart" style="background: white; padding: 15px; border-radius: 8px;"></canvas>
                </div>
                
                <div>
                    <h3 style="margin-bottom: 15px;">🏆 Produk Terlaris</h3>
                    <div id="topProducts" style="background: white; padding: 15px; border-radius: 8px; max-height: 300px; overflow-y: auto;">
                        <!-- Produk terlaris akan diisi JS -->
                    </div>
                </div>
            </div>
        </section>

        <!-- KASIR -->
        <section id="kasir" class="section">
            <h2 class="section-title">Kasir
                <button class="btn btn-success" onclick="processPayment()">
                    💳 Proses Pembayaran
                </button>
            </h2>
            
            <div class="cart-container">
                <div class="cart-items">
                    <h3>Keranjang Belanja</h3>
                    <div id="cartItems">
                        <div style="text-align: center; padding: 40px; color: #7f8c8d;">
                            <div style="font-size: 48px; margin-bottom: 20px;">🛒</div>
                            <p>Keranjang kosong</p>
                            <p style="font-size: 14px;">Pilih produk dari daftar di bawah</p>
                        </div>
                    </div>
                </div>
                
                <div class="cart-summary">
                    <h3>Ringkasan</h3>
                    <div style="margin: 20px 0;">
                        <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                            <span>Subtotal:</span>
                            <span id="subtotal">Rp 0</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                            <span>Pajak (10%):</span>
                            <span id="tax">Rp 0</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; font-size: 18px; font-weight: bold; padding-top: 10px; border-top: 2px solid #3498db;">
                            <span>TOTAL:</span>
                            <span id="total">Rp 0</span>
                        </div>
                    </div>
                    
                    <div class="btn-group">
                        <button class="btn btn-danger" onclick="clearCart()">🗑️ Kosongkan</button>
                        <button class="btn btn-warning" onclick="printReceipt()">🖨️ Cetak Struk</button>
                    </div>
                </div>
            </div>
            
            <h3 style="margin-top: 30px; margin-bottom: 15px;">Daftar Produk</h3>
            <div class="product-grid" id="productListKasir">
                <!-- Produk untuk kasir akan diisi JS -->
            </div>
        </section>

        <!-- PRODUK & STOK -->
        <section id="produk" class="section">
            <h2 class="section-title">Manajemen Produk & Stok
                <button class="btn btn-primary" onclick="showAddProductModal()">
                    ➕ Tambah Produk
                </button>
            </h2>
            
            <div class="report-filters">
                <div class="filter-group">
                    <label>Filter Kategori</label>
                    <select class="form-control" id="filterCategory" onchange="filterProducts()">
                        <option value="">Semua Kategori</option>
                        <option value="Sembako">Sembako</option>
                        <option value="Minuman">Minuman</option>
                        <option value="Snack">Snack</option>
                        <option value="Kebersihan">Kebersihan</option>
                    </select>
                </div>
                
                <div class="filter-group">
                    <label>Filter Stok</label>
                    <select class="form-control" id="filterStock" onchange="filterProducts()">
                        <option value="">Semua Stok</option>
                        <option value="low">Stok Rendah (< 10)</option>
                        <option value="medium">Stok Sedang (10-50)</option>
                        <option value="high">Stok Tinggi (> 50)</option>
                    </select>
                </div>
                
                <div class="filter-group">
                    <label>Cari Produk</label>
                    <input type="text" class="form-control" id="searchProduct" placeholder="Nama produk..." onkeyup="filterProducts()">
                </div>
            </div>
            
            <div class="product-grid" id="productListManage">
                <!-- Produk untuk manajemen akan diisi JS -->
            </div>
        </section>

        <!-- LAPORAN PENJUALAN -->
        <section id="laporan" class="section">
            <h2 class="section-title">Laporan Penjualan
                <button class="btn btn-success" onclick="exportToExcel()">
                    📥 Export Excel
                </button>
            </h2>
            
            <div class="report-filters">
                <div class="filter-group">
                    <label>Periode Laporan</label>
                    <select class="form-control" id="reportPeriod" onchange="generateReport()">
                        <option value="today">Hari Ini</option>
                        <option value="yesterday">Kemarin</option>
                        <option value="week">Minggu Ini</option>
                        <option value="month">Bulan Ini</option>
                        <option value="custom">Custom</option>
                    </select>
                </div>
                
                <div class="filter-group" id="customDateRange" style="display: none;">
                    <div class="form-row">
                        <div class="form-group">
                            <label>Dari Tanggal</label>
                            <input type="date" class="form-control" id="startDate">
                        </div>
                        <div class="form-group">
                            <label>Sampai Tanggal</label>
                            <input type="date" class="form-control" id="endDate">
                        </div>
                    </div>
                </div>
                
                <div class="filter-group">
                    <button class="btn btn-primary" onclick="generateReport()" style="margin-top: 23px;">
                        🔍 Generate Laporan
                    </button>
                </div>
            </div>
            
            <div class="dashboard-cards" style="margin-bottom: 30px;">
                <div class="card">
                    <h3>Total Penjualan</h3>
                    <div class="value" id="reportTotal">Rp 0</div>
                </div>
                
                <div class="card">
                    <h3>Total Transaksi</h3>
                    <div class="value" id="reportTransactions">0</div>
                </div>
                
                <div class="card">
                    <h3>Rata-rata per Transaksi</h3>
                    <div class="value" id="reportAverage">Rp 0</div>
                </div>
                
                <div class="card">
                    <h3>Produk Terjual</h3>
                    <div class="value" id="reportItems">0 item</div>
                </div>
            </div>
            
            <h3>Detail Transaksi</h3>
            <div style="overflow-x: auto;">
                <table class="report-table">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Tanggal</th>
                            <th>No. Transaksi</th>
                            <th>Produk</th>
                            <th>Jumlah</th>
                            <th>Total</th>
                            <th>Kasir</th>
                        </tr>
                    </thead>
                    <tbody id="reportTableBody">
                        <!-- Data laporan akan diisi JS -->
                    </tbody>
                </table>
            </div>
        </section>

        <!-- RIWAYAT TRANSAKSI -->
        <section id="transaksi" class="section">
            <h2 class="section-title">Riwayat Transaksi
                <button class="btn btn-warning" onclick="refreshTransactions()">
                    🔄 Refresh
                </button>
            </h2>
            
            <div style="overflow-x: auto;">
                <table class="report-table">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Tanggal</th>
                            <th>No. Transaksi</th>
                            <th>Items</th>
                            <th>Total</th>
                            <th>Bayar</th>
                            <th>Kembali</th>
                            <th>Kasir</th>
                            <th>Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="transactionTableBody">
                        <!-- Data transaksi akan diisi JS -->
                    </tbody>
                </table>
            </div>
        </section>
    </div>

    <!-- MODAL TAMBAH/EDIT PRODUK -->
    <div id="productModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 id="modalTitle">Tambah Produk Baru</h3>
                <button class="close-btn" onclick="closeModal()">&times;</button>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label>Kode Produk</label>
                    <input type="text" class="form-control" id="productCode" placeholder="Auto generate" readonly>
                </div>
                
                <div class="form-group">
                    <label>Nama Produk</label>
                    <input type="text" class="form-control" id="productName" placeholder="Contoh: Beras 5kg">
                </div>
                
                <div class="form-row">
                    <div class="form-group">
                        <label>Harga Beli (Rp)</label>
                        <input type="number" class="form-control" id="productCost" placeholder="0">
                    </div>
                    
                    <div class="form-group">
                        <label>Harga Jual (Rp)</label>
                        <input type="number" class="form-control" id="productPrice" placeholder="0">
                    </div>
                </div>
                
                <div class="form-row">
                    <div class="form-group">
                        <label>Stok Awal</label>
                        <input type="number" class="form-control" id="productStock" placeholder="0">
                    </div>
                    
                    <div class="form-group">
                        <label>Stok Minimal</label>
                        <input type="number" class="form-control" id="productMinStock" value="10">
                    </div>
                </div>
                
                <div class="form-group">
                    <label>Kategori</label>
                    <select class="form-control" id="productCategory">
                        <option value="Sembako">Sembako</option>
                        <option value="Minuman">Minuman</option>
                        <option value="Snack">Snack</option>
                        <option value="Kebersihan">Kebersihan</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label>Satuan</label>
                    <select class="form-control" id="productUnit">
                        <option value="pcs">pcs</option>
                        <option value="kg">kg</option>
                        <option value="liter">liter</option>
                        <option value="pack">pack</option>
                        <option value="dus">dus</option>
                    </select>
                </div>
            </div>
            <div class="modal-footer">
                <button class="btn btn-danger" onclick="closeModal()">Batal</button>
                <button class="btn btn-success" onclick="saveProduct()">Simpan Produk</button>
            </div>
        </div>
    </div>

    <!-- AREA PRINT STRUK -->
    <div id="printArea" style="display: none;"></div>

    <!-- SCRIPT -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // DATA APLIKASI
        let products = JSON.parse(localStorage.getItem('kasir_products')) || [
            { 
                id: 1, 
                code: 'BR001',
                name: "Beras 5kg", 
                price: 60000, 
                cost: 50000,
                category: "Sembako",
                unit: "pack",
                stock: 45,
                minStock: 10
            },
            { 
                id: 2, 
                code: 'MG002',
                name: "Minyak Goreng 2L", 
                price: 35000,
                cost: 28000,
                category: "Sembako",
                unit: "botol",
                stock: 28,
                minStock: 10
            },
            { 
                id: 3, 
                code: 'GP003',
                name: "Gula Pasir 1kg", 
                price: 15000,
                cost: 12000,
                category: "Sembako",
                unit: "kg",
                stock: 12,
                minStock: 10
            },
            { 
                id: 4, 
                code: 'TL004',
                name: "Telur 1kg", 
                price: 28000,
                cost: 22000,
                category: "Sembako",
                unit: "kg",
                stock: 8,
                minStock: 10
            },
            { 
                id: 5, 
                code: 'SB005',
                name: "Sabun Mandi", 
                price: 5000,
                cost: 3500,
                category: "Kebersihan",
                unit: "pcs",
                stock: 120,
                minStock: 20
            },
            { 
                id: 6, 
                code: 'SP006',
                name: "Shampoo", 
                price: 12000,
                cost: 9000,
                category: "Kebersihan",
                unit: "botol",
                stock: 35,
                minStock: 10
            },
            { 
                id: 7, 
                code: 'AM007',
                name: "Air Mineral 600ml", 
                price: 3000,
                cost: 2000,
                category: "Minuman",
                unit: "botol",
                stock: 150,
                minStock: 30
            },
            { 
                id: 8, 
                code: 'KS008',
                name: "Kopi Sachet", 
                price: 2000,
                cost: 1500,
                category: "Minuman",
                unit: "pcs",
                stock: 89,
                minStock: 20
            },
            { 
                id: 9, 
                code: 'TC009',
                name: "Teh Celup", 
                price: 1500,
                cost: 1000,
                category: "Minuman",
                unit: "pcs",
                stock: 65,
                minStock: 20
            },
            { 
                id: 10, 
                code: 'BS010',
                name: "Biskuit", 
                price: 8000,
                cost: 6000,
                category: "Snack",
                unit: "pack",
                stock: 42,
                minStock: 15
            }
        ];

        let cart = [];
        let transactions = JSON.parse(localStorage.getItem('kasir_transactions')) || [];
        let currentProductId = null;
        let salesChart = null;

        // INISIALISASI
        document.addEventListener('DOMContentLoaded', function() {
            loadDashboard();
            renderKasirProducts();
            renderManageProducts();
            loadTransactions();
            generateReport();
            
            // Setup filter tanggal
            document.getElementById('reportPeriod').addEventListener('change', function() {
                const customDiv = document.getElementById('customDateRange');
                customDiv.style.display = this.value === 'custom' ? 'block' : 'none';
            });
            
            // Set tanggal default
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('startDate').value = today;
            document.getElementById('endDate').value = today;
        });

        // NAVIGASI
        function showSection(sectionId) {
            // Update nav link aktif
            document.querySelectorAll('.nav-link').forEach(link => {
                link.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Tampilkan section
            document.querySelectorAll('.section').forEach(section => {
                section.classList.remove('active');
            });
            document.getElementById(sectionId).classList.add('active');
            
            // Refresh data jika diperlukan
            if (sectionId === 'dashboard') loadDashboard();
            if (sectionId === 'produk') renderManageProducts();
        }

        // DASHBOARD
        function loadDashboard() {
            const today = new Date().toLocaleDateString('id-ID');
            let totalHarian = 0;
            let totalTransaksi = 0;
            let produkTerjual = 0;
            let stokHampirHabis = 0;
            
            // Hitung statistik hari ini
            transactions.forEach(trans => {
                if (trans.date === today) {
                    totalHarian += trans.total;
                    totalTransaksi++;
                    produkTerjual += trans.items.reduce((sum, item) => sum + item.quantity, 0);
                }
            });
            
            // Hitung stok hampir habis
            products.forEach(product => {
                if (product.stock < product.minStock) {
                    stokHampirHabis++;
                }
            });
            
            // Update dashboard
            document.getElementById('totalHarian').textContent = formatCurrency(totalHarian);
            document.getElementById('totalTransaksi').textContent = totalTransaksi;
            document.getElementById('stokHampirHabis').textContent = `${stokHampirHabis} produk`;
            document.getElementById('produkTerjual').textContent = `${produkTerjual} item`;
            
            // Update grafik
            updateSalesChart();
            
            // Update produk terlaris
            updateTopProducts();
        }

        function updateSalesChart() {
            const ctx = document.getElementById('salesChart').getContext('2d');
            
            // Data dummy untuk 7 hari terakhir
            const labels = [];
            const data = [];
            
            for (let i = 6; i >= 0; i--) {
                const date = new Date();
                date.setDate(date.getDate() - i);
                labels.push(date.toLocaleDateString('id-ID', { weekday: 'short' }));
                
                // Hitung penjualan untuk hari ini (dummy)
                const sales = Math.floor(Math.random() * 500000) + 100000;
                data.push(sales);
            }
            
            if (salesChart) salesChart.destroy();
            
            salesChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Penjualan (Rp)',
                        data: data,
                        borderColor: '#3498db',
                        backgroundColor: 'rgba(52, 152, 219, 0.1)',
                        borderWidth: 2,
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return 'Rp ' + (value/1000).toFixed(0) + 'k';
                                }
                            }
                        }
                    }
                }
            });
        }

        function updateTopProducts() {
            const container = document.getElementById('topProducts');
            let productSales = {};
            
            // Hitung total penjualan per produk
            transactions.forEach(trans => {
                trans.items.forEach(item => {
                    if (!productSales[item.name]) {
                        productSales[item.name] = 0;
                    }
                    productSales[item.name] += item.quantity;
                });
            });
            
            // Urutkan dari terbesar
            const sortedProducts = Object.entries(productSales)
                .sort((a, b) => b[1] - a[1])
                .slice(0, 5);
            
            let html = '';
            if (sortedProducts.length > 0) {
                sortedProducts.forEach(([name, qty], index) => {
                    const product = products.find(p => p.name === name);
                    const stockBadge = product ? getStockBadge(product.stock) : '';
                    
                    html += `
                        <div style="display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom:
