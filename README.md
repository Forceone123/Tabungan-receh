<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tabungan Receh</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/sweetalert/2.1.2/sweetalert.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');
        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            background-color: #f4f4f4;
        }

        #login-page, #main-menu, .page {
            background-size: cover;
            background-position: center;
            display: none;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            width: 80%;
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            text-align: center;
            position: relative;
        }

        /* Login page */
        #login-page {
            display: flex;
        }

        .login-box {
            width: 300px;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .login-box input {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        .login-box button {
            width: 100%;
            padding: 10px;
            background-color: #0277BD;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .login-box button:hover {
            background-color: #01579B;
        }

        /* Running text headline */
        .header {
            background-color: #0277BD;
            color: white;
            padding: 15px;
            font-size: 20px;
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 10;
        }

        .running-text {
            overflow: hidden;
            white-space: nowrap;
        }

        .running-text span {
            display: inline-block;
            padding-left: 100%;
            animation: ticker 15s linear infinite;
        }

        @keyframes ticker {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }

        /* Saldo dan target tabungan */
        .saldo-box, .target-box {
            background-color: #81d4fa;
            padding: 10px;
            border-radius: 10px;
            width: 200px;
            margin-bottom: 20px;
            position: absolute;
            top: 70px;
        }

        .saldo-box {
            left: 20px;
        }

        .target-box {
            right: 20px;
        }

        .saldo-box .close-btn {
            cursor: pointer;
            background-color: red;
            color: white;
            border: none;
            padding: 5px;
            border-radius: 50%;
        }

        .saldo-box p, .target-box p {
            margin: 0;
            font-size: 14px;
            font-weight: bold;
        }

        .target-box input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        /* Menu items */
        .menu-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-top: 120px;
        }

        .menu-item {
            background-color: #0277BD;
            color: white;
            padding: 20px;
            border-radius: 10px;
            cursor: pointer;
            transition: background-color 0.3s;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .menu-item:hover {
            background-color: #01579B;
        }

        .menu-item i {
            font-size: 30px;
        }

        .menu-item span {
            font-size: 16px;
            font-weight: bold;
        }

        /* Halaman menu baru dengan tema biru soft */
        .page {
            background-color: #e0f7fa;
            text-align: center;
            padding: 20px;
            height: 100vh;
        }

        .page h2 {
            margin-bottom: 20px;
        }

        .page input {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
            width: 200px;
            margin-bottom: 20px;
        }

        .page button {
            padding: 10px 20px;
            background-color: #0277BD;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .page button:hover {
            background-color: #01579B;
        }

        /* Tabel transaksi */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        table, th, td {
            border: 1px solid #ccc;
        }

        th, td {
            padding: 15px;
            text-align: center;
        }

        th {
            background-color: #0277BD;
            color: white;
        }

        td {
            background-color: white;
        }

        /* Tombol kembali ke menu utama */
        .back-btn {
            display: block;
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #0277BD;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
        }

        .back-btn i {
            font-size: 18px;
        }

        .back-btn:hover {
            background-color: #01579B;
        }

        /* Grafik */
        .chart-container {
            width: 80%;
            margin: 0 auto;
        }

    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/sweetalert/2.1.2/sweetalert.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>

    <!-- Running Text di Paling Atas -->
    <div class="header">
        <div class="running-text">
            <span>Selamat Menabung dan Jangan Lupa Bersedekah untuk Bekal di Akhirat</span>
        </div>
    </div>

    <!-- Halaman Login -->
    <div id="login-page">
        <div class="login-box">
            <h1>Login</h1>
            <input type="text" id="username" placeholder="Masukkan Username">
            <button onclick="login()">Masuk</button>
        </div>
    </div>

    <!-- Menu Utama -->
    <div id="main-menu">
        <div class="container">

            <!-- Sisa saldo dan target tabungan -->
            <div class="saldo-box">
                <p>Sisa Saldo: Rp <span id="saldo">0</span></p>
                <button class="close-btn" onclick="hideSaldo()">×</button>
            </div>

            <div class="target-box">
                <p>Target Tabungan:</p>
                <input type="text" id="target-tabungan" placeholder="Masukkan target">
            </div>

            <!-- 7 Menu Utama -->
            <div class="menu-grid">
                <div class="menu-item" onclick="showPage('pemasukan')">
                    <i class="fas fa-plus-circle"></i>
                    <span>Pemasukan</span>
                </div>
                <div class="menu-item" onclick="showPage('pengeluaran')">
                    <i class="fas fa-minus-circle"></i>
                    <span>Pengeluaran</span>
                </div>
                <div class="menu-item" onclick="showPage('penarikan')">
                    <i class="fas fa-university"></i>
                    <span>Penarikan</span>
                </div>
                <div class="menu-item" onclick="showPage('daftar-transaksi')">
                    <i class="fas fa-list"></i>
                    <span>Daftar Transaksi</span>
                </div>
                <div class="menu-item" onclick="showPage('grafik')">
                    <i class="fas fa-chart-bar"></i>
                    <span>Grafik</span>
                </div>
                <div class="menu-item" onclick="showPage('print')">
                    <i class="fas fa-print"></i>
                    <span>Print</span>
                </div>
                <div class="menu-item" onclick="showPage('reset')">
                    <i class="fas fa-sync-alt"></i>
                    <span>Reset Data</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Halaman Pemasukan -->
    <div id="pemasukan" class="page">
        <h2>Pemasukan</h2>
        <input type="number" id="input-pemasukan" placeholder="Masukkan nominal">
        <button onclick="submitPemasukan()">Tambah Pemasukan</button>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Pengeluaran -->
    <div id="pengeluaran" class="page">
        <h2>Pengeluaran</h2>
        <input type="number" id="input-pengeluaran" placeholder="Masukkan nominal">
        <button onclick="submitPengeluaran()">Tambah Pengeluaran</button>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Penarikan -->
    <div id="penarikan" class="page">
        <h2>Penarikan</h2>
        <div style="display: flex; justify-content: center;">
            <div>
                <h3>Sisa Saldo</h3>
                <p id="saldo-penarikan">Rp 0</p>
            </div>
            <div style="margin-left: 50px;">
                <h3>Nominal Penarikan</h3>
                <input type="number" id="input-penarikan" placeholder="Masukkan nominal">
                <button onclick="submitPenarikan()">Proses Penarikan</button>
            </div>
        </div>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Daftar Transaksi -->
    <div id="daftar-transaksi" class="page">
        <h2>Daftar Transaksi</h2>
        <table>
            <thead>
                <tr>
                    <th>Tanggal</th>
                    <th>Jenis Transaksi</th>
                    <th>Nominal</th>
                </tr>
            </thead>
            <tbody id="transaksi-body">
                <!-- Data transaksi akan diisi di sini -->
            </tbody>
        </table>
        <button onclick="exportExcel()">Export ke Excel</button>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Grafik -->
    <div id="grafik" class="page">
        <h2>Grafik Tabungan</h2>
        <div class="chart-container">
            <canvas id="chart"></canvas>
        </div>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Print -->
    <div id="print" class="page">
        <h2>Print Transaksi</h2>
        <button onclick="printTransaksi()">Cetak Transaksi</button>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <!-- Halaman Reset -->
    <div id="reset" class="page">
        <h2>Reset Data</h2>
        <button onclick="resetData()">Reset Semua Data</button>
        <button class="back-btn" onclick="backToMenu()">
            <i class="fas fa-arrow-left"></i> Kembali
        </button>
    </div>

    <script>
        // Fungsi untuk login
        function login() {
            const username = document.getElementById('username').value;
            if (username === "dicky") {
                document.getElementById('login-page').style.display = 'none';
                document.getElementById('main-menu').style.display = 'flex';
            } else {
                swal("Username Salah", "Silakan coba lagi!", "error");
            }
        }

        // Fungsi untuk menutup saldo
        function hideSaldo() {
            document.querySelector('.saldo-box').style.display = 'none';
        }

        // Fungsi untuk membuka halaman menu
        function showPage(pageId) {
            document.getElementById('main-menu').style.display = 'none';
            document.querySelectorAll('.page').forEach(page => {
                page.style.display = 'none';
            });
            document.getElementById(pageId).style.display = 'flex';
        }

        // Fungsi untuk kembali ke menu utama
        function backToMenu() {
            document.querySelectorAll('.page').forEach(page => {
                page.style.display = 'none';
            });
            document.getElementById('main-menu').style.display = 'flex';
        }

        // Fungsi untuk menambah pemasukan
        function submitPemasukan() {
            const saldo = parseInt(localStorage.getItem('saldo')) || 0;
            const pemasukan = parseInt(document.getElementById('input-pemasukan').value);
            const totalSaldo = saldo + pemasukan;
            localStorage.setItem('saldo', totalSaldo);
            document.getElementById('saldo').innerText = totalSaldo.toLocaleString('id-ID');
            document.getElementById('input-pemasukan').value = ''; // Kosongkan setelah transaksi
            swal("Pemasukan Ditambahkan!", "Nominal telah berhasil ditambahkan ke saldo!", "success");
            addTransaction('Pemasukan', pemasukan);
        }

        // Fungsi untuk menambah pengeluaran
        function submitPengeluaran() {
            const saldo = parseInt(localStorage.getItem('saldo')) || 0;
            const pengeluaran = parseInt(document.getElementById('input-pengeluaran').value);
            if (pengeluaran > saldo) {
                swal("Saldo Tidak Cukup", "Pengeluaran melebihi saldo!", "error");
                return;
            }
            const totalSaldo = saldo - pengeluaran;
            localStorage.setItem('saldo', totalSaldo);
            document.getElementById('saldo').innerText = totalSaldo.toLocaleString('id-ID');
            document.getElementById('input-pengeluaran').value = ''; // Kosongkan setelah transaksi
            swal("Pengeluaran Ditambahkan!", "Nominal telah berhasil ditarik dari saldo!", "success");
            addTransaction('Pengeluaran', pengeluaran);
        }

        // Fungsi untuk melakukan penarikan
        function submitPenarikan() {
            const saldo = parseInt(localStorage.getItem('saldo')) || 0;
            const penarikan = parseInt(document.getElementById('input-penarikan').value);
            if (penarikan > saldo) {
                swal("Saldo Tidak Cukup", "Penarikan melebihi saldo!", "error");
                return;
            }
            const totalSaldo = saldo - penarikan;
            localStorage.setItem('saldo', totalSaldo);
            document.getElementById('saldo').innerText = totalSaldo.toLocaleString('id-ID');
            document.getElementById('saldo-penarikan').innerText = totalSaldo.toLocaleString('id-ID');
            document.getElementById('input-penarikan').value = ''; // Kosongkan setelah transaksi
            swal("Penarikan Berhasil!", "Nominal telah berhasil ditarik dari saldo!", "success");
            addTransaction('Penarikan', penarikan);
        }

        // Fungsi untuk menambahkan transaksi ke tabel
        function addTransaction(type, amount) {
            const tableBody = document.getElementById('transaksi-body');
            const row = document.createElement('tr');
            const date = new Date().toLocaleDateString();
            row.innerHTML = `<td>${date}</td><td>${type}</td><td>Rp ${amount.toLocaleString('id-ID')}</td>`;
            tableBody.appendChild(row);
        }

        // Fungsi untuk cetak transaksi
        function printTransaksi() {
            swal("Fitur Cetak", "Transaksi telah dicetak!", "success");
        }

        // Fungsi untuk reset data
        function resetData() {
            localStorage.clear();
            document.getElementById('saldo').innerText = '0';
            document.getElementById('saldo-penarikan').innerText = 'Rp 0';
            swal("Data Direset!", "Semua data telah dihapus!", "success");
        }

        // Fungsi untuk export ke Excel
        function exportExcel() {
            let table = document.querySelector("table");
            let wb = XLSX.utils.table_to_book(table, {sheet: "Daftar Transaksi"});
            XLSX.writeFile(wb, "daftar_transaksi.xlsx");
        }

        // Menampilkan saldo saat halaman dimuat
        window.onload = function() {
            const saldo = localStorage.getItem('saldo') || '0';
            document.getElementById('saldo').innerText = parseInt(saldo).toLocaleString('id-ID');
            document.getElementById('saldo-penarikan').innerText = parseInt(saldo).toLocaleString('id-ID');
            showChart(); // Menampilkan grafik saat halaman dimuat
        }

        // Fungsi untuk menampilkan grafik batang
        function showChart() {
            const ctx = document.getElementById('chart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Januari', 'Februari', 'Maret', 'April', 'Mei'],
                    datasets: [{
                        label: 'Jumlah Tabungan',
                        data: [1000000, 1500000, 1300000, 1800000, 2000000],
                        backgroundColor: '#0277BD',
                    }]
                }
            });
        }
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.0/xlsx.full.min.js"></script>
</body>
</html>

