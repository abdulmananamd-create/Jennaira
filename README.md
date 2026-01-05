# Jennaira
Aplikasi kasir sederhana
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Kasir dengan Print Struk</title>
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
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background-color: #2c3e50;
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        header h1 {
            font-size: 28px;
            margin-bottom: 5px;
        }
        
        header p {
            opacity: 0.8;
        }
        
        .app-content {
            display: flex;
            flex-wrap: wrap;
        }
        
        .product-section {
            flex: 1;
            min-width: 300px;
            padding: 20px;
            border-right: 1px solid #eee;
        }
        
        .cart-section {
            flex: 1;
            min-width: 300px;
            padding: 20px;
        }
        
        .section-title {
            font-size: 20px;
            color: #2c3e50;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #3498db;
        }
        
        .product-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .product-card {
            background-color: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid #eaeaea;
        }
        
        .product-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
            background-color: #e8f4fc;
        }
        
        .product-name {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .product-price {
            color: #27ae60;
            font-weight: bold;
            font-size: 16px;
        }
        
        .product-category {
            font-size: 12px;
            color: #7f8c8d;
            margin-top: 5px;
        }
        
        .cart-items {
            max-height: 300px;
            overflow-y: auto;
            margin-bottom: 20px;
            border: 1px solid #eee;
            border-radius: 5px;
            padding: 10px;
        }
        
        .cart-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        
        .cart-item:last-child {
            border-bottom: none;
        }
        
        .item-name {
            flex: 2;
        }
        
        .item-qty {
            flex: 1;
            text-align: center;
        }
        
        .item-price {
            flex: 1;
            text-align: right;
            font-weight: bold;
        }
        
        .cart-summary {
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        
        .summary-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .summary-total {
            font-size: 20px;
            font-weight: bold;
            color: #2c3e50;
            border-top: 2px solid #3498db;
            padding-top: 10px;
            margin-top: 10px;
        }
        
        .button-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        button {
            padding: 12px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.2s;
            flex: 1;
            min-width: 120px;
        }
        
        .btn-primary {
            background-color: #3498db;
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #2980b9;
        }
        
        .btn-success {
            background-color: #27ae60;
            color: white;
        }
        
        .btn-success:hover {
            background-color: #219653;
        }
        
        .btn-warning {
            background-color: #f39c12;
            color: white;
        }
        
        .btn-warning:hover {
            background-color: #d68910;
        }
        
        .btn-danger {
            background-color: #e74c3c;
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #c0392b;
        }
        
        .receipt-section {
            margin-top: 30px;
            padding: 20px;
            border-top: 1px solid #eee;
        }
        
        .receipt {
            background-color: white;
            border: 1px dashed #ccc;
            padding: 20px;
            max-width: 400px;
            margin: 0 auto;
            font-family: 'Courier New', monospace;
            display: none;
        }
        
        .receipt-header {
            text-align: center;
            margin-bottom: 15px;
        }
        
        .receipt-line {
            border-top: 1px dashed #333;
            margin: 10px 0;
        }
        
        .receipt-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
        }
        
        .print-options {
            text-align: center;
            margin-top: 20px;
            display: none;
        }
        
        @media print {
            body * {
                visibility: hidden;
            }
            .receipt-print, .receipt-print * {
                visibility: visible;
            }
            .receipt-print {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                padding: 10px;
                font-size: 12px;
            }
            .no-print {
                display: none !important;
            }
            .receipt-print .receipt {
                border: none;
                box-shadow: none;
                max-width: 100%;
                padding: 0;
            }
        }
        
        @media (max-width: 768px) {
            .app-content {
                flex-direction: column;
            }
            
            .product-section {
                border-right: none;
                border-bottom: 1px solid #eee;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Aplikasi Kasir dengan Print Struk</h1>
            <p>Untuk toko retail atau usaha kecil menengah</p>
        </header>
        
        <div class="app-content">
            <section class="product-section">
                <h2 class="section-title">Daftar Produk</h2>
                <div class="product-list" id="productList">
                    <!-- Produk akan diisi oleh JavaScript -->
                </div>
                
                <h2 class="section-title">Tambah Produk Manual</h2>
                <div class="manual-add">
                    <div style="margin-bottom: 10px;">
                        <input type="text" id="productName" placeholder="Nama Produk" style="width: 100%; padding: 10px; margin-bottom: 10px; border: 1px solid #ddd; border-radius: 5px;">
                        <input type="number" id="productPrice" placeholder="Harga" style="width: 100%; padding: 10px; margin-bottom: 10px; border: 1px solid #ddd; border-radius: 5px;">
                        <button class="btn-primary" onclick="addManualProduct()">Tambah ke Keranjang</button>
                    </div>
                </div>
            </section>
            
            <section class="cart-section">
                <h2 class="section-title">Keranjang Belanja</h2>
                <div class="cart-items" id="cartItems">
                    <!-- Item keranjang akan diisi oleh JavaScript -->
                    <div style="text-align: center; padding: 20px; color: #7f8c8d;">
                        Keranjang kosong. Klik produk untuk menambahkannya.
                    </div>
                </div>
                
                <div class="cart-summary">
                    <div class="summary-row">
                        <span>Subtotal:</span>
                        <span id="subtotal">Rp 0</span>
                    </div>
                    <div class="summary-row">
                        <span>Pajak (10%):</span>
                        <span id="tax">Rp 0</span>
                    </div>
                    <div class="summary-row summary-total">
                        <span>TOTAL:</span>
                        <span id="total">Rp 0</span>
                    </div>
                </div>
                
                <div class="button-group">
                    <button class="btn-success" onclick="processPayment()">Proses Pembayaran</button>
                    <button class="btn-warning" onclick="clearCart()">Kosongkan Keranjang</button>
                    <button class="btn-danger" onclick="cancelTransaction()">Batalkan Transaksi</button>
                </div>
            </section>
        </div>
        
        <section class="receipt-section no-print">
            <h2 class="section-title">Struk Pembayaran</h2>
            <div class="receipt" id="receipt">
                <div class="receipt-header">
                    <h3>TOKO MAKMUR SEJAHTERA</h3>
                    <p>Jl. Contoh No. 123, Kota Contoh</p>
                    <p>Telp: (021) 123-4567</p>
                    <p>================================</p>
                    <p id="receiptDate">Tanggal: </p>
                    <p id="receiptNumber">No. Struk: </p>
                </div>
                <div id="receiptItems">
                    <!-- Item struk akan diisi oleh JavaScript -->
                </div>
                <div class="receipt-line"></div>
                <div class="receipt-item">
                    <span>Subtotal:</span>
                    <span id="receiptSubtotal">Rp 0</span>
                </div>
                <div class="receipt-item">
                    <span>Pajak (10%):</span>
                    <span id="receiptTax">Rp 0</span>
                </div>
                <div class="receipt-item" style="font-weight: bold;">
                    <span>TOTAL:</span>
                    <span id="receiptTotal">Rp 0</span>
                </div>
                <div class="receipt-item">
                    <span>Bayar:</span>
                    <span id="receiptPaid">Rp 0</span>
                </div>
                <div class="receipt-item">
                    <span>Kembali:</span>
                    <span id="receiptChange">Rp 0</span>
                </div>
                <div style="text-align: center; margin-top: 20px;">
                    <p>Terima kasih atas kunjungan Anda</p>
                    <p>** Barang yang sudah dibeli tidak dapat dikembalikan **</p>
                </div>
            </div>
            
            <div class="print-options" id="printOptions">
                <button class="btn-primary" onclick="printReceipt()">Cetak Struk</button>
                <button class="btn-warning" onclick="saveReceipt()">Simpan sebagai PDF</button>
                <button class="btn-danger" onclick="closeReceipt()">Tutup Struk</button>
            </div>
        </section>
    </div>

    <!-- Area untuk print saja -->
    <div id="printArea" style="display: none;"></div>

    <script>
        // Data produk
        const products = [
            { id: 1, name: "Beras 5kg", price: 60000, category: "Sembako" },
            { id: 2, name: "Minyak Goreng 2L", price: 35000, category: "Sembako" },
            { id: 3, name: "Gula Pasir 1kg", price: 15000, category: "Sembako" },
            { id: 4, name: "Telur 1kg", price: 28000, category: "Sembako" },
            { id: 5, name: "Sabun Mandi", price: 5000, category: "Kebersihan" },
            { id: 6, name: "Shampoo", price: 12000, category: "Kebersihan" },
            { id: 7, name: "Pasta Gigi", price: 8000, category: "Kebersihan" },
            { id: 8, name: "Air Mineral 600ml", price: 3000, category: "Minuman" },
            { id: 9, name: "Kopi Sachet", price: 2000, category: "Minuman" },
            { id: 10, name: "Teh Celup", price: 1500, category: "Minuman" },
            { id: 11, name: "Biskuit", price: 8000, category: "Snack" },
            { id: 12, name: "Cokelat Batang", price: 10000, category: "Snack" }
        ];

        // Keranjang belanja
        let cart = [];
        let cartTotal = 0;
        let subtotal = 0;
        let tax = 0;
        let receiptNumber = 1;

        // Inisialisasi aplikasi
        document.addEventListener('DOMContentLoaded', function() {
            renderProducts();
            updateCart();
            
            // Generate nomor struk awal
            generateReceiptNumber();
        });

        // Render daftar produk
        function renderProducts() {
            const productList = document.getElementById('productList');
            productList.innerHTML = '';
            
            products.forEach(product => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card';
                productCard.innerHTML = `
                    <div class="product-name">${product.name}</div>
                    <div class="product-price">Rp ${product.price.toLocaleString('id-ID')}</div>
                    <div class="product-category">${product.category}</div>
                `;
                productCard.addEventListener('click', () => addToCart(product));
                productList.appendChild(productCard);
            });
        }

        // Tambah produk ke keranjang
        function addToCart(product) {
            const existingItem = cart.find(item => item.id === product.id);
            
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({
                    id: product.id,
                    name: product.name,
                    price: product.price,
                    quantity: 1
                });
            }
            
            updateCart();
        }

        // Tambah produk manual ke keranjang
        function addManualProduct() {
            const nameInput = document.getElementById('productName');
            const priceInput = document.getElementById('productPrice');
            
            const name = nameInput.value.trim();
            const price = parseInt(priceInput.value);
            
            if (!name || isNaN(price) || price <= 0) {
                alert('Masukkan nama dan harga produk yang valid');
                return;
            }
            
            // Cari ID unik untuk produk baru
            const newId = products.length > 0 ? Math.max(...products.map(p => p.id)) + 1 : 1;
            
            cart.push({
                id: newId,
                name: name,
                price: price,
                quantity: 1
            });
            
            // Reset input
            nameInput.value = '';
            priceInput.value = '';
            
            updateCart();
        }

        // Update tampilan keranjang
        function updateCart() {
            const cartItems = document.getElementById('cartItems');
            
            if (cart.length === 0) {
                cartItems.innerHTML = '<div style="text-align: center; padding: 20px; color: #7f8c8d;">Keranjang kosong. Klik produk untuk menambahkannya.</div>';
                updateSummary(0, 0, 0);
                return;
            }
            
            // Hitung total
            subtotal = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            tax = subtotal * 0.1; // Pajak 10%
            cartTotal = subtotal + tax;
            
            // Render item keranjang
            cartItems.innerHTML = '';
            cart.forEach(item => {
                const cartItem = document.createElement('div');
                cartItem.className = 'cart-item';
                cartItem.innerHTML = `
                    <div class="item-name">${item.name}</div>
                    <div class="item-qty">
                        <button onclick="changeQuantity(${item.id}, -1)" style="padding: 2px 8px; margin-right: 5px;">-</button>
                        ${item.quantity}
                        <button onclick="changeQuantity(${item.id}, 1)" style="padding: 2px 8px; margin-left: 5px;">+</button>
                    </div>
                    <div class="item-price">Rp ${(item.price * item.quantity).toLocaleString('id-ID')}</div>
                `;
                cartItems.appendChild(cartItem);
            });
            
            updateSummary(subtotal, tax, cartTotal);
        }

        // Ubah jumlah item
        function changeQuantity(productId, delta) {
            const itemIndex = cart.findIndex(item => item.id === productId);
            
            if (itemIndex !== -1) {
                cart[itemIndex].quantity += delta;
                
                // Hapus item jika jumlahnya 0
                if (cart[itemIndex].quantity <= 0) {
                    cart.splice(itemIndex, 1);
                }
                
                updateCart();
            }
        }

        // Update ringkasan belanja
        function updateSummary(sub, taxAmount, total) {
            document.getElementById('subtotal').textContent = `Rp ${sub.toLocaleString('id-ID')}`;
            document.getElementById('tax').textContent = `Rp ${taxAmount.toLocaleString('id-ID')}`;
            document.getElementById('total').textContent = `Rp ${total.toLocaleString('id-ID')}`;
        }

        // Generate nomor struk
        function generateReceiptNumber() {
            const now = new Date();
            const year = now.getFullYear().toString().substr(-2);
            const month = (now.getMonth() + 1).toString().padStart(2, '0');
            const day = now.getDate().toString().padStart(2, '0');
            const number = receiptNumber.toString().padStart(4, '0');
            
            return `STR${year}${month}${day}${number}`;
        }

        // Proses pembayaran
        function processPayment() {
            if (cart.length === 0) {
                alert('Keranjang belanja kosong!');
                return;
            }
            
            const total = cartTotal;
            const payment = prompt(`Total yang harus dibayar: Rp ${total.toLocaleString('id-ID')}\nMasukkan jumlah uang yang dibayarkan:`, total);
            
            if (payment === null) return;
            
            const paid = parseFloat(payment);
            
            if (isNaN(paid) || paid < total) {
                alert('Jumlah pembayaran tidak valid atau kurang dari total!');
                return;
            }
            
            const change = paid - total;
            
            // Tampilkan struk
            generateReceipt(total, paid, change);
            
            // Tampilkan opsi print
            document.getElementById('printOptions').style.display = 'block';
            
            // Scroll ke struk
            document.getElementById('receipt').scrollIntoView({ behavior: 'smooth' });
            
            // Reset keranjang setelah pembayaran
            setTimeout(() => {
                cart = [];
                updateCart();
                alert(`Pembayaran berhasil! Kembalian: Rp ${change.toLocaleString('id-ID')}`);
            }, 500);
        }

        // Generate struk
        function generateReceipt(total, paid, change) {
            const receipt = document.getElementById('receipt');
            const receiptItems = document.getElementById('receiptItems');
            const receiptNum = generateReceiptNumber();
            
            // Update nomor struk
            document.getElementById('receiptNumber').textContent = `No. Struk: ${receiptNum}`;
            
            // Update tanggal
            const now = new Date();
            const dateStr = now.toLocaleDateString('id-ID', {
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
            document.getElementById('receiptDate').textContent = dateStr;
            
            // Update item struk
            receiptItems.innerHTML = '';
            cart.forEach(item => {
                const itemTotal = item.price * item.quantity;
                const receiptItem = document.createElement('div');
                receiptItem.className = 'receipt-item';
                receiptItem.innerHTML = `
                    <span>${item.name} (${item.quantity}x)</span>
                    <span>Rp ${itemTotal.toLocaleString('id-ID')}</span>
                `;
                receiptItems.appendChild(receiptItem);
            });
            
            // Update total struk
            document.getElementById('receiptSubtotal').textContent = `Rp ${subtotal.toLocaleString('id-ID')}`;
            document.getElementById('receiptTax').textContent = `Rp ${tax.toLocaleString('id-ID')}`;
            document.getElementById('receiptTotal').textContent = `Rp ${total.toLocaleString('id-ID')}`;
            document.getElementById('receiptPaid').textContent = `Rp ${paid.toLocaleString('id-ID')}`;
            document.getElementById('receiptChange').textContent = `Rp ${change.toLocaleString('id-ID')}`;
            
            // Tampilkan struk
            receipt.style.display = 'block';
            
            // Simpan data struk untuk print
            preparePrintContent(receiptNum, dateStr, total, paid, change);
            
            // Increment nomor struk untuk transaksi berikutnya
            receiptNumber++;
        }

        // Siapkan konten untuk dicetak
        function preparePrintContent(receiptNum, dateStr, total, paid, change) {
            const printArea = document.getElementById('printArea');
            
            let itemsHtml = '';
            cart.forEach(item => {
                const itemTotal = item.price * item.quantity;
                itemsHtml += `
                    <div style="display: flex; justify-content: space-between; margin-bottom: 3px;">
                        <span>${item.name} (${item.quantity}x)</span>
                        <span>Rp ${itemTotal.toLocaleString('id-ID')}</span>
                    </div>
                `;
            });
            
            printArea.innerHTML = `
                <div class="receipt-print">
                    <div class="receipt">
                        <div style="text-align: center; margin-bottom: 10px;">
                            <h3 style="margin: 5px 0;">TOKO MAKMUR SEJAHTERA</h3>
                            <p style="margin: 2px 0; font-size: 10px;">Jl. Contoh No. 123, Kota Contoh</p>
                            <p style="margin: 2px 0; font-size: 10px;">Telp: (021) 123-4567</p>
                            <p style="margin: 5px 0;">===============================</p>
                            <p style="margin: 2px 0; font-size: 10px;">${dateStr}</p>
                            <p style="margin: 2px 0; font-size: 10px;">${receiptNum}</p>
                        </div>
                        <div style="margin-bottom: 10px;">
                            ${itemsHtml}
                        </div>
                        <div style="border-top: 1px dashed #000; margin: 8px 0;"></div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 3px;">
                            <span>Subtotal:</span>
                            <span>Rp ${subtotal.toLocaleString('id-ID')}</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 3px;">
                            <span>Pajak (10%):</span>
                            <span>Rp ${tax.toLocaleString('id-ID')}</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 5px; font-weight: bold;">
                            <span>TOTAL:</span>
                            <span>Rp ${total.toLocaleString('id-ID')}</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 3px;">
                            <span>Bayar:</span>
                            <span>Rp ${paid.toLocaleString('id-ID')}</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; margin-bottom: 10px; font-weight: bold;">
                            <span>Kembali:</span>
                            <span>Rp ${change.toLocaleString('id-ID')}</span>
                        </div>
                        <div style="text-align: center; margin-top: 15px;">
                            <p style="margin: 3px 0; font-size: 10px;">Terima kasih atas kunjungan Anda</p>
                            <p style="margin: 3px 0; font-size: 9px;">** Barang yang sudah dibeli tidak dapat dikembalikan **</p>
                        </div>
                    </div>
                </div>
            `;
        }

        // Fungsi untuk mencetak struk
        function printReceipt() {
            window.print();
        }

        // Fungsi untuk menyimpan sebagai PDF (simulasi)
        function saveReceipt() {
            alert('Fitur simpan sebagai PDF akan membuka jendela print. Pilih "Save as PDF" di tujuan printer.');
            setTimeout(() => {
                window.print();
            }, 500);
        }

        // Tutup struk
        function closeReceipt() {
            document.getElementById('receipt').style.display = 'none';
            document.getElementById('printOptions').style.display = 'none';
        }

        // Kosongkan keranjang
        function clearCart() {
            if (cart.length === 0) {
                alert('Keranjang sudah kosong!');
                return;
            }
            
            if (confirm('Apakah Anda yakin ingin mengosongkan keranjang?')) {
                cart = [];
                updateCart();
            }
        }

        // Batalkan transaksi
        function cancelTransaction() {
            if (cart.length === 0) {
                alert('Tidak ada transaksi untuk dibatalkan!');
                return;
            }
            
            if (confirm('Apakah Anda yakin ingin membatalkan transaksi ini?')) {
                cart = [];
                updateCart();
                document.getElementById('receipt').style.display = 'none';
                document.getElementById('printOptions').style.display = 'none';
                alert('Transaksi dibatalkan.');
            }
        }
    </script>
</body>
</html>
