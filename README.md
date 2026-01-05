<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kasir Sederhana dengan Export Excel</title>
    
    <!-- SIMPLE STYLE - NO COMPLEX CSS -->
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        
        body {
            background: #f0f2f5;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        
        header {
            background: #2c3e50;
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        nav {
            background: #34495e;
            padding: 10px;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .nav-btn {
            background: #3498db;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }
        
        .nav-btn:hover {
            background: #2980b9;
        }
        
        .nav-btn.active {
            background: #27ae60;
        }
        
        .content {
            padding: 20px;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .stat-card {
            background: white;
            border-radius: 8px;
            padding: 15px;
            border-left: 4px solid #3498db;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .stat-card.sales { border-color: #27ae60; }
        .stat-card.transactions { border-color: #e74c3c; }
        .stat-card.products { border-color: #f39c12; }
        
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .product-card {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 15px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .product-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        th, td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background: #2c3e50;
            color: white;
        }
        
        .btn {
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            margin: 5px;
        }
        
        .btn-success {
            background: #27ae60;
            color: white;
        }
        
        .btn-primary {
            background: #3498db;
            color: white;
        }
        
        .btn-excel {
            background: #217346;
            color: white;
        }
        
        .btn-danger {
            background: #e74c3c;
            color: white;
        }
        
        .loading {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255,255,255,0.9);
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            z-index: 1000;
        }
        
        .alert {
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 15px;
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
        
        @media (max-width: 768px) {
            .products-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            nav {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <!-- LOADING -->
    <div id="loading" class="loading" style="display: none;">
        <div>Loading aplikasi...</div>
    </div>

    <!-- ALERT -->
    <div id="alert" class="alert"></div>

    <!-- APLIKASI -->
    <div class="container">
        <header>
            <h1>💰 Kasir Sederhana</h1>
            <p>Aplikasi kasir dengan export Excel</p>
        </header>
        
        <nav>
            <button class="nav-btn active" onclick="showTab('dashboard')">📊 Dashboard</button>
            <button class="nav-btn" onclick="showTab('kasir')">💼 Kasir</button>
            <button class="nav-btn" onclick="showTab('produk')">📦 Produk</button>
            <button class="nav-btn" onclick="showTab('laporan')">📈 Laporan</button>
            <button class="nav-btn" onclick="showTab('export')">📥 Export</button>
        </nav>
        
        <div class="content">
            <!-- DASHBOARD -->
            <div id="dashboard" class="tab-content active">
                <h2>📊 Dashboard</h2>
                <div class="stats">
                    <div class="stat-card sales">
                        <h3>Penjualan Hari Ini</h3>
                        <p id="salesToday">Rp 0</p>
                    </div>
                    <div class="stat-card transactions">
                        <h3>Transaksi Hari Ini</h3>
                        <p id="transactionsToday">0</p>
                    </div>
                    <div class="stat-card products">
                        <h3>Total Produk</h3>
                        <p id="totalProducts">0</p>
                    </div>
                </div>
                
                <button class="btn btn-primary" onclick="refreshDashboard()">🔄 Refresh</button>
                <button class="btn btn-excel" onclick="exportDashboard()">📥 Export Excel</button>
            </div>
            
            <!-- KASIR -->
            <div id="kasir" class="tab-content">
                <h2>💼 Kasir</h2>
                
                <div style="display: grid; grid-template-columns: 2fr 1fr; gap: 20px;">
                    <div>
                        <h3>Keranjang</h3>
                        <div id="cart" style="background: #f8f9fa; padding: 15px; border-radius: 8px; min-height: 200px;">
                            Keranjang kosong
                        </div>
                        
                        <div style="margin-top: 15px;">
                            <h3>Daftar Produk</h3>
                            <div class="products-grid" id="productsKasir"></div>
                        </div>
                    </div>
                    
                    <div style="background: #f8f9fa; padding: 20px; border-radius: 8px;">
                        <h3>Pembayaran</h3>
                        <div style="margin: 15px 0;">
                            <p>Total: <span id="totalAmount">Rp 0</span></p>
                            <input type="number" id="payment" placeholder="Jumlah bayar" style="width: 100%; padding: 10px; margin: 10px 0;">
                            <p>Kembali: <span id="change">Rp 0</span></p>
                        </div>
                        <button class="btn btn-success" onclick="processPayment()" style="width: 100%;">💳 Bayar</button>
                        <button class="btn btn-danger" onclick="clearCart()" style="width: 100%; margin-top: 10px;">🗑️ Kosongkan</button>
                    </div>
                </div>
            </div>
            
            <!-- PRODUK -->
            <div id="produk" class="tab-content">
                <h2>📦 Manajemen Produk</h2>
                
                <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">
                    <input type="text" id="searchProduct" placeholder="Cari produk..." style="padding: 10px; width: 100%;">
                </div>
                
                <div style="text-align: right; margin-bottom: 15px;">
                    <button class="btn btn-success" onclick="addProduct()">➕ Tambah Produk</button>
                    <button class="btn btn-excel" onclick="exportProducts()">📥 Export Excel</button>
                </div>
                
                <table>
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Nama</th>
                            <th>Harga</th>
                            <th>Stok</th>
                            <th>Kategori</th>
                            <th>Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="productsTable"></tbody>
                </table>
            </div>
            
            <!-- LAPORAN -->
            <div id="laporan" class="tab-content">
                <h2>📈 Laporan Penjualan</h2>
                
                <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">
                    <select id="reportType" style="padding: 10px; width: 200px;">
                        <option value="today">Hari Ini</option>
                        <option value="week">Minggu Ini</option>
                        <option value="month">Bulan Ini</option>
                    </select>
                    <button class="btn btn-primary" onclick="loadReport()">🔍 Tampilkan</button>
                    <button class="btn btn-excel" onclick="exportReport()">📥 Export Excel</button>
                </div>
                
                <table>
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Tanggal</th>
                            <th>ID Transaksi</th>
                            <th>Total</th>
                            <th>Bayar</th>
                            <th>Kembali</th>
                        </tr>
                    </thead>
                    <tbody id="reportTable"></tbody>
                </table>
            </div>
            
            <!-- EXPORT -->
            <div id="export" class="tab-content">
                <h2>📥 Export Data</h2>
                
                <div style="background: #f8f9fa; padding: 20px; border-radius: 8px; margin-top: 20px;">
                    <h3>Pilih data untuk di-export:</h3>
                    
                    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
                        <div style="background: white; padding: 20px; border-radius: 8px; text-align: center; cursor: pointer;" onclick="exportData('products')">
                            <div style="font-size: 30px; margin-bottom: 10px;">📦</div>
                            <h4>Data Produk</h4>
                            <p>Export semua produk</p>
                        </div>
                        
                        <div style="background: white; padding: 20px; border-radius: 8px; text-align: center; cursor: pointer;" onclick="exportData('transactions')">
                            <div style="font-size: 30px; margin-bottom: 10px;">💰</div>
                            <h4>Transaksi</h4>
                            <p>Export semua transaksi</p>
                        </div>
                        
                        <div style="background: white; padding: 20px; border-radius: 8px; text-align: center; cursor: pointer;" onclick="exportData('all')">
                            <div style="font-size: 30px; margin-bottom: 10px;">📊</div>
                            <h4>Semua Data</h4>
                            <p>Export semua data</p>
                        </div>
                    </div>
                    
                    <div style="margin-top: 30px;">
                        <h4>Export berdasarkan tanggal:</h4>
                        <input type="date" id="exportDate" style="padding: 10px; margin: 10px;">
                        <button class="btn btn-excel" onclick="exportByDate()">📅 Export by Date</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL TAMBAH PRODUK -->
    <div id="productModal" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center;">
        <div style="background: white; padding: 30px; border-radius: 10px; width: 90%; max-width: 400px;">
            <h3>Tambah Produk</h3>
            <div style="margin: 20px 0;">
                <input type="text" id="productName" placeholder="Nama Produk" style="width: 100%; padding: 10px; margin: 10px 0;">
                <input type="number" id="productPrice" placeholder="Harga" style="width: 100%; padding: 10px; margin: 10px 0;">
                <input type="number" id="productStock" placeholder="Stok" style="width: 100%; padding: 10px; margin: 10px 0;">
                <select id="productCategory" style="width: 100%; padding: 10px; margin: 10px 0;">
                    <option value="Sembako">Sembako</option>
                    <option value="Minuman">Minuman</option>
                    <option value="Snack">Snack</option>
                </select>
            </div>
            <div style="text-align: right;">
                <button class="btn btn-danger" onclick="closeModal()">Batal</button>
                <button class="btn btn-success" onclick="saveProduct()">Simpan</button>
            </div>
        </div>
    </div>

    <!-- SCRIPT SEDERHANA -->
    <script>
        // ==================== INISIALISASI ====================
        console.log('Memulai aplikasi kasir...');
        
        // Data awal
        let products = [];
        let cart = [];
        let transactions = [];
        let currentTab = 'dashboard';
        
        // Load data dari localStorage
        function loadData() {
            try {
                products = JSON.parse(localStorage.getItem('kasir_products')) || getDefaultProducts();
                transactions = JSON.parse(localStorage.getItem('kasir_transactions')) || [];
                console.log('Data loaded:', { products: products.length, transactions: transactions.length });
            } catch (error) {
                console.error('Error loading data:', error);
                products = getDefaultProducts();
                transactions = [];
            }
        }
        
        function getDefaultProducts() {
            return [
                { id: 1, name: 'Beras 5kg', price: 60000, stock: 50, category: 'Sembako' },
                { id: 2, name: 'Minyak Goreng 2L', price: 35000, stock: 30, category: 'Sembako' },
                { id: 3, name: 'Gula Pasir 1kg', price: 15000, stock: 40, category: 'Sembako' },
                { id: 4, name: 'Telur 1kg', price: 28000, stock: 25, category: 'Sembako' },
                { id: 5, name: 'Sabun Mandi', price: 5000, stock: 100, category: 'Kebersihan' },
                { id: 6, name: 'Shampoo', price: 12000, stock: 50, category: 'Kebersihan' },
                { id: 7, name: 'Air Mineral 600ml', price: 3000, stock: 200, category: 'Minuman' },
                { id: 8, name: 'Kopi Sachet', price: 2000, stock: 150, category: 'Minuman' },
                { id: 9, name: 'Teh Celup', price: 1500, stock: 120, category: 'Minuman' },
                { id: 10, name: 'Biskuit', price: 8000, stock: 80, category: 'Snack' }
            ];
        }
        
        // ==================== NAVIGASI ====================
        function showTab(tabId) {
            // Update active button
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Show selected tab
            document.getElementById(tabId).classList.add('active');
            currentTab = tabId;
            
            // Load data for tab
            if (tabId === 'dashboard') {
                loadDashboard();
            } else if (tabId === 'kasir') {
                loadProductsForKasir();
                updateCartDisplay();
            } else if (tabId === 'produk') {
                loadProductsTable();
            } else if (tabId === 'laporan') {
                loadReport();
            }
        }
        
        // ==================== DASHBOARD ====================
        function loadDashboard() {
            const today = new Date().toLocaleDateString('id-ID');
            let salesToday = 0;
            let transactionsToday = 0;
            
            transactions.forEach(trans => {
                if (trans.date === today) {
                    salesToday += trans.total;
                    transactionsToday++;
                }
            });
            
            document.getElementById('salesToday').textContent = 'Rp ' + salesToday.toLocaleString('id-ID');
            document.getElementById('transactionsToday').textContent = transactionsToday;
            document.getElementById('totalProducts').textContent = products.length;
        }
        
        function refreshDashboard() {
            loadDashboard();
            showAlert('success', 'Dashboard diperbarui');
        }
        
        // ==================== KASIR ====================
        function loadProductsForKasir() {
            const container = document.getElementById('productsKasir');
            container.innerHTML = '';
            
            products.forEach(product => {
                const div = document.createElement('div');
                div.className = 'product-card';
                div.innerHTML = `
                    <div style="font-weight: bold;">${product.name}</div>
                    <div style="color: #27ae60; font-weight: bold;">Rp ${product.price.toLocaleString('id-ID')}</div>
                    <div style="font-size: 12px; color: #666;">Stok: ${product.stock}</div>
                `;
                div.onclick = () => addToCart(product);
                container.appendChild(div);
            });
        }
        
        function addToCart(product) {
            if (product.stock <= 0) {
                showAlert('error', 'Stok habis!');
                return;
            }
            
            const existing = cart.find(item => item.id === product.id);
            if (existing) {
                if (existing.quantity >= product.stock) {
                    showAlert('error', 'Stok tidak cukup!');
                    return;
                }
                existing.quantity++;
            } else {
                cart.push({
                    id: product.id,
                    name: product.name,
                    price: product.price,
                    quantity: 1
                });
            }
            
            updateCartDisplay();
            showAlert('success', `${product.name} ditambahkan`);
        }
        
        function updateCartDisplay() {
            const container = document.getElementById('cart');
            const totalEl = document.getElementById('totalAmount');
            const changeEl = document.getElementById('change');
            const paymentInput = document.getElementById('payment');
            
            if (cart.length === 0) {
                container.innerHTML = 'Keranjang kosong';
                totalEl.textContent = 'Rp 0';
                changeEl.textContent = 'Rp 0';
                return;
            }
            
            let html = '';
            let total = 0;
            
            cart.forEach((item, index) => {
                const itemTotal = item.price * item.quantity;
                total += itemTotal;
                
                html += `
                    <div style="display: flex; justify-content: space-between; padding: 5px 0; border-bottom: 1px solid #ddd;">
                        <div>
                            ${item.name} 
                            <button onclick="updateCartQty(${index}, -1)" style="padding: 2px 8px; margin: 0 5px;">-</button>
                            ${item.quantity}
                            <button onclick="updateCartQty(${index}, 1)" style="padding: 2px 8px; margin: 0 5px;">+</button>
                        </div>
                        <div>Rp ${itemTotal.toLocaleString('id-ID')}</div>
                    </div>
                `;
            });
            
            container.innerHTML = html;
            totalEl.textContent = 'Rp ' + total.toLocaleString('id-ID');
            
            // Update change
            const payment = parseFloat(paymentInput.value) || 0;
            const change = payment - total;
            changeEl.textContent = 'Rp ' + (change > 0 ? change.toLocaleString('id-ID') : '0');
        }
        
        function updateCartQty(index, delta) {
            const item = cart[index];
            const product = products.find(p => p.id === item.id);
            
            if (!product) return;
            
            const newQty = item.quantity + delta;
            
            if (newQty < 1) {
                cart.splice(index, 1);
            } else if (newQty > product.stock) {
                showAlert('error', 'Stok tidak cukup!');
                return;
            } else {
                item.quantity = newQty;
            }
            
            updateCartDisplay();
        }
        
        function clearCart() {
            if (cart.length === 0) return;
            
            if (confirm('Kosongkan keranjang?')) {
                cart = [];
                updateCartDisplay();
                showAlert('success', 'Keranjang dikosongkan');
            }
        }
        
        function processPayment() {
            if (cart.length === 0) {
                showAlert('error', 'Keranjang kosong!');
                return;
            }
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            const payment = parseFloat(document.getElementById('payment').value) || 0;
            
            if (payment < total) {
                showAlert('error', 'Pembayaran kurang!');
                return;
            }
            
            // Update stock
            cart.forEach(cartItem => {
                const product = products.find(p => p.id === cartItem.id);
                if (product) {
                    product.stock -= cartItem.quantity;
                }
            });
            
            // Save transaction
            const transaction = {
                id: 'TRX-' + Date.now(),
                date: new Date().toLocaleDateString('id-ID'),
                time: new Date().toLocaleTimeString('id-ID'),
                items: [...cart],
                total: total,
                payment: payment,
                change: payment - total
            };
            
            transactions.unshift(transaction);
            saveData();
            
            // Show success
            showAlert('success', `Pembayaran berhasil! Kembali: Rp ${(payment - total).toLocaleString('id-ID')}`);
            
            // Reset
            cart = [];
            document.getElementById('payment').value = '';
            updateCartDisplay();
            
            // Refresh data
            loadDashboard();
            loadProductsForKasir();
        }
        
        // ==================== PRODUK ====================
        function loadProductsTable() {
            const search = document.getElementById('searchProduct').value.toLowerCase();
            const container = document.getElementById('productsTable');
            
            const filtered = products.filter(p => 
                p.name.toLowerCase().includes(search) ||
                p.category.toLowerCase().includes(search)
            );
            
            let html = '';
            filtered.forEach((product, index) => {
                html += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${product.name}</td>
                        <td>Rp ${product.price.toLocaleString('id-ID')}</td>
                        <td>${product.stock}</td>
                        <td>${product.category}</td>
                        <td>
                            <button class="btn btn-primary" onclick="editProduct(${product.id})" style="padding: 5px 10px; font-size: 12px;">Edit</button>
                            <button class="btn btn-danger" onclick="deleteProduct(${product.id})" style="padding: 5px 10px; font-size: 12px;">Hapus</button>
                        </td>
                    </tr>
                `;
            });
            
            container.innerHTML = html || '<tr><td colspan="6" style="text-align: center;">Tidak ada produk</td></tr>';
        }
        
        function addProduct() {
            document.getElementById('productModal').style.display = 'flex';
            document.getElementById('productName').value = '';
            document.getElementById('productPrice').value = '';
            document.getElementById('productStock').value = '';
            document.getElementById('productCategory').value = 'Sembako';
        }
        
        function editProduct(id) {
            const product = products.find(p => p.id === id);
            if (!product) return;
            
            document.getElementById('productModal').style.display = 'flex';
            document.getElementById('productName').value = product.name;
            document.getElementById('productPrice').value = product.price;
            document.getElementById('productStock').value = product.stock;
            document.getElementById('productCategory').value = product.category;
            
            // Set ID untuk edit
            document.getElementById('productModal').dataset.productId = id;
        }
        
        function closeModal() {
            document.getElementById('productModal').style.display = 'none';
            delete document.getElementById('productModal').dataset.productId;
        }
        
        function saveProduct() {
            const name = document.getElementById('productName').value.trim();
            const price = parseInt(document.getElementById('productPrice').value);
            const stock = parseInt(document.getElementById('productStock').value);
            const category = document.getElementById('productCategory').value;
            const modal = document.getElementById('productModal');
            const productId = modal.dataset.productId;
            
            if (!name || !price || !stock) {
                showAlert('error', 'Harap isi semua field!');
                return;
            }
            
            if (productId) {
                // Edit existing
                const product = products.find(p => p.id == productId);
                if (product) {
                    product.name = name;
                    product.price = price;
                    product.stock = stock;
                    product.category = category;
                }
            } else {
                // Add new
                const newId = products.length > 0 ? Math.max(...products.map(p => p.id)) + 1 : 1;
                products.push({
                    id: newId,
                    name: name,
                    price: price,
                    stock: stock,
                    category: category
                });
            }
            
            saveData();
            closeModal();
            loadProductsTable();
            loadProductsForKasir();
            showAlert('success', 'Produk disimpan!');
        }
        
        function deleteProduct(id) {
            if (!confirm('Hapus produk ini?')) return;
            
            const index = products.findIndex(p => p.id === id);
            if (index !== -1) {
                products.splice(index, 1);
                saveData();
                loadProductsTable();
                loadProductsForKasir();
                showAlert('success', 'Produk dihapus!');
            }
        }
        
        // ==================== LAPORAN ====================
        function loadReport() {
            const type = document.getElementById('reportType').value;
            const container = document.getElementById('reportTable');
            
            let filtered = [...transactions];
            
            if (type === 'today') {
                const today = new Date().toLocaleDateString('id-ID');
                filtered = transactions.filter(t => t.date === today);
            } else if (type === 'week') {
                const weekAgo = new Date();
                weekAgo.setDate(weekAgo.getDate() - 7);
                filtered = transactions.filter(t => {
                    const transDate = new Date(t.date.split('/').reverse().join('-'));
                    return transDate >= weekAgo;
                });
            }
            
            let html = '';
            filtered.forEach((trans, index) => {
                html += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${trans.date}<br><small>${trans.time}</small></td>
                        <td>${trans.id}</td>
                        <td>Rp ${trans.total.toLocaleString('id-ID')}</td>
                        <td>Rp ${trans.payment.toLocaleString('id-ID')}</td>
                        <td>Rp ${trans.change.toLocaleString('id-ID')}</td>
                    </tr>
                `;
            });
            
            container.innerHTML = html || '<tr><td colspan="6" style="text-align: center;">Tidak ada transaksi</td></tr>';
        }
        
        // ==================== EXPORT EXCEL ====================
        function exportDashboard() {
            exportToExcel('dashboard');
        }
        
        function exportProducts() {
            exportToExcel('products');
        }
        
        function exportReport() {
            exportToExcel('report');
        }
        
        function exportData(type) {
            exportToExcel(type);
        }
        
        function exportByDate() {
            const date = document.getElementById('exportDate').value;
            if (!date) {
                showAlert('error', 'Pilih tanggal terlebih dahulu');
                return;
            }
            
            exportToExcel('date', date);
        }
        
        function exportToExcel(type, date = null) {
            try {
                // Buat data untuk export
                let data = [];
                let filename = '';
                
                switch(type) {
                    case 'dashboard':
                        const today = new Date().toLocaleDateString('id-ID');
                        const todaySales = transactions
                            .filter(t => t.date === today)
                            .reduce((sum, t) => sum + t.total, 0);
                        
                        data = [
                            ['LAPORAN DASHBOARD'],
                            ['Tanggal', new Date().toLocaleDateString('id-ID')],
                            [''],
                            ['Penjualan Hari Ini', `Rp ${todaySales.toLocaleString('id-ID')}`],
                            ['Transaksi Hari Ini', transactions.filter(t => t.date === today).length],
                            ['Total Produk', products.length],
                            [''],
                            ['Rekap Produk Terlaris']
                        ];
                        
                        // Tambah produk terlaris
                        let productSales = {};
                        transactions.forEach(t => {
                            t.items.forEach(item => {
                                productSales[item.name] = (productSales[item.name] || 0) + item.quantity;
                            });
                        });
                        
                        Object.entries(productSales)
                            .sort((a, b) => b[1] - a[1])
                            .slice(0, 10)
                            .forEach(([name, qty]) => {
                                data.push([name, qty]);
                            });
                        
                        filename = `Dashboard_${new Date().toISOString().split('T')[0]}.csv`;
                        break;
                        
                    case 'products':
                        data = [
                            ['DATA PRODUK'],
                            ['Tanggal Export', new Date().toLocaleDateString('id-ID')],
                            [''],
                            ['No', 'Nama Produk', 'Harga', 'Stok', 'Kategori']
                        ];
                        
                        products.forEach((product, index) => {
                            data.push([
                                index + 1,
                                product.name,
                                product.price,
                                product.stock,
                                product.category
                            ]);
                        });
                        
                        filename = `Produk_${new Date().toISOString().split('T')[0]}.csv`;
                        break;
                        
                    case 'report':
                        const reportType = document.getElementById('reportType').value;
                        let reportTransactions = [...transactions];
                        
                        if (reportType === 'today') {
                            const today = new Date().toLocaleDateString('id-ID');
                            reportTransactions = transactions.filter(t => t.date === today);
                        } else if (reportType === 'week') {
                            const weekAgo = new Date();
                            weekAgo.setDate(weekAgo.getDate() - 7);
                            reportTransactions = transactions.filter(t => {
                                const transDate = new Date(t.date.split('/').reverse().join('-'));
                                return transDate >= weekAgo;
                            });
                        }
                        
                        data = [
                            ['LAPORAN TRANSAKSI'],
                            ['Periode', reportType === 'today' ? 'Hari Ini' : reportType === 'week' ? 'Minggu Ini' : 'Semua'],
                            [''],
                            ['No', 'Tanggal', 'ID Transaksi', 'Items', 'Total', 'Bayar', 'Kembali']
                        ];
                        
                        reportTransactions.forEach((trans, index) => {
                            const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join(', ');
                            data.push([
                                index + 1,
                                trans.date,
                                trans.id,
                                items,
                                trans.total,
                                trans.payment,
                                trans.change
                            ]);
                        });
                        
                        filename = `Laporan_${reportType}_${new Date().toISOString().split('T')[0]}.csv`;
                        break;
                        
                    case 'transactions':
                        data = [
                            ['DATA TRANSAKSI'],
                            ['Total Transaksi', transactions.length],
                            [''],
                            ['No', 'Tanggal', 'ID Transaksi', 'Items', 'Total', 'Bayar', 'Kembali']
                        ];
                        
                        transactions.forEach((trans, index) => {
                            const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join(', ');
                            data.push([
                                index + 1,
                                trans.date,
                                trans.id,
                                items,
                                trans.total,
                                trans.payment,
                                trans.change
                            ]);
                        });
                        
                        filename = `Transaksi_${new Date().toISOString().split('T')[0]}.csv`;
                        break;
                        
                    case 'all':
                        // Export semua data dalam beberapa sheet (simulasi)
                        data = [
                            ['DATA LENGKAP KASIR'],
                            ['Tanggal Export', new Date().toLocaleDateString('id-ID')],
                            [''],
                            ['=== PRODUK ==='],
                            ['No', 'Nama', 'Harga', 'Stok', 'Kategori']
                        ];
                        
                        products.forEach((p, i) => {
                            data.push([i + 1, p.name, p.price, p.stock, p.category]);
                        });
                        
                        data.push(['', '']);
                        data.push(['=== TRANSAKSI ===']);
                        data.push(['No', 'Tanggal', 'ID', 'Total']);
                        
                        transactions.forEach((t, i) => {
                            data.push([i + 1, t.date, t.id, t.total]);
                        });
                        
                        filename = `Backup_Kasir_${new Date().toISOString().split('T')[0]}.csv`;
                        break;
                        
                    case 'date':
                        if (!date) {
                            showAlert('error', 'Tanggal tidak valid');
                            return;
                        }
                        
                        const selectedDate = new Date(date).toLocaleDateString('id-ID');
                        const dateTransactions = transactions.filter(t => t.date === selectedDate);
                        
                        data = [
                            ['LAPORAN TANGGAL'],
                            ['Tanggal', selectedDate],
                            ['Jumlah Transaksi', dateTransactions.length],
                            [''],
                            ['No', 'ID Transaksi', 'Items', 'Total', 'Bayar', 'Kembali']
                        ];
                        
                        dateTransactions.forEach((trans, index) => {
                            const items = trans.items.map(item => `${item.name} (${item.quantity}x)`).join(', ');
                            data.push([
                                index + 1,
                                trans.id,
                                items,
                                trans.total,
                                trans.payment,
                                trans.change
                            ]);
                        });
                        
                        filename = `Laporan_${date}.csv`;
                        break;
                        
                    default:
                        showAlert('error', 'Jenis export tidak dikenal');
                        return;
                }
                
                // Convert to CSV
                const csv = data.map(row => 
                    row.map(cell => {
                        // Escape quotes and wrap in quotes if contains comma
                        if (typeof cell === 'string' && (cell.includes(',') || cell.includes('"'))) {
                            return '"' + cell.replace(/"/g, '""') + '"';
                        }
                        return cell;
                    }).join(',')
                ).join('\n');
                
                // Create and download file
                const blob = new Blob(['\ufeff' + csv], { type: 'text/csv;charset=utf-8' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = filename;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
                
                showAlert('success', `Data berhasil diexport: ${filename}`);
                
            } catch (error) {
                console.error('Export error:', error);
                showAlert('error', 'Gagal export: ' + error.message);
            }
        }
        
        // ==================== UTILITAS ====================
        function saveData() {
            try {
                localStorage.setItem('kasir_products', JSON.stringify(products));
                localStorage.setItem('kasir_transactions', JSON.stringify(transactions));
            } catch (error) {
                console.error('Save error:', error);
            }
        }
        
        function showAlert(type, message) {
            const alertDiv = document.getElementById('alert');
            alertDiv.textContent = message;
            alertDiv.className = `alert alert-${type}`;
            alertDiv.style.display = 'block';
            
            setTimeout(() => {
                alertDiv.style.display = 'none';
            }, 3000);
        }
        
        function showLoading(show) {
            document.getElementById('loading').style.display = show ? 'flex' : 'none';
        }
        
        // ==================== START APLIKASI ====================
        // Inisialisasi saat halaman dimuat
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Memulai aplikasi...');
            showLoading(true);
            
            // Load data
            loadData();
            
            // Setup event listeners
            document.getElementById('searchProduct').addEventListener('input', loadProductsTable);
            document.getElementById('payment').addEventListener('input', updateCartDisplay);
            
            // Load initial data
            loadDashboard();
            loadProductsForKasir();
            loadProductsTable();
            loadReport();
            
            // Hide loading
            setTimeout(() => {
                showLoading(false);
                showAlert('success', 'Aplikasi siap digunakan!');
            }, 500);
            
            console.log('Aplikasi siap');
        });
    </script>
</body>
</html>
