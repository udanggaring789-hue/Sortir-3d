
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Penyortir Angka 3D</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #101827;
    color: white;
    min-height: 100vh;
    padding: 25px 15px;
}

.container {
    max-width: 650px;
    margin: 35px auto;
    background: #1c293b;
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 10px 35px rgba(0,0,0,0.3);
}

h1 {
    text-align: center;
    font-size: 26px;
    margin-bottom: 10px;
    color: #38bdf8;
}

.subtitle {
    text-align: center;
    color: #a9b8ca;
    font-size: 14px;
    margin-bottom: 30px;
}

label {
    display: block;
    margin-bottom: 10px;
    font-weight: bold;
    color: #e2e8f0;
}

textarea {
    width: 100%;
    min-height: 150px;
    resize: vertical;
    padding: 15px;
    border: 1px solid #3b4c63;
    border-radius: 10px;
    background: #101827;
    color: white;
    font-size: 16px;
    line-height: 1.6;
    outline: none;
}

textarea:focus {
    border-color: #38bdf8;
}

button {
    width: 100%;
    padding: 15px;
    margin-top: 15px;
    border: none;
    border-radius: 10px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
}

.process-btn {
    background: #0ea5e9;
    color: white;
}

.process-btn:hover {
    background: #0284c7;
}

.copy-btn {
    background: #16a34a;
    color: white;
}

.copy-btn:hover {
    background: #15803d;
}

.result-box {
    margin-top: 25px;
}

.stats {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    margin: 15px 0;
}

.stat {
    flex: 1;
    background: #101827;
    padding: 12px;
    border-radius: 8px;
    text-align: center;
}

.stat strong {
    display: block;
    font-size: 22px;
    color: #38bdf8;
}

.stat span {
    font-size: 12px;
    color: #a9b8ca;
}

#result {
    min-height: 150px;
    background: #101827;
}

.message {
    text-align: center;
    color: #94a3b8;
    margin-top: 15px;
    font-size: 13px;
}

@media (max-width: 480px) {
    .container {
        padding: 18px;
    }

    h1 {
        font-size: 22px;
    }
}
</style>
</head>

<body>

<div class="container">

    <h1> SORTIR ANGKA 3D </h1>
    <p class="subtitle">
        Penyaring angka kembar berjejer otomatis
    </p>

    <label for="input">MASUKKAN ANGKA</label>

    <textarea
        id="input"
        placeholder="Contoh: 123*245*335*776*121*343"
    ></textarea>

    <button class="process-btn" onclick="proses()">
        PROSES SORTIR
    </button>

    <div class="result-box">

        <label for="result">HASIL SORTIR</label>

        <div class="stats">
            <div class="stat">
                <strong id="totalInput">0</strong>
                <span>Total Input</span>
            </div>

            <div class="stat">
                <strong id="totalHasil">0</strong>
                <span>Angka Lolos</span>
            </div>

            <div class="stat">
                <strong id="totalHapus">0</strong>
                <span>Dihapus</span>
            </div>
        </div>

        <textarea
            id="result"
            readonly
            placeholder="Hasil sortir akan muncul di sini..."
        ></textarea>

        <button class="copy-btn" onclick="salinHasil()">
            SALIN HASIL
        </button>

        <p class="message" id="message">
            Masukkan angka lalu tekan tombol proses.
        </p>

    </div>

</div>

<script>

function proses() {

    const input = document.getElementById("input").value;

    // Pisahkan angka berdasarkan tanda *
    const angka = input
        .split("*")
        .map(item => item.trim())
        .filter(item => item !== "");

    // Hilangkan angka yang memiliki digit kembar berjejer
    const hasil = angka.filter(item => {

        // Pastikan hanya angka 3 digit
        if (!/^\d{3}$/.test(item)) {
            return false;
        }

        // Cek apakah ada dua digit bersebelahan yang sama
        if (
            item[0] === item[1] ||
            item[1] === item[2]
        ) {
            return false;
        }

        return true;

    });

    const dihapus = angka.length - hasil.length;

    // Tampilkan hasil
    document.getElementById("result").value =
        hasil.join("*");

    // Tampilkan statistik
    document.getElementById("totalInput").textContent =
        angka.length;

    document.getElementById("totalHasil").textContent =
        hasil.length;

    document.getElementById("totalHapus").textContent =
        dihapus;

    document.getElementById("message").textContent =
        hasil.length > 0
        ? "Sortir berhasil! " + hasil.length + " angka lolos."
        : "Tidak ada angka yang lolos sortir.";

}

function salinHasil() {

    const hasil = document.getElementById("result");

    if (!hasil.value) {
        document.getElementById("message").textContent =
            "Belum ada hasil untuk disalin.";
        return;
    }

    navigator.clipboard.writeText(hasil.value)
        .then(() => {
            document.getElementById("message").textContent =
                "Hasil berhasil disalin!";
        })
        .catch(() => {
            hasil.select();
            document.execCommand("copy");
            document.getElementById("message").textContent =
                "Hasil berhasil disalin!";
        });

}

</script>

</body>
</html>
