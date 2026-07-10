<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator Kelulusan</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, Helvetica, Arial, sans-serif;
            background: linear-gradient(135deg, #eef2f3 0%, #8e9eab 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        .card {
            background-color: #ffffff;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            width: 100%;
            max-width: 420px;
            overflow: hidden;
            transition: transform 0.3s ease;
        }
        .card-header {
            background-color: #2c3e50;
            color: #ffffff;
            padding: 25px;
            text-align: center;
        }
        .card-header h2 {
            font-size: 22px;
            font-weight: 600;
            letter-spacing: 0.5px;
        }
        .card-header p {
            font-size: 13px;
            color: #bdc3c7;
            margin-top: 5px;
        }
        .card-body {
            padding: 30px 25px;
        }
        .form-group {
            margin-bottom: 22px;
        }
        .form-group label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            color: #34495e;
            margin-bottom: 8px;
        }
        .input-wrapper {
            position: relative;
        }
        .input-wrapper input {
            width: 100%;
            padding: 14px 16px;
            font-size: 16px;
            border: 2px solid #dcdde1;
            border-radius: 8px;
            color: #2c3e50;
            outline: none;
            transition: all 0.3s ease;
        }
        .input-wrapper input:focus {
            border-color: #3498db;
            box-shadow: 0 0 8px rgba(52, 152, 219, 0.2);
        }
        .badge-weight {
            display: inline-block;
            font-size: 11px;
            font-weight: 600;
            color: #7f8c8d;
            background-color: #f5f6fa;
            padding: 3px 8px;
            border-radius: 4px;
            margin-top: 6px;
        }
        .btn-submit {
            width: 100%;
            background-color: #3498db;
            color: #ffffff;
            border: none;
            padding: 15px;
            font-size: 16px;
            font-weight: 600;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s ease, transform 0.1s ease;
            box-shadow: 0 4px 12px rgba(52, 152, 219, 0.3);
        }
        .btn-submit:hover {
            background-color: #2980b9;
        }
        .btn-submit:active {
            transform: scale(0.98);
        }
        .result-box {
            margin-top: 25px;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            display: none;
            animation: fadeIn 0.4s ease forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .status-title {
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 5px;
            font-weight: bold;
        }
        .status-score {
            font-size: 32px;
            font-weight: 800;
        }
        .status-desc {
            font-size: 15px;
            margin-top: 5px;
            font-weight: 600;
        }
        .kompeten {
            background-color: #e3fcef;
            color: #0e6245;
            border: 1px solid #a3e9c9;
        }
        .belum-kompeten {
            background-color: #ffebe9;
            color: #b71e12;
            border: 1px solid #ffd0cc;
        }
    </style>
</head>
<body>

<div class="card">
    <div class="card-header">
        <h2>Kalkulator Kelulusan</h2>
        <p>Sistem Hitung Otomatis Nilai Akhir Peserta</p>
    </div>
    
    <div class="card-body">
        <div class="form-group">
            <label for="teori">Nilai Teori</label>
            <div class="input-wrapper">
                <input type="number" id="teori" min="0" max="100" placeholder="0 - 100" required autocomplete="off">
            </div>
            <span class="badge-weight">Bobot Nilai: 20%</span>
        </div>
        
        <div class="form-group">
            <label for="praktek">Nilai Praktek</label>
            <div class="input-wrapper">
                <input type="number" id="praktek" min="0" max="100" placeholder="0 - 100" required autocomplete="off">
            </div>
            <span class="badge-weight">Bobot Nilai: 80%</span>
        </div>
        
        <button type="button" id="btnHitung" class="btn-submit" onclick="hitungKelulusan()">Hitung Kelulusan</button>
        
        <div id="result" class="result-box"></div>
    </div>
</div>

<script>
    const inputTeori = document.getElementById('teori');
    const inputPraktek = document.getElementById('praktek');

    // Perbaikan Fitur Navigasi Enter (Menggunakan keydown + keyup untuk kompatibilitas penuh)
    inputTeori.addEventListener('keyup', function(event) {
        if (event.key === 'Enter' || event.keyCode === 13) {
            event.preventDefault();
            inputPraktek.focus();
            inputPraktek.select(); // Otomatis memblok teks di kolom kedua jika ada isinya
        }
    });

    inputPraktek.addEventListener('keyup', function(event) {
        if (event.key === 'Enter' || event.keyCode === 13) {
            event.preventDefault();
            hitungKelulusan();
        }
    });

    // Fungsi Perhitungan
    function hitungKelulusan() {
        let nilaiTeori = parseFloat(inputTeori.value);
        let nilaiPraktek = parseFloat(inputPraktek.value);
        let resultDiv = document.getElementById('result');

        // Validasi input kosong atau tidak valid
        if (isNaN(nilaiTeori) || isNaN(nilaiPraktek) || nilaiTeori < 0 || nilaiTeori > 100 || nilaiPraktek < 0 || nilaiPraktek > 100) {
            resultDiv.style.display = 'block';
            resultDiv.className = 'result-box belum-kompeten';
            resultDiv.innerHTML = '<div class="status-title">Peringatan</div><div class="status-desc" style="font-size:13px;">Masukkan nilai Teori & Praktek yang valid (0-100)</div>';
            return;
        }

        // Hitung Nilai Akhir (Teori 20%, Praktek 80%)
        let nilaiAkhir = (nilaiTeori * 0.2) + (nilaiPraktek * 0.8);
        nilaiAkhir = nilaiAkhir.toFixed(2);

        resultDiv.style.display = 'block';

        // Penentuan Kompeten (Minimal 80)
        if (nilaiAkhir >= 80) {
            resultDiv.className = 'result-box kompeten';
            resultDiv.innerHTML = `
                <div class="status-title">Nilai Akhir</div>
                <div class="status-score">${nilaiAkhir}</div>
                <div class="status-desc">KOMPETEN</div>
            `;
        } else {
            resultDiv.className = 'result-box belum-kompeten';
            resultDiv.innerHTML = `
                <div class="status-title">Nilai Akhir</div>
                <div class="status-score">${nilaiAkhir}</div>
                <div class="status-desc">BELUM KOMPETEN</div>
            `;
        }
    }
</script>

</body>
</html>
